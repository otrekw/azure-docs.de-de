---
title: 'Delegierter Zugriff in Windows Virtual Desktop (klassisch): Azure'
description: Es wird beschrieben, wie Sie Verwaltungsfunktionen in einer Windows Virtual Desktop-Bereitstellung (klassisch) delegieren, und es werden Beispiele gezeigt.
author: Heidilohr
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 0152dc5102fd3f77418448234cab1234d25b97c6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88008150"
---
# <a name="delegated-access-in-windows-virtual-desktop-classic"></a>Delegierter Zugriff in Windows Virtual Desktop (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop (klassisch). Der Dienst unterstützt keine Windows Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Windows Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../delegated-access-virtual-desktop.md) weiter.

Windows Virtual Desktop verfügt über ein Modell mit delegiertem Zugriff, mit dem Sie für Benutzer eine bestimmte Zugriffsebene festlegen können, indem Sie ihnen eine Rolle zuweisen. Eine Rollenzuweisung besteht aus drei Komponenten: Sicherheitsprinzipal, Rollendefinition und Bereich. Das Modell für delegierten Zugriff von Windows Virtual Desktop basiert auf dem Azure RBAC-Modell (rollenbasierte Zugriffssteuerung). Weitere Informationen zu bestimmten Rollenzuweisungen und den zugehörigen Komponenten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../../role-based-access-control/built-in-roles.md).

Beim delegierten Zugriff von Windows Virtual Desktop werden für jedes Element der Rollenzuweisung die folgenden Werte unterstützt:

* Sicherheitsprinzipal
    * Benutzer
    * Dienstprinzipale
* Rollendefinition
    * Integrierte Rollen
* `Scope`
    * Mandantengruppen
    * Mandanten
    * Hostpools
    * App-Gruppen

## <a name="built-in-roles"></a>Integrierte Rollen

Der delegierte Zugriff in Windows Virtual Desktop verfügt über mehrere integrierte Rollendefinitionen, die Sie Benutzern und Dienstprinzipalen zuweisen können.

* RDS-Besitzer können alles verwalten, einschließlich des Zugriffs auf Ressourcen.
* RDS-Mitwirkende können alles verwalten, besitzen jedoch keinen Zugriff auf Ressourcen.
* RDS-Leser können alles anzeigen, jedoch keine Änderungen vornehmen.
* RDS-Bediener können Diagnoseaktivitäten anzeigen.

## <a name="powershell-cmdlets-for-role-assignments"></a>PowerShell-Cmdlets für Rollenzuweisungen

Sie können die folgenden Cmdlets ausführen, um Rollenzuweisungen zu erstellen, anzuzeigen und zu entfernen:

* Mit **Get-RdsRoleAssignment** wird eine Liste mit Rollenzuweisungen angezeigt.
* Mit **New-RdsRoleAssignment** wird eine neue Rollenzuweisung erstellt.
* Mit **Remove-RdsRoleAssignment** werden Rollenzuweisungen gelöscht.

### <a name="accepted-parameters"></a>Akzeptierte Parameter

Sie können die drei grundlegenden Cmdlets mit den folgenden Parametern ändern:

* **AadTenantId**: Gibt die ID des Azure Active Directory-Mandanten an, dessen Mitglied der Dienstprinzipal ist.
* **AppGroupName**: Der Name der Remotedesktop-App-Gruppe.
* **Diagnostics**: Gibt den Diagnosebereich an. (Muss entweder mit dem Parameter **Infrastructure** oder **Tenant** gekoppelt werden.)
* **HostPoolName**: Name des Remotedesktop-Hostpools.
* **Infrastructure**: Gibt den Infrastrukturbereich an.
* **RoleDefinitionName**: Der Name der Remotedesktopdienste-Rolle der rollenbasierten Zugriffssteuerung, die dem Benutzer, der Gruppe oder der App zugewiesen ist. (Beispiel: Remotedesktopdienste-Besitzer, Remotedesktopdienste-Leser usw.)
* **ServerPrincipleName**: Der Name der Azure Active Directory-Anwendung.
* **SignInName**: Die E-Mail-Adresse oder der Benutzerprinzipalname des Benutzers.
* **TenantName**: Der Name des Remotedesktop-Mandanten.

## <a name="next-steps"></a>Nächste Schritte

Eine vollständigere Liste mit PowerShell-Cmdlets, die von den einzelnen Rollen verwendet werden können, finden Sie in der [PowerShell-Referenz](/powershell/windows-virtual-desktop/overview).

Eine Anleitung zum Einrichten einer Windows Virtual Desktop-Umgebung finden Sie unter [Windows Virtual Desktop-Umgebung](environment-setup-2019.md).
