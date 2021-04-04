---
title: 'PowerShell-Skript: Auflisten der an einen Consumer gesendeten Azure Data Share-Einladungen'
description: Hier erfahren Sie, wie dieses PowerShell-Skript an einen Benutzer gesendete Einladungen abruft. Außerdem enthält der Artikel ein Beispiel des Skripts, das Sie verwenden können.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: article
ms.date: 07/07/2019
ms.openlocfilehash: 5f75894094fa2a15dbc9e2809ed6c3631df96c3f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92221212"
---
# <a name="use-powershell-to-get-a-data-share-invitation"></a>Verwenden von PowerShell zum Abrufen einer Einladung zu einer Datenfreigabe

Dieses PowerShell-Skript ruft Einladungen ab, die an einen Consumer gesendet wurden.

## <a name="sample-script"></a>Beispielskript
```powershell
# Set variables with your own values
$invitationId = "<invitationId>"
$location = "<location>"

#List invitations sent to a consumer
Get-AzDataShareInvitation

#Get a specific invitation sent to a consumer
Get-AzDataShareInvitation -location -invitationId 

```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [Get-AzDataShareInvitation](/powershell/module/az.datashare/get-azdatashareinvitation) | Abrufen und Auflisten gesendeter Einladungen zu Datenfreigaben |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Data Share finden Sie unter [Azure PowerShell-Beispiele für Azure Data Share](../../samples-powershell.md).
