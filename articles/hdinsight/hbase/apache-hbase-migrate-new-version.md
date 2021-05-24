---
title: Migrieren eines HBase-Clusters zu einer neuen Version – Azure HDInsight
description: Hier erfahren Sie, wie Sie Apache HBase-Cluster in Azure HDInsight zu einer neueren Version migrieren.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 05/06/2021
ms.openlocfilehash: 1d6f2d66a00601334a9cab4695b4e78c77a67917
ms.sourcegitcommit: 3de22db010c5efa9e11cffd44a3715723c36696a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2021
ms.locfileid: "109654788"
---
# <a name="migrate-an-apache-hbase-cluster-to-a-new-version"></a>Migrieren eines Apache HBase-Clusters zu einer neuen Version

In diesem Artikel wird erläutert, wie Sie ein Update für Ihren Apache HBase-Cluster in Azure HDInsight auf eine neuere Version durchführen.

Dieser Artikel ist nur relevant, wenn Sie dasselbe Azure Storage-Konto für Ihre Quell- und Zielcluster verwenden. Informationen zum Durchführen eines Upgrades mit einem neuen oder anderen Azure Storage-Konto für Ihren Zielcluster finden Sie unter [Migrieren von Apache HBase zu einer neuen Version mit einem neuen Azure Storage-Konto](apache-hbase-migrate-new-version-new-storage-account.md).

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

Weitere Informationen über HDInsight-Versionen und die Kompatibilität finden Sie unter [Azure HDInsight-Versionen](../hdinsight-component-versioning.md).

## <a name="apache-hbase-cluster-migration-overview"></a>Übersicht über die Migration von Apache HBase-Clustern

Führen Sie die folgenden einfachen Schritte aus, um ein Upgrade für Ihre Apache HBase-Cluster in Azure HDInsight durchzuführen. Ausführlichere Anweisungen finden Sie in den ausführlichen Schritten und Befehlen.

Bereiten Sie den Quellcluster vor:
1. Beenden Sie die Datenerfassung.
1. Leeren Sie die Memstore-Daten.
1. Beenden Sie HBase über Ambari.
1. Sichern Sie das WAL-Verzeichnis (Write Ahead Log), wenn Sie Cluster mit beschleunigten Schreibvorgängen verwenden.

Bereiten Sie den Zielcluster vor:
1. Erstellen Sie den Zielcluster.
1. Beenden Sie HBase über Ambari.
1. Ändern Sie `fs.defaultFS` in der HDFS-Dienstkonfiguration so, dass auf den ursprünglichen Quellclustercontainer verwiesen wird.
1. Ändern Sie `hbase.rootdir` in der HBase-Dienstkonfiguration so, dass auf den ursprünglichen Quellclustercontainer verwiesen wird, wenn Sie Cluster mit beschleunigten Schreibvorgängen verwenden.
1. Bereinigen Sie die Zookeeper-Daten.

Führen Sie die Migration durch:
1. Bereinigen und migrieren Sie das WAL-Verzeichnis.
1. Kopieren Sie die Apps aus dem Standardcontainer des Zielclusters in den ursprünglichen Quellcontainer.
1. Starten Sie alle Dienste über den Ambari-Zielcluster.
1. Überprüfen Sie HBase.
1. Löschen Sie den Quellcluster.

## <a name="detailed-migration-steps-and-commands"></a>Ausführliche Schritte und Befehle für die Migration

Verwenden Sie diese ausführlichen Schritte und Befehle zum Migrieren Ihres Apache HBase-Clusters.

### <a name="prepare-the-source-cluster"></a>Vorbereiten des Quellclusters

1. Beenden Sie die Datenerfassung im HBase-Quellcluster.
   
