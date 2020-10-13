---
title: 'Verbinden von Excel mit Apache Hadoop mithilfe von Power Query: Azure HDInsight'
description: Erfahren Sie, wie Sie Business Intelligence-Komponenten nutzen und wie Sie mithilfe von Power Query für Excel auf Daten in Hadoop in HDInsight zugreifen können.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 12/17/2019
ms.openlocfilehash: 8664efd40bb5392f56803515f09cccc800fdf21c
ms.sourcegitcommit: 4313e0d13714559d67d51770b2b9b92e4b0cc629
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2020
ms.locfileid: "91397112"
---
# <a name="connect-excel-to-apache-hadoop-by-using-power-query"></a>Verbinden von Excel mit Apache Hadoop mithilfe von Power Query

Eine der wichtigsten Features der Big Data-Lösung von Microsoft ist die Integration von Microsoft BI-Komponenten (Business Intelligence) in Apache Hadoop-Cluster in Azure HDInsight. Ein gutes Beispiel ist die Möglichkeit, Excel mithilfe von Microsoft Power Query für Excel-Add-Ins mit dem Azure Storage-Konto zu verbinden, das die dem Hadoop-Cluster zugeordneten Daten enthält. Dieser Artikel beschreibt die Einrichtung und Verwendung von Power Query für die Abfrage von Daten aus einem mit HDInsight verwalteten Hadoop-Cluster.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Apache Hadoop-Cluster in HDInsight. Weitere Informationen finden Sie unter [Erste Schritte mit HDInsight unter Linux](./apache-hadoop-linux-tutorial-get-started.md).
* Eine Arbeitsstation mit Windows 10, Windows 7, Windows Server 2008 R2 oder einem späteren Betriebssystem
* Microsoft 365 Apps for Enterprise, Office 2016, Office 2013 Professional Plus, Excel 2013 Standalone oder Office 2010 Professional Plus.

## <a name="install-microsoft-power-query"></a>Installieren von Microsoft Power Query

Power Query kann Daten importieren, die von einem Hadoop-Auftrag, der in einem HDInsight-Cluster ausgeführt wird, ausgegeben bzw. generiert wurden.

Power Query wurde in Excel 2016 in das Menüband „Daten“ im Abschnitt zum Abrufen und Transformieren integriert. Laden Sie bei älteren Excel-Versionen Microsoft Power Query für Excel aus dem [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkID=286689) herunter, und installieren Sie es.

## <a name="import-hdinsight-data-into-excel"></a>Importieren von HDInsight-Daten in Excel

Das Power Query für Excel-Add-In erleichtert den Import von Daten aus Ihrem HDInsight-Cluster in Excel. Business Intelligence-Tools wie z. B. PowerPivot und Power Map dienen zur Inspektion, Analyse und Darstellung der Daten.

1. Starten Sie Excel.

1. Erstellen Sie eine neue leere Arbeitsmappe.

1. Führen Sie je nach Excel-Version die folgenden Schritte aus:

   * Excel 2016

     * Wählen Sie **Daten** > **Daten abrufen** > **Aus Azure** > **Aus Azure HDInsight (HDFS)** aus.

       ![HDI.PowerQuery.SelectHdiSource.2016](./media/apache-hadoop-connect-excel-power-query/powerquery-selecthdisource-excel2016.png)

   * Excel 2013/2010

     * Wählen Sie **Power Query** > **Aus Azure** > **Aus Microsoft Azure HDInsight** aus.

       ![HDI.PowerQuery.SelectHdiSource](./media/apache-hadoop-connect-excel-power-query/powerquery-selecthdisource.png)

       **Hinweis:** Falls das Menü **Power Query** nicht angezeigt wird, klicken Sie auf **Datei** > **Optionen** > **Add-Ins**, und wählen Sie unten auf der Seite im Dropdownfeld **Verwalten** die Option **COM Add-Ins** aus. Klicken Sie auf die Schaltfläche **Los...** , und vergewissern Sie sich, dass das Optionsfeld des Power Query für Excel-Add-Ins markiert ist.

       **Hinweis:** Mit Power Query können Sie auch Daten aus dem Hadoop Distributed File System (HDFS) importieren. Wählen Sie dazu **Aus anderen Quellen** aus.

1. Geben Sie im Dialogfeld **Azure HDInsight (HDFS)** im Textfeld **Kontoname oder URL** den Namen des mit dem Cluster verbundenen Azure Blob Storage-Kontos ein. Klicken Sie anschließend auf **OK**. Bei diesem Konto kann es sich um das Standardspeicherkonto oder ein verknüpftes Speicherkonto handeln.  Das Format ist `https://StorageAccountName.blob.core.windows.net/`.

1. Geben Sie unter **Kontoschlüssel** den Schlüssel für das Blob Storage-Konto ein, und wählen Sie dann **Verbinden** aus. (Sie müssen die Kontoinformationen nur beim ersten Zugriff auf diesen Speicher eingeben.)

1. Doppelklicken Sie im **Navigationsbereich** links neben dem Query-Editor auf den Namen des mit dem Cluster verbundenen Blob Storage-Containers. Der Containername ist standardmäßig derselbe Name wie der Clustername.

1. Suchen Sie **HiveSampleData.txt** in der Spalte **Name** (der Ordnerpfad lautet **../hive/warehouse/hivesampletable/** ), und wählen Sie **Binär** links neben „HiveSampleData.txt“ aus. „HiveSampleData.txt“ enthält alle Cluster. Sie können optional auch Ihre eigene Datei verwenden.

    ![HDI Power Query für Datenimport in Excel](./media/apache-hadoop-connect-excel-power-query/powerquery-importdata.png)

1. Bei Bedarf können Sie die Spaltennamen ändern. Wählen Sie **Schließen & laden** aus, wenn Sie fertig sind.  Die Daten wurden in Ihre Arbeitsmappe geladen.

    ![HDI Power Query für importierte Tabellen in Excel](./media/apache-hadoop-connect-excel-power-query/powerquery-importedtable.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie Daten aus HDInsight mithilfe von Power Query in Excel verwenden können. Auf ähnliche Weise können Sie Daten aus HDInsight für die Azure SQL-Datenbank abrufen. Außerdem können Sie Daten in HDInsight hochladen. Weitere Informationen erhalten Sie in den folgenden Artikeln:

* [Visualisieren von Apache Hive-Daten mit Microsoft Power BI in Azure HDInsight](apache-hadoop-connect-hive-power-bi.md)
* [Visualisieren von Interactive Query-Hive-Daten mit Power BI in Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)
* [Verwenden von Apache Zeppelin zum Ausführen von Apache Hive-Abfragen in Azure HDInsight](../interactive-query/hdinsight-connect-hive-zeppelin.md).
* [Verbinden von Excel mit Hadoop in Azure HDInsight mithilfe des Microsoft Hive ODBC-Treibers](apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Verbinden mit Azure HDInsight und Ausführen von Apache Hive-Abfragen mithilfe von Data Lake-Tools für Visual Studio](apache-hadoop-visual-studio-tools-get-started.md)
* [Verwenden von Azure HDInsight-Tools für Visual Studio Code](../hdinsight-for-vscode.md)
* [Upload data to HDInsight (Hochladen von Daten in HDInsight)](./../hdinsight-upload-data.md).
