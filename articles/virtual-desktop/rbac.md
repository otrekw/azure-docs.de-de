---
title: 'Integrierte Rollen für Windows Virtual Desktop: Azure'
description: Hier finden Sie eine Übersicht über integrierte Rollen für Windows Virtual Desktop, die für Azure RBAC verfügbar sind.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 12/15/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 52f0151fd4be7505cf7beea0eeb54f8e34404997
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577561"
---
# <a name="built-in-roles-for-windows-virtual-desktop"></a>Integrierte Rollen für Windows Virtual Desktop

Windows Virtual Desktop verwendet die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) von Azure, um Benutzern und Administratoren Rollen zuzuweisen. Diese Rollen erteilen Administratoren die Berechtigung, bestimmte Aufgaben auszuführen. Weitere Informationen zu integrierten Rollen für Azure RBAC finden Sie unter [Integrierte Azure-Rollen](../role-based-access-control/built-in-roles.md).

Die standardmäßigen integrierten Rollen für Azure sind Besitzer, Mitwirkender und Leser. Windows Virtual Desktop verfügt jedoch über zusätzliche Rollen, mit denen Sie Verwaltungsrollen für Hostpools, App-Gruppen und Arbeitsbereiche trennen können. Diese Trennung ermöglicht Ihnen eine genauere Steuerung der administrativen Aufgaben. Diese Rollen sind gemäß den Azure-Standardrollen und der Methodik minimaler Berechtigungen benannt.

Windows Virtual Desktop verfügt über keine bestimmte Besitzerrolle. Allerdings können Sie für die Dienstobjekte eine Standardbesitzerrolle verwenden.

## <a name="desktop-virtualization-contributor"></a>Desktopvirtualisierungsmitwirkender

Mit der Rolle „Desktopvirtualisierungsmitwirkender“ können Sie alle Aspekte der Bereitstellung verwalten. Sie gewährt Ihnen jedoch keinen Zugriff auf Computeressourcen. Außerdem benötigen Sie die Rolle „Benutzerzugriffsadministrator“, um App-Gruppen für Benutzer oder Benutzergruppen zu veröffentlichen.


- Microsoft.DesktopVirtualization/\* 
- Microsoft.Resources/subscriptions/resourceGroups/read
- Microsoft.Resources/deployments/\*
- Microsoft.Authorization/\*/read
- Microsoft.Insights/alertRules/\*
- Microsoft.Support/\*

## <a name="desktop-virtualization-reader"></a>Desktopvirtualisierungsleser

Mit der Rolle „Desktopvirtualisierungsleser“ können Sie alle Elemente in der Bereitstellung anzeigen, aber keine Änderungen vornehmen.

- Microsoft.DesktopVirtualization/\*/read
- Microsoft.Resources/subscriptions/resourceGroups/read
- Microsoft.Resources/deployments/read
- Microsoft.Authorization/\*/read
- Microsoft.Insights/alertRules/\*
- Microsoft.Support/\*

## <a name="host-pool-contributor"></a>Hostpoolmitwirkender

Mit der Rolle „Hostpoolmitwirkender“ können Sie alle Aspekte von Hostpools einschließlich des Zugriffs auf Ressourcen verwalten. Sie benötigen eine zusätzliche Mitwirkenderrolle, „Mitwirkender von virtuellen Computern“, um virtuelle Computer zu erstellen. Sie benötigen AppGroup- und Arbeitsbereichsmitwirkender-Rollen zum Erstellen eines Hostpools über das Portal, oder Sie können die Rolle „Desktopvirtualisierungsmitwirkender“ verwenden.

In der folgenden Liste wird beschrieben, auf welche Berechtigungen diese Rolle zugreifen kann:

- Microsoft.DesktopVirtualization/hostpools/\*
- Microsoft.Resources/subscriptions/resourceGroups/read
- Microsoft.Resources/deployments/\*
- Microsoft.Authorization/\*/read
- Microsoft.Insights/alertRules/\*
- Microsoft.Support/\*

## <a name="host-pool-reader"></a>Hostpoolleser

Mit der Rolle „Hostpoolleser“ können Sie alle Elemente im Hostpool anzeigen, jedoch keine Änderungen vornehmen.

- Microsoft.DesktopVirtualization/hostpools/\*/read
- Microsoft.Resources/subscriptions/resourceGroups/read
- Microsoft.Resources/deployments/read
- Microsoft.Authorization/\*/read
- Microsoft.Insights/alertRules/\*
- Microsoft.Support/\*

## <a name="application-group-contributor"></a>Anwendungsgruppenmitwirkender

Mit der Rolle „Anwendungsgruppenmitwirkender“ können Sie alle Aspekte von App-Gruppen verwalten. Um App-Gruppen für Benutzer oder Benutzergruppen zu veröffentlichen, benötigen Sie die Rolle „Benutzerzugriffsadministrator“.

In der folgenden Liste wird beschrieben, auf welche Berechtigungen diese Rolle zugreifen kann:

- Microsoft.DesktopVirtualization/applicationgroups/\*
- Microsoft.DesktopVirtualization/hostpools/read
- Microsoft.DesktopVirtualization/hostpools/sessionhosts/read
- Microsoft.Resources/subscriptions/resourceGroups/read
- Microsoft.Resources/deployments/\*
- Microsoft.Authorization/\*/read
- Microsoft.Insights/alertRules/\*
- Microsoft.Support/\*

