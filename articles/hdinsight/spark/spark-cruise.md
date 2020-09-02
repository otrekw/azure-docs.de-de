---
title: Verwenden von SparkCruise in Azure HDInsight zum Beschleunigen von Apache Spark-Abfragen
description: Erfahren Sie, wie Sie mit der SparkCruise-Optimierungsplattform die Effizienz von Apache Spark-Abfragen verbessern.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 07/27/2020
ms.openlocfilehash: 1a73b4707f83d6a23dffc20d95aa7b8a0fa465b3
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88649056"
---
# <a name="sparkcruise-on-azure-hdinsight"></a>SparkCruise in Azure HDInsight

In diesem Dokument wird das Azure HDInsight-Feature *SparkCruise* erläutert, das automatisch Apache Spark-Berechnungen wiederverwendet, um die Abfrageeffizienz zu erhöhen.

## <a name="overview"></a>Übersicht

Die Abfragen, die Sie auf einer Analyseplattform wie Apache Spark ausführen, werden in einen Abfrageplan zerlegt, der kleinere Unterabfragen enthält. Diese Unterabfragen können in Abfrageplänen für mehrere Abfragen wiederholt angezeigt werden. Jedes Mal, wenn sie auftreten, werden sie erneut ausgeführt, um die Ergebnisse zurückzugeben. Die erneute Ausführung derselben Abfrage kann jedoch ineffizient sein und zu unnötigen Berechnungskosten führen.

*SparkCruise* ist eine Plattform zur Workloadoptimierung, die gängige Berechnungen wiederverwenden kann, wodurch die gesamte Abfrageausführungszeit und die Datenübertragungskosten gesenkt werden. Die Plattform verwendet das Konzept einer *materialisierten Sicht* – eine Abfrage, deren Ergebnisse in einem vorab berechneten Formular gespeichert werden. Diese Ergebnisse können dann wiederverwendet werden, wenn die Abfrage selbst später erneut angezeigt wird, anstatt die Ergebnisse erneut zu berechnen.

## <a name="setup-and-installation"></a>Setup und Installation

SparkCruise ist für alle HDInsight 4.0-Cluster mit Spark 2.3 oder 2.4 verfügbar. Die SparkCruise-Bibliotheksdateien werden in Ihrem HDInsight-Cluster im `/opt/peregrine/`-Verzeichnis installiert. Damit *SparkCruise* ordnungsgemäß funktioniert, sind die folgenden, standardmäßig festgelegten Konfigurationseigenschaften erforderlich.

* `spark.sql.queryExecutionListeners` ist auf `com.microsoft.peregrine.spark.listeners.PlanLogListener` festgelegt, wodurch die Protokollierung von Abfrageplänen aktiviert wird.
* `spark.sql.extensions` ist auf `com.microsoft.peregrine.spark.extensions.SparkExtensionsHdi` festgelegt, wodurch die Optimiererregeln für die Onlinematerialisierung und Wiederverwendung aktiviert werden.

## <a name="computation-reuse-in-spark-sql"></a>Wiederverwendung von Berechnungen in Spark SQL

Das folgende Beispielszenario zeigt, wie Apache Spark-Abfragen mit *SparkCruise* optimiert werden. 

1. Stellen Sie über SSH eine Verbindung mit dem Hauptknoten Ihres Spark-Clusters her.
1. Geben Sie `spark-shell`ein.
1. Führen Sie den folgenden Codeausschnitt aus, der einige grundlegende Abfragen mithilfe von Beispieldaten auf dem Cluster ausführt.

    ```scala
    spark.sql("select count(*) from hivesampletable").collect
    spark.sql("select count(*) from hivesampletable").collect
    spark.sql("select distinct market from hivesampletable where querytime like '11%'").show
    spark.sql("select distinct state, country from hivesampletable where querytime like '11%'").show
    :quit
    ```
1. Analysieren Sie mit dem *SparkCruise*-Abfrageanalysetool die Abfragepläne vorheriger Abfragen, die in den Spark-Anwendungsprotokollen gespeichert werden. 

    ```
    sudo /opt/peregrine/analyze/peregrine.sh analyze views
    ```

1. Zeigen Sie die Ausgabe des Analyseprozesses an, eine Feedbackdatei. Diese Feedbackdatei enthält Anmerkungen zu zukünftigen Spark SQL-Abfragen. 

    ```
    sudo /opt/peregrine/analyze/peregrine.sh show
    ```

