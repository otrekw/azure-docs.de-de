---
title: Migrieren eines HBase-Clusters zu einer neuen Version und einem neuen Speicherkonto – Azure HDInsight
description: Erfahren Sie, wie Sie einen Apache HBase-Cluster in Azure HDInsight zu einer neueren Version mit einem anderen Azure Storage-Konto migrieren.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 05/06/2021
ms.openlocfilehash: 4ed4de8c134575bba1b961f918216eb3cca9b6b1
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111963983"
---
# <a name="migrate-apache-hbase-to-a-new-version-and-storage-account"></a>Migrieren von Apache HBase zu einer neuen Version und einem neuen Speicherkonto

In diesem Artikel wird erläutert, wie Sie ein Update für Ihren Apache HBase-Cluster in Azure HDInsight auf eine neuere Version mit einem anderen Azure Storage-Konto durchführen.

Dieser Artikel ist nur relevant, wenn Sie verschiedene Speicherkonten für Ihre Quell- und Zielcluster verwenden müssen. Informationen zum Upgrade von Versionen mit demselben Speicherkonto für Ihre Quell- und Zielcluster finden Sie unter [Migrieren eines Apache HBase-Clusters zu einer neuen Version.](apache-hbase-migrate-new-version.md)

Die Downtime durch das Upgrade sollte nur wenige Minuten dauern. Sie ist auf das Leeren sämtlicher In-Memory-Daten und auf die Konfiguration und den Neustart der Dienste im neuen Cluster zurückzuführen. Die Ergebnisse variieren je nach Kontenanzahl, Datenmenge und anderen Variablen.

## <a name="review-apache-hbase-compatibility"></a>Überprüfen der Apache HBase-Kompatibilität

