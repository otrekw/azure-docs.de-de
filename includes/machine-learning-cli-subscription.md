---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/26/2020
ms.author: larryfr
ms.openlocfilehash: 428a3ad17c81b465635207de622398e814289d87
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2020
ms.locfileid: "81616833"
---
> [!TIP]
> Nach der Anmeldung sehen Sie eine Liste der mit Ihrem Azure-Konto verknüpften Abonnements. Die Abonnementinformation mit `isDefault: true` ist das derzeit aktivierte Abonnement für Azure CLI-Befehle. Dieses Abonnement muss dasselbe sein, das Ihren Azure Machine Learning-Arbeitsbereich enthält. Sie finden die Abonnement-ID im [Azure-Portal](https://portal.azure.com), indem Sie zur Übersichtsseite Ihres Arbeitsbereichs navigieren. Sie können das SDK auch verwenden, um die Abonnement-ID aus dem Arbeitsbereichsobjekt abzurufen. Beispiel: `Workspace.from_config().subscription_id`.
> 
> Um ein anderes Abonnement auszuwählen, verwenden Sie den Befehl `az account set -s <subscription name or ID>`, und geben Sie den Namen oder die ID des Abonnements an, zu dem Sie wechseln möchten. Weitere Informationen zur Abonnementauswahl finden Sie unter [Verwenden mehrerer Azure-Abonnements](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest).