---
title: Behandeln von Fehlern bei der Ressourcenerstellung in Azure HDInsight
description: In diesem Artikel werden allgemeine Fehler bei Kapazitätsproblemen behandelt. Außerdem erfahren Sie, wie Sie diese verhindern.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: seoapr2020
ms.date: 04/22/2020
ms.openlocfilehash: a47781bcdf60d863d6dcf77d70bc0ee496a51388
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2020
ms.locfileid: "92538798"
---
# <a name="troubleshoot-resource-creation-failures-in-azure-hdinsight"></a>Behandeln von Fehlern bei der Ressourcenerstellung in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight beschrieben.

## <a name="error-the-deployment-would-exceed-the-quota-of-800"></a>Error: Für die Bereitstellung wird die Kontingentgrenze von „800“ überschritten.

Azure verfügt über eine Kontingentgrenze von 800 Bereitstellungen pro Ressourcengruppe. Pro Ressourcengruppe, Abonnement, Konto oder anderen Bereichen werden unterschiedliche Kontingente angewendet. Ihr Abonnement kann beispielsweise so konfiguriert werden, um die Anzahl der Kerne für eine Region zu begrenzen. Der Versuch, einen virtuellen Computer mit mehr Kernen als zulässig bereitzustellen, führt zu einer Fehlermeldung, die besagt, dass das Kontingent überschritten wurde.

Um dieses Problem zu beheben, löschen Sie die Bereitstellungen, die nicht mehr benötigt werden, über das Azure-Portal, die CLI oder PowerShell.

Weitere Informationen finden Sie unter [Das Beheben von Fehlern bei Ressourcenkontingenten](../azure-resource-manager/templates/error-resource-quota.md).

## <a name="error-the-maximum-node-exceeded-the-available-cores-in-this-region"></a>Error: Die maximale Knotenzahl hat die Zahl der verfügbaren Kerne in dieser Region überschritten.

Ihr Abonnement kann so konfiguriert werden, dass die Anzahl von Kernen für eine Region begrenzt wird. Der Versuch, eine Ressource mit mehr Kernen als zulässig bereitzustellen, führt zu einer Fehlermeldung, die besagt, dass das Kontingent überschritten wurde.

Führen Sie die folgenden Schritte aus, um eine Erhöhung des Kontingents anzufordern:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), und wählen Sie **Hilfe und Support** aus.

1. Wählen Sie **Neue Supportanfrage** aus.

1. Geben Sie auf der Seite **Neue Supportanfrage** auf der Registerkarte **Grundlagen** die folgenden Informationen an:

   * **Problemtyp:** Wählen Sie **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.
   * **Abonnement:** Wählen Sie das Abonnement aus, das Sie ändern möchten.
   * **Kontingenttyp:** Wählen Sie **HDInsight** aus.

Weitere Informationen finden Sie unter [Kontingente](hdinsight-capacity-planning.md#quotas).

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support** . Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](../azure-portal/supportability/how-to-create-azure-support-request.md). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.