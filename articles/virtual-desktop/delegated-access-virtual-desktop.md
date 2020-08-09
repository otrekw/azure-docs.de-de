---
title: 'Delegierter Zugriff in Windows Virtual Desktop: Azure'
description: Es wird beschrieben, wie Sie Verwaltungsfunktionen in einer Windows Virtual Desktop-Bereitstellung delegieren, und es werden Beispiele gezeigt.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: df4d1b98eac782641fb36c614d2df9508c6131ad
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87498666"
---
# <a name="delegated-access-in-windows-virtual-desktop"></a>Delegierter Zugriff in Windows Virtual Desktop

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Windows Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/delegated-access-virtual-desktop-2019.md).

Windows Virtual Desktop verfügt über ein Modell mit delegiertem Zugriff, mit dem Sie für Benutzer eine bestimmte Zugriffsebene festlegen können, indem Sie ihnen eine Rolle zuweisen. Eine Rollenzuweisung besteht aus drei Komponenten: Sicherheitsprinzipal, Rollendefinition und Bereich. Das Modell für delegierten Zugriff von Windows Virtual Desktop basiert auf dem Azure RBAC-Modell (rollenbasierte Zugriffssteuerung). Weitere Informationen zu bestimmten Rollenzuweisungen und den zugehörigen Komponenten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../role-based-access-control/built-in-roles.md).

Beim delegierten Zugriff von Windows Virtual Desktop werden für jedes Element der Rollenzuweisung die folgenden Werte unterstützt:

* Sicherheitsprinzipal
    * Benutzer
    * Benutzergruppen
    * Dienstprinzipale
* Rollendefinition
    * Integrierte Rollen
    * Benutzerdefinierte Rollen
* `Scope`
    * Hostpools
    * App-Gruppen
    * Arbeitsbereiche

## <a name="powershell-cmdlets-for-role-assignments"></a>PowerShell-Cmdlets für Rollenzuweisungen

Bevor Sie beginnen, stellen Sie sicher, dass Sie die Anweisungen unter [Einrichten des PowerShell-Moduls](powershell-module.md) befolgt haben, um das Windows Virtual Desktop PowerShell-Modul einzurichten, falls dies noch nicht geschehen ist.

Windows Virtual Desktop verwendet rollenbasierte Zugriffssteuerung (Azure RBAC) von Azure beim Veröffentlichen von App-Gruppen für Benutzer oder Benutzergruppen. Die Benutzerrolle „Desktopvirtualisierung“ wird dem Benutzer oder der Benutzergruppe zugewiesen, und der Bereich ist die App-Gruppe. Diese Rolle gewährt dem Benutzer speziellen Datenzugriff auf die App-Gruppe.  

Führen Sie das folgende Cmdlet aus, um einer App-Gruppe Azure Active Directory-Benutzer hinzuzufügen:

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <hostpoolname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'  
```

Führen Sie das folgende Cmdlet aus, um einer App-Gruppe eine Azure Active Directory-Benutzergruppe hinzuzufügen:

```powershell
New-AzRoleAssignment -ObjectId <usergroupobjectid> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <hostpoolname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups' 
```

## <a name="next-steps"></a>Nächste Schritte

Eine vollständigere Liste mit PowerShell-Cmdlets, die von den einzelnen Rollen verwendet werden können, finden Sie in der [PowerShell-Referenz](/powershell/windows-virtual-desktop/overview).

Eine vollständige Liste der Rollen, die in Azure RBAC unterstützt werden, finden Sie unter [Integrierten Azure-Rollen](../role-based-access-control/built-in-roles.md).

Eine Anleitung zum Einrichten einer Windows Virtual Desktop-Umgebung finden Sie unter [Windows Virtual Desktop-Umgebung](environment-setup.md).
