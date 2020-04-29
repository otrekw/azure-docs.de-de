---
title: 'Mit PowerShell: Hochladen und Binden von TLS/SSL'
description: Hier erfahren Sie, wie Sie mit Azure PowerShell die Bereitstellung und Verwaltung von App Service automatisieren. In diesem Beispiel wird gezeigt, wie Sie ein benutzerdefiniertes TLS-/SSL-Zertifikat an eine App binden.
author: msangapu-msft
tags: azure-service-management
ms.assetid: 23e83b74-614a-49a0-bc08-7542120eeec5
ms.topic: sample
ms.date: 03/20/2017
ms.custom: mvc, seodec18
ms.openlocfilehash: 5116585b701717a82b757cae70f938c321a1f7d1
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81532543"
---
# <a name="bind-a-custom-tlsssl-certificate-to-a-web-app-using-powershell"></a>Binden eines benutzerdefinierten TLS-/SSL-Zertifikats an eine Web-App mit PowerShell

Dieses Skriptbeispiel erstellt eine Web-App und die zugehörigen Ressourcen in App Service und bindet dann das TLS-/SSL-Zertifikat eines benutzerdefinierten Domänennamens an die App. 

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/overview), und führen Sie dann `Connect-AzAccount` aus, um eine Verbindung mit Azure herzustellen. Stellen Sie darüber hinaus Folgendes sicher:

- Eine Verbindung mit Azure wurde mit dem Befehl `az login` hergestellt.
- Sie haben Zugriff auf die Seite „DNS-Konfiguration“ der Domänenregistrierungsstelle.
- Sie verfügen über eine gültige PFX-Datei und das zugehörige Kennwort für das TLS-/SSL-Zertifikat, das Sie hochladen und binden möchten.

## <a name="sample-script"></a>Beispielskript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom TLS/SSL certificate to a web app")]

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
| [Set-AzAppServicePlan](/powershell/module/az.websites/set-azappserviceplan) | Ändert einen App Service-Plan, um den zugehörigen Tarif zu ändern. |
| [Set-AzWebApp](/powershell/module/az.websites/set-azwebapp) | Ändert die Konfiguration einer Web-App. |
| [New-AzWebAppSSLBinding](/powershell/module/az.websites/new-azwebappsslbinding) | Erstellt eine TLS-/SSL-Zertifikatsbindung für eine Web-App. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure App Service-Web-Apps finden Sie unter [Azure PowerShell-Beispiele](../samples-powershell.md).
