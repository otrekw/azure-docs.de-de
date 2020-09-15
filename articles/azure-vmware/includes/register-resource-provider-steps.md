---
title: Registrieren des Azure VMware Solution-Ressourcenanbieters
description: Hier sind die Schritte zum Registrieren des Azure VMware Solution-Ressourcenanbieters erläutert.
ms.topic: include
ms.date: 09/04/2020
ms.openlocfilehash: 08741181b778b44289868a9f3fb1c2368955248c
ms.sourcegitcommit: 8791f69d44150767807d215cafc4076f3ed43f9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89512363"
---
Für die Verwendung von Azure VMware Solution müssen Sie zunächst den Ressourcenanbieter für Ihr Abonnement registrieren.

```
azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

Informationen zu weiteren Möglichkeiten für die Ressourcenanbieterregistrierung finden Sie unter [Azure-Ressourcenanbieter und -typen](../../azure-resource-manager/management/resource-providers-and-types.md).