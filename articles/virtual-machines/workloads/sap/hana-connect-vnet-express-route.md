---
title: Verbinden eines virtuellen Netzwerks mit SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Verbindung eines virtuellen Netzwerks mit SAP HANA in Azure (große Instanzen) herstellen.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-sap
ms.subservice: baremetal-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/1/2021
ms.author: madhukan
ms.custom: H1Hack27Feb2017, devx-track-azurepowershell
ms.openlocfilehash: e451f969c1518a920f7828d92fdfed65cd80f69c
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111412383"
---
# <a name="connect-a-virtual-network-to-hana-large-instances"></a>Verbinden eines virtuellen Netzwerks mit großen HANA-Instanzen

Sie haben [ein virtuelles Azure-Netzwerk erstellt](hana-connect-azure-vm-large-instances.md). Jetzt können Sie dieses Netzwerk mit großen SAP HANA-Instanzen (auch als BareMetal Infrastructure-Instanzen bekannt) verbinden. In diesem Artikel werden die Schritte beschrieben, die Sie ausführen müssen.

## <a name="create-an-azure-expressroute-gateway-on-the-virtual-network"></a>Erstellen eines Azure ExpressRoute-Gateways im virtuellen Netzwerk

Erstellen Sie zuerst ein Azure ExpressRoute-Gateway im virtuellen Netzwerk. Über dieses Gateway können Sie das virtuelle Netzwerk mit der ExpressRoute-Leitung verknüpfen, für die eine Verbindung mit Ihrem Mandanten im Umfeld von HANA (große Instanzen) hergestellt wird.

> [!NOTE] 
> Die vollständige Ausführung dieses Schritts kann bis zu 30 Minuten dauern. Sie erstellen das neue Gateway im vorgesehenen Azure-Abonnement und stellen dann die Verbindung mit dem angegebenen virtuellen Azure-Netzwerk her.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

