---
title: Abfragespeicher – Azure Database for PostgreSQL (Flex Server)
description: In diesem Artikel wird das Abfragespeicherfeature in Azure Database for PostgreSQL (Flex Server) beschrieben.
author: ssen-msft
ms.author: ssen
ms.service: postgresql
ms.topic: conceptual
ms.date: 04/01/2021
ms.openlocfilehash: 6ab2ea6f6544bcf561e92f00b5afee693393c157
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107558661"
---
# <a name="monitor-performance-with-query-store"></a>Überwachen der Leistung mit Abfragespeicher
**Anwendungsbereich:** Azure Database for PostgreSQL – Flex Server-Versionen 11 und höher

Das Abfragespeicherfeature in Azure Database for PostgreSQL bietet eine Möglichkeit, um die Abfrageleistung im Zeitverlauf nachzuverfolgen. Der Abfragespeicher vereinfacht das Beheben von Leistungsproblemen, da er es Ihnen ermöglicht, die am längsten ausgeführten und ressourcenintensivsten Abfragen schnell zu ermitteln. Der Abfragespeicher erfasst automatisch einen Verlauf der Abfragen und Laufzeitstatistiken und bewahrt diese auf, damit Sie sie überprüfen können. Es teilt die Daten nach Zeit auf, so dass Sie zeitliche Nutzungsmuster sehen können. Die Daten für alle Benutzer, Datenbanken und Abfragen werden in einer Datenbank namens **azure_sys** in der Azure Database for PostgreSQL-Instanz gespeichert.

> [!IMPORTANT]
> Nehmen Sie an der **azure_sys**-Datenbank oder ihrem Schema keine Änderungen vor. Andernfalls funktionieren der Abfragespeicher und die entsprechenden Leistungsfeatures nicht mehr ordnungsgemäß.
## <a name="enabling-query-store"></a>Aktivieren des Abfragespeichers
Der Abfragespeicher ist ein optionales Feature. Daher ist er auf einem Server nicht standardmäßig aktiviert. Der Abfragespeicher wird global für alle Datenbanken auf einem bestimmten Server aktiviert oder deaktiviert. Ein Aktivieren oder Deaktivieren für einzelne Datenbanken ist nicht möglich.
### <a name="enable-query-store-using-the-azure-portal"></a>Aktivieren des Abfragespeichers über das Azure-Portal
1. Melden Sie sich beim Azure-Portal an, und wählen Sie Ihren Azure Database for PostgreSQL-Server aus.
2. Wählen Sie im Bereich **Einstellungen** im Menü die Option **Serverparameter**.
3. Suchen Sie nach dem Parameter `pg_qs.query_capture_mode`.
4. Legen Sie für den Wert auf `TOP` oder `ALL` fest und **Speichern**.
Es kann bis zu 20 Minuten dauern, bis der erste Datenbatch in der azure_sys-Datenbank gespeichert ist.
## <a name="information-in-query-store"></a>Informationen im Abfragespeicher
Abfragespeicher verfügt über einen Speicher:
- Ein Speicher für Laufzeitstatistiken zum Aufbewahren der Informationen aus den Abfragespeicherstatistiken.

Häufige Szenarien für die Verwendung des Abfragespeichers sind u.a.:
- Bestimmen der Häufigkeit zum Ausführen einer Abfrage in einem angegebenen Zeitfenster
- Vergleichen der durchschnittlichen Ausführungsdauer einer Abfrage für bestimmte Zeitfenster zum Anzeigen großer Abweichungen
- Identifizierung der am längsten laufenden Abfragen in den letzten paar Stunden Um die Speicherverwendung zu minimieren, werden die Laufzeit-Ausführungsstatistiken im Speicher für Laufzeitstatistiken für ein festes, konfigurierbares Zeitfenster zusammengefasst. Die Informationen in diesen Speichern können mithilfe von Ansichten abgefragt werden.
## <a name="access-query-store-information"></a>Zugreifen auf Abfragespeicherinformationen
Abfragespeicherdaten werden in der Datenbank „azure_sys“ auf Ihrem Postgres-Server gespeichert. Die folgende Abfrage gibt Informationen über Abfragen im Abfragespeicher zurück:
```sql
SELECT * FROM query_store.qs_view; 
``` 

