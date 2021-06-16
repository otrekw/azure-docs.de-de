---
title: Includedatei für PowerShell für Azure DNS
description: Includedatei für PowerShell für Azure DNS
services: dns
author: subsarma
ms.service: dns
ms.topic: include file for PowerShell for Azure DNS
ms.date: 04/28//2021
ms.author: subsarma
ms.custom: include file for PowerShell for Azure DNS, devx-track-azurepowershell
ms.openlocfilehash: 2454ab0a95e8f69fb72e85b8fad083793e00735e
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110720388"
---
## <a name="set-up-azure-powershell-for-azure-dns"></a>Einrichten des Azure PowerShell für Azure DNS

### <a name="before-you-begin"></a>Vorbereitung

[!INCLUDE [requires-azurerm](requires-azurerm.md)]

Vergewissern Sie sich vor Beginn der Konfiguration, dass Sie über Folgendes verfügen:

* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) registrieren.
* Sie müssen die aktuelle Version der PowerShell-Cmdlets für Azure Resource Manager installieren. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

### <a name="sign-in-to-your-azure-account"></a>Anmelden bei Ihrem Azure-Konto

Öffnen Sie die PowerShell-Konsole, und stellen Sie eine Verbindung mit Ihrem Konto her. Weitere Informationen finden Sie unter [Anmelden mit Azure PowerShell](/powershell/azure/azurerm/authenticate-azureps).

```azurepowershell-interactive
Connect-AzAccount
```

### <a name="select-the-subscription"></a>Auswählen des Abonnements
 
Überprüfen Sie die Abonnements für das Konto.

```azurepowershell-interactive
Get-AzSubscription
```

Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Azure Resource Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Da alle DNS-Ressourcen global und nicht regional sind, hat die Auswahl des Speicherorts für die Ressourcengruppe jedoch keine Auswirkungen auf Azure DNS.

Dieser Schritt kann übersprungen werden, wenn Sie eine vorhandene Ressourcengruppe verwenden.

```azurepowershell-interactive
New-AzResourceGroup -Name MyDNSResourceGroup -location "West US"
```
