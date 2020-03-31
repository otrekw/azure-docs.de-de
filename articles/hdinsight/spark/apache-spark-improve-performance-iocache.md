---
title: Leistung von Apache Spark – Azure HDInsight IO Cache (Vorschauversion)
description: Enthält Informationen zu Azure HDInsight IO Cache und seiner Verwendung zum Verbessern der Apache Spark-Leistung.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/23/2019
ms.openlocfilehash: 43875b87d26f144b85454077fd3c044c820132bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75494988"
---
# <a name="improve-performance-of-apache-spark-workloads-using-azure-hdinsight-io-cache"></a>Verbessern der Leistung von Apache Spark-Workloads per Azure HDInsight IO Cache

IO Cache ist ein Dienst zum Zwischenspeichern von Daten für Azure HDInsight, mit dem die Leistung von Apache Spark-Aufträgen verbessert wird. IO Cache funktioniert auch mit [Apache TEZ](https://tez.apache.org/)- und [Apache Hive](https://hive.apache.org/)-Workloads, die auf [Apache Spark](https://spark.apache.org/)-Clustern ausgeführt werden können. Für IO Cache wird eine Open-Source-Komponente für die Zwischenspeicherung mit dem Namen RubiX verwendet. RubiX ist ein lokaler Datenträgercache zur Verwendung mit Big Data-Analysemodulen, bei denen über Cloudspeichersysteme auf Daten zugegriffen wird. RubiX ist unter Zwischenspeicherungssystemen einzigartig, da SSDs (Solid-State Drives) genutzt werden, anstatt regulären Arbeitsspeicher für die Zwischenspeicherung zu reservieren. Der Dienst IO Cache startet und verwaltet RubiX-Metadatenserver auf jedem Workerknoten des Clusters. Außerdem werden alle Dienste des Clusters für die transparente Nutzung des RubiX-Caches konfiguriert.

Die meisten SSDs ermöglichen eine Bandbreite von mehr als 1 GB pro Sekunde. Diese Bandbreite wird durch den In-Memory-Dateicache des Betriebssystems ergänzt und reicht aus, um Module für Big Data-Computeaufgaben, z.B. Apache Spark, zu laden. Das Betriebssystem bleibt für Apache Spark verfügbar, damit Aufgaben mit starker Abhängigkeit vom Arbeitsspeicher, z.B. Shufflevorgänge, verarbeitet werden können. Indem die exklusive Nutzung des regulären Arbeitsspeichers möglich ist, kann für Apache Spark eine optimale Ressourcennutzung erzielt werden.  

> [!Note]  
> Für IO Cache wird derzeit RubiX als Komponente für die Zwischenspeicherung genutzt, aber dies kann sich in zukünftigen Versionen des Diensts ändern. Es ist ratsam, IO Cache-Schnittstellen zu verwenden und keine direkten Abhängigkeiten von der RubiX-Implementierung einzurichten.
>IO Cache wird derzeit nur für Azure Blob Storage unterstützt.

## <a name="benefits-of-azure-hdinsight-io-cache"></a>Vorteile von Azure HDInsight IO Cache

Durch die Verwendung von IO Cache wird eine Leistungssteigerung für Aufträge erzielt, bei denen Daten aus Azure Blob Storage gelesen werden.

Sie müssen keine Änderungen an Ihren Spark-Aufträgen vornehmen, um bei der Verwendung von IO Cache Leistungssteigerungen zu erzielen. Wenn IO Cache deaktiviert ist, werden mit diesem Spark-Code Daten remote aus Azure Blob Storage gelesen: `spark.read.load('wasbs:///myfolder/data.parquet').count()`. Wenn der IO Cache aktiviert ist, bewirkt die gleiche Codezeile einen zwischengespeicherten Lesevorgang per IO Cache. Bei den folgenden Lesevorgängen werden die Daten lokal von der SSD gelesen. Workerknoten im HDInsight-Cluster sind mit lokal angefügten, dedizierten SSD-Laufwerken ausgerüstet. Bei HDInsight IO Cache werden diese lokalen SSDs für die Zwischenspeicherung verwendet, um die geringstmögliche Latenzebene und die höchste Bandbreite zu erzielen.

## <a name="getting-started"></a>Erste Schritte

Azure HDInsight IO Cache ist in der Vorschauversion standardmäßig deaktiviert. IO Cache ist in Azure HDInsight 3.6+-Spark-Clustern verfügbar, in denen Apache Spark 2.3 ausgeführt wird.  Führen Sie die folgenden Schritte aus, um IO Cache in HDInsight 4.0 zu aktivieren:

1. Navigieren Sie in einem Webbrowser zu `https://CLUSTERNAME.azurehdinsight.net`, wobei `CLUSTERNAME` der Name Ihres Clusters ist.

1. Wählen Sie auf der linken Seite den Dienst **IO Cache** aus.

1. Wählen Sie **Aktionen** (**Dienstaktionen** in HDI 3.6) und dann **Aktivieren** aus.

    ![Aktivieren des Diensts IO Cache in Ambari](./media/apache-spark-improve-performance-iocache/ambariui-enable-iocache.png "Aktivieren des Diensts IO Cache in Ambari")

1. Vergewissern Sie sich, dass alle betroffenen Dienste im Cluster neu gestartet wurden.

> [!NOTE]  
> Auch wenn in der Statusanzeige „Aktiviert“ angezeigt wird, wird IO Cache erst aktiviert, nachdem Sie die anderen betroffenen Dienste neu gestartet haben.

## <a name="troubleshooting"></a>Problembehandlung
  
Unter Umständen erhalten Sie Datenträgerspeicher-Fehler, wenn Sie nach dem Aktivieren von IO Cache Spark-Aufträge ausführen. Diese Fehler treten auf, da für Spark zum Speichern von Daten bei Shufflevorgängen auch lokaler Datenträgerspeicher verwendet wird. Es kann sein, dass für Spark kein SSD-Speicherplatz mehr vorhanden ist, nachdem IO Cache aktiviert und der Speicherplatz für den Spark-Speicher reduziert wurde. Standardmäßig beträgt die von IO Cache genutzte Speichermenge die Hälfte des gesamten SSD-Speicherplatzes. Die Nutzung des Datenträgerspeichers für IO Cache kann in Ambari konfiguriert werden. Wenn Sie Datenträgerspeicher-Fehler erhalten, sollten Sie die SSD-Speichermenge reduzieren, die für IO Cache genutzt wird, und den Dienst neu starten. Führen Sie die folgenden Schritte aus, um den festgelegten Speicherplatz für IO Cache zu ändern:

1. Wählen Sie in Apache Ambari auf der linken Seite den Dienst **HDFS** aus.

1. Wählen Sie die Registerkarten **Configs** und **Advanced**.

    ![Bearbeiten der erweiterten Konfiguration für Hadoop Distributed File System](./media/apache-spark-improve-performance-iocache/ambariui-hdfs-service-configs-advanced.png "Bearbeiten der erweiterten Konfiguration für Hadoop Distributed File System")

1. Scrollen Sie nach unten, und erweitern Sie den Bereich **Custom core-site**.

1. Suchen Sie nach der Eigenschaft **hadoop.cache.data.fullness.percentage**.

1. Ändern Sie den Wert im Feld.

    ![Bearbeiten des Prozentsatzes für die Speicherbelegung für IO Cache](./media/apache-spark-improve-performance-iocache/ambariui-cache-data-fullness-percentage-property.png "Bearbeiten des Prozentsatzes für die Speicherbelegung für IO Cache")

1. Wählen Sie oben rechts die Option **Save**.

1. Wählen Sie **Restart** > **Restart All Affected**.

    ![Apache Ambari: Neustarten aller betroffenen Instanzen](./media/apache-spark-improve-performance-iocache/ambariui-restart-all-affected.png "Neustarten aller betroffenen Instanzen")

1. Wählen Sie **Confirm Restart All**.

Deaktivieren Sie IO Cache, falls dies nicht funktioniert.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich weiter über IO Cache, z.B. über Leistungsbenchmarks, in diesem Blogbeitrag: [Apache Spark jobs gain up to 9x speed up with HDInsight IO Cache](https://azure.microsoft.com/blog/apache-spark-speedup-with-hdinsight-io-cache/) (Bis zu neunfache Geschwindigkeit für Apache Spark-Aufträge mit HDInsight IO Cache).