## <a name="configuration-options"></a>Konfigurationsoptionen
Wenn der Abfragespeicher aktiviert ist, speichert er Daten in Aggregationsfenstern von 15 Minuten mit bis zu 500 unterschiedlichen Abfragen pro Fenster. Die folgenden Optionen stehen für die Konfiguration der Abfragespeicherparameter zur Verfügung.

| **Parameter** | **Beschreibung** | **Standard** | **Bereich**|
|---|---|---|---|
| pg_qs.query_capture_mode | Legt fest, welche Anweisungen nachverfolgt werden. | none | none, top, all |
| pg_qs.max_query_text_length | Legt die maximale Abfragelänge fest, die gespeichert werden kann. Längere Abfragen werden abgeschnitten. | 6000 | 100 – 10.000 |
| pg_qs.retention_period_in_days | Legt den Aufbewahrungszeitraum fest. | 7 | 1 – 30 |
| pg_qs.track_utility | Legt fest, ob Dienstprogrammbefehle nachverfolgt werden. | on | on, off |

Verwenden Sie das[Azure-Portal](howto-configure-server-parameters-using-portal.md), um für einen Parameter einen anderen Wert abzurufen oder festzulegen.

## <a name="views-and-functions"></a>Ansichten und Funktionen
Mithilfe der folgenden Ansichten und Funktionen können Sie den Abfragespeicher anzeigen und verwalten. In der öffentlichen PostgreSQL-Rolle kann jeder diese Ansichten verwenden, um die Daten im Abfragespeicher anzuzeigen. Diese Ansichten sind nur in der **azure_sys**-Datenbank verfügbar.
Abfragen werden normalisiert, indem ihre Struktur nach dem Entfernen von Literalen und Konstanten untersucht wird. Wenn zwei Abfragen mit Ausnahme von Literalwerten identisch sind, haben sie dieselbe Abfrage-ID.
### <a name="query_storeqs_view"></a>query_store.qs_view
In dieser Ansicht werden alle Daten im Abfragespeicher zurückgegeben. Es gibt eine Zeile für jede einzelne Datenbank-ID, Benutzer-ID und Abfrage-ID. 

