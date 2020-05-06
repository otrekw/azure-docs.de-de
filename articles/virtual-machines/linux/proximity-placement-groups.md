---
title: Verwenden von Näherungsplatzierungsgruppen
description: Erfahren Sie mehr über das Erstellen und Verwenden von Näherungsplatzierungsgruppen für VMs in Azure.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: cynthn
ms.openlocfilehash: f89b28e7a3c29e45efa2796788e27325c01d7098
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81759253"
---
# <a name="deploy-vms-to-proximity-placement-groups-using-azure-cli"></a>Bereitstellen von VMs für Näherungsplatzierungsgruppen mit Azure CLI

Um den Abstand zwischen den VMs so stark wie möglich zu verringern und somit die geringstmögliche Latenz zu erzielen, sollten Sie sie in einer [Näherungsplatzierungsgruppe](co-location.md#proximity-placement-groups) bereitstellen.

Eine Näherungsplatzierungsgruppe ist eine logische Gruppierung, mit der ein möglichst geringer Abstand zwischen Azure-Compute-Ressourcen sichergestellt wird. Näherungsplatzierungsgruppen sind für Workloads hilfreich, die eine geringe Latenz erfordern.


## <a name="create-the-proximity-placement-group"></a>Erstellen der Näherungsplatzierungsgruppe
Erstellen Sie eine Näherungsplatzierungsgruppe mit [`az ppg create`](/cli/azure/ppg#az-ppg-create). 

```azurecli-interactive
az group create --name myPPGGroup --location westus
az ppg create \
   -n myPPG \
   -g myPPGGroup \
   -l westus \
   -t standard 
```

## <a name="list-proximity-placement-groups"></a>Auflisten von Näherungsplatzierungsgruppen

Mit [az ppg list](/cli/azure/ppg#az-ppg-list) können Sie alle Ihre Näherungsplatzierungsgruppen auflisten.

```azurecli-interactive
az ppg list -o table
```

## <a name="create-a-vm"></a>Erstellen einer VM

Erstellen Sie einen virtuellen Computer innerhalb der Näherungsplatzierungsgruppe mit [new az vm](/cli/azure/vm#az-vm-create).

```azurecli-interactive
az vm create \
   -n myVM \
   -g myPPGGroup \
   --image UbuntuLTS \
   --ppg myPPG  \
   --generate-ssh-keys \
   --size Standard_D1_v2  \
   -l westus
```

Sie können die VM in der Näherungsplatzierungsgruppe mit [az ppg show](/cli/azure/ppg#az-ppg-show) anzeigen.

```azurecli-interactive
az ppg show --name myppg --resource-group myppggroup --query "virtualMachines"
```

## <a name="availability-sets"></a>Verfügbarkeitsgruppen
Sie können auch eine Verfügbarkeitsgruppe in ihrer Näherungsplatzierungsgruppe erstellen. Verwenden Sie den gleichen `--ppg`-Parameter mit [az vm availability-set create](/cli/azure/vm/availability-set#az-vm-availability-set-create), um eine Verfügbarkeitsgruppe zu erstellen, und alle in der Verfügbarkeitsgruppe erstellten virtuellen Computer werden ebenfalls in derselben Näherungsplatzierungsgruppe erstellt.

## <a name="scale-sets"></a>Skalierungsgruppen

Sie können auch eine Skalierungsgruppe in ihrer Näherungsplatzierungsgruppe erstellen. Verwenden Sie den gleichen `--ppg`-Parameter mit [az vmss create](/cli/azure/vmss?view=azure-cli-latest#az-vmss-create), um eine Skalierungsgruppe zu erstellen, und alle Instanzen werden in derselben Näherungsplatzierungsgruppe erstellt.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [Azure CLI](/cli/azure/ppg)-Befehle für Näherungsplatzierungsgruppen.
