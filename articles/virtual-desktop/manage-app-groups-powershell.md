---
title: Verwalten von App-Gruppen für Azure Virtual Desktop PowerShell – Azure
description: Erfahren Sie, wie Sie Azure Virtual Desktop-App-Gruppen mit PowerShell verwalten.
author: Heidilohr
ms.topic: how-to
ms.date: 04/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: a900d1d92509fab7b777ca5864a51c7699cb294e
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111749073"
---
# <a name="manage-app-groups-using-powershell"></a>Verwalten von App-Gruppen mit PowerShell

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop mit Azure Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Azure Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/manage-app-groups-2019.md).

Die Standard-App-Gruppe, die für einen neuen Azure Virtual Desktop-Hostpool erstellt wird, veröffentlicht auch den vollständigen Desktop. Zusätzlich können Sie RemoteApp-Anwendungsgruppen für den Hostpool erstellen. In diesem Tutorial erfahren Sie, wie Sie eine RemoteApp-Gruppe erstellen und individuelle **Startmenü**-Apps veröffentlichen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer RemoteApp-Gruppe
> * Gewähren von Zugriff auf RemoteApp-Programme

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie die Anweisungen unter [Einrichten des PowerShell-Moduls](powershell-module.md) zum Einrichten Ihres PowerShell-Moduls und zum Anmelden bei Ihrem Azure-Konto befolgt haben.

## <a name="create-a-remoteapp-group"></a>Erstellen einer RemoteApp-Gruppe

So erstellen Sie eine RemoteApp-Gruppe mit PowerShell:

1. Führen Sie das folgende PowerShell-Cmdlet aus, um eine neue leere RemoteApp-App-Gruppe zu erstellen:

   ```powershell
   New-AzWvdApplicationGroup -Name <appgroupname> -ResourceGroupName <resourcegroupname> -ApplicationGroupType "RemoteApp" -HostPoolArmPath '/subscriptions/SubscriptionId/resourcegroups/ResourceGroupName/providers/Microsoft.DesktopVirtualization/hostPools/HostPoolName'-Location <azureregion>
   ```

2. Optional: Vergewissern Sie sich, dass die App-Gruppe erstellt wurde. Führen Sie hierzu das folgende Cmdlet aus, um eine Liste mit allen App-Gruppen für den Hostpool anzuzeigen.

   ```powershell
   Get-AzWvdApplicationGroup -Name <appgroupname> -ResourceGroupName <resourcegroupname>
   ```

3. Führen Sie das folgende Cmdlet aus, um eine Liste mit allen **Startmenü**-Apps im VM-Image des Hostpools abzurufen. Notieren Sie sich die Werte für **FilePath**, **IconPath** und **IconIndex** sowie weitere wichtige Informationen für die zu veröffentlichende Anwendung.

   ```powershell
   Get-AzWvdStartMenuItem -ApplicationGroupName <appgroupname> -ResourceGroupName <resourcegroupname> | Format-List | more
   ```

   In der Ausgabe sollten alle Startmenüelemente in einem Format wie dem folgenden angezeigt werden:

   ```powershell
   AppAlias            : access
   CommandLineArgument :
   FilePath            : C:\Program Files\Microsoft Office\root\Office16\MSACCESS.EXE
   FriendlyName        :
   IconIndex           : 0
   IconPath            : C:\Program Files\Microsoft Office\Root\VFS\Windows\Installer\{90160000-000F-0000-1000-0000000FF1CE}\accicons.exe
   Id                  : /subscriptions/resourcegroups/providers/Microsoft.DesktopVirtualization/applicationgroups/startmenuitems/Access
   Name                : 0301RAG/Access
   Type                : Microsoft.DesktopVirtualization/applicationgroups/startmenuitems

   AppAlias            : charactermap
   CommandLineArgument :
   FilePath            : C:\windows\system32\charmap.exe
   FriendlyName        :
   IconIndex           : 0
   IconPath            : C:\windows\system32\charmap.exe
   Id                  : /subscriptions/resourcegroups/providers/Microsoft.DesktopVirtualization/applicationgroups/startmenuitems/Character Map
   Name                : 0301RAG/Character Map
   Type                : Microsoft.DesktopVirtualization/applicationgroups/startmenuitems
   ```

4. Führen Sie das folgende Cmdlet aus, um die Anwendung auf der Grundlage ihres App-Alias (`AppAlias`) zu installieren. `AppAlias` ist in der Ausgabe von Schritt 3 enthalten.

   ```powershell
   New-AzWvdApplication -AppAlias <appalias> -GroupName <appgroupname> -Name <remoteappname> -ResourceGroupName <resourcegroupname> -CommandLineSetting <DoNotAllow|Allow|Require>
   ```

5. Optional: Führen Sie das folgende Cmdlet aus, um ein neues RemoteApp-Programm in der in Schritt 1 erstellten Anwendungsgruppe zu veröffentlichen.

   ```powershell
   New-AzWvdApplication -GroupName <appgroupname> -Name <remoteappname> -ResourceGroupName <resourcegroupname> -Filepath <filepath> -IconPath <iconpath> -IconIndex <iconindex> -CommandLineSetting <DoNotAllow|Allow|Require>
   ```

6. Führen Sie das folgende Cmdlet aus, um sich zu vergewissern, dass die App veröffentlicht wurde:

   ```powershell
   Get-AzWvdApplication -GroupName <appgroupname> -ResourceGroupName <resourcegroupname>
   ```

7. Wiederholen Sie die Schritte 1 bis 5 für jede Anwendung, die Sie für diese App-Gruppe veröffentlichen möchten.
8. Führen Sie das folgende Cmdlet aus, um Benutzern Zugriff auf die RemoteApp-Programme in der App-Gruppe zu gewähren.

   ```powershell
   New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <appgroupname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
   ```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie aus unseren Tutorials zu dieser Vorgehensweise gelangt sind, lesen Sie [Erstellen eines Hostpools zum Überprüfen von Dienstupdates](create-validation-host-pool.md). Mit einem Überprüfungshostpool können Sie Dienstupdates überwachen, bevor Sie sie in Ihrer Produktionsumgebung bereitstellen.