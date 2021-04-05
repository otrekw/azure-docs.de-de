---
title: 'PowerShell-Skript: Erstellen eines neuen Azure Data Share-Kontos'
description: Dieses PowerShell-Skript erstellt ein neues Data Share-Konto.
author: joannapea
ms.service: data-share
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 4608afc16398a4cd1fa47ee1ae2cd4a4489a3f96
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92221365"
---
# <a name="use-powershell-to-create-a-data-share-account-in-azure"></a>Verwenden von PowerShell zum Erstellen eines Data Share-Kontos in Azure

Dieses PowerShell-Skript erstellt ein neues Data Share-Konto. 

## <a name="sample-script"></a>Beispielskript

```powershell
# Set variables with your own values
$resourceGroupName = "<Resource group name"
$location = "<Location>"
$dataShareAccountName = "<Data share account name>"

# Create a new Azure Data Share account
New-AzDataShareAccount -ResourceGroupName $resourceGroupName -Name $dataShareAccountName -Location $location
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [New-AzDataShareAccount](/powershell/module/az.datashare/new-azdatashareaccount) | Erstellt ein Data Share-Konto |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Data Share finden Sie unter [Azure PowerShell-Beispiele für Azure Data Share](../../samples-powershell.md).
