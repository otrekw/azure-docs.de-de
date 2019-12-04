---
title: Fehler „InvalidClassException“ aus Apache Spark – Azure HDInsight
description: Apache Spark-Auftrag schlägt mit InvalidClassException, Klassenversionskonflikt in Azure HDInsight fehl
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 124d5586180258589c5db17454b8fbf1e465fc24
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74106488"
---
# <a name="apache-spark-job-fails-with-invalidclassexception-class-version-mismatch-in-azure-hdinsight"></a>Apache Spark-Auftrag schlägt mit InvalidClassException, Klassenversionskonflikt in Azure HDInsight fehl

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Apache Spark-Komponenten in Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Sie versuchen, einen Apache Spark-Auftrag in einem Spark 2. x-Cluster zu erstellen. Dabei tritt ein Fehler ähnlich dem folgenden auf:

```
18/09/18 09:32:26 WARN TaskSetManager: Lost task 0.0 in stage 1.0 (TID 1, wn7-dev-co.2zyfbddadfih0xdq0cdja4g.ax.internal.cloudapp.net, executor 4): java.io.InvalidClassException:
org.apache.commons.lang3.time.FastDateFormat; local class incompatible: stream classdesc serialVersionUID = 2, local class serialVersionUID = 1
        at java.io.ObjectStreamClass.initNonProxy(ObjectStreamClass.java:699)
        at java.io.ObjectInputStream.readNonProxyDesc(ObjectInputStream.java:1885)
        at java.io.ObjectInputStream.readClassDesc(ObjectInputStream.java:1751)
        at java.io.ObjectInputStream.readOrdinaryObject(ObjectInputStream.java:2042)
        at java.io.ObjectInputStream.readObject0(ObjectInputStream.java:1573)
```

## <a name="cause"></a>Ursache

Dieser Fehler kann durch Hinzufügen einer zusätzlichen JAR-Datei zur `spark.yarn.jars`-Konfiguration verursacht werden, insbesondere einer „schattierten“ (shaded) JAR-Datei, die eine andere Version des `commons-lang3`-Pakets enthält und einen Klassenkonflikt verursacht. Standardmäßig verwendet Spark 2.1/2/3 Version 3.5 von `commons-lang3`.

> [!TIP]
> Eine Bibliothek zu schattieren, bedeutet, den Inhalt der Bibliothek in Ihrer eigenen JAR-Datei zu speichern, wodurch das Paket geändert wird. Dies unterscheidet sich vom Verpacken der Bibliothek, bei dem die Bibliothek ohne erneutes Verpacken Ihrer eigenen JAR-Datei hinzugefügt wird.

## <a name="resolution"></a>Lösung

Entfernen Sie die JAR-Datei, oder kompilieren Sie die angepasste JAR-Datei (AzureLogAppender) erneut, und verwenden Sie das [maven-shade-Plug-In](https://maven.apache.org/plugins/maven-shade-plugin/examples/class-relocation.html), um Klassen zu verschieben.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
