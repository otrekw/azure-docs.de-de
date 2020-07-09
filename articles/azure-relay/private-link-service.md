---
title: Integrieren von Azure Relay in den Azure Private Link-Dienst
description: Es wird beschrieben, wie Sie Azure Relay in den Azure Private Link-Dienst integrieren.
ms.date: 06/23/2020
ms.topic: article
ms.openlocfilehash: a113e52b892a25fd2b12a18d73df443d9a9866f2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85317323"
---
# <a name="integrate-azure-relay-with-azure-private-link-preview"></a>Integrieren von Azure Relay in Azure Private Link (Vorschau)
Mit Azure Private Link können Sie über einen **privaten Endpunkt** in Ihrem virtuellen Netzwerk auf Azure-Dienste wie Azure Relay, Azure Service Bus, Azure Event Hubs, Azure Storage und Azure Cosmos DB sowie auf in Azure gehostete Kunden-/Partnerdienste zugreifen. Weitere Informationen finden Sie unter [Was ist Azure Private Link? (Vorschau)](../private-link/private-link-overview.md).

Ein **privater Endpunkt** ist eine Netzwerkschnittstelle, mit der Ihre Workloads, die in einem virtuellen Netzwerk ausgeführt werden, eine private und sichere Verbindung mit einem Dienst herstellen können, der über eine **Private Link-Ressource** verfügt (z. B. über einen Relaynamespace). Der private Endpunkt verwendet eine private IP-Adresse aus Ihrem VNET und bindet den Dienst dadurch in Ihr VNET ein. Der gesamte für den Dienst bestimmte Datenverkehr kann über den privaten Endpunkt geleitet werden. Es sind also keine Gateways, NAT-Geräte, ExpressRoute-Verbindungen, VPN-Verbindungen oder öffentlichen IP-Adressen erforderlich. Der Datenverkehr zwischen Ihrem virtuellen Netzwerk und dem Dienst wird über das Microsoft-Backbonenetzwerk übertragen und dadurch vom öffentlichen Internet isoliert. Sie können eine gewünschte Granularitätsebene für die Zugriffssteuerung festlegen, indem Sie Verbindungen mit bestimmten Azure Relay-Namespaces zulassen. 


> [!IMPORTANT]
> Diese Funktion steht derzeit als **Vorschau** zur Verfügung. 
>
> Wir unterstützen derzeit Private Link-Verbindungen für Absenderclients. 


## <a name="add-a-private-endpoint-using-azure-portal"></a>Hinzufügen eines privaten Endpunkts über das Azure-Portal

### <a name="prerequisites"></a>Voraussetzungen
Um einen Azure Relay-Namespace in Azure Private Link (Vorschau) zu integrieren, benötigen Sie die folgenden Entitäten oder Berechtigungen:

- Einen Azure Relay-Namespace
- Ein virtuelles Azure-Netzwerk
- Ein Subnetz in dem virtuellen Netzwerk
- Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ für das virtuelle Netzwerk.

Der private Endpunkt und das virtuelle Netzwerk müssen sich in der gleichen Region befinden. Wenn Sie über das Portal eine Region für den privaten Endpunkt auswählen, wird automatisch nach virtuellen Netzwerken in dieser Region gefiltert. Der Namespace kann sich in einer anderen Region befinden.

Der private Endpunkt verwendet eine private IP-Adresse in Ihrem virtuellen Netzwerk.

### <a name="steps"></a>Schritte
Eine Schritt-für-Schritt-Anleitung zum Erstellen eines neuen Azure Relay-Namespace und seiner Entitäten finden Sie unter [Erstellen eines Azure Relay-Namespace mithilfe des Azure-Portals](relay-create-namespace-portal.md).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Geben Sie in der Suchleiste den Suchbegriff **Relays** ein.
3. Wählen Sie in der Liste den **Namespace** aus, dem Sie einen privaten Endpunkt hinzufügen möchten.
4. Wählen Sie die Registerkarte **Netzwerk** unter **Einstellungen** aus.
5. Wählen Sie im oberen Seitenbereich die Registerkarte **Private Endpunktverbindungen (Vorschau)** aus.
6. Wählen Sie im oberen Seitenbereich die Schaltfläche **+ Privater Endpunkt** aus.

    ![Hinzufügen einer Schaltfläche für den privaten Endpunkt](./media/private-link-service/add-private-endpoint-button.png)
7. Führen Sie auf der Seite **Grundlagen** die folgenden Schritte aus: 
    1. Wählen Sie das **Azure-Abonnement** aus, in dem Sie den privaten Endpunkt erstellen möchten. 
    2. Wählen Sie die **Ressourcengruppe** für die private Endpunktressource aus.
    3. Geben Sie einen **Namen** für den privaten Endpunkt ein. 
    5. Wählen Sie eine **Region** für den privaten Endpunkt aus. Ihr privater Endpunkt muss sich in derselben Region wie Ihr virtuelles Netzwerk befinden, kann aber in einer anderen Region als der Azure Relay-Namespace enthalten sein, mit dem Sie eine Verbindung herstellen. 
    6. Klicken Sie auf **Weiter: Ressource >** unten auf der Seite aus.

        ![Erstellen des privaten Endpunkts: Seite „Grundlagen“](./media/private-link-service/create-private-endpoint-basics-page.png)
8. Führen Sie auf der Seite **Ressource** die folgenden Schritte aus:
    1. Führen Sie die folgenden Schritte aus, wenn Sie als Verbindungsmethode **Verbindung mit einer Azure-Ressource in meinem Verzeichnis herstellen** auswählen, über Zugriff vom Typ „Besitzer“ oder „Mitwirkender“ auf den Namespace verfügen und sich dieser Namespace in demselben Verzeichnis wie der private Endpunkt befindet: 
        1. Wählen Sie das **Azure-Abonnement** aus, in dem Ihr **Azure Relay-Namespace** enthalten ist. 
        2. Wählen Sie unter **Ressourcentyp** **Microsoft.Relay/namespaces** als **Ressourcentyp** aus.
        3. Wählen Sie als **Ressource** in der Dropdownliste einen Relaynamespace aus. 
        4. Vergewissern Sie sich, dass die **Unterressource des Ziels** auf **Namespace** festgelegt ist.
        5. Klicken Sie auf **Weiter: Konfiguration >** unten auf der Seite aus. 
        
            ![Erstellen des privaten Endpunkts: Seite „Ressourcen“](./media/private-link-service/create-private-endpoint-resource-page.png)    
    2. Führen Sie die folgenden Schritte aus, wenn Sie **Stellen Sie über eine Ressourcen-ID oder einen Alias eine Verbindung mit einer Azure-Ressource her** auswählen, weil sich der Namespace nicht in demselben Verzeichnis wie der private Endpunkt befindet:
        1. Geben Sie die **Ressourcen-ID** oder den **Alias** ein. Dabei kann es sich um die Ressourcen-ID oder den Alias handeln, den jemand für Sie freigegeben hat. Die einfachste Möglichkeit zum Abrufen dieser Ressourcen-ID besteht darin, im Azure-Portal zum Azure Relay-Namespace zu navigieren und den Teil des URI ab `/subscriptions/` zu kopieren. Dies ist ein Beispiel hierfür: `/subscriptions/000000000-0000-0000-0000-000000000000000/resourceGroups/myresourcegroup/providers/Microsoft.Relay/namespaces/myrelaynamespace.` 
        2. Geben Sie für **Zielunterressource** **Namespace** ein. Dabei handelt es sich um den Unterressourcentyp, auf den der private Endpunkt zugreifen kann.
        3. (Optional) Geben Sie eine **Anforderungsnachricht** ein. Der Ressourcenbesitzer sieht diese Nachricht beim Verwalten der Verbindung mit dem privaten Endpunkt.
        4. Wählen Sie anschließend **Weiter: Konfiguration >** unten auf der Seite aus.

            ![Erstellen des privaten Endpunkts: Verbinden mithilfe der Ressourcen-ID](./media/private-link-service/connect-resource-id.png)
9. Wählen Sie auf der Seite **Konfiguration** das Subnetz in einem virtuellen Netzwerk aus, in dem Sie den privaten Endpunkt bereitstellen möchten. 
    1. Wählen Sie ein **virtuelles Netzwerk** aus. Nur virtuelle Netzwerke im aktuell ausgewählten Abonnement und am aktuell ausgewählten Standort werden in der Dropdownliste aufgeführt. 
    2. Wählen Sie ein **Subnetz** innerhalb des ausgewählten virtuellen Netzwerks aus. 
    3. Wählen Sie **In private DNS-Zone integrieren** aus, wenn Sie Ihren privaten Endpunkt in eine private DNS-Zone integrieren möchten. 
    
        Für die Herstellung einer privaten Verbindung mit Ihrem privaten Endpunkt benötigen Sie einen DNS-Eintrag. Es wird empfohlen, den privaten Endpunkt in eine **private DNS-Zone** zu integrieren. Sie können auch Ihre eigenen DNS-Server verwenden oder DNS-Einträge mithilfe der Hostdateien auf Ihren virtuellen Computern erstellen. Weitere Informationen finden Sie unter [DNS-Konfiguration für private Azure-Endpunkte](../private-link/private-endpoint-dns.md). In diesem Beispiel ist die Option **In private DNS-Zone integrieren** ausgewählt, und es wird eine private DNS-Zone für Sie erstellt. 
    3. Klicken Sie auf **Weiter: Tags >** unten auf der Seite aus. 

        ![Erstellen des privaten Endpunkts: Seite „Konfiguration“](./media/private-link-service/create-private-endpoint-configuration-page.png)
10. Erstellen Sie auf der Seite **Tags** beliebige Tags (Namen und Werte), die Sie dem privaten Endpunkt und der privaten DNS-Zone zuordnen möchten (wenn Sie diese Option aktiviert haben). Wählen Sie dann am unteren Rand der Seite die Schaltfläche **Überprüfen und erstellen** aus. 
11. Überprüfen Sie auf der Seite **Überprüfen und erstellen** alle Einstellungen, und wählen Sie dann **Erstellen** aus, um den privaten Endpunkt zu erstellen.
    
    ![Erstellen des privaten Endpunkts: Seite “Überprüfen und erstellen“](./media/private-link-service/create-private-endpoint-review-create-page.png)
12. Auf der Seite **Privater Endpunkt** wird der Status der Verbindung mit dem privaten Endpunkt angezeigt. Wenn Sie Besitzer des Relaynamespace sind oder Verwaltungszugriff darauf besitzen und die Option **Hiermit wird eine Verbindung mit einer Azure-Ressource im eigenen Verzeichnis hergestellt** als **Verbindungsmethode** ausgewählt haben, sollte die Endpunktverbindung **automatisch genehmigt** sein. Wenn sie sich im Status **Ausstehend** befindet, lesen Sie den Abschnitt [Verwalten eines privaten Endpunkts mit dem Azure-Portal](#manage-private-endpoints-using-azure-portal).

    ![Seite „Privater Endpunkt“](./media/private-link-service/private-endpoint-page.png)
13. Navigieren Sie zurück zur Seite **Netzwerk** des **Namespace**, und wechseln Sie zur Registerkarte **Private Endpunktverbindungen (Vorschau)** . Der private Endpunkt, den Sie erstellt haben, sollte angezeigt werden. 

    ![Privater Endpunkt ist erstellt](./media/private-link-service/private-endpoint-created.png)

## <a name="add-a-private-endpoint-using-powershell"></a>Hinzufügen eines privaten Endpunkts mit PowerShell
Im folgenden Beispiel wird gezeigt, wie Azure PowerShell verwendet wird, um eine private Endpunktverbindung mit einem Azure Relay-Namespace herzustellen.

Der private Endpunkt und das virtuelle Netzwerk müssen sich in der gleichen Region befinden. Der Azure Relay-Namespace kann sich in einer anderen Region befinden. Außerdem verwendet der private Endpunkt eine private IP-Adresse in Ihrem virtuellen Netzwerk.

```azurepowershell-interactive

$rgName = "<RESOURCE GROUP NAME>"
$vnetlocation = "<VNET LOCATION>"
$vnetName = "<VIRTUAL NETWORK NAME>"
$subnetName = "<SUBNET NAME>"
$namespaceLocation = "<NAMESPACE LOCATION>"
$namespaceName = "<NAMESPACE NAME>"
$peConnectionName = "<PRIVATE ENDPOINT CONNECTION NAME>"

# create resource group
az group create -l $vnetLocation -n $rgName

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

# create a relay namespace
$namespaceResource = New-AzResource -Location $namespaceLocation -ResourceName $namespaceName -ResourceGroupName $rgName -Properties @{} -ResourceType "Microsoft.Relay/namespaces" 

# create a private link service connection
$privateEndpointConnection = New-AzPrivateLinkServiceConnection `
                                -Name $peConnectionName `
                                -PrivateLinkServiceId $namespaceResource.ResourceId `
                                -GroupId "namespace"

# get subnet object that you will use in the next step                                
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  $rgName -Name $vnetName
$subnet = $virtualNetwork | Select -ExpandProperty subnets `
                                | Where-Object  {$_.Name -eq $subnetName}  
   
# now, create private endpoint   
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName $rgName  `
                                -Name $vnetName   `
                                -Location $vnetlocation `
                                -Subnet  $subnet   `
                                -PrivateLinkServiceConnection $privateEndpointConnection

(Get-AzResource -ResourceId $namespaceResource.ResourceId -ExpandProperties).Properties


```

## <a name="manage-private-endpoints-using-azure-portal"></a>Verwalten eines privaten Endpunkts mit dem Azure-Portal

Wenn Sie einen privaten Endpunkt erstellen, muss die Verbindung genehmigt werden. Wenn sich die Ressource (Relaynamespace), für die Sie einen privaten Endpunkt erstellen, in Ihrem Verzeichnis befindet, können Sie die Verbindungsanforderung genehmigen, sofern Sie über Verwaltungsberechtigungen für den Relaynamespace verfügen. Wenn Sie eine Verbindung mit einem Relaynamespace herstellen, für den Sie keinen Verwaltungszugriff besitzen, müssen Sie warten, bis der Besitzer dieser Ressource Ihre Verbindungsanforderung genehmigt hat.

Es gibt vier Möglichkeiten für den Bereitstellungsstatus:

| Dienstaktion | Zustand des privaten Endpunkts des Dienstconsumers | BESCHREIBUNG |
|--|--|--|
| Keine | Ausstehend | Die Verbindung wurde manuell erstellt, und die Genehmigung des Besitzers des Azure Relay-Namespace steht aus. |
| Genehmigen | Genehmigt | Die Verbindung wurde automatisch oder manuell genehmigt und ist zur Verwendung bereit. |
| Reject | Rejected (Abgelehnt) | Die Verbindung wurde vom Besitzer des Azure Relay-Namespace abgelehnt. |
| Remove (Entfernen) | Getrennt | Die Verbindung wurde vom Besitzer des Azure Relay-Namespace entfernt, und der private Endpunkt wird informativ und sollte zur Bereinigung gelöscht werden. |
 
###  <a name="approve-reject-or-remove-a-private-endpoint-connection"></a>Genehmigt die Verbindung eines privaten Endpunkts, lehnt sie ab oder entfernt sie.

1. Melden Sie sich beim Azure-Portal an.
1. Geben Sie auf der Suchleiste den Suchbegriff **Relay** ein.
1. Wählen Sie den **Namespace** aus, den Sie verwalten möchten.
1. Wählen Sie die Registerkarte **Netzwerk** aus.
5. Wechseln Sie zu dem entsprechenden Abschnitt unten, der Ihrem gewünschten Vorgang entspricht: Genehmigen, Ablehnen oder Entfernen. 

### <a name="approve-a-private-endpoint-connection"></a>Genehmigen einer Verbindung mit einem privaten Endpunkt

1. Sollten ausstehende Verbindungen vorhanden sein, wird in der Liste eine Verbindung mit dem Bereitstellungsstatus **Ausstehend** angezeigt. 
2. Wählen Sie den **privaten Endpunkt** aus, den Sie genehmigen möchten.
3. Wählen Sie die Schaltfläche **Genehmigen** aus.

    ![Genehmigen des privaten Endpunkts](./media/private-link-service/private-endpoint-approve.png)
4. Geben Sie auf der Seite **Verbindung genehmigen** einen optionalen **Kommentar** ein, und wählen Sie dann **Ja** aus. Wenn Sie **Nein** auswählen, geschieht nichts. 

    ![Seite „Verbindung genehmigen“](./media/private-link-service/approve-connection-page.png)
5. Der Status der Verbindung sollte in der Liste in **Genehmigt** geändert werden.

### <a name="reject-a-private-endpoint-connection"></a>Ablehnen einer Verbindung mit einem privaten Endpunkt

1. Falls Sie eine Verbindung mit einem privaten Endpunkt ablehnen möchten, wählen Sie die Endpunktverbindung aus, und klicken Sie auf die Schaltfläche **Ablehnen**. Dies funktioniert sowohl für ausstehende Anforderungen als auch für bereits vorhandene Verbindungen, die vorher genehmigt wurden.

    ![Schaltfläche „Ablehnen“](./media/private-link-service/private-endpoint-reject.png)
2. Geben Sie auf der Seite **Verbindung ablehnen** einen optionalen Kommentar ein, und wählen Sie dann **Ja** aus. Wenn Sie **Nein** auswählen, geschieht nichts. 

    ![Seite „Verbindung ablehnen“](./media/private-link-service/reject-connection-page.png)
3. Der Status der Verbindung sollte in der Liste in **Abgelehnt** geändert werden.


### <a name="remove-a-private-endpoint-connection"></a>Entfernen einer Verbindung mit einem privaten Endpunkt

1. Um eine Verbindung mit einem privaten Endpunkt zu entfernen, wählen Sie sie in der Liste aus, und wählen Sie **Entfernen** auf der Symbolleiste aus. 

    ![Schaltfläche „Entfernen“](./media/private-link-service/remove-endpoint.png)
2. Wählen Sie auf der Seite **Verbindung löschen** **Ja** aus, um das Löschen des privaten Endpunkts zu bestätigen. Wenn Sie **Nein** auswählen, geschieht nichts. 

    ![Seite „Verbindung löschen“](./media/private-link-service/delete-connection-page.png)
3. Der Status sollte in **Getrennt** geändert werden. Anschließend wird der Endpunkt nicht mehr in der Liste angezeigt. 

## <a name="validate-that-the-private-link-connection-works"></a>Überprüfen, ob die Private Link-Verbindung funktioniert
Sie sollten überprüfen, ob Ressourcen innerhalb desselben Subnetzes des privaten Endpunkts über dessen private IP-Adresse eine Verbindung mit dem Azure Relay-Namespace herstellen.

Erstellen Sie für diesen Test einen virtuellen Computer. Eine entsprechende Anleitung finden Sie unter [Erstellen eines virtuellen Windows-Computers im Azure-Portal](../virtual-machines/windows/quick-create-portal.md).

Gehen Sie auf der Registerkarte **Netzwerk** wie folgt vor: 

1. Geben Sie ein **virtuelles Netzwerk** und ein **Subnetz** an. Sie müssen das virtuelle Netzwerk auswählen, in dem Sie den privaten Endpunkt bereitgestellt haben.
2. Geben Sie eine **öffentliche IP-Ressource** an.
3. Wählen Sie für **NIC-Netzwerksicherheitsgruppe** die Option **Keine** aus.
4. Wählen Sie für den **Lastenausgleich** **Nein** aus.

Stellen Sie eine Verbindung mit der VM her, öffnen Sie die Befehlszeile, und führen Sie den folgenden Befehl aus:

```console
nslookup <your-relay-namespace-name>.servicebus.windows.net
```

Das Ergebnis sollte in etwa wie folgt aussehen. 

```console
Non-authoritative answer:
Name:    <namespace-name>.privatelink.servicebus.windows.net
Address:  10.0.0.4 (private IP address associated with the private endpoint)
Aliases:  <namespace-name>.servicebus.windows.net
```

## <a name="limitations-and-design-considerations"></a>Einschränkungen und Entwurfsaspekte

### <a name="design-considerations"></a>Überlegungen zum Entwurf
- Der private Endpunkt für Azure Relay befindet sich in der **öffentlichen Vorschauphase**. 
- Preisinformationen finden Sie unter [Azure Private Link – Preise](https://azure.microsoft.com/pricing/details/private-link/).

### <a name="limitations"></a>Einschränkungen 
- Maximal zulässige Anzahl privater Endpunkte pro Azure Relay-Namespace: 64.
- Maximal zulässige Anzahl von Azure Relay-Namespaces mit privaten Endpunkten pro Abonnement: 64.
- Regeln für Netzwerksicherheitsgruppen (NSGs) und benutzerdefinierte Routen gelten nicht für den privaten Endpunkt. Weitere Informationen hierzu finden Sie unter [Azure Private Link-Dienst: Einschränkungen](../private-link/private-link-service-overview.md#limitations).

## <a name="next-steps"></a>Nächste Schritte

- [Was ist der Azure Private Link-Dienst?](../private-link/private-link-service-overview.md)
- Weitere Informationen zu [Azure Relay](relay-what-is-it.md)
