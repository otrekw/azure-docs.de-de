---
title: Verwenden der Befehlszeilenschnittstelle zum Bereitstellen von Azure Spot-VMs
description: Erfahren Sie, wie Sie mithilfe der Befehlszeilenschnittstelle Azure Spot-VMs bereitstellen, um Kosten zu sparen.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 03/25/2020
ms.author: cynthn
ms.reviewer: jagaveer
ms.openlocfilehash: 3f341271c208cc56a704c836433c33af0129a4ac
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758361"
---
# <a name="deploy-spot-vms-using-the-azure-cli"></a>Bereitstellen von Spot-VMs mithilfe der Azure-Befehlszeilenschnittstelle

Mithilfe von [Azure Spot-VMs](spot-vms.md) profitieren Sie von unserer ungenutzten Kapazität und können erhebliche Kosten einsparen. Wenn die Kapazität von Azure wieder benötigt wird, werden die Spot-VMs durch die Azure-Infrastruktur entfernt. Aus diesem Grund eignen sich Spot-VMs hervorragend für Workloads, die Unterbrechungen tolerieren, z. B. Batchverarbeitungsaufträge, Dev/Test-Umgebungen, umfangreiche Computeworkloads und mehr.

Die Preise für Spot-VMs variieren je nach Region und SKU. Weitere Informationen finden Sie unter den VM-Preisen für [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) und [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). 

Sie haben die Möglichkeit, einen maximalen Preis festzulegen, den Sie pro Stunde für die VM bezahlen möchten. Der maximale Preis für eine Spot-VM kann in US-Dollar (USD) mit bis zu fünf Dezimalstellen festgelegt werden. Der Wert `0.98765` würde beispielsweise einem maximalen Preis von 0,98765 US-Dollar pro Stunde entsprechen. Wenn Sie den maximalen Preis auf `-1` festlegen, wird die VM nicht basierend auf dem Preis entfernt. Der Preis für die VM entspricht dem aktuellen Preis für Spot-VMs oder dem Preis für eine Standard-VM, je nachdem, welcher Preis niedriger ist, solange Kapazität und Kontingente verfügbar sind. Weitere Informationen zum Festlegen des maximalen Preises finden Sie unter [Spot-VMs – Preise](spot-vms.md#pricing).

Der Vorgang zum Erstellen einer VM mit Spot unter Verwendung der Azure-Befehlszeilenschnittstelle ist identisch mit dem im [Schnellstartartikel](/azure/virtual-machines/linux/quick-create-cli) beschriebenen Vorgang. Fügen Sie einfach den Parameter „--priority Spot“ hinzu, und geben Sie einen maximalen Preis oder `-1` an.


## <a name="install-azure-cli"></a>Installieren der Azure-Befehlszeilenschnittstelle

Zum Erstellen von Spot-VMs muss die Azure-Befehlszeilenschnittstelle der Version 2.0.74 oder höher ausgeführt werden. Führen Sie **az --version** aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

Melden Sie sich mit [az login](/cli/azure/reference-index#az-login) in Azure an.

```azurecli
az login
```

## <a name="create-a-spot-vm"></a>Erstellen einer Spot-VM

Dieses Beispiel zeigt, wie Sie eine Linux-Spot-VM bereitstellen, die nicht basierend auf dem Preis entfernt wird. 

```azurecli
az group create -n mySpotGroup -l eastus
az vm create \
    --resource-group mySpotGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --max-price -1
```

Nachdem die VM erstellt wurde, können Sie den maximalen Abrechnungspreis für alle VMs in der Ressourcengruppe abfragen.

```azurecli
az vm list \
   -g mySpotGroup \
   --query '[].{Name:name, MaxPrice:billingProfile.maxPrice}' \
   --output table
```

**Nächste Schritte**

Sie können eine Spot-VM auch mit [Azure PowerShell](../windows/spot-powershell.md) oder einer [Vorlage](spot-template.md) erstellen.

Informationen zu eventuell auftretenden Fehlern finden Sie unter [Fehlercodes](../error-codes-spot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
