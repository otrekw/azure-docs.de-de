---
title: 'PowerShell-Skript: Erstellen einer Azure Data Share-Instanz'
description: Dieses PowerShell-Skript erstellt eine neue Datenfreigabe in einem bestehenden Data Share-Konto.
author: joannapea
ms.service: data-share
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 36c1bdfef2afe4c34796a804784317a5e7fe12ff
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92221348"
---
# <a name="use-powershell-to-create-a-data-share-in-azure"></a>Verwenden von PowerShell zum Erstellen einer Datenfreigabe in Azure

Dieses PowerShell-Skript erstellt eine neue Datenfreigabe in einem bestehenden Data Share-Konto.

## <a name="sample-script"></a>Beispielskript

```powershell

# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"

# Create a new Azure Data Share
New-AzDataShare -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -Name $dataShareName

```


## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [New-AzDataShare](/powershell/module/az.datashare/new-azdatashare) | Erstellt eine Datenfreigabe |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Data Share finden Sie unter [Azure PowerShell-Beispiele für Azure Data Share](../../samples-powershell.md).
