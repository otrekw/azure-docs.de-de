---
title: Einschränkungen – Azure Database for MariaDB
description: Dieser Artikel beschreibt die Einschränkungen in Azure Database for MariaDB, z.B. die Anzahl von Verbindungs- und Speicher-Engine-Optionen.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 4/1/2020
ms.openlocfilehash: d4450689f6865c19436e437e09a3aa9f286c6e21
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83653138"
---
# <a name="limitations-in-azure-database-for-mariadb"></a>Einschränkungen in Azure Database for MariaDB
In den folgenden Abschnitten werden die Kapazitäts- und funktionalen Beschränkungen sowie Beschränkungen bei der Unterstützung der Speicher-Engine und von Datenmanipulationsanweisungen im Datenbankdienst beschrieben.

## <a name="server-parameters"></a>Serverparameter

Die minimalen und maximalen Werte für mehrere häufig verwendete Serverparameter werden durch den Tarif und die virtuellen Kerne festgelegt. In den folgenden Tabellen finden Sie die Grenzwerte.

### <a name="max_connections"></a>max_connections

|**Tarif**|**vCore(s)**|**Standardwert**|**Mindestwert**|**Höchstwert**|
|---|---|---|---|---|
|Basic|1|50|10|50|
|Basic|2|100|10|100|
|Universell|2|300|10|600|
|Universell|4|625|10|1250|
|Universell|8|1250|10|2500|
|Universell|16|2500|10|5\.000|
|Universell|32|5\.000|10|10000|
|Universell|64|10000|10|20000|
|Arbeitsspeicheroptimiert|2|600|10|800|
|Arbeitsspeicheroptimiert|4|1250|10|2500|
|Arbeitsspeicheroptimiert|8|2500|10|5\.000|
|Arbeitsspeicheroptimiert|16|5\.000|10|10000|
|Arbeitsspeicheroptimiert|32|10000|10|20000|

Wenn Verbindungen den Grenzwert übersteigen, erhalten Sie möglicherweise den folgenden Fehler:
> FEHLER 1040 (08004): Zu viele Verbindungen

> [!IMPORTANT]
> Für eine optimale Erfahrung empfehlen wir, dass Sie einen Verbindungspooler wie ProxySQL verwenden, um Verbindungen effizient zu verwalten.

Das Erstellen neuer Clientverbindungen mit MariaDB nimmt Zeit in Anspruch, und nach der Herstellung belegen diese Verbindungen Datenbankressourcen, auch wenn Sie sich im Leerlauf befinden. Die meisten Anwendungen fordern viele kurzlebige Verbindungen an, was diese Situation erschwert. Das Ergebnis sind weniger Ressourcen, die für ihre tatsächliche Workload verfügbar sind, was zu verringerter Leistung führt. Ein Verbindungspooler, der Verbindungen im Leerlauf reduziert und vorhandene Verbindungen wiederverwendet, hilft dabei, dies zu vermeiden. Weitere Informationen zum Einrichten von ProxySQL finden Sie in unserem [Blogbeitrag](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042).

### <a name="query_cache_size"></a>query_cache_size

Der Abfragecache ist standardmäßig deaktiviert. Um den Abfragecache zu aktivieren, konfigurieren Sie den Parameter `query_cache_type`. 

