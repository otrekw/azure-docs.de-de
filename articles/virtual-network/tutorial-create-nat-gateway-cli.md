---
title: 'Tutorial: Erstellen eines NAT-Gateways: Azure CLI'
titlesuffix: Azure Virtual Network NAT
description: Beginnen Sie mit dem Erstellen eines NAT-Gateways mithilfe der Azure CLI.
author: asudbring
ms.author: allensu
ms.service: virtual-network
ms.subservice: nat
ms.topic: tutorial
ms.date: 03/10/2021
ms.custom: template-tutorial
ms.openlocfilehash: 5dd431a5a7377c409be0794511c5f402d1c5a3a9
ms.sourcegitcommit: b572ce40f979ebfb75e1039b95cea7fce1a83452
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "102636742"
---
# <a name="tutorial-create-a-nat-gateway-using-the-azure-cli"></a>Tutorial: Erstellen eines NAT-Gateways mithilfe der Azure CLI

In diesem Tutorial erfahren Sie, wie Sie den Azure Virtual Network NAT-Dienst verwenden. Sie erstellen ein NAT-Gateway, um ausgehende Konnektivität für einen virtuellen Computer in Azure bereitzustellen. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen Sie ein virtuelles Netzwerk.
> * Erstellen Sie eine VM.
> * Erstellen Sie ein NAT-Gateway, und ordnen Sie es dem virtuellen Netzwerk zu.
> * Stellen Sie eine Verbindung mit dem virtuellen Computer her, und überprüfen Sie die NAT-IP-Adresse.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

- Für diesen Schnellstart ist mindestens Version 2.0.28 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen **myResourceGroupNAT** am Standort **eastus2** erstellt:

```azurecli-interactive
  az group create \
    --name myResourceGroupNAT \
    --location eastus2
```

## <a name="create-the-nat-gateway"></a>Erstellen des NAT-Gateways

In diesem Abschnitt erstellen Sie das NAT-Gateway und unterstützende Ressourcen.

### <a name="create-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

Für den Zugriff auf das Internet benötigen Sie mindestens eine öffentliche IP-Adresse für das NAT-Gateway. Verwenden Sie [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create), um in **myResourceGroupNAT** eine öffentliche IP-Ressource namens **myPublicIP** zu erstellen. 

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupNAT \
    --name myPublicIP \
    --sku standard \
    --allocation static
```

### <a name="create-nat-gateway-resource"></a>Erstellen einer NAT-Gatewayressource

Erstellen Sie mit [az network nat gateway create](/cli/azure/network/nat#az_network_nat_gateway_create) ein globales Azure-NAT-Gateway. Als Ergebnis dieses Befehls wird eine Gatewayressource namens **myNATgateway** erstellt, die die öffentliche IP-Adresse **myPublicIP** verwendet. Das Leerlauftimeout wird auf zehn Minuten festgelegt.  

```azurecli-interactive
  az network nat gateway create \
    --resource-group myResourceGroupNAT \
    --name myNATgateway \
    --public-ip-addresses myPublicIP \
    --idle-timeout 10       
  ```

### <a name="create-virtual-network"></a>Virtuelles Netzwerk erstellen

Erstellen Sie ein virtuelles Netzwerk mit dem Namen **myVnet** und dem Subnetz **mySubnet** in der Ressourcengruppe **myResourceGroup**, indem Sie den Befehl [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) verwenden. Der IP-Adressraum für das virtuelle Netzwerk ist **10.1.0.0/16**. Das Subnetz im virtuellen Netzwerk ist **10.1.0.0/24**.

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroupNAT \
    --location eastus2 \
    --name myVnet \
    --address-prefix 10.1.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 10.1.0.0/24
```

### <a name="create-bastion-host"></a>Erstellen eines Bastionhosts

Erstellen Sie einen Azure Bastion-Host mit dem Namen **myBastionHost** für den Zugriff auf den virtuellen Computer. 

Erstellen Sie mit [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) ein Azure Bastion-Subnetz.

```azurecli-interactive
az network vnet subnet create \
    --resource-group myResourceGroupNAT \
    --name AzureBastionSubnet \
    --vnet-name myVNet \
    --address-prefixes 10.1.1.0/24
```

Erstellen Sie mit [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) eine öffentliche IP-Adresse für den Bastionhost. 

```azurecli-interactive
az network public-ip create \
    --resource-group myResourceGroupNAT \
    --name myBastionIP \
    --sku Standard
```

Erstellen Sie mit [az network bastion create](/cli/azure/network/bastion#az-network-bastion-create) den Bastionhost. 

```azurecli-interactive
az network bastion create \
    --resource-group myResourceGroupNAT \
    --name myBastionHost \
    --public-ip-address myBastionIP \
    --vnet-name myVNet \
    --location eastus2
```

### <a name="configure-nat-service-for-source-subnet"></a>Konfigurieren des NAT-Diensts für das Quellsubnetz

Mit [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update) konfigurieren Sie das Quellsubnetz **mySubnet** im virtuellen Netzwerk **myVnet** so, dass die spezifische NAT-Gatewayressource **myNATgateway** verwendet wird. Mit diesem Befehl wird der NAT-Dienst im angegebenen Subnetz aktiviert.

```azurecli-interactive
  az network vnet subnet update \
    --resource-group myResourceGroupNAT \
    --vnet-name myVnet \
    --name mySubnet \
    --nat-gateway myNATgateway
```

Der gesamte ausgehende Datenverkehr an Ziele im Internet verwendet jetzt das NAT-Gateway.  Eine UDR muss nicht konfiguriert werden.


## <a name="virtual-machine"></a>Virtueller Computer

In diesem Abschnitt erstellen Sie einen virtuellen Computer, um das NAT-Gateway zu testen und die öffentliche IP-Adresse der ausgehenden Verbindung zu überprüfen.

Erstellen Sie den virtuellen Computer mit [az vm create](/cli/azure/vm#az-vm-create).

```azurecli-interactive
az vm create \
    --name myVM \
    --resource-group myResourceGroupNAT  \
    --admin-username azureuser \
    --image win2019datacenter \
    --public-ip-address "" \
    --subnet mySubnet \
    --vnet-name myVNet
```

Warten Sie, bis die Erstellung des virtuellen Computers abgeschlossen ist, bevor Sie mit dem nächsten Abschnitt fortfahren.

## <a name="test-nat-gateway"></a>Testen des NAT-Gateways

In diesem Abschnitt testen Sie das NAT-Gateway. Sie ermitteln zunächst die öffentliche IP-Adresse des NAT-Gateways. Anschließend stellen Sie eine Verbindung mit dem virtuellen Testcomputer her und überprüfen die ausgehende Verbindung über das NAT-Gateway.
    
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)

1. Suchen Sie auf dem Bildschirm **Übersicht** die öffentliche IP-Adresse für das NAT-Gateway. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend **myPublicIP** aus.

2. Notieren Sie sich die öffentliche IP-Adresse:

    :::image type="content" source="./media/tutorial-create-nat-gateway-portal/find-public-ip.png" alt-text="Ermitteln der öffentlichen IP-Adresse des NAT-Gateways" border="true":::

3. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myVM** (in der Ressourcengruppe **myResourceGroupNAT**) aus.

4. Wählen Sie auf der Seite **Übersicht** die Option **Verbinden** und dann **Bastion** aus.

5. Wählen Sie die blaue Schaltfläche **Bastion verwenden** aus.

6. Geben Sie den Benutzernamen und das Kennwort ein, die Sie bei der VM-Erstellung verwendet haben.

7. Öffnen Sie **Internet Explorer** auf **myTestVM**.

8. Geben Sie in der Adressleiste **https://whatsmyip.com** ein.

9. Vergewissern Sie sich, dass die angezeigte IP-Adresse mit der NAT-Gatewayadresse übereinstimmt, die Sie im vorherigen Schritt notiert haben:

    :::image type="content" source="./media/tutorial-create-nat-gateway-portal/my-ip.png" alt-text="Internet Explorer mit externer ausgehenden IP-Adresse" border="true":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht mehr benötigen, löschen Sie die das virtuelle Netzwerk, den virtuellen Computer und das NAT-Gateway wie folgt:

```azurecli-interactive 
  az group delete \
    --name myResourceGroupNAT
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Virtual Network NAT finden Sie unter folgendem Link:
> [!div class="nextstepaction"]
> [Übersicht über Virtual Network NAT](nat-overview.md)
