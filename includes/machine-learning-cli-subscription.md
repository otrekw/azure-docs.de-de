---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/26/2020
ms.author: larryfr
ms.openlocfilehash: df4735699f71256598bfd418a505ae8d3e5b816c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80296874"
---
Nach der Anmeldung sehen Sie eine Liste der mit Ihrem Azure-Konto verknüpften Abonnements. Die Abonnementinformation mit `isDefault: true` ist das derzeit aktivierte Abonnement für Azure CLI-Befehle. Dieses Abonnement muss dasselbe sein, das Ihren Azure Machine Learning-Arbeitsbereich enthält. Sie finden die Abonnement-ID im [Azure-Portal](https://portal.azure.com), indem Sie zur Übersichtsseite Ihres Arbeitsbereichs navigieren. Sie können das SDK auch verwenden, um die Abonnement-ID aus dem Arbeitsbereichsobjekt abzurufen. Beispiel: `Workspace.from_config().subscription_id`.
    
Zur Auswahl eines anderen Abonnements verwenden Sie den Befehl [az account set](https://docs.microsoft.com/cli/azure/account#az-account-set) mit der ID des Abonnements, zu dem Sie wechseln möchten. Weitere Informationen zur Abonnementauswahl finden Sie unter [Verwenden mehrerer Azure-Abonnements](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest).