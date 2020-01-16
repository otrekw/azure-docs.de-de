---
title: Die Apache Ambari-Tez-Ansicht wird in Azure HDInsight langsam geladen
description: Die Apache Ambari-Tez-Ansicht wird möglicherweise langsam oder gar nicht in Azure HDInsight geladen
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: f4b1ffbc1e5b8147279d1e0320bd5f55aec90ea2
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75895092"
---
# <a name="scenario-apache-ambari-tez-view-loads-slowly-in-azure-hdinsight"></a>Szenario: Die Apache Ambari-Tez-Ansicht wird in Azure HDInsight langsam geladen

Dieser Artikel beschreibt Schritte zur Problembehandlung sowie mögliche Lösungen für Probleme bei der Verwendung von Interactive Query-Komponenten in Azure HDInsight-Clustern.

## <a name="issue"></a>Problem

Die Apache Ambari-Tez-Ansicht wird möglicherweise langsam oder gar nicht geladen. Beim Laden der Ambari-Tez-Ansicht sehen Sie möglicherweise, dass Prozesse auf Hauptknoten nicht mehr reagieren.

## <a name="cause"></a>Ursache

Der Zugriff auf Yarn ATS-APIs kann manchmal zu einer schlechten Leistung bei vor dem Oktober 2017 erstellten Clustern führen, wenn auf dem Cluster eine große Anzahl von Hive-Aufträgen ausgeführt wird.

## <a name="resolution"></a>Lösung

Dieses Problem wurde im Oktober 2017 behoben. Wenn Sie Ihren Cluster neu erstellen, wird dieses Problem nicht mehr auftreten.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