## <a name="application-group-reader"></a>Anwendungsgruppenleser

Mit der Rolle „Anwendungsgruppenleser“ können Sie alle Elemente in der App-Gruppe anzeigen, aber keine Änderungen vornehmen.

In der folgenden Liste wird beschrieben, auf welche Berechtigungen diese Rolle zugreifen kann:

- Microsoft.DesktopVirtualization/applicationgroups/\*/read
- Microsoft.DesktopVirtualization/applicationgroups/read
- Microsoft.DesktopVirtualization/hostpools/read
- Microsoft.DesktopVirtualization/hostpools/sessionhosts/read
- Microsoft.Resources/subscriptions/resourceGroups/read
- Microsoft.Resources/deployments/read
- Microsoft.Authorization/\*/read
- Microsoft.Insights/alertRules/\*
- Microsoft.Support/\*

## <a name="workspace-contributor"></a>Arbeitsbereichsmitwirkender

Mit der Rolle „Arbeitsbereichsmitwirkender“ können Sie alle Aspekte von Arbeitsbereichen verwalten. Um Informationen zu Anwendungen zu erhalten, die den App-Gruppen hinzugefügt werden, muss Ihnen auch die Rolle „Anwendungsgruppenleser“ zugewiesen werden.

In der folgenden Liste wird beschrieben, auf welche Berechtigungen diese Rolle zugreifen kann:

- Microsoft.DesktopVirtualization/workspaces/\*
- Microsoft.DesktopVirtualization/applicationgroups/read
- Microsoft.Resources/subscriptions/resourceGroups/read
- Microsoft.Resources/deployments/\*
- Microsoft.Authorization/\*/read
- Microsoft.Insights/alertRules/\*
- Microsoft.Support/\*

## <a name="workspace-reader"></a>Arbeitsbereichsleser

Mit der Rolle „Arbeitsbereichsleser“ können Sie alle Elemente im Arbeitsbereich anzeigen, jedoch keine Änderungen vornehmen.

In der folgenden Liste wird beschrieben, auf welche Berechtigungen diese Rolle zugreifen kann:

- Microsoft.DesktopVirtualization/workspaces/read
- Microsoft.DesktopVirtualization/applicationgroups/read
- Microsoft.Resources/subscriptions/resourceGroups/read
- Microsoft.Resources/deployments/read
- Microsoft.Authorization/\*/read
- Microsoft.Insights/alertRules/\*
- Microsoft.Support/\*

## <a name="user-session-operator"></a>Benutzersitzungsoperator

Mit der Rolle „Benutzersitzungsoperator“ können Sie Nachrichten senden, Sitzungen trennen und die Funktion „Abmelden“ verwenden, um Sitzungen beim Sitzungshost abzumelden. Mit dieser Rolle können Sie jedoch keine Aktionen zur Sitzungshostverwaltung wie Entfernen des Sitzungshosts, Ändern des Ausgleichsmodus usw. durchführen. Diese Rolle kann Zuweisungen anzeigen, aber keine Administratoren ändern. Es wird empfohlen, diese Rolle bestimmten Hostpools zuzuweisen. Wenn Sie diese Berechtigung auf Ressourcengruppenebene erteilen, verfügt der Administrator über die Leseberechtigung für alle Hostpools unter einer Ressourcengruppe.

In der folgenden Liste wird beschrieben, auf welche Berechtigungen diese Rolle zugreifen kann:

- Microsoft.DesktopVirtualization/hostpools/read
- Microsoft.DesktopVirtualization/hostpools/sessionhosts/read
- Microsoft.DesktopVirtualization/hostpools/sessionhosts/usersessions/\*
- Microsoft.Resources/subscriptions/resourceGroups/read
- Microsoft.Resources/deployments/read
- Microsoft.Authorization/\*/read
- Microsoft.Insights/alertRules/\*
- Microsoft.Support/\*

## <a name="session-host-operator"></a>Sitzungshostoperator

Mit der Rolle „Sitzungshostmitwirkender“ können Sie Sitzungshosts anzeigen und entfernen sowie den Ausgleichsmodus ändern. Diese Rolle kann nicht Sitzungshosts mithilfe des Azure-Portals hinzufügen, da sie nicht über die Schreibberechtigung für Hostpoolobjekte verfügt. Wenn das Registrierungstoken gültig ist (generiert und nicht abgelaufen), können Sie diese Rolle außerhalb des Azure-Portals verwenden, um dem Hostpool Sitzungshosts hinzuzufügen, wenn der Administrator über die Rolle „Mitwirkender von virtuellen Computern“ über Computeberechtigungen verfügt.

In der folgenden Liste wird beschrieben, auf welche Berechtigungen diese Rolle zugreifen kann:

- Microsoft.DesktopVirtualization/hostpools/read
- Microsoft.DesktopVirtualization/hostpools/sessionhosts/\*
- Microsoft.Resources/subscriptions/resourceGroups/read
- Microsoft.Resources/deployments/read
- Microsoft.Authorization/\*/read
- Microsoft.Insights/alertRules/\*
- Microsoft.Support/\*
