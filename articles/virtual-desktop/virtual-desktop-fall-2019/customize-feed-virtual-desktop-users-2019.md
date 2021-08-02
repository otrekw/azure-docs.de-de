---
title: Anpassen des Feeds für Benutzer von Azure Virtual Desktop (klassisch) – Azure
description: Erfahren Sie, wie Sie den Feed für Benutzer von Azure Virtual Desktop (klassisch) mit PowerShell-Cmdlets anpassen.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 7a28185d16f0bc7787eccc76a9f4a0a98d5c6a38
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111754149"
---
# <a name="customize-feed-for-azure-virtual-desktop-classic-users"></a>Anpassen des Feeds für Benutzer von Azure Virtual Desktop (klassisch) – Azure

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../customize-feed-for-virtual-desktop-users.md) weiter.

Sie können den Feed anpassen, damit die RemoteApp- und Remotedesktopressourcen den Benutzern auf erkennbare Weise angezeigt werden.

Zunächst müssen Sie das [PowerShell-Modul für Azure Virtual Desktop](/powershell/windows-virtual-desktop/overview/) herunterladen und importieren, um es in Ihrer PowerShell-Sitzung verwenden zu können. Führen Sie anschließend das folgende Cmdlet aus, um sich bei Ihrem Konto anzumelden:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

## <a name="customize-the-display-name-for-a-remoteapp"></a>Anpassen des Anzeigenamens für eine RemoteApp

Sie können den Anzeigenamen für eine veröffentlichte RemoteApp ändern, indem Sie ihn festlegen. Standardmäßig ist der Anzeigename identisch mit dem Namen des RemoteApp-Programms.

Um eine Liste der veröffentlichten RemoteApps für eine App-Gruppe abzurufen, führen Sie das folgende PowerShell-Cmdlet aus:

```powershell
Get-RdsRemoteApp -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname>
```

> [!div class="mx-imgBorder"]
> ![Screenshot des PowerShell-Cmdlets Get-RDSRemoteApp mit hervorgehobenem Namen und Anzeigenamen, um einen Anzeigenamen anzupassen](../media/get-rdsremoteapp.png)

Um einer RemoteApp einen Anzeigenamen zuzuweisen, führen Sie das folgende PowerShell-Cmdlet aus:

```powershell
Set-RdsRemoteApp -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname> -Name <existingappname> -FriendlyName <newfriendlyname>
```

> [!div class="mx-imgBorder"]
> ![Screenshot des PowerShell-Cmdlets Set-RDSRemoteApp mit neuem hervorgehobenem Namen und neuem Anzeigenamen.](../media/set-rdsremoteapp.png)

## <a name="customize-the-display-name-for-a-remote-desktop"></a>Anpassen des Anzeigenamens für einen Remotedesktop

Sie können den Anzeigenamen für einen veröffentlichten Remotedesktop ändern, indem Sie einen Anzeigenamen festlegen. Wenn Sie manuell mit PowerShell einen Hostpool und eine Desktop-App-Gruppe erstellt haben, lautet der Anzeigename standardmäßig „Sitzungsdesktop“. Wenn Sie den Hostpool und die Desktop-App-Gruppe über die Azure Resource Manager-Vorlage von GitHub oder das Angebot im Azure Marketplace erstellt haben, ist der Standardanzeigename mit dem Namen des Hostpools identisch.

Führen Sie zum Abrufen der Remotedesktopressource das folgende PowerShell-Cmdlet aus:

```powershell
Get-RdsRemoteDesktop -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname>
```

> [!div class="mx-imgBorder"]
> ![Screenshot des PowerShell-Cmdlets Get-RDSRemoteApp mit hervorgehobenem Namen und Anzeigenamen.](../media/get-rdsremotedesktop.png)

Führen Sie zum Zuweisen eines Anzeigenamens zur Remotedesktopressource das folgende PowerShell-Cmdlet aus:

```powershell
Set-RdsRemoteDesktop -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname> -FriendlyName <newfriendlyname>
```

> [!div class="mx-imgBorder"]
> ![Screenshot des PowerShell-Cmdlets Set-RDSRemoteApp mit hervorgehobenem Namen und neuem Anzeigenamen.](../media/set-rdsremotedesktop.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie den Feed für Benutzer angepasst haben, können Sie sich bei einem Azure Virtual Desktop-Client anmelden, um ihn zu testen. Wechseln Sie dafür zu den Anleitungen für das Herstellen einer Verbindung mit Azure Virtual Desktop:

 * [Herstellen einer Verbindung über Windows 10 oder Windows 7](connect-windows-7-10-2019.md)
 * [Herstellen einer Verbindung über einen Webbrowser](connect-web-2019.md)
