---
title: 'Erstellen eines virtuellen Netzwerks: Schnellstart – Azure CLI'
titlesuffix: Azure Virtual Network
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie über Azure CLI ein virtuelles Netzwerk erstellen. Ein virtuelles Netzwerk ermöglicht die Kommunikation der Azure-Ressourcen untereinander und mit dem Internet.
author: KumudD
ms.service: virtual-network
ms.topic: quickstart
ms.date: 03/06/2021
ms.author: kumud
ms.custom: devx-track-azurecli
ms.openlocfilehash: 0795404c2dc5377d60896863f6a088c4b2ffd1ad
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106060804"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-cli"></a>Schnellstart: Erstellen eines virtuellen Netzwerks über die Azure-Befehlszeilenschnittstelle

In einem virtuellen Netzwerk können Azure-Ressourcen wie virtuelle Computer (VMs) privat miteinander und mit dem Internet kommunizieren. 

In dieser Schnellstartanleitung erfahren Sie, wie Sie ein virtuelles Netzwerk erstellen. Nach dem Erstellen eines virtuellen Netzwerks stellen Sie zwei virtuelle Computer im virtuellen Netzwerk bereit. Anschließend stellen Sie über das Internet eine Verbindung mit den VMs her und kommunizieren privat über das neue virtuelle Netzwerk.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

- Für diesen Schnellstart ist mindestens Version 2.0.28 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="create-a-resource-group-and-a-virtual-network"></a>Erstellen einer Ressourcengruppe und eines virtuellen Netzwerks

Bevor Sie ein virtuelles Netzwerk erstellen können, müssen Sie eine Ressourcengruppe zum Hosten des virtuellen Netzwerks erstellen. Erstellen Sie mit [az group create](/cli/azure/group#az_group_create) eine Ressourcengruppe. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen **CreateVNetQS-rg** am Standort **eastus** (USA, Osten) erstellt:

```azurecli-interactive
az group create \
    --name CreateVNetQS-rg \
    --location eastus
```

Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) ein virtuelles Netzwerk. In diesem Beispiel wird ein virtuelles Standardnetzwerk namens **myVNet** mit einem einzelnen Subnetz namens **default** (Standard) erstellt:

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group CreateVNetQS-rg \
  --subnet-name default
```

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk.

### <a name="create-the-first-vm"></a>Erstellen des ersten virtuellen Computers

Erstellen Sie mit [az vm create](/cli/azure/vm#az_vm_create) einen virtuellen Computer. 

Wenn SSH-Schlüssel nicht bereits an einem Standardschlüsselspeicherort vorhanden sind, werden sie durch den Befehl erstellt. Um einen bestimmten Satz von Schlüsseln zu verwenden, nutzen Sie die Option `--ssh-key-value`. 

Die `--no-wait`-Option erstellt den virtuellen Computer im Hintergrund. Sie können mit dem nächsten Schritt fortfahren. 

In diesem Beispiel wird ein virtueller Computer namens **myVM1** erstellt:

```azurecli-interactive
az vm create \
  --resource-group CreateVNetQS-rg \
  --name myVM1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --public-ip-address myPublicIP-myVM1 \
  --no-wait
```

### <a name="create-the-second-vm"></a>Erstellen des zweiten virtuellen Computers

Im vorherigen Schritt haben Sie die `--no-wait`-Option verwendet. Sie können fortfahren, und die zweite VM namens **myVM2** erstellen.

```azurecli-interactive
az vm create \
  --resource-group CreateVNetQS-rg \
  --name myVM2 \
  --image UbuntuLTS \
  --public-ip-address myPublicIP-myVM2 \
  --generate-ssh-keys
```

### <a name="azure-cli-output-message"></a>Azure CLI-Ausgabenachricht

Die Erstellung der VMs kann einige Minuten dauern. Nachdem Azure die virtuellen Computer erstellt hat, gibt Azure CLI etwa folgende Ausgabe zurück:

```output
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/CreateVNetQS-rg/providers/Microsoft.Compute/virtualMachines/myVM2",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "CreateVNetQS-rg"
  "zones": ""
}
```

## <a name="vm-public-ip"></a>Öffentliche IP-Adresse für die VM

Verwenden Sie [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show), um die öffentliche IP-Adresse **myVM2** abzurufen.

```azurecli-interactive
az network public-ip show \
  --resource-group CreateVNetQS-rg  \
  --name myPublicIP-myVM2 \
  --query ipAddress \
  --output tsv
```

## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Ersetzen Sie in diesem Befehl `<publicIpAddress>` durch die öffentliche IP-Adresse Ihres virtuellen Computers **myVM2**:

```bash
ssh <publicIpAddress>
```

## <a name="communicate-between-vms"></a>Kommunikation zwischen VMs

Wenn Sie die private Kommunikation zwischen den virtuellen Computern **myVM2** und **myVM1** überprüfen möchten, geben Sie diesen Befehl ein:

```bash
ping myVM1 -c 4
```

Sie erhalten vier Antworten von *10.0.0.4*.

Beenden Sie die SSH-Sitzung mit dem virtuellen Computer **myVM2**.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe und alle enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit [az group delete](/cli/azure/group#az_group_delete) entfernen:

```azurecli-interactive
az group delete \
    --name CreateVNetQS-rg \
    --yes
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung gilt: 

* Sie haben ein virtuelles Standardnetzwerk und zwei virtuelle Computer erstellt. 
* Sie haben über das Internet eine Verbindung mit einem der virtuellen Computer hergestellt und privat zwischen den beiden virtuellen Computern kommuniziert.

Die private Kommunikation zwischen VMs ist in einem virtuellen Netzwerk uneingeschränkt. 

Fahren Sie mit dem nächsten Artikel fort, um mehr über das Konfigurieren verschiedener Typen der VM-Netzwerkkommunikation zu erfahren:
> [!div class="nextstepaction"]
> [Erstellen von Netzwerksicherheitsgruppen über das Azure-Portal](tutorial-filter-network-traffic.md)
