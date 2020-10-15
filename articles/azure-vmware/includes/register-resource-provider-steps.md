---
title: Registrieren des Azure VMware Solution-Ressourcenanbieters
description: Hier sind die Schritte zum Registrieren des Azure VMware Solution-Ressourcenanbieters erläutert.
ms.topic: include
ms.date: 09/21/2020
ms.openlocfilehash: 96d15546c5102a69a0b19f92de33d35d2e9ab6c2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91575740"
---
<!-- Used in avs-deployment.md and tutorial-create-private-cloud.md -->

Für die Verwendung von Azure VMware Solution müssen Sie zunächst den Ressourcenanbieter für Ihr Abonnement registrieren.

```azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

>[!TIP]
>Alternativ können Sie die grafische Benutzeroberfläche verwenden, um den **Microsoft.AVS**-Ressourcenanbieter zu registrieren.  Weitere Informationen finden Sie unter [Registrieren des Ressourcenanbieters](../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider).  
