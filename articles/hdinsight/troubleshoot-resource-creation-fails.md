---
title: Behandeln von Fehlern bei der Ressourcenerstellung in Azure HDInsight
description: In diesem Artikel werden allgemeine Fehler bei Kapazitätsproblemen behandelt. Außerdem erfahren Sie, wie Sie diese verhindern.
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: seoapr2020
ms.date: 04/22/2020
ms.openlocfilehash: bad934e02184c46c19dcda91e18e7c7ce2f1c884
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98944648"
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

[!INCLUDE [troubleshooting next steps](../../includes/hdinsight-troubleshooting-next-steps.md)]