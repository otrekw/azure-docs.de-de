---
title: Integrieren von Azure Event Hubs in den Azure Private Link-Dienst
description: Erfahren Sie, wie Sie Azure Event Hubs in den Azure Private Link-Dienst integrieren.
services: event-hubs
author: spelluru
ms.author: spelluru
ms.date: 03/12/2020
ms.service: event-hubs
ms.topic: article
ms.openlocfilehash: cff1b3b79b34d3f0bed27a2ea50799185958a8ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79473771"
---
# <a name="integrate-azure-event-hubs-with-azure-private-link-preview"></a>Integrieren von Azure Event Hubs in Azure Private Link (Vorschau)
Mit dem Azure Private Link-Dienst können Sie über einen **privaten Endpunkt** in Ihrem virtuellen Netzwerk auf Azure-Dienste wie Azure Event Hubs, Azure Storage und Azure Cosmos DB sowie auf in Azure gehostete Kunden-/Partnerdienste zugreifen.

Ein privater Endpunkt ist eine Netzwerkschnittstelle, die Sie privat und sicher mit einem von Azure Private Link betriebenen Dienst verbindet. Der private Endpunkt verwendet eine private IP-Adresse aus Ihrem VNET und bindet den Dienst dadurch in Ihr VNET ein. Der gesamte für den Dienst bestimmte Datenverkehr kann über den privaten Endpunkt geleitet werden. Es sind also keine Gateways, NAT-Geräte, ExpressRoute-/VPN-Verbindungen oder öffentlichen IP-Adressen erforderlich. Der Datenverkehr zwischen Ihrem virtuellen Netzwerk und dem Dienst wird über das Microsoft-Backbone-Netzwerk übertragen und dadurch vom öffentlichen Internet isoliert. Sie können eine Verbindung mit einer Instanz einer Azure-Ressource herstellen, was ein Höchstmaß an Granularität bei der Zugriffssteuerung ermöglicht.

Weitere Informationen finden Sie unter [Was ist Azure Private Link?](../private-link/private-link-overview.md).

> [!NOTE]
> Diese Funktion wird nur für den **Dedicated**-Tarif unterstützt. Weitere Informationen zum Dedicated-Tarif finden Sie unter [Übersicht über Event Hubs Dedicated](event-hubs-dedicated-overview.md). 
>
> Diese Funktion steht derzeit als **Vorschau** zur Verfügung. 


## <a name="add-a-private-endpoint-using-azure-portal"></a>Hinzufügen eines privaten Endpunkts über das Azure-Portal

### <a name="prerequisites"></a>Voraussetzungen

Um einen Event Hubs-Namespace in Azure Private Link zu integrieren, benötigen Sie die folgenden Entitäten oder Berechtigungen:

- Ein Event Hubs-Namespace
- Ein virtuelles Azure-Netzwerk
- Ein Subnetz in dem virtuellen Netzwerk
- Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ für den Namespace und für das virtuelle Netzwerk.

Der private Endpunkt und das virtuelle Netzwerk müssen sich in der gleichen Region befinden. Wenn Sie über das Portal eine Region für den privaten Endpunkt auswählen, wird automatisch nach virtuellen Netzwerken in dieser Region gefiltert. Der Namespace kann sich in einer anderen Region befinden.

Der private Endpunkt verwendet eine private IP-Adresse in Ihrem virtuellen Netzwerk.

### <a name="steps"></a>Schritte
Wenn Sie bereits über einen Event Hubs-Namespace verfügen, können Sie wie folgt eine Private Link-Verbindung erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Geben Sie auf der Suchleiste den Suchbegriff **Event Hubs** ein.
3. Wählen Sie in der Liste den **Namespace** aus, dem Sie einen privaten Endpunkt hinzufügen möchten.
4. Wählen Sie die Registerkarte **Netzwerk** unter **Einstellungen** aus.
5. Wählen Sie im oberen Seitenbereich die Registerkarte **Private Endpunktverbindungen (Vorschau)** aus. Wenn Sie keinen Dedicated-Tarif von Event Hubs verwenden, wird folgende Meldung angezeigt: **Private Endpunktverbindungen auf Event Hubs werden nur von Namespaces unterstützt, die unter einem dedizierten Cluster erstellt werden**.
6. Wählen Sie im oberen Seitenbereich die Schaltfläche **+ Privater Endpunkt** aus.

    ![Image](./media/private-link-service/private-link-service-3.png)
