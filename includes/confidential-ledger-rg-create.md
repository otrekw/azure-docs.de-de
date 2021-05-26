---
author: msmbaldwin
ms.service: confidential-ledger
ms.topic: include
ms.date: 05/05/2021
ms.author: msmbaldwin
ms.openlocfilehash: bebab5bbab271b83b6b7e039bdf68d6254bd5fe2
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386002"
---
Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Erstellen Sie mit dem Azure CLI-Befehl [az group create](/cli/azure/group#az_group_create) oder mit dem Azure PowerShell-Cmdlet [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
```azurecli
az group create --name "myResourceGroup" -l "EastUS"
```
# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azurepowershell)

```azurepowershell
New-AzResourceGroup -Name myResourceGroup -Location eastus
```
---