1. Leeren Sie den HBase-Quellcluster, für den Sie das Upgrade durchführen.
   
   HBase schreibt eingehende Daten in einen In-Memory-Speicher, der als *Memstore* bezeichnet wird. Wenn der Memstore eine bestimmte Größe erreicht, wird er von HBase zur langfristigen Speicherung im Speicherkonto des Clusters auf den Datenträger geschrieben. Beim Löschen des Quellclusters nach einem Upgrade werden auch alle Daten in den Memstore-Instanzen gelöscht. Zum Beibehalten der Daten müssen Sie die Memstore-Instanzen aller Tabellen manuell auf den Datenträger leeren, bevor Sie das Upgrade durchführen.
   
   Sie können die Memstore-Daten leeren, indem Sie das Skript [flush_all_tables.sh](https://github.com/Azure/hbase-utils/blob/master/scripts/flush_all_tables.sh) über das GitHub-Repository [Azure hbase-utils](https://github.com/Azure/hbase-utils/) durchführen.
   
   Sie können die Memstore-Daten auch leeren, indem Sie den folgenden HBase-Shellbefehl über den HDInsight-Cluster ausführen:
   
   ```bash
   hbase shell
   flush "<table-name>"
   ```
   
1. Melden Sie sich mit `https://<OLDCLUSTERNAME>.azurehdinsight.net` bei [Apache Ambari](https://ambari.apache.org/) auf dem Quellcluster an, und beenden Sie die HBase-Dienste.
   
1. Aktivieren Sie über die Eingabeaufforderung zur Bestätigung das Kontrollkästchen, um den Wartungsmodus für HBase zu aktivieren.
   
   Weitere Informationen zur Verbindungsherstellung mit Ambari sowie zur Verwendung finden Sie unter [Verwalten von HDInsight-Clustern mithilfe der Ambari-Webbenutzeroberfläche](../hdinsight-hadoop-manage-ambari.md).
   
1. Wenn Ihr HBase-Quellcluster nicht über das Feature [Beschleunigte Schreibvorgänge](apache-hbase-accelerated-writes.md) verfügt, überspringen Sie diesen Schritt. Sichern Sie für HBase-Quellcluster mit beschleunigten Schreibvorgängen das WAL-Verzeichnis unter HDFS, indem Sie die folgenden Befehle über eine SSH-Sitzung auf einem beliebigen Zookeeper-Knoten oder -Workerknoten des Quellclusters ausführen.
   
   ```bash
   hdfs dfs -mkdir /hbase-wal-backup
   hdfs dfs -cp hdfs://mycluster/hbasewal /hbase-wal-backup
   ```
   
### <a name="prepare-the-destination-cluster"></a>Vorbereiten des Zielclusters

1. [Richten Sie im Azure-Portal einen neuen HDInsight-Zielcluster](../hdinsight-hadoop-provision-linux-clusters.md) mithilfe desselben Speicherkontos ein, das für den Quellcluster verwendet wurde, aber verwenden Sie einen anderen Containernamen:

   
1. Melden Sie sich auf dem neuen Cluster unter `https://<NEWCLUSTERNAME>.azurehdinsight.net` bei [Apache Ambari](https://ambari.apache.org/) an, und beenden Sie die HBase-Dienste.
   
1. Ändern Sie unter **Services** > **HDFS** > **Configs** > **Advanced** > **Advanced core-site** (Dienste > HDFS > Konfiguration > Erweitert > Core-Website (erweitert)) die HDFS-Einstellung `fs.defaultFS`, sodass diese auf den Containernamen des ursprünglichen Quellclusters verweist. Beispielsweise sollte die Einstellung im folgenden Screenshot in `wasbs://hbase-upgrade-old-2021-03-22` geändert werden.
   
   :::image type="content" source="./media/apache-hbase-migrate-new-version/hdfs-advanced-settings.png" alt-text="Ändern des Containernamens in Ambari unter „Services > HDFS > Configs > Advanced > Advanced core-site“" border="false":::
   
1. Wenn Ihr Zielcluster über das Feature für beschleunigte Schreibvorgänge verfügt, ändern Sie den Pfad `hbase.rootdir` so, dass er auf den Containernamen des ursprünglichen Quellclusters verweist. Der folgende Pfad sollte beispielsweise in `hbase-upgrade-old-2021-03-22` geändert werden. Überspringen Sie diesen Schritt, wenn Ihr Cluster nicht über beschleunigte Schreibvorgänge verfügt.
   
   :::image type="content" source="./media/apache-hbase-migrate-new-version/change-container-name-for-hbase-rootdir.png" alt-text="Ändern des Containernamens für das HBase-Stammverzeichnis in Ambari" border="true":::
   
1. Bereinigen Sie die Zookeeper-Daten auf dem Zielcluster, indem Sie die folgenden Befehle auf einem beliebigen Zookeeper-Knoten oder -Workerknoten ausführen:
   
   ```bash
   hbase zkcli
   rmr /hbase-unsecure
   quit
   ```
   
### <a name="clean-and-migrate-wal"></a>Bereinigen und Migrieren des WAL-Verzeichnisses

Führen Sie die folgenden Befehle abhängig von Ihrer HDI-Quellversion und dem Vorhandensein beschleunigter Schreibvorgänge auf Ihren Quell- und Zielclustern aus.

- Der Zielcluster weist immer HDI-Version 4.0 auf, da HDI 3.6 im Basic-Support enthalten ist und für neue Cluster nicht empfohlen wird.
- Der HDFS-Kopierbefehl lautet `hdfs dfs <copy properties starting with -D> -cp <source> <destination> # Serial execution`.

> [!NOTE]  
> - Für den WASB-Speichertyp entspricht `<source-container-fullpath>` dem Pfad `wasbs://<source-container-name>@<storageaccountname>.blob.core.windows.net`.
> - Für den Azure Data Lake Storage Gen2-Speichertyp entspricht `<source-container-fullpath>` dem Pfad `abfs://<source-container-name>@<storageaccountname>.dfs.core.windows.net`.

- [Der Quellcluster entspricht HDI 3.6 mit beschleunigten Schreibvorgängen, und der Zielcluster verfügt über beschleunigte Schreibvorgänge](#the-source-cluster-is-hdi-36-or-hdi-40-with-accelerated-writes-and-the-destination-cluster-has-accelerated-writes).
- [Der Quellcluster entspricht HDI 3.6 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt über beschleunigte Schreibvorgänge](#the-source-cluster-is-hdi-36-without-accelerated-writes-and-the-destination-cluster-has-accelerated-writes).
- [Der Quellcluster entspricht HDI 3.6 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt nicht über beschleunigte Schreibvorgänge](#the-source-cluster-is-hdi-36-without-accelerated-writes-and-the-destination-cluster-doesnt-have-accelerated-writes).
- [Der Quellcluster entspricht HDI 4.0 mit beschleunigten Schreibvorgängen, und der Zielcluster verfügt über beschleunigte Schreibvorgänge](#the-source-cluster-is-hdi-36-or-hdi-40-with-accelerated-writes-and-the-destination-cluster-has-accelerated-writes).
- [Der Quellcluster entspricht HDI 4.0 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt über beschleunigte Schreibvorgänge](#the-source-cluster-is-hdi-40-without-accelerated-writes-and-the-destination-cluster-has-accelerated-writes).
- [Der Quellcluster entspricht HDI 4.0 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt nicht über beschleunigte Schreibvorgänge](#the-source-cluster-is-hdi-40-without-accelerated-writes-and-the-destination-cluster-doesnt-have-accelerated-writes).

#### <a name="the-source-cluster-is-hdi-36-or-hdi-40-with-accelerated-writes-and-the-destination-cluster-has-accelerated-writes"></a>Der Quellcluster entspricht HDI 3.6 oder HDI 4.0 mit beschleunigten Schreibvorgängen, und der Zielcluster verfügt über beschleunigte Schreibvorgänge

Bereinigen Sie die WAL FS-Daten für den Zielcluster, und kopieren Sie das WAL-Verzeichnis aus dem Quellcluster in die HDFS-Instanz des Zielclusters. Kopieren Sie das Verzeichnis, indem Sie die folgenden Befehle auf einem beliebigen Zookeeper-Knoten oder -Workerknoten im Zielcluster ausführen:

```bash   
sudo -u hbase hdfs dfs -rm -r hdfs://mycluster/hbasewal
sudo -u hbase hdfs dfs -cp <source-container-fullpath>/hbase-wal-backup/hbasewal hdfs://mycluster/
```
#### <a name="the-source-cluster-is-hdi-36-without-accelerated-writes-and-the-destination-cluster-has-accelerated-writes"></a>Der Quellcluster entspricht HDI 3.6 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt über beschleunigte Schreibvorgänge

Bereinigen Sie die WAL FS-Daten für den Zielcluster, und kopieren Sie das WAL-Verzeichnis aus dem Quellcluster in die HDFS-Instanz des Zielclusters. Kopieren Sie das Verzeichnis, indem Sie die folgenden Befehle auf einem beliebigen Zookeeper-Knoten oder -Workerknoten im Zielcluster ausführen:

```bash
sudo -u hbase hdfs dfs -rm -r hdfs://mycluster/hbasewal
sudo -u hbase hdfs dfs -Dfs.azure.page.blob.dir="/hbase/WALs,/hbase/MasterProcWALs,/hbase/oldWALs" -cp <source-container>/hbase/*WALs hdfs://mycluster/hbasewal
```

#### <a name="the-source-cluster-is-hdi-36-without-accelerated-writes-and-the-destination-cluster-doesnt-have-accelerated-writes"></a>Der Quellcluster entspricht HDI 3.6 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt nicht über beschleunigte Schreibvorgänge

Bereinigen Sie die WAL FS-Daten für den Zielcluster, und kopieren Sie das WAL-Verzeichnis des Quellclusters in die HDFS-Instanz des Zielclusters. Führen Sie zum Kopieren des Verzeichnisses die folgenden Befehle auf einem beliebigen Zookeeper-Knoten oder -Workerknoten im Zielcluster aus:

```bash
sudo -u hbase hdfs dfs -rm -r /hbase-wals/*
sudo -u hbase hdfs dfs -Dfs.azure.page.blob.dir="/hbase/WALs,/hbase/MasterProcWALs,/hbase/oldWALs" -cp <source-container-fullpath>/hbase/*WALs /hbase-wals
```

#### <a name="the-source-cluster-is-hdi-40-without-accelerated-writes-and-the-destination-cluster-has-accelerated-writes"></a>Der Quellcluster entspricht HDI 4.0 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt über beschleunigte Schreibvorgänge

Bereinigen Sie die WAL FS-Daten für den Zielcluster, und kopieren Sie das WAL-Verzeichnis aus dem Quellcluster in die HDFS-Instanz des Zielclusters. Kopieren Sie das Verzeichnis, indem Sie die folgenden Befehle auf einem beliebigen Zookeeper-Knoten oder -Workerknoten im Zielcluster ausführen:

```bash
sudo -u hbase hdfs dfs -rm -r hdfs://mycluster/hbasewal
sudo -u hbase hdfs dfs -cp <source-container-fullpath>/hbase-wals/* hdfs://mycluster/hbasewal
   ```

#### <a name="the-source-cluster-is-hdi-40-without-accelerated-writes-and-the-destination-cluster-doesnt-have-accelerated-writes"></a>Der Quellcluster entspricht HDI 4.0 ohne beschleunigte Schreibvorgänge, und der Zielcluster verfügt nicht über beschleunigte Schreibvorgänge

Bereinigen Sie die WAL FS-Daten für den Zielcluster, und kopieren Sie das WAL-Verzeichnis des Quellclusters in die HDFS-Instanz des Zielclusters. Führen Sie zum Kopieren des Verzeichnisses die folgenden Befehle auf einem beliebigen Zookeeper-Knoten oder -Workerknoten im Zielcluster aus:

```bash
sudo -u hbase hdfs dfs -rm -r /hbase-wals/*
sudo -u hbase hdfs dfs -Dfs.azure.page.blob.dir="/hbase-wals" -cp <source-container-fullpath>/hbase-wals /
```

### <a name="complete-the-migration"></a>Fertigstellen der Migration

1. Kopieren Sie mithilfe des `sudo -u hdfs`-Benutzerkontexts den Ordner `/hdp/apps/<new-version-name>` und dessen Inhalte aus `<destination-container-fullpath>` in den Ordner `/hdp/apps` unter `<source-container-fullpath>`. Sie können den Ordner kopieren, indem Sie die folgenden Befehle im Zielcluster ausführen:
   
   ```bash   
   sudo -u hdfs hdfs dfs -cp /hdp/apps/<hdi-version> <source-container-fullpath>/hdp/apps
   ```
   
   Beispiel:
   ```bash
   sudo -u hdfs hdfs dfs -cp /hdp/apps/4.1.3.6 wasbs://hbase-upgrade-old-2021-03-22@hbaseupgrade.blob.core.windows.net/hdp/apps
   ```
   
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