7. Führen Sie auf der Seite **Grundlagen** die folgenden Schritte aus: 
    1. Wählen Sie das **Azure-Abonnement** aus, in dem Sie den privaten Endpunkt erstellen möchten. 
    2. Wählen Sie die **Ressourcengruppe** für die private Endpunktressource aus.
    3. Geben Sie einen **Namen** für den privaten Endpunkt ein. 
    5. Wählen Sie eine **Region** für den privaten Endpunkt aus. Ihr privater Endpunkt muss sich in derselben Region wie Ihr virtuelles Netzwerk befinden, kann aber in einer anderen Region als die Private Link-Ressource enthalten sein, mit der Sie eine Verbindung herstellen. 
    6. Wählen Sie die Schaltfläche **Weiter: Ressource >** unten auf der Seite aus.

        ![Erstellen des privaten Endpunkts: Seite „Grundlagen“](./media/private-link-service/create-private-endpoint-basics-page.png)
8. Führen Sie auf der Seite **Ressource** die folgenden Schritte aus:
    1. Wenn Sie als Verbindungsmethode **Hiermit wird eine Verbindung mit einer Azure-Ressource im eigenen Verzeichnis hergestellt** auswählen, führen Sie die folgenden Schritte aus: 
        1. Wählen Sie das **Azure-Abonnement** aus, in dem Ihr **Event Hubs-Namespace** vorhanden ist. 
        2. Wählen Sie für den **Ressourcentyp** **Microsoft.EventHub/namespaces** als **Ressourcentyp** aus.
        3. Wählen Sie als **Ressource** in der Dropdownliste einen Event Hubs-Namespace aus. 
        4. Vergewissern Sie sich, dass die **Unterressource des Ziels** auf **Namespace** festgelegt ist.
        5. Wählen Sie die Schaltfläche **Weiter: Konfiguration >** unten auf der Seite aus. 
        
            ![Erstellen des privaten Endpunkts: Seite „Ressourcen“](./media/private-link-service/create-private-endpoint-resource-page.png)    
    2. Wenn Sie **Verbindung mit einer Azure-Ressource mithilfe einer Ressourcen-ID oder eines Alias herstellen** auswählen, führen Sie die folgenden Schritte aus:
        1. Geben Sie die **Ressourcen-ID** oder den **Alias** ein. Dabei kann es sich um die Ressourcen-ID oder den Alias handeln, die jemand für Sie freigegeben hat.
        2. Geben Sie für **Zielunterressource** **Namespace** ein. Dabei handelt es sich um den Unterressourcentyp, auf den der private Endpunkt zugreifen kann.
        3. (Optional) Geben Sie eine **Anforderungsnachricht** ein. Der Ressourcenbesitzer sieht diese Nachricht beim Verwalten der Verbindung mit dem privaten Endpunkt.
        4. Wählen Sie dann die Schaltfläche **Weiter: Konfiguration >** unten auf der Seite aus.

            ![Erstellen des privaten Endpunkts: Verbinden mithilfe der Ressourcen-ID](./media/private-link-service/connect-resource-id.png)
9. Wählen Sie auf der Seite **Konfiguration** das Subnetz in einem virtuellen Netzwerk aus, in dem Sie den privaten Endpunkt bereitstellen möchten. 
    1. Wählen Sie ein **virtuelles Netzwerk** aus. Nur virtuelle Netzwerke im aktuell ausgewählten Abonnement und am aktuell ausgewählten Standort werden in der Dropdownliste aufgeführt. 
    2. Wählen Sie ein **Subnetz** innerhalb des ausgewählten virtuellen Netzwerks aus. 
    3. Wählen Sie die Schaltfläche **Weiter: Tags >** unten auf der Seite aus. 

        ![Erstellen des privaten Endpunkts: Seite „Konfiguration“](./media/private-link-service/create-private-endpoint-configuration-page.png)
