---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/10/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 50ce8530aca40eed07741f35be1a57bbd7cc1868
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77133603"
---
Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten.

Wenn Sie Azure PowerShell lokal ausführen, stellen Sie eine Verbindung mit Ihrem Azure-Konto her. Das Cmdlet *Connect-AzAccount* fordert Sie zur Eingabe von Anmeldeinformationen auf. Nach der Authentifizierung werden Ihre Kontoeinstellungen heruntergeladen, damit sie Azure PowerShell zur Verfügung stehen. Wenn Sie stattdessen Azure Cloud Shell verwenden, ist es nicht erforderlich, *Connect-AzAccount-* auszuführen. Azure Cloud Shell stellt automatisch eine Verbindung mit Ihrem Azure-Konto her.

```azurepowershell
Connect-AzAccount
```

Falls Sie über mehrere Abonnements verfügen, rufen Sie eine Liste Ihrer Azure-Abonnements ab.

```azurepowershell-interactive
Get-AzSubscription
```

Geben Sie das Abonnement an, das Sie verwenden möchten.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName "Name of subscription"
```