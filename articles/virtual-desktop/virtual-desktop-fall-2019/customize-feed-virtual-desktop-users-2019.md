---
title: Anpassen des Feeds für Benutzer von Windows Virtual Desktop (klassisch) – Azure
description: Erfahren Sie, wie Sie den Feed für Benutzer von Windows Virtual Desktop (klassisch) mit PowerShell-Cmdlets anpassen.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: cd7496690ec88fbe4297386c32d1b8a2c3234577
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91540760"
---
# <a name="customize-feed-for-windows-virtual-desktop-classic-users"></a>Anpassen des Feeds für Benutzer von Windows Virtual Desktop (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für den Windows Virtual Desktop-Dienst (klassisch), der keine Windows Virtual Desktop-Objekte in Azure Resource Manager unterstützt. Wenn Sie Windows Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../customize-feed-for-virtual-desktop-users.md) weiter.

Sie können den Feed anpassen, damit die RemoteApp- und Remotedesktopressourcen den Benutzern auf erkennbare Weise angezeigt werden.

Zunächst müssen Sie das [Windows Virtual Desktop-PowerShell-Modul herunterladen und importieren](/powershell/windows-virtual-desktop/overview/), um es in Ihrer PowerShell-Sitzung verwenden zu können. Führen Sie anschließend das folgende Cmdlet aus, um sich bei Ihrem Konto anzumelden:

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

Nachdem Sie den Feed für Benutzer angepasst haben, können Sie sich bei einem Windows Virtual Desktop-Client anmelden, um ihn zu testen. Wechseln Sie dafür zu den Vorgehensweisen für das Herstellen einer Verbindung mit Windows Virtual Desktop:

 * [Herstellen einer Verbindung über Windows 10 oder Windows 7](connect-windows-7-10-2019.md)
 * [Herstellen einer Verbindung über einen Webbrowser](connect-web-2019.md)
