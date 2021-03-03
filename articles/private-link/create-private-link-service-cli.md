---
title: Erstellen eines Azure Private Link-Diensts mithilfe der Azure CLI
description: Informationen zum Erstellen eines Azure Private Link-Diensts mithilfe der Azure CLI
services: private-link
author: asudbring
ms.service: private-link
ms.topic: quickstart
ms.date: 01/22/2021
ms.author: allensu
ms.openlocfilehash: 27ce0b2646b6c380e86b377d3dba287f7791794e
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2021
ms.locfileid: "100653685"
---
# <a name="create-a-private-link-service-using-azure-cli"></a>Erstellen eines Private Link-Diensts mithilfe der Azure CLI

Führen Sie die ersten Schritte zum Erstellen eines Private Link-Diensts für Ihren Dienst aus.  Gewähren Sie für Private Link den Zugriff auf Ihren Dienst oder Ihre Ressource hinter Azure Load Balancer Standard.  Benutzer Ihres Diensts verfügen über privaten Zugriff aus ihrem virtuellen Netzwerk.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)] 

- Für diesen Schnellstart ist mindestens Version 2.0.28 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe:

* Name: **CreatePrivLinkService-rg** 
* Standort: **eastus**

```azurecli-interactive
  az group create \
    --name CreatePrivLinkService-rg \
    --location eastus2

```

## <a name="create-an-internal-load-balancer"></a>Erstellen eines internen Load Balancers

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und eine interne Azure Load Balancer-Instanz.

### <a name="virtual-network"></a>Virtuelles Netzwerk

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des Lastenausgleichs, der auf Ihren Private Link-Dienst zugreift.

Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) ein virtuelles Netzwerk:

* Name: **myVNet**
* Adresspräfix: **10.1.0.0/16**
* Subnetzname: **mySubnet**
* Subnetzpräfix: **10.1.0.0/24**
* Ressourcengruppe: **CreatePrivLinkService-rg**
* Standort: **eastus2**
* Deaktivieren Sie die Netzwerkrichtlinie für den Private Link-Dienst im Subnetz.

```azurecli-interactive
  az network vnet create \
    --resource-group CreatePrivLinkService-rg \
    --location eastus2 \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefixes 10.1.0.0/24

```

Aktualisieren Sie das Subnetz mit [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update), um Netzwerkrichtlinien für den Privat Link-Dienst zu deaktivieren.

```azurecli-interactive
az network vnet subnet update \
    --name mySubnet \
    --resource-group CreatePrivLinkService-rg \
    --vnet-name myVNet \
    --disable-private-link-service-network-policies true
```

### <a name="create-standard-load-balancer"></a>Erstellen eines Standard-Lastenausgleichs

In diesem Abschnitt erfahren Sie, wie Sie die folgenden Komponenten des Lastenausgleichs erstellen und konfigurieren:

  * Front-End-IP-Pool, der den eingehenden Netzwerkdatenverkehr für den Lastenausgleich empfängt
  * Back-End-IP-Pool, an den der Front-End-Pool den Netzwerkdatenverkehr sendet, für den ein Lastenausgleich durchgeführt wurde
  * Integritätstest zum Ermitteln der Integrität der Back-End-VM-Instanzen
  * Lastenausgleichsregel, mit der definiert wird, wie Datenverkehr auf die virtuellen Computer verteilt werden soll

### <a name="create-the-load-balancer-resource"></a>Erstellen der Lastenausgleichsressource

Erstellen Sie mit [az network lb create](/cli/azure/network/lb#az-network-lb-create) einen öffentlichen Lastenausgleich:

* Name: **myLoadBalancer**
* Front-End-Pool namens **myFrontEnd**
* Back-End-Pool namens **myBackEndPool**
* Dem virtuellen Netzwerk **myVNet** zugeordnet
* Dem Back-End-Subnetz **mySubnet** zugeordnet

```azurecli-interactive
  az network lb create \
    --resource-group CreatePrivLinkService-rg \
    --name myLoadBalancer \
    --sku Standard \
    --vnet-name myVnet \
    --subnet mySubnet \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool
```

### <a name="create-the-health-probe"></a>Erstellen des Integritätstests

Von einem Integritätstest werden alle VM-Instanzen überprüft, um sicherzustellen, dass sie Netzwerkdatenverkehr senden können. 

Ist der Test bei einem virtuellen Computer nicht erfolgreich, wird er aus dem Lastenausgleich entfernt. Nach Behebung des Fehlers wird der virtuelle Computer dem Lastenausgleich wieder hinzugefügt.

Erstellen Sie mit [az network lb probe create](/cli/azure/network/lb/probe#az-network-lb-probe-create) einen Integritätstest:

* Überwacht die Integrität der virtuellen Computer
* Name: **myHealthProbe**
* Protokoll: **TCP**
* Überwachter Port: **80**

```azurecli-interactive
  az network lb probe create \
    --resource-group CreatePrivLinkService-rg \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-the-load-balancer-rule"></a>Erstellen der Lastenausgleichsregel

Durch eine Lastenausgleichsregel wird Folgendes definiert:

* Front-End-IP-Konfiguration für den eingehenden Datenverkehr.
* Back-End-IP-Pool zum Empfangen des Datenverkehrs.
* Erforderliche Quell- und Zielports. 

Erstellen Sie mit [az network lb rule create](/cli/azure/network/lb/rule#az-network-lb-rule-create) eine Lastenausgleichsregel:

* Name: **myHTTPRule**
* Lauscht am **Port 80** im Front-End-Pool **myFrontEnd**
* Sendet Netzwerkdatenverkehr, für den ein Lastenausgleich durchgeführt wurde, an den Back-End-Adresspool **myBackEndPool** unter Verwendung von **Port 80** 
* Verwendet den Integritätstest **myHealthProbe**
* Protokoll: **TCP**
* Leerlaufzeitüberschreitung von **15 Minuten**.
* Aktivieren Sie die TCP-Zurücksetzung.

```azurecli-interactive
  az network lb rule create \
    --resource-group CreatePrivLinkService-rg \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe \
    --idle-timeout 15 \
    --enable-tcp-reset true
```

## <a name="create-a-private-link-service"></a>Erstellen eines Private Link-Diensts

In diesem Abschnitt erstellen Sie eine Instanz des Private Link-Diensts, für die der im vorherigen Schritt erstellte Azure Load Balancer verwendet wird.

Erstellen Sie mit [az network private-link-service create](/cli/azure/network/private-link-service#az-network-private-link-service-create) einen Private Link-Dienst unter Verwendung der Front-End-IP-Konfiguration von Load Balancer Standard:

* Name: **myPrivateLinkService**
* Virtuelles Netzwerk: **myVNet**
* Dem Load Balancer Standard **myLoadBalancer** und der Front-End-Konfiguration **myFrontEnd** zugeordnet
* Standort: **eastus2**
 
```azurecli-interactive
az network private-link-service create \
    --resource-group CreatePrivLinkService-rg \
    --name myPrivateLinkService \
    --vnet-name myVNet \
    --subnet mySubnet \
    --lb-name myLoadBalancer \
    --lb-frontend-ip-configs myFrontEnd \
    --location eastus2
```

Ihr Private Link-Dienst wird erstellt und kann Datenverkehr empfangen. Konfigurieren Sie Ihre Anwendung hinter Ihrer Instanz von Load Balancer Standard, falls Sie den Datenverkehrsfluss anzeigen möchten.


## <a name="create-private-endpoint"></a>Erstellen eines privaten Endpunkts

In diesem Abschnitt ordnen Sie den Private Link-Dienst einem privaten Endpunkt zu. Ein virtuelles Netzwerk enthält den privaten Endpunkt für den Private Link-Dienst. In diesem virtuellen Netzwerk sind die Ressourcen enthalten, die auf Ihren Private Link-Dienst zugreifen.

### <a name="create-private-endpoint-virtual-network"></a>Erstellen eines virtuellen Netzwerks des privaten Endpunkts

Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) ein virtuelles Netzwerk:

* Name: **myVNetPE**
* Adresspräfix: **11.1.0.0/16**
* Subnetzname: **mySubnetPE**
* Subnetzpräfix: **11.1.0.0/24**
* Ressourcengruppe: **CreatePrivLinkService-rg**
* Standort: **eastus2**

```azurecli-interactive
  az network vnet create \
    --resource-group CreatePrivLinkService-rg \
    --location eastus2 \
    --name myVNetPE \
    --address-prefixes 11.1.0.0/16 \
    --subnet-name mySubnetPE \
    --subnet-prefixes 11.1.0.0/24
```

Aktualisieren Sie das Subnetz mit [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update), um Netzwerkrichtlinien für private Endpunkte zu deaktivieren:

```azurecli-interactive
az network vnet subnet update \
    --name mySubnetPE \
    --resource-group CreatePrivLinkService-rg \
    --vnet-name myVNetPE \
    --disable-private-endpoint-network-policies true
```

### <a name="create-endpoint-and-connection"></a>Erstellen eines Endpunkts und einer Verbindung

* Rufen Sie mit [az network private-link-service show](/cli/azure/network/private-link-service#az_network_private_link_service_show) die Ressourcen-ID des Private Link-Diensts ab. Der Befehl legt die Ressourcen-ID für die spätere Verwendung in einer Variable ab.

* Erstellen Sie mit [az network private-endpoint create](/cli/azure/network/private-endpoint#az_network_private_endpoint_create) den privaten Endpunkt im virtuellen Netzwerk, das Sie zuvor erstellt haben.

* Name: **MyPrivateEndpoint**
* Ressourcengruppe: **CreatePrivLinkService-rg**
* Verbindungsname: **myPEconnectiontoPLS**
* Standort: **eastus2**
* Im virtuellen Netzwerk **myVNetPE** und im Subnetz **mySubnetPE**

```azurecli-interactive
  export resourceid=$(az network private-link-service show \
    --name myPrivateLinkService \
    --resource-group CreatePrivLinkService-rg \
    --query id \
    --output tsv)

  az network private-endpoint create \
    --connection-name myPEconnectiontoPLS \
    --name myPrivateEndpoint \
    --private-connection-resource-id $resourceid \
    --resource-group CreatePrivLinkService-rg \
    --subnet mySubnetPE \
    --manual-request false \
    --vnet-name myVNetPE 

```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Verwenden Sie den Befehl [az group delete](/cli/azure/group#az-group-delete), um die Ressourcengruppe, den Private Link-Dienst, den Load Balancer und alle zugehörigen Ressourcen zu entfernen, wenn Sie sie nicht mehr benötigen.

```azurecli-interactive
  az group delete \
    --name CreatePrivLinkService-rg 
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Folgendes durchgeführt:

* Erstellen eines virtuellen Netzwerks und einer internen Azure Load Balancer-Instanz
* Erstellen eines Private Link-Diensts

Weitere Informationen zu privaten Azure-Endpunkten finden Sie unter:
> [!div class="nextstepaction"]
> [Schnellstart: Erstellen eines privaten Endpunkts mit Azure CLI](create-private-endpoint-cli.md)
