---
title: 'Schnellstart: Erstellen eines internen Lastenausgleichs – Azure CLI'
titleSuffix: Azure Load Balancer
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie einen internen Lastenausgleich über die Azure-Befehlszeilenschnittstelle erstellen.
services: load-balancer
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a load balancer so that I can load balance internal traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/19/2020
ms.author: allensu
ms.custom: mvc, devx-track-js, devx-track-azurecli
ms.openlocfilehash: 15060a367bba2d50d7054730321f7f20d4c25e46
ms.sourcegitcommit: 67b44a02af0c8d615b35ec5e57a29d21419d7668
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2021
ms.locfileid: "97916676"
---
# <a name="quickstart-create-an-internal-load-balancer-to-load-balance-vms-using-azure-cli"></a>Schnellstart: Erstellen eines internen Load Balancers für den Lastenausgleich virtueller Computer mit der Azure CLI

Beginnen Sie mit der Verwendung von Azure Load Balancer, indem Sie mithilfe der Azure CLI einen internen Lastenausgleich und drei virtuelle Computer erstellen.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)] 

- Für diesen Schnellstart ist mindestens Version 2.0.28 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe:

* Benannt als **CreateIntLBQS-rg**. 
* Standort: **eastus**

```azurecli-interactive
  az group create \
    --name CreateIntLBQS-rg \
    --location eastus

```
---

# <a name="standard-sku"></a>[**Standard-SKU**](#tab/option-1-create-load-balancer-standard)

>[!NOTE]
>Ein Lastenausgleich mit Standard-SKU wird für Produktionsworkloads empfohlen. Weitere Informationen zu SKUs finden Sie unter **[Azure Load Balancer-SKUs](skus.md)** .

:::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/resources-diagram-internal.png" alt-text="Für den Schnellstart erstellte Load Balancer Standard-Ressourcen." border="false":::

## <a name="configure-virtual-network---standard"></a>Konfigurieren des virtuellen Netzwerks: Standard

Erstellen Sie zunächst die unterstützenden virtuellen Netzwerkressourcen, bevor Sie virtuelle Computer und Ihren Lastenausgleich bereitstellen.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) ein virtuelles Netzwerk:

* Name: **myVNet**
* Adresspräfix: **10.1.0.0/16**
* Subnetzname: **myBackendSubnet**
* Subnetzpräfix: **10.1.0.0/24**
* In der Ressourcengruppe **CreateIntLBQS-rg**.
* Standort: **eastus**

```azurecli-interactive
  az network vnet create \
    --resource-group CreateIntLBQS-rg \
    --location eastus \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name myBackendSubnet \
    --subnet-prefixes 10.1.0.0/24
```

### <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

Verwenden Sie [az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create), um eine öffentliche IP-Adresse für den Bastionhost zu erstellen:

* Erstellen einer standardmäßigen zonenredundanten öffentlichen IP-Adresse namens **myBastionIP**
* In **CreateIntLBQS-rg**.

```azurecli-interactive
az network public-ip create \
    --resource-group CreateIntLBQS-rg  \
    --name myBastionIP \
    --sku Standard
```
### <a name="create-a-bastion-subnet"></a>Erstellen eines Bastionsubnetzes

Erstellen Sie mit [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) ein Bastionsubnetz:

* Name: **AzureBastionSubnet**
* Adresspräfix: **10.1.1.0/24**
* Virtuelles Netzwerk: **myVNet**
* In Ressourcengruppe **CreateIntLBQS-rg**.

```azurecli-interactive
az network vnet subnet create \
    --resource-group CreateIntLBQS-rg  \
    --name AzureBastionSubnet \
    --vnet-name myVNet \
    --address-prefixes 10.1.1.0/24
```

### <a name="create-bastion-host"></a>Erstellen eines Bastionhosts

Erstellen Sie mit [az network bastion create](/cli/azure/network/bastion#az-network-bastion-create) einen Bastionhost:

* Name: **myBastionHost**
* In **CreateIntLBQS-rg**.
* Der öffentlichen IP-Adresse **myBastionIP** zugeordnet
* Dem virtuellen Netzwerk **myVNet** zugeordnet
* Standort: **eastus**

```azurecli-interactive
az network bastion create \
    --resource-group CreateIntLBQS-rg  \
    --name myBastionHost \
    --public-ip-address myBastionIP \
    --vnet-name myVNet \
    --location eastus
```

Die Bereitstellung des Azure Bastion-Hosts kann einige Minuten dauern.


### <a name="create-a-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe

Für einen Standard-Lastenausgleich müssen die virtuellen Computer unter der Back-End-Adresse über Netzwerkschnittstellen verfügen, die einer Netzwerksicherheitsgruppe angehören. 

Erstellen Sie mit [az network nsg create](/cli/azure/network/nsg#az-network-nsg-create) eine Netzwerksicherheitsgruppe:

* Name: **myNSG**
* In Ressourcengruppe **CreateIntLBQS-rg**.

```azurecli-interactive
  az network nsg create \
    --resource-group CreateIntLBQS-rg \
    --name myNSG
```

### <a name="create-a-network-security-group-rule"></a>Erstellen einer Netzwerksicherheitsgruppen-Regel

Erstellen Sie mit [az network nsg rule create](/cli/azure/network/nsg/rule#az-network-nsg-rule-create) eine Netzwerksicherheitsgruppen-Regel:

* Name: **myNSGRuleHTTP**
* Netzwerksicherheitsgruppe: **myNSG** (aus dem vorherigen Schritt)
* In Ressourcengruppe **CreateIntLBQS-rg**.
* Protokoll **(*)**
* Richtung: **Eingehend**
* Quelle: **(*)**
* Ziel: **(*)**
* Zielport: **Port 80**
* Zugriff: **Zulassen**
* Priorität: **200**

```azurecli-interactive
  az network nsg rule create \
    --resource-group CreateIntLBQS-rg \
    --nsg-name myNSG \
    --name myNSGRuleHTTP \
    --protocol '*' \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 200
```

## <a name="create-backend-servers---standard"></a>Erstellen von Back-End-Servern: Standard

In diesem Abschnitt wird Folgendes erstellt:

* Drei Netzwerkschnittstellen für die virtuellen Computer
* Drei virtuelle Computer als Back-End-Server für den Lastenausgleich

### <a name="create-network-interfaces-for-the-virtual-machines"></a>Erstellen von Netzwerkschnittstellen für die virtuellen Computer

Erstellen Sie mit [az network nic create](/cli/azure/network/nic#az-network-nic-create) drei Netzwerkschnittstellen:

* Namen: **myNicVM1**, **myNicVM2** und **myNicVM3**
* In Ressourcengruppe **CreateIntLBQS-rg**.
* Virtuelles Netzwerk: **myVNet**
* Subnetz: **myBackendSubnet**
* Netzwerksicherheitsgruppe: **myNSG**

```azurecli-interactive
  array=(myNicVM1 myNicVM2 myNicVM3)
  for vmnic in "${array[@]}"
  do
    az network nic create \
        --resource-group CreateIntLBQS-rg \
        --name $vmnic \
        --vnet-name myVNet \
        --subnet myBackEndSubnet \
        --network-security-group myNSG
  done
```

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie mit [az vm create](/cli/azure/vm#az-vm-create) die virtuellen Computer:

* **myVM1**, **myVM2** und **myVM3**.
* In Ressourcengruppe **CreateIntLBQS-rg**.
* Angefügt an die Netzwerkschnittstellen **myNicVM1**, **myNicVM2** und **myNicVM3**.
* Image des virtuellen Computers: **win2019datacenter**
* In **Zone 1**, **Zone 2** und **Zone 3**

```azurecli-interactive
  array=(1 2 3)
  for n in "${array[@]}"
  do
    az vm create \
    --resource-group CreateIntLBQS-rg \
    --name myVM$n \
    --nics myNicVM$n \
    --image win2019datacenter \
    --admin-username azureuser \
    --zone $n \
    --no-wait
  done
```

Die Bereitstellung der virtuellen Computer kann eine Weile dauern.

## <a name="create-standard-load-balancer"></a>Erstellen eines Standard-Lastenausgleichs

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
* Dem Back-End-Subnetz **myBackendSubnet** zugeordnet

```azurecli-interactive
  az network lb create \
    --resource-group CreateIntLBQS-rg \
    --name myLoadBalancer \
    --sku Standard \
    --vnet-name myVnet \
    --subnet myBackendSubnet \
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
    --resource-group CreateIntLBQS-rg \
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
    --resource-group CreateIntLBQS-rg \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe \
    --disable-outbound-snat true \
    --idle-timeout 15 \
    --enable-tcp-reset true
```
>[!NOTE]
>Die virtuellen Computer im Back-End-Pool verfügen mit dieser Konfiguration nicht über ausgehende Internetkonnektivität. </br> Weitere Informationen zu ausgehender Konnektivität finden Sie unter: </br> **[Ausgehende Verbindungen in Azure](load-balancer-outbound-connections.md)**</br> Optionen zum Bereitstellen von Konnektivität: </br> **[Lastenausgleichskonfiguration (nur ausgehender Datenverkehr)](egress-only.md)** </br> **[Was ist Virtual Network NAT?](../virtual-network/nat-overview.md)**

### <a name="add-virtual-machines-to-load-balancer-backend-pool"></a>Hinzufügen virtueller Computer zum Back-End-Pool des Lastenausgleichs

Fügen Sie mit [az network nic ip-config address-pool add](/cli/azure/network/nic/ip-config/address-pool#az-network-nic-ip-config-address-pool-add) die virtuellen Computer zum Back-End-Pool hinzu:

* Back-End-Adresspool: **myBackEndPool**
* In Ressourcengruppe **CreateIntLBQS-rg**.
* Zugeordnet zu den Netzwerkschnittstellen **myNicVM1**, **myNicVM2** und **myNicVM3**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**

```azurecli-interactive
  array=(VM1 VM2 VM3)
  for vm in "${array[@]}"
  do
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNic$vm \
   --resource-group CreateIntLBQS-rg \
   --lb-name myLoadBalancer
  done

```

# <a name="basic-sku"></a>[**Basic-SKU**](#tab/option-1-create-load-balancer-basic)

>[!NOTE]
>Ein Lastenausgleich mit Standard-SKU wird für Produktionsworkloads empfohlen. Weitere Informationen zu SKUs finden Sie unter **[Azure Load Balancer-SKUs](skus.md)** .

:::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/resources-diagram-internal-basic.png" alt-text="Im Schnellstart erstellte Load Balancer Basic-Ressourcen." border="false":::

## <a name="configure-virtual-network---basic"></a>Konfigurieren des virtuellen Netzwerks: Basic

Erstellen Sie zunächst die unterstützenden virtuellen Netzwerkressourcen, bevor Sie virtuelle Computer und Ihren Lastenausgleich bereitstellen.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet#az-network-vnet-createt) ein virtuelles Netzwerk:

* Name: **myVNet**
* Adresspräfix: **10.1.0.0/16**
* Subnetzname: **myBackendSubnet**
* Subnetzpräfix: **10.1.0.0/24**
* In der Ressourcengruppe **CreateIntLBQS-rg**.
* Standort: **eastus**

```azurecli-interactive
  az network vnet create \
    --resource-group CreateIntLBQS-rg \
    --location eastus \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name myBackendSubnet \
    --subnet-prefixes 10.1.0.0/24
```

### <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

Verwenden Sie [az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create), um eine öffentliche IP-Adresse für den Bastionhost zu erstellen:

* Erstellen einer standardmäßigen zonenredundanten öffentlichen IP-Adresse namens **myBastionIP**
* In **CreateIntLBQS-rg**.

```azurecli-interactive
az network public-ip create \
    --resource-group CreateIntLBQS-rg \
    --name myBastionIP \
    --sku Standard
```
### <a name="create-a-bastion-subnet"></a>Erstellen eines Bastionsubnetzes

Erstellen Sie mit [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) ein Bastionsubnetz:

* Name: **AzureBastionSubnet**
* Adresspräfix: **10.1.1.0/24**
* Virtuelles Netzwerk: **myVNet**
* In Ressourcengruppe **CreateIntLBQS-rg**.

```azurecli-interactive
az network vnet subnet create \
    --resource-group CreateIntLBQS-rg \
    --name AzureBastionSubnet \
    --vnet-name myVNet \
    --address-prefixes 10.1.1.0/24
```

### <a name="create-bastion-host"></a>Erstellen eines Bastionhosts

Erstellen Sie mit [az network bastion create](/cli/azure/network/bastion#az-network-bastion-create) einen Bastionhost:

* Name: **myBastionHost**
* In **CreateIntLBQS-rg**.
* Der öffentlichen IP-Adresse **myBastionIP** zugeordnet
* Dem virtuellen Netzwerk **myVNet** zugeordnet
* Standort: **eastus**

```azurecli-interactive
az network bastion create \
    --resource-group CreateIntLBQS-rg \
    --name myBastionHost \
    --public-ip-address myBastionIP \
    --vnet-name myVNet \
    --location eastus
```

Die Bereitstellung des Azure Bastion-Hosts kann einige Minuten dauern.

### <a name="create-a-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe

Für einen Standard-Lastenausgleich müssen die virtuellen Computer unter der Back-End-Adresse über Netzwerkschnittstellen verfügen, die einer Netzwerksicherheitsgruppe angehören. 

Erstellen Sie mit [az network nsg create](/cli/azure/network/nsg#az-network-nsg-create) eine Netzwerksicherheitsgruppe:

* Name: **myNSG**
* In Ressourcengruppe **CreateIntLBQS-rg**.

```azurecli-interactive
  az network nsg create \
    --resource-group CreateIntLBQS-rg \
    --name myNSG
```

### <a name="create-a-network-security-group-rule"></a>Erstellen einer Netzwerksicherheitsgruppen-Regel

Erstellen Sie mit [az network nsg rule create](/cli/azure/network/nsg/rule#az-network-nsg-rule-create) eine Netzwerksicherheitsgruppen-Regel:

* Name: **myNSGRuleHTTP**
* Netzwerksicherheitsgruppe: **myNSG** (aus dem vorherigen Schritt)
* In Ressourcengruppe **CreateIntLBQS-rg**.
* Protokoll **(*)**
* Richtung: **Eingehend**
* Quelle: **(*)**
* Ziel: **(*)**
* Zielport: **Port 80**
* Zugriff: **Zulassen**
* Priorität: **200**

```azurecli-interactive
  az network nsg rule create \
    --resource-group CreateIntLBQS-rg \
    --nsg-name myNSG \
    --name myNSGRuleHTTP \
    --protocol '*' \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 200
```

## <a name="create-backend-servers---basic"></a>Erstellen von Back-End-Servern: Basic

In diesem Abschnitt wird Folgendes erstellt:

* Drei Netzwerkschnittstellen für die virtuellen Computer
* Verfügbarkeitsgruppe für die virtuellen Computer
* Drei virtuelle Computer als Back-End-Server für den Lastenausgleich

### <a name="create-network-interfaces-for-the-virtual-machines"></a>Erstellen von Netzwerkschnittstellen für die virtuellen Computer

Erstellen Sie mit [az network nic create](/cli/azure/network/nic#az-network-nic-create) drei Netzwerkschnittstellen:

* Namen: **myNicVM1**, **myNicVM2** und **myNicVM3**
* In Ressourcengruppe **CreateIntLBQS-rg**.
* Virtuelles Netzwerk: **myVNet**
* Subnetz: **myBackendSubnet**
* Netzwerksicherheitsgruppe: **myNSG**

```azurecli-interactive
  array=(myNicVM1 myNicVM2 myNicVM3)
  for vmnic in "${array[@]}"
  do
    az network nic create \
        --resource-group CreateIntLBQS-rg \
        --name $vmnic \
        --vnet-name myVNet \
        --subnet myBackEndSubnet \
        --network-security-group myNSG
  done
```

### <a name="create-availability-set-for-virtual-machines"></a>Erstellen einer Verfügbarkeitsgruppe für virtuelle Computer

Erstellen Sie mit [az vm availability-set create](/cli/azure/vm/availability-set#az-vm-availability-set-create) die Verfügbarkeitsgruppe:

* **myAvailabilitySet**.
* In Ressourcengruppe **CreateIntLBQS-rg**.
* Standort: **eastus**

```azurecli-interactive
  az vm availability-set create \
    --name myAvailabilitySet \
    --resource-group CreateIntLBQS-rg \
    --location eastus 
    
```

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie mit [az vm create](/cli/azure/vm#az-vm-create) die virtuellen Computer:

* **myVM1**, **myVM2** und **myVM3**.
* In Ressourcengruppe **CreateIntLBQS-rg**.
* Angefügt an die Netzwerkschnittstellen **myNicVM1**, **myNicVM2** und **myNicVM3**.
* Image des virtuellen Computers: **win2019datacenter**
* In **myAvailabilitySet**


```azurecli-interactive
  array=(1 2 3)
  for n in "${array[@]}"
  do
    az vm create \
    --resource-group CreateIntLBQS-rg \
    --name myVM$n \
    --nics myNicVM$n \
    --image win2019datacenter \
    --admin-username azureuser \
    --availability-set myAvailabilitySet \
    --no-wait
  done
```
Die Bereitstellung der virtuellen Computer kann eine Weile dauern.

## <a name="create-basic-load-balancer"></a>Erstellen eines Basic-Lastenausgleichs

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
* Dem Back-End-Subnetz **myBackendSubnet** zugeordnet

```azurecli-interactive
  az network lb create \
    --resource-group CreateIntLBQS-rg \
    --name myLoadBalancer \
    --sku Basic \
    --vnet-name myVNet \
    --subnet myBackendSubnet \
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
    --resource-group CreateIntLBQS-rg \
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

```azurecli-interactive
  az network lb rule create \
    --resource-group CreateIntLBQS-rg \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe \
    --idle-timeout 15 
```
### <a name="add-virtual-machines-to-load-balancer-backend-pool"></a>Hinzufügen virtueller Computer zum Back-End-Pool des Lastenausgleichs

Fügen Sie mit [az network nic ip-config address-pool add](/cli/azure/network/nic/ip-config/address-pool#az-network-nic-ip-config-address-pool-add) die virtuellen Computer zum Back-End-Pool hinzu:

* Back-End-Adresspool: **myBackEndPool**
* In Ressourcengruppe **CreateIntLBQS-rg**.
* Zugeordnet zu den Netzwerkschnittstellen **myNicVM1**, **myNicVM2** und **myNicVM3**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**

```azurecli-interactive
  array=(VM1 VM2 VM3)
  for vm in "${array[@]}"
  do
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNic$vm \
   --resource-group CreateIntLBQS-rg \
   --lb-name myLoadBalancer
  done

```
---

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs

### <a name="create-test-virtual-machine"></a>Erstellen eines virtuellen Testcomputers

Erstellen Sie die Netzwerkschnittstelle mit [az network nic create](/cli/azure/network/nic#az-network-nic-create):

* Name: **myNicTestVM**
* In Ressourcengruppe **CreateIntLBQS-rg**.
* Virtuelles Netzwerk: **myVNet**
* Subnetz: **myBackendSubnet**
* Netzwerksicherheitsgruppe: **myNSG**

```azurecli-interactive
  az network nic create \
    --resource-group CreateIntLBQS-rg \
    --name myNicTestVM \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
Erstellen Sie den virtuellen Computer mit [az vm create](/cli/azure/vm#az-vm-create):

* Name: **myTestVM**
* In Ressourcengruppe **CreateIntLBQS-rg**.
* Angefügt an die Netzwerkschnittstelle **myNicTestVM**
* Image des virtuellen Computers: **Win2019Datacenter**

```azurecli-interactive
  az vm create \
    --resource-group CreateIntLBQS-rg \
    --name myTestVM \
    --nics myNicTestVM \
    --image Win2019Datacenter \
    --admin-username azureuser \
    --no-wait
```
Es kann einige Minuten dauern, bis der virtuelle Computer bereitgestellt ist.

## <a name="install-iis"></a>Installieren von IIS

Verwenden Sie [az vm extension set](/cli/azure/vm/extension#az_vm_extension_set), um IIS auf den virtuellen Computern zu installieren, und legen Sie die Standardwebsite auf den Computernamen fest.

```azurecli-interactive
  array=(myVM1 myVM2 myVM3)
    for vm in "${array[@]}"
    do
     az vm extension set \
       --publisher Microsoft.Compute \
       --version 1.8 \
       --name CustomScriptExtension \
       --vm-name $vm \
       --resource-group CreateIntLBQS-rg \
       --settings '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}'
  done

```

### <a name="test"></a>Test

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie im Bildschirm **Übersicht** nach der privaten IP-Adresse für den Lastenausgleich. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend **myLoadBalancer** aus.

3. Notieren Sie sich in der **Übersicht** von **myLoadBalancer** die Adresse neben **Private IP-Adresse**, oder kopieren Sie sie.

4. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myTestVM** (in der Ressourcengruppe **CreateIntLBQS-rg**) aus.

5. Wählen Sie auf der Seite **Übersicht** die Option **Verbinden** und dann **Bastion** aus.

6. Geben Sie den Benutzernamen und das Kennwort ein, die Sie bei der VM-Erstellung verwendet haben.

7. Öffnen Sie **Internet Explorer** auf **myTestVM**.

8. Geben Sie die IP-Adresse aus dem vorherigen Schritt in die Adressleiste des Browsers ein. Die Standardseite des IIS-Webservers wird im Browser angezeigt.

    :::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/load-balancer-test.png" alt-text="Erstellen eines internen Standard-Lastenausgleichs" border="true":::
   
Um zu sehen, wie Datenverkehr durch den Lastenausgleich auf alle drei virtuellen Computer verteilt wird, können Sie die Standardseite des IIS-Webservers der einzelnen virtuellen Computer anpassen und dann über den Clientcomputer eine Aktualisierung Ihres Webbrowsers erzwingen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Verwenden Sie den Befehl [az group delete](/cli/azure/group#az-group-delete), um die Ressourcengruppe, den Lastenausgleich und alle zugehörigen Ressourcen zu entfernen, wenn Sie sie nicht mehr benötigen.

```azurecli-interactive
  az group delete \
    --name CreateIntLBQS-rg
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung gilt:

* Erstellen eines Standard-Lastenausgleichs oder eines öffentlichen Lastenausgleichs
* Anfügen von virtuellen Computern 
* Konfigurieren der Datenverkehrsregel für den Lastenausgleich sowie des Integritätstests
* Testen des Lastenausgleichs

Fahren Sie mit dem folgenden Artikel fort, um sich weiter über Azure Load Balancer zu informieren:
> [!div class="nextstepaction"]
> [Was versteht man unter Azure Load Balancer?](load-balancer-overview.md)