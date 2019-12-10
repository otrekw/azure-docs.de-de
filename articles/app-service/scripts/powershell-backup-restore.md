---
title: 'PowerShell: Wiederherstellen einer App-Sicherung'
description: Hier erfahren Sie, wie Sie mit Azure PowerShell die Bereitstellung und Verwaltung von App Service automatisieren. In diesem Beispiel wird gezeigt, wie eine App auf der Grundlage einer Sicherung wiederhergestellt wird.
author: msangapu-msft
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.topic: sample
ms.date: 11/21/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 026c275ce4bc85ff3f964bfbb175cc01ff611df0
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2019
ms.locfileid: "74685769"
---
# <a name="restore-a-web-app-from-a-backup-using-azure-powershell"></a>Wiederherstellen einer Web-App aus einer Sicherung mit Azure PowerShell

Mit dem folgenden Skriptbeispiel wird eine zuvor erstellte Sicherung aus einer vorhandenen Web-App abgerufen und durch Überschreiben des Inhalts wiederhergestellt. 

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/overview), und führen Sie dann `Connect-AzAccount` aus, um eine Verbindung mit Azure herzustellen. 

## <a name="sample-script"></a>Beispielskript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-restore/backup-restore.ps1?highlight=1-2 "Restore a web app from a backup")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Wenn Sie die Web-App nicht mehr benötigen, entfernen Sie mit dem folgenden Befehl die Ressourcengruppe, die Web-App und alle zugehörigen Ressourcen:

```powershell
Remove-AzResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzWebAppBackupList](/powershell/module/az.websites/get-azwebappbackuplist) | Ruft eine Liste der Sicherungen für eine Web-App ab. |
| [Restore-AzWebAppBackup](/powershell/module/az.websites/restore-azwebappbackup) | Stellt eine Web-App aus einer zuvor ausgeführten Sicherung wieder her. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure App Service-Web-Apps finden Sie unter [Azure PowerShell-Beispiele](../samples-powershell.md).
