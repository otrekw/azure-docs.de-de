---
title: 'PowerShell-Skript: Überwachen der Nutzung von Azure Data Share'
description: Dieses PowerShell-Skript ruft Nutzungsmetriken einer gesendeten Datenfreigabe ab.
author: joannapea
ms.service: data-share
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: c37875f912bf509b3a1edc878e73b1b5b1d5f407
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110703926"
---
# <a name="use-powershell-to-monitor-the-usage-of-a-sent-data-share"></a>Verwenden von PowerShell zum Überwachen der Verwendung einer gesendeten Datenfreigabe

Dieses PowerShell-Skript überwacht die Datennutzung durch Auflisten der Synchronisierungen einer gesendeten Datenfreigabe und Abrufen der Details zu einer bestimmten Synchronisierung.

## <a name="sample-script"></a>Beispielskript


```powershell
# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"
$synchronizationId = "<Azure synchronization id>"

# List synchronizations on a share
Get-AzDataShareSynchronization -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareName $dataShareName

#Get details of a specific synchronization
Get-AzDataShareSynchronizationDetails -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareName $dataShareName -SynchronizationId $synchronizationId
```


## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [Get-AzDataShareSynchronization](/powershell/module/az.datashare/get-azdatasharesynchronization) | Auflisten der Synchronisierung in einer Freigabe. |
| [Get-AzDataShareSynchronizationDetails](/powershell/module/az.datashare/get-azdatasharesynchronizationdetail) | Ruft die Synchronisierungsdetails einer Freigabesynchronisierung ab |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Data Share finden Sie unter [Azure PowerShell-Beispiele für Azure Data Share](../../samples-powershell.md).
