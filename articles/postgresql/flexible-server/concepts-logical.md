---
title: Logische Replikation und logische Decodierung – Azure Database for PostgreSQL – Flexible Server
description: Weitere Informationen zur logischen Replikation und Decodierung in Azure Database for PostgreSQL – Flexible Server
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: fd0826ad11a153d72ee47f35930d25f0df498418
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931791"
---
# <a name="logical-replication-and-logical-decoding-in-azure-database-for-postgresql---flexible-server"></a>Logische Replikation und Decodierung in Azure Database for PostgreSQL – Flexible Server

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

Die PostgreSQL-Features zur logischen Replikation und logischen Decodierung werden in Azure Database for PostgreSQL – Flexible Server unterstützt.

## <a name="comparing-logical-replication-and-logical-decoding"></a>Vergleich der logischen Replikation mit der logischen Decodierung
Die logische Replikation und die logische Decodierung weisen Ähnlichkeiten auf. Folgendes gilt für beide Konzepte:
* Sie ermöglichen es Ihnen, Daten aus Postgres zu replizieren.
* Beide verwenden das [Write-Ahead-Protokoll](https://www.postgresql.org/docs/current/wal.html) (Write-Ahead Log, WAL) als Änderungsquelle.
* Sie verwenden [logische Replikationsslots](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html#LOGICALDECODING-REPLICATION-SLOTS), um Daten zu senden. Ein Slot stellt einen Datenstrom von Änderungen dar.
* Sie bestimmen mithilfe der [Eigenschaft REPLICA IDENTITY](https://www.postgresql.org/docs/current/sql-altertable.html#SQL-CREATETABLE-REPLICA-IDENTITY) einer Tabelle, welche Änderungen gesendet werden können.
* Sie replizieren keine DDL-Änderungen.


Die beiden Technologien unterscheiden sich wie folgt: Logische Replikation 
* Ermöglicht es Ihnen, eine Tabelle oder Tabellengruppe anzugeben, die repliziert werden soll
* Repliziert Daten zwischen PostgreSQL-Instanzen

Logische Decodierung 
* Extrahiert Änderungen aus allen Tabellen in einer Datenbank 
* Kann Daten nicht direkt zwischen PostgreSQL-Instanzen senden


## <a name="pre-requisites-for-logical-replication-and-logical-decoding"></a>Voraussetzungen für die logische Replikation und die logische Decodierung

1. Legen Sie den Serverparameter `wal_level` auf `logical` fest.
2. Starten Sie den Server neu, um die Änderung an `wal_level` zu übernehmen.
3. Vergewissern Sie sich, dass Ihre PostgreSQL-Instanz Netzwerkdatenverkehr von Ihrer Verbindungsressource zulässt.
4. Verwenden Sie den Administratorbenutzer für das Ausführen von Replikationsbefehlen.

## <a name="using-logical-replication-and-logical-decoding"></a>Verwenden der logischen Replikation und der logischen Decodierung

### <a name="logical-replication"></a>Logische Replikation
Bei der logischen Replikation wird üblicherweise vom „Verleger“ und vom „Abonnenten“ gesprochen. 
* Beim Verleger handelt es sich um die PostgreSQL-Datenbank, **von** der die Daten gesendet werden. 
* Beim Abonnenten handelt es sich um die PostgreSQL-Datenbank, **an** die die Daten gesendet werden.

Nachstehend finden Sie einen Beispielcode, mit dem Sie die logische Replikation ausprobieren können.

1. Stellen Sie eine Verbindung mit dem Verleger her. Erstellen Sie eine Tabelle, und fügen Sie Daten hinzu.
   ```SQL
   CREATE TABLE basic(id SERIAL, name varchar(40));
   INSERT INTO basic(name) VALUES ('apple');
   INSERT INTO basic(name) VALUES ('banana');
   ```

2. Erstellen Sie eine Veröffentlichung für die Tabelle.
   ```SQL
   CREATE PUBLICATION pub FOR TABLE basic;
   ```

3. Stellen Sie eine Verbindung mit dem Abonnenten her. Erstellen Sie eine Tabelle mit demselben Schema wie für den Verleger.
   ```SQL
   CREATE TABLE basic(id SERIAL, name varchar(40));
   ```

4. Erstellen Sie ein Abonnement, das eine Verbindung mit der zuvor erstellten Veröffentlichung herstellt.
   ```SQL
   CREATE SUBSCRIPTION sub CONNECTION 'host=<server>.postgres.database.azure.com user=<admin> dbname=<dbname>' PUBLICATION pub;
   ```

5. Sie können die Tabelle nun auf dem Abonnenten abfragen. Sie werden sehen, dass dieser Daten vom Verleger empfangen hat.
   ```SQL
   SELECT * FROM basic;
   ```

Sie können der Tabelle des Verlegers weitere Zeilen hinzufügen und die Änderungen auf dem Abonnenten anzeigen.

Weitere Informationen über die [logische Replikation](https://www.postgresql.org/docs/current/logical-replication.html) finden Sie in der PostgreSQL-Dokumentation.

### <a name="logical-decoding"></a>Logische Decodierung
Die logische Decodierung kann über das Streamingprotokoll oder die SQL-Schnittstelle genutzt werden. 

#### <a name="streaming-protocol"></a>Streamingprotokoll
Häufig ist das Verarbeiten von Änderungen mithilfe des Streamingprotokolls vorzuziehen. Sie können einen eigenen Consumer/Connector erstellen oder einen Drittanbieterdienst wie [Debezium](https://debezium.io/) verwenden. 

In der wal2json-Dokumentation finden Sie [ein Beispiel für die Verwendung des Streamingprotokolls mit pg_recvlogical](https://github.com/eulerto/wal2json#pg_recvlogical).

#### <a name="sql-interface"></a>SQL-Schnittstelle 
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

Weitere Informationen über die [logische Decodierung](https://www.postgresql.org/docs/current/logicaldecoding.html) finden Sie in der PostgreSQL-Dokumentation.


## <a name="monitoring"></a>Überwachung
Sie müssen die logische Decodierung überwachen. Alle nicht verwendeten Replikationsslots müssen gelöscht werden. Slots halten Verbindungen zu den Postgres-WAL-Protokollen und den relevanten Systemkatalogen aufrecht, bis Änderungen gelesen wurden. Wenn Ihr Abonnent oder Consumer ausfällt oder nicht ordnungsgemäß konfiguriert wurde, werden die nicht verarbeiteten Protokolle aufbewahrt und füllen den Speicher. Außerdem erhöhen nicht verarbeitete Protokolle das Risiko von Transaktions-ID-Umläufen. Beide Situationen können dazu führen, dass der Server nicht mehr verfügbar ist. Daher ist es wichtig, dass die logischen Replikationsslots kontinuierlich verarbeitet werden. Wenn ein logischer Replikationsslot nicht mehr verwendet wird, löschen Sie ihn sofort.

In der Spalte „active“ (aktiv) in der Ansicht „pg_replication_slots“ wird angegeben, ob ein Consumer mit einem Slot verbunden ist.
```SQL
SELECT * FROM pg_replication_slots;
```

[Legen Sie Benachrichtigungen](howto-alert-on-metrics.md) für die Servermetriken **Maximum Used Transaction IDs** (Maximal verwendete Transaktions-IDs) and **Verwendeter Speicher** fest, damit Sie informiert werden, wenn die normalen Schwellenwerte überschritten werden. 

## <a name="read-replicas"></a>Lesereplikate
Azure Database for PostgreSQL-Lesereplikate werden für flexible Server derzeit nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu [Netzwerkoptionen](concepts-networking.md)
* Weitere Informationen zu in flexiblen Servern verfügbaren [Erweiterungen](concepts-extensions.md)
* Weitere Informationen zu [Hochverfügbarkeit](concepts-high-availability.md)

