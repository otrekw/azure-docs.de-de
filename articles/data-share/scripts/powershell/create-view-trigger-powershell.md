---
title: 'PowerShell-Skript: Erstellen und Anzeigen von Azure Data Share-Momentaufnahmetriggern'
description: Mit diesem PowerShell-Skript werden Momentaufnahmetrigger für Freigeben erstellt und abgerufen.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: article
ms.date: 07/07/2019
ms.openlocfilehash: 05b70641873ab352ca92ac199d99c05b2fe0f4e6
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92221297"
---
# <a name="use-powershell-to-create-and-share-snapshot-triggers"></a>Verwenden von PowerShell zum Erstellen und Freigeben von Momentaufnahmetriggern

Mit diesem PowerShell-Skript werden Momentaufnahmetrigger für Freigeben erstellt und abgerufen.

## <a name="sample-script"></a>Beispielskript

```powershell
# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"
$subscriptionName = "<Share subscription name>"
$recurrenceInterval = "<Synchronization recurrence interval>"
$synchronizationTime = "<Synchronization time>"

# Create a new snapshot trigger
New-AzDataShareTrigger -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareSubscriptionName $subscriptionName -Name $dataShareName  -RecurrenceInterval $recurrenceInterval -SynchronizationTime $synchronizationTime

#List snapshot triggers
Get-AzDataShareTrigger -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareSubscriptionName $subscriptionName  -Name $dataShareName

#Get a specific share snapshot trigger
Get-AzDataShareTrigger -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareSubscriptionName -Name $dataShareName
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [New-AzDataShareTrigger](/powershell/module/az.datashare/new-azdatasharetrigger) | Erstellen eines Momentaufnahmetriggers für eine Freigabe |
| [Get-AzDataShareTrigger](/powershell/module/az.datashare/get-azdatasharesynchronizationsetting) | Ruft die Synchronisierungseinstellungen einer Freigabesynchronisierung ab |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Data Share finden Sie unter [Azure PowerShell-Beispiele für Azure Data Share](../../samples-powershell.md).
