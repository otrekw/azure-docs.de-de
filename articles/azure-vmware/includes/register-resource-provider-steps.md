---
title: Registrieren des Azure VMware Solution-Ressourcenanbieters
description: Hier sind die Schritte zum Registrieren des Azure VMware Solution-Ressourcenanbieters erläutert.
ms.topic: include
ms.date: 09/21/2020
ms.openlocfilehash: 96d15546c5102a69a0b19f92de33d35d2e9ab6c2
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2020
ms.locfileid: "91575740"
---
<!-- Used in avs-deployment.md and tutorial-create-private-cloud.md -->

Für die Verwendung von Azure VMware Solution müssen Sie zunächst den Ressourcenanbieter für Ihr Abonnement registrieren.

```azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

>[!TIP]
>Alternativ können Sie die grafische Benutzeroberfläche verwenden, um den **Microsoft.AVS**-Ressourcenanbieter zu registrieren.  Weitere Informationen finden Sie unter [Registrieren des Ressourcenanbieters](../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider).  
