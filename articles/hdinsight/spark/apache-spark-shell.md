---
title: Verwenden einer interaktiven Spark-Shell in Azure HDInsight
description: Eine interaktive Spark-Shell bietet einen „Lesen-Ausführen-Anzeigen“-Prozesses, um Spark-Befehle nacheinander auszuführen und die Ergebnisse anzuzeigen.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/12/2019
ms.openlocfilehash: f088b8210b8170d22e84d131f0a72f5f8caa3b92
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75435229"
---
# <a name="run-apache-spark-from-the-spark-shell"></a>Ausführen von Apache Spark aus der Spark-Shell

Eine interaktive [Apache Spark](https://spark.apache.org/)-Shell bietet eine REPL-Umgebung (Read-Execute-Print-Loop, „Lesen-Ausführen-Anzeigen“-Schleife), um Spark-Befehle nacheinander auszuführen und die Ergebnisse anzuzeigen. Dieser Vorgang ist nützlich für Entwicklung und Debuggen. Spark bietet für jede unterstützte Sprache eine Shell: Scala, Python und R.

## <a name="run-an-apache-spark-shell"></a>Ausführen einer Apache Spark-Shell

1. Verwenden Sie einen [ssh-Befehl](../hdinsight-hadoop-linux-use-ssh-unix.md) zum Herstellen der Verbindung mit dem Cluster. Bearbeiten Sie den folgenden Befehl, indem Sie CLUSTERNAME durch den Namen Ihres Clusters ersetzen, und geben Sie den Befehl dann ein:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Spark umfasst Shells für Scala (spark-shell) und Python (pyspark). Geben Sie in der SSH-Sitzung einen der folgenden Befehle ein:

    ```bash
    spark-shell
    pyspark
    ```

    Sie können jetzt Spark-Befehle in der entsprechenden Sprache eingeben.

1. Einige grundlegende Beispielbefehle:

    ```scala
    // Load data
    var data = spark.read.format("csv").option("header", "true").option("inferSchema", "true").load("/HdiSamples/HdiSamples/SensorSampleData/building/building.csv")

    // Show data
    data.show()

    // Select certain columns
    data.select($"BuildingID", $"Country").show(10)

    // exit shell
    :q
    ```

## <a name="sparksession-and-sparkcontext-instances"></a>SparkSession- und SparkContext-Instanzen

Beim Ausführen der Spark-Shell werden standardmäßig automatisch Instanzen von SparkSession und SparkContext für Sie erzeugt.

Um auf die SparkSession-Instanz zuzugreifen, geben Sie `spark` ein. Um auf die SparkContext-Instanz zuzugreifen, geben Sie `sc` ein.

## <a name="important-shell-parameters"></a>Wichtige Shellparameter

Der Spark-Shell-Befehl (`spark-shell` oder `pyspark`) unterstützt viele Befehlszeilenparameter. Um eine vollständige Liste der Parameter anzuzeigen, starten Sie die Spark-Shell mit dem Schalter `--help`. Einige dieser Parameter gelten möglicherweise nur für `spark-submit`, den die Spark-Shell umschließt.

| Schalter | description | Beispiel |
| --- | --- | --- |
| --master MASTER-URL | Gibt die Master-URL an. In HDInsight ist dieser Wert immer `yarn`. | `--master yarn`|
| --jars JAR-LISTE | Durch Trennzeichen getrennte Liste der lokalen JAR-Dateien zum Einschließen der Treiber- und Executorklassenpfade. In HDInsight besteht diese Liste aus Pfaden zum Standarddateisystem in Azure Storage oder Data Lake Storage. | `--jars /path/to/examples.jar` |
| --packages MAVEN-KOORDINATEN | Durch Trennzeichen getrennte Liste der Maven-Koordinaten von JAR-Dateien zum Einschließen der Treiber- und Executorklassenpfade. Durchsucht zuerst das lokale, anschließend das zentrale Maven-Repository und dann etwaige zusätzliche Remote-Repositorys, die mit `--repositories` angegeben werden. Das Format für die Koordinaten ist *groupId*:*artifactId*:*version*. | `--packages "com.microsoft.azure:azure-eventhubs:0.14.0"`|
| --py-files LISTE | Nur für Python eine durch Trennzeichen getrennte Liste der im PYTHONPATH zu platzierenden ZIP-, EGG- oder PY-Dateien. | `--pyfiles "samples.py"` |

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Einführung in Apache Spark in Azure HDInsight](apache-spark-overview.md).
- Informationen zum Arbeiten mit Spark-Clustern und SparkSQL finden Sie unter [Erstellen eines Apache Spark-Clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md).
- Informationen zum Schreiben von Anwendungen, die Streamingdaten mit Spark verarbeiten, finden Sie unter [Übersicht über strukturierte Apache Spark-Streamings](apache-spark-streaming-overview.md).
