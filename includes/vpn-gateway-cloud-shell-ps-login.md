---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/29/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 24d146da7946176c92902698d0f52ae01baf79ee
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "93061613"
---
Wenn Sie PowerShell lokal ausführen, öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Azure-Konto her. Das Cmdlet *Connect-AzAccount* fordert Sie zur Eingabe von Anmeldeinformationen auf. Nach der Authentifizierung werden Ihre Kontoeinstellungen heruntergeladen, damit sie Azure PowerShell zur Verfügung stehen.

Wenn Sie Azure Cloud Shell verwenden, anstatt PowerShell lokal auszuführen, werden Sie feststellen, dass Sie *Connect-AzAccount* nicht ausführen müssen. Azure Cloud Shell stellt automatisch eine Verbindung mit Ihrem Azure-Konto her, nachdem Sie **Testen** ausgewählt haben.

1. Wenn Sie PowerShell lokal ausführen, melden Sie sich an.

   ```azurepowershell
   Connect-AzAccount
   ```

1. Falls Sie über mehrere Abonnements verfügen, rufen Sie eine Liste Ihrer Azure-Abonnements ab.

   ```azurepowershell-interactive
   Get-AzSubscription
   ```

1. Geben Sie das Abonnement an, das Sie verwenden möchten.

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionName "Name of subscription"
   ```
