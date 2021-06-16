---
author: MikeRayMSFT
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 11/25/2018
ms.author: mikeray
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 998f771efe9bf1dac34a083f71f78c1dba36c385
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110720933"
---
## <a name="start-your-powershell-session"></a>Starten der PowerShell-Sitzung
 

Führen Sie das Cmdlet [**Connect-AzAccount**](/powershell/module/Az.Accounts/Connect-AzAccount) aus. Daraufhin wird eine Anmeldeseite angezeigt, auf der Sie Ihre Anmeldeinformationen eingeben müssen. Verwenden Sie die gleichen Anmeldeinformationen wie für die Anmeldung beim Azure-Portal.

```powershell
Connect-AzAccount
```

Wenn Sie über mehrere Abonnements verfügen, wählen Sie mit dem Cmdlet [**Set-AzContext**](/powershell/module/az.accounts/set-azcontext) das Abonnement aus, das für die PowerShell-Sitzung verwendet werden soll. Führen Sie das Cmdlet [**Get-AzContext**](/powershell/module/az.accounts/get-azcontext) aus, um das Abonnement anzuzeigen, das für die aktuelle PowerShell-Sitzung verwendet wird. Um alle Ihre Abonnements anzuzeigen, führen Sie [**Get-AzSubscription**](/powershell/module/az.accounts/get-azsubscription) aus.

```powershell
Set-AzContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```