Weitere Informationen zu diesem Parameter finden Sie in der [MariaDB-Dokumentation](https://mariadb.com/kb/en/server-system-variables/#query_cache_size).

|**Tarif**|**vCore(s)**|**Standardwert**|**Mindestwert**|**Höchstwert**|
|---|---|---|---|---|
|Basic|1|Nicht konfigurierbar im Basic-Tarif|–|–|
|Basic|2|Nicht konfigurierbar im Basic-Tarif|–|–|
|Universell|2|0|0|16777216|
|Universell|4|0|0|33554432|
|Universell|8|0|0|67108864|
|Universell|16|0|0|134217728|
|Universell|32|0|0|134217728|
|Universell|64|0|0|134217728|
|Arbeitsspeicheroptimiert|2|0|0|33554432|
|Arbeitsspeicheroptimiert|4|0|0|67108864|
|Arbeitsspeicheroptimiert|8|0|0|134217728|
|Arbeitsspeicheroptimiert|16|0|0|134217728|
|Arbeitsspeicheroptimiert|32|0|0|134217728|

### <a name="sort_buffer_size"></a>sort_buffer_size

Weitere Informationen zu diesem Parameter finden Sie in der [MariaDB-Dokumentation](https://mariadb.com/kb/en/server-system-variables/#sort_buffer_size).

|**Tarif**|**vCore(s)**|**Standardwert**|**Mindestwert**|**Höchstwert**|
|---|---|---|---|---|
|Basic|1|Nicht konfigurierbar im Basic-Tarif|–|–|
|Basic|2|Nicht konfigurierbar im Basic-Tarif|–|–|
|Universell|2|524288|32768|4194304|
|Universell|4|524288|32768|8388608|
|Universell|8|524288|32768|16777216|
|Universell|16|524288|32768|33554432|
|Universell|32|524288|32768|33554432|
|Universell|64|524288|32768|33554432|
|Arbeitsspeicheroptimiert|2|524288|32768|8388608|
|Arbeitsspeicheroptimiert|4|524288|32768|16777216|
|Arbeitsspeicheroptimiert|8|524288|32768|33554432|
|Arbeitsspeicheroptimiert|16|524288|32768|33554432|
|Arbeitsspeicheroptimiert|32|524288|32768|33554432|

### <a name="join_buffer_size"></a>join_buffer_size

Weitere Informationen zu diesem Parameter finden Sie in der [MariaDB-Dokumentation](https://mariadb.com/kb/en/server-system-variables/#join_buffer_size).

|**Tarif**|**vCore(s)**|**Standardwert**|**Mindestwert**|**Höchstwert**|
|---|---|---|---|---|
|Basic|1|Nicht konfigurierbar im Basic-Tarif|–|–|
|Basic|2|Nicht konfigurierbar im Basic-Tarif|–|–|
|Universell|2|262144|128|268435455|
|Universell|4|262144|128|536870912|
|Universell|8|262144|128|1073741824|
|Universell|16|262144|128|2147483648|
|Universell|32|262144|128|4294967295|
|Universell|64|262144|128|4294967295|
|Arbeitsspeicheroptimiert|2|262144|128|536870912|
|Arbeitsspeicheroptimiert|4|262144|128|1073741824|
|Arbeitsspeicheroptimiert|8|262144|128|2147483648|
|Arbeitsspeicheroptimiert|16|262144|128|4294967295|
|Arbeitsspeicheroptimiert|32|262144|128|4294967295|

### <a name="max_heap_table_size"></a>max_heap_table_size

Weitere Informationen zu diesem Parameter finden Sie in der [MariaDB-Dokumentation](https://mariadb.com/kb/en/server-system-variables/#max_heap_table_size).

|**Tarif**|**vCore(s)**|**Standardwert**|**Mindestwert**|**Höchstwert**|
|---|---|---|---|---|
|Basic|1|Nicht konfigurierbar im Basic-Tarif|–|–|
|Basic|2|Nicht konfigurierbar im Basic-Tarif|–|–|
|Universell|2|16777216|16384|268435455|
|Universell|4|16777216|16384|536870912|
|Universell|8|16777216|16384|1073741824|
|Universell|16|16777216|16384|2147483648|
|Universell|32|16777216|16384|4294967295|
|Universell|64|16777216|16384|4294967295|
|Arbeitsspeicheroptimiert|2|16777216|16384|536870912|
|Arbeitsspeicheroptimiert|4|16777216|16384|1073741824|
|Arbeitsspeicheroptimiert|8|16777216|16384|2147483648|
|Arbeitsspeicheroptimiert|16|16777216|16384|4294967295|
|Arbeitsspeicheroptimiert|32|16777216|16384|4294967295|

### <a name="tmp_table_size"></a>tmp_table_size

Weitere Informationen zu diesem Parameter finden Sie in der [MariaDB-Dokumentation](https://mariadb.com/kb/en/server-system-variables/#tmp_table_size).

|**Tarif**|**vCore(s)**|**Standardwert**|**Mindestwert**|**Höchstwert**|
|---|---|---|---|---|
|Basic|1|Nicht konfigurierbar im Basic-Tarif|–|–|
|Basic|2|Nicht konfigurierbar im Basic-Tarif|–|–|
|Universell|2|16777216|1024|67108864|
|Universell|4|16777216|1024|134217728|
|Universell|8|16777216|1024|268435456|
|Universell|16|16777216|1024|536870912|
|Universell|32|16777216|1024|1073741824|
|Universell|64|16777216|1024|1073741824|
|Arbeitsspeicheroptimiert|2|16777216|1024|134217728|
|Arbeitsspeicheroptimiert|4|16777216|1024|268435456|
|Arbeitsspeicheroptimiert|8|16777216|1024|536870912|
|Arbeitsspeicheroptimiert|16|16777216|1024|1073741824|
|Arbeitsspeicheroptimiert|32|16777216|1024|1073741824|

### <a name="time_zone"></a>time_zone

Die Zeitzonentabellen können durch Aufrufen der gespeicherten Prozedur `mysql.az_load_timezone` über ein Tool wie die MySQL-Befehlszeile oder MySQL Workbench aufgefüllt werden. Informationen zum Aufrufen der gespeicherten Prozedur und zum Festlegen der globalen Zeitzonen oder Zeitzonen auf Sitzungsebene finden Sie in den Artikeln für das [Azure-Portal](howto-server-parameters.md#working-with-the-time-zone-parameter) und die [Azure CLI](howto-configure-server-parameters-cli.md#working-with-the-time-zone-parameter).

### <a name="innodb_file_per_table"></a>innodb_file_per_table

MariaDB speichert die InnoDB-Tabelle in verschiedenen Tabellenbereichen basierend auf der Konfiguration, die Sie während der Tabellenerstellung angegeben haben. Der [Systemtabellenbereich](https://mariadb.com/kb/en/innodb-system-tablespaces/) ist der Speicherbereich für das InnoDB-Datenwörterbuch. Ein [file-per-table-Tabellenbereich](https://mariadb.com/kb/en/innodb-file-per-table-tablespaces/) enthält die Daten und Indizes für eine einzelne InnoDB-Tabelle und wird im Dateisystem in einer eigenen Datendatei gespeichert. Dieses Verhalten wird vom `innodb_file_per_table`-Serverparameter gesteuert. Durch Festlegen von `innodb_file_per_table` auf `OFF` werden Tabellen von InnoDB im Systemtabellenbereich erstellt. Andernfalls werden die Tabellen von InnoDB im „file-per-table“-Tabellenbereich erstellt.

Azure Database for MariaDB unterstützt bis zu **ein Terabyte** in einer einzelnen Datendatei. Wenn die Datenbankgröße ein Terabyte überschreitet, sollten Sie die Tabelle im [innodb_file_per_table](https://mariadb.com/kb/en/innodb-system-variables/#innodb_file_per_table)-Tabellenbereich erstellen. Wenn eine einzelne Tabelle größer als ein Terabyte ist, sollten Sie die Partitionstabelle verwenden.

## <a name="storage-engine-support"></a>Speicher-Engine-Unterstützung

### <a name="supported"></a>Unterstützt
- [InnoDB](https://mariadb.com/kb/en/library/xtradb-and-innodb/)
- [MEMORY](https://mariadb.com/kb/en/library/memory-storage-engine/)

### <a name="unsupported"></a>Nicht unterstützt
- [MyISAM](https://mariadb.com/kb/en/library/myisam-storage-engine/)
- [BLACKHOLE](https://mariadb.com/kb/en/library/blackhole/)
- [ARCHIVE](https://mariadb.com/kb/en/library/archive/)

## <a name="privilege-support"></a>Berechtigungsunterstützung

### <a name="unsupported"></a>Nicht unterstützt
- DBA-Rolle: Viele Serverparameter und -einstellungen können unbeabsichtigterweise die Serverleistung beeinträchtigen oder ACID-Eigenschaften des DBMS verschlechtern. Um die Dienstintegrität und die SLA auf Produktebene aufrechtzuerhalten, stellt dieser Dienst die DBA-Rolle nicht zur Verfügung. Das Standardbenutzerkonto, das zusammen mit einer neuen Datenbankinstanz erstellt wird, gestattet dem Benutzer die Ausführung der meisten DDL- und DML-Anweisungen in der verwalteten Datenbankinstanz.
- SUPER-Berechtigung: Auf ähnliche Weise ist die [SUPER-Berechtigung](https://mariadb.com/kb/en/library/grant/#global-privileges) ebenfalls eingeschränkt.
- DEFINER: Erfordert erhöhte Berechtigungen zum Erstellen und ist beschränkt. Entfernen Sie beim Importieren von Daten mithilfe einer Sicherung die `CREATE DEFINER`-Befehle manuell oder mithilfe des Befehls `--skip-definer`, wenn Sie einen mysqldump ausführen.

## <a name="data-manipulation-statement-support"></a>Unterstützung von Datenmanipulationsanweisungen

### <a name="supported"></a>Unterstützt
- `LOAD DATA INFILE` wird unterstützt, jedoch muss der Parameter `[LOCAL]` angegeben und an einen UNC-Pfad (über das SMB-Protokoll eingebundene Azure Storage-Instanz) weitergeleitet werden.

### <a name="unsupported"></a>Nicht unterstützt
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>Funktionale Beschränkungen

### <a name="scale-operations"></a>Skalierungsvorgänge
- Die dynamische Skalierung auf den oder vom Basic-Tarif aus wird zurzeit nicht unterstützt.
- Die Verringerung der Größe des Serverspeichers wird nicht unterstützt.

### <a name="server-version-upgrades"></a>Upgrades von Serverversionen
- Die automatisierte Migration zwischen Hauptversionen von Datenbank-Engines wird derzeit nicht unterstützt.

### <a name="point-in-time-restore"></a>Point-in-Time-Wiederherstellung
- Wenn Sie das PITR-Feature verwenden, wird der neue Server mit den gleichen Konfigurationen erstellt wie der Server, auf dem er basiert.
- Die Wiederherstellung eines gelöschten Servers wird nicht unterstützt.

### <a name="subscription-management"></a>Abonnementverwaltung
- Die dynamische Verschiebung von vorab erstellten Servern zwischen Abonnement- und Ressourcengruppen wird derzeit nicht unterstützt.

### <a name="vnet-service-endpoints"></a>VNET-Dienstendpunkte
- VNET-Dienstendpunkte werden nur für Server vom Typ „Universell“ und „Arbeitsspeicheroptimiert“ unterstützt.

### <a name="storage-size"></a>Speichergröße
- Informationen zu den Beschränkungen der Speichergröße pro Tarif finden Sie unter [Tarife](concepts-pricing-tiers.md).

## <a name="current-known-issues"></a>Aktuelle bekannte Probleme
- Die MariaDB-Serverinstanz zeigt die falsche Serverversion an, nachdem die Verbindung hergestellt wurde. Um die richtige Engine-Version der Serverinstanzen abzurufen, verwenden Sie den Befehl `select version();`.

## <a name="next-steps"></a>Nächste Schritte
- [Verfügbaren Funktionen auf den einzelnen Dienstebenen](concepts-pricing-tiers.md)
- [Unterstützte MariaDB-Datenbankversionen](concepts-supported-versions.md)
