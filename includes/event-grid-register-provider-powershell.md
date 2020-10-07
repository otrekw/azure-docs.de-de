---
title: include file
description: include file
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 07/05/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 68a208af1a9aa9e73f2af99021d195f264fb21f1
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "67178707"
---
## <a name="enable-event-grid-resource-provider"></a>Aktivieren des Event Grid-Ressourcenanbieters

Wenn Sie zuvor in Ihrem Azure-Abonnement noch nicht Event Grid verwendet haben, sollten Sie den Event Grid-Ressourcenanbieter registrieren. Führen Sie den folgenden Befehl aus:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.EventGrid
```

Es kann einen Moment dauern, bis die Registrierung abgeschlossen ist. Führen Sie Folgendes aus, um den Status zu überprüfen:

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.EventGrid
```

Wenn `RegistrationStatus``Registered` ist, können Sie den Vorgang fortsetzen.
