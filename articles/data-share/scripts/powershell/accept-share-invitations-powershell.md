---
title: 'PowerShell-Skript: Akzeptieren einer Einladung für Azure Data Share'
description: Dieses PowerShell-Skript akzeptiert Einladungen von einer vorhandenen Datenfreigabe.
services: data-share
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: article
ms.date: 07/07/2019
ms.openlocfilehash: 24091bafac7ad5c558b975d52064f12715b3d622
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92221399"
---
# <a name="use-powershell-to-accept-a-data-share-invitation"></a>Verwenden von PowerShell zum Akzeptieren einer Einladung von einer Datenfreigabe

Dieses PowerShell-Skript akzeptiert Einladungen, die an einen Consumer gesendet werden.

## <a name="sample-script"></a>Beispielskript
```powershell
#List invitations sent to a consumer
Get-AzDataShareInvitation

# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"
$invitationId = "<Invitation id>"

#Accept a specific invitation by creating a share subscription
New-AzDataShareSubscription -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -Name $dataShareName -InvitationId $invitationId

```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [Get-AzDataShareInvitation](/powershell/module/az.datashare/get-azdatashareinvitation) | Abrufen und Auflisten gesendeter Einladungen zu Datenfreigaben |
| [New-AzDataShareSubscription](/powershell/module/az.datashare/get-azdatasharesubscription) | Erstellen eines Abonnements für eine Datenfreigabe |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Data Share finden Sie unter [Azure PowerShell-Beispiele für Azure Data Share](../../samples-powershell.md).