Vergewissern Sie sich vor dem Apache HBase-Upgrade, dass die HBase-Versionen im Quell- und Zielcluster kompatibel sind. Überprüfen Sie die HBase-Versionskompatibilitätsmatrix und die Versionshinweise im [HBase-Referenzleitfaden](https://hbase.apache.org/book.html#upgrading), um sicherzustellen, dass Ihre Anwendung mit der neuen Version kompatibel ist.

Hier sehen Sie ein Beispiel für eine Kompatibilitätsmatrix. Hierbei steht „Y“ für Kompatibilität und „N“ für eine potenzielle Inkompatibilität.

| Kompatibilitätstyp | Hauptversion| Nebenversion | Patch |
| --- | --- | --- | --- |
| Client-Server-Verbindungskompatibilität | N | J | J |
| Server-Server-Kompatibilität | N | J | J |
| Dateiformatkompatibilität | N | J | J |
| Client-API-Kompatibilität | N | J | J |
| Client-Binärkompatibilität | N | N | J |
| **Serverseitig eingeschränkte API-Kompatibilität** |  |  |  |
| Stable | N | J | J |
| In Entwicklung | N | N | J |
| Instabil | N | N | N |
| Abhängigkeitskompatibilität | N | J | J |
| Betriebskompatibilität | N | N | J |

Kritische Inkompatibilitäten sollten in den Versionshinweisen für die HBase-Version beschrieben werden. Testen Sie Ihre Anwendung in einem Cluster mit der Zielversion von HDInsight und HBase.

Weitere Informationen über HDInsight-Versionen und die Kompatibilität finden Sie unter [Azure HDInsight-Versionen](../hdinsight-component-versioning.md).

## <a name="apache-hbase-cluster-migration-overview"></a>Übersicht über die Migration von Apache HBase-Clustern

Führen Sie die folgenden einfachen Schritte aus, um Upgrade und Migration für einen Apache HBase-Cluster in Azure HDInsight auf ein neues Speicherkonto durchzuführen. Ausführlichere Anweisungen finden Sie in den ausführlichen Schritten und Befehlen.

Bereiten Sie den Quellcluster vor:
1. Beenden Sie die Datenerfassung.
1. Leeren Sie die Memstore-Daten.
1. Beenden Sie HBase über Ambari.
1. Sichern Sie das WAL-Verzeichnis (Write Ahead Log), wenn Sie Cluster mit beschleunigten Schreibvorgängen verwenden.

Bereiten Sie den Zielcluster vor:
1. Erstellen Sie den Zielcluster.
1. Beenden Sie HBase über Ambari.
1. Bereinigen Sie die Zookeeper-Daten.
1. Wechseln zum Benutzer „HBase“.

Führen Sie die Migration durch:
1. Bereinigen Sie das Zieldateisystem, migrieren Sie die Daten, und entfernen Sie `/hbase/hbase.id`.
1. Bereinigen und migrieren Sie das WAL-Verzeichnis.
1. Starten Sie alle Dienste über den Ambari-Zielcluster.
1. Überprüfen Sie HBase.
1. Löschen Sie den Quellcluster.

## <a name="detailed-migration-steps-and-commands"></a>Ausführliche Schritte und Befehle für die Migration

Verwenden Sie die folgenden ausführlichen Schritte und Befehle zum Migrieren Ihres Apache HBase-Clusters mit einem neuen Speicherkonto.

### <a name="prepare-the-source-cluster"></a>Vorbereiten des Quellclusters

1. Beenden Sie die Datenerfassung im HBase-Quellcluster.
   
1. Leeren Sie den HBase-Quellcluster, für den Sie das Upgrade durchführen.
   
   HBase schreibt eingehende Daten in einen In-Memory-Speicher, der als *Memstore* bezeichnet wird. Wenn der Memstore eine bestimmte Größe erreicht, wird er von HBase zur langfristigen Speicherung im Speicherkonto des Clusters auf den Datenträger geschrieben. Beim Löschen des Quellclusters nach einem Upgrade werden auch alle Daten in den Memstore-Instanzen gelöscht. Zum Beibehalten der Daten müssen Sie die Memstore-Instanzen aller Tabellen manuell auf den Datenträger leeren, bevor Sie das Upgrade durchführen.
   
   Sie können die Memstore-Daten leeren, indem Sie das Skript [flush_all_tables.sh](https://github.com/Azure/hbase-utils/blob/master/scripts/flush_all_tables.sh) über das GitHub-Repository [hbase-utils](https://github.com/Azure/hbase-utils/) durchführen.
   
   Sie können die Memstore-Daten auch leeren, indem Sie den folgenden HBase-Shellbefehl im HDInsight-Cluster ausführen:
   
   ```bash
   hbase shell
   flush "<table-name>"
   ```
   
1. Melden Sie sich mit `https://<OLDCLUSTERNAME>.azurehdinsight.net` bei [Apache Ambari](https://ambari.apache.org/) auf dem Quellcluster an, und beenden Sie die HBase-Dienste.
   
1. Aktivieren Sie über die Eingabeaufforderung zur Bestätigung das Kontrollkästchen, um den Wartungsmodus für HBase zu aktivieren.
   
   Weitere Informationen zur Verbindungsherstellung mit Ambari sowie zur Verwendung finden Sie unter [Verwalten von HDInsight-Clustern mithilfe der Ambari-Webbenutzeroberfläche](../hdinsight-hadoop-manage-ambari.md).
   
1. Wenn Ihr HBase-Quellcluster nicht über das Feature [Beschleunigte Schreibvorgänge](apache-hbase-accelerated-writes.md) verfügt, überspringen Sie diesen Schritt. Sichern Sie für HBase-Quellcluster mit beschleunigten Schreibvorgängen das WAL-Verzeichnis unter HDFS, indem Sie die folgenden Befehle über eine SSH-Sitzung auf einem beliebigen Zookeeper-Knoten des Quellclusters oder Workerknoten ausführen.
   
   ```bash
   hdfs dfs -mkdir /hbase-wal-backup
   hdfs dfs -cp hdfs://mycluster/hbasewal /hbase-wal-backup
   ```

### <a name="prepare-the-destination-cluster"></a>Vorbereiten des Zielclusters

1. Richten Sie im Azure-Portal [ein neues HDInsight-Zielcluster ein](../hdinsight-hadoop-provision-linux-clusters.md), der ein anderes Speicherkonto als das Quellcluster verwendet.
   
1. Melden Sie sich auf dem neuen Cluster unter `https://<NEWCLUSTERNAME>.azurehdinsight.net` bei [Apache Ambari](https://ambari.apache.org/) an, und beenden Sie die HBase-Dienste.
   
1. Bereinigen Sie die Zookeeper-Daten auf dem Zielcluster, indem Sie die folgenden Befehle auf einem beliebigen Zookeeper-Knoten oder -Workerknoten ausführen:
   
   ```bash
   hbase zkcli
   rmr /hbase-unsecure
   quit
   ```
   
1. Wechseln Sie den Benutzer in „HBase“, indem Sie `sudo su hbase` ausführen.

### <a name="clean-and-migrate-the-file-system-and-wal"></a>Bereinigen und Migrieren von Dateisystem und WAL

Führen Sie die folgenden Befehle abhängig von Ihrer HDI-Quellversion und dem Vorhandensein beschleunigter Schreibvorgänge auf Ihren Quell- und Zielclustern aus. Der Zielcluster weist immer HDI-Version 4.0 auf, da HDI 3.6 im Basic-Support enthalten ist und für neue Cluster nicht empfohlen wird.

- [Der Quellcluster entspricht HDI 3.6 mit beschleunigten Schreibvorgängen, und der Zielcluster verfügt über beschleunigte Schreibvorgänge](#the-source-cluster-is-hdi-36-or-hdi-40-with-accelerated-writes-and-the-destination-cluster-has-accelerated-writes).
- [Der Quellcluster entspricht HDI 3.6 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt über beschleunigte Schreibvorgänge](#the-source-cluster-is-hdi-36-without-accelerated-writes-and-the-destination-cluster-has-accelerated-writes).
- [Der Quellcluster entspricht HDI 3.6 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt nicht über beschleunigte Schreibvorgänge](#the-source-cluster-is-hdi-36-without-accelerated-writes-and-the-destination-cluster-doesnt-have-accelerated-writes).
- [Der Quellcluster entspricht HDI 4.0 mit beschleunigten Schreibvorgängen, und der Zielcluster verfügt über beschleunigte Schreibvorgänge](#the-source-cluster-is-hdi-36-or-hdi-40-with-accelerated-writes-and-the-destination-cluster-has-accelerated-writes).
- [Der Quellcluster entspricht HDI 4.0 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt über beschleunigte Schreibvorgänge](#the-source-cluster-is-hdi-40-without-accelerated-writes-and-the-destination-cluster-has-accelerated-writes).
- [Der Quellcluster entspricht HDI 4.0 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt nicht über beschleunigte Schreibvorgänge](#the-source-cluster-is-hdi-40-without-accelerated-writes-and-the-destination-cluster-doesnt-have-accelerated-writes).

Für das Speicherkonto entspricht `<container-endpoint-url>` der URL `https://<storageaccount>.blob.core.windows.net/<container-name>`. Übergeben Sie das SAS-Token für das Speicherkonto ganz am Ende der URL.

- Für den WASB-Speichertyp entspricht `<container-fullpath>` dem Pfad `wasbs://<container-name>@<storageaccount>.blob.core.windows.net`.
- Für den Azure Data Lake Storage Gen2-Speichertyp entspricht `<container-fullpath>` dem Pfad `abfs://<container-name>@<storageaccount>.dfs.core.windows.net`.

#### <a name="copy-commands"></a>Kopierbefehle

Der HDFS-Kopierbefehl lautet `hdfs dfs <copy properties starting with -D> -cp`.
 
Verwenden Sie `hadoop distcp`, um die Leistung beim Kopieren von Dateien, die nicht in einem Seitenblob enthalten sind, zu verbessern: `hadoop distcp <copy properties starting with -D>`
 
Um den Schlüssel des Speicherkontos zu übergeben, verwenden Sie:
- `-Dfs.azure.account.key.<storageaccount>.blob.core.windows.net='<storage account key>'`
- `-Dfs.azure.account.keyprovider.<storageaccount>.blob.core.windows.net=org.apache.hadoop.fs.azure.SimpleKeyProvider`

Sie können auch [AzCopy](../../storage/common/storage-ref-azcopy.md) verwenden, um die Leistung beim Kopieren von HBase-Datendateien zu verbessern.
   
1. Führen Sie den AzCopy-Befehl aus:
   
   ```bash
   azcopy cp "<source-container-endpoint-url>/hbase" "<target-container-endpoint-url>" --recursive
   ```

1. Führen Sie diesen Schritt nach dem Kopieren durch, wenn das Zielspeicherkonto ein Azure-Blobspeicher ist. Überspringen Sie diesen Schritt, wenn das Zielspeicherkonto ein Data Lake Storage Gen2-Konto ist.
   
   Der Hadoop WASB-Treiber verwendet spezielle Blobs der Größe 0, die jedem Verzeichnis entsprechen. AzCopy überspringt diese Dateien beim Kopieren. Einige WASB-Vorgänge verwenden diese Blobs, daher müssen Sie sie im Zielcluster erstellen. Führen Sie den folgenden Hadoop-Befehl auf einem beliebigen Knoten im Zielcluster aus, um die Blobs zu erstellen:
   
   ```bash
   sudo -u hbase hadoop fs -chmod -R 0755 /hbase
   ```

Sie können AzCopy unter [Erste Schritte mit AzCopy](../../storage/common/storage-use-azcopy-v10.md) herunterladen. Weitere Informationen zur Verwendung von AzCopy finden Sie unter [azcopy copy](../../storage/common/storage-ref-azcopy-copy.md).

#### <a name="the-source-cluster-is-hdi-36-or-hdi-40-with-accelerated-writes-and-the-destination-cluster-has-accelerated-writes"></a>Der Quellcluster entspricht HDI 3.6 oder HDI 4.0 mit beschleunigten Schreibvorgängen, und der Zielcluster verfügt über beschleunigte Schreibvorgänge

1. Führen Sie die folgenden Befehle aus, um das Dateisystem zu bereinigen und die Daten zu migrieren:
   
   ```bash
   hdfs dfs -rm -r /hbase 
   hadoop distcp <source-container-fullpath>/hbase /
   ```
   
1. Entfernen Sie `hbase.id`, indem Sie `hdfs dfs -rm /hbase/hbase.id` ausführen.
   
1. Führen Sie zum Bereinigten und Migrieren von WAL die folgenden Befehle aus:
   
   ```bash
   hdfs dfs -rm -r hdfs://<destination-cluster>/hbasewal
   hdfs dfs -cp <source-container-fullpath>/hbase-wal-backup/hbasewal hdfs://<destination-cluster>/hbasewal
   ```

#### <a name="the-source-cluster-is-hdi-36-without-accelerated-writes-and-the-destination-cluster-has-accelerated-writes"></a>Der Quellcluster entspricht HDI 3.6 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt über beschleunigte Schreibvorgänge

1. Führen Sie die folgenden Befehle aus, um das Dateisystem zu bereinigen und die Daten zu migrieren:
   
   ```bash
   hdfs dfs -rm -r /hbase 
   hdfs dfs -Dfs.azure.page.blob.dir="/hbase/WALs,/hbase/MasterProcWALs,/hbase/oldWALs,/hbase-wals" -cp <source-container-fullpath>/hbase /
   hdfs dfs -rm -r /hbase/*WALs
   ```
   
1. Entfernen Sie `hbase.id`, indem Sie `hdfs dfs -rm /hbase/hbase.id` ausführen.
   
1. Führen Sie zum Bereinigten und Migrieren von WAL die folgenden Befehle aus:
   
   ```bash
   hdfs dfs -rm -r hdfs://<destination-cluster>/hbasewal/*
   hdfs dfs -Dfs.azure.page.blob.dir="/hbase/WALs,/hbase/MasterProcWALs,/hbase/oldWALs,/hbase-wals" -cp <source-container-fullpath>/hbase/*WALs hdfs://<destination-cluster>/hbasewal
   ```

#### <a name="the-source-cluster-is-hdi-36-without-accelerated-writes-and-the-destination-cluster-doesnt-have-accelerated-writes"></a>Der Quellcluster entspricht HDI 3.6 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt nicht über beschleunigte Schreibvorgänge

1. Führen Sie die folgenden Befehle aus, um das Dateisystem zu bereinigen und die Daten zu migrieren:
   
   ```bash
   hdfs dfs -rm -r /hbase 
   hdfs dfs -Dfs.azure.page.blob.dir="/hbase/WALs,/hbase/MasterProcWALs,/hbase/oldWALs,/hbase-wals" -cp <source-container-fullpath>/hbase /
   hdfs dfs -rm -r /hbase/*WALs
   ```
   
1. Entfernen Sie `hbase.id`, indem Sie `hdfs dfs -rm /hbase/hbase.id` ausführen.
   
1. Führen Sie zum Bereinigten und Migrieren von WAL die folgenden Befehle aus:
   
   ```bash
   hdfs dfs -rm -r /hbase-wals/*
   hdfs dfs -Dfs.azure.page.blob.dir="/hbase/WALs,/hbase/MasterProcWALs,/hbase/oldWALs,/hbase-wals" -cp <source-container-fullpath>/hbase/*WALs /hbase-wals
   ```

#### <a name="the-source-cluster-is-hdi-40-without-accelerated-writes-and-the-destination-cluster-has-accelerated-writes"></a>Der Quellcluster entspricht HDI 4.0 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt über beschleunigte Schreibvorgänge

1. Führen Sie die folgenden Befehle aus, um das Dateisystem zu bereinigen und die Daten zu migrieren:
   
   ```bash
   hdfs dfs -rm -r /hbase 
   hadoop distcp <source-container-fullpath>/hbase /
   ```
   
1. Entfernen Sie `hbase.id`, indem Sie `hdfs dfs -rm /hbase/hbase.id` ausführen.
   
1. Führen Sie zum Bereinigten und Migrieren von WAL die folgenden Befehle aus:
   
   ```bash
   hdfs dfs -rm -r hdfs://<destination-cluster>/hbasewal
   hdfs dfs -Dfs.azure.page.blob.dir="/hbase-wals" -cp <source-container-fullpath>/hbase-wals hdfs://<destination-cluster>/hbasewal
   ```

#### <a name="the-source-cluster-is-hdi-40-without-accelerated-writes-and-the-destination-cluster-doesnt-have-accelerated-writes"></a>Der Quellcluster entspricht HDI 4.0 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt nicht über beschleunigte Schreibvorgänge

1. Führen Sie die folgenden Befehle aus, um das Dateisystem zu bereinigen und die Daten zu migrieren:
   
   ```bash
   hdfs dfs -rm -r /hbase 
   hadoop distcp <source-container-fullpath>/hbase /
   ```
   
1. Entfernen Sie `hbase.id`, indem Sie `hdfs dfs -rm /hbase/hbase.id` ausführen.
   
1. Führen Sie zum Bereinigten und Migrieren von WAL die folgenden Befehle aus:
   
   ```bash
   hdfs dfs -rm -r /hbase-wals/*
   hdfs dfs -Dfs.azure.page.blob.dir="/hbase-wals" -cp <source-container-fullpath>/hbase-wals /
   ```

### <a name="complete-the-migration"></a>Fertigstellen der Migration

1. Speichern Sie Ihre Änderungen im Zielcluster, und starten Sie alle erforderlichen Dienste wie von Ambari angegeben neu.
   
1. Verweisen Sie Ihre Anwendung auf den Zielcluster.
   
   > [!NOTE]  
   > Der statische DNS-Name Ihrer Anwendung wird geändert, wenn Sie das Upgrade durchführen. Anstatt eine Hartcodierung für diesen DNS-Namen durchzuführen, können Sie einen CNAME-Eintrag in den DNS-Einstellungen Ihres Domänennamens konfigurieren, die auf den Namen des Clusters verweisen. Eine weitere Möglichkeit ist die Verwendung einer Konfigurationsdatei für Ihre Anwendung, die Sie ohne erneute Bereitstellung aktualisieren können.
   
1. Starten Sie die Datenerfassung.
   
1. Überprüfen Sie die HBase-Konsistenz und die einfachen DDL- (Datendefinitionssprache) und DML-Vorgänge (Datenbearbeitungssprache).
   
1. Wenn der Zielcluster zufriedenstellend ist, löschen Sie den Quellcluster.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Apache HBase](https://hbase.apache.org/) und zum Upgraden von HDInsight-Clustern finden Sie in den folgenden Artikeln:

- [Aktualisieren eines HDInsight-Clusters auf eine neuere Version](../hdinsight-upgrade-cluster.md)
- [Verwalten von HDInsight-Clustern mithilfe der Apache Ambari-Webbenutzeroberfläche](../hdinsight-hadoop-manage-ambari.md)
- [Versionen von Azure HDInsight](../hdinsight-component-versioning.md)
- [Optimieren von Apache HBase](../optimize-hbase-ambari.md)