10. Erstellen Sie auf der Seite **Tags** beliebige Tags (Namen und Werte), die Sie der privaten Endpunktressource zuordnen möchten. Wählen Sie dann am unteren Rand der Seite die Schaltfläche **Überprüfen und erstellen** aus. 
11. Überprüfen Sie auf der Seite **Überprüfen und erstellen** alle Einstellungen, und wählen Sie dann **Erstellen** aus, um den privaten Endpunkt zu erstellen.
    
    ![Erstellen des privaten Endpunkts: Seite “Überprüfen und erstellen“](./media/private-link-service/create-private-endpoint-review-create-page.png)
12. Vergewissern Sie sich, dass die von Ihnen erstellte private Endpunktverbindung in der Liste der Endpunkte angezeigt wird. In diesem Beispiel wird der private Endpunkt automatisch genehmigt, weil Sie eine Verbindung mit einer Azure-Ressource in Ihrem Verzeichnis hergestellt haben und über ausreichende Berechtigungen verfügen. 

    ![Privater Endpunkt ist erstellt](./media/private-link-service/private-endpoint-created.png)

## <a name="add-a-private-endpoint-using-powershell"></a>Hinzufügen eines privaten Endpunkts mit PowerShell
Im folgenden Beispiel wird gezeigt, wie Azure PowerShell verwendet wird, um eine private Endpunktverbindung zu erstellen. Es wird kein dedizierter Cluster für Sie erstellt. Führen Sie die Schritte in [diesem Artikel](event-hubs-dedicated-cluster-create-portal.md) aus, um einen dedizierten Event Hubs-Cluster zu erstellen. 

```azurepowershell-interactive
# create resource group

$rgName = "<RESOURCE GROUP NAME>"
$vnetlocation = "<VIRTUAL NETWORK LOCATION>"
$vnetName = "<VIRTUAL NETWORK NAME>"
$subnetName = "<SUBNET NAME>"
$namespaceLocation = "<NAMESPACE LOCATION>"
$namespaceName = "<NAMESPACE NAME>"
$peConnectionName = "<PRIVATE ENDPOINT CONNECTION NAME>"

# create virtual network
$virtualNetwork = New-AzVirtualNetwork `
                    -ResourceGroupName $rgName `
                    -Location $vnetlocation `
                    -Name $vnetName `
                    -AddressPrefix 10.0.0.0/16

# create subnet with endpoint network policy disabled
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
                    -Name $subnetName `
                    -AddressPrefix 10.0.0.0/24 `
                    -PrivateEndpointNetworkPoliciesFlag "Disabled" `
                    -VirtualNetwork $virtualNetwork

# update virtual network
$virtualNetwork | Set-AzVirtualNetwork

# create an event hubs namespace in a dedicated cluster
$namespaceResource = New-AzResource -Location $namespaceLocation `
                                    -ResourceName $namespaceName `
                                    -ResourceGroupName $rgName `
                                    -Sku @{name = "Standard"; capacity = 1} `
                                    -Properties @{clusterArmId = "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/clusters/<EVENT HUBS CLUSTER NAME>"} `
                                    -ResourceType "Microsoft.EventHub/namespaces" -ApiVersion "2018-01-01-preview"

# create private endpoint connection
$privateEndpointConnection = New-AzPrivateLinkServiceConnection `
                                -Name $peConnectionName `
                                -PrivateLinkServiceId $namespaceResource.ResourceId `
                                -GroupId "namespace"

# get subnet object that you will use later
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  $rgName -Name $vnetName
$subnet = $virtualNetwork | Select -ExpandProperty subnets `
                                | Where-Object  {$_.Name -eq $subnetName}  
   
# create a private endpoint   
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName $rgName  `
                                -Name $vnetName   `
                                -Location $vnetlocation `
                                -Subnet  $subnet   `
                                -PrivateLinkServiceConnection $privateEndpointConnection

(Get-AzResource -ResourceId $namespaceResource.ResourceId -ExpandProperties).Properties


```

