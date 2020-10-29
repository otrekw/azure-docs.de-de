---
title: Fehler „DomainNotFound“ bei der Clustererstellung in Azure HDInsight
description: Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: 1b8d437b14e98e2b10df782e5587d06fa4ed3653
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2020
ms.locfileid: "92540805"
---
# <a name="scenario-cluster-creation-fails-with-domainnotfound-error-in-azure-hdinsight"></a>Szenario: Fehler „DomainNotFound“ bei der Clustererstellung in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Fehlermeldung `DomainNotFound` bei der Clustererstellung für HDI Secure (Enterprise-Sicherheitspaket).

## <a name="cause"></a>Ursache

Falsche DNS-Einstellungen.

## <a name="resolution"></a>Lösung

Wenn die in eine Domäne eingebundenen Cluster bereitgestellt werden, erstellt HDI einen internen Benutzernamen und ein Kennwort in AAD DS (für jeden Cluster) und bindet alle Clusterknoten in diese Domäne ein. Der Domänenbeitritt erfolgt mithilfe von Samba-Tools. Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt werden:

* Der Domänenname sollte über DNS aufgelöst werden.
* Die IP-Adresse der Domänencontroller sollte in den DNS-Einstellungen für das virtuelle Netzwerk festgelegt werden, in dem der Cluster bereitgestellt wird.
* Wenn das virtuelle Netzwerk per Peering mit dem virtuellen Netzwerk von AAD DS verknüpft ist, muss dies manuell ausgeführt werden.
* Wenn Sie DNS-Weiterleitungen verwenden, muss der Domänenname innerhalb des virtuellen Netzwerks korrekt aufgelöst werden.
* Der Domänenbeitritt sollte nicht durch Sicherheitsrichtlinien (NSGs) blockiert werden.

### <a name="additional-debugging-steps"></a>Weitere Debuggingschritte

* Bereitstellen einer Windows-VM im gleichen Subnetz und Einbinden des Computers mit einem Benutzernamen und einem Kennwort in die Domäne (dies kann über die Benutzeroberfläche der Systemsteuerung erfolgen) oder

* Bereitstellen einer Ubuntu-VM im gleichen Subnetz und Einbinden des Computers in die Domäne
  * SSH-Verbindung mit dem Computer
  * sudo su
  * Ausführen des Skripts mit Benutzername und Kennwort
  * Das Skript führt einen Ping aus, erstellt die erforderlichen Konfigurationsdateien und dann die Domäne. Wenn dies erfolgreich ausgeführt wird, sind Ihre DNS-Einstellungen in Ordnung.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support** . Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.