---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/26/2020
ms.author: larryfr
ms.openlocfilehash: 493c674fa161bf33436e560fdcbb9196410db931
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102209466"
---
> [!TIP]
> Nach der Anmeldung sehen Sie eine Liste der mit Ihrem Azure-Konto verknüpften Abonnements. Die Abonnementinformation mit `isDefault: true` ist das derzeit aktivierte Abonnement für Azure CLI-Befehle. Dieses Abonnement muss dasselbe sein, das Ihren Azure Machine Learning-Arbeitsbereich enthält. Sie finden die Abonnement-ID im [Azure-Portal](https://portal.azure.com), indem Sie zur Übersichtsseite Ihres Arbeitsbereichs navigieren. Sie können das SDK auch verwenden, um die Abonnement-ID aus dem Arbeitsbereichsobjekt abzurufen. Beispiel: `Workspace.from_config().subscription_id`.
> 
> Um ein anderes Abonnement auszuwählen, verwenden Sie den Befehl `az account set -s <subscription name or ID>`, und geben Sie den Namen oder die ID des Abonnements an, zu dem Sie wechseln möchten. Weitere Informationen zur Abonnementauswahl finden Sie unter [Verwenden mehrerer Azure-Abonnements](/cli/azure/manage-azure-subscriptions-azure-cli).