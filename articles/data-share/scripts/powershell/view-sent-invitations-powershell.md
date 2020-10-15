---
title: 'PowerShell-Skript: Auflisten der an einen Consumer gesendeten Azure Data Share-Einladungen | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie dieses PowerShell-Skript an einen Benutzer gesendete Einladungen abruft. Außerdem enthält der Artikel ein Beispiel des Skripts, das Sie verwenden können.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 3f0374ba5b98c145359d18143584e23fc543c552
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90985617"
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
| [Get-AzDataShareInvitation](/powershell/module/az.datashare/get-azdatashareinvitation?view=azps-2.6.0) | Abrufen und Auflisten gesendeter Einladungen zu Datenfreigaben |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Data Share finden Sie unter [Azure PowerShell-Beispiele für Azure Data Share](../../samples-powershell.md).
