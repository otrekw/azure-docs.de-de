---
title: Azure Firewall –Private SNAT-IP-Adressbereiche
description: Sie können private IP-Adressbereiche so konfigurieren, dass die Firewall Datenverkehr nicht per SNAT an diese Adressen weiterleitet.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 03/20/2020
ms.author: victorh
ms.openlocfilehash: ed8cef00b7de67458c607373c724a3717f14a7cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064809"
---
# <a name="azure-firewall-snat-private-ip-address-ranges"></a>Azure Firewall –Private SNAT-IP-Adressbereiche

Azure Firewall bietet kein SNAT mit Netzwerkregeln, wenn die Ziel-IP-Adresse ein privater IP-Adressbereich gemäß [IANA RFC 1918](https://tools.ietf.org/html/rfc1918) ist. Anwendungsregeln werden immer mithilfe eines [transparenten Proxy](https://wikipedia.org/wiki/Proxy_server#Transparent_proxy) unabhängig von der Ziel-IP-Adresse angewendet.

Wenn Ihre Organisation einen öffentlichen IP-Adressbereich für private Netzwerke verwendet, leitet Azure Firewall den Datenverkehr per SNAT an eine der privaten IP-Adressen der Firewall in AzureFirewallSubnet weiter. Sie können Azure Firewall jedoch so konfigurieren, dass Ihr öffentlicher IP-Adressbereich **nicht** per SNAT weitergeleitet wird.

## <a name="configure-snat-private-ip-address-ranges"></a>Konfigurieren von privaten SNAT-IP-Adressbereichen

Sie können mithilfe von Azure PowerShell einen IP-Adressbereich angeben, den die Firewall nicht per SNAT weiterleiten wird.

### <a name="new-firewall"></a>Neue Firewall

Bei einer neuen Firewall lautet der Azure PowerShell-Befehl so:

`New-AzFirewall -Name $GatewayName -ResourceGroupName $RG -Location $Location -VirtualNetworkName $vnet.Name -PublicIpName $LBPip.Name -PrivateRange @("IANAPrivateRanges","IPRange1", "IPRange2")`

> [!NOTE]
> „IANAPrivateRanges“ wird auf die aktuellen Standardeinstellungen von Azure Firewall erweitert, während die anderen Bereiche hinzugefügt werden.

Weitere Informationen finden Sie unter [New-AzFirewall](https://docs.microsoft.com/powershell/module/az.network/new-azfirewall?view=azps-3.3.0).

### <a name="existing-firewall"></a>Vorhandene Firewall

Zum Konfigurieren einer vorhandenen Firewall verwenden Sie die folgenden Azure PowerShell-Befehle:

```azurepowershell
$azfw = Get-AzFirewall -ResourceGroupName "Firewall Resource Group name"
$azfw.PrivateRange = @("IANAPrivateRanges","IPRange1", "IPRange2")
Set-AzFirewall -AzureFirewall $azfw
```

### <a name="templates"></a>Vorlagen

Sie können im Abschnitt `additionalProperties` Folgendes hinzufügen:

```
"additionalProperties": {
                    "Network.SNAT.PrivateRanges": "IANAPrivateRanges , IPRange1, IPRange2"
                },
```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie eine [Azure Firewall bereitstellen und konfigurieren](tutorial-firewall-deploy-portal.md).