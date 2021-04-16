---
title: Apache Yarn-Protokoll nicht lesbar in Azure HDInsight
description: Hier finden Sie Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern.
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: 02a79de8aee169f5f702d5fae67194c62363e8c4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98943051"
---
# <a name="scenario-unable-to-read-apache-yarn-log-in-azure-hdinsight"></a>Szenario: Das Apache YARN-Protokoll kann in Azure HDInsight nicht gelesen werden

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Die im Speicherkonto aufgefundenen Apache Yarn-Protokolle sind nicht für Menschen lesbar. Der Dateiparser funktioniert nicht und gibt die folgende Fehlermeldung aus:

```
java.io.IOException: Not a valid BCFile.
```

## <a name="cause"></a>Ursache

Das Apache Yarn-Protokoll wird im Format `IndexFile` aggregiert, das vom Dateiparser nicht unterstützt wird.

## <a name="resolution"></a>Lösung

1. Navigieren Sie in einem Webbrowser zu `https://CLUSTERNAME.azurehdinsight.net`, wobei `CLUSTERNAME` der Name Ihres Clusters ist.

1. Navigieren Sie über die Ambari-Benutzeroberfläche zu **YARN** > **Configs** > **Advanced** > **Advanced yarn-site** (YARN > Konfigurationen > Erweitert > Erweiterte Yarn-Website).

1. WASB-Speicher: Der Standardwert für `yarn.log-aggregation.file-formats` lautet `IndexedFormat,TFile`. Ändern Sie den Wert in `TFile`.

1. ADLS-Speicher: Der Standardwert für `yarn.nodemanager.log-aggregation.compression-type` lautet `gz`. Ändern Sie den Wert in `none`.

1. Speichern Sie die Änderung, und starten Sie alle betroffenen Dienste neu.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.