Mit dem `analyze`-Befehl werden die Abfragepläne analysiert und eine tabellarische Darstellung der Workload erstellt. Diese Workloadtabelle kann mit dem Notebook *WorkloadInsights* abgefragt werden, das im Repository [HDInsight SparkCruise Samples](https://github.com/Azure-Samples/azure-sparkcruise-samples) enthalten ist. Anschließend identifiziert der Befehl `views` allgemeine Unterplanausdrücke und wählt interessante Unterplanausdrücke zur zukünftigen Materialisierung und Wiederverwendung aus. Die Ausgabe ist eine Feedbackdatei mit Anmerkungen zu zukünftigen Spark SQL-Abfragen. 

Die Ausgabe des `show`-Befehls entspricht folgendem Text:

```bash
Feedback file -->

1593761760087311271 Materialize /peregrine/views/1593761760087311271
1593761760087311271 Reuse /peregrine/views/1593761760087311271
18446744073621796959 Materialize /peregrine/views/18446744073621796959
18446744073621796959 Reuse /peregrine/views/18446744073621796959
11259615723090744908 Materialize /peregrine/views/11259615723090744908
11259615723090744908 Reuse /peregrine/views/11259615723090744908
9409467400931056980 Materialize /peregrine/views/9409467400931056980
9409467400931056980 Reuse /peregrine/views/9409467400931056980

Materialized subexpressions -->

Found 4 items
-rw-r--r--   1 sshuser sshuser     113445 2020-07-24 16:46 /peregrine/views/logical_ir.csv
-rw-r--r--   1 sshuser sshuser     169458 2020-07-24 16:46 /peregrine/views/physical_ir.csv
-rw-r--r--   1 sshuser sshuser      25730 2020-07-24 16:46 /peregrine/views/views.csv
-rw-r--r--   1 sshuser sshuser        536 2020-07-24 16:46 /peregrine/views/views.stp
```

Die Feedbackdatei enthält Einträge im folgenden Format: `subplan-identifier [Materialize|Reuse] input/path/to/action`. In diesem Beispiel gibt es zwei eindeutige Signaturen: eine stellt die ersten beiden wiederholten Abfragen dar, die zweite das Filterprädikat in den letzten beiden Abfragen. Mit dieser Feedbackdatei materialisieren die folgenden Abfragen bei erneuter Übermittlung jetzt automatisch gängige Unterpläne und wiederverwenden sie. 

Um die Optimierungen zu testen, führen Sie einen weiteren Satz von Beispielabfragen aus.

1. Geben Sie `spark-shell`ein.
1. Ausführen des folgenden Codeausschnitts

    ```scala
    spark.sql("select count(*) from hivesampletable").collect
    spark.sql("select count(*) from hivesampletable").collect
    spark.sql("select distinct state, country from hivesampletable where querytime like '12%'").show
    spark.sql("select distinct market from hivesampletable where querytime like '12%'").show
    :quit
    ```

1. Sehen Sie sich die Feedbackdatei erneut an, um die Signaturen der wiederverwendeten Abfragen zu sehen.

    ```
    sudo /opt/peregrine/analyze/peregrine.sh show
    ```

Der `show`-Befehl erzeugt eine Ausgabe ähnlich dem folgenden Text:

```bash
Feedback file -->

1593761760087311271 Materialize /peregrine/views/1593761760087311271
1593761760087311271 Reuse /peregrine/views/1593761760087311271
18446744073621796959 Materialize /peregrine/views/18446744073621796959
18446744073621796959 Reuse /peregrine/views/18446744073621796959
11259615723090744908 Materialize /peregrine/views/11259615723090744908
11259615723090744908 Reuse /peregrine/views/11259615723090744908
9409467400931056980 Materialize /peregrine/views/9409467400931056980
9409467400931056980 Reuse /peregrine/views/9409467400931056980

Materialized subexpressions -->

Found 8 items
drwxr-xr-x   - root root          0 2020-07-24 17:21 /peregrine/views/11259615723090744908
drwxr-xr-x   - root root          0 2020-07-24 17:21 /peregrine/views/1593761760087311271
drwxr-xr-x   - root root          0 2020-07-24 17:22 /peregrine/views/18446744073621796959
drwxr-xr-x   - root root          0 2020-07-24 17:21 /peregrine/views/9409467400931056980
-rw-r--r--   1 root root     113445 2020-07-24 16:46 /peregrine/views/logical_ir.csv
-rw-r--r--   1 root root     169458 2020-07-24 16:46 /peregrine/views/physical_ir.csv
-rw-r--r--   1 root root      25730 2020-07-24 16:46 /peregrine/views/views.csv
-rw-r--r--   1 root root        536 2020-07-24 16:46 /peregrine/views/views.stp

```

Obwohl sich der Literalwert in der Abfrage von `'11%'` in `'12%'`geändert hat, kann *SparkCruise* weiterhin vorherige Abfragen mit neuen Abfragen mit geringfügigen Variationen wie der Entwicklung von Literalwerten und Datasetversionen abgleichen. Wenn eine Abfrage wesentliche Änderungen enthält, können Sie das Analysetool erneut ausführen, um neue Abfragen zu identifizieren, die wiederverwendet werden können.

Im Hintergrund löst *SparkCruise* eine Unterabfrage aus, um den ausgewählten Unterplan aus der ersten Abfrage zu materialisieren, die ihn enthält. Spätere Abfragen können die materialisierten Unterpläne direkt lesen, anstatt sie neu zu berechnen. In dieser Workload werden die Unterpläne von der ersten und dritten Abfrage mit einem Onlineverfahren materialisiert. Wir können die Planänderung von Abfragen sehen, nachdem die gängigen Unterpläne materialisiert worden sind.

Sie sehen, dass jetzt vier neue materialisierte Teilausdrücke verfügbar sind, die in nachfolgenden Abfragen wiederverwendet werden können.

## <a name="clean-up"></a>Bereinigung

Die Feedbackdateien, materialisierten Unterpläne und Abfrageprotokolle werden Spark-Sitzungen übergreifend beibehalten. Führen Sie den folgenden Befehl aus, um diese Dateien zu entfernen:

```bash
sudo /opt/peregrine/analyze/peregrine.sh clean
```

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden des Notebooks für Workloaderkenntnisse zum Ermitteln der Vorteile von SparkCruise](https://github.com/Azure-Samples/azure-sparkcruise-samples/tree/main/SparkCruise)
* [Verbessern der Leistung von Apache Spark-Workloads per Azure HDInsight IO Cache](apache-spark-improve-performance-iocache.md)
* [Optimieren von Apache Spark-Aufträgen in HDInsight](./apache-spark-perf.md)
* [SparkCruise: Handsfree Computation Reuse in Spark](https://people.cs.umass.edu/~aroy/sparkcruise-vldb19.pdf) (Automatische Wiederverwendung von Berechnungen in Spark)
* [Apache Spark-Richtlinien zu Azure HDInsight](./spark-best-practices.md)
