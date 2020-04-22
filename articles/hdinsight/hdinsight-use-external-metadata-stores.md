---
title: Verwenden von externen Metadatenspeichern – Azure HDInsight
description: Verwenden von externen Metadatenspeichern für Azure HDInsight-Cluster
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/03/2020
ms.openlocfilehash: e53164d1e25f8a8d0a14d21c0544d95cf912fe9f
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "81313963"
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Verwenden von externen Metadatenspeichern in Azure HDInsight

HDInsight ermöglicht Ihnen das Steuern Ihrer Daten und Metadaten mit externen Datenspeichern. Dieses Feature ist für [Apache Hive-Metastore](#custom-metastore), [Apache Oozie-Metastore](#apache-oozie-metastore) und [Apache Ambari-Datenbank](#custom-ambari-db) verfügbar.

Der Apache Hive-Metastore in HDInsight ist ein wesentlicher Bestandteil der Apache Hadoop-Architektur. Ein Metastore ist das zentrale Schemarepository. Der Metastore wird von anderen Tools für den Zugriff auf Big Data wie Apache Spark, Interactive Query (LLAP), Presto oder Apache Pig verwendet. HDInsight verwendet eine Azure SQL-Datenbank als Hive-Metastore.

![Architektur des Hive-Metadatenspeichers in HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

Sie können einen Metastore für Ihre HDInsight-Cluster auf zwei Arten einrichten:

* [Standardmetastore](#default-metastore)
* [Benutzerdefinierter Metastore](#custom-metastore)

## <a name="default-metastore"></a>Standardmetastore

HDInsight erstellt standardmäßig einen Metastore für jeden Clustertyp. Sie können stattdessen einen benutzerdefinierten Metastore angeben. Beim Standardmetastore sind folgende Aspekte zu berücksichtigen:

* Keine zusätzlichen Kosten. HDInsight erstellt für jeden Clustertyp einen Metastore ohne zusätzliche Kosten.

* Jeder Standardmetastore ist Teil des Clusterlebenszyklus. Wenn Sie einen Cluster löschen, werden der entsprechende Metastore und die jeweiligen Metadaten ebenfalls gelöscht.

* Der Standardmetastore kann nicht für andere Cluster freigegeben werden.

* Der Standardmetastore verwendet die Azure SQL-Basisdatenbank, die auf fünf DTUs (Datenbankübertragungseinheiten) begrenzt ist.
Dieser Standardmetastore wird in der Regel für relativ einfache Workloads verwendet. Workloads, für die weder mehrere Cluster noch eine Beibehaltung von Metadaten über den Lebenszyklus des Clusters hinaus erforderlich sind.

## <a name="custom-metastore"></a>Benutzerdefinierter Metastore

HDInsight unterstützt auch benutzerdefinierte Metastores, der für Produktionscluster empfohlen werden:

* Sie geben Ihre eigene Azure SQL-Datenbank als Metastore an.

* Der Lebenszyklus des Metastore ist nicht an den Lebenszyklus eines Clusters gebunden, sodass Sie Cluster ohne Verlust von Metadaten erstellen und löschen können. Metadaten, wie z. B. Ihre Hive-Schemas, bleiben auch nach dem Löschen und erneuten Erstellen des HDInsight-Clusters erhalten.

* Ein benutzerdefinierter Metastore ermöglicht das Anfügen mehrerer Cluster und Clustertypen an diesen Metastore. Beispielsweise kann ein einzelner Metastore für Interactive Query-, Hive- und Spark-Cluster in HDInsight freigegeben werden.

* Die Kosten für einen Metastore (Azure SQL-Datenbank) richten sich nach der von Ihnen ausgewählten Leistungsstufe.

* Sie können den Metastore nach Bedarf hochskalieren.

* Cluster und externer Metastore müssen in der gleichen Region gehostet werden.

![Anwendungsfall für den Hive-Metadatenspeicher in HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)

### <a name="create-and-config-azure-sql-database-for-the-custom-metastore"></a>Erstellen und Konfigurieren der Azure SQL-Datenbank für den benutzerdefinierten Metastore

Vor dem Einrichten eines benutzerdefinierten Hive-Metastores für einen HDInsight-Cluster müssen Sie eine Azure SQL-Datenbank erstellen, oder es muss bereits eine Azure SQL-Datenbank vorhanden sein.  Weitere Informationen finden Sie unter [Quickstart: Erstellen einer Einzeldatenbank in Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-get-started?tabs=azure-portal).

Beim Erstellen des Clusters muss der HDInsight-Dienst eine Verbindung mit dem externen metastore herstellen und Ihre Anmeldeinformationen überprüfen. Konfigurieren Sie die Regeln der Azure SQL-Datenbank-Firewall, um Azure-Diensten und -Ressourcen den Zugriff auf den Server zu ermöglichen. Aktivieren Sie diese Option im Azure-Portal, indem Sie **Serverfirewall festlegen** auswählen. Wählen Sie dann unterhalb von **Zugriff auf öffentliches Netzwerk verweigern** **Nein** und unterhalb von **Azure-Diensten Zugriff auf den Server erlauben** für den Azure SQL-Datenbankserver bzw. die Azure SQL-Datenbank **Ja** aus. Weitere Informationen finden Sie unter [IP-Firewallregeln für Azure SQL-Datenbank und Azure SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure#use-the-azure-portal-to-manage-server-level-ip-firewall-rules).

![Schaltfläche zum Festlegen der Serverfirewall](./media/hdinsight-use-external-metadata-stores/configure-azure-sql-database-firewall1.png)

![Zugriff auf Azure-Dienste zulassen](./media/hdinsight-use-external-metadata-stores/configure-azure-sql-database-firewall2.png)

### <a name="select-a-custom-metastore-during-cluster-creation"></a>Auswählen eines benutzerdefinierten Metastore während der Clustererstellung

Sie können jederzeit einen Verweis des Clusters auf eine zuvor erstellte Azure SQL-Datenbank festlegen. Bei der Clustererstellung über das Portal wird die Option über **Speicher > Metastore-Einstellungen** angegeben.

![Azure-Portal für Hive-Metadatenspeicher in HDInsight](./media/hdinsight-use-external-metadata-stores/azure-portal-cluster-storage-metastore.png)

## <a name="hive-metastore-guidelines"></a>Richtlinien für Hive-Metastore

* Verwenden Sie wann immer möglich einen benutzerdefinierten Metastore, um das Trennen von Computeressourcen (Ihre ausgeführten Cluster) und Metadaten (im Metastore gespeichert) zu erleichtern.

* Beginnen Sie mit einem S2-Tarif, der 50 DTUs und 250 GB Speicher bietet. Wenn Sie einen Engpass feststellen, können Sie die Datenbank zentral hochskalieren.

* Wenn Sie mehreren HDInsight-Clustern Zugriff auf separate Daten gewähren möchten, verwenden Sie für den Metastore auf jedem Cluster eine eigene Datenbank. Wenn Sie einen Metastore für mehrere HDInsight-Cluster freigeben, bedeutet dies, dass die Cluster dieselben Metadaten und zugrunde liegenden Benutzerdatendateien verwenden.

* Sichern Sie Ihren benutzerdefinierten Metastore regelmäßig. Die Azure SQL-Datenbank generiert Sicherungen automatisch, der Aufbewahrungszeitraum der Sicherungen variiert jedoch. Weitere Informationen finden Sie unter [Informationen zu automatischen Sicherungen von SQL-Datenbank](../sql-database/sql-database-automated-backups.md).

* Platzieren Sie den Metastore und den HDInsight-Cluster in derselben Region. Diese Konfiguration bietet die höchste Leistung und die niedrigsten Gebühren für ausgehenden Netzwerkdatenverkehr.

* Überwachen Sie den Metastore in Hinblick auf Leistung und Verfügbarkeit. Verwenden Sie dazu Überwachungstools für die Azure SQL-Datenbank, z. B. Azure Monitor-Protokolle.

* Wenn eine neue, höhere Version von Azure HDInsight anhand einer vorhandenen benutzerdefinierten Metastoredatenbank erstellt wird, aktualisiert das System das Metastoreschema. Das Upgrade kann nicht rückgängig gemacht werden, ohne die Datenbank aus einer Sicherung wiederherzustellen.

* Falls Sie einen Metastore für mehrere Cluster freigeben, achten Sie darauf, dass alle Cluster die gleiche HDInsight-Version haben. Verschiedene Hive-Versionen verwenden unterschiedliche Schemas der Metastoredatenbank. So können Sie beispielsweise einen Metastore nicht für Hive 2.1- und Hive 3.1-Cluster freigeben.

* In HDInsight 4.0 verwenden Spark und Hive unabhängige Kataloge für den Zugriff auf SparkSQL- oder Hive-Tabellen. Eine von Spark erstellte Tabelle befindet sich im Spark-Katalog. Eine von Hive erstellte Tabelle befindet sich im Hive-Katalog. Dies unterscheidet sich von HDInsight 3.6, wo in Hive und Spark der gleiche Katalog verwendet wurde. Die Integration von Hive und Spark in HDInsight 4.0 basiert auf Hive Warehouse Connector (HWC). HWC funktioniert als Brücke zwischen Spark und Hive. [Informationen zu Hive Warehouse Connector](../hdinsight/interactive-query/apache-hive-warehouse-connector.md).

## <a name="apache-oozie-metastore"></a>Apache Oozie-Metastore

Apache Oozie ist ein Koordinationssystem für Workflows zur Verwaltung von Hadoop-Aufträgen. Oozie unterstützt Hadoop-Aufträge für Apache MapReduce, Pig, Hive und andere.  Oozie verwendet einen Metastore zum Speichern von Details zu Workflows. Zur Leistungssteigerung bei Verwendung von Oozie können Sie eine Azure SQL-Datenbank als benutzerdefinierten Metastore verwenden. Der Metastore bietet Zugriff auf Oozie-Auftragsdaten, nachdem Sie den Cluster gelöscht haben.

Anleitungen zum Erstellen eines Oozie-Metastore mit einer Azure SQL-Datenbank finden Sie unter [Verwenden von Apache Oozie für Workflows](hdinsight-use-oozie-linux-mac.md).

## <a name="custom-ambari-db"></a>Benutzerdefinierte Ambari-Datenbank

Informationen zum Verwenden Ihrer eigenen externen Datenbank mit Apache Ambari in HDInsight finden Sie unter [Benutzerdefinierte Apache Ambari-Datenbank](hdinsight-custom-ambari-db.md).

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten von Clustern in HDInsight mit Apache Hadoop, Apache Spark, Apache Kafka usw.](./hdinsight-hadoop-provision-linux-clusters.md)
