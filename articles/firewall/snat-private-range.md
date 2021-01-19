---
title: Azure Firewall –Private SNAT-IP-Adressbereiche
description: Sie können IP-Adressbereiche für SNAT konfigurieren.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 01/11/2021
ms.author: victorh
ms.openlocfilehash: 0df91680dadbc4ac19299a4df48a585a11f044e8
ms.sourcegitcommit: 3af12dc5b0b3833acb5d591d0d5a398c926919c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98072240"
---
# <a name="azure-firewall-snat-private-ip-address-ranges"></a>Azure Firewall –Private SNAT-IP-Adressbereiche

Azure Firewall bietet eine SNAT für sämtlichen ausgehenden Datenverkehr an öffentliche IP-Adressen. Azure Firewall bietet standardmäßig kein SNAT mit Netzwerkregeln, wenn die Ziel-IP-Adresse ein privater IP-Adressbereich gemäß [IANA RFC 1918](https://tools.ietf.org/html/rfc1918) ist. Anwendungsregeln werden immer mithilfe eines [transparenten Proxys](https://wikipedia.org/wiki/Proxy_server#Transparent_proxy) unabhängig von der IP-Zieladresse angewendet.

Diese Logik funktioniert gut, wenn Sie Datenverkehr direkt an das Internet weiterleiten. Wenn Sie jedoch [Tunnelerzwingung](forced-tunneling.md) aktiviert haben, wird der Internet-gebundene Datenverkehr an eine der privaten IP-Adressen der Firewall in AzureFirewallSubnet übersetzt, wodurch die Quelle vor Ihrer lokalen Firewall verborgen wird.

Wenn Ihre Organisation einen öffentlichen IP-Adressbereich für private Netzwerke verwendet, leitet Azure Firewall den Datenverkehr per SNAT an eine der privaten IP-Adressen der Firewall in AzureFirewallSubnet weiter. Sie können Azure Firewall jedoch so konfigurieren, dass Ihr öffentlicher IP-Adressbereich **nicht** per SNAT weitergeleitet wird. Eine einzelne IP-Adresse können Sie beispielsweise so angeben: `192.168.1.10`. Einen Bereich von IP-Adressen können Sie wie folgt angeben: `192.168.1.0/24`.

- Wenn Sie Azure Firewall so konfigurieren möchten, dass SNAT unabhängig von der Ziel-IP-Adresse **niemals** angewendet wird, verwenden Sie **0.0.0.0/0** als privaten IP-Adressbereich. Mit dieser Konfiguration kann Azure Firewall niemals Datenverkehr direkt an das Internet weiterleiten. 

- Wenn Sie die Firewall so konfigurieren möchten, dass SNAT unabhängig von der Ziel-Adresse **immer** angewendet wird, verwenden Sie **255.255.255.255/32** als privaten IP-Adressbereich.

> [!IMPORTANT]
> Der angegebene private Adressbereich gilt nur für Netzwerkregeln. Zurzeit erfolgt bei Anwendungsregeln immer eine SNAT.

> [!IMPORTANT]
> Wenn Sie Ihre eigenen privaten IP-Adressbereiche festlegen und die standardmäßigen IANA RFC 1918-Adressbereiche beibehalten möchten, stellen Sie sicher, dass Ihre benutzerdefinierte Liste den IANA RFC 1918-Bereich noch enthält. 

## <a name="configure-snat-private-ip-address-ranges---azure-powershell"></a>Konfigurieren von privaten SNAT-IP-Adressbereichen – Azure PowerShell

Sie können mithilfe von Azure PowerShell private IP-Adressbereiche für die Firewall angeben.

### <a name="new-firewall"></a>Neue Firewall

Bei einer neuen Firewall lautet das Azure PowerShell-Cmdlet:

```azurepowershell
$azFw = @{
    Name               = '<fw-name>'
    ResourceGroupName  = '<resourcegroup-name>'
    Location           = '<location>'
    VirtualNetworkName = '<vnet-name>'
    PublicIpName       = '<public-ip-name>'
    PrivateRange       = @("IANAPrivateRanges", "192.168.1.0/24", "192.168.1.10")
}

New-AzFirewall @azFw
```
> [!NOTE]
> Zum Bereitstellen von Azure Firewall mit `New-AzFirewall` sind ein VNet und eine öffentliche IP-Adresse erforderlich. Einen vollständigen Bereitstellungsleitfaden finden Sie unter [Bereitstellen und Konfigurieren von Azure Firewall mithilfe von Azure PowerShell](deploy-ps.md).

> [!NOTE]
> „IANAPrivateRanges“ wird auf die aktuellen Standardeinstellungen von Azure Firewall erweitert, während die anderen Bereiche hinzugefügt werden. Damit der IANAPrivateRanges-Standard in Ihrer privaten Bereichsspezifikation beibehalten wird, muss er in Ihrer `PrivateRange`-Spezifikation verbleiben, wie in den folgenden Beispielen gezeigt wird.

Weitere Informationen finden Sie unter [New-AzFirewall](/powershell/module/az.network/new-azfirewall?view=azps-3.3.0).

### <a name="existing-firewall"></a>Vorhandene Firewall

Zum Konfigurieren einer vorhandenen Firewall verwenden Sie die folgenden Azure PowerShell-Cmdlets:

```azurepowershell
$azfw = Get-AzFirewall -Name '<fw-name>' -ResourceGroupName '<resourcegroup-name>'
$azfw.PrivateRange = @("IANAPrivateRanges","192.168.1.0/24", "192.168.1.10")
Set-AzFirewall -AzureFirewall $azfw
```

## <a name="configure-snat-private-ip-address-ranges---azure-cli"></a>Konfigurieren von privaten SNAT-IP-Adressbereichen – Azure-Befehlszeilenschnittstelle

Sie können mithilfe der Azure-Befehlszeilenschnittstelle private IP-Adressbereiche für die Firewall angeben.

### <a name="new-firewall"></a>Neue Firewall

Bei einer neuen Firewall lautet der Azure CLI-Befehl:

```azurecli-interactive
az network firewall create \
-n <fw-name> \
-g <resourcegroup-name> \
--private-ranges 192.168.1.0/24 192.168.1.10 IANAPrivateRanges
```

> [!NOTE]
> Zum Bereitstellen von Azure Firewall mithilfe des Befehls `az network firewall create` der Azure-Befehlszeilenschnittstelle sind zusätzliche Konfigurationsschritte erforderlich, um öffentliche IP-Adressen und IP-Konfigurationen zu erstellen. Einen vollständigen Bereitstellungsleitfaden finden Sie unter [Bereitstellen und Konfigurieren von Azure Firewall mit der Azure-Befehlszeilenschnittstelle](deploy-cli.md).

> [!NOTE]
> „IANAPrivateRanges“ wird auf die aktuellen Standardeinstellungen von Azure Firewall erweitert, während die anderen Bereiche hinzugefügt werden. Damit der IANAPrivateRanges-Standard in Ihrer privaten Bereichsspezifikation beibehalten wird, muss er in Ihrer `PrivateRange`-Spezifikation verbleiben, wie in den folgenden Beispielen gezeigt wird.

### <a name="existing-firewall"></a>Vorhandene Firewall

Der Befehl zum Konfigurieren einer vorhandenen Firewall mit der Azure-Befehlszeilenschnittstelle lautet:

```azurecli-interactive
az network firewall update \
-n <fw-name> \
-g <resourcegroup-name> \
--private-ranges 192.168.1.0/24 192.168.1.10 IANAPrivateRanges
```

## <a name="configure-snat-private-ip-address-ranges---arm-template"></a>Konfigurieren von privaten SNAT-IP-Adressbereichen – ARM-Vorlage

Zum Konfigurieren von SNAT während der Bereitstellung mit einer ARM-Vorlage können Sie der `additionalProperties`-Eigenschaft Folgendes hinzufügen:

```json
"additionalProperties": {
   "Network.SNAT.PrivateRanges": "IANAPrivateRanges , IPRange1, IPRange2"
},
```

## <a name="configure-snat-private-ip-address-ranges---azure-portal"></a>Konfigurieren von privaten SNAT-IP-Adressbereichen – Azure-Portal

Sie können mithilfe des Azure-Portals private IP-Adressbereiche für die Firewall angeben.

1. Wählen Sie Ihre Ressourcengruppe und dann Ihre Firewall aus.
2. Wählen Sie auf der Seite **Übersicht** die Option **Private IP-Bereiche** aus und dann den Standardwert **IANA RFC 1918**.

   Die Seite **Präfixe privater IP-Adressen bearbeiten** wird geöffnet:

   :::image type="content" source="media/snat-private-range/private-ip.png" alt-text="Präfixe privater IP-Adressen bearbeiten":::

1. Standardmäßig ist **IANAPrivateRanges** konfiguriert.
2. Bearbeiten Sie die privaten IP-Adressbereiche für Ihre Umgebung, und wählen Sie dann **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Azure Firewall-Tunnelerzwingung](forced-tunneling.md).