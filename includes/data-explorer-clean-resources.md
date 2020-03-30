---
author: lucygoldbergmicrosoft
ms.service: data-explorer
ms.topic: include
ms.date: 11/28/2019
ms.author: lugoldbe
ms.openlocfilehash: 9598724db630db7fc48545b58f2b0c1cad464aa4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "74667847"
---
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen. 

### <a name="clean-up-resources-using-the-azure-portal"></a>Bereinigen von Ressourcen über das Azure-Portal

Löschen Sie die Ressourcen im Azure-Portal, indem Sie die Schritte zum [Bereinigen von Ressourcen](../articles/data-explorer/create-cluster-database-portal.md#clean-up-resources) ausführen.

### <a name="clean-up-resources-using-powershell"></a>Bereinigen von Ressourcen über PowerShell

Wenn die Cloud Shell noch geöffnet ist, muss die erste Zeile (Read-Host) nicht kopiert/ausgeführt werden.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```