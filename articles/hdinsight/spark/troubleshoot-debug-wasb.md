---
title: Debuggen von WASB-Dateivorgängen in Azure HDInsight
description: Dieser Artikel beschreibt Problembehandlungsschritte und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/07/2020
ms.openlocfilehash: 1256575eea7ee80b41a875c6bcd9b281b98aa360
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77163307"
---
# <a name="debug-wasb-file-operations-in-azure-hdinsight"></a>Debuggen von WASB-Dateivorgängen in Azure HDInsight

In manchen Situationen müssen Sie genau wissen, welche Vorgänge vom WASB-Treiber in Azure Storage gestartet wurden. Für die Clientseite generiert der WASB-Treiber Protokolle für jeden Dateisystemvorgang auf **DEBUGGING**-Ebene. Der WASB-Treiber verwendet log4j zum Steuern des Protokolliergrads, die Standardeinstellung ist die Ebene **INFO**. Informationen zu Analyseprotokollen für Azure Storage-Server finden Sie unter [Azure Storage Analytics-Protokollierung](../../storage/common/storage-analytics-logging.md).

Ein generiertes Protokoll sieht in etwa wie folgt aus:

```log
18/05/13 04:15:55 DEBUG NativeAzureFileSystem: Moving wasb://xxx@yyy.blob.core.windows.net/user/livy/ulysses.txt/_temporary/0/_temporary/attempt_20180513041552_0000_m_000000_0/part-00000 to wasb://xxx@yyy.blob.core.windows.net/user/livy/ulysses.txt/part-00000
```

## <a name="turn-on-wasb-debug-log-for-file-operations"></a>Aktivieren des WASB-Debugprotokolls für Dateivorgänge

1. Navigieren Sie in einem Webbrowser zu `https://CLUSTERNAME.azurehdinsight.net`, wobei `CLUSTERNAME` der Name Ihres Spark-Clusters ist.

1. Navigieren Sie zu **Spark2** > **Configs** > **advanced spark2-log4j-properties**.

1. Ändern Sie `log4j.appender.console.Threshold=INFO` zu `log4j.appender.console.Threshold=DEBUG`.

1. Navigieren Sie zu **Advanced livy2-log4j-properties**.

1. Fügen Sie die folgende Eigenschaft hinzu:

    ```
    log4j.logger.org.apache.hadoop.fs.azure.NativeAzureFileSystem=DEBUG
    ```

1. Speichern Sie die Änderungen.

## <a name="additional-logging"></a>Zusätzliche Protokollierung

Die oben genannten Protokolle bieten allgemeine Informationen zu den Vorgängen im Dateisystem. Wenn die Informationen in diesen Protokollen nicht ausreichen oder Sie API-Aufrufe für Blobspeicher untersuchen möchten, fügen Sie `fs.azure.storage.client.logging=true` zu `core-site` hinzu. Diese Einstellung aktiviert die Java-SDK-Protokolle für den WASB-Speichertreiber und gibt jeden Aufruf des Blobspeicherservers aus. Wenn Sie Ihre Untersuchungen abgeschlossen haben, entfernen Sie die Einstellung wieder, da sich sonst der Datenträger schnell füllen kann, wodurch sich möglicherweise der Prozess verlangsamt.

Wenn das Back-End auf Azure Data Lake basiert, verwenden Sie die folgende log4j-Einstellung für die Komponente (zum Beispiel Spark/Tez/HDFS):

```
log4j.logger.com.microsoft.azure.datalake.store=ALL,adlsFile
log4j.additivity.com.microsoft.azure.datalake.store=true
log4j.appender.adlsFile=org.apache.log4j.FileAppender
log4j.appender.adlsFile.File=/var/log/adl/adl.log
log4j.appender.adlsFile.layout=org.apache.log4j.PatternLayout
log4j.appender.adlsFile.layout.ConversionPattern=%p\t%d{ISO8601}\t%r\t%c\t[%t]\t%m%n
```

Suchen Sie in `/var/log/adl/adl.log` nach den Protokollen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
