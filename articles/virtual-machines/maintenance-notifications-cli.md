---
title: Erhalten von Wartungsbenachrichtigungen per CLI
description: Zeigen Sie mithilfe der Azure CLI Wartungsbenachrichtigungen für in Azure ausgeführte virtuelle Computer an, und starten Sie eine Self-Service-Wartung.
author: shants123
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/19/2019
ms.author: shants
ms.openlocfilehash: 4ad57c1c71a51f948bd405a5487a1e27e36bfff7
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "77920891"
---
# <a name="handling-planned-maintenance-notifications-using-the-azure-cli"></a>Behandeln von Benachrichtigungen zu geplanten Wartungen mit der Azure CLI

**Dieser Artikel gilt für virtuelle Computer, auf denen sowohl Linux als auch Windows ausgeführt wird.**

Sie können mithilfe der CLI prüfen, wann die [Wartung](maintenance-notifications.md) Ihrer virtuellen Computer geplant ist. Informationen zu geplanten Wartungen können mithilfe von [azure vm get-instance-view](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-get-instance-view) angezeigt werden.
 
Wartungsinformationen werden nur zurückgegeben, wenn eine Wartung geplant ist. 

```azurecli-interactive
az vm get-instance-view -n myVM -g myResourceGroup --query instanceView.maintenanceRedeployStatus
```

## <a name="start-maintenance"></a>Wartung starten

Der folgende Aufruf startet die Wartung für einen virtuellen Computer, wenn `IsCustomerInitiatedMaintenanceAllowed` auf „true“ festgelegt ist.

```azurecli-interactive
az vm perform-maintenance -g myResourceGroup -n myVM 
```

## <a name="classic-deployments"></a>Klassische Bereitstellungen

[!INCLUDE [classic-vm-deprecation](../../includes/classic-vm-deprecation.md)]

Wenn Sie ältere virtuelle Computer besitzen, die mit dem klassischen Bereitstellungsmodell bereitgestellt wurden, können Sie mit der klassischen Azure CLI virtuelle Computer abfragen und die Wartung initiieren.

Stellen Sie sicher, dass Sie den richtigen Modus für die Nutzung klassischer virtueller Computer verwenden. Geben Sie dazu Folgendes ein:

```
azure config mode asm
```

Geben Sie zum Abrufen des Wartungsstatus eines virtuellen Computers mit dem Namen *myVM* Folgendes ein:

```
azure vm show myVM 
``` 

Wenn Sie die Wartung auf dem klassischen virtuellen Computer *myVM* im Dienst *myService* und der Bereitstellung *myDeployment* starten möchten, geben Sie Folgendes ein:

```
azure compute virtual-machine initiate-maintenance --service-name myService --name myDeployment --virtual-machine-name myVM
```

## <a name="next-steps"></a>Nächste Schritte

Sie können die geplante Wartung auch mithilfe von [Azure PowerShell](maintenance-notifications-powershell.md) oder über das [Portal](maintenance-notifications-portal.md) vornehmen.
