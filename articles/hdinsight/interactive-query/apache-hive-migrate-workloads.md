---
title: Migrieren von Azure HDInsight 3.6-Hive-Workloads zu HDInsight 4.0
description: Erfahren Sie, wie Sie Apache Hive-Workloads in HDInsight 3.6 zu HDInsight 4.0 migrieren.
author: kevxmsft
ms.author: kevx
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 11/4/2020
ms.openlocfilehash: b13e8e088eff95071247a53ad1a4a18879f94053
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101742193"
---
# <a name="migrate-azure-hdinsight-36-hive-workloads-to-hdinsight-40"></a>Migrieren von Azure HDInsight 3.6-Hive-Workloads zu HDInsight 4.0

HDInsight 4.0 bietet gegenüber HDInsight 3.6 verschiedene Vorteile. Hier finden Sie eine [Übersicht über die neuen Funktionen in HDInsight 4.0](../hdinsight-version-release.md).

In diesem Artikel werden die Schritte zum Migrieren von Hive-Workloads von HDInsight 3.6 zu 4.0 behandelt, einschließlich:

* Kopieren des Hive-Metastore und Upgrade des Schemas
* Sichere Migration für die ACID-Kompatibilität
* Beibehaltung von Hive-Sicherheitsrichtlinien

Die neuen und alten HDInsight-Cluster müssen Zugriff auf dieselben Speicherkonten haben.

Die Migration von Hive-Tabellen zu einem neuen Speicherkonto muss als separater Schritt durchgeführt werden. Weitere Informationen finden Sie unter [Hive-Migration zwischen Speicherkonten](./hive-migration-across-storage-accounts.md).

## <a name="steps-to-upgrade"></a>Schritte zum Upgrade

### <a name="1-prepare-the-data"></a>1. Aufbereiten der Daten

* HDInsight 3.6 unterstützt standardmäßig keine ACID-Tabellen. Wenn jedoch ACID-Tabellen vorhanden sind, führen Sie eine MAJOR-Komprimierung für diese aus. Ausführliche Informationen zur Komprimierung finden Sie im [Handbuch zur Hive-Sprache](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-AlterTable/Partition/Compact).

* Bei Verwendung von [Azure Data Lake Storage Gen1](../overview-data-lake-storage-gen1.md) sind die Speicherorte für Hive-Tabellen wahrscheinlich von den HDFS-Konfigurationen des Clusters abhängig. Führen Sie die folgende Skriptaktion aus, um die Portierung dieser Speicherorte auf andere Cluster zu ermöglichen. Weitere Informationen finden Sie unter [Skriptaktion für einen ausgeführten Cluster](../hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster).

    |Eigenschaft | Wert |
    |---|---|
    |Bash-Skript-URI|`https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/hive-adl-expand-location-v01.sh`|
    |Knotentyp(en)|Head|
    |Parameter||

### <a name="2-copy-the-sql-database"></a>2. Kopieren der SQL-Datenbank

* Wenn der Cluster einen Standard-Hive-Metastore verwendet, befolgen Sie diese [Anleitung](./hive-default-metastore-export-import.md), um Metadaten in einen externen Metastore zu exportieren. Erstellen Sie dann eine Kopie des externen Metastores für das Upgrade.