|**Name**   |**Typ** | **Referenzen**  | **Beschreibung**|
|---|---|---|---|
|runtime_stats_entry_id |BIGINT | | Die ID aus der Tabelle runtime_stats_entries|
|user_id    |oid    |pg_authid.oid  |Die OID des Benutzers, der die Anweisung ausgeführt hat|
|db_id  |oid    |pg_database.oid    |Die OID der Datenbank, in der die Anweisung ausgeführt wurde|
|query_id   |BIGINT  || Interner Hash, der von der Analysestruktur der Anweisung berechnet wurde|
|query_sql_text |Varchar(10000)  || Der Text einer repräsentativen Anweisung. Unterschiedliche Abfragen mit der gleichen Struktur werden gruppiert; dieser Text ist der Text für die erste Abfrage im Cluster.|
|plan_id    |BIGINT |   |ID des Plans, der dieser Abfrage entspricht, noch nicht verfügbar|
|start_time |timestamp  ||  Abfragen werden nach Zeitrahmen zusammengefasst. Die Zeitspanne eines Zeitrahmens beträgt standardmäßig 15 Minuten. Dies ist die Startzeit des Zeitrahmens für diesen Eintrag.|
|end_time   |timestamp  ||  Dies ist die Endzeit des Zeitrahmens für diesen Eintrag.|
|calls  |BIGINT  || Häufigkeit der Abfrageausführung|
|total_time |double precision   ||  Gesamte Abfrageausführungsdauer in Millisekunden|
|min_time   |double precision   ||  Mindestwert der Abfrageausführungsdauer in Millisekunden|
|max_time   |double precision   ||  Höchstwert der Abfrageausführungsdauer in Millisekunden|
|mean_time  |double precision   ||  Durchschnittliche Abfrageausführungsdauer in Millisekunden|
|stddev_time|   double precision    ||  Standardabweichung der Abfrageausführungsdauer in Millisekunden |
|rows   |BIGINT ||  Gesamtanzahl der Zeilen, die abgerufen wurden oder von der Anweisung betroffen sind|
|shared_blks_hit|   BIGINT  ||  Gesamtanzahl der freigegebenen Blockcachetreffer der Anweisung|
|shared_blks_read|  BIGINT  ||  Gesamtanzahl der freigegebenen Blöcke, die von der Anweisung gelesen wurden|
|shared_blks_dirtied|   BIGINT   || Gesamtanzahl der freigegebenen Blöcke, die von der Anweisung geändert wurden |
|shared_blks_written|   BIGINT  ||  Gesamtanzahl der freigegebenen Blöcke, die von der Anweisung geschrieben wurden|
|local_blks_hit|    BIGINT ||   Gesamtanzahl der lokalen Blockcachetreffer der Anweisung|
|local_blks_read|   BIGINT   || Gesamtanzahl der lokalen Blöcke, die von der Anweisung gelesen wurden|
|local_blks_dirtied|    BIGINT  ||  Gesamtanzahl der lokalen Blöcke, die von der Anweisung geändert wurden|
|local_blks_written|    BIGINT  ||  Gesamtanzahl der lokalen Blöcke, die von der Anweisung geschrieben wurden|
|temp_blks_read |BIGINT  || Gesamtanzahl der temporären Blöcke, die von der Anweisung gelesen wurden|
|temp_blks_written| BIGINT   || Gesamtanzahl der temporären Blöcke, die von der Anweisung geschrieben wurden|
|blk_read_time  |double precision    || Gesamte Zeit, die die Anweisung zum Lesen von Blöcken benötigt hat, in Millisekunden (wenn track_io_timing aktiviert ist, andernfalls Null)|
|blk_write_time |double precision    || Gesamte Zeit, die die Anweisung zum Schreiben von Blöcken benötigt hat, in Millisekunden (wenn track_io_timing aktiviert ist, andernfalls Null)|
    
### <a name="query_storequery_texts_view"></a>query_store.query_texts_view
Diese Ansicht gibt alle Abfragetextdaten im Abfragespeicher zurück. Für jeden einzelnen query_text gibt es eine Zeile.

| **Name** | **Typ** | **Beschreibung** |
|--|--|--|
| query_text_id | BIGINT | ID der Tabelle query_texts |
| query_sql_text | Varchar(10000) | Der Text einer repräsentativen Anweisung. Unterschiedliche Abfragen mit der gleichen Struktur werden gruppiert; dieser Text ist der Text für die erste Abfrage im Cluster. |

### <a name="functions"></a>Functions
`qs_reset` verwirft alle Statistiken, die bisher vom Abfragespeicher gesammelt werden. Diese Funktion kann nur von der Serveradministratorrolle ausgeführt werden.

`staging_data_reset` verwirft alle Statistiken, die vom Abfragespeicher im Arbeitsspeicher erfasst werden (d.h. die Daten im Arbeitsspeicher, für die noch kein Flushvorgang in die Datenbank durchgeführt wurde). Diese Funktion kann nur von der Serveradministratorrolle ausgeführt werden.

## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme
- Wenn ein PostgreSQL-Server über den Parameter „default_transaction_read_only“ verfügt, erfasst der Abfragespeicher keine Daten.
## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Szenarien, in denen der Abfragespeicher hilfreich ist](concepts-query-store-scenarios.md).
- Erfahren Sie mehr über die [bewährten Methoden für die Verwendung des Abfragespeichers](concepts-query-store-best-practices.md).
