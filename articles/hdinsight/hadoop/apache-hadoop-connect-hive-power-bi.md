---
title: Visualisieren von Apache Hive-Daten mit Power BI – Azure HDInsight
description: Erfahren Sie, wie Sie Microsoft Power BI verwenden, um von Azure HDInsight verarbeitete Hive-Daten zu visualisieren.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/24/2020
ms.openlocfilehash: bb448a4befb15618485b2b5951222761180a1f22
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104866742"
---
# <a name="visualize-apache-hive-data-with-microsoft-power-bi-using-odbc-in-azure-hdinsight"></a>Visualisieren von Apache Hive-Daten mit Microsoft Power BI mithilfe von ODBC in Azure HDInsight

Hier erfahren Sie, wie Sie Microsoft Power BI Desktop mithilfe von ODBC mit Azure HDInsight verbinden und Apache Hive-Daten visualisieren.

> [!IMPORTANT]
> Sie können den Hive-ODBC-Treiber verwenden, um Importe über den generischen ODBC-Connector in Power BI Desktop durchzuführen. Aufgrund der fehlenden Interaktivität der Hive-Abfrage-Engine wird die Verwendung für BI-Workloads jedoch nicht empfohlen. Aus Leistungsgründen sind der [interaktive HDInsight-Abfrageconnector](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md) und der [HDInsight Spark-Connector](/power-bi/spark-on-hdinsight-with-direct-connect) eine bessere Wahl.

In diesem Artikel laden Sie die Daten aus der Hive-Tabelle `hivesampletable` in Power BI. Die Hive-Tabelle enthält einige Mobiltelefon-Nutzungsdaten. Anschließend zeichnen Sie die Nutzungsdaten auf eine Weltkarte:

:::image type="content" source="./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png" alt-text="HDInsight Power BI Kartenbericht" border="true":::

Die Informationen gelten auch für den neuen Clustertyp [Interactive Query](../interactive-query/apache-interactive-query-get-started.md). Weitere Informationen zum Herstellen einer Verbindung mit HDInsight Interactive Query mithilfe von direkten Abfragen finden Sie unter [Visualisieren von Interactive Query-Hive-Daten mit Power BI mithilfe direkter Abfragen in Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie diesen Artikel durchgehen können, benötigen Sie Folgendes:

* HDInsight-Cluster. Bei dem Cluster kann es sich entweder um einen HDInsight-Cluster mit Hive oder um einen neu veröffentlichten Interactive Query-Cluster handeln. Informationen zum Erstellen von Clustern finden Sie unter [Cluster erstellen](apache-hadoop-linux-tutorial-get-started.md).

* [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/). Sie können eine Kopie aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331) herunterladen.

## <a name="create-hive-odbc-data-source"></a>Erstellen einer Hive ODBC-Datenquelle

Informationen finden Sie unter [Erstellen einer Hive ODBC-Datenquelle](apache-hadoop-connect-excel-hive-odbc-driver.md#create-apache-hive-odbc-data-source).

## <a name="load-data-from-hdinsight"></a>Laden von Daten aus HDInsight

Die Hive-Tabelle **hivesampletable** ist in allen HDInsight-Clustern enthalten.

1. Starten Sie Power BI Desktop.

1. Navigieren Sie im oberen Menü zu **Start** > **Daten abrufen** > **Mehr...** .

    :::image type="content" source="./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-open-odbc.png" alt-text="HDInsight Excel Power BI – Daten öffnen" border="true":::

1. Wählen Sie im Dialogfeld **Daten abrufen** links die Option **Andere** und rechts die Option **ODBC** aus, und wählen Sie anschließend unten **Verbinden** aus.

1. Wählen Sie in der Dropdownliste des Dialogfelds **Aus ODBC** den im letzten Abschnitt erstellten Datenquellennamen aus. Klicken Sie dann auf **OK**.

1. Bei der ersten Verwendung wird das Dialogfeld **ODBC-Treiber** geöffnet. Wählen Sie im linken Menü die Option **Standard oder Benutzerdefiniert** aus. Wählen Sie anschließend **Verbinden** aus, um den **Navigator** zu öffnen.

1. Erweitern Sie im Dialogfeld **Navigator** die Optionen **ODBC > HIVE > Standard**, wählen Sie **hivesampletable** aus, und wählen Sie anschließend **Laden** aus.

## <a name="visualize-data"></a>Visualisieren von Daten

Fahren Sie ab der vorherigen Prozedur fort.

1. Wählen Sie im Bereich „Visualisierungen“ die Option **Karte** (Globussymbol) aus.

    :::image type="content" source="./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png" alt-text="HDInsight Power BI passt Bericht an" border="true":::

1. Wählen Sie im Bereich **Felder** die Optionen **country** und **devicemake** aus. Ihnen werden die auf die Karte gezeichneten Daten angezeigt.

1. Erweitern Sie die Karte.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie Daten aus HDInsight mithilfe von Power BI visualisieren.  Weitere Informationen finden Sie in den folgenden Artikeln:

* [Verbinden von Excel mit Hadoop in Azure HDInsight mithilfe des Microsoft Hive ODBC-Treibers](./apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Verbinden von Excel mit Apache Hadoop mithilfe von Power Query](apache-hadoop-connect-excel-power-query.md)
* [Visualisieren von Interactive Query Apache Hive-Daten mit Microsoft Power BI mithilfe direkter Abfragen](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)