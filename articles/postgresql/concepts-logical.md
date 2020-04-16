---
title: Logische Decodierung – Azure Database for PostgreSQL (Einzelserver)
description: Im Folgenden werden die logische Decodierung und wal2json für Change Data Capture in Azure Database for PostgreSQL (Einzelserver) beschrieben.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/31/2020
ms.openlocfilehash: 1213b38f2b67e8fed179cfda4308943808893e1b
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80522574"
---
# <a name="logical-decoding"></a>Logische Decodierung
 
Die [logische Decodierung in PostgreSQL](https://www.postgresql.org/docs/current/logicaldecoding.html) ermöglicht das Streamen von Datenänderungen an externe Consumer. Die logische Decodierung wird besonders häufig für das Ereignisstreaming und Change Data Capture-Szenarien verwendet.

Für die logische Decodierung wird ein Ausgabe-Plug-In verwendet, um das Write-Ahead-Protokoll (WAL) von Postgres in ein lesbares Format zu konvertieren. Azure Database for PostgreSQL bietet zwei Ausgabe-Plug-Ins: [test_decoding](https://www.postgresql.org/docs/current/test-decoding.html) und [wal2json](https://github.com/eulerto/wal2json).
 

> [!NOTE]
> Die logische Decodierung in Azure Database for PostgreSQL (Einzelserver) befindet sich in der Public Preview.


## <a name="set-up-your-server"></a>Einrichten des Servers
Um die logische Decodierung zu verwenden, aktivieren Sie auf dem Server das Speichern und Streamen des WAL. 

1. Legen Sie „azure.replication_support“ mithilfe der Azure-Befehlszeilenschnittstelle auf `logical` fest. 
   ```
   az postgres server configuration set --resource-group mygroup --server-name myserver --name azure.replication_support --value logical
   ```

   > [!NOTE]
   > Wenn Sie Lesereplikate verwenden, ermöglicht das Festlegen von „azure.replication_support“ auf `logical` auch die Ausführung von Replikaten. Wenn Sie die logische Decodierung nicht mehr verwenden, setzen Sie die Einstellung wieder auf `replica` zurück. 


2. Starten Sie den Server neu, um die Änderung zu übernehmen.
   ```
   az postgres server restart --resource-group mygroup --name myserver
   ```

## <a name="start-logical-decoding"></a>Starten der logischen Decodierung

Die logische Decodierung kann über das Streamingprotokoll oder die SQL-Schnittstelle genutzt werden. Beide Methoden verwenden [Replikationsslots](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html#LOGICALDECODING-REPLICATION-SLOTS). Ein Slot stellt einen Datenstrom von Änderungen aus einem Singleton dar.

Die Verwendung eines Replikationsslots erfordert die Replikationsberechtigungen von Postgres. Derzeit ist die Replikationsberechtigung nur für den Serveradministrator verfügbar. 

### <a name="streaming-protocol"></a>Streamingprotokoll
Häufig ist das Verarbeiten von Änderungen mithilfe des Streamingprotokolls vorzuziehen. Sie können einen eigenen Consumer/Connector erstellen oder ein Tool wie [Debezium](https://debezium.io/) verwenden. 

In der wal2json-Dokumentation finden Sie [ein Beispiel für die Verwendung des Streamingprotokolls mit pg_recvlogical](https://github.com/eulerto/wal2json#pg_recvlogical).


### <a name="sql-interface"></a>SQL-Schnittstelle
Im folgenden Beispiel wird die SQL-Schnittstelle mit dem wal2json-Plug-In verwendet.
 
1. Erstellen Sie einen Slot.
   ```SQL
   SELECT * FROM pg_create_logical_replication_slot('test_slot', 'wal2json');
   ```
 
2. Geben Sie SQL-Befehle aus. Beispiel:
   ```SQL
   CREATE TABLE a_table (
      id varchar(40) NOT NULL,
      item varchar(40),
      PRIMARY KEY (id)
   );
   
   INSERT INTO a_table (id, item) VALUES ('id1', 'item1');
   DELETE FROM a_table WHERE id='id1';
   ```

3. Verarbeiten Sie die Änderungen.
   ```SQL
   SELECT data FROM pg_logical_slot_get_changes('test_slot', NULL, NULL, 'pretty-print', '1');
   ```

   Die Ausgabe sieht wie folgt aus:
   ```
   {
         "change": [
         ]
   }
   {
         "change": [
                  {
                           "kind": "insert",
                           "schema": "public",
                           "table": "a_table",
                           "columnnames": ["id", "item"],
                           "columntypes": ["character varying(40)", "character varying(40)"],
                           "columnvalues": ["id1", "item1"]
                  }
         ]
   }
   {
         "change": [
                  {
                           "kind": "delete",
                           "schema": "public",
                           "table": "a_table",
                           "oldkeys": {
                                 "keynames": ["id"],
                                 "keytypes": ["character varying(40)"],
                                 "keyvalues": ["id1"]
                           }
                  }
         ]
   }
   ```

4. Löschen Sie den Slot, wenn Sie ihn nicht mehr benötigen.
   ```SQL
   SELECT pg_drop_replication_slot('test_slot'); 
   ```


## <a name="monitoring-slots"></a>Überwachen von Slots

Sie müssen die logische Decodierung überwachen. Alle nicht verwendeten Replikationsslots müssen gelöscht werden. Slots halten Verbindungen zu den Postgres-WAL-Protokollen und den relevanten Systemkatalogen aufrecht, bis Änderungen von einem Consumer gelesen wurden. Wenn Ihr Consumer ausfällt oder nicht ordnungsgemäß konfiguriert wurde, werden die nicht verarbeiteten Protokolle aufbewahrt und füllen den Speicher. Außerdem erhöhen nicht verarbeitete Protokolle das Risiko von Transaktions-ID-Umläufen. Beide Situationen können dazu führen, dass der Server nicht mehr verfügbar ist. Daher ist es wichtig, dass die logischen Replikationsslots kontinuierlich verarbeitet werden. Wenn ein logischer Replikationsslot nicht mehr verwendet wird, löschen Sie ihn sofort.

In der Spalte „active“ (aktiv) in der Ansicht „pg_replication_slots“ wird angegeben, ob ein Consumer mit einem Slot verbunden ist.
```SQL
SELECT * FROM pg_replication_slots;
```

[Legen Sie Warnungen](howto-alert-on-metric.md) für die Metriken *Verwendeter Speicher* und *Maximale Verzögerung zwischen Replikaten* fest, damit Sie benachrichtigt werden, wenn diese Werte die normalen Schwellenwerte überschreiten. 

> [!IMPORTANT]
> Sie müssen nicht verwendete Replikationsslots löschen. Andernfalls kann es vorkommen, dass der Server nicht mehr verfügbar wird.

## <a name="how-to-drop-a-slot"></a>Löschen von Slots
Wenn Sie einen Replikationsslot nicht aktiv nutzen, sollten Sie ihn löschen.

So löschen Sie einen Replikationsslot namens `test_slot` mithilfe von SQL
```SQL
SELECT pg_drop_replication_slot('test_slot');
```

> [!IMPORTANT]
> Wenn Sie die logische Decodierung nicht mehr verwenden, ändern Sie „azure.replication_support“ zurück in `replica` oder `off`. Die von `logical` verwalteten WAL-Details sind ausführlicher und sollten deaktiviert werden, wenn die logische Decodierung nicht verwendet wird. 

 
## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [logischen Decodierung](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html) finden Sie in der Postgres-Dokumentation.
* Wenden Sie sich an [unser Team](mailto:AskAzureDBforPostgreSQL@service.microsoft.com), wenn Sie Fragen zur logischen Decodierung haben.
* Informieren Sie sich über [Lesereplikate](concepts-read-replicas.md).

