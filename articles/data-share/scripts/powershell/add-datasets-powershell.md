---
title: 'PowerShell-Skript: Hinzufügen eines Blobdatasets in Azure Data Share'
description: Mit diesem PowerShell-Skript wird einer vorhandenen Freigabe ein Blobdataset hinzugefügt.
services: data-share
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: article
ms.date: 07/07/2019
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 0135e0ace5c71d12afbe5af720a35cecac62fc92
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110697109"
---
# <a name="use-powershell-to-create-a-data-share-in-azure"></a>Verwenden von PowerShell zum Erstellen einer Datenfreigabe in Azure

Mit diesem PowerShell-Skript wird einer vorhandenen Datenfreigabe ein Blobdataset hinzugefügt.

## <a name="sample-script"></a>Beispielskript

```powershell
# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"
$blobDatasetName = "<Dataset name>"
$blobContainer = "<Blob container name>"
$blobFilePath = "<Blob file path>"
$storageAccountResourceId = "<Storage account resource id>"

#Add a blob dataset to the datashare
New-AzDataShareDataSet -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareName $dataShareName -Name $blobDataSetName -StorageAccountResourceId $storageAccountResourceId -FilePath $blobFilePath

```


## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [New-AzDataShareDataSet](/powershell/module/az.datashare/new-azdatasharedataset) | Fügt einer Datenfreigabe ein Dataset hinzu. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Data Share finden Sie unter [Azure PowerShell-Beispiele für Azure Data Share](../../samples-powershell.md).
