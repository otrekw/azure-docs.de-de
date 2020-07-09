---
title: Migrieren von Workloads von Apache Spark 2.1 oder 2.2 zu 2.3 oder 2.4 – Azure HDInsight
description: Erfahren Sie, wie Sie Apache Spark 2.1 und 2.2 zu 2.3 oder 2.4 migrieren können.
author: ashishthaps1
ms.author: ashishth
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/20/2020
ms.openlocfilehash: 1452a22303af169e9501c85336785f7627ac7e88
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83727418"
---
# <a name="migrate-apache-spark-21-and-22-workloads-to-23-and-24"></a>Migrieren von Workloads von Apache Spark 2.1 oder 2.2 zu 2.3 oder 2.4

In diesem Dokument wird erläutert, wie Apache Spark-Workloads unter Spark 2.1 und 2.2 zu Spark 2.3 oder 2.4 migriert werden können.

Wie in den [Versionshinweisen](../hdinsight-release-notes.md#upcoming-changes) erörtert, werden ab dem 1. Juli 2020 die folgenden Clusterkonfigurationen nicht unterstützt und Kunden können mit diesen Konfigurationen keine neuen Cluster erstellen:
 - Spark 2.1 und 2.2 in einem HDInsight 3.6 Spark-Cluster
 - Spark 2.3 in einem HDInsight 4.0 Spark-Cluster

Bestehende Cluster in diesen Konfigurationen werden ohne Support von Microsoft in der vorliegenden Form ausgeführt. Wenn Sie mit Spark 2.1 oder 2.2 unter HDInsight 3.6 arbeiten, wechseln Sie bis zum 30. Juni 2020 zu Spark 2.3 unter HDInsight 3.6, um eine mögliche Unterbrechung des Systems/Supports zu vermeiden. Wenn Sie mit Spark 2.3 auf einem HDInsight 4.0-Cluster arbeiten, wechseln Sie bis zum 30. Juni 2020 zu Spark 2.4 unter HDInsight 4.0, um eine mögliche Unterbrechung des Systems/Supports zu vermeiden.

Allgemeine Informationen zum Migrieren eines HDInsight-Cluster von 3.6 zu 4.0 finden Sie unter [Migrieren eines HDInsight-Clusters zu einer neueren Version](../hdinsight-upgrade-cluster.md). Allgemeine Informationen zum Migrieren zu einer neueren Version von Apache Spark finden Sie unter [Apache Spark: Versionsrichtlinie](https://spark.apache.org/versioning-policy.html).

## <a name="guidance-on-spark-version-upgrades-on-hdinsight"></a>Leitfaden zu Spark-Versionsupgrades unter HDInsight

| Upgradeszenario | Mechanismus | Zu beachtende Aspekte | Spark Hive-Integration |
|------------------|-----------|--------------------|------------------------|
|HDInsight 3.6 Spark 2.1 zu HDInsight 3.6 Spark 2.3| Erneutes Erstellen von Clustern mit HDInsight Spark 2.3 | Lesen Sie die folgenden Artikel: <br> [Apache Spark: Upgrade von Spark SQL 2.2 auf 2.3](https://spark.apache.org/docs/latest/sql-migration-guide-upgrade.html#upgrading-from-spark-sql-22-to-23) <br><br> [Apache Spark: Upgrade von Spark SQL 2.1 auf 2.2](https://spark.apache.org/docs/latest/sql-migration-guide-upgrade.html#upgrading-from-spark-sql-21-to-22) | Keine Änderung |
|HDInsight 3.6 Spark 2.2 zu HDInsight 3.6 Spark 2.3 | Erneutes Erstellen von Clustern mit HDInsight Spark 2.3 | Lesen Sie die folgenden Artikel: <br> [Apache Spark: Upgrade von Spark SQL 2.2 auf 2.3](https://spark.apache.org/docs/latest/sql-migration-guide-upgrade.html#upgrading-from-spark-sql-22-to-23) | Keine Änderung |
| HDInsight 3.6 Spark 2.1 zu HDInsight 4.0 Spark 2.4 | Erneutes Erstellen von Clustern mit HDInsight 4.0 Spark 2.4 | Lesen Sie die folgenden Artikel: <br> [Apache Spark: Upgrade von Spark SQL 2.3 auf 2.4](https://spark.apache.org/docs/latest/sql-migration-guide-upgrade.html#upgrading-from-spark-sql-23-to-24) <br><br> [Apache Spark: Upgrade von Spark SQL 2.2 auf 2.3](https://spark.apache.org/docs/latest/sql-migration-guide-upgrade.html#upgrading-from-spark-sql-22-to-23) <br><br> [Apache Spark: Upgrade von Spark SQL 2.1 auf 2.2](https://spark.apache.org/docs/latest/sql-migration-guide-upgrade.html#upgrading-from-spark-sql-21-to-22) | Die Spark Hive-Integration wurde in HDInsight 4.0 geändert. <br><br> In HDInsight 4.0 verwenden Spark und Hive unabhängige Kataloge für den Zugriff auf SparkSQL- oder Hive-Tabellen. Eine von Spark erstellte Tabelle befindet sich im Spark-Katalog. Eine von Hive erstellte Tabelle befindet sich im Hive-Katalog. Dies unterscheidet sich von HDInsight 3.6, wo in Hive und Spark der gleiche Katalog verwendet wurde. Die Integration von Hive und Spark in HDInsight 4.0 basiert auf Hive Warehouse Connector (HWC). HWC funktioniert als Brücke zwischen Spark und Hive. Informationen zu Hive Warehouse Connector <br> Wenn Sie in HDInsight 4.0 den Metastore für Hive und Spark freigeben möchten, können Sie in Ihrem Spark-Cluster den Wert für die Eigenschaft „metastore.catalog.default“ in „hive“ ändern. Diese Eigenschaft finden Sie in Ambari unter „Advanced spark2-hive-site-override“. Es ist wichtig zu verstehen, dass die Freigabe des Metastores nur für externe Hive-Tabellen funktioniert. Sie funktioniert nicht bei internen/verwalteten Hive-Tabellen oder ACID-Tabellen. <br><br>Weitere Informationen finden Sie unter [Migrieren von Azure HDInsight 3.6-Hive-Workloads zu HDInsight 4.0](../interactive-query/apache-hive-migrate-workloads.md).<br><br> |
| HDInsight 3.6 Spark 2.2 zu HDInsight 4.0 Spark 2.4 | Erneutes Erstellen von Clustern mit HDInsight 4.0 Spark 2.4 | Lesen Sie die folgenden Artikel: <br> [Apache Spark: Upgrade von Spark SQL 2.3 auf 2.4](https://spark.apache.org/docs/latest/sql-migration-guide-upgrade.html#upgrading-from-spark-sql-23-to-24) <br><br> [Apache Spark: Upgrade von Spark SQL 2.2 auf 2.3](https://spark.apache.org/docs/latest/sql-migration-guide-upgrade.html#upgrading-from-spark-sql-22-to-23) | Die Spark Hive-Integration wurde in HDInsight 4.0 geändert. <br><br> In HDInsight 4.0 verwenden Spark und Hive unabhängige Kataloge für den Zugriff auf SparkSQL- oder Hive-Tabellen. Eine von Spark erstellte Tabelle befindet sich im Spark-Katalog. Eine von Hive erstellte Tabelle befindet sich im Hive-Katalog. Dies unterscheidet sich von HDInsight 3.6, wo in Hive und Spark der gleiche Katalog verwendet wurde. Die Integration von Hive und Spark in HDInsight 4.0 basiert auf Hive Warehouse Connector (HWC). HWC funktioniert als Brücke zwischen Spark und Hive. Informationen zu Hive Warehouse Connector <br> Wenn Sie in HDInsight 4.0 den Metastore für Hive und Spark freigeben möchten, können Sie in Ihrem Spark-Cluster den Wert für die Eigenschaft „metastore.catalog.default“ in „hive“ ändern. Diese Eigenschaft finden Sie in Ambari unter „Advanced spark2-hive-site-override“. Es ist wichtig zu verstehen, dass die Freigabe des Metastores nur für externe Hive-Tabellen funktioniert. Sie funktioniert nicht bei internen/verwalteten Hive-Tabellen oder ACID-Tabellen. <br><br>Weitere Informationen finden Sie unter [Migrieren von Azure HDInsight 3.6-Hive-Workloads zu HDInsight 4.0](../interactive-query/apache-hive-migrate-workloads.md).|

## <a name="next-steps"></a>Nächste Schritte

* [Migrieren eines HDInsight-Clusters zu einer neueren Version](../hdinsight-upgrade-cluster.md)
* [Migrieren von Azure HDInsight 3.6-Hive-Workloads zu HDInsight 4.0](../interactive-query/apache-hive-migrate-workloads.md)
