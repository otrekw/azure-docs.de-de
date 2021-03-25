---
title: Optimieren von Clustern mit Apache Ambari in Azure HDInsight
description: Verwenden Sie die Apache Ambari-Webbenutzeroberfläche, um Azure HDInsight-Cluster zu konfigurieren und zu optimieren.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 05/04/2020
ms.openlocfilehash: 2146ccb0c4d7f263c3e1a69db9b172649fcd25ea
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104863495"
---
# <a name="optimize-clusters-with-apache-ambari-in-azure-hdinsight"></a>Optimieren von Clustern mit Apache Ambari in Azure HDInsight

HDInsight stellt Apache Hadoop-Cluster für größere Datenverarbeitungsanwendungen bereit. Das Verwalten, Überwachen und Optimieren dieser komplexen Cluster mit mehreren Knoten kann eine Herausforderung darstellen. Apache Ambari ist eine Webbenutzeroberfläche zum Verwalten und Überwachen von HDInsight-Linux-Clustern.

Eine Einführung in die Nutzung der Ambari-Webbenutzeroberfläche finden Sie unter [Verwalten von HDInsight-Clustern mithilfe der Apache Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md).

Melden Sie sich unter `https://CLUSTERNAME.azurehdidnsight.net` mit Ihren Clusteranmeldeinformationen an Ambari an. Auf dem ersten Bildschirm wird ein Dashboard mit einer Übersicht angezeigt.

:::image type="content" source="./media/hdinsight-changing-configs-via-ambari/apache-ambari-dashboard.png" alt-text="Apache Ambari – angezeigtes Benutzerdashboard":::

Die Ambari-Webbenutzeroberfläche wird verwendet, um Hosts, Dienste, Warnungen, Konfigurationen und Ansichten zu verwalten. Ambari kann nicht zum Erstellen eines HDInsight-Clusters oder zum Aktualisieren von Diensten genutzt werden. Es kann auch nicht verwendet werden, um Stapel und Versionen zu verwalten, die In- bzw. Außerbetriebnahme von Hosts durchzuführen oder dem Cluster Dienste hinzuzufügen.

## <a name="manage-your-clusters-configuration"></a>Verwalten der Konfiguration Ihres Clusters

Konfigurationseinstellungen können für die Optimierung eines bestimmten Diensts verwendet werden. Wählen Sie zum Ändern der Konfigurationseinstellungen eines Diensts den Dienst auf der Randleiste **Dienste** (auf der linken Seite) aus. Navigieren Sie dann auf der Seite mit den Dienstdetails zur Registerkarte **Configs** (Konfigurationen).

:::image type="content" source="./media/hdinsight-changing-configs-via-ambari/ambari-services-sidebar.png" alt-text="Apache Ambari – Randleiste „Dienste“":::

## <a name="modify-namenode-java-heap-size"></a>Ändern der NameNode-Java-Heapgröße

Die NameNode-Java-Heapgröße ist von vielen Faktoren abhängig, z. B. der Auslastung des Clusters, der Anzahl von Dateien und der Anzahl von Blöcken. Die Standardgröße 1 GB funktioniert für die meisten Cluster gut, aber für einige Workloads kann mehr oder weniger Arbeitsspeicher erforderlich sein.

Gehen Sie wie folgt vor, um die NameNode-Java-Heapgröße zu ändern:

1. Wählen Sie in der Randleiste „Dienste“ die Option **HDFS**, und navigieren Sie zur Registerkarte **Configs** (Konfigurationen).

    :::image type="content" source="./media/hdinsight-changing-configs-via-ambari/ambari-apache-hdfs-config.png" alt-text="Apache Ambari – HDFS-Konfiguration":::

1. Suchen Sie nach der Einstellung **NameNode Java heap size** (NameNode-Java-Heapgröße). Sie können auch das Textfeld **Filter** verwenden, um nach einer bestimmten Einstellung zu suchen. Wählen Sie das Symbol **Stift** neben dem Namen der Einstellung aus.

    :::image type="content" source="./media/hdinsight-changing-configs-via-ambari/ambari-java-heap-size.png" alt-text="Apache Ambari – NameNode-Java-Heapgröße":::

1. Geben Sie den neuen Wert in das Textfeld ein, und drücken Sie anschließend die **EINGABETASTE**, um die Änderung zu speichern.

    :::image type="content" source="./media/hdinsight-changing-configs-via-ambari/java-heap-size-edit1.png" alt-text="Ambari – Bearbeiten der NameNode-Java-Heapgröße 1":::

1. Die NameNode-Java-Heapgröße wird von 2 GB in 1 GB geändert.

    :::image type="content" source="./media/hdinsight-changing-configs-via-ambari/java-heap-size-edited.png" alt-text="Bearbeitete NameNode-Java-Heapgröße 2":::

1. Speichern Sie Ihre Änderungen, indem Sie oben auf dem Konfigurationsbildschirm auf die grüne Schaltfläche **Speichern** klicken.

    :::image type="content" source="./media/hdinsight-changing-configs-via-ambari/ambari-save-changes1.png" alt-text="Apache Ambari – Speichern der Konfiguration":::

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von HDInsight-Clustern mithilfe der Apache Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md)
* [Apache Ambari-REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)
* [Optimieren von Apache HBase](./optimize-hbase-ambari.md)
* [Optimieren von Apache Hive](./optimize-hive-ambari.md)
* [Optimieren von Apache Pig](./optimize-pig-ambari.md)
