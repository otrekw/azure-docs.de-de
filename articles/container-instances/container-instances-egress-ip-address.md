---
title: Konfigurieren statischer ausgehender IP-Adressen
description: Konfigurieren von Azure Firewall und benutzerdefinierten Routen für Azure Container Instances-Workloads, die die öffentliche IP-Adresse der Firewall für eingehenden und ausgehenden Datenverkehr verwenden
ms.topic: article
ms.date: 07/16/2020
ms.openlocfilehash: 497645b9fe7f908cc9b8b4d7ed0ba5e201570160
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2020
ms.locfileid: "89566567"
---
# <a name="configure-a-single-public-ip-address-for-outbound-and-inbound-traffic-to-a-container-group"></a>Konfigurieren einer einzelnen öffentlichen IP-Adresse für eingehenden und ausgehenden Datenverkehr in eine und von einer Containergruppe

Durch Einrichten einer [Containergruppe](container-instances-container-groups.md) mit einer extern zugänglichen IP-Adresse können externe Clients die IP-Adresse für den Zugriff auf einen Container in der Gruppe nutzen. So kann beispielsweise ein Browser auf eine Web-App zugreifen, die in einem Container ausgeführt wird. Derzeit verwendet eine Containergruppe allerdings eine andere IP-Adresse für den ausgehenden Datenverkehr. Diese ausgehende IP-Adresse wird nicht programmgesteuert verfügbar gemacht, sodass die Überwachung und Konfiguration von Clientfirewallregeln für Containergruppen etwas komplexer ist.

Der vorliegende Artikel führt Sie durch die Schritte zum Konfigurieren einer Containergruppe in einem [virtuellen Netzwerk](container-instances-virtual-network-concepts.md), das in [Azure Firewall](../firewall/overview.md) integriert ist. Indem Sie eine benutzerdefinierte Route zur Containergruppe sowie Firewallregeln einrichten, können Sie Datenverkehr in die und aus der Containergruppe weiterleiten und identifizieren. Der eingehende und ausgehende Containerdatenverkehr verwendet die öffentliche IP-Adresse der Firewall. Ein einzelne ausgehende IP-Adresse kann von mehreren Containergruppen verwendet werden, die in dem Subnetz des virtuellen Netzwerks bereitgestellt sind, das an Azure Container Instances delegiert ist.

In diesem Artikel verwenden Sie die Azure-Befehlszeilenschnittstelle (Azure CLI), um die Ressourcen für das Szenario zu erstellen:

* Containergruppen, die in einem delegierten Subnetz [im virtuellen Netzwerk](container-instances-vnet.md) bereitgestellt sind 
* Eine Azure Firewall-Instanz, die im Netzwerk mit einer statischen öffentlichen IP-Adresse bereitgestellt ist
* Eine benutzerdefinierte Route im Subnetz der Containergruppen
* Eine NAT-Regel für den eingehenden Firewalldatenverkehr und eine Anwendungsregel für den ausgehenden Datenverkehr

Danach überprüfen Sie den Datenverkehr in und aus Beispielcontainergruppen über die Firewall.

## <a name="deploy-aci-in-a-virtual-network"></a>Bereitstellen von Azure Container Instances (ACI) in einem virtuellen Netzwerk

In der Regel verfügen Sie bereits über ein virtuelles Azure-Netzwerk, in der Sie eine Containergruppe bereitstellen können. Zum Zweck der Veranschaulichung: Die folgenden Befehle erstellen während der Erstellung der Containergruppe ein virtuelles Netzwerk und ein Subnetz. Das Subnetz wird an Azure Container Instances delegiert. 

Die Containergruppe führt eine kleine Web-App aus dem Image `aci-helloworld` aus. Wie in anderen Artikeln der Dokumentation beschrieben, verpackt dieses Image eine kleine in Node.js geschriebene Web-App für eine statische HTML-Seite.

Sofern erforderlich, erstellen Sie mit dem Befehl [az group create][az-group-create] zunächst eine Azure-Ressourcengruppe. Beispiel:

```azurecli
az group create --name myResourceGroup --location eastus
```

Um die folgenden Befehlsbeispiele zu vereinfachen, verwenden Sie eine Umgebungsvariable für den Namen der Ressourcengruppe:

```console
export RESOURCE_GROUP_NAME=myResourceGroup
```

Erstellen Sie die Containergruppe mit dem Befehl [az container create][az-container-create]:

```azurecli
az container create \
  --name appcontainer \
  --resource-group $RESOURCE_GROUP_NAME \
  --image mcr.microsoft.com/azuredocs/aci-helloworld \
  --vnet aci-vnet \
  --vnet-address-prefix 10.0.0.0/16 \
  --subnet aci-subnet \
  --subnet-address-prefix 10.0.0.0/24
```

> [!TIP]
> Passen Sie den Wert von `--subnet address-prefix` an den IP-Adressraum an, den Sie in Ihrem Subnetz benötigen. Das kleinste unterstützte Subnetz ist /29 und stellt acht IP-Adressen bereit. Einige IP-Adressen sind für die Verwendung durch Azure reserviert.

Zur Verwendung in einem späteren Schritt rufen Sie die private IP-Adresse der Containergruppe ab, indem Sie den Befehl [az container show][az-container-show] ausführen:

```azurecli
ACI_PRIVATE_IP="$(az container show --name appcontainer \
  --resource-group $RESOURCE_GROUP_NAME \
  --query ipAddress.ip --output tsv)"
```

## <a name="deploy-azure-firewall-in-network"></a>Bereitstellen von Azure Firewall im Netzwerk

Verwenden Sie in den folgenden Abschnitten die Azure CLI, um eine Azure Firewall-Instanz im virtuellen Netzwerk bereitzustellen. Hintergrundinformationen dazu finden Sie im [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal](../firewall/deploy-cli.md).

Verwenden Sie zunächst den Befehl [az network vnet subnet create][az-network-vnet-subnet-create], um ein Subnetz namens AzureFirewallSubnet für die Firewall hinzuzufügen. AzureFirewallSubnet ist der *erforderliche* Name dieses Subnetzes.

```azurecli
az network vnet subnet create \
  --name AzureFirewallSubnet \
  --resource-group $RESOURCE_GROUP_NAME \
  --vnet-name aci-vnet   \
  --address-prefix 10.0.1.0/26
```

Verwenden Sie die folgenden [Azure CLI-Befehle](../firewall/deploy-cli.md), um eine Firewall im Subnetz zu erstellen.

Fügen Sie der Azure CLI mit dem Befehl [az extension add][az-extension-add] die Firewallerweiterung hinzu, falls diese noch nicht installiert ist:

```azurecli
az extension add --name azure-firewall
```

Erstellen der Firewallressourcen:

```azurecli
az network firewall create \
  --name myFirewall \
  --resource-group $RESOURCE_GROUP_NAME \
  --location eastus

az network public-ip create \
  --name fw-pip \
  --resource-group $RESOURCE_GROUP_NAME \
  --location eastus \
  --allocation-method static \
  --sku standard
    
az network firewall ip-config create \
  --firewall-name myFirewall \
  --name FW-config \
  --public-ip-address fw-pip \
  --resource-group $RESOURCE_GROUP_NAME \
  --vnet-name aci-vnet
```

Aktualisieren Sie die Firewallkonfiguration mit dem Befehl [az network firewall update][az-network-firewall-update]:

```azurecli
az network firewall update \
  --name myFirewall \
  --resource-group $RESOURCE_GROUP_NAME
```

Rufen Sie mit dem Befehl [az network firewall ip-config list][az-network-firewall-ip-config-list] die private IP-Adresse der Firewall ab. Diese private IP-Adresse wird in einem späteren Befehl verwendet.


```azurecli
FW_PRIVATE_IP="$(az network firewall ip-config list \
  --resource-group $RESOURCE_GROUP_NAME \
  --firewall-name myFirewall \
  --query "[].privateIpAddress" --output tsv)"
```
Rufen Sie mit dem Befehl [az network public-ip show][az-network-public-ip-show] die öffentliche IP-Adresse ab. Diese öffentliche IP-Adresse wird in einem späteren Befehl verwendet.

```azurecli
FW_PUBLIC_IP="$(az network public-ip show \
  --name fw-pip \
  --resource-group $RESOURCE_GROUP_NAME \
  --query ipAddress --output tsv)"
```

## <a name="define-user-defined-route-on-aci-subnet"></a>Definieren einer benutzerdefinierten Route im ACI-Subnetz

Definieren Sie eine benutzerdefinierte Route im ACI-Subnetz, um Datenverkehr an die Azure Firewall-Instanz umzuleiten. Weitere Informationen finden Sie unter [Weiterleiten von Netzwerkdatenverkehr](../virtual-network/tutorial-create-route-table-cli.md). 

### <a name="create-route-table"></a>Erstellen einer Routingtabelle

Führen Sie zunächst den folgenden Befehl [az network route-table create][az-network-route-table-create] aus, um die Routingtabelle zu erstellen. Erstellen Sie die Routingtabelle in derselben Region, in der sich auch das virtuelle Netzwerk befindet.

```azurecli
az network route-table create \
  --name Firewall-rt-table \
  --resource-group $RESOURCE_GROUP_NAME \
  --location eastus \
  --disable-bgp-route-propagation true
```

### <a name="create-route"></a>Erstellen der Route

Führen Sie [az network-route-table route create][az-network-route-table-route-create] aus, um eine Route in der Routingtabelle zu erstellen. Um Datenverkehr an die Firewall weiterzuleiten, legen Sie den Typ des nächsten Hops auf `VirtualAppliance` fest, und übergeben Sie die private IP-Adresse der Firewall als Adresse des nächsten Hops.

```azurecli
az network route-table route create \
  --resource-group $RESOURCE_GROUP_NAME \
  --name DG-Route \
  --route-table-name Firewall-rt-table \
  --address-prefix 0.0.0.0/0 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address $FW_PRIVATE_IP
```

### <a name="associate-route-table-to-aci-subnet"></a>Zuordnen der Routingtabelle zum ACI-Subnetz

Führen Sie den Befehl [az network vnet subnet update][az-network-vnet-subnet-update] aus, um die Routingtabelle dem Subnetz zuzuordnen, das an Azure Container Instances delegiert wurde.

```azurecli
az network vnet subnet update \
  --name aci-subnet \
  --resource-group $RESOURCE_GROUP_NAME \
  --vnet-name aci-vnet \
  --address-prefixes 10.0.0.0/24 \
  --route-table Firewall-rt-table
```

## <a name="configure-rules-on-firewall"></a>Konfigurieren von Regeln in der Firewall

Standardmäßig verweigert (blockiert) Azure Firewall eingehenden und ausgehenden Datenverkehr. 

### <a name="configure-nat-rule-on-firewall-to-aci-subnet"></a>Konfigurieren einer NAT-Regel zum ACI-Subnetz in der Firewall

Erstellen Sie eine [NAT-Regel](../firewall/rule-processing.md) in der Firewall, um eingehenden Internetdatenverkehr in den Anwendungscontainer, den Sie zuvor im Netzwerk gestartet haben, zu übersetzen und zu filtern. Details dazu finden Sie unter [Filtern von eingehendem Internetdatenverkehr mit Azure Firewall-DNAT](../firewall/tutorial-firewall-dnat.md).

Erstellen Sie mit dem Befehl [az network firewall nat-rule create][az-network-firewall-nat-rule-create] eine NAT-Regel und eine NAT-Sammlung:

```azurecli
az network firewall nat-rule create \
  --firewall-name myFirewall \
  --collection-name myNATCollection \
  --action dnat \
  --name myRule \
  --protocols TCP \
  --source-addresses '*' \
  --destination-addresses $FW_PUBLIC_IP \
  --destination-ports 80 \
  --resource-group $RESOURCE_GROUP_NAME \
  --translated-address $ACI_PRIVATE_IP \
  --translated-port 80 \
  --priority 200
```

Fügen Sie nach Bedarf NAT-Regeln hinzu, um Datenverkehr an andere IP-Adressen im Subnetz zu filtern. Beispielsweise könnten andere Containergruppen im Subnetz IP-Adressen für eingehenden Datenverkehr verfügbar machen, oder der Containergruppe könnten nach einem Neustart andere interne IP-Adressen zugewiesen werden.

### <a name="create-outbound-application-rule-on-the-firewall"></a>Erstellen einer ausgehenden Anwendungsregel in der Firewall

Führen Sie den Befehl [az network firewall application-rule create][az-network-firewall-application-rule-create] aus, um eine Ausgangsregel in der Firewall zu erstellen. Diese Beispielregel lässt den Zugriff aus dem Subnetz, das an Azure Container Instances delegiert wurde, auf den vollqualifizierten Domänennamen `checkip.dyndns.org` zu. In einem späteren Schritt wird HTTP-Zugriff auf die Website genutzt, um die ausgehenden IP-Adressen aus Azure Container Instances zu bestätigen.

```azurecli
az network firewall application-rule create \
  --collection-name myAppCollection \
  --firewall-name myFirewall \
  --name Allow-CheckIP \
  --protocols Http=80 Https=443 \
  --resource-group $RESOURCE_GROUP_NAME \
  --target-fqdns checkip.dyndns.org \
  --source-addresses 10.0.0.0/24 \
  --priority 200 \
  --action Allow
```

## <a name="test-container-group-access-through-the-firewall"></a>Testen des Containergruppenzugriffs über die Firewall

Mit den folgenden Abschnitten wird überprüft, ob das an Azure Container Instances delegierte Subnetz ordnungsgemäß hinter der Azure Firewall-Instanz konfiguriert ist. In den vorherigen Schritten wurde eingehender Datenverkehr an das Subnetz und ausgehender Datenverkehr aus dem Subnetz durch die Firewall geleitet.

### <a name="test-ingress-to-a-container-group"></a>Testen des eingehenden Datenverkehrs an eine Containergruppe

Testen Sie den eingehenden Datenverkehr an den *App-Container*, der im virtuellen Netzwerk ausgeführt wird, indem Sie zur öffentlichen IP-Adresse der Firewall wechseln. Zuvor haben Sie die öffentliche IP-Adresse in der Variable „$FW_PUBLIC_IP“ gespeichert:

```bash
echo $FW_PUBLIC_IP
```

Die Ausgabe sieht in etwa wie folgt aus:

```console
52.142.18.133
```

Sofern die NAT-Regel in der Firewall ordnungsgemäß konfiguriert ist, wird Folgendes angezeigt, wenn Sie die öffentliche IP-Adresse der Firewall in Ihren Browser eingeben:

:::image type="content" source="media/container-instances-egress-ip-address/aci-ingress-ip-address.png" alt-text="Wechseln zur öffentlichen IP-Adresse der Firewall":::

### <a name="test-egress-from-a-container-group"></a>Testen des ausgehenden Datenverkehrs aus einer Containergruppe


Stellen Sie den folgenden Beispielcontainer im virtuellen Netzwerk bereit. Bei Ausführung des Containers sendet er eine einzelne HTTP-Anforderung an `http://checkip.dyndns.org`, wodurch die IP-Adresse des Absenders (ausgehende IP-Adresse) angezeigt wird. Wenn die Anwendungsregel in der Firewall ordnungsgemäß konfiguriert ist, wird die öffentliche IP-Adresse der Firewall zurückgegeben.

```azurecli
az container create \
  --resource-group $RESOURCE_GROUP_NAME \
  --name testegress \
  --image mcr.microsoft.com/azuredocs/aci-tutorial-sidecar \
  --command-line "curl -s http://checkip.dyndns.org" \
  --restart-policy OnFailure \
  --vnet aci-vnet \
  --subnet aci-subnet
```

Sehen Sie sich die Containerprotokolle an, um zu überprüfen, ob die IP-Adresse mit der öffentlichen IP-Adresse der Firewall übereinstimmt.

```azurecli
az container logs \
  --resource-group $RESOURCE_GROUP_NAME \
  --name testegress 
```

Die Ausgabe Die Ausgabe lautet in etwa wie folgt:

```console
<html><head><title>Current IP Check</title></head><body>Current IP Address: 52.142.18.133</body></html>
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Containergruppen in einem virtuellen Netzwerk hinter eine Azure-Firewall eingerichtet. Sie haben eine benutzerdefinierte Route sowie NAT- und Anwendungsregeln in der Firewall konfiguriert. Mit dieser Konfiguration haben Sie eine einzelne, statische IP-Adresse für den eingehenden und ausgehenden Datenverkehr in Azure Container Instances eingerichtet.

Weitere Informationen zum Verwalten von Datenverkehr und Schützen von Azure-Ressourcen finden Sie in der Dokumentation zu [Azure Firewall](../firewall/index.yml).



[az-group-create]: /cli/azure/group#az-group-create
[az-container-create]: /cli/azure/container#az-container-create
[az-network-vnet-subnet-create]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-create
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-network-firewall-update]: /cli/azure/ext/azure-firewall/network/firewall#ext-azure-firewall-az-network-firewall-update
[az-network-public-ip-show]: /cli/azure/network/public-ip/#az-network-public-ip-show
[az-network-route-table-create]:/cli/azure/network/route-table/#az-network-route-table-create
[az-network-route-table-route-create]: /cli/azure/network/route-table/route#az-network-route-table-route-create
[az-network-firewall-ip-config-list]: /cli/azure/ext/azure-firewall/network/firewall/ip-config#ext-azure-firewall-az-network-firewall-ip-config-list
[az-network-vnet-subnet-update]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-update
[az-container-exec]: /cli/azure/container#az-container-exec
[az-vm-create]: /cli/azure/vm#az-vm-create
[az-vm-open-port]: /cli/azure/vm#az-vm-open-port
[az-vm-list-ip-addresses]: /cli/azure/vm#az-vm-list-ip-addresses
[az-network-firewall-application-rule-create]: /cli/azure/ext/azure-firewall/network/firewall/application-rule#ext-azure-firewall-az-network-firewall-application-rule-create
[az-network-firewall-nat-rule-create]: /cli/azure/ext/azure-firewall/network/firewall/nat-rule#ext-azure-firewall-az-network-firewall-nat-rule-create






