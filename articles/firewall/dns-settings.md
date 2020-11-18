---
title: DNS-Einstellungen für Azure Firewall
description: Sie können Azure Firewall mit DNS-Server- und DNS-Proxyeinstellungen konfigurieren.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 11/06/2020
ms.author: victorh
ms.openlocfilehash: ad0ac040b510783656617ddbf2063cd94c80aae7
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2020
ms.locfileid: "94380945"
---
# <a name="azure-firewall-dns-settings"></a>DNS-Einstellungen für Azure Firewall

Sie können einen benutzerdefinierten DNS-Server konfigurieren und den DNS-Proxy für Azure Firewall aktivieren. Sie können diese Einstellungen über die Seite **DNS-Einstellungen** konfigurieren, wenn Sie die Firewall bereitstellen, oder die Einstellungen zu einem späteren Zeitpunkt vornehmen.

## <a name="dns-servers"></a>DNS-Server

Ein DNS-Server verwaltet Domänennamen und löst diese in IP-Adressen auf. Azure Firewall verwendet standardmäßig Azure DNS für die Namensauflösung. Mit der Einstellung **DNS-Server** können Sie eigene DNS-Server für die Namensauflösung von Azure Firewall konfigurieren. Hierbei kann es sich um einen einzelnen oder mehrerer Server handeln.

> [!NOTE]
> Bei über Azure Firewall Manager verwalteten Firewalls werden die DNS-Einstellungen in der zugehörigen Azure Firewall-Richtlinie konfiguriert.

### <a name="configure-custom-dns-servers---azure-portal"></a>Konfigurieren benutzerdefinierter DNS-Server: Azure-Portal

1. Wählen Sie unter Azure Firewall die Option **Einstellungen** und danach **DNS-Einstellungen** aus.
2. Unter **DNS-Server** können Sie vorhandene DNS-Server eingeben oder hinzufügen, die zuvor in Ihrem virtuellen Netzwerk angegeben wurden.
3. Wählen Sie **Speichern** aus.
4. Die Firewall leitet nun DNS-Datenverkehr zur Namensauflösung an den/die angegebenen DNS-Server weiter.

:::image type="content" source="media/dns-settings/dns-servers.png" alt-text="DNS-Server":::

### <a name="configure-custom-dns-servers---azure-cli"></a>Konfigurieren benutzerdefinierter DNS-Server: Azure CLI

Das folgende Beispiel aktualisiert Azure Firewall über die Azure CLI mit benutzerdefinierten DNS-Servern.

```azurecli-interactive
az network firewall update \
    --name fwName \ 
    --resource-group fwRG \
    --dns-servers 10.1.0.4 10.1.0.5
```

> [!IMPORTANT]
> Der Befehl `az network firewall` erfordert die Installation der Azure CLI-Erweiterung `azure-firewall`. Diese kann mit dem Befehl `az extension add --name azure-firewall` installiert werden. 

### <a name="configure-custom-dns-servers---azure-powershell"></a>Konfigurieren benutzerdefinierter DNS-Server: Azure PowerShell

Das folgende Beispiel aktualisiert Azure Firewall über Azure PowerShell mit benutzerdefinierten DNS-Servern.

```azurepowershell
$dnsServers = @("10.1.0.4", "10.1.0.5")
$azFw = Get-AzFirewall -Name "fwName" -ResourceGroupName "fwRG"
$azFw.DNSServer = $dnsServers

$azFw | Set-AzFirewall
```

## <a name="dns-proxy"></a>DNS-Proxy

Sie haben auch die Möglichkeit, Azure Firewall als DNS-Proxy zu konfigurieren. Ein DNS-Proxy fungiert als Vermittler für DNS-Anforderungen, die virtuelle Clientcomputer an einen DNS-Server senden. Wenn Sie einen benutzerdefinierten DNS-Server konfigurieren, sollten Sie den DNS-Proxy aktivieren, um Konflikte bei der DNS-Auflösung zu vermeiden. Zudem sollten Sie die FQDN-Filterung in Netzwerkregeln aktivieren.

Wenn Sie den DNS-Proxy nicht aktivieren, werden die DNS-Anforderungen des Clients unter Umständen zu einem anderen Zeitpunkt als bei der Firewall an einen DNS-Server weitergeleitet oder geben eine andere Antwort als die Firewall zurück. Der DNS-Proxy fügt Azure Firewall in den Pfad der Clientanforderungen ein, um Inkonsistenzen zu vermeiden.

Wenn Azure Firewall ein DNS-Proxy ist, können zwei Arten von Zwischenspeicherungsfunktionen auftreten:

- Positiver Cache: Die DNS-Auflösung ist erfolgreich. Die Firewall verwendet den TTL-Wert (Time-To-Live) des Pakets oder Objekts. 

- Negativer Cache: Die DNS-Auflösung erzielt keine Antwort oder keine Auflösung. Die Firewall speichert diese Information eine Stunde lang zwischen.

Der DNS-Proxy speichert alle aufgelösten IP-Adressen von FQDNs in Netzwerkregeln. Als Best Practice sollten Sie FQDNs verwenden, die in eine einzige IP-Adresse aufgelöst werden.  

### <a name="dns-proxy-configuration"></a>Konfiguration des DNS-Proxys

Die Konfiguration des DNS-Proxys umfasst drei Schritte:
1. Zuerst müssen Sie den DNS-Proxy in den DNS-Einstellungen von Azure Firewall aktivieren.
2. Konfigurieren Sie optional den benutzerdefinierten DNS-Server, oder verwenden Sie den angegebenen Standardserver.
3. Zum Schluss müssen Sie die private IP-Adresse von Azure Firewall in den DNS-Servereinstellungen Ihres virtuellen Netzwerks als benutzerdefinierte DNS-Adresse konfigurieren. Dadurch wird sichergestellt, dass der DNS-Datenverkehr an Azure Firewall weitergeleitet wird.

#### <a name="configure-dns-proxy---azure-portal"></a>Konfigurieren des DNS-Proxys: Azure-Portal

Zum Konfigurieren des DNS-Proxys müssen Sie die DNS-Servereinstellung des virtuellen Netzwerks so konfigurieren, dass die private IP-Adresse der Firewall verwendet wird. Aktivieren Sie dann den DNS-Proxy in den DNS-Einstellungen von **Azure Firewall**.

##### <a name="configure-virtual-network-dns-servers"></a>Konfigurieren der DNS-Server für das virtuelle Netzwerk 

1. Wählen Sie das virtuelle Netzwerk aus, an das der DNS-Datenverkehr über Azure Firewall weitergeleitet werden soll.
2. Wählen Sie unter **Einstellungen** die Option **DNS-Server** aus.
3. Klicken Sie unter **DNS-Server** auf die Option **Benutzerdefiniert**.
4. Geben Sie die private IP-Adresse der Firewall ein.
5. Wählen Sie **Speichern** aus.
6. Starten Sie die mit dem virtuellen Netzwerk verbundenen VMs, damit ihnen die neuen DNS-Servereinstellungen zugewiesen werden. VMs nutzen ihre aktuellen DNS-Einstellungen weiter, bis sie neu gestartet werden.

##### <a name="enable-dns-proxy"></a>Aktivieren des DNS-Proxys

1. Wählen Sie Ihre Azure Firewall-Instanz aus.
2. Wählen Sie unter **Einstellungen** die Option **DNS-Einstellungen** aus.
3. Der **DNS-Proxy** ist standardmäßig deaktiviert. Wenn die Firewall aktiviert ist, lauscht sie an Port 53 und leitet DNS-Anforderungen an die konfigurierten DNS-Server weiter.
4. Überprüfen Sie die Konfiguration der **DNS-Server**, um sicherzustellen, dass die Einstellungen zu Ihrer Umgebung passen.
5. Wählen Sie **Speichern** aus.

:::image type="content" source="media/dns-settings/dns-proxy.png" alt-text="DNS-Proxy":::

#### <a name="configure-dns-proxy---azure-cli"></a>Konfigurieren des DNS-Proxys: Azure CLI

Sie können die Azure CLI verwenden, um DNS-Proxyeinstellungen in Azure Firewall zu konfigurieren und virtuelle Netzwerke zu aktualisieren, sodass diese Azure Firewall als DNS-Server verwenden.

##### <a name="configure-virtual-network-dns-servers"></a>Konfigurieren der DNS-Server für das virtuelle Netzwerk

Dieses Beispiel konfiguriert das VNET für die Verwendung von Azure Firewall als DNS-Server.
 
```azurecli-interactive
az network vnet update \
    --name VNetName \ 
    --resource-group VNetRG \
    --dns-servers <firewall-private-IP>
```

##### <a name="enable-dns-proxy"></a>Aktivieren des DNS-Proxys

Dieses Beispiel aktiviert das DNS-Proxyfeature in Azure Firewall.

```azurecli-interactive
az network firewall update \
    --name fwName \ 
    --resource-group fwRG \
    --enable-dns-proxy true
```

#### <a name="configure-dns-proxy---azure-powershell"></a>Konfigurieren des DNS-Proxys: Azure PowerShell

Sie können Azure PowerShell verwenden, um DNS-Proxyeinstellungen in Azure Firewall zu konfigurieren und virtuelle Netzwerke zu aktualisieren, sodass diese Azure Firewall als DNS-Server verwenden.

##### <a name="configure-virtual-network-dns-servers"></a>Konfigurieren der DNS-Server für das virtuelle Netzwerk

 Dieses Beispiel konfiguriert das VNET für die Verwendung von Azure Firewall als DNS-Server.

```azurepowershell
$dnsServers = @("<firewall-private-IP>")
$VNet = Get-AzVirtualNetwork -Name "VNetName" -ResourceGroupName "VNetRG"
$VNet.DhcpOptions.DnsServers = $dnsServers

$VNet | Set-AzVirtualNetwork
```

##### <a name="enable-dns-proxy"></a>Aktivieren des DNS-Proxys

Dieses Beispiel aktiviert das DNS-Proxyfeature in Azure Firewall.

```azurepowershell
$azFw = Get-AzFirewall -Name "fwName" -ResourceGroupName "fwRG"
$azFw.DNSEnableProxy = $true

$azFw | Set-AzFirewall
```

## <a name="next-steps"></a>Nächste Schritte

[FQDN-Filterung in Netzwerkregeln](fqdn-filtering-network-rules.md)
