---
title: 'Tutorial: Laden von Daten und Ausführen von Abfragen mit Apache Spark: Azure HDInsight'
description: 'Tutorial: Informationen zum Laden von Daten und Anwenden interaktiver Abfragen auf Spark-Cluster in Azure HDInsight.'
ms.service: hdinsight
ms.topic: tutorial
ms.custom: hdinsightactive,mvc
ms.date: 02/12/2020
ms.openlocfilehash: de17bf02392f0bb05820fabba3f9057e067391cf
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104865909"
---
# <a name="tutorial-load-data-and-run-queries-on-an-apache-spark-cluster-in-azure-hdinsight"></a>Tutorial: Laden von Daten und Ausführen von Abfragen auf einem Apache Spark-Cluster in Azure HDInsight

In diesem Tutorial erfahren Sie, wie Sie auf der Grundlage einer CSV-Datei einen Dataframe erstellen und interaktive Spark SQL-Abfragen für einen [Apache Spark](https://spark.apache.org/)-Cluster in Azure HDInsight ausführen. In Spark ist ein Dataframe eine verteilte Sammlung von Daten, die in benannten Spalten organisiert sind. Dataframe entspricht vom Konzept her einer Tabelle in einer relationalen Datenbank oder einem Datenrahmen in R/Python.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines Dataframes aus einer CSV-Datei
> * Ausführen von Abfragen gegen Dataframes

## <a name="prerequisites"></a>Voraussetzungen

Ein Apache Spark-Cluster unter HDInsight. Weitere Informationen finden Sie unter [Erstellen eines Apache Spark-Clusters](./apache-spark-jupyter-spark-sql-use-portal.md).

## <a name="create-a-jupyter-notebook"></a>Erstellen eines Jupyter Notebooks

Jupyter Notebook ist eine interaktive Notebook-Umgebung, die verschiedene Programmiersprachen unterstützt. Das Notebook ermöglicht Ihnen, mit Ihren Daten zu interagieren, Code mit Markdowntext zu kombinieren und einfache Visualisierungen durchzuführen.

1. Bearbeiten Sie die URL `https://SPARKCLUSTER.azurehdinsight.net/jupyter`, indem Sie `SPARKCLUSTER` durch den Namen Ihres Spark-Clusters ersetzen. Geben Sie dann die bearbeitete URL in einem Webbrowser ein. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden.

2. Wählen Sie auf der Jupyter-Webseite **Neu** > **PySpark** aus, um ein Notebook zu erstellen.

   :::image type="content" source="./media/apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png " alt-text="Erstellen einer Jupyter Notebook-Instanz zum Ausführen einer interaktiven Spark SQL-Abfrage" border="true":::

   Es wird ein neues Notebook erstellt und geöffnet, das den Namen „Untitled(`Untitled.ipynb`)“ hat.

    > [!NOTE]  
    > Wenn Sie den PySpark-Kernel zum Erstellen eines Notebooks verwenden, wird die `spark`-Sitzung automatisch für Sie erstellt, wenn Sie die erste Codezelle ausführen. Die Sitzung muss nicht explizit erstellt werden.

## <a name="create-a-dataframe-from-a-csv-file"></a>Erstellen eines Dataframes aus einer CSV-Datei

Anwendungen können Dataframes direkt auf der Grundlage von Dateien oder Ordnern im Remotespeicher (etwa in Azure Storage oder Azure Data Lake Storage), auf der Grundlage einer Hive-Tabelle oder auf der Grundlage anderer von Spark unterstützter Datenquellen (beispielsweise Cosmos DB, Azure SQL-Datenbank, DW usw.) erstellen. Der folgende Screenshot zeigt eine Momentaufnahme der in diesem Tutorial verwendeten HVAC.csv-Datei. Die CSV-Datei enthält alle HDInsight Spark-Cluster. Die Daten erfassen die Temperaturunterschiede in einigen Gebäuden.

:::image type="content" source="./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png " alt-text="Momentaufnahme der Daten für die interaktive Spark SQL-Abfrage" border="true":::

1. Fügen Sie den folgenden Code in eine leere Zelle des Jupyter Notebooks ein, und drücken Sie **UMSCHALT+EINGABE**, um den Code auszuführen. Mit dem Code werden die Typen importiert, die für dieses Szenario benötigt werden:

    ```python
    from pyspark.sql import *
    from pyspark.sql.types import *
    ```

    Beim Ausführen einer interaktiven Abfrage in Jupyter wird in der Titelleiste Ihres Webbrowserfensters oder Ihrer Registerkarte neben dem Notebooktitel der Status **(Beschäftigt)** angezeigt. Außerdem sehen Sie in der rechten oberen Ecke einen ausgefüllten Kreis neben dem Text **PySpark**. Wenn der Auftrag abgeschlossen ist, wird ein Kreis ohne Füllung angezeigt.

    :::image type="content" source="./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png " alt-text="Status einer interaktiven Spark SQL-Abfrage" border="true":::

1. Beachten Sie die zurückgegebene Sitzungs-ID. In der obigen Abbildung lautet die Sitzungs-ID 0. Sie können die Sitzungsdetails bei Bedarf auch abrufen, indem Sie zu `https://CLUSTERNAME.azurehdinsight.net/livy/sessions/ID/statements` navigieren. Dabei steht „CLUSTERNAME“ für den Namen Ihres Spark-Clusters und „ID“ für Ihre Sitzungs-ID.

1. Führen Sie den folgenden Code aus, um einen Dataframe und eine temporäre Tabelle (**hvac**) zu erstellen.

    ```python
    # Create a dataframe and table from sample data
    csvFile = spark.read.csv('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv', header=True, inferSchema=True)
    csvFile.write.saveAsTable("hvac")
    ```

## <a name="run-queries-on-the-dataframe"></a>Ausführen von Abfragen gegen Dataframes

Nach dem Erstellen der Tabelle führen Sie eine interaktive Abfrage für die Daten aus.

1. Führen Sie den folgenden Code in einer leeren Zelle des Notebooks aus:

    ```sql
    %%sql
    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```

   Die folgende Ausgabe in Tabellenform wird angezeigt.

     :::image type="content" source="./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png " alt-text="Tabellenausgabe des Ergebnisses der interaktiven Spark-Abfrage" border="true":::

2. Sie können die Ergebnisse auch in anderen Visualisierungen anzeigen. Wählen Sie zum Anzeigen eines Bereichsdiagramms für die gleiche Ausgabe **Bereich** aus, und legen Sie die anderen Werte wie gezeigt fest.

    :::image type="content" source="./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png " alt-text="Bereichsdiagramm des Ergebnisses der interaktiven Spark-Abfrage" border="true":::

3. Navigieren Sie auf der Menüleiste des Notebooks zu **File** (Datei) > **Save and Checkpoint** (Speichern und Prüfpunkt).

4. Wenn Sie jetzt mit dem [nächsten Tutorial](apache-spark-use-bi-tools.md) fortfahren, lassen Sie das Notebook geöffnet. Fahren Sie andernfalls das Notebook herunter, um die Clusterressourcen freizugeben. Navigieren Sie dazu auf der Menüleiste des Notebooks zu **File** (Datei) >  **Close and Halt** (Schließen und anhalten) aus.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Da Ihre Daten und Jupyter Notebooks bei Verwendung von HDInsight in Azure Storage oder Azure Data Lake Storage gespeichert werden, können Sie einen Cluster problemlos löschen, wenn er nicht verwendet wird. Für einen HDInsight-Cluster fallen auch dann Gebühren an, wenn er nicht verwendet wird. Da die Gebühren für den Cluster erheblich höher sind als die Kosten für den Speicher, ist es sinnvoll, nicht verwendete Cluster zu löschen. Wenn Sie vorhaben, sofort mit dem nächsten Tutorial fortzufahren, können Sie den Cluster beibehalten.

Öffnen Sie den Cluster im Azure-Portal, und wählen Sie **Löschen** aus.

:::image type="content" source="./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png " alt-text="Löschen eines HDInsight-Clusters" border="true":::

Sie können auch den Namen der Ressourcengruppe auswählen, um die Seite für die Ressourcengruppe zu öffnen, und dann **Ressourcengruppe löschen** auswählen. Indem Sie die Ressourcengruppe löschen, löschen Sie sowohl den HDInsight Spark-Cluster als auch das Standardspeicherkonto.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie auf der Grundlage einer CSV-Datei einen Datenrahmen erstellen und interaktive Spark SQL-Abfragen für einen Apache Spark-Cluster in Azure HDInsight ausführen. Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie die in Apache Spark registrierten Daten in ein BI-Analyse-Tool wie Power BI gezogen werden können.

> [!div class="nextstepaction"]
> [Analysieren von Daten mithilfe von BI-Tools](apache-spark-use-bi-tools.md)
