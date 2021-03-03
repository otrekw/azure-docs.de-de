---
title: 'Schnellstart: Erstellen und Konfigurieren einer Route Server-Instanz mithilfe der Azure-Befehlszeilenschnittstelle'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle eine Route Server-Instanz erstellen und konfigurieren.
services: route-server
author: duongau
ms.service: route-server
ms.topic: quickstart
ms.date: 03/02/2021
ms.author: duau
ms.openlocfilehash: 518baa47fd16d69bf935cd3253f5bebeb413b513
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101680560"
---
# <a name="quickstart-create-and-configure-route-server-using-azure-cli"></a>Schnellstart: Erstellen und Konfigurieren einer Route Server-Instanz mithilfe der Azure-Befehlszeilenschnittstelle 

In diesem Artikel wird beschrieben, wie Sie Azure Route Server mithilfe der Azure-Befehlszeilenschnittstelle für das Peering mit virtuellen Netzwerkgeräten (Network Virtual Appliances, NVAs) in Ihrem virtuellen Netzwerk konfigurieren. Azure Route Server lernt Routen vom NVA und programmiert sie für die virtuellen Computer im virtuellen Netzwerk. Außerdem kündigt Azure Route Server die virtuellen Netzwerkrouten für das NVA an. Weitere Informationen finden Sie unter [Azure Route Server](overview.md).

> [!IMPORTANT]
> Azure Route Server (Vorschau) befindet sich derzeit in der öffentlichen Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

##  <a name="prerequisites"></a>Voraussetzungen 

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
* Stellen Sie sicher, dass Sie über die aktuelle Azure-Befehlszeilenschnittstelle verfügen, oder verwenden Sie Azure Cloud Shell im Portal. 
* Beachten Sie die [Diensteinschränkungen für Azure Route Server](route-server-faq.md#limitations). 

##  <a name="create-a-route-server"></a>Erstellen einer Route Server-Instanz 

###  <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus. 

[!INCLUDE [sign in](../../includes/expressroute-cloud-shell-connect.md)] 

### <a name="create-a-resource-group-and-virtual-network"></a>Erstellen einer Ressourcengruppe und eines virtuellen Netzwerks 

Bevor Sie eine Azure Route Server-Instanz erstellen können, benötigen Sie ein virtuelles Netzwerk, um die Bereitstellung zu hosten. Verwenden Sie den folgenden Befehl, um eine Ressourcengruppe und ein virtuelles Netzwerk zu erstellen. Wenn Sie bereits über ein virtuelles Netzwerk verfügen, können Sie mit dem nächsten Abschnitt fortfahren.

```azurecli-interactive
az group create -n “RouteServerRG” -l “westus” 
az network vnet create -g “RouteServerRG” -n “myVirtualNetwork” --address-prefix “10.0.0.0/16” 
``` 

### <a name="add-a-subnet"></a>Hinzufügen eines Subnetzes 

1. Fügen Sie ein Subnetz mit dem Namen *RouteServerSubnet* hinzu, in dem Sie die Azure Route Server-Instanz bereitstellen. Dieses Subnetz ist nur ein dediziertes Subnetz für Azure Route Server. Das RouteServerSubnet muss /27 oder ein kürzeres Präfix (z. B. /26 oder /25) sein. Andernfalls erhalten Sie eine Fehlermeldung, wenn Sie die Azure Route Server-Instanz hinzufügen.

    ```azurecli-interactive 
    az network vnet subnet create -g “RouteServerRG” --vnet-name “myVirtualNetwork” --name “RouteServerSubnet” --address-prefix “10.0.0.0/24”  
    ``` 

1. Rufen Sie die RouteServerSubnet-ID ab. Verwenden Sie den folgenden Befehl, um die Ressourcen-ID aller Subnetze im virtuellen Netzwerk anzuzeigen: 

    ```azurecli-interactive 
    subnet_id = $(az network vnet subnet show -n “RouteServerSubnet” --vnet-name “myVirtualNetwork” -g “RouteServerRG” --query id -o tsv) 
    ``` 

Die RouteServerSubnet-ID sieht wie folgt aus: 

`/subscriptions/<subscriptionID>/resourceGroups/RouteServerRG/providers/Microsoft.Network/virtualNetworks/myVirtualNetwork/subnets/RouteServerSubnet`

## <a name="create-the-route-server"></a>Erstellen der Route Server-Instanz 

Erstellen Sie die Route Server-Instanz mit dem folgenden Befehl: 

```azurecli-interactive
az network routeserver create -n “myRouteServer” -g “RouteServerRG” --hosted-subnet $subnet_id  
``` 

Der Speicherort muss dem Speicherort Ihres virtuellen Netzwerks entsprechen. Bei „HostedSubnet“ handelt es sich um die RouteServerSubnet-ID, die Sie im vorherigen Abschnitt abgerufen haben. 

## <a name="create-peering-with-an-nva"></a>Erstellen eines Peerings mit einem NVA 

Verwenden Sie den folgenden Befehl, um das Peering zwischen der Route Server-Instanz und dem NVA einzurichten: 

```azurecli-interactive 

az network routeserver peering create --routeserver-name “myRouteServer” -g “RouteServerRG” --peer-ip “nva_ip” --peer-asn “nva_asn” -n “NVA1_name” 

``` 

„nva_ip“ ist die dem NVA zugewiesene IP-Adresse des virtuellen Netzwerks. „nva_asn“ ist die im NVA konfigurierte Autonomous System Number (ASN). Die ASN kann eine beliebige 16-Bit-Zahl sein, die nicht im Bereich von 65515-65520 liegt. Die ASNs in diesem Bereich sind von Microsoft reserviert. 

Wenn Sie das Peering mit einem anderen NVA oder einer anderen Instanz desselben NVAs zwecks Redundanz einrichten möchten, verwenden Sie den folgenden Befehl:

```azurecli-interactive 

az network routeserver peering create --routeserver-name “myRouteServer” -g “RouteServerRG” --peer-ip “nva_ip” --peer-asn “nva_asn” -n “NVA2_name” 
``` 

## <a name="complete-the-configuration-on-the-nva"></a>Abschließen der Konfiguration auf dem NVA 

Um die Konfiguration auf dem NVA abzuschließen und die BGP-Sitzungen zu aktivieren, benötigen Sie die IP-Adresse und die ASN von Azure Route Server. Sie können diese Informationen mit dem folgenden Befehl abrufen: 

```azurecli-interactive 
az network routeserver show -g “RouteServerRG” -n “myRouteServer” 
``` 

Die Ausgabe enthält die folgenden Informationen. 

```azurecli-interactive 
RouteServerAsn  : 65515 

RouteServerIps  : {10.5.10.4, 10.5.10.5}  "virtualRouterAsn": 65515, 

  "virtualRouterIps": [ 

    "10.0.0.4", 

    "10.0.0.5" 

  ], 

``` 

## <a name="configure-route-exchange"></a>Konfigurieren des Routenaustauschs 

Wenn Sie über ein ExpressRoute-Gateway und ein Azure VPN-Gateway in demselben VNet verfügen und diese Routen austauschen sollen, können Sie den Routenaustausch auf der Azure Route Server-Instanz aktivieren.

> [!IMPORTANT]
> Stellen Sie bei Bereitstellungen ohne bestehende Infrastruktur sicher, dass Sie zuerst das Azure VPN-Gateway erstellen, bevor Sie die Azure Route Server-Instanz erstellen. Andernfalls schlägt die Bereitstellung von Azure VPN Gateway fehl.
> 

1. Verwenden Sie den folgenden Befehl, um den Routenaustausch zwischen Azure Route Server und den Gateways zu aktivieren:

```azurecli-interactive 
az network routeserver update -g “RouteServerRG” -n “myRouteServer” --allow-b2b-traffic true 

``` 

2. Verwenden Sie den folgenden Befehl, um den Routenaustausch zwischen Azure Route Server und den Gateways zu deaktivieren:

```azurecli-interactive
az network routeserver update -g “RouteServerRG” -n “myRouteServer” --allow-b2b-traffic false 
``` 

## <a name="troubleshooting"></a>Problembehandlung 

Mit dem folgenden Befehl können Sie die von Azure Route Server angekündigten und empfangenen Routen anzeigen:

```azurecli-interactive 
az network routeserver peering list-advertised-routes -g RouteServerRG --vrouter-name myRouteServer -n NVA1_name 
az network routeserver peering list-learned-routes -g RouteServerRG --vrouter-name myRouteServer -n NVA1_name 
``` 

## <a name="clean-up"></a>Bereinigung 

Wenn Sie die Azure Route Server-Instanz nicht mehr benötigen, verwenden Sie die folgenden Befehle, um das BGP-Peering und dann die Route Server-Instanz zu entfernen. 

1. Entfernen Sie das BGP-Peering zwischen Azure Route Server und einem NVA mit dem folgenden Befehl:

```azurecli-interactive
az network routeserver peering delete --routeserver-name “myRouteServer” -g “RouteServerRG” -n “NVA2_name” 
``` 

2. Entfernen Sie die Azure Route Server-Instanz mit dem folgenden Befehl: 

```azurecli-interactive 
az network routeserver delete -n “myRouteServer” -g “RouteServerRG” 
``` 

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich nach dem Erstellen der Azure Route Server-Instanz darüber, wie Azure Route Server mit ExpressRoute- und VPN-Gateways interagiert: 

> [!div class="nextstepaction"]
> [Unterstützung von Azure ExpressRoute und Azure VPN](expressroute-vpn-support.md)
 
