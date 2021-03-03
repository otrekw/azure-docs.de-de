---
title: Registrieren des Azure VMware Solution-Ressourcenanbieters
description: Hier sind die Schritte zum Registrieren des Azure VMware Solution-Ressourcenanbieters erläutert.
ms.topic: include
ms.date: 02/17/2021
ms.openlocfilehash: cd4a6f3003945973f0fe5367eb198823595a412e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750482"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-create-private-cloud.md -->

Für die Verwendung von Azure VMware Solution müssen Sie zunächst den Ressourcenanbieter für Ihr Abonnement registrieren. Weitere Informationen zu Ressourcenanbietern finden Sie unter [Azure-Ressourcenanbieter und -typen](../../azure-resource-manager/management/resource-providers-and-types.md).

### <a name="azure-cli"></a>Azure CLI 

```azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

### <a name="azure-portal"></a>Azure-Portal
 
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im Menü des Azure-Portals die Option **Alle Dienste** aus.

1. Geben Sie im Feld **Alle Dienste****Abonnement** ein, und wählen Sie dann **Abonnements** aus.

1. Wählen Sie das Abonnement aus der Abonnentenliste aus, um es anzuzeigen.

1. Wählen Sie die Option **Ressourcenanbieter** aus, und geben Sie **Microsoft.AVS** als Suchbegriff ein. 
 
1. Wählen Sie **Registrieren** aus, falls der Ressourcenanbieter nicht registriert ist.