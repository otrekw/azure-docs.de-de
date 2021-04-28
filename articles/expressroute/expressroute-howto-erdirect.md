---
title: 'Azure ExpressRoute: Konfigurieren von ExpressRoute Direct'
description: Hier erfahren Sie, wie Sie Azure ExpressRoute Direct mit Azure PowerShell so konfigurieren, dass eine direkte Verbindung mit dem globalen Netzwerk von Microsoft hergestellt wird.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 12/14/2020
ms.author: duau
ms.openlocfilehash: 2c49fe65cf97207a495f6c0cc78585489a0db6ff
ms.sourcegitcommit: b28e9f4d34abcb6f5ccbf112206926d5434bd0da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2021
ms.locfileid: "107228247"
---
# <a name="how-to-configure-expressroute-direct"></a>Konfigurieren von ExpressRoute Direct

ExpressRoute Direct ermöglicht das Herstellen einer direkten Verbindung mit dem globalen Netzwerk von Microsoft – dank strategisch verteilten Peeringstandorten auf der ganzen Welt. Weitere Informationen über ExpressRoute finden Sie unter [Informationen zu ExpressRoute Direct](expressroute-erdirect-about.md).

## <a name="before-you-begin"></a>Voraussetzungen

Bevor Sie ExpressRoute Direct nutzen können, müssen Sie zunächst Ihr Abonnement registrieren. Bevor Sie ExpressRoute Direct nutzen können, müssen Sie zunächst Ihr Abonnement registrieren. Führen Sie zu Ihrer Registrierung folgende Schritte über Azure PowerShell aus:
1.  Melden Sie sich bei Azure an, und wählen Sie das Abonnement aus, für das Sie sich registrieren möchten.

    ```azurepowershell-interactive
    Connect-AzAccount 

    Select-AzSubscription -Subscription "<SubscriptionID or SubscriptionName>"
    ```

2. Registrieren Sie Ihr Abonnement für die Public Preview mit dem folgenden Befehl:
    ```azurepowershell-interactive
    Register-AzProviderFeature -FeatureName AllowExpressRoutePorts -ProviderNamespace Microsoft.Network
    ```

Überprüfen Sie nach der Registrierung, ob der Ressourcenanbieter **Microsoft.Network** in Ihrem Abonnement registriert ist. Durch Registrieren eines Ressourcenanbieters wird Ihr Abonnement für die Verwendung mit dem Ressourcenanbieter konfiguriert.

## <a name="create-the-resource"></a><a name="resources"></a>Erstellen der Ressource

1. Melden Sie sich bei Azure an, und wählen Sie das Abonnement aus. Die ExpressRoute Direct-Ressource und ExpressRoute-Leitungen müssen sich im selben Abonnement befinden.

   ```powershell
   Connect-AzAccount 

   Select-AzSubscription -Subscription "<SubscriptionID or SubscriptionName>"
   ```
   
2. Registrieren Sie Ihr Abonnement noch mal bei Microsoft.Network, um auf die APIs „expressrouteportslocation“ und „expressrouteport“ zugreifen zu können.

   ```powershell
   Register-AzResourceProvider -ProviderNameSpace "Microsoft.Network"
   ```   
3. Listen Sie alle Standorte auf, an denen ExpressRoute Direct unterstützt wird.
  
   ```powershell
   Get-AzExpressRoutePortsLocation
   ```

   **Beispielausgabe**
  
   ```powershell
   Name                : Equinix-Ashburn-DC2
   Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Ashburn-D
                        C2
   ProvisioningState   : Succeeded
   Address             : 21715 Filigree Court, DC2, Building F, Ashburn, VA 20147
   Contact             : support@equinix.com
   AvailableBandwidths : []

   Name                : Equinix-Dallas-DA3
   Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Dallas-DA
                        3
   ProvisioningState   : Succeeded
   Address             : 1950 N. Stemmons Freeway, Suite 1039A, DA3, Dallas, TX 75207
   Contact             : support@equinix.com
   AvailableBandwidths : []

   Name                : Equinix-San-Jose-SV1
   Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-San-Jose-
                        SV1
   ProvisioningState   : Succeeded
   Address             : 11 Great Oaks Blvd, SV1, San Jose, CA 95119
   Contact             : support@equinix.com
   AvailableBandwidths : []
   ```
4. Bestimmen Sie, ob ein oben aufgelisteter Standort verfügbare Bandbreite aufweist.

   ```powershell
   Get-AzExpressRoutePortsLocation -LocationName "Equinix-San-Jose-SV1"
   ```

   **Beispielausgabe**

   ```powershell
   Name                : Equinix-San-Jose-SV1
   Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-San-Jose-
                        SV1
   ProvisioningState   : Succeeded
   Address             : 11 Great Oaks Blvd, SV1, San Jose, CA 95119
   Contact             : support@equinix.com
   AvailableBandwidths : [
                          {
                            "OfferName": "100 Gbps",
                            "ValueInGbps": 100
                          }
                        ]
   ```
5. Erstellen Sie eine ExpressRoute Direct-Ressource basierend auf dem oben gewählten Standort.

   ExpressRoute Direct unterstützt die Kapselungen QinQ und Dot1Q. Bei Wahl von QinQ wird jeder ExpressRoute-Leitung dynamisch ein S-Tag zugewiesen, das in der gesamten ExpressRoute Direct-Ressource eindeutig ist. Jedes C-Tag für die Leitung muss innerhalb der Leitung eindeutig sein, jedoch nicht innerhalb von ExpressRoute Direct.  

   Wenn die Kapselung Dot1Q ausgewählt wird, müssen Sie die Eindeutigkeit des C-Tags (VLAN) für die gesamte ExpressRoute Direct-Ressource sicherstellen.  

   > [!IMPORTANT]
   > Für ExpressRoute Direct kann nur ein Kapselungstyp festgelegt werden. Nach der ExpressRoute Direct-Erstellung kann die Kapselung nicht geändert werden.
   > 
 
   ```powershell 
   $ERDirect = New-AzExpressRoutePort -Name $Name -ResourceGroupName $ResourceGroupName -PeeringLocation $PeeringLocationName -BandwidthInGbps 100.0 -Encapsulation QinQ | Dot1Q -Location $AzureRegion
   ```

   > [!NOTE]
   > Das Attribut „Kapselung“ kann auch auf Dot1Q festgelegt werden. 
   >

   **Beispielausgabe:**

   ```powershell
   Name                       : Contoso-Direct
   ResourceGroupName          : Contoso-Direct-rg
   Location                   : westcentralus
   Id                         : /subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/exp
                               ressRoutePorts/Contoso-Direct
   Etag                       : W/"<etagnumber> "
   ResourceGuid               : <number>
   ProvisioningState          : Succeeded
   PeeringLocation            : Equinix-Seattle-SE2
   BandwidthInGbps            : 100
   ProvisionedBandwidthInGbps : 0
   Encapsulation              : QinQ
   Mtu                        : 1500
   EtherType                  : 0x8100
   AllocationDate             : Saturday, September 1, 2018
   Links                      : [
                                 {
                                   "Name": "link1",
                                   "Etag": "W/\"<etagnumber>\"",
                                   "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                               Network/expressRoutePorts/Contoso-Direct/links/link1",
                                   "RouterName": "tst-09xgmr-cis-1",
                                   "InterfaceName": "HundredGigE2/2/2",
                                   "PatchPanelId": "PPID",
                                   "RackId": "RackID",
                                   "ConnectorType": "SC",
                                   "AdminState": "Disabled",
                                   "ProvisioningState": "Succeeded"
                                 },
                                 {
                                   "Name": "link2",
                                   "Etag": "W/\"<etagnumber>\"",
                                   "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                               Network/expressRoutePorts/Contoso-Direct/links/link2",
                                   "RouterName": "tst-09xgmr-cis-2",
                                   "InterfaceName": "HundredGigE2/2/2",
                                   "PatchPanelId": "PPID",
                                   "RackId": "RackID",
                                   "ConnectorType": "SC",
                                   "AdminState": "Disabled",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]
   Circuits                   : []
   ```

## <a name="generate-the-letter-of-authorization-loa"></a><a name="authorization"></a>Generieren des Genehmigungsschreibens (Letter of Authorization, LOA)

Verweisen Sie auf die kürzlich erstellte ExpressRoute Direct-Ressource, geben Sie einen Kundennamen ein, für den das Genehmigungsschreiben erstellt werden soll, und definieren Sie optional einen Speicherort für das Dokument. Ohne Verweis auf einen Dateipfad wird das Dokument in das aktuelle Verzeichnis heruntergeladen.

  ```powershell 
   New-AzExpressRoutePortLOA -ExpressRoutePort $ERDirect -CustomerName TestCustomerName -Destination "C:\Users\SampleUser\Downloads" 
   ```
 **Beispielausgabe**

   ```powershell
   Written Letter of Authorization To: C:\Users\SampleUser\Downloads\LOA.pdf
   ```

## <a name="change-admin-state-of-links"></a><a name="state"></a>Ändern des Verwaltungsstatus von Verknüpfungen
   
Dieser Prozess sollte verwendet werden, um einen Layer-1-Test durchzuführen, der sicherstellt, dass jede Kreuzverbindung ordnungsgemäß in jeden Router für primäre und sekundäre Verbindungen eingebunden ist.
1. Rufen Sie ExpressRoute Direct-Details ab.

   ```powershell
   $ERDirect = Get-AzExpressRoutePort -Name $Name -ResourceGroupName $ResourceGroupName
   ```
2. Legen Sie „Verknüpfung“ auf „Aktiviert“ fest. Wiederholen Sie diesen Schritt, um jede Verknüpfung auf „Aktiviert“ festzulegen.

   „Links[0]“ ist der primäre Port, „Links[1]“ der sekundäre Port.

   ```powershell
   $ERDirect.Links[0].AdminState = "Enabled"
   Set-AzExpressRoutePort -ExpressRoutePort $ERDirect
   $ERDirect = Get-AzExpressRoutePort -Name $Name -ResourceGroupName $ResourceGroupName
   $ERDirect.Links[1].AdminState = "Enabled"
   Set-AzExpressRoutePort -ExpressRoutePort $ERDirect
   ```
   **Beispielausgabe:**

   ```powershell
   Name                       : Contoso-Direct
   ResourceGroupName          : Contoso-Direct-rg
   Location                   : westcentralus
   Id                         : /subscriptions/<number>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/exp
                             ressRoutePorts/Contoso-Direct
   Etag                       : W/"<etagnumber> "
   ResourceGuid               : <number>
   ProvisioningState          : Succeeded
   PeeringLocation            : Equinix-Seattle-SE2
   BandwidthInGbps            : 100
   ProvisionedBandwidthInGbps : 0
   Encapsulation              : QinQ
   Mtu                        : 1500
   EtherType                  : 0x8100
   AllocationDate             : Saturday, September 1, 2018
   Links                      : [
                               {
                                 "Name": "link1",
                                 "Etag": "W/\"<etagnumber>\"",
                                 "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                             Network/expressRoutePorts/Contoso-Direct/links/link1",
                                 "RouterName": "tst-09xgmr-cis-1",
                                 "InterfaceName": "HundredGigE2/2/2",
                                 "PatchPanelId": "PPID",
                                 "RackId": "RackID",
                                 "ConnectorType": "SC",
                                 "AdminState": "Enabled",
                                 "ProvisioningState": "Succeeded"
                               },
                               {
                                 "Name": "link2",
                                 "Etag": "W/\"<etagnumber>\"",
                                 "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                             Network/expressRoutePorts/Contoso-Direct/links/link2",
                                 "RouterName": "tst-09xgmr-cis-2",
                                 "InterfaceName": "HundredGigE2/2/2",
                                 "PatchPanelId": "PPID",
                                 "RackId": "RackID",
                                 "ConnectorType": "SC",
                                 "AdminState": "Enabled",
                                 "ProvisioningState": "Succeeded"
                               }
                             ]
   Circuits                   : []
   ```

   Verwenden Sie die gleiche Vorgehensweise mit `AdminState = "Disabled"`, um die Ports zu deaktivieren.

## <a name="create-a-circuit"></a><a name="circuit"></a>Erstellen einer Leitung

Sie können standardmäßig 10 Leitungen im Abonnement erstellen, in dem sich die ExpressRoute Direct-Ressource befindet. Dieser Grenzwert kann vom Support heraufgesetzt werden. Sie sind für die Nachverfolgung der Werte für die bereitgestellte und belegte Bandbreite zuständig. Die bereitgestellte Bandbreite ist die Summe der Bandbreite aller Leitungen in der ExpressRoute Direct-Ressource, und die belegte Bandbreite ist die physische Nutzung der zugrunde liegenden physischen Schnittstellen.

Es gibt zusätzliche Leitungsbandbreiten, die für ExpressRoute Direct nur zur Unterstützung der oben beschriebenen Szenarien genutzt werden können. Die Bandbreiten sind 40 GBit/s und 100 GBit/s.

**SkuTier** kann „Local“, „Standard“ oder „Premium“ sein.

**SkuFamily** kann nur „MeteredData“ sein. „Unlimited“ wird für ExpressRoute Direct nicht unterstützt.

Erstellen Sie in der ExpressRoute-Direct-Ressource eine Leitung.

  ```powershell
  New-AzExpressRouteCircuit -Name $Name -ResourceGroupName $ResourceGroupName -ExpressRoutePort $ERDirect -BandwidthinGbps 100.0  -Location $AzureRegion -SkuTier Premium -SkuFamily MeteredData 
  ```

  Andere Bandbreiten entsprechen 5,0, 10,0 und 40,0.

  **Beispielausgabe:**

  ```powershell
  Name                             : ExpressRoute-Direct-ckt
  ResourceGroupName                : Contoso-Direct-rg
  Location                         : westcentralus
  Id                               : /subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Netwo
                                   rk/expressRouteCircuits/ExpressRoute-Direct-ckt
  Etag                             : W/"<etagnumber>"
  ProvisioningState                : Succeeded
  Sku                              : {
                                     "Name": "Premium_MeteredData",
                                     "Tier": "Premium",
                                     "Family": "MeteredData"
                                   }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
    ServiceProviderProperties        : null
  ExpressRoutePort                 : {
                                     "Id": "/subscriptions/<subscriptionID>n/resourceGroups/Contoso-Direct-rg/providers/Micros
                                   oft.Network/expressRoutePorts/Contoso-Direct"
                                   }
  BandwidthInGbps                  : 10
  Stag                             : 2
  ServiceKey                       : <number>
  Peerings                         : []
  Authorizations                   : []
  AllowClassicOperations           : False
  GatewayManagerEtag     
  ```
## <a name="delete-the-resource"></a>Löschen der Ressource
Vor dem Löschen der ExpressRoute Direct-Ressource müssen Sie zunächst alle ExpressRoute-Verbindungen löschen, die auf dem ExpressRoute Direct-Portpaar erstellt wurden.
Sie können die ExpressRoute Direct-Ressource löschen, indem Sie den folgenden Befehl ausführen:
 ```powershell
   Remove-azexpressrouteport -Name $Name -Resourcegroupname -$ResourceGroupName
   ```
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu ExpressRoute Direct finden Sie in der [Übersicht](expressroute-erdirect-about.md).
