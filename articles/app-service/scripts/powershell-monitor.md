---
title: 'Mit PowerShell: Überwachen mit Webserverprotokollen'
description: Hier erfahren Sie, wie Sie mit Azure PowerShell die Bereitstellung und Verwaltung von App Service automatisieren. In diesem Beispiel wird gezeigt, wie Sie eine App mit Webserverprotokollen überwachen.
tags: azure-service-management
ms.assetid: 5805d7cd-9e56-4eba-bd85-75b013690ff5
ms.topic: sample
ms.date: 03/20/2017
ms.custom: mvc, devx-track-azurepowershell
ms.openlocfilehash: 8a3af13060d932b1a39a85bbc631732f8f859dfb
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89070274"
---
# <a name="monitor-a-web-appwith-web-server-logs"></a>Überwachen einer Web-App mit Webserverprotokollen

In diesem Szenario erstellen Sie eine Ressourcengruppe, einen App Service-Plan und eine Web-App und konfigurieren die Web-App so, dass Webserverprotokolle aktiviert sind. Danach laden Sie die Protokolldateien zur Überprüfung herunter.

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/), und führen Sie dann `Connect-AzAccount` aus, um eine Verbindung mit Azure herzustellen.

## <a name="sample-script"></a>Beispielskript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/monitor-with-logs/monitor-with-logs.ps1 "Monitor a web app with web server logs")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach dem Ausführen des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die Web-App und alle zugehörigen Ressourcen entfernt werden.

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | Erstellt einen App Service-Plan. |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Erstellt die Web-App. |
| [Set-AzWebApp](/powershell/module/az.websites/set-azwebapp) | Ändert die Konfiguration einer Web-App. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).

Zusätzliche Azure PowerShell-Beispiele für Azure App Service-Web-Apps finden Sie unter [Azure PowerShell-Beispiele](../samples-powershell.md).