## <a name="manage-private-endpoints-using-azure-portal"></a>Verwalten eines privaten Endpunkts mit dem Azure-Portal

Wenn Sie einen privaten Endpunkt erstellen, muss die Verbindung genehmigt werden. Wenn sich die Ressource, für die Sie einen privaten Endpunkt erstellen, in Ihrem Verzeichnis befindet, können Sie die Verbindungsanforderung genehmigen, sofern Sie über ausreichende Berechtigungen verfügen. Wenn Sie eine Verbindung mit einer Azure-Ressource in einem anderen Verzeichnis herstellen, müssen Sie warten, bis der Besitzer dieser Ressource Ihre Verbindungsanforderung genehmigt hat.

Es gibt vier Möglichkeiten für den Bereitstellungsstatus:

| Dienstaktion | Zustand des privaten Endpunkts des Dienstconsumers | BESCHREIBUNG |
|--|--|--|
| Keine | Ausstehend | Die Verbindung wurde manuell erstellt, und die Genehmigung des Besitzers der Private Link-Ressource steht aus. |
| Genehmigen | Genehmigt | Die Verbindung wurde automatisch oder manuell genehmigt und ist zur Verwendung bereit. |
| Reject | Rejected (Abgelehnt) | Die Verbindung wurde vom Besitzer der Private Link-Ressource abgelehnt. |
| Remove (Entfernen) | Getrennt | Die Verbindung wurde vom Besitzer der Private Link-Ressource entfernt, der private Endpunkt wird informativ und sollte zur Bereinigung gelöscht werden. |
 
###  <a name="approve-reject-or-remove-a-private-endpoint-connection"></a>Genehmigt die Verbindung eines privaten Endpunkts, lehnt sie ab oder entfernt sie.

1. Melden Sie sich beim Azure-Portal an.
2. Geben Sie auf der Suchleiste den Suchbegriff **Event Hubs** ein.
3. Wählen Sie den **Namespace** aus, den Sie verwalten möchten.
4. Wählen Sie die Registerkarte **Netzwerk** aus.
5. Wechseln Sie zu dem entsprechenden Abschnitt unten, der Ihrem gewünschten Vorgang entspricht: Genehmigen, Ablehnen oder Entfernen.

### <a name="approve-a-private-endpoint-connection"></a>Genehmigen einer Verbindung mit einem privaten Endpunkt
1. Sollten ausstehende Verbindungen vorhanden sein, wird in der Liste eine Verbindung mit dem Bereitstellungsstatus **Ausstehend** angezeigt. 
2. Wählen Sie den **privaten Endpunkt** aus, den Sie genehmigen möchten.
3. Wählen Sie die Schaltfläche **Genehmigen** aus.

    ![Image](./media/private-link-service/approve-private-endpoint.png)
4. Geben Sie auf der Seite **Verbindung genehmigen** einen Kommentar ein (optional), und wählen Sie dann **Ja** aus. Wenn Sie **Nein** auswählen, geschieht nichts. 
5. Der Status der Verbindung mit dem privaten Endpunkt sollte in der Liste in **Genehmigt** geändert werden. 

### <a name="reject-a-private-endpoint-connection"></a>Ablehnen einer Verbindung mit einem privaten Endpunkt

1. Falls Sie eine Verbindung mit einem privaten Endpunkt ablehnen möchten, wählen Sie die Verbindung aus, und klicken Sie auf die Schaltfläche **Ablehnen**. Dies funktioniert sowohl für ausstehende Anforderungen als auch für bereits vorhandene Verbindungen.

    ![Image](./media/private-link-service/private-endpoint-reject-button.png)
2. Geben Sie auf der Seite **Verbindung ablehnen** einen Kommentar ein (optional), und wählen Sie dann **Ja** aus. Wenn Sie **Nein** auswählen, geschieht nichts. 
3. Der Status der Verbindung mit dem privaten Endpunkt sollte in der Liste in **Abgelehnt** geändert werden. 

