---
title: Bereitstellen von Azure-Spot-VMs über das Portal
description: Erfahren Sie, wie Sie Spot-VMs über das Azure-Portal bereitstellen, um Kosten zu sparen.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 09/14/2020
ms.author: cynthn
ms.reviewer: jagaveer
ms.openlocfilehash: 28f21cad2387712fe638067600087ba6adbbfa42
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100554927"
---
# <a name="deploy-azure-spot-virtual-machines-using-the-azure-portal"></a>Bereitstellen von Azure-Spot-VMs über das Azure-Portal

Wenn Sie [Azure Spot-VMs](spot-vms.md) verwenden, profitieren Sie von unserer ungenutzten Kapazität und erzielen erhebliche Kosteneinsparungen. Wenn die Kapazität von Azure wieder benötigt wird, werden die Azure-Spot-VMs durch die Azure-Infrastruktur entfernt. Aus diesem Grund eignen sich Azure-Spot-VMs hervorragend für Workloads, die Unterbrechungen tolerieren, z. B. Batchverarbeitungsaufträge, Dev/Test-Umgebungen, umfangreiche Computeworkloads und mehr.

Die Preise für Azure-Spot-VMs variieren je nach Region und SKU. Weitere Informationen finden Sie unter den VM-Preisen für [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) und [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). Weitere Informationen zum Festlegen des maximalen Preises finden Sie unter [Azure-Spot-VMs – Preise](spot-vms.md#pricing).

Sie haben die Möglichkeit, einen maximalen Preis festzulegen, den Sie pro Stunde für die VM bezahlen möchten. Der maximale Preis für eine Azure-Spot-VM kann in US-Dollar (USD) mit bis zu fünf Dezimalstellen festgelegt werden. Der Wert `0.05701` würde beispielsweise einem maximalen Preis von 0,05701 US-Dollar pro Stunde entsprechen. Wenn Sie den maximalen Preis auf `-1` festlegen, wird die VM nicht basierend auf dem Preis entfernt. Der Preis für die VM entspricht dem aktuellen Preis für Spot-VMs oder dem Preis für eine Standard-VM, je nachdem, welcher Preis niedriger ist, solange Kapazitäten und Kontingente verfügbar sind.

Wenn der virtuelle Computer entfernt wird, haben Sie die Möglichkeit, entweder den virtuellen Computer und den zugrunde liegenden Datenträger zu löschen oder die VM freizugeben, damit sie später neu gestartet werden kann.


## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Wenn Sie eine VM bereitstellen, können Sie eine Azure Spot-Instanz verwenden.


Im Abschnitt **Instanzdetails** auf der Registerkarte **Grundlagen** ist **Nein** der Standardwert für die Verwendung einer Azure Spot-Instanz.

![Screenshot: Auswählen der Option „Nein“ hinsichtlich der Verwendung einer Azure Spot-Instanz](./media/spot-portal/no.png)

Wenn Sie **Ja** auswählen, wird der Abschnitt erweitert, und Sie können den [Entfernungstyp und die Entfernungsrichtlinie bestimmen](spot-vms.md#eviction-policy). 

![Screenshot: Auswählen der Option „Ja“ hinsichtlich der Verwendung einer Azure Spot-Instanz](./media/spot-portal/yes.png)

Sie können auch die Preis- und Entfernungsraten mit anderen ähnlichen Regionen vergleichen, indem Sie **Preisverlauf anzeigen und Preise in Regionen in der Nähe vergleichen** auswählen.

In diesem Beispiel ist die Region „Kanada, Mitte“ kostengünstiger und weist eine niedrigere Entfernungsrate als die Region „USA, Osten“ auf.

:::image type="content" source="./media/spot-portal/pricing.png" alt-text="Screenshot: Regionsoptionen mit Unterschied in den Preisen und Entfernungsraten.":::

Sie können die Region ändern, indem Sie die Option auswählen, die am besten für Sie geeignet ist, und dann **OK** auswählen.

## <a name="simulate-an-eviction"></a>Simulieren einer Entfernung

Sie können die Entfernung einer Azure-Spot-VM [simulieren](/rest/api/compute/virtualmachines/simulateeviction), um zu testen, wie gut die Anwendung auf einen plötzlichen Entfernungsvorgang reagiert. 

Ersetzen Sie Folgendes durch Ihre Informationen: 

- `subscriptionId`
- `resourceGroupName`
- `vmName`


```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/simulateEviction?api-version=2020-06-01
```

## <a name="next-steps"></a>Nächste Schritte

Sie können Azure-Spot-VMs auch mithilfe von [PowerShell](./windows/spot-powershell.md), der [Befehlszeilenschnittstelle](./linux/spot-cli.md) oder einer [Vorlage](./linux/spot-template.md) erstellen.
