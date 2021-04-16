---
title: Apache Tez-Anwendung reagiert in Azure HDInsight nicht mehr
description: Apache Tez-Anwendung reagiert in Azure HDInsight nicht mehr
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 08/09/2019
ms.openlocfilehash: 56c68c26ae953034283031e2427b7a4afadee94e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98935942"
---
# <a name="scenario-apache-tez-application-hangs-in-azure-hdinsight"></a>Szenario: Apache Tez-Anwendung reagiert in Azure HDInsight nicht mehr

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Nach dem Übermitteln eines Apache Hive-Auftrags wird in der Tez-Ansicht der Auftragsstatus „Wird ausgeführt“ angezeigt, es scheint jedoch kein Fortschritt mehr stattzufinden.

## <a name="cause"></a>Ursache

Es wurden zu viele Aufträge übermittelt, die Yarn-Warteschlange ist lang.

## <a name="resolution"></a>Lösung

Skalieren Sie den Cluster zentral hoch, oder warten Sie, bis die Yarn-Warteschlange abgenommen hat.

Standardmäßig steuert `yarn.scheduler.capacity.maximum-applications` die maximale Anzahl der ausgeführten und ausstehenden Anwendungen. Der Standardwert ist `10000`.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.