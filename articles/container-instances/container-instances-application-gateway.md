---
title: Statische IP-Adresse für eine Containergruppe
description: Erstellen einer Containergruppe in einem virtuellen Netzwerk und Verwenden eines Azure-Anwendungsgateways, um eine statische Front-End-IP-Adresse für eine Container-Web-App verfügbar zu machen
ms.topic: article
ms.date: 03/16/2020
ms.openlocfilehash: 0131780fdb04a71837d5ae9bf5498bf2bd499f8a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98035052"
---
# <a name="expose-a-static-ip-address-for-a-container-group"></a>Verfügbarmachen einer statischen IP-Adresse für eine Containergruppe

In diesem Artikel wird eine Möglichkeit gezeigt, wie Sie mithilfe eines Azure-[Anwendungsgateways](../application-gateway/overview.md) eine statische öffentliche IP-Adresse für eine [Containergruppe](container-instances-container-groups.md) verfügbar machen können. Führen Sie diese Schritte aus, wenn Sie einen statischen Einstiegspunkt für eine externe Container-App benötigen, die in Azure Container Instances ausgeführt wird. 

In diesem Artikel verwenden Sie die Azure-Befehlszeilenschnittstelle (Azure CLI), um die Ressourcen für das Szenario zu erstellen:

* Ein virtuelles Azure-Netzwerk
* Eine [im virtuellen Netzwerk](container-instances-vnet.md) bereitgestellte Containergruppe, die eine kleine Web-App hostet
* Ein Anwendungsgateway mit einer öffentlichen Front-End-IP-Adresse, ein Listener zum Hosten einer Website auf dem Gateway und eine Route zur Back-End-Containergruppe

Solange das Anwendungsgateway ausgeführt wird und die Containergruppe eine stabile private IP-Adresse im delegierten Subnetz des Netzwerks verfügbar macht, kann über diese öffentliche IP-Adresse auf die Containergruppe zugegriffen werden.

> [!NOTE]
> Die Azure-Gebühren für ein Anwendungsgateway werden basierend auf der Zeit, in der das Gateway bereitgestellt und verfügbar ist, sowie der Menge verarbeiteter Daten berechnet. Siehe [Preise](https://azure.microsoft.com/pricing/details/application-gateway/).

## <a name="create-virtual-network"></a>Virtuelles Netzwerk erstellen

In der Regel verfügen Sie bereits über ein virtuelles Azure-Netzwerk. Wenn dies nicht der Fall ist, erstellen Sie mit den folgenden Beispielbefehlen eines. Das virtuelle Netzwerk erfordert separate Subnetze für das Anwendungsgateway und die Containergruppe.

Erstellen Sie ggf. eine Azure-Ressourcengruppe. Beispiel:

```azureci
az group create --name myResourceGroup --location eastus
```

Erstellen Sie mit dem Befehl [az network vnet create][az-network-vnet-create] ein virtuelles Netzwerk. Dieser Befehl erstellt das Subnetz *myAGSubnet* im Netzwerk.

```azurecli
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroup \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
```

Erstellen Sie mit dem Befehl [az network vnet subnet create][az-network-vnet-subnet-create] ein Subnetz für die Back-End-Containergruppe. In diesem Beispiel heißt das Subnetz *myACISubnet*.

```azurecli
az network vnet subnet create \
  --name myACISubnet \
  --resource-group myResourceGroup \
  --vnet-name myVNet   \
  --address-prefix 10.0.2.0/24
```

Erstellen Sie mit dem Befehl [az network public-ip create][az-network-public-ip-create] eine statische öffentliche IP-Ressource. In einem späteren Schritt wird diese Adresse als Front-End des Anwendungsgateways konfiguriert.

```azurecli
az network public-ip create \
  --resource-group myResourceGroup \
  --name myAGPublicIPAddress \
  --allocation-method Static \
  --sku Standard
```

## <a name="create-container-group"></a>Erstellen einer Containergruppe

Führen Sie den folgenden Befehl [az container create][az-container-create] aus, um eine Containergruppe in dem virtuellen Netzwerk zu erstellen, das Sie im vorherigen Schritt konfiguriert haben. 

Die Gruppe wird im Subnetz *myACISubnet* bereitgestellt und enthält eine einzelne Instanz namens *appcontainer*, die das Image `aci-helloworld` pullt. Wie in anderen Artikeln der Dokumentation beschrieben, verpackt dieses Image eine kleine in Node.js geschriebene Web-App für eine statische HTML-Seite. 

```azurecli
az container create \
  --name appcontainer \
  --resource-group myResourceGroup \
  --image mcr.microsoft.com/azuredocs/aci-helloworld \
  --vnet myVNet \
  --subnet myACISubnet
```

Bei erfolgreicher Bereitstellung wird der Containergruppe eine private IP-Adresse im virtuellen Netzwerk zugewiesen. Führen Sie beispielsweise den folgenden Befehl [az container show][az-container-show] aus, um die IP-Adresse der Gruppe abzurufen:

```azurecli
az container show \
  --name appcontainer --resource-group myResourceGroup \
  --query ipAddress.ip --output tsv
```

Die Ausgabe lautet in etwa wie folgt: `10.0.2.4`.

Speichern Sie die IP-Adresse zur Verwendung in einem späteren Schritt in einer Umgebungsvariablen:

```azurecli
ACI_IP=$(az container show \
  --name appcontainer \
  --resource-group myResourceGroup \
  --query ipAddress.ip --output tsv)
```

> [!IMPORTANT]
> Wenn eine Containergruppe beendet, gestartet oder neu gestartet wird, kann sich die private IP-Adresse der Gruppe ändern. In diesem Fall müssen Sie die Konfiguration des Anwendungsgateways aktualisieren.

## <a name="create-application-gateway"></a>Erstellen eines Anwendungsgateways

Erstellen Sie wie im [Schnellstart für Application Gateway](../application-gateway/quick-create-cli.md) beschrieben ein Anwendungsgateway im virtuellen Netzwerk. Der folgende Befehl [az network application-gateway create][az-network-application-gateway-create] erstellt ein Gateway mit einer öffentlichen Front-End-IP-Adresse und eine Route zur Back-End-Containergruppe. Ausführliche Informationen zu den Gatewayeinstellungen finden Sie in der [Dokumentation zu Application Gateway](../application-gateway/index.yml).

```azurecli
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroup \
  --capacity 2 \
  --sku Standard_v2 \
  --http-settings-protocol http \
  --public-ip-address myAGPublicIPAddress \
  --vnet-name myVNet \
  --subnet myAGSubnet \
  --servers "$ACI_IP" 
```


Die Erstellung des Anwendungsgateways in Azure kann bis zu 15 Minuten in Anspruch nehmen. 

## <a name="test-public-ip-address"></a>Testen der öffentlichen IP-Adresse
  
Nun können Sie den Zugriff auf die Web-App testen, die in der Containergruppe hinter dem Anwendungsgateway ausgeführt wird.

Führen Sie den Befehl [az network public-ip show][az-network-public-ip-show] aus, um die öffentliche Front-End-IP-Adresse des Gateways abzurufen:

```azurecli
az network public-ip show \
--resource-group myresourcegroup \
--name myAGPublicIPAddress \
--query [ipAddress] \
--output tsv
```

Die Ausgabe ist eine öffentliche IP-Adresse wie `52.142.18.133`.

Wenn die Konfiguration erfolgreich war, können Sie die ausgeführte Web-App anzeigen, indem Sie in Ihrem Browser zur öffentlichen IP-Adresse des Gateways navigieren. Ist der Zugriff erfolgreich, sehen Sie eine Seite wie die folgende:

![Screenshot eines Browsers mit ausgeführter Anwendung in einer Azure-Containerinstanz](./media/container-instances-application-gateway/aci-app-app-gateway.png)

## <a name="next-steps"></a>Nächste Schritte

* Erstellen Sie mit einer [Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-wordpress-vnet) eine Containergruppe mit einer WordPress-Containerinstanz als Back-End-Server hinter einem Anwendungsgateway.
* Sie können auch ein Anwendungsgateway mit einem Zertifikat für die SSL-Terminierung konfigurieren. Weitere Informationen finden Sie in der [Übersicht](../application-gateway/ssl-overview.md) und im [Tutorial](../application-gateway/create-ssl-portal.md).
* Je nach Szenario können Sie ggf. andere Azure-Lastenausgleichslösungen mit Azure Container Instances verwenden. Beispielsweise können Sie mit [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) Datenverkehr auf mehrere Containerinstanzen und Regionen verteilen. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://aaronmsft.com/posts/azure-container-instances/).

[az-network-vnet-create]:  /cli/azure/network/vnet#az-network-vnet-create
[az-network-vnet-subnet-create]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-create
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-show]: /cli/azure/network/public-ip#az-network-public-ip-show
[az-network-application-gateway-create]: /cli/azure/network/application-gateway#az-network-application-gateway-create
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