* Wenn der Cluster einen externen Hive-Metastore verwendet, erstellen Sie eine Kopie davon. Optionen sind u. a. [Exportieren/Importieren](../../azure-sql/database/database-export.md) und [Zeitpunktwiederherstellung](../../azure-sql/database/recovery-using-backups.md#point-in-time-restore).

### <a name="3-upgrade-the-metastore-schema"></a>3. Aktualisieren des Metastoreschemas

In diesem Schritt wird das [`Hive Schema Tool`](https://cwiki.apache.org/confluence/display/Hive/Hive+Schema+Tool) von HDInsight 4.0 verwendet, um das Metastoreschema zu aktualisieren.

> [!Warning]
> Dieser Schritt kann nicht rückgängig gemacht werden. Führen Sie diesen Vorgang nur bei einer Kopie des Metastores aus.

1. Erstellen Sie einen temporären HDInsight 4.0-Cluster, um auf 4.0-Hive-`schematool` zuzugreifen. Für diesen Schritt können Sie den [Standard-Hive-Metastore](../hdinsight-use-external-metadata-stores.md#default-metastore) verwenden.

1. Führen Sie vom HDInsight 4.0-Cluster `schematool` aus, um den Ziel-HDInsight 3.6-Metastore zu aktualisieren:

    ```sh
    SERVER='servername.database.windows.net'  # replace with your SQL Server
    DATABASE='database'  # replace with your 3.6 metastore SQL Database
    USERNAME='username'  # replace with your 3.6 metastore username
    PASSWORD='password'  # replace with your 3.6 metastore password
    STACK_VERSION=$(hdp-select status hive-server2 | awk '{ print $3; }')
    /usr/hdp/$STACK_VERSION/hive/bin/schematool -upgradeSchema -url "jdbc:sqlserver://$SERVER;databaseName=$DATABASE;trustServerCertificate=false;encrypt=true;hostNameInCertificate=*.database.windows.net;" -userName "$USERNAME" -passWord "$PASSWORD" -dbType "mssql" --verbose
    ```

    > [!NOTE]
    > Dieses Hilfsprogramm verwendet Client-`beeline` zum Ausführen von SQL-Skripts in `/usr/hdp/$STACK_VERSION/hive/scripts/metastore/upgrade/mssql/upgrade-*.mssql.sql`.
    >
    > Die SQL-Syntax in diesen Skripts ist nicht unbedingt mit anderen Clienttools kompatibel. Beispielsweise verlangen [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) und der [Abfrage-Editor im Azure-Portal](../../azure-sql/database/connect-query-portal.md) nach jedem Befehl das Schlüsselwort `GO`.
    >
    > Wenn ein Skript aufgrund der Ressourcenkapazität oder von Transaktionstimeouts fehlschlägt, skalieren Sie die SQL-Datenbank hoch.

1. Überprüfen Sie die endgültige Version mit der Abfrage `select schema_version from dbo.version`.

    Die Ausgabe sollte dem folgenden bash-Befehl aus dem HDInsight 4.0-Cluster entsprechen.

    ```bash
    grep . /usr/hdp/$(hdp-select --version)/hive/scripts/metastore/upgrade/mssql/upgrade.order.mssql | tail -n1 | rev | cut -d'-' -f1 | rev
    ```

1. Löschen Sie den temporären HDInsight 4.0-Cluster.

### <a name="4-deploy-a-new-hdinsight-40-cluster"></a>4. Bereitstellen eines neuen HDInsight 4.0-Clusters

Erstellen Sie einen neuen HDInsight 4.0-Cluster, wobei Sie die gleichen Speicherkonten und den [aktualisierten Hive-Metastore auswählen](../hdinsight-use-external-metadata-stores.md#select-a-custom-metastore-during-cluster-creation).

* Der neue Cluster muss nicht über das gleiche Standarddateisystem verfügen.

* Wenn der Metastore Tabellen enthält, die sich in mehreren Speicherkonten befinden, müssen Sie diese Speicherkonten dem neuen Cluster hinzufügen, um auf diese Tabellen zuzugreifen. Weitere Informationen finden Sie unter [Hinzufügen zusätzlicher Speicherkonten zu HDInsight](../hdinsight-hadoop-add-storage.md).

* Wenn Hive-Aufträge fehlschlagen, weil kein Zugriff auf den Speicher möglich ist, überprüfen Sie, ob sich der Tabellenspeicherort in einem Speicherkonto befindet, das dem Cluster hinzugefügt wurde.

    Verwenden Sie den folgenden Hive-Befehl zum Ermitteln des Tabellenspeicherorts:

    ```sql
    SHOW CREATE TABLE ([db_name.]table_name|view_name);
    ```

### <a name="5-convert-tables-for-acid-compliance"></a>5. Konvertieren von Tabellen für die ACID-Konformität

Verwaltete Tabellen müssen für HDInsight 4.0 ACID-kompatibel sein. Führen Sie `strictmanagedmigration` auf HDInsight 4.0 mit der Eigenschaft `'external.table.purge'='true'` aus, um alle nicht per ACID verwalteten Tabellen in externe Tabellen zu konvertieren. Führen Sie folgenden Befehl auf dem Hauptknoten aus:

```bash
sudo su - hive
STACK_VERSION=$(hdp-select status hive-server2 | awk '{ print $3; }')
/usr/hdp/$STACK_VERSION/hive/bin/hive --config /etc/hive/conf --service strictmanagedmigration --hiveconf hive.strict.managed.tables=true -m automatic --modifyManagedTables
```

## <a name="secure-hive-across-hdinsight-versions"></a>Sichern von Hive in verschiedenen HDInsight-Versionen

HDInsight kann optional mit dem HDInsight-Enterprise-Sicherheitspaket (ESP) in Azure Active Directory integriert werden. Das Enterprise-Sicherheitspaket verwendet Kerberos und Apache Ranger, um die Berechtigungen bestimmter Ressourcen im Cluster zu verwalten. Für Hive in HDInsight 3.6 bereitgestellte Ranger-Richtlinien können wie folgt zu HDInsight 4.0 migriert werden:

1. Navigieren Sie in Ihrem HDInsight 3.6-Cluster zum Bereich „Ranger Service Manager“.
2. Navigieren Sie zur Richtlinie namens **HIVE**, und exportieren Sie die Richtlinie in eine JSON-Datei.
3. Stellen Sie sicher, dass alle Benutzer, auf die in der JSON-Datei mit der exportierten Richtlinie verwiesen wird, im neuen Cluster vorhanden sind. Wenn in der JSON-Datei mit der Richtlinie auf einen Benutzer verwiesen wird, der nicht im neuen Cluster vorhanden ist, fügen Sie den Benutzer dem neuen Cluster hinzu, oder entfernen Sie den Verweis aus der Richtlinie.
4. Navigieren Sie in Ihrem HDInsight 4.0-Cluster zum Bereich **Ranger Service Manager**.
5. Navigieren Sie zur Richtlinie namens **HIVE**, und importieren Sie die in Schritt 2 erstellte JSON-Datei mit der Ranger-Richtlinie.

## <a name="hive-changes-in-hdinsight-40-that-may-require-application-changes"></a>Änderungen an Hive in HDInsight 4.0, die möglicherweise Änderungen an Anwendungen erfordern

* Informationen zur gemeinsamen Nutzung des Metastores durch Spark und Hive für ACID-Tabellen finden Sie unter [Zusätzliche Konfiguration mit dem Hive Warehouse Connector](./apache-hive-warehouse-connector.md).

* HDInsight 4.0 verwendet die [speicherbasierte Autorisierung](https://cwiki.apache.org/confluence/display/Hive/Storage+Based+Authorization+in+the+Metastore+Server). Wenn Sie als ein anderer Benutzer als Hive Dateiberechtigungen ändern oder Ordner erstellen, treten wahrscheinlich Hive-Fehler aufgrund von Speicherberechtigungen auf. Um das Problem zu beheben, gewähren Sie dem Benutzer die Zugriffsberechtigung `rw-`. Weitere Informationen finden Sie im [Handbuch zu HDFS-Berechtigungen](https://hadoop.apache.org/docs/r2.7.1/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

* `HiveCLI` wurde durch `Beeline` ersetzt.

Weitere Änderungen finden Sie in der [Ankündigung von HDInsight 4.0](../hdinsight-version-release.md).

## <a name="further-reading"></a>Weitere Informationsquellen

* [Ankündigung von HDInsight 4.0](../hdinsight-version-release.md)
* [HDInsight 4.0 deep dive](https://azure.microsoft.com/blog/deep-dive-into-azure-hdinsight-4-0/) (Ausführliche Informationen zu HDInsight 4.0)
* [Hive 3-ACID-Tabellen](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.1.0/using-hiveql/content/hive_3_internals.html)
