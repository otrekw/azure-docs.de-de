---
title: 'Schnellstart: Erstellen eines öffentlichen Lastenausgleichs: Azure CLI'
titleSuffix: Azure Load Balancer
description: In dieser Schnellstartanleitung wird veranschaulicht, wie Sie über die Azure-Befehlszeilenschnittstelle einen öffentlichen Lastenausgleich erstellen.
services: load-balancer
documentationcenter: na
author: asudbring
manager: KumudD
tags: azure-resource-manager
Customer intent: I want to create a load balancer so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/23/2020
ms.author: allensu
ms.custom: mvc, devx-track-js, devx-track-azurecli
ms.openlocfilehash: 0b6973f50e4d51c1c94e262a08681ec0431f511e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91333981"
---
# <a name="quickstart-create-a-public-load-balancer-to-load-balance-vms-using-azure-cli"></a>Schnellstart: Erstellen eines öffentlichen Lastenausgleichs für virtuelle Computer mithilfe der Azure CLI

Beginnen Sie mit der Verwendung von Azure Load Balancer, indem Sie mithilfe der Azure CLI einen öffentlichen Lastenausgleich und drei virtuelle Computer erstellen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Azure CLI (lokal installiert) oder Azure Cloud Shell

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diese Schnellstartanleitung mindestens die Azure CLI-Version 2.0.28 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mit [az group create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) eine Ressourcengruppe:

* Name: **myResourceGroupLB** 
* Standort: **eastus**

```azurecli-interactive
  az group create \
    --name myResourceGroupLB \
    --location eastus
```
---

# <a name="standard-sku"></a>[**Standard-SKU**](#tab/option-1-create-load-balancer-standard)

>[!NOTE]
>Ein Lastenausgleich mit Standard-SKU wird für Produktionsworkloads empfohlen. Weitere Informationen zu SKUs finden Sie unter **[Azure Load Balancer-SKUs](skus.md)** .

## <a name="configure-virtual-network"></a>Konfigurieren eines virtuellen Netzwerks

Erstellen Sie zunächst die unterstützenden virtuellen Netzwerkressourcen, bevor Sie virtuelle Computer bereitstellen und Ihren Lastenausgleich testen.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie mit [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-createt) ein virtuelles Netzwerk:

* Name: **myVNet**
* Adresspräfix: **10.1.0.0/16**
* Subnetzname: **myBackendSubnet**
* Subnetzpräfix: **10.1.0.0/24**
* Ressourcengruppe: **myResourceGroupLB**
* Standort: **eastus**

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroupLB \
    --location eastus \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name myBackendSubnet \
    --subnet-prefixes 10.1.0.0/24
```

### <a name="create-a-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe

Für einen Standard-Lastenausgleich müssen die virtuellen Computer unter der Back-End-Adresse über Netzwerkschnittstellen verfügen, die einer Netzwerksicherheitsgruppe angehören. 

Erstellen Sie mit [az network nsg create](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create) eine Netzwerksicherheitsgruppe:

* Name: **myNSG**
* Ressourcengruppe: **myResourceGroupLB**

```azurecli-interactive
  az network nsg create \
    --resource-group myResourceGroupLB \
    --name myNSG
```

### <a name="create-a-network-security-group-rule"></a>Erstellen einer Netzwerksicherheitsgruppen-Regel

Erstellen Sie mit [az network nsg rule create](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create) eine Netzwerksicherheitsgruppen-Regel:

* Name: **myNSGRuleHTTP**
* Netzwerksicherheitsgruppe: **myNSG** (aus dem vorherigen Schritt)
* Ressourcengruppe: **myResourceGroupLB**
* Protokoll **(*)**
* Richtung: **Eingehend**
* Quelle: **(*)**
* Ziel: **(*)**
* Zielport: **Port 80**
* Zugriff: **Zulassen**
* Priorität: **200**

```azurecli-interactive
  az network nsg rule create \
    --resource-group myResourceGroupLB \
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

### <a name="create-network-interfaces-for-the-virtual-machines"></a>Erstellen von Netzwerkschnittstellen für die virtuellen Computer

Erstellen Sie mit [az network nic create](https://docs.microsoft.com/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) drei Netzwerkschnittstellen:

#### <a name="vm1"></a>VM1

* Name: **myNicVM1**
* Ressourcengruppe: **myResourceGroupLB**
* Virtuelles Netzwerk: **myVNet**
* Subnetz: **myBackendSubnet**
* Netzwerksicherheitsgruppe: **myNSG**

```azurecli-interactive

  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM1 \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
#### <a name="vm2"></a>VM2

* Name: **myNicVM2**
* Ressourcengruppe: **myResourceGroupLB**
* Virtuelles Netzwerk: **myVNet**
* Subnetz: **myBackendSubnet**

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM2 \
    --vnet-name myVnet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
#### <a name="vm3"></a>VM3

* Name: **myNicVM3**
* Ressourcengruppe: **myResourceGroupLB**
* Virtuelles Netzwerk: **myVNet**
* Subnetz: **myBackendSubnet**
* Netzwerksicherheitsgruppe: **myNSG**

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM3 \
    --vnet-name myVnet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```

## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Abschnitt wird Folgendes erstellt:

* Eine Cloudkonfigurationsdatei namens **cloud-init.txt** für die Serverkonfiguration
* Drei virtuelle Computer als Back-End-Server für den Lastenausgleich

### <a name="create-cloud-init-configuration-file"></a>Erstellen der Konfigurationsdatei „cloud-init“

Verwenden Sie eine Konfigurationsdatei namens „cloud-init“, um NGINX zu installieren und eine Node.js-App namens „Hello World“ auf einem virtuellen Linux-Computer auszuführen. 

Erstellen Sie in Ihrer aktuellen Shell eine Datei mit dem Namen „cloud-init.txt“. Kopieren Sie die folgende Konfiguration, und fügen Sie sie in die Shell ein. Achten Sie darauf, die gesamte Datei „cloud-init“ ordnungsgemäß zu kopieren – insbesondere die erste Zeile:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```
### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie mit [az vm create](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) die virtuellen Computer:

#### <a name="vm1"></a>VM1
* Name: **myVM1**
* Ressourcengruppe: **myResourceGroupLB**
* Angefügt an die Netzwerkschnittstelle **myNicVM1**
* VM-Image: **UbuntuLTS**
* Konfigurationsdatei: **cloud-init.txt** (aus dem vorherigen Schritt)
* Zone: **Zone 1**

```azurecli-interactive
  az vm create \
    --resource-group myResourceGroupLB \
    --name myVM1 \
    --nics myNicVM1 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --zone 1 \
    --no-wait
    
```
#### <a name="vm2"></a>VM2
* Name: **myVM2**
* Ressourcengruppe: **myResourceGroupLB**
* Angefügt an die Netzwerkschnittstelle **myNicVM2**
* VM-Image: **UbuntuLTS**
* Konfigurationsdatei: **cloud-init.txt** (aus dem vorherigen Schritt)
* Zone: **Zone 2**

```azurecli-interactive
  az vm create \
    --resource-group myResourceGroupLB \
    --name myVM2 \
    --nics myNicVM2 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --zone 2 \
    --no-wait
```

#### <a name="vm3"></a>VM3
* Name **myVM3**
* Ressourcengruppe: **myResourceGroupLB**
* Angefügt an die Netzwerkschnittstelle **myNicVM3**
* VM-Image: **UbuntuLTS**
* Konfigurationsdatei: **cloud-init.txt** (aus dem vorherigen Schritt)
* Zone: **Zone 3**

```azurecli-interactive
   az vm create \
    --resource-group myResourceGroupLB \
    --name myVM3 \
    --nics myNicVM3 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --zone 3 \
    --no-wait
```
Die Bereitstellung der virtuellen Computer kann eine Weile dauern.

## <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

Um über das Internet auf Ihre Web-App zugreifen zu können, benötigen Sie eine öffentliche IP-Adresse für den Lastenausgleich. 

Verwenden Sie [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) für Folgendes:

* Erstellen einer standardmäßigen zonenredundanten öffentlichen IP-Adresse namens **myPublicIP**
* Ressourcengruppe: **myResourceGroupLB**

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myPublicIP \
    --sku Standard
```

So erstellen Sie eine zonenredundante öffentliche IP-Adresse in Zone 1:

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myPublicIP \
    --sku Standard \
    --zone 1
```

## <a name="create-standard-load-balancer"></a>Erstellen eines Standard-Lastenausgleichs

In diesem Abschnitt erfahren Sie, wie Sie die folgenden Komponenten des Lastenausgleichs erstellen und konfigurieren:

  * Front-End-IP-Pool, der den eingehenden Netzwerkdatenverkehr für den Lastenausgleich empfängt
  * Back-End-IP-Pool, an den der Front-End-Pool den Netzwerkdatenverkehr sendet, für den ein Lastenausgleich durchgeführt wurde
  * Integritätstest zum Ermitteln der Integrität der Back-End-VM-Instanzen
  * Lastenausgleichsregel, mit der definiert wird, wie Datenverkehr auf die virtuellen Computer verteilt werden soll

### <a name="create-the-load-balancer-resource"></a>Erstellen der Lastenausgleichsressource

Erstellen Sie mit [az network lb create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest#az-network-lb-create) einen öffentlichen Lastenausgleich:

* Name: **myLoadBalancer**
* Front-End-Pool namens **myFrontEnd**
* Back-End-Pool namens **myBackEndPool**
* Zugeordnet zur öffentlichen IP-Adresse **myPublicIP**, die im vorherigen Schritt erstellt wurde 

```azurecli-interactive
  az network lb create \
    --resource-group myResourceGroupLB \
    --name myLoadBalancer \
    --sku Standard \
    --public-ip-address myPublicIP \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool       
```

### <a name="create-the-health-probe"></a>Erstellen des Integritätstests

Von einem Integritätstest werden alle VM-Instanzen überprüft, um sicherzustellen, dass sie Netzwerkdatenverkehr senden können. 

Ist der Test bei einem virtuellen Computer nicht erfolgreich, wird er aus dem Lastenausgleich entfernt. Nach Behebung des Fehlers wird der virtuelle Computer dem Lastenausgleich wieder hinzugefügt.

Erstellen Sie mit [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest#az-network-lb-probe-create) einen Integritätstest:

* Überwacht die Integrität der virtuellen Computer
* Name: **myHealthProbe**
* Protokoll: **TCP**
* Überwachter Port: **80**

```azurecli-interactive
  az network lb probe create \
    --resource-group myResourceGroupLB \
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

Erstellen Sie mit [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create) eine Lastenausgleichsregel:

* Name: **myHTTPRule**
* Lauscht am **Port 80** im Front-End-Pool **myFrontEnd**
* Sendet Netzwerkdatenverkehr, für den ein Lastenausgleich durchgeführt wurde, an den Back-End-Adresspool **myBackEndPool** unter Verwendung von **Port 80** 
* Verwendet den Integritätstest **myHealthProbe**
* Protokoll: **TCP**
* Aktivieren Sie die ausgehende Übersetzung der Quellnetzwerkadresse (Source Network Address Translation, SNAT) unter Verwendung der Front-End-IP-Adresse.

```azurecli-interactive
  az network lb rule create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe \
    --disable-outbound-snat true 
```
### <a name="add-virtual-machines-to-load-balancer-backend-pool"></a>Hinzufügen virtueller Computer zum Back-End-Pool des Lastenausgleichs

Fügen Sie mit [az network nic ip-config address-pool add](https://docs.microsoft.com/cli/azure/network/nic/ip-config/address-pool?view=azure-cli-latest#az-network-nic-ip-config-address-pool-add) die virtuellen Computer zum Back-End-Pool hinzu:

#### <a name="vm1"></a>VM1
* Back-End-Adresspool: **myBackEndPool**
* Ressourcengruppe: **myResourceGroupLB**
* Zugeordnet zur Netzwerkschnittstelle **myNicVM1** und zu **ipconfig1**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM1 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm2"></a>VM2
* Back-End-Adresspool: **myBackEndPool**
* Ressourcengruppe: **myResourceGroupLB**
* Zugeordnet zur Netzwerkschnittstelle **myNicVM2** und zu **ipconfig1**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM2 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm3"></a>VM3
* Back-End-Adresspool: **myBackEndPool**
* Ressourcengruppe: **myResourceGroupLB**
* Zugeordnet zur Netzwerkschnittstelle **myNicVM3** und zu **ipconfig1**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM3 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

## <a name="create-outbound-rule-configuration"></a>Erstellen der Konfiguration für Ausgangsregeln
Ausgangsregeln für den Lastenausgleich konfigurieren das ausgehende SNAT für virtuelle Computer im Back-End-Pool. 

Weitere Informationen zu ausgehenden Verbindungen finden Sie unter [Ausgehende Verbindungen in Azure](load-balancer-outbound-connections.md).

### <a name="create-outbound-public-ip-address-or-public-ip-prefix"></a>Erstellen einer ausgehenden öffentlichen IP-Adresse oder eines öffentlichen IP-Präfix

Verwenden Sie [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create), um eine einzelne IP-Adresse für die ausgehende Konnektivität zu erstellen.  

Verwenden Sie [az network public-ip prefix create](https://docs.microsoft.com/cli/azure/network/public-ip/prefix?view=azure-cli-latest#az-network-public-ip-prefix-create), um ein öffentliches IP-Präfix für die ausgehende Konnektivität zu erstellen.

Weitere Informationen zum Skalieren der NAT für ausgehenden Datenverkehr und der ausgehenden Konnektivität finden Sie unter [Skalieren der NAT für ausgehenden Datenverkehr mit mehreren IP-Adressen](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#scale).

#### <a name="public-ip"></a>Öffentliche IP-Adresse

* Name: **myPublicIPOutbound**
* Ressourcengruppe: **myResourceGroupLB**

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myPublicIPOutbound \
    --sku Standard
```

So erstellen Sie eine zonenredundante öffentliche IP-Adresse in Zone 1:

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myPublicIPOutbound \
    --sku Standard \
    --zone 1
```
#### <a name="public-ip-prefix"></a>Präfix für öffentliche IP-Adressen

* Name: **myPublicIPPrefixOutbound**
* Ressourcengruppe: **myResourceGroupLB**
* Präfixlänge: **28**

```azurecli-interactive
  az network public-ip prefix create \
    --resource-group myResourceGroupLB \
    --name myPublicIPPrefixOutbound \
    --length 28
```
So erstellen Sie ein zonenredundantes öffentliche IP-Präfix in Zone 1:

```azurecli-interactive
  az network public-ip prefix create \
    --resource-group myResourceGroupLB \
    --name myPublicIPPrefixOutbound \
    --length 28 \
    --zone 1
```

### <a name="create-outbound-frontend-ip-configuration"></a>Erstellen der ausgehenden Front-End-IP-Konfiguration

Erstellen Sie mit [az network lb frontend-ip create](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip?view=azure-cli-latest#az-network-lb-frontend-ip-create) eine neue Front-End-IP-Konfiguration:

Verwenden Sie abhängig von Ihrer Entscheidung im vorherigen Schritt die Befehle für die öffentliche IP-Adresse oder die Befehle für das öffentliche IP-Präfix.

#### <a name="public-ip"></a>Öffentliche IP-Adresse

* Name: **myFrontEndOutbound**
* Ressourcengruppe: **myResourceGroupLB**
* Zugeordnet zur öffentlichen IP-Adresse **myPublicIPOutbound**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**

```azurecli-interactive
  az network lb frontend-ip create \
    --resource-group myResourceGroupLB \
    --name myFrontEndOutbound \
    --lb-name myLoadBalancer \
    --public-ip-address myPublicIPOutbound 
```

#### <a name="public-ip-prefix"></a>Präfix für öffentliche IP-Adressen

* Name: **myFrontEndOutbound**
* Ressourcengruppe: **myResourceGroupLB**
* Zugeordnet zum öffentlichen IP-Präfix **myPublicIPPrefixOutbound**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**

```azurecli-interactive
  az network lb frontend-ip create \
    --resource-group myResourceGroupLB \
    --name myFrontEndOutbound \
    --lb-name myLoadBalancer \
    --public-ip-prefix myPublicIPPrefixOutbound 
```

### <a name="create-outbound-pool"></a>Erstellen eines ausgehenden Pools

Erstellen Sie mit [az network lb address-pool create](https://docs.microsoft.com/cli/azure/network/lb/address-pool?view=azure-cli-latest#az-network-lb-address-pool-create) einen neuen ausgehenden Pool:

* Name: **myBackEndPoolOutbound**
* Ressourcengruppe: **myResourceGroupLB**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**

```azurecli-interactive
  az network lb address-pool create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myBackendPoolOutbound
```
### <a name="create-outbound-rule"></a>Erstellen einer Ausgangsregel

Erstellen Sie mit [az network lb outbound-rule create](https://docs.microsoft.com/cli/azure/network/lb/outbound-rule?view=azure-cli-latest#az-network-lb-outbound-rule-create) eine neue Ausgangsregel für den ausgehenden Back-End-Pool:

* Name: **myOutboundRule**
* Ressourcengruppe: **myResourceGroupLB**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**
* Zugeordnet zum Front-End **myFrontEndOutbound**
* Protokoll: **Alle**
* Leerlauftimeout: **15**
* Ausgehende Ports: **10000**
* Zugeordnet zum Back-End-Pool **myBackEndPoolOutbound**

```azurecli-interactive
  az network lb outbound-rule create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myOutboundRule \
    --frontend-ip-configs myFrontEndOutbound \
    --protocol All \
    --idle-timeout 15 \
    --outbound-ports 10000 \
    --address-pool myBackEndPoolOutbound
```
### <a name="add-virtual-machines-to-outbound-pool"></a>Hinzufügen virtueller Computer zum ausgehenden Pool

Fügen Sie mit [az network nic ip-config address-pool add](https://docs.microsoft.com/cli/azure/network/nic/ip-config/address-pool?view=azure-cli-latest#az-network-nic-ip-config-address-pool-add) die virtuellen Computer dem ausgehenden Pool hinzu:


#### <a name="vm1"></a>VM1
* Back-End-Adresspool: **myBackEndPoolOutbound**
* Ressourcengruppe: **myResourceGroupLB**
* Zugeordnet zur Netzwerkschnittstelle **myNicVM1** und zu **ipconfig1**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPoolOutbound \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM1 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm2"></a>VM2
* Back-End-Adresspool: **myBackEndPoolOutbound**
* Ressourcengruppe: **myResourceGroupLB**
* Zugeordnet zur Netzwerkschnittstelle **myNicVM2** und zu **ipconfig1**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPoolOutbound \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM2 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm3"></a>VM3
* Back-End-Adresspool: **myBackEndPoolOutbound**
* Ressourcengruppe: **myResourceGroupLB**
* Zugeordnet zur Netzwerkschnittstelle **myNicVM3** und zu **ipconfig1**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPoolOutbound \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM3 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

# <a name="basic-sku"></a>[**Basic-SKU**](#tab/option-1-create-load-balancer-basic)

>[!NOTE]
>Ein Lastenausgleich mit Standard-SKU wird für Produktionsworkloads empfohlen. Weitere Informationen zu SKUs finden Sie unter **[Azure Load Balancer-SKUs](skus.md)** .

## <a name="configure-virtual-network"></a>Konfigurieren eines virtuellen Netzwerks

Erstellen Sie zunächst die unterstützenden virtuellen Netzwerkressourcen, bevor Sie virtuelle Computer bereitstellen und Ihren Lastenausgleich testen.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie mit [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-createt) ein virtuelles Netzwerk:

* Name: **myVNet**
* Adresspräfix: **10.1.0.0/16**
* Subnetzname: **myBackendSubnet**
* Subnetzpräfix: **10.1.0.0/24**
* Ressourcengruppe: **myResourceGroupLB**
* Standort: **eastus**

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroupLB \
    --location eastus \
    --name myVNet \
    --address-prefixes 10.1.0.0/16 \
    --subnet-name myBackendSubnet \
    --subnet-prefixes 10.1.0.0/24
```

### <a name="create-a-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe

Für einen Standard-Lastenausgleich müssen die virtuellen Computer unter der Back-End-Adresse über Netzwerkschnittstellen verfügen, die einer Netzwerksicherheitsgruppe angehören. 

Erstellen Sie mit [az network nsg create](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create) eine Netzwerksicherheitsgruppe:

* Name: **myNSG**
* Ressourcengruppe: **myResourceGroupLB**

```azurecli-interactive
  az network nsg create \
    --resource-group myResourceGroupLB \
    --name myNSG
```

### <a name="create-a-network-security-group-rule"></a>Erstellen einer Netzwerksicherheitsgruppen-Regel

Erstellen Sie mit [az network nsg rule create](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create) eine Netzwerksicherheitsgruppen-Regel:

* Name: **myNSGRuleHTTP**
* Netzwerksicherheitsgruppe: **myNSG** (aus dem vorherigen Schritt)
* Ressourcengruppe: **myResourceGroupLB**
* Protokoll **(*)**
* Richtung: **Eingehend**
* Quelle: **(*)**
* Ziel: **(*)**
* Zielport: **Port 80**
* Zugriff: **Zulassen**
* Priorität: **200**

```azurecli-interactive
  az network nsg rule create \
    --resource-group myResourceGroupLB \
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

### <a name="create-network-interfaces-for-the-virtual-machines"></a>Erstellen von Netzwerkschnittstellen für die virtuellen Computer

Erstellen Sie mit [az network nic create](https://docs.microsoft.com/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) drei Netzwerkschnittstellen:

#### <a name="vm1"></a>VM1

* Name: **myNicVM1**
* Ressourcengruppe: **myResourceGroupLB**
* Virtuelles Netzwerk: **myVNet**
* Subnetz: **myBackendSubnet**
* Netzwerksicherheitsgruppe: **myNSG**

```azurecli-interactive

  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM1 \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
#### <a name="vm2"></a>VM2

* Name: **myNicVM2**
* Ressourcengruppe: **myResourceGroupLB**
* Virtuelles Netzwerk: **myVNet**
* Subnetz: **myBackendSubnet**
* Netzwerksicherheitsgruppe: **myNSG**

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM2 \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```
#### <a name="vm3"></a>VM3

* Name: **myNicVM3**
* Ressourcengruppe: **myResourceGroupLB**
* Virtuelles Netzwerk: **myVNet**
* Subnetz: **myBackendSubnet**
* Netzwerksicherheitsgruppe: **myNSG**

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNicVM3 \
    --vnet-name myVNet \
    --subnet myBackEndSubnet \
    --network-security-group myNSG
```

## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Abschnitt wird Folgendes erstellt:

* Eine Cloudkonfigurationsdatei namens **cloud-init.txt** für die Serverkonfiguration 
* Verfügbarkeitsgruppe für die virtuellen Computer
* Drei virtuelle Computer als Back-End-Server für den Lastenausgleich


Installieren Sie NGINX auf den virtuellen Computern, um zu überprüfen, ob der Lastenausgleich erfolgreich erstellt wurde.

### <a name="create-cloud-init-configuration-file"></a>Erstellen der Konfigurationsdatei „cloud-init“

Verwenden Sie eine Konfigurationsdatei namens „cloud-init“, um NGINX zu installieren und eine Node.js-App namens „Hello World“ auf einem virtuellen Linux-Computer auszuführen. 

Erstellen Sie in Ihrer aktuellen Shell eine Datei mit dem Namen „cloud-init.txt“. Kopieren Sie die folgende Konfiguration, und fügen Sie sie in die Shell ein. Achten Sie darauf, die gesamte Datei „cloud-init“ ordnungsgemäß zu kopieren – insbesondere die erste Zeile:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```
### <a name="create-availability-set-for-virtual-machines"></a>Erstellen einer Verfügbarkeitsgruppe für virtuelle Computer

Erstellen Sie mit [az vm availability-set create](https://docs.microsoft.com/cli/azure/vm/availability-set?view=azure-cli-latest#az-vm-availability-set-create) die Verfügbarkeitsgruppe:

* Name: **myAvSet**
* Ressourcengruppe: **myResourceGroupLB**
* Standort: **eastus**

```azurecli-interactive
  az vm availability-set create \
    --name myAvSet \
    --resource-group myResourceGroupLB \
    --location eastus 
    
```

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie mit [az vm create](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) die virtuellen Computer:

#### <a name="vm1"></a>VM1
* Name: **myVM1**
* Ressourcengruppe: **myResourceGroupLB**
* Angefügt an die Netzwerkschnittstelle **myNicVM1**
* VM-Image: **UbuntuLTS**
* Konfigurationsdatei: **cloud-init.txt** (aus dem vorherigen Schritt)
* Verfügbarkeitsgruppe: **myAvSet**

```azurecli-interactive
  az vm create \
    --resource-group myResourceGroupLB \
    --name myVM1 \
    --nics myNicVM1 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --availability-set myAvSet \
    --no-wait 
```
#### <a name="vm2"></a>VM2
* Name: **myVM2**
* Ressourcengruppe: **myResourceGroupLB**
* Angefügt an die Netzwerkschnittstelle **myNicVM2**
* VM-Image: **UbuntuLTS**
* Konfigurationsdatei: **cloud-init.txt** (aus dem vorherigen Schritt)
* Zone: **Zone 2**

```azurecli-interactive
  az vm create \
    --resource-group myResourceGroupLB \
    --name myVM2 \
    --nics myNicVM2 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --availability-set myAvSet  \
    --no-wait
```

#### <a name="vm3"></a>VM3
* Name **myVM3**
* Ressourcengruppe: **myResourceGroupLB**
* Angefügt an die Netzwerkschnittstelle **myNicVM3**
* VM-Image: **UbuntuLTS**
* Konfigurationsdatei: **cloud-init.txt** (aus dem vorherigen Schritt)
* Zone: **Zone 3**

```azurecli-interactive
   az vm create \
    --resource-group myResourceGroupLB \
    --name myVM3 \
    --nics myNicVM3 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --availability-set myAvSet  \
    --no-wait
```
Die Bereitstellung der virtuellen Computer kann eine Weile dauern.


## <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

Um über das Internet auf Ihre Web-App zugreifen zu können, benötigen Sie eine öffentliche IP-Adresse für den Lastenausgleich. 

Verwenden Sie [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) für Folgendes:

* Erstellen einer standardmäßigen zonenredundanten öffentlichen IP-Adresse namens **myPublicIP**
* Ressourcengruppe: **myResourceGroupLB**

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myPublicIP \
    --sku Basic
```

## <a name="create-basic-load-balancer"></a>Erstellen eines Basic-Lastenausgleichs

In diesem Abschnitt erfahren Sie, wie Sie die folgenden Komponenten des Lastenausgleichs erstellen und konfigurieren:

  * Front-End-IP-Pool, der den eingehenden Netzwerkdatenverkehr für den Lastenausgleich empfängt
  * Back-End-IP-Pool, an den der Front-End-Pool den Netzwerkdatenverkehr sendet, für den ein Lastenausgleich durchgeführt wurde
  * Integritätstest zum Ermitteln der Integrität der Back-End-VM-Instanzen
  * Lastenausgleichsregel, mit der definiert wird, wie Datenverkehr auf die virtuellen Computer verteilt werden soll

### <a name="create-the-load-balancer-resource"></a>Erstellen der Lastenausgleichsressource

Erstellen Sie mit [az network lb create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest#az-network-lb-create) einen öffentlichen Lastenausgleich:

* Name: **myLoadBalancer**
* Front-End-Pool namens **myFrontEnd**
* Back-End-Pool namens **myBackEndPool**
* Zugeordnet zur öffentlichen IP-Adresse **myPublicIP**, die im vorherigen Schritt erstellt wurde 

```azurecli-interactive
  az network lb create \
    --resource-group myResourceGroupLB \
    --name myLoadBalancer \
    --sku Basic \
    --public-ip-address myPublicIP \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool       
```

### <a name="create-the-health-probe"></a>Erstellen des Integritätstests

Von einem Integritätstest werden alle VM-Instanzen überprüft, um sicherzustellen, dass sie Netzwerkdatenverkehr senden können. 

Ist der Test bei einem virtuellen Computer nicht erfolgreich, wird er aus dem Lastenausgleich entfernt. Nach Behebung des Fehlers wird der virtuelle Computer dem Lastenausgleich wieder hinzugefügt.

Erstellen Sie mit [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest#az-network-lb-probe-create) einen Integritätstest:

* Überwacht die Integrität der virtuellen Computer
* Name: **myHealthProbe**
* Protokoll: **TCP**
* Überwachter Port: **80**

```azurecli-interactive
  az network lb probe create \
    --resource-group myResourceGroupLB \
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

Erstellen Sie mit [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create) eine Lastenausgleichsregel:

* Name: **myHTTPRule**
* Lauscht am **Port 80** im Front-End-Pool **myFrontEnd**
* Sendet Netzwerkdatenverkehr, für den ein Lastenausgleich durchgeführt wurde, an den Back-End-Adresspool **myBackEndPool** unter Verwendung von **Port 80** 
* Verwendet den Integritätstest **myHealthProbe**
* Protokoll: **TCP**

```azurecli-interactive
  az network lb rule create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```

### <a name="add-virtual-machines-to-load-balancer-backend-pool"></a>Hinzufügen virtueller Computer zum Back-End-Pool des Lastenausgleichs

Fügen Sie mit [az network nic ip-config address-pool add](https://docs.microsoft.com/cli/azure/network/nic/ip-config/address-pool?view=azure-cli-latest#az-network-nic-ip-config-address-pool-add) die virtuellen Computer zum Back-End-Pool hinzu:


#### <a name="vm1"></a>VM1
* Back-End-Adresspool: **myBackEndPool**
* Ressourcengruppe: **myResourceGroupLB**
* Zugeordnet zur Netzwerkschnittstelle **myNicVM1** und zu **ipconfig1**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM1 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm2"></a>VM2
* Back-End-Adresspool: **myBackEndPool**
* Ressourcengruppe: **myResourceGroupLB**
* Zugeordnet zur Netzwerkschnittstelle **myNicVM2** und zu **ipconfig1**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM2 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```

#### <a name="vm3"></a>VM3
* Back-End-Adresspool: **myBackEndPool**
* Ressourcengruppe: **myResourceGroupLB**
* Zugeordnet zur Netzwerkschnittstelle **myNicVM3** und zu **ipconfig1**
* Zugeordnet zum Lastenausgleich **myLoadBalancer**

```azurecli-interactive
  az network nic ip-config address-pool add \
   --address-pool myBackendPool \
   --ip-config-name ipconfig1 \
   --nic-name myNicVM3 \
   --resource-group myResourceGroupLB \
   --lb-name myLoadBalancer
```
---

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs

Rufen Sie mithilfe von [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-show) die öffentliche IP-Adresse des Lastenausgleichs ab. 

Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein.

```azurecli-interactive
  az network public-ip show \
    --resource-group myResourceGroupLB \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```
:::image type="content" source="./media/load-balancer-standard-public-cli/running-nodejs-app.png" alt-text="Testen des Lastenausgleichs" border="true":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Verwenden Sie den Befehl [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete), um die Ressourcengruppe, den Lastenausgleich und alle zugehörigen Ressourcen zu entfernen, wenn Sie sie nicht mehr benötigen.

```azurecli-interactive
  az group delete \
    --name myResourceGroupLB
```

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung wurde Folgendes vermittelt:

* Erstellen eines Standard-Lastenausgleichs oder eines öffentlichen Lastenausgleichs
* Anfügen von virtuellen Computern 
* Konfigurieren der Datenverkehrsregel für den Lastenausgleich sowie des Integritätstests
* Testen des Lastenausgleichs

Weitere Informationen zu Azure Load Balancer finden Sie unter [Was versteht man unter Azure Load Balancer?](load-balancer-overview.md) sowie unter [Häufig gestellte Fragen zu Load Balancer](load-balancer-faqs.md).

Weitere Informationen zu Load Balancer und Verfügbarkeitszonen finden Sie [hier](load-balancer-standard-availability-zones.md).
