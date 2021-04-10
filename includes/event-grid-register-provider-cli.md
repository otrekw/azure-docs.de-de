---
title: include file
description: include file
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 08/17/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: cd778da5fd53cb8744a9f267384fcc8e11941582
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105645350"
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