### <a name="remove-a-private-endpoint-connection"></a>Entfernen einer Verbindung mit einem privaten Endpunkt

1. Um eine Verbindung mit einem privaten Endpunkt zu entfernen, wählen Sie sie in der Liste aus, und wählen Sie **Entfernen** auf der Symbolleiste aus.
2. Wählen Sie auf der Seite **Verbindung löschen** **Ja** aus, um das Löschen des privaten Endpunkts zu bestätigen. Wenn Sie **Nein** auswählen, geschieht nichts.
3. Der Status sollte in **Getrennt** geändert werden. Anschließend wird der Endpunkt nicht mehr in der Liste angezeigt.

## <a name="validate-that-the-private-link-connection-works"></a>Überprüfen, ob die Private Link-Verbindung funktioniert

Vergewissern Sie sich, dass die Ressourcen innerhalb des Subnetzes, in dem sich auch die private Endpunktressource befindet, mit Ihrem Event Hubs-Namespace eine Verbindung über eine private IP-Adresse herstellen, und dass die Integration in die private DNS-Zone korrekt ist.

Erstellen Sie zunächst einen virtuellen Computer. Eine entsprechende Anleitung finden Sie unter [Schnellstart: Erstellen eines virtuellen Windows-Computers im Azure-Portal](../virtual-machines/windows/quick-create-portal.md).

Gehen Sie auf der Registerkarte **Netzwerk** wie folgt vor:

1. Geben Sie ein **virtuelles Netzwerk** und ein **Subnetz** an. Sie können ein neues virtuelles Netzwerk erstellen oder ein bereits vorhandenes virtuelles Netzwerk auswählen. Vergewissern Sie sich bei Verwendung eines bereits vorhandenen Netzwerks, dass die Region übereinstimmt.
1. Geben Sie eine **öffentliche IP-Ressource** an.
1. Wählen Sie für **NIC-Netzwerksicherheitsgruppe** die Option **Keine** aus.
1. Wählen Sie für den **Lastenausgleich** **Nein** aus.

Öffnen Sie die Befehlszeile, und führen Sie den folgenden Befehl aus:

```console
nslookup <your-event-hubs-namespace-name>.servicebus.windows.net
```

Wenn Sie den Befehl „ns lookup“ ausführen, um die IP-Adresse eines Event Hubs-Namespace über einen öffentlichen Endpunkt aufzulösen, sieht das Ergebnis wie folgt aus:

```console
c:\ >nslookup <your-event-hubs-namespae-name>.servicebus.windows.net

Non-authoritative answer:
Name:    
Address:  (public IP address)
Aliases:  <your-event-hubs-namespace-name>.servicebus.windows.net
```

Wenn Sie den Befehl „ns lookup“ ausführen, um die IP-Adresse eines Event Hubs-Namespace über einen privaten Endpunkt aufzulösen, sieht das Ergebnis wie folgt aus:

```console
c:\ >nslookup your_event-hubs-namespace-name.servicebus.windows.net

Non-authoritative answer:
Name:    
Address:  10.1.0.5 (private IP address)
Aliases:  <your-event-hub-name>.servicebus.windows.net
```

## <a name="limitations-and-design-considerations"></a>Einschränkungen und Entwurfsaspekte

**Preise:** Preisinformationen finden Sie unter [Azure Private Link – Preise](https://azure.microsoft.com/pricing/details/private-link/).

**Einschränkungen:**  Der private Endpunkt für Azure Event Hubs befindet sich in der Public Preview-Phase. Dieses Feature steht in allen öffentlichen Azure-Regionen zur Verfügung.

**Maximal zulässige Anzahl privater Endpunkte pro Event Hubs-Namespace:** 120.

Weitere Informationen finden Sie unter [Was ist der Azure Private Link-Dienst? – Einschränkungen](../private-link/private-link-service-overview.md#limitations).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Azure Private Link](../private-link/private-link-service-overview.md).
- Weitere Informationen zu [Azure Event Hubs](event-hubs-about.md).
