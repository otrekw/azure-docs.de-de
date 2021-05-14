---
title: Azure PowerShell-Skriptbeispiel – Sichern und Wiederherstellen eines Diensts | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die API Management-Dienstinstanz sichern und wiederherstellen. Sehen Sie sich ein Beispielskript an, und zeigen Sie zusätzliche verfügbare Ressourcen an.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.topic: sample
ms.date: 11/16/2017
ms.author: apimpm
ms.custom: mvc, devx-track-azurepowershell
ms.openlocfilehash: e879c4df24e7913aac2aac09078bbf7f817ffcea
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2021
ms.locfileid: "107812604"
---
# <a name="backup-and-restore-service"></a>Sichern und Wiederherstellen eines Diensts

Das Beispielskript in diesem Artikel veranschaulicht, wie die API Management-Dienstinstanz gesichert und wiederhergestellt wird. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial mindestens Version 1.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable Az` aus, um die Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-Az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/api-management/backup-restore-apim-service/backup_restore_apim_service.ps1 "Backup and restore the APIM service instance")]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) entfernen.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).

Weitere Azure PowerShell-Beispiele für Azure API Management finden Sie in den [PowerShell-Beispielen](../powershell-samples.md).
