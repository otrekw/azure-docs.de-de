---
title: include file
description: include file
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 08/17/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 579ea74c4767ba1afb6e0128c9f12ff83f7f3998
ms.sourcegitcommit: e7179fa4708c3af01f9246b5c99ab87a6f0df11c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/30/2020
ms.locfileid: "97826633"
---
## <a name="enable-the-event-grid-resource-provider"></a>Aktivieren des Event Grid-Ressourcenanbieters

Wenn Sie zuvor in Ihrem Azure-Abonnement noch nicht Event Grid verwendet haben, sollten Sie den Event Grid-Ressourcenanbieter registrieren. Führen Sie den folgenden Befehl aus, um den Anbieter zu registrieren:

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

Es kann einen Moment dauern, bis die Registrierung abgeschlossen ist. Führen Sie Folgendes aus, um den Status zu überprüfen:

```azurecli-interactive
az provider show --namespace Microsoft.EventGrid --query "registrationState"
```

Wenn `registrationState``Registered` ist, können Sie den Vorgang fortsetzen.
