---
title: Bereitstellen von Azure Firewall mit mehreren öffentlichen IP-Adressen mithilfe von Azure PowerShell
description: Bereitstellen einer Firewall mit mehreren öffentlichen IP-Adressen zum Schutz eines virtuellen Hubs
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: how-to
ms.date: 07/09/2020
ms.author: victorh
ms.openlocfilehash: 87af7f0f9b446fb3a54a600f61409c2cfc1a2494
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86189366"
---
# <a name="deploy-an-azure-firewall-with-multiple-public-ip-addresses"></a>Bereitstellen einer Azure Firewall-Instanz mit mehreren öffentlichen IP-Adressen

Wenn Sie einen virtuellen Hub mit Azure Firewall schützen möchten, können Sie die Firewall mithilfe von Azure PowerShell mit mehreren öffentlichen IP-Adressen bereitstellen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-the-firewall"></a>Bereitstellen der Firewall

Verwenden Sie das folgende Azure PowerShell-Beispiel, um eine Azure Firewall-Instanz zum Schutz eines virtuellen Hubs mit mehreren öffentlichen IP-Adressen bereitzustellen:

```azurepowershell
Select-AzSubscription -SubscriptionId <subscription ID> 

$rgName = <resource group name> 
$vHub = Get-AzVirtualHub -Name <hub name> 
$vHubId = $vHub.Id 
$fwpips = New-AzFirewallHubPublicIpAddress -Count 3
$hubIpAddresses = New-AzFirewallHubIpAddress -PublicIPs $fwpips 
$fw = New-AzFirewall -Name <firewall name> -ResourceGroupName $rgName `
     -Location <location> -Sku AZFW_Hub -HubIPAddresses $hubIpAddresses `
     -VirtualHubId $vHubId 
```

### <a name="delete-a-public-ip-address"></a>Löschen einer öffentlichen IP-Adresse

Sie können Azure PowerShell verwenden, um eine öffentliche IP-Adresse aus einer Azure Firewall-Instanz zu löschen. Mit dem folgenden Beispiel wird eine öffentliche IP-Adresse aus einer Firewall gelöscht. Es beginnt mit drei öffentlichen IP-Adressen.

```azurepowershell
Select-AzSubscription -SubscriptionId <subscription ID>

$azfw = get-azfirewall -Name <firewall name> -ResourceGroupName <resource group name>
$ip1 = New-AzFirewallPublicIpAddress -Address <first ip address to keep>
$ip2 = New-AzFirewallPublicIpAddress -Address <second ip address to keep>
$ipAddresses = $ip1,$ip2
$azfw.HubIPAddresses.publicIPs.Addresses = $ipAddresses
$azfw.HubIPAddresses.publicIPs.count = 2
Set-AzFirewall -AzureFirewall $azfw
```

## <a name="next-steps"></a>Nächste Schritte

[Was ist ein geschützter virtueller Hub?](secured-virtual-hub.md)