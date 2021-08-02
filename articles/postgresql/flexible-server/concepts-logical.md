---
title: Logische Replikation und logische Decodierung – Azure Database for PostgreSQL – Flexible Server
description: Weitere Informationen zur logischen Replikation und Decodierung in Azure Database for PostgreSQL – Flexible Server
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/10/2021
ms.openlocfilehash: e3e468b774503b42fd46e66492f09982e8d1d9a6
ms.sourcegitcommit: e39ad7e8db27c97c8fb0d6afa322d4d135fd2066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111982266"
---
# <a name="logical-replication-and-logical-decoding-in-azure-database-for-postgresql---flexible-server"></a>Logische Replikation und Decodierung in Azure Database for PostgreSQL – Flexible Server

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

Azure Database für PostgreSQL - Flexible Server unterstützt die folgenden Methoden der logischen Datenextraktion und -replikation:
1. **Logische Replikation**
   1. Verwendung Sie PostgreSQL [native logische Replikation](https://www.postgresql.org/docs/12/logical-replication.html), um Datenobjekte zu replizieren. Die logische Replikation ermöglicht eine detaillierte Kontrolle über die Datenreplikation, einschließlich der Datenreplikation auf Tabellenebene.
   <!--- 2. Using [pglogical](https://github.com/2ndQuadrant/pglogical) extension that provides logical streaming replication and additional capabilities such as copying initial schema of the database, support for TRUNCATE, ability to replicate DDL etc. -->
2. **Logische Dekodierung**, die durch [Dekodierung](https://www.postgresql.org/docs/12/logicaldecoding-explanation.html) des Inhalts des Write-Ahead-Logs (WAL) realisiert wird. 

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
* kann keine Daten direkt zwischen PostgreSQL-Instanzen senden.

## <a name="pre-requisites-for-logical-replication-and-logical-decoding"></a>Voraussetzungen für die logische Replikation und die logische Decodierung

1. Wechseln Sie im Portal zur Seite „Serverparameter“.
2. Legen Sie den Serverparameter `wal_level` auf `logical` fest.
<!---
3. If you want to use pglogical extension, search for the `shared_preload_libaries` parameter, and select `pglogical` from the drop-down box. Also update `max_worker_processes` parameter value to at least 16. -->
3. Speichern Sie die Änderungen, und starten Sie den Server neu, um die Änderung von `wal_level` zu übernehmen.
4. Vergewissern Sie sich, dass Ihre PostgreSQL-Instanz Netzwerkdatenverkehr von Ihrer Verbindungsressource zulässt.
5. Erteilen Sie dem Administratorbenutzer Replikationsberechtigungen.
   ```SQL
   ALTER ROLE <adminname> WITH REPLICATION;
   ```

## <a name="using-logical-replication-and-logical-decoding"></a>Verwenden der logischen Replikation und der logischen Decodierung

### <a name="native-logical-replication"></a>Native logische Replikation
Bei der logischen Replikation wird üblicherweise vom „Verleger“ und vom „Abonnenten“ gesprochen. 
* Beim Verleger handelt es sich um die PostgreSQL-Datenbank, **von** der die Daten gesendet werden. 
* Beim Abonnenten handelt es sich um die PostgreSQL-Datenbank, **an** die die Daten gesendet werden.

Nachstehend finden Sie einen Beispielcode, mit dem Sie die logische Replikation ausprobieren können.

1. Stellen Sie eine Verbindung mit der Verlegerdatenbank her. Erstellen Sie eine Tabelle, und fügen Sie Daten hinzu.
   ```SQL
   CREATE TABLE basic(id SERIAL, name varchar(40));
   INSERT INTO basic(name) VALUES ('apple');
   INSERT INTO basic(name) VALUES ('banana');
   ```

2. Erstellen Sie eine Veröffentlichung für die Tabelle.
   ```SQL
   CREATE PUBLICATION pub FOR TABLE basic;
   ```

3. Stellen Sie eine Verbindung mit der Abonnentendatenbank her. Erstellen Sie eine Tabelle mit demselben Schema wie für den Verleger.
   ```SQL
   CREATE TABLE basic(id SERIAL, name varchar(40));
   ```

4. Erstellen Sie ein Abonnement, das eine Verbindung mit der zuvor erstellten Veröffentlichung herstellt.
   ```SQL
   CREATE SUBSCRIPTION sub CONNECTION 'host=<server>.postgres.database.azure.com user=<admin> dbname=<dbname> password=<password>' PUBLICATION pub;
   ```

5. Sie können die Tabelle nun auf dem Abonnenten abfragen. Sie werden sehen, dass dieser Daten vom Verleger empfangen hat.
   ```SQL
   SELECT * FROM basic;
   ```
   Sie können der Tabelle des Verlegers weitere Zeilen hinzufügen und die Änderungen auf dem Abonnenten anzeigen.

   Wenn Sie die Daten nicht sehen können, aktivieren Sie die Anmeldeberechtigung für `azure_pg_admin`, und überprüfen Sie den Tabelleninhalt. 
   ```SQL 
   ALTER ROLE azure_pg_admin login;
   ```


Weitere Informationen über die [logische Replikation](https://www.postgresql.org/docs/current/logical-replication.html) finden Sie in der PostgreSQL-Dokumentation.

<!---
### pglogical extension

Here is an example of configuring pglogical at the provider database server and the subscriber. Please refer to pglogical extension documentation for more details. Also make sure you have performed pre-requisite tasks listed above.

1. Install pglogical extension in the database in both the provider and the subscriber database servers.
    ```SQL
   \C myDB
   CREATE EXTENSION pglogical;
   ```
2. At the **provider** (source/publisher) database server, create the provider node.
   ```SQL
   select pglogical.create_node( node_name := 'provider1', 
   dsn := ' host=myProviderServer.postgres.database.azure.com port=5432 dbname=myDB user=myUser password=myPassword');
   ```
3. Create a replication set.
   ```SQL
   select pglogical.create_replication_set('myreplicationset');
   ```
4. Add all tables in the database to the replication set.
   ```SQL
   SELECT pglogical.replication_set_add_all_tables('myreplicationset', '{public}'::text[]);
   ```

   As an alternate method, ou can also add tables from a specific schema (for example, testUser) to a default replication set.
   ```SQL
   SELECT pglogical.replication_set_add_all_tables('default', ARRAY['testUser']);
   ```

5. At the **subscriber** database server, create a subscriber node.
   ```SQL
   select pglogical.create_node( node_name := 'subscriber1', 
   dsn := ' host=mySubscriberServer.postgres.database.azure.com port=5432 dbname=myDB user=myUser password=myPasword' );
   ```
6. Create a subscription to start the synchronization and the replication process.
    ```SQL
   select pglogical.create_subscription (
   subscription_name := 'subscription1',
   replication_sets := array['myreplicationset'],
   provider_dsn := 'host=myProviderServer.postgres.database.azure.com port=5432 dbname=myDB user=myUser password=myPassword');
   ```
7. You can then verify the subscription status.
   ```SQL
   SELECT subscription_name, status FROM pglogical.show_subscription_status();
   ```
-->
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

## <a name="limitations"></a>Einschränkungen
* Es gelten **logische Replikationseinschränkungen**, wie [hier](https://www.postgresql.org/docs/12/logical-replication-restrictions.html) dokumentiert.
* **Lesereplikate:** Lesereplikate von Azure Database for PostgreSQL werden mit flexible Server derzeit nicht unterstützt.
* **Slots und Hochverfügbarkeitsfailover:** Logische Replikationsslots auf dem primären Server sind auf dem Standbyserver in der sekundären Verfügbarkeitszone nicht verfügbar. Dies gilt für Sie, wenn der Server die Option für die zonenredundante Hochverfügbarkeit verwendet. Im Fall eines Failovers auf den Standbyserver sind logische Replikationsslots im Standbymodus nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu [Netzwerkoptionen](concepts-networking.md)
* Weitere Informationen zu in flexiblen Servern verfügbaren [Erweiterungen](concepts-extensions.md)
* Weitere Informationen zu [Hochverfügbarkeit](concepts-high-availability.md)

