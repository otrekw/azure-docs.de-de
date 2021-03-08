---
title: Hive-Workloadmigration zu neuem Konto in Azure Storage
description: Hive-Workloadmigration zu neuem Konto in Azure Storage
author: kevxmsft
ms.author: kevx
ms.reviewer: ''
ms.service: hdinsight
ms.topic: how-to
ms.date: 12/11/2020
ms.openlocfilehash: 0d62bebddb7751c168ba2e487b2391a40bbc6e67
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101744047"
---
# <a name="hive-workload-migration-to-new-account-in-azure-storage"></a>Hive-Workloadmigration zu neuem Konto in Azure Storage

Erfahren Sie, wie Sie Skriptaktionen verwenden, um Hive-Tabellen über Speicherkonten hinweg in HDInsight zu kopieren. Dies kann nützlich sein, wenn Sie zu [`Azure Data Lake Storage Gen2`](../hdinsight-hadoop-use-data-lake-storage-gen2.md) migrieren.

Informationen zum manuellen Kopieren einer einzelnen Hive-Tabelle in HDInsight 4.0 finden Sie unter [Hive-Export/-Import](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ImportExport).

## <a name="prerequisites"></a>Voraussetzungen

* Ein neuer HDInsight-Cluster mit folgenden Konfigurationen:
  * Sein Standarddateisystem ist das Zielspeicherkonto. Weitere Informationen finden Sie unter [Verwenden von Azure Storage mit Azure HDInsight-Clustern](../hdinsight-hadoop-use-blob-storage.md).
  * Die Clusterversion muss mit der des Quellclusters identisch sein.
  * Es wird eine neue externe Hive-Metastore-DB verwendet. Weitere Informationen finden Sie unter [Verwenden von externen Metadatenspeichern](../hdinsight-use-external-metadata-stores.md#select-a-custom-metastore-during-cluster-creation).
* Ein Speicherkonto, auf das die ursprünglichen und neue Cluster zugreifen können. Weitere Informationen zu zulässigen sekundären Speichertypen finden Sie unter [Hinzufügen zusätzlicher Speicherkonten zu HDInsight](../hdinsight-hadoop-add-storage.md) und [Speichertypen und Features](../hdinsight-hadoop-compare-storage-options.md#storage-types-and-features).

    Hier einige Optionen:
  * Fügen Sie das Zielspeicherkonto dem ursprünglichen Cluster hinzu.
  * Fügen Sie das ursprüngliche Speicherkonto dem neuen Cluster hinzu.
  * Fügen Sie den ursprünglichen und den neuen Clustern ein zwischengeschaltetes Speicherkonto hinzu.

## <a name="how-it-works"></a>Funktionsweise

Wir führen eine Skriptaktion aus, um Hive-Tabellen aus dem ursprünglichen Cluster in ein angegebenes HDFS-Verzeichnis zu exportieren. Weitere Informationen finden Sie unter [Skriptaktion für einen ausgeführten Cluster](../hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster).

Anschließend führen wir eine weitere Skriptaktion für den neuen Cluster aus, um die Hive-Tabellen aus dem HDFS-Verzeichnis zu importieren.

Das Skript erstellt die Tabellen im Standarddateisystem des neuen Clusters neu. Native Tabellen kopieren auch Ihre gespeicherten Daten. Nicht native Tabellen werden nur nach Definition kopiert. Weitere Informationen zu nicht nativen Tabellen finden Sie unter [Hive-Speicherhandler](https://cwiki.apache.org/confluence/display/Hive/StorageHandlers).

Der Pfad externer Tabellen, die sich nicht im Hive-Warehouseverzeichnis befinden, wird beibehalten. Andere Tabellen werden in den Hive-Standardpfad des Zielclusters kopiert. Siehe Hive-Eigenschaften `hive.metastore.warehouse.external.dir` und `hive.metastore.warehouse.dir`.

Die Skripts behalten keine benutzerdefinierten Dateiberechtigungen im Zielcluster bei.

> [!NOTE]
>
> Dieser Leitfaden unterstützt das Kopieren von Metadatenobjekten im Zusammenhang mit Hive-Datenbanken, Tabellen und Partitionen. Andere Metadatenobjekte müssen manuell neu erstellt werden.
>
> * Für `Views` unterstützt Hive den Befehl `SHOW VIEWS` ab Hive 2.2.0 für HDInsight 4.0. Verwenden Sie `SHOW CREATE TABLE` für die Sichtdefinition. Fragen Sie für frühere Versionen von Hive die Metastore-SQL-Datenbank ab, um Sichten anzuzeigen.
> * Verwenden Sie für die `Materialized Views` die Befehle `SHOW MATERIALIZED VIEWS`, `DESCRIBE FORMATTED` und `CREATE MATERIALIZED VIEW`. Ausführliche Informationen finden Sie unter [Materialisierte Sichten](https://cwiki.apache.org/confluence/display/Hive/Materialized+views).
> * Für `Constraints` (unterstützt ab Hive 2.1.0 in HDInsight 4.0) verwenden Sie `DESCRIBE EXTENDED`, um Einschränkungen für eine Tabelle aufzulisten, und `ALTER TABLE`, um Einschränkungen hinzuzufügen. Weitere Informationen finden Sie unter [ALTER TABLE-Einschränkungen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-AlterTableConstraints).

## <a name="copy-hive-tables"></a>Kopieren von Hive-Tabellen

1. Wenden Sie die Skriptaktion „export“ auf den ursprünglichen Cluster mit den folgenden Feldern an.

    Hierdurch werden zwischengeschaltete Hive-Skripts generiert und ausgeführt. Sie werden im angegebenen `<hdfs-export-path>` gespeichert.

    Optional können Sie `--run-script=false` verwenden, um sie vor der manuellen Ausführung anzupassen.

    |Eigenschaft | Wert |
    |---|---|
    |Bash-Skript-URI|`https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/export-hive-data-v01.sh`|
    |Knotentyp(en)|Head|
    |Parameter|`<hdfs-export-path>` `--run-script`|

    ```sh
    usage: generate Hive export and import scripts and export Hive data to specified HDFS path
           [--run-script={true,false}]
           hdfs-export-path

    positional arguments:

        hdfs-export-path      remote HDFS directory to write export data to

    optional arguments:
        --run-script={true,false}
                            whether to execute the generated Hive export script
                            (default: true)
    ```

2. Nach erfolgreichem Abschluss des Exports wenden Sie die Skriptaktion „import“ auf den neuen Cluster mit den folgenden Feldern an.

    |Eigenschaft | Wert |
    |---|---|
    |Bash-Skript-URI|`https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/import-hive-data-v01.sh`|
    |Knotentyp(en)|Head|
    |Parameter|`<hdfs-export-path>`|

    ```sh
    usage: download Hive import script from specified HDFS path and execute it
           hdfs-export-path

    positional arguments:

      hdfs-export-path      remote HDFS directory to download Hive import script from

    ```

## <a name="verification"></a>Überprüfung

Laden Sie das Skript herunter, und führen Sie es als root-Benutzer [`hive_contents.sh`](https://hdiconfigactions.blob.core.windows.net/linuxhivemigrationv01/hive_contents.sh) auf dem primären Knoten jedes Clusters aus. Vergleichen Sie den Inhalt der Ausgabedatei `/tmp/hive_contents.out`. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit HDInsight (Apache Hadoop) per SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="cleanup-additional-storage-usage"></a>Bereinigen zusätzlicher Speicherauslastung

Nachdem die Speichermigration abgeschlossen und überprüft wurde, können Sie die Daten im angegebenen HDFS-Exportpfad löschen.

Die Option ist die Verwendung des HDFS-Befehls `hdfs dfs -rm -R`.

## <a name="option-reduce-additional-storage-usage"></a>Option: Verringern der zusätzlichen Speicherauslastung

Die Aktion zum Exportieren eines Skripts verdoppelt wahrscheinlich die Speicherauslastung aufgrund von Hive. Allerdings ist es möglich, die zusätzliche Speicherauslastung durch manuelles Migrieren jeweils einer Datenbank oder Tabelle einzuschränken.

1. Geben Sie `--run-script=false` an, um die Ausführung des generierten Hive-Skripts zu überspringen. Die Hive-Export- und -Importskripts werden weiterhin im Exportpfad gespeichert.

2. Führen Sie Codeausschnitte der Hive-Export- und -Importskripts datenbankweise oder tabellenweise aus, und bereinigen Sie den Exportpfad nach jeder migrierten Datenbank oder Tabelle.

## <a name="next-steps"></a>Nächste Schritte

* [Azure Data Lake Storage Gen2](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
* [Verwenden von externen Metadatenspeichern](../hdinsight-use-external-metadata-stores.md#select-a-custom-metastore-during-cluster-creation)
* [Speichertypen und Features](../hdinsight-hadoop-compare-storage-options.md#storage-types-and-features)
* [Skriptaktion auf einem ausgeführten Cluster](../hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster)