- Wenn bereits ein Gateway vorhanden ist, überprüfen Sie, ob es sich dabei um ein ExpressRoute-Gateway handelt. Falls es sich nicht um ein ExpressRoute-Gateway handelt, sollten Sie das Gateway löschen und als ExpressRoute-Gateway neu erstellen. Wenn bereits ein ExpressRoute-Gateway eingerichtet ist, fahren Sie mit dem folgenden Abschnitt [Verknüpfen virtueller Netzwerke](#link-virtual-networks) in diesem Artikel fort. 

- Verwenden Sie entweder das [Azure-Portal](https://portal.azure.com/) oder PowerShell, um ein mit Ihrem virtuellen Netzwerk verbundenes ExpressRoute-VPN-Gateway zu erstellen.
    - Wenn Sie das Azure-Portal verwenden, fügen Sie ein neues **virtuelles Netzwerkgateway** hinzu, und wählen Sie dann **ExpressRoute** als Gatewaytyp aus.
    - Bei Verwendung von PowerShell laden Sie zuerst das aktuelle [Azure PowerShell SDK](https://azure.microsoft.com/downloads/) herunter, und verwenden Sie dieses. 
 
    Über die folgenden Befehle erstellen Sie ein ExpressRoute-Gateway. Die Texte, denen _$_ vorangestellt ist, sind benutzerdefinierte Variablen, die mit Ihren spezifischen Informationen aktualisiert werden müssen.

    ```powershell
    # These Values should already exist, update to match your environment
    $myAzureRegion = "eastus"
    $myGroupName = "SAP-East-Coast"
    $myVNetName = "VNet01"
    
    # These values are used to create the gateway, update for how you wish the GW components to be named
    $myGWName = "VNet01GW"
    $myGWConfig = "VNet01GWConfig"
    $myGWPIPName = "VNet01GWPIP"
    $myGWSku = "UltraPerformance" # Supported values for HANA large instances are: UltraPerformance
    
    # These Commands create the Public IP and ExpressRoute Gateway
    $vnet = Get-AzVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
    $subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    New-AzPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
    -Location $myAzureRegion -AllocationMethod Dynamic
    $gwpip = Get-AzPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
    $gwipconfig = New-AzVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
    -PublicIpAddressId $gwpip.Id
    
    New-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
    -IpConfigurations $gwipconfig -GatewayType ExpressRoute `
    -GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
    ```

Die einzige unterstützte Gateway-SKU für SAP HANA in Azure (große Instanzen) ist **UltraPerformance**.

## <a name="link-virtual-networks"></a>Verknüpfen virtueller Netzwerke

Das virtuelle Azure-Netzwerk verfügt jetzt über ein ExpressRoute-Gateway. Verwenden Sie die von Microsoft bereitgestellten Autorisierungsinformationen, um das ExpressRoute-Gateway mit der ExpressRoute-Leitung von SAP HANA (große Instanzen) zu verbinden. Sie können die Verbindung im Azure-Portal oder mit PowerShell herstellen. Die PowerShell-Anleitung lautet wie folgt: 

Führen Sie die folgenden Befehle für jedes ExpressRoute-Gateway aus, und verwenden Sie dabei für jede Verbindung eine andere AuthGUID. Die ersten beiden Einträge im folgenden Skript stammen aus den von Microsoft bereitgestellten Informationen. Darüber hinaus ist die AuthGUID spezifisch für jedes virtuelle Netzwerk und das zugehörige Gateway. Wenn Sie ein weiteres virtuelles Azure-Netzwerk hinzufügen möchten, müssen Sie eine weitere AuthID für Ihre ExpressRoute-Leitung abrufen, die große SAP HANA-Instanzen mit Azure von Microsoft verbindet. 

```powershell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID -ExpressRouteGatewayBypass
```

> [!NOTE]
> Der letzte Parameter **ExpressRouteGatewayBypass** im Befehl „New-AzVirtualNetworkGatewayConnection“ ist neu und dient zum Aktivieren von ExpressRoute FastPath. Diese Funktionalität wurde im Mai 2019 hinzugefügt und reduziert die Netzwerklatenz zwischen Ihren Einheiten von HANA (große Instanzen) und Azure-VMs. Weitere Informationen finden Sie unter [SAP HANA-Netzwerkarchitektur (große Instanzen)](./hana-network-architecture.md). Stellen Sie sicher, dass Sie die aktuelle Version von PowerShell-Cmdlets ausführen, bevor Sie die Befehle ausführen.

Möglicherweise müssen Sie das Gateway mit mehreren ExpressRoute-Leitungen verbinden, die Ihrem Abonnement zugeordnet sind. In diesem Fall müssen Sie diesen Schritt mehrmals ausführen. Beispielsweise ist die Wahrscheinlichkeit hoch, dass Sie das virtuelle Netzwerkgateway, das das virtuelle Netzwerk mit Ihrem lokalen Netzwerk verbindet, mit der ExpressRoute-Leitung verbinden.

## <a name="applying-expressroute-fastpath-to-existing-hana-large-instance-expressroute-circuits"></a>Anwenden von ExpressRoute FastPath auf vorhandene ExpressRoute-Verbindungen von HANA (große Instanzen)
Sie haben erfahren, wie Sie die Verbindung für eine neue ExpressRoute-Leitung herstellen, die per Bereitstellung von HANA (große Instanzen) auf einem Azure ExpressRoute-Gateway in einem Ihrer virtuellen Azure-Netzwerke erstellt wurde. Aber was passiert, wenn Sie Ihre ExpressRoute-Verbindungen bereits eingerichtet haben und Ihre virtuellen Netzwerke bereits mit HANA (große Instanzen) verbunden sind? 

Der neue ExpressRoute FastPath reduziert die Netzwerklatenz. Sie sollten die Änderung anwenden, um diese reduzierte Latenz zu nutzen. Die Befehle zum Herstellen einer Verbindung mit einer neuen ExpressRoute-Leitung und zum Ändern einer vorhandenen ExpressRoute-Leitung sind identisch. Sie müssen also diese Folge von PowerShell-Befehlen ausführen, um eine vorhandene Verbindung zu ändern. 

```powershell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID -ExpressRouteGatewayBypass
```

Es ist wichtig, dass Sie den letzten Parameter wie oben gezeigt hinzufügen, um die Funktionalität von ExpressRoute FastPath zu aktivieren.


## <a name="expressroute-global-reach"></a>ExpressRoute Global Reach

Aktivieren Sie Global Reach für eines der folgenden Szenarien:

 - HANA-Systemreplikation ohne zusätzliche Proxys oder Firewalls.
 - Kopieren von Sicherungen zwischen HANA-Einheiten (große Instanzen) in zwei unterschiedlichen Regionen zur Durchführung von Systemkopien oder -aktualisierungen.

So aktivieren Sie Global Reach:

- Geben Sie einen Adressraumbereich eines Adressraums vom Typ „/29“ an. Dieser Adressbereich darf sich nicht mit einem der anderen Adressraumbereiche überschneiden, die Sie bisher verwendet haben, um eine Verbindung von HANA (große Instanzen) mit Azure herzustellen. Der Adressbereich sollte sich auch nicht mit einem der IP-Adressbereiche überschneiden, die Sie an anderer Stelle in Azure oder lokal verwendet haben.
- Es gilt eine Einschränkung in Bezug auf die ASNs (Autonomous System Number), die genutzt werden können, um Ihre lokalen Routen für HANA (große Instanzen) anzukündigen. Für Ihre lokale Umgebung dürfen keine Routen mit privaten ASNs im Bereich 65000 bis 65020 oder 65515 angekündigt werden. 
- Wenn für die lokale Umgebung der direkte Zugriff auf HANA (große Instanzen) ermöglicht wird, müssen Sie eine Gebühr für die Leitung berechnen, die für die Verbindung mit Azure verwendet wird. Weitere Informationen zu den Preisen für das Global Reach-Add-On finden Sie unter [Azure ExpressRoute – Preise](https://azure.microsoft.com/pricing/details/expressroute/).

Erstellen Sie wie unter [Öffnen einer Supportanfrage für HANA (große Instanzen)](./hana-li-portal.md#open-a-support-request-for-hana-large-instances) beschrieben eine Supportanfrage, um ein oder beide Szenarien auf Ihre Bereitstellung anzuwenden.

Die Daten und Schlüsselwörter, die Sie für Microsoft zum Weiterleiten und Ausführen Ihrer Anforderung verwenden müssen, lauten wie folgt:

- Dienst: Große SAP HANA-Instanz
- Problemtyp: Konfiguration und Setup
- Problemuntertyp: Mein Problem ist oben nicht aufgeführt.
- Betreff „Netzwerk ändern: Global Reach hinzufügen“
- Details: „Hinzufügen von Global Reach zu HANA (große Instanzen) für HANA-Mandanten (große Instanzen).“ oder „Hinzufügen von Global Reach zur lokalen Umgebung für HANA-Mandanten (große Instanzen).“
- Weitere Details zum Fall „HANA (große Instanzen) für HANA-Mandanten (große Instanzen)“: Sie müssen die **beiden Azure-Regionen** definieren, in denen sich die beiden zu verbindenden Mandanten befinden, **UND** Sie müssen den **IP-Adressbereich vom Typ „/29“** übermitteln.
- Weitere Details zum Fall „Lokale Umgebung für HANA-Mandanten (große Instanz)“: 
    - Definieren Sie die **Azure-Region**, in der der HANA-Mandant (große Instanzen) bereitgestellt wird, mit dem Sie eine direkte Verbindung herstellen möchten. 
    - Geben Sie die **GUID für die Authentifizierung** und die **Leitungspeer-ID** an, die Sie erhalten haben, als Sie Ihre ExpressRoute-Leitung zwischen Ihrer lokalen Umgebung und Azure eingerichtet haben. 
    - Benennen Sie Ihre **ASN**. 
    - Geben Sie einen **/29-IP-Adressbereich** für ExpressRoute Global Reach an.

> [!NOTE]
> Falls beide Fälle verarbeitet werden sollen, müssen Sie zwei unterschiedliche IP-Adressbereiche vom Typ „/29“ angeben, die sich nicht mit anderen bisher genutzten IP-Adressbereichen überschneiden. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über andere Netzwerkanforderungen, die Sie möglicherweise für die Bereitstellung von SAP HANA (große Instanzen) in Azure erfüllen müssen.

> [!div class="nextstepaction"]
> [Zusätzliche Netzwerkanforderungen für große Instanzen](hana-additional-network-requirements.md)
