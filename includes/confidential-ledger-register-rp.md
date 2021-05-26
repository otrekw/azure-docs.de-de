---
author: msmbaldwin
ms.service: confidential-ledger
ms.topic: include
ms.date: 05/05/2021
ms.author: msmbaldwin
ms.openlocfilehash: 5a94cdae7cc332b8e0df5b2ebdc6ae57b67c8a68
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385990"
---
Ein Ressourcenanbieter ist ein Dienst, der Azure-Ressourcen bereitstellt. Verwenden Sie an der Azure-Befehlszeilenschnittstelle (CLI) den Befehl [az provider register](/cli/azure/provider#az_provider_register) oder das Azure PowerShell-Cmdlet [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), um den Confidential Ledger-Ressourcenanbieter „microsoft.ConfidentialLedger“ zu registrieren.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
```azurecli
az provider register --namespace "microsoft.ConfidentialLedger"
```
# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azurepowershell)

```azurepowershell
Register-AzureRmResourceProvider -ProviderNamespace "microsoft.ConfidentialLedger"
```
---

Mit dem Azure CLI-Befehl [az provider register](/cli/azure/provider#az_provider_show) oder mit dem Azure PowerShell-Cmdlet [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider) können Sie überprüfen, ob die Registrierung abgeschlossen ist.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
```azurecli
az provider show --namespace "microsoft.ConfidentialLedger"
```
# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azurepowershell)

```azurepowershell
Get-AzureRmResourceProvider -ProviderNamespace "microsoft.ConfidentialLedger"
```
---
