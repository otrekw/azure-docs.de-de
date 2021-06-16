---
title: Azure Firewall –Private SNAT-IP-Adressbereiche
description: Sie können IP-Adressbereiche für SNAT konfigurieren.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 04/14/2021
ms.author: victorh
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 6235e5ec34987c0a383ea776aabf0a00e345ce63
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110693679"
---
# <a name="azure-firewall-snat-private-ip-address-ranges"></a>Azure Firewall –Private SNAT-IP-Adressbereiche

Azure Firewall bietet eine SNAT für sämtlichen ausgehenden Datenverkehr an öffentliche IP-Adressen. Azure Firewall bietet standardmäßig kein SNAT mit Netzwerkregeln, wenn die Ziel-IP-Adresse ein privater IP-Adressbereich gemäß [IANA RFC 1918](https://tools.ietf.org/html/rfc1918) oder ein geteilter Adressraum gemäß [IANA RFC 6598](https://tools.ietf.org/html/rfc6598) ist. Anwendungsregeln werden immer mithilfe eines [transparenten Proxys](https://wikipedia.org/wiki/Proxy_server#Transparent_proxy) unabhängig von der IP-Zieladresse angewendet.

Diese Logik funktioniert gut, wenn Sie Datenverkehr direkt an das Internet weiterleiten. Wenn Sie jedoch [Tunnelerzwingung](forced-tunneling.md) aktiviert haben, wird der Internet-gebundene Datenverkehr an eine der privaten IP-Adressen der Firewall in AzureFirewallSubnet übersetzt, wodurch die Quelle vor Ihrer lokalen Firewall verborgen wird.

Wenn Ihre Organisation einen öffentlichen IP-Adressbereich für private Netzwerke verwendet, leitet Azure Firewall den Datenverkehr per SNAT an eine der privaten IP-Adressen der Firewall in AzureFirewallSubnet weiter. Sie können Azure Firewall jedoch so konfigurieren, dass Ihr öffentlicher IP-Adressbereich **nicht** per SNAT weitergeleitet wird. Eine einzelne IP-Adresse können Sie beispielsweise so angeben: `192.168.1.10`. Einen Bereich von IP-Adressen können Sie wie folgt angeben: `192.168.1.0/24`.

- Wenn Sie Azure Firewall so konfigurieren möchten, dass SNAT unabhängig von der Ziel-IP-Adresse **niemals** angewendet wird, verwenden Sie **0.0.0.0/0** als privaten IP-Adressbereich. Mit dieser Konfiguration kann Azure Firewall niemals Datenverkehr direkt an das Internet weiterleiten. 

- Wenn Sie die Firewall so konfigurieren möchten, dass SNAT unabhängig von der Ziel-Adresse **immer** angewendet wird, verwenden Sie **255.255.255.255/32** als privaten IP-Adressbereich.

> [!IMPORTANT]
> Der angegebene private Adressbereich gilt nur für Netzwerkregeln. Zurzeit erfolgt bei Anwendungsregeln immer eine SNAT.

> [!IMPORTANT]
> Wenn Sie Ihre eigenen privaten IP-Adressbereiche festlegen und die standardmäßigen IANA RFC 1918-Adressbereiche beibehalten möchten, stellen Sie sicher, dass Ihre benutzerdefinierte Liste den IANA RFC 1918-Bereich noch enthält. 

Sie können die privaten SNAT-IP-Adressen mithilfe der folgenden Methoden konfigurieren. Sie müssen die privaten SNAT-Adressen mithilfe der für Ihre Konfiguration geeigneten Methode konfigurieren. Firewalls, die einer Firewallrichtlinie zugeordnet sind, müssen den Bereich in der Richtlinie angeben und dürfen nicht `AdditionalProperties` verwenden.


|Methode            |Verwenden klassischer Regeln  |Verwenden der Firewallrichtlinie  |
|---------|---------|---------|
|Azure-Portal     | [Unterstützt](#classic-rules-3)| [Unterstützt](#firewall-policy-1)|
|Azure PowerShell     |[Konfigurieren von `PrivateRange`](#classic-rules)|Derzeit nicht unterstützt|
|Azure CLI|[Konfigurieren von `--private-ranges`](#classic-rules-1)|Derzeit nicht unterstützt|
|ARM-Vorlage     |[Konfigurieren von `AdditionalProperties` in der Firewalleigenschaft](#classic-rules-2)|[Konfigurieren von `snat/privateRanges` in der Firewallrichtlinie](#firewall-policy)|


## <a name="configure-snat-private-ip-address-ranges---azure-powershell"></a>Konfigurieren von privaten SNAT-IP-Adressbereichen – Azure PowerShell
### <a name="classic-rules"></a>Klassische Regeln

Sie können mithilfe von Azure PowerShell private IP-Adressbereiche für die Firewall angeben.

> [!NOTE]
> Die Firewalleigenschaft `PrivateRange` wird für Firewalls ignoriert, die einer Firewallrichtlinie zugeordnet sind. Sie müssen die Eigenschaft `SNAT` in `firewallPolicies` verwenden, wie unter [Konfigurieren von privaten SNAT-IP-Adressbereichen – ARM-Vorlage](#firewall-policy)beschrieben.

#### <a name="new-firewall"></a>Neue Firewall

Für eine neue Firewall mit klassischen Regeln lautet das Azure PowerShell-Cmdlet folgendermaßen:

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

Weitere Informationen finden Sie unter [New-AzFirewall](/powershell/module/az.network/new-azfirewall).

#### <a name="existing-firewall"></a>Vorhandene Firewall

Zum Konfigurieren einer vorhandenen Firewall mit klassischen Regeln verwenden Sie die folgenden Azure PowerShell-Cmdlets:

```azurepowershell
$azfw = Get-AzFirewall -Name '<fw-name>' -ResourceGroupName '<resourcegroup-name>'
$azfw.PrivateRange = @("IANAPrivateRanges","192.168.1.0/24", "192.168.1.10")
Set-AzFirewall -AzureFirewall $azfw
```

## <a name="configure-snat-private-ip-address-ranges---azure-cli"></a>Konfigurieren von privaten SNAT-IP-Adressbereichen – Azure-Befehlszeilenschnittstelle
### <a name="classic-rules"></a>Klassische Regeln

Sie können mithilfe der Azure CLI private IP-Adressbereiche für die Firewall mit klassischen Regeln angeben. 

#### <a name="new-firewall"></a>Neue Firewall

Für eine neue Firewall mit klassischen Regeln lautet der Azure CLI-Befehl folgendermaßen:

```azurecli-interactive
az network firewall create \
-n <fw-name> \
-g <resourcegroup-name> \
--private-ranges 192.168.1.0/24 192.168.1.10 IANAPrivateRanges
```

> [!NOTE]
> Zum Bereitstellen von Azure Firewall mithilfe des Befehls `az network firewall create` der Azure-Befehlszeilenschnittstelle sind zusätzliche Konfigurationsschritte erforderlich, um öffentliche IP-Adressen und IP-Konfigurationen zu erstellen. Einen vollständigen Bereitstellungsleitfaden finden Sie unter [Bereitstellen und Konfigurieren von Azure Firewall mit der Azure-Befehlszeilenschnittstelle](deploy-cli.md).

> [!NOTE]
> „IANAPrivateRanges“ wird auf die aktuellen Standardeinstellungen von Azure Firewall erweitert, während die anderen Bereiche hinzugefügt werden. Damit der IANAPrivateRanges-Standard in Ihrer privaten Bereichsspezifikation beibehalten wird, muss er in Ihrer `private-ranges`-Spezifikation verbleiben, wie in den folgenden Beispielen gezeigt wird.

#### <a name="existing-firewall"></a>Vorhandene Firewall

Der Azure CLI-Befehl zum Konfigurieren einer vorhandenen Firewall mit klassischen Regeln lautet folgendermaßen:

```azurecli-interactive
az network firewall update \
-n <fw-name> \
-g <resourcegroup-name> \
--private-ranges 192.168.1.0/24 192.168.1.10 IANAPrivateRanges
```

## <a name="configure-snat-private-ip-address-ranges---arm-template"></a>Konfigurieren von privaten SNAT-IP-Adressbereichen – ARM-Vorlage
### <a name="classic-rules"></a>Klassische Regeln

Zum Konfigurieren von SNAT während der Bereitstellung mit einer ARM-Vorlage können Sie der `additionalProperties`-Eigenschaft Folgendes hinzufügen:

```json
"additionalProperties": {
   "Network.SNAT.PrivateRanges": "IANAPrivateRanges , IPRange1, IPRange2"
},
```
### <a name="firewall-policy"></a>Firewallrichtlinie

Azure Firewalls, die einer Firewallrichtlinie zugeordnet sind, unterstützen private SNAT-Bereiche seit API-Version 2020-11-01. Derzeit können Sie eine Vorlage verwenden, um den privaten SNAT-Bereich in der Firewallrichtlinie zu aktualisieren. Im folgenden Beispiel wird die Firewall so konfiguriert, dass SNAT-Netzwerkdatenverkehr **immer** zugelassen wird:

```json
{ 

            "type": "Microsoft.Network/firewallPolicies", 
            "apiVersion": "2020-11-01", 
            "name": "[parameters('firewallPolicies_DatabasePolicy_name')]", 
            "location": "eastus", 
            "properties": { 
                "sku": { 
                    "tier": "Standard" 
                }, 
                "snat": { 
                    "privateRanges": [255.255.255.255/32] 
                } 
            } 
```

## <a name="configure-snat-private-ip-address-ranges---azure-portal"></a>Konfigurieren von privaten SNAT-IP-Adressbereichen – Azure-Portal
### <a name="classic-rules"></a>Klassische Regeln

Sie können mithilfe des Azure-Portals private IP-Adressbereiche für die Firewall angeben.

1. Wählen Sie Ihre Ressourcengruppe und dann Ihre Firewall aus.
2. Wählen Sie auf der Seite **Übersicht** die Option **Private IP-Bereiche** aus und dann den Standardwert **IANA RFC 1918**.

   Die Seite **Präfixe privater IP-Adressen bearbeiten** wird geöffnet:

   :::image type="content" source="media/snat-private-range/private-ip.png" alt-text="Präfixe privater IP-Adressen bearbeiten":::

1. Standardmäßig ist **IANAPrivateRanges** konfiguriert.
2. Bearbeiten Sie die privaten IP-Adressbereiche für Ihre Umgebung, und wählen Sie dann **Speichern** aus.

### <a name="firewall-policy"></a>Firewallrichtlinie

1.  Wählen Sie Ihre Ressourcengruppe und dann Ihre Firewallrichtlinie aus.
2.  Wählen Sie **Private IP-Adressbereiche (SNAT)** in der Spalte **Einstellungen** aus.

    Standardmäßig ist **SNAT-Standardverhalten der Azure Firewall-Richtlinie verwenden** ausgewählt. 
3. Deaktivieren Sie das Kontrollkästchen, um die SNAT-Konfiguration anzupassen, und wählen Sie unter **SNAT durchführen** die Bedingungen für die Ausführung von SNAT für Ihre Umgebung aus.
      :::image type="content" source="media/snat-private-range/private-ip-ranges-snat.png" alt-text="Private IP-Adressbereiche (SNAT)":::


4.   Wählen Sie **Übernehmen**.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Azure Firewall-Tunnelerzwingung](forced-tunneling.md).
