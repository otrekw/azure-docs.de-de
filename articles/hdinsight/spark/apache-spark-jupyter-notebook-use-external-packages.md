---
title: Verwenden benutzerdefinierter Maven-Pakete mit Jupyter in Spark – Azure HDInsight
description: Eine Schritt-für-Schritt-Anleitung zum Konfigurieren von Jupyter-Notebooks, die mit Spark-Clustern in HDInsight verfügbar sind, sodass sie benutzerdefinierte Maven-Pakete verwenden.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 11/22/2019
ms.openlocfilehash: 82c61fe77e7bffea6a20e47c71561ab6dc86d12b
ms.sourcegitcommit: 28c93f364c51774e8fbde9afb5aa62f1299e649e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/30/2020
ms.locfileid: "97822249"
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Verwenden externer Pakete mit Jupyter Notebooks in Apache Spark-Clustern unter HDInsight

Erfahren Sie, wie Sie ein [Jupyter Notebook](https://jupyter.org/) im Apache Spark-Cluster auf HDInsight konfigurieren, um externe, von der Community bereitgestellte Apache **Maven**-Pakete zu verwenden, die nicht standardmäßig im Cluster enthalten sind.

Sie können das [Maven Repository](https://search.maven.org/) nach einer vollständigen Liste der verfügbaren Pakete durchsuchen. Sie können die Liste der verfügbaren Pakete auch aus anderen Quellen abrufen. Beispielsweise steht eine vollständige Liste der von der Community bereitgestellten Pakete auf [Spark-Pakete](https://spark-packages.org/)zur Verfügung.

In diesem Artikel erfahren Sie, wie Sie das Paket [spark-csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) mit dem Jupyter Notebook verwenden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

* Kenntnisse im Umgang mit Jupyter Notebooks mit Spark in HDInsight. Weitere Informationen finden Sie unter [Tutorial: Laden von Daten und Ausführen von Abfragen auf einem Apache Spark-Cluster in Azure HDInsight](./apache-spark-load-data-run-query.md).

* Das [URI-Schema](../hdinsight-hadoop-linux-information.md#URI-and-scheme) für Ihren primären Clusterspeicher. Dies ist `wasb://` für Azure Storage, `abfs://` für Azure Data Lake Storage Gen2 oder `adl://` für Azure Data Lake Storage Gen1. Wenn die sichere Übertragung für Azure Storage oder Data Lake Storage Gen2 aktiviert ist, lautet der URI `wasbs://` bzw. `abfss://`. Siehe auch die Informationen zur [sicheren Übertragung](../../storage/common/storage-require-secure-transfer.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Verwenden von externen Paketen mit Jupyter Notebooks

1. Navigieren Sie zu `https://CLUSTERNAME.azurehdinsight.net/jupyter`, wobei `CLUSTERNAME` der Name Ihres Spark-Clusters ist.

1. Erstellen Sie ein neues Notebook. Wählen Sie **Neu** und dann **Spark** aus.

    ![Erstellen eines neuen Spark Jupyter Notebooks](./media/apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Erstellen eines neuen Jupyter Notebooks")

1. Ein neues Notebook mit dem Namen „Untitled.pynb“ wird erstellt und geöffnet. Wählen Sie oben den Namen des Notebooks aus, und geben Sie einen Anzeigenamen ein.

    ![Angeben eines Namens für das Notebook](./media/apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Angeben eines neuen Namens für das Notebook")

1. Verwenden Sie den Magic-Befehl `%%configure`, um das Notebook so zu konfigurieren, dass es ein externes Paket verwendet. Stellen Sie sicher, dass Notebooks, die externe Pakete verwenden, die `%%configure` -Magic in der ersten Codezelle aufrufen. Dadurch wird sichergestellt, dass der Kernel für die Verwendung des Pakets konfiguriert ist, bevor die Sitzung gestartet wird.

    >[!IMPORTANT]  
    >Wenn Sie vergessen, den Kernel in der ersten Zelle zu konfigurieren, können Sie `%%configure` mit dem Parameter `-f` verwenden. Allerdings wird die Sitzung dadurch neu gestartet, und der bisherige Fortschritt geht verloren.

    | HDInsight-Version | Get-Help |
    |-------------------|---------|
    | Für HDInsight 3.5 und HDInsight 3.6 | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.11:1.5.0" }}`|
    |Für HDInsight 3.3 und HDInsight 3.4 | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|

1. Im obigen Codeausschnitt werden die Maven-Koordinaten für das externe Paket im zentralen Maven-Repository erwartet. In diesem Codeausschnitt ist `com.databricks:spark-csv_2.11:1.5.0` die Maven-Koordinate für das Paket **spark-csv** . Nachstehend finden Sie eine Anleitung zur Erstellung von Koordinaten für ein Paket.

    a. Suchen Sie das Paket im Maven-Repository. In diesem Artikel verwenden wir [spark-csv](https://mvnrepository.com/artifact/com.databricks/spark-csv).

    b. Sammeln Sie im Repository die Werte für **GroupId**, **ArtifactId** und **Version**. Vergewissern Sie sich, dass die erfassten Werte Ihrem Cluster entsprechen. In diesem Fall wird ein Scala 2.11- und Spark 1.5.0-Paket verwendet, möglicherweise müssen Sie jedoch für die jeweilige Scala- oder Spark-Version in Ihrem Cluster andere Versionen auswählen. Sie können die Scala-Version im Cluster ermitteln, indem Sie `scala.util.Properties.versionString` im Spark Jupyter-Kernel oder in Spark-Submit ausführen. Sie können die Spark-Version im Cluster ermitteln, indem Sie `sc.version` für Jupyter Notebooks ausführen.

    ![Verwenden von externen Paketen mit Jupyter Notebook](./media/apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Verwenden von externen Paketen mit Jupyter Notebook")

    c. Verketten Sie die drei Werte, getrennt durch einen Doppelpunkt ( **:** ).

    ```scala
    com.databricks:spark-csv_2.11:1.5.0
    ```

1. Führen Sie die Codezelle mit der `%%configure` -Magic aus. Dadurch wird die zugrunde liegende Livy-Sitzung für die Verwendung des von Ihnen bereitgestellten Pakets konfiguriert. Sie können das Paket nun in den folgenden Codezellen in Ihrem Notebook verwenden, wie unten dargestellt.

    ```scala
    val df = spark.read.format("com.databricks.spark.csv").
    option("header", "true").
    option("inferSchema", "true").
    load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    ```

    Für HDInsight 3.4 und ältere Versionen sollten Sie den folgenden Codeausschnitt verwenden.

    ```scala
    val df = sqlContext.read.format("com.databricks.spark.csv").
    option("header", "true").
    option("inferSchema", "true").
    load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    ```

1. Anschließend können Sie die Codeausschnitte wie unten dargestellt ausführen, um die Daten aus dem im vorherigen Schritt erstellten Datenrahmen anzuzeigen.

    ```scala
    df.show()
   
    df.select("Time").count()
    ```

## <a name="see-also"></a><a name="seealso"></a>Weitere Informationen

* [Übersicht: Apache Spark für Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Szenarien

* [Apache Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](apache-spark-use-bi-tools.md)
* [Apache Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](apache-spark-machine-learning-mllib-ipython.md)
* [Websiteprotokollanalyse mithilfe von Apache Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von Anwendungen

* [Erstellen einer eigenständigen Anwendung mit Scala](apache-spark-create-standalone-application.md)
* [Ausführen von Remoteaufträgen in einem Apache Spark-Cluster mithilfe von Apache Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen

* [Verwenden von externen Python-Paketen mit Jupyter-Notebooks in Apache Spark-Clustern unter HDInsight (Linux)](apache-spark-python-package-installation.md)
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](apache-spark-intellij-tool-plugin.md)
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Apache Spark-Anwendungen](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden von Apache Zeppelin Notebooks mit einem Apache Spark-Cluster unter HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernel für Jupyter Notebook in Apache Spark-Clustern für HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Ressourcen verwalten

* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](apache-spark-job-debugging.md)
