---
title: Windows Virtual Desktop – Zuweisungstyp für persönliche Desktops – Azure
description: Konfigurieren der automatischen oder direkten Zuweisung für einen Windows Virtual Desktop-Hostpool mit persönlichen Desktops.
services: virtual-desktop
author: HeidiLohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 07/09/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 17df23a16bec91e5c333bc760143eac250b62699
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87292631"
---
# <a name="configure-the-personal-desktop-host-pool-assignment-type"></a>Konfigurieren des Zuweisungstyps für den Hostpool mit persönlichen Desktops

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Windows Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/configure-host-pool-personal-desktop-assignment-type-2019.md).

Sie können den Zuweisungstyp Ihres Hostpools mit persönlichen Desktops konfigurieren, um Ihre Windows Virtual Desktop-Umgebung besser an Ihre Bedürfnisse anzupassen. In diesem Thema erfahren Sie, wie Sie die automatische oder direkte Zuweisung für Ihre Benutzer konfigurieren.

>[!NOTE]
> Die Anweisungen in diesem Artikel gelten nur für Hostpools mit persönlichen Desktops, nicht für gepoolte Hostpools, da Benutzer in gepoolten Hostpools keinen bestimmten Sitzungshosts zugewiesen sind.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie das Windows Virtual Desktop PowerShell-Modul bereits heruntergeladen und installiert haben. Wenn dies nicht der Fall ist, befolgen Sie die Anweisungen unter [Einrichten des PowerShell-Moduls](powershell-module.md).

## <a name="configure-automatic-assignment"></a>Konfigurieren der automatischen Zuweisung

Die automatische Zuweisung ist der Standardzuweisungstyp für neue Hostpools mit persönlichen Desktops, die in Ihrer Windows Virtual Desktop-Umgebung erstellt werden. Für das automatische Zuweisen von Benutzern ist kein bestimmter Sitzungshost erforderlich.

Um Benutzer automatisch zuzuweisen, weisen Sie sie zunächst dem Hostpool mit persönlichen Desktops zu, damit sie den Desktop in ihrem Feed sehen können. Wenn ein zugewiesener Benutzer den Desktop in seinem Feed startet, beansprucht er einen verfügbaren Sitzungshost, wenn er nicht bereits eine Verbindung mit dem Hostpool hergestellt hat, wodurch der Zuweisungsvorgang abgeschlossen wird.

Führen Sie das folgende PowerShell-Cmdlet aus, um einen Hostpool für das automatische Zuweisen von Benutzern zu VMs zu konfigurieren:

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -PersonalDesktopAssignmentType Automatic
```

Führen Sie das folgende PowerShell-Cmdlet aus, um einen Benutzer zum Hostpool mit persönlichen Desktops zuzuweisen:

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <appgroupname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

## <a name="configure-direct-assignment"></a>Konfigurieren der direkten Zuweisung

Im Gegensatz zur automatischen Zuweisung müssen Sie bei der direkten Zuweisung den Benutzer sowohl dem Hostpool mit persönlichen Desktops als auch einem bestimmten Sitzungshost zuweisen, bevor er sich mit seinem persönlichen Desktop verbinden kann. Wenn der Benutzer nur einem Hostpool ohne eine Sitzungshostzuweisung zugewiesen ist, kann er nicht auf Ressourcen zugreifen.

Um einen Hostpool so zu konfigurieren, dass eine direkte Zuweisung von Benutzern zu Sitzungshosts erforderlich ist, führen Sie das folgende PowerShell-Cmdlet aus:

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -PersonalDesktopAssignmentType Direct
```

Führen Sie das folgende PowerShell-Cmdlet aus, um einen Benutzer zum Hostpool mit persönlichen Desktops zuzuweisen:

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <appgroupname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

Um einen Benutzer einem bestimmten Sitzungshost zuzuweisen, führen Sie das folgende PowerShell-Cmdlet aus:

```powershell
Update-AzWvdSessionHost -HostPoolName <hostpoolname> -Name <sessionhostname> -ResourceGroupName <resourcegroupname> -AssignedUser <userupn>
```

So weisen Sie im Azure-Portal einen Benutzer einem Sitzungshost direkt zu

1. Melden Sie sich unter <https://portal.azure.com> beim Azure-Portal an.
2. Geben Sie **Windows Virtual Desktop** in die Suchleiste ein.
3. Wählen Sie unter **Dienste** die Option **Windows Virtual Desktop** aus.
4. Klicken Sie auf der Seite „Windows Virtual Desktop“ im Menü auf der linken Seite des Fensters auf **Hostpools**.
5. Klicken Sie auf den Namen des Hostpools, den Sie aktualisieren möchten.
6. Navigieren Sie als Nächstes zum Menü auf der linken Seite des Fensters, und klicken Sie auf **Anwendungsgruppen**.
7. Klicken Sie auf den Namen der Desktop-App-Gruppe, die Sie bearbeiten möchten, und dann im Menü auf der linken Seite des Fensters auf **Zuweisungen**.
8. Klicken Sie auf **+ Hinzufügen**, und wählen Sie dann die Benutzer oder Benutzergruppen aus, für die Sie diese Desktop-App-Gruppe veröffentlichen möchten.
9. Klicken Sie in der Informationsleiste auf **Assign VM** (VM zuweisen), um einem Benutzer einen Sitzungshost zuzuweisen.
10. Wählen Sie den Sitzungshost aus, den Sie dem Benutzer zuweisen möchten, und klicken Sie dann auf **Zuweisen**.
11. Wählen Sie in der Liste der verfügbaren Benutzer den Benutzer aus, dem Sie den Sitzungshost zuweisen möchten.
12. Klicken Sie auf **Auswählen**, wenn Sie fertig sind.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie den persönlichen Desktopzuweisungstyp konfiguriert haben, können Sie sich an einem Windows Virtual Desktop-Client anmelden, um ihn als Teil einer Benutzersitzung zu testen. In den nächsten zwei Vorgehensweisen erfahren Sie, wie Sie mit dem ausgewählten Client eine Verbindung zu einer Sitzung herstellen:

- [Herstellen einer Verbindung mit dem Windows-Desktopclient](connect-windows-7-10.md)
- [Herstellen einer Verbindung mit dem Webclient](connect-web.md)
- [Herstellen einer Verbindung mit dem Android-Client](connect-android.md)
- [Herstellen einer Verbindung mit dem iOS-Client](connect-ios.md)
- [Herstellen einer Verbindung mit dem macOS-Client](connect-macos.md)