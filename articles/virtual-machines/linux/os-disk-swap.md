---
title: Tauschen von Betriebssystemdatenträgern mithilfe der Befehlszeilenschnittstelle
description: Ändern Sie den von einem virtuellen Azure-Computer verwendeten Betriebssystem-Datenträger mithilfe der CLI.
author: cynthn
ms.service: virtual-machines
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 04/24/2018
ms.author: cynthn
ms.openlocfilehash: 24cd71cafa9dc0bd9131255caf50841f6d5909d1
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107311242"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-the-cli"></a>Ändern des von einem virtuellen Azure-Computer verwendeten Betriebssystem-Datenträgers mithilfe der CLI


Wenn Sie über einen virtuellen Computer verfügen, aber den Datenträger für einen Sicherungsdatenträger oder einem anderen Betriebssystem-Datenträger austauschen möchten, können Sie die Betriebssystem-Datenträger mit der Azure CLI austauschen. Sie müssen den virtuellen Computer nicht löschen und neu erstellen. Sie können auch einen verwalteten Datenträger in einer anderen Ressourcengruppe verwenden, solange er nicht bereits in Gebrauch ist.

Der virtuelle Computer muss sich im Zustand „Beendet (Zuordnung aufgehoben)“ befinden, und dann kann die Ressourcen-ID des verwalteten Datenträgers durch die Ressourcen-ID eines anderen verwalteten Datenträgers ersetzt werden. 

Stellen Sie sicher, dass VM-Größe und Speichertyp mit dem Datenträger kompatibel sind, den Sie anfügen möchten. Wenn der Datenträger, den Sie verwenden möchten, sich z.B. im Storage Premium befindet, dann muss der virtuelle Computer für Storage Premium geeignet sein (z.B. eine Größe der DS-Serie).

Für diesen Artikel ist mindestens Version 2.0.25 der Azure CLI erforderlich. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli). 


Rufen Sie mit [az disk list](/cli/azure/disk) eine Liste der Datenträger in der Ressourcengruppe ab.

```azurecli-interactive
az disk list \
   -g myResourceGroupDisk \
   --query '[*].{diskId:id}' \
   --output table
```


Beenden Sie mit [az vm stop](/cli/azure/vm) den virtuellen Computer (heben Sie seine Zuordnung auf), ehe Sie die Datenträger austauschen.

```azurecli-interactive
az vm stop \
   -n myVM \
   -g myResourceGroup
```


Verwenden Sie [az vm update](/cli/azure/vm#az-vm-update) mit der vollständigen Ressourcen-ID des neuen Datenträgers für den `--osdisk`-Parameter. 

```azurecli-interactive 
az vm update \
   -g myResourceGroup \
   -n myVM \
   --os-disk /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/myDisk 
   ```
   
Starten Sie den virtuellen Computer mit [az vm start](/cli/azure/vm) neu.

```azurecli-interactive
az vm start \
   -n myVM \
   -g myResourceGroup
```

   
**Nächste Schritte**

Wie Sie eine Kopie eines Datenträgers erstellen, erfahren Sie unter [Erstellen einer Momentaufnahme](snapshot-copy-managed-disk.md).
