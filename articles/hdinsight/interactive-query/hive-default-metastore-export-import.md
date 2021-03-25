---
title: Migrieren des standardmäßigen Hive-Metastore zu einem externen Metastore in Azure HDInsight
description: Migrieren des standardmäßigen Hive-Metastore zu einem externen Metastore in Azure HDInsight
author: kevxmsft
ms.author: kevx
ms.reviewer: ''
ms.service: hdinsight
ms.topic: how-to
ms.date: 11/4/2020
ms.openlocfilehash: 825204fe40125a65e8e6f27c6973417813700a9e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743361"
---
# <a name="migrate-default-hive-metastore-db-to-external-metastore-db"></a>Migrieren der standardmäßigen Hive-Metastore-Datenbank zu einer externen Metastore-Datenbank

In diesem Artikel wird erläutert, wie Sie Metadaten aus einer [standardmäßigen Metastore-Datenbank](../hdinsight-use-external-metadata-stores.md#default-metastore) für Hive zu einer externen SQL-Datenbank-Instanz in HDInsight migrieren. 

## <a name="why-migrate-to-external-metastore-db"></a>Gründe für die Migration zu einer externen Metastore-Datenbank

* Die standardmäßige Metastore-Datenbank ist auf die Basic-SKU beschränkt und kann Workloads im Produktionsmaßstab nicht verarbeiten.

* Eine externe Metastore-Datenbank ermöglicht Kunden die horizontale Skalierung von Hive-Computeressourcen durch Hinzufügen neuer HDInsight-Cluster, die dieselbe Metastore-Datenbank verwenden.

* Bei der Migration von HDInsight 3.6 zu 4.0 müssen Metadaten obligatorisch zu einer externen Metastore-Datenbank migriert werden, bevor ein Upgrade der Hive-Schemaversion ausgeführt werden kann. Weitere Informationen finden Sie unter [Migrieren von Azure HDInsight 3.6-Hive-Workloads zu HDInsight 4.0](./apache-hive-migrate-workloads.md).

Da die standardmäßige Metastore-Datenbank nur über begrenzte Computekapazität verfügt, empfiehlt es sich, während der Migration der Metadaten die Auslastung durch andere Aufträge im Cluster auf ein Minimum zu beschränken.

Die Quell- und Zieldatenbanken müssen dieselbe HDInsight-Version und dieselben Speicherkonten verwenden. Im Fall eines Upgrades von HDInsight 3.6 auf 4.0 führen Sie zuerst die Schritte in diesem Artikel aus. Danach führen Sie die [hier](./apache-hive-migrate-workloads.md) beschriebenen offiziellen Schritte für das Upgrade aus.

## <a name="prerequisites"></a>Voraussetzungen

Bei Verwendung von [Azure Data Lake Storage Gen1](../overview-data-lake-storage-gen1.md) sind die Speicherorte für Hive-Tabellen wahrscheinlich von den HDFS-Konfigurationen des Clusters für Azure Data Lake Storage Gen1 abhängig. Führen Sie die folgende Skriptaktion aus, um diese Speicherorte für andere Cluster portierbar zu machen. Weitere Informationen finden Sie unter [Skriptaktion für einen ausgeführten Cluster](../hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster).

Diese Aktion ähnelt dem Ersetzen von Symlinks durch ihre vollständigen Pfade.

|Eigenschaft | Wert |
|---|---|
|Bash-Skript-URI|`https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/hive-adl-expand-location-v01.sh`|
|Knotentyp(en)|Head|
|Parameter|""|

## <a name="migrate-with-exportimport-using-sqlpackage"></a>Migrieren mit Export/Import über sqlpackage

Nur nach dem 15.10.2020 erstellte HDInsight-Cluster unterstützen SQL-Export-/Importfunktionen für die standardmäßige Hive-Metastore-Datenbank über `sqlpackage`.

1. Installieren Sie [sqlpackage](https://docs.microsoft.com/sql/tools/sqlpackage-download#get-sqlpackage-net-core-for-linux) im Cluster.

2. Exportieren Sie mit dem folgenden Befehl die standardmäßige Metastore-Datenbank in eine BACPAC-Datei.

    ```bash
    wget "https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/hive_metastore_tool.py"
    SQLPACKAGE_FILE='/home/sshuser/sqlpackage/sqlpackage'  # replace with sqlpackage location
    TARGET_FILE='hive.bacpac'
    sudo python hive_metastore_tool.py --sqlpackagefile $SQLPACKAGE_FILE --targetfile $TARGET_FILE
    ```

3. Speichern Sie die BACPAC-Datei. Der Befehl könnte wie folgt aussehen.

    ```bash
    hdfs dfs -mkdir -p /bacpacs
    hdfs dfs -put $TARGET_FILE /bacpacs/
    ```

4. Importieren Sie die BACPAC-Datei in eine neue Datenbank, indem Sie die [hier](../../azure-sql/database/database-import.md) aufgeführten Schritte ausführen.

5. Die neue Datenbank kann als [externe Metastore-Datenbank in einem neuen HDInsight-Cluster konfiguriert](../hdinsight-use-external-metadata-stores.md#select-a-custom-metastore-during-cluster-creation) werden.

## <a name="migrate-using-hive-script"></a>Migrieren mithilfe eines Hive-Skripts

Cluster, die vor dem 15.10.2020 erstellt wurden, unterstützen das Exportieren/Importieren der standardmäßigen Metastore-Datenbank nicht.

Befolgen Sie für solche Cluster den Leitfaden [Kopieren von Hive-Tabellen zwischen Speicherkonten](./hive-migration-across-storage-accounts.md), und verwenden Sie dabei einen zweiten Cluster mit einer [externen Hive-Metastore-Datenbank](../hdinsight-use-external-metadata-stores.md#select-a-custom-metastore-during-cluster-creation). Der zweite Cluster kann dasselbe Speicherkonto nutzen, muss aber ein neues Standarddateisystem verwenden.

### <a name="option-to-shallow-copy"></a>Option zum Erstellen flacher Kopien
Wenn mithilfe der oben beschriebenen Schritte tiefe Kopien von Tabellen erstellt werden, verdoppelt sich der Speicherverbrauch. In diesem Fall müssen Sie die Daten im Quellspeichercontainer manuell bereinigen.
Stattdessen können Sie flache Kopien der Tabellen erstellen, wenn es sich nicht um transaktionale Daten handelt. In HDInsight 3.6 sind standardmäßig alle Hive-Tabellen nicht transaktional, in HDInsight 4.0 dagegen sind nur externe Tabellen nicht transaktional. Für transaktionale Tabellen müssen tiefe Kopien erstellt werden. Führen Sie die folgenden Schritte aus, um das Erstellen flacher Kopien für nicht transaktionale Tabellen zu ermöglichen:

1. Führen Sie das Skript [hive-ddls.sh](https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/hive-ddls.sh) im primären Hauptknoten des Quellclusters aus, um die DDL für jede Hive-Tabelle zu generieren.
2. Die DDL wird in ein lokales Hive-Skript namens `/tmp/hdi_hive_ddls.hql` geschrieben. Führen Sie dieses Skript in einem Zielcluster aus, der eine externe Hive-Metastore-Datenbank verwendet.

## <a name="verify-that-all-hive-tables-are-imported"></a>Überprüfen, ob alle Hive-Tabellen importiert wurden

Der folgende Befehl verwendet eine SQL-Abfrage in der Metastore-Datenbank, um alle Hive-Tabellen und ihre Datenspeicherorte auszugeben. Vergleichen Sie die Ausgabe des neuen und des alten Cluster, um sicherzustellen, dass in der neuen Metastore-Datenbank keine Tabellen fehlen.

```bash
SCRIPT_FNAME='hive_metastore_tool.py'
SCRIPT="/tmp/$SCRIPT_FNAME"
wget -O "$SCRIPT" "https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/$SCRIPT_FNAME"
OUTPUT_FILE='/tmp/hivetables.csv'
QUERY="SELECT DBS.NAME, TBLS.TBL_NAME, SDS.LOCATION FROM SDS, TBLS, DBS WHERE TBLS.SD_ID = SDS.SD_ID AND TBLS.DB_ID = DBS.DB_ID ORDER BY DBS.NAME, TBLS.TBL_NAME ASC;"
sudo python "$SCRIPT" --query "$QUERY" > $OUTPUT_FILE
```

## <a name="further-reading"></a>Weitere Informationsquellen

* [Migrieren von Azure HDInsight 3.6-Hive-Workloads zu HDInsight 4.0](./apache-hive-migrate-workloads.md)
* [Hive-Workloadmigration zu neuem Konto in Azure Storage](./hive-migration-across-storage-accounts.md)
* [Herstellen einer Verbindung mit Apache Beeline in HDInsight oder lokales Installieren](../hadoop/connect-install-beeline.md)
* [Szenario: Fehler vom Typ „Berechtigung verweigert“ beim Erstellen einer Apache Hive-Tabelle in Azure HDInsight](./interactive-query-troubleshoot-permission-error-create-table.md)
