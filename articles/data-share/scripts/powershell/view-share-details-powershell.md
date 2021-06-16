---
title: 'PowerShell-Skript: Auflisten vorhandener Freigaben in Azure Data Share'
description: Dieses PowerShell-Skript listet Details zu Freigaben auf und zeigt diese an.
services: data-share
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: article
ms.date: 07/07/2019
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 2692cbd836b351e5c41dfe40168b365885997ac0
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110703902"
---
# <a name="use-powershell-to-view-the-details-of-a-sent-data-share"></a>Verwenden von PowerShell zum Anzeigen der Details einer gesendeten Datenfreigabe

Dieses PowerShell-Skript listet Datenfreigaben eines vorhandenen Kontos auf und ruft die Details einer bestimmten Freigabe ab.


## <a name="sample-script"></a>Beispielskript

```powershell

# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"

#Lists all data shares within an account
Get-AzDataShare -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName

#Gets details of a specific data share
Get-AzDataShare -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -Name $dataShareName

```


## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [Get-AzDataShare](/powershell/module/az.datashare/get-azdatashare) | Ruft die Freigaben in einem Konto ab und listet diese auf |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Data Share finden Sie unter [Azure PowerShell-Beispiele für Azure Data Share](../../samples-powershell.md).
