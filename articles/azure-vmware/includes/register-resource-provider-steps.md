---
title: Registrieren des Azure VMware Solution-Ressourcenanbieters
description: Hier sind die Schritte zum Registrieren des Azure VMware Solution-Ressourcenanbieters erläutert.
ms.topic: include
ms.date: 12/24/2020
ms.openlocfilehash: 7d24ce86f24c941c7d48d3b73576dcdfda120f51
ms.sourcegitcommit: 489ce69c0ff3f5188889ecfef5ffa76f7121e0d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/24/2020
ms.locfileid: "97770820"
---
<!-- Used in avs-deployment.md and tutorial-create-private-cloud.md -->

Für die Verwendung von Azure VMware Solution müssen Sie zunächst den Ressourcenanbieter für Ihr Abonnement registrieren.  

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