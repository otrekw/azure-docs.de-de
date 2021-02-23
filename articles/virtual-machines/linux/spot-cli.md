---
title: Bereitstellen von Azure-Spot-VMs mithilfe der Befehlszeilenschnittstelle
description: Erfahren Sie, wie Sie mithilfe der Befehlszeilenschnittstelle Azure-Spot-VMs bereitstellen, um Kosten zu sparen.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 06/26/2020
ms.author: cynthn
ms.reviewer: jagaveer
ms.openlocfilehash: abb29c0826e38af0bbbc1b59e41234acdaaca0f9
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100554785"
---
# <a name="deploy-azure-spot-virtual-machines-using-the-azure-cli"></a>Bereitstellen von Azure-Spot-VMs mithilfe der Azure-Befehlszeilenschnittstelle

Wenn Sie [Azure Spot-VMs](../spot-vms.md) verwenden, profitieren Sie von unserer ungenutzten Kapazität und erzielen erhebliche Kosteneinsparungen. Wenn die Kapazität von Azure wieder benötigt wird, werden die Azure-Spot-VMs durch die Azure-Infrastruktur entfernt. Aus diesem Grund eignen sich Azure-Spot-VMs hervorragend für Workloads, die Unterbrechungen tolerieren, z. B. Batchverarbeitungsaufträge, Dev/Test-Umgebungen, umfangreiche Computeworkloads und mehr.

Die Preise für Azure-Spot-VMs variieren je nach Region und SKU. Weitere Informationen finden Sie unter den VM-Preisen für [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) und [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). 

Sie haben die Möglichkeit, einen maximalen Preis festzulegen, den Sie pro Stunde für die VM bezahlen möchten. Der maximale Preis für eine Azure-Spot-VM kann in US-Dollar (USD) mit bis zu fünf Dezimalstellen festgelegt werden. Der Wert `0.98765` würde beispielsweise einem maximalen Preis von 0,98765 US-Dollar pro Stunde entsprechen. Wenn Sie den maximalen Preis auf `-1` festlegen, wird die VM nicht basierend auf dem Preis entfernt. Der Preis für die VM entspricht dem aktuellen Preis für Azure-Spot-VMs oder dem Preis für eine Standard-VM, je nachdem, welcher Preis niedriger ist, solange Kapazität und Kontingente verfügbar sind. Weitere Informationen zum Festlegen des maximalen Preises finden Sie unter [Azure-Spot-VMs – Preise](../spot-vms.md#pricing).

Der Vorgang zum Erstellen einer Azure-Spot-VM mithilfe der Azure-Befehlszeilenschnittstelle ist identisch mit dem im [Schnellstartartikel](./quick-create-cli.md) beschriebenen Vorgang. Fügen Sie einfach den Parameter „--priority Spot“ hinzu, legen Sie `--eviction-policy` entweder auf die Standardeinstellung „Deallocate“ oder auf `Delete` fest, und geben Sie einen maximalen Preis oder `-1` an. 


## <a name="install-azure-cli"></a>Installieren der Azure-Befehlszeilenschnittstelle

Zum Erstellen von Azure-Spot-VMs muss mindestens Version 2.0.74 der Azure-Befehlszeilenschnittstelle ausgeführt werden. Führen Sie **az --version** aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

Melden Sie sich mit [az login](/cli/azure/reference-index#az-login) in Azure an.

```azurecli
az login
```

## <a name="create-an-azure-spot-virtual-machine"></a>Erstellen einer Azure-Spot-VM

Dieses Beispiel zeigt, wie Sie eine Azure-Spot-VM mit Linux bereitstellen, die nicht basierend auf dem Preis entfernt wird. Die Entfernungsrichtlinie wird so festgelegt, dass die Zuordnung der VM aufgehoben wird. Dadurch kann sie zu einem späteren Zeitpunkt neu gestartet werden. Wenn Sie die VM und den zugrunde liegenden Datenträger beim Entfernen löschen möchten, legen Sie `--eviction-policy` auf `Delete` fest.

```azurecli
az group create -n mySpotGroup -l eastus
az vm create \
    --resource-group mySpotGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --max-price -1 \
    --eviction-policy Deallocate
```



Nachdem die VM erstellt wurde, können Sie den maximalen Abrechnungspreis für alle VMs in der Ressourcengruppe abfragen.

```azurecli
az vm list \
   -g mySpotGroup \
   --query '[].{Name:name, MaxPrice:billingProfile.maxPrice}' \
   --output table
```

## <a name="simulate-an-eviction"></a>Simulieren einer Entfernung

Sie können die Entfernung einer Azure-Spot-VM [simulieren](/rest/api/compute/virtualmachines/simulateeviction), um zu testen, wie gut die Anwendung auf einen plötzlichen Entfernungsvorgang reagiert. 

Ersetzen Sie Folgendes durch Ihre Informationen: 

- `subscriptionId`
- `resourceGroupName`
- `vmName`


```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/simulateEviction?api-version=2020-06-01
```

**Nächste Schritte**

Sie können eine Azure-Spot-VM auch mithilfe von [Azure PowerShell](../windows/spot-powershell.md), über das [Portal](../spot-portal.md) oder mit einer [Vorlage](spot-template.md) erstellen.

Fragen Sie Preisinformationen ab, indem Sie die [Azure-Einzelhandelspreis-API](/rest/api/cost-management/retail-prices/azure-retail-prices) verwenden, um Informationen zu Azure-Spot-VMs zu erhalten. `meterName` und `skuName` enthalten beide `Spot`.

Informationen zu eventuell auftretenden Fehlern finden Sie unter [Fehlercodes](../error-codes-spot.md).