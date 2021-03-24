---
title: Problembehandlung für Apache Spark in Azure HDInsight
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zum Arbeiten mit Apache Spark und Azure HDInsight.
ms.service: hdinsight
ms.reviewer: jasonh
ms.topic: troubleshooting
ms.date: 08/22/2019
ms.custom: seodec18
ms.openlocfilehash: b54b9d932505ada890ac21c1b8de3178ad2f0042
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104867507"
---
# <a name="troubleshoot-apache-spark-by-using-azure-hdinsight"></a>Problembehandlung bei Apache Spark mit Azure HDInsight

Lernen Sie die wichtigsten Probleme und ihre Lösungen bei der Arbeit mit Apache Spark-Nutzlasten in [Apache Ambari](https://ambari.apache.org/) kennen.

## <a name="how-do-i-configure-an-apache-spark-application-by-using-apache-ambari-on-clusters"></a>Wie konfiguriere ich eine Apache Spark-Anwendung über Apache Ambari in Clustern?

Spark-Konfigurationswerte können optimiert werden, um eine `OutofMemoryError`-Ausnahme der Apache Spark-Anwendung zu vermeiden. Die folgenden Schritte zeigen die Standardwerte der Spark-Konfiguration in Azure HDInsight:

1. Melden Sie sich unter `https://CLUSTERNAME.azurehdidnsight.net` mit Ihren Clusteranmeldeinformationen bei Ambari an. Auf dem ersten Bildschirm wird ein Dashboard mit einer Übersicht angezeigt. Es gibt geringfügige kosmetische Unterschiede zwischen HDInsight 3.6 und 4.0.

1. Navigieren Sie zu **Spark2** > **Configs**.

    :::image type="content" source="./media/apache-troubleshoot-spark/apache-spark-ambari-config2.png" alt-text="Registerkarte „Configs“ auswählen" border="true":::

1. Wählen Sie in der Liste der Konfigurationen den Eintrag **Custom-spark2-defaults** aus, und erweitern Sie diesen.

1. Suchen Sie nach der Werteinstellung, die Sie anpassen müssen, z.B. **spark.executor.memory**. In diesem Fall ist der Wert **9728m** zu hoch.

    :::image type="content" source="./media/apache-troubleshoot-spark/apache-spark-ambari-config4.png" alt-text="„custom-spark-defaults“ auswählen" border="true":::

1. Legen Sie den Wert auf die empfohlene Einstellung fest. Für diese Einstellung wird der Wert **2048m** empfohlen.

1. Speichern Sie den Wert und anschließend die Konfiguration. Wählen Sie **Speichern** aus.

    :::image type="content" source="./media/apache-troubleshoot-spark/apache-spark-ambari-config6a.png" alt-text="Wert in „2048m“ ändern" border="true":::

    Schreiben Sie eine Notiz zu den Konfigurationsänderungen, und wählen Sie **Save** (Speichern) aus.

    :::image type="content" source="./media/apache-troubleshoot-spark/apache-spark-ambari-config6c.png" alt-text="Notiz zu den von Ihnen vorgenommenen Änderungen eingeben" border="true":::

    Sie werden benachrichtigt, wenn eine Konfiguration Ihre Aufmerksamkeit erfordert. Notieren Sie sich die Elemente, und wählen Sie anschließend **Proceed Anyway** (Dennoch fortfahren) aus.

    :::image type="content" source="./media/apache-troubleshoot-spark/apache-spark-ambari-config6b.png" alt-text="„Proceed Anyway“ (Dennoch fortfahren) auswählen" border="true":::

1. Wenn eine Konfiguration gespeichert ist, werden Sie aufgefordert, den Dienst neu starten. Wählen Sie **Neu starten** aus.

    :::image type="content" source="./media/apache-troubleshoot-spark/apache-spark-ambari-config7a.png" alt-text="„Neu starten“ auswählen" border="true":::

    Bestätigen Sie den Neustart.

    :::image type="content" source="./media/apache-troubleshoot-spark/apache-spark-ambari-config7b.png" alt-text="„Confirm Restart All“ (Bestätigen: Alle neu starten) auswählen" border="true":::

    Sie können die ausgeführten Prozesse überprüfen.

    :::image type="content" source="./media/apache-troubleshoot-spark/apache-spark-ambari-config7c.png" alt-text="Ausgeführte Prozesse überprüfen" border="true":::

1. Sie können Konfigurationen hinzufügen. Wählen Sie in der Liste der Konfigurationen **Custom-spark2-defaults** aus, und wählen Sie dann **Add Property** (Eigenschaft hinzufügen) aus.

    :::image type="content" source="./media/apache-troubleshoot-spark/apache-spark-ambari-config8.png" alt-text="„Add Property“ (Eigenschaft hinzufügen) auswählen" border="true":::

1. Definieren Sie eine neue Eigenschaft. Sie können eine einzelne Eigenschaft mithilfe eines Dialogfelds für bestimmte Einstellungen definieren (z.B. den Datentyp). Sie können auch mehrere Eigenschaften definieren, indem Sie eine Eigenschaft pro Zeile angeben.

    In diesem Beispiel wird die Eigenschaft **spark.driver.memory** mit dem Wert **4g** definiert.

    :::image type="content" source="./media/apache-troubleshoot-spark/apache-spark-ambari-config9.png" alt-text="Neue Eigenschaft definieren" border="true":::

1. Speichern Sie die Konfiguration, und starten Sie den Dienst wie in Schritt 6 und 7 beschrieben neu.

Diese Änderungen betreffen den gesamten Cluster, sie können jedoch beim Senden des Spark-Auftrags überschrieben werden.

## <a name="how-do-i-configure-an-apache-spark-application-by-using-a-jupyter-notebook-on-clusters"></a>Wie konfiguriere ich eine Apache Spark-Anwendung über ein Jupyter-Notebook in Clustern?

Geben Sie in der ersten Zelle im Jupyter-Notebook nach der Anweisung **%%configure** die Spark-Konfigurationen in einem gültigen JSON-Format ein. Ändern Sie die tatsächlichen Werte entsprechend den jeweiligen Anforderungen:

:::image type="content" source="./media/apache-troubleshoot-spark/add-configuration-cell.png" alt-text="Konfiguration hinzufügen" border="true":::

## <a name="how-do-i-configure-an-apache-spark-application-by-using-apache-livy-on-clusters"></a>Wie konfiguriere ich eine Apache Spark-Anwendung über Apache Livy in Clustern?

Senden Sie die Spark-Anwendung mit einem REST-Client wie cURL an Livy. Führen Sie einen Befehl aus, der dem folgenden ähnelt: Ändern Sie die tatsächlichen Werte entsprechend den jeweiligen Anforderungen:

```apache
curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
```

## <a name="how-do-i-configure-an-apache-spark-application-by-using-spark-submit-on-clusters"></a>Wie konfiguriere ich eine Apache Spark-Anwendung über spark-submit in Clustern?

Starten Sie spark-shell mit einem Befehl, der dem folgenden ähnelt. Ändern Sie den tatsächlichen Wert der Konfigurationen entsprechend den jeweiligen Anforderungen:

```apache
spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
```

### <a name="additional-reading"></a>Zusätzliche Lektüre

[Übermitteln von Apache Spark-Aufträgen in HDInsight-Clustern](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* [Spark memory management overview](https://spark.apache.org/docs/latest/tuning.html#memory-management-overview) (Übersicht über die Spark-Speicherverwaltung).

* [Debugging Spark application on HDInsight clusters](/archive/blogs/azuredatalake/spark-debugging-101) (Debuggen der Spark-Anwendung in HDInsight-Clustern).

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.