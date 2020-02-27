---
title: Löschen eines Gateways für virtuelle Netzwerke über das Portal Azure klassisch
description: Löschen eines Gateways des virtuellen Netzwerks mithilfe von PowerShell im klassischen Bereitstellungsmodell.
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 01/09/2020
ms.author: cherylmc
ms.openlocfilehash: e7283f5e28edc6f7beaad3a2743aa155f6ea6e14
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77198648"
---
# <a name="delete-a-virtual-network-gateway-using-powershell-classic"></a>Löschen eines Gateways des virtuellen Netzwerks mit PowerShell (klassisch)

> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-delete-vnet-gateway-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [Klassisch – PowerShell](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>

In diesem Artikel erfahren Sie, wie Sie ein VPN-Gateway im klassischen Bereitstellungsmodell mithilfe von PowerShell löschen. Ändern Sie nach dem Löschen des Gateways des virtuellen Netzwerks die Netzwerkkonfigurationsdatei, um nicht mehr verwendete Elemente zu entfernen.

## <a name="connect"></a>Schritt 1: Herstellen einer Verbindung mit Azure

### <a name="1-install-the-latest-powershell-cmdlets"></a>1. Installieren Sie die neuesten Azure PowerShell-Cmdlets.

[!INCLUDE [vpn-gateway-classic-powershell](../../includes/vpn-gateway-powershell-classic-locally.md)]

### <a name="2-connect-to-your-azure-account"></a>2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her.

Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

1. Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten. Verwenden Sie den folgenden Befehl, um zur Dienstverwaltung zu wechseln:

   ```powershell
   azure config mode asm
   ```
2. Stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

   ```powershell
   Add-AzureAccount
   ```

## <a name="export"></a>Schritt 2: Exportieren und Anzeigen der Netzwerkkonfigurationsdatei

Erstellen Sie auf Ihrem Computer ein Verzeichnis, und exportieren Sie die Netzwerkkonfigurationsdatei in das Verzeichnis. Sie verwenden diese Datei sowohl, um die aktuellen Konfigurationsinformationen anzuzeigen, als auch um die Netzwerkkonfiguration zu ändern.

In diesem Beispiel wird die Netzwerkkonfigurationsdatei in das Verzeichnis „C:\AzureNet“ exportiert.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Öffnen Sie die Datei in einem Text-Editor, und zeigen Sie den Namen für Ihr klassisches VNet an. Wenn Sie im Azure-Portal ein VNet erstellen, ist der vollständige von Azure verwendete Name im Portal nicht zu sehen. Beispielsweise kann ein VNet, das im Azure-Portal anscheinend den Namen „ClassicVNet1“ trägt, in der Netzwerkkonfigurationsdatei einen viel längeren Namen haben. Der Name sieht möglicherweise wie folgt aus: ‚Gruppe ClassicRG1 ClassicVNet1‘. Namen virtueller Netzwerke werden unter **VirtualNetworkSite name =** aufgelistet. Verwenden Sie zum Ausführen von PowerShell-Cmdlets die Namen in der Netzwerkkonfigurationsdatei.

## <a name="delete"></a>Schritt 3: Löschen des Gateways des virtuellen Netzwerks

Wenn Sie ein Gateway eines virtuellen Netzwerks löschen, werden alle Verbindungen mit dem VNet durch das Gateway getrennt. Wenn P2S-Clients mit Ihrem VNet verbunden sind, werden diese ohne Warnung getrennt.

In diesem Beispiel wird das Gateway für virtuelle Netzwerke gelöscht. Vergewissern Sie sich, dass Sie den vollständigen Namen des virtuellen Netzwerks aus der Netzwerkkonfigurationsdatei verwenden.

```powershell
Remove-AzureVNetGateway -VNetName "Group ClassicRG1 ClassicVNet1"
```

Bei Erfolg wird diese Rückgabe angezeigt:

```
Status : Successful
```

## <a name="modify"></a>Schritt 4: Ändern der Netzwerkkonfigurationsdatei

Wenn Sie das Gateway eines virtuellen Netzwerks löschen, ändert das Cmdlet die Netzwerkkonfigurationsdatei nicht. Sie müssen die Datei ändern, um die Elemente zu entfernen, die nicht mehr verwendet werden. Die folgenden Abschnitte helfen Ihnen beim Ändern der heruntergeladenen Netzwerkkonfigurationsdatei.

### <a name="lnsref"></a>Verweise auf lokale Netzwerkstandorte

Um Verweisinformationen auf Sites zu entfernen, nehmen Sie Konfigurationsänderungen an **ConnectionsToLocalNetwork/LocalNetworkSiteRef** vor. Das Entfernen eines lokalen Siteverweises veranlasst Azure, einen Tunnel zu löschen. Je nach der von Ihnen erstellten Konfiguration wird möglicherweise keine Datei **LocalNetworkSiteRef** aufgelistet.

```
<Gateway>
   <ConnectionsToLocalNetwork>
     <LocalNetworkSiteRef name="D1BFC9CB_Site2">
       <Connection type="IPsec" />
     </LocalNetworkSiteRef>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

Beispiel:

```
<Gateway>
   <ConnectionsToLocalNetwork>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

### <a name="lns"></a>Lokale Netzwerkstandorte

Entfernen Sie alle lokalen Sites, die Sie nicht mehr verwenden. Je nach der von Ihnen erstellten Konfiguration ist möglicherweise keine **LocalNetworkSite** aufgelistet.

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
  <LocalNetworkSite name="Site3">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>57.179.18.164</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

In diesem Beispiel haben wir nur Site3 entfernt.

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

### <a name="clientaddresss"></a>Clientadresspool

Wenn Ihr VNet über eine P2S-Verbindung verfügte, haben Sie einen **VPNClientAddressPool**. Entfernen Sie die Clientadresspools, die dem gelöschten Gateway des virtuellen Netzwerks entsprechen.

```
<Gateway>
    <VPNClientAddressPool>
      <AddressPrefix>10.1.0.0/24</AddressPrefix>
    </VPNClientAddressPool>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

Beispiel:

```
<Gateway>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

### <a name="gwsub"></a>GatewaySubnet

Löschen Sie das **GatewaySubnet**, das dem VNet entspricht.

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
   <Subnet name="GatewaySubnet">
     <AddressPrefix>10.11.1.0/29</AddressPrefix>
   </Subnet>
 </Subnets>
```

Beispiel:

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
 </Subnets>
```

## <a name="upload"></a>Schritt 5: Hochladen der Netzwerkkonfigurationsdatei

Speichern Sie die Änderungen, und laden Sie die Netzwerkkonfiguration nach Azure hoch. Stellen Sie sicher, dass Sie den Dateipfad wie für Ihre Umgebung erforderlich ändern.

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

Bei Erfolg sieht die Rückgabe so ähnlich aus wie in diesem Beispiel:

```
OperationDescription        OperationId                      OperationStatus                                                
--------------------        -----------                      ---------------                                           
Set-AzureVNetConfig         e0ee6e66-9167-cfa7-a746-7casb9   Succeeded
```
