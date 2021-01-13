---
title: 'Delegierter Zugriff in Windows Virtual Desktop: Azure'
description: Es wird beschrieben, wie Sie Verwaltungsfunktionen in einer Windows Virtual Desktop-Bereitstellung delegieren, und es werden Beispiele gezeigt.
author: Heidilohr
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: f2aa2c74704cf89c082d2837b39e82902efa0a62
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88010054"
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
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <appgroupname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

Führen Sie das folgende Cmdlet aus, um einer App-Gruppe eine Azure Active Directory-Benutzergruppe hinzuzufügen:

```powershell
New-AzRoleAssignment -ObjectId <usergroupobjectid> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <appgroupname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

## <a name="next-steps"></a>Nächste Schritte

Eine vollständigere Liste mit PowerShell-Cmdlets, die von den einzelnen Rollen verwendet werden können, finden Sie in der [PowerShell-Referenz](/powershell/windows-virtual-desktop/overview).

Eine vollständige Liste der Rollen, die in Azure RBAC unterstützt werden, finden Sie unter [Integrierten Azure-Rollen](../role-based-access-control/built-in-roles.md).

Eine Anleitung zum Einrichten einer Windows Virtual Desktop-Umgebung finden Sie unter [Windows Virtual Desktop-Umgebung](environment-setup.md).
