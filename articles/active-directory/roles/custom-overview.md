---
title: Übersicht über die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure Active Directory
description: Hier finden Sie Informationen zu den Komponenten einer Rollenzuweisung und eines eingeschränkten Bereichs in Azure Active Directory.
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: overview
ms.date: 11/20/2020
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d7c0c7c5a0fe5ff3100e754c99b476676cab222
ms.sourcegitcommit: ee8ce2c752d45968a822acc0866ff8111d0d4c7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2021
ms.locfileid: "113733374"
---
# <a name="overview-of-role-based-access-control-in-azure-active-directory"></a>Übersicht über die rollenbasierte Zugriffssteuerung in Azure Active Directory

In diesem Artikel werden die Grundlagen der rollenbasierten Zugriffssteuerung in Azure Active Directory (Azure AD) beschrieben. Mithilfe von Azure AD-Rollen können Sie Ihren Administratoren differenzierte Berechtigungen erteilen und so das Prinzip der geringsten Rechte implementieren. Den integrierten und benutzerdefinierten Rollen in Azure AD liegen ähnliche Konzepte zugrunde wie dem [rollenbasierten Zugriffssteuerungssystem für Azure-Ressourcen](../../role-based-access-control/overview.md) (Azure-Rollen). [Die beiden rollenbasierten Zugriffssteuerungssysteme unterscheiden sich](../../role-based-access-control/rbac-and-directory-admin-roles.md) wie folgt:

- Azure AD-Rollen dienen zum Steuern des Zugriffs auf Azure AD-Ressourcen wie Benutzer, Gruppen und Anwendungen unter Verwendung der Graph-API.
- Azure-Rollen dienen zum Steuern des Zugriffs auf Azure-Ressourcen wie virtuelle Computer oder Speicher unter Verwendung der Azure-Ressourcenverwaltung.

Beide Systeme enthalten ähnlich verwendete Rollendefinitionen und Rollenzuweisungen. Azure AD-Rollenberechtigungen können allerdings nicht in benutzerdefinierten Azure-Rollen verwendet werden (und umgekehrt).

## <a name="understand-azure-ad-role-based-access-control"></a>Grundlegendes zur rollenbasierten Zugriffssteuerung von Azure AD
Azure AD unterstützt zwei Arten von Rollendefinitionen:
* [Integrierte Rollen](./permissions-reference.md)
* [Benutzerdefinierte Rollen](./custom-create.md)

Integrierte Rollen sind vordefinierte Rollen mit einem festen Berechtigungssatz. Diese Rollendefinitionen können nicht geändert werden. Es gibt viele [integrierte Rollen](./permissions-reference.md), die von Azure AD unterstützt werden, und die Liste wird immer länger. Zur Vervollständigung und zum Erfüllen Ihrer speziellen Anforderungen unterstützt Azure AD aber auch [benutzerdefinierte Rollen](./custom-create.md). Das Erteilen von Berechtigungen mithilfe benutzerdefinierter Azure AD-Rollen ist ein zweistufiger Prozess, bei dem eine benutzerdefinierte Rollendefinition erstellt und dann mithilfe einer Rollenzuweisung zugewiesen wird. Eine benutzerdefinierte Rollendefinition ist eine Sammlung von Berechtigungen, die Sie aus einer vordefinierten Liste hinzufügen. Bei diesen Berechtigungen handelt es sich um die gleichen Berechtigungen, die in den integrierten Rollen verwendet werden.  

Nachdem Sie Ihre benutzerdefinierte Rollendefinition erstellt (oder eine integrierte Rolle verwendet) haben, können Sie diese durch Erstellen einer Rollenzuweisung einem Benutzer zuweisen. Eine Rollenzuweisung erteilt einem Benutzer die Berechtigungen in einer Rollendefinition für einen bestimmten Bereich. Durch diesen zweistufigen Prozess können Sie eine einzelne Rollendefinition erstellen und dann für verschiedene Bereiche mehrmals zuweisen. Ein Bereich definiert die Gruppe von Azure AD-Ressourcen, auf die das Rollenmitglied Zugriff hat. Der gängigste Bereich ist der organisationsweite Bereich. Eine benutzerdefinierte Rolle kann für den organisationsweiten Bereich zugewiesen werden. Dies bedeutet, dass das Rollenmitglied über die Rollenberechtigungen für alle Ressourcen in der Organisation verfügt. Eine benutzerdefinierte Rolle kann auch für einen Objektbereich zugewiesen werden. Ein Beispiel für einen Objektbereich ist eine einzelne Anwendung. Die gleiche Rolle kann einem Benutzer für alle Anwendungen in der Organisation und dann einem anderen Benutzer nur für die App der Contoso-Spesenabrechnungen zugewiesen werden.  

### <a name="how-azure-ad-determines-if-a-user-has-access-to-a-resource"></a>So ermittelt Azure AD, ob ein Benutzer Zugriff auf eine Ressource hat

Im Folgenden finden Sie die allgemeinen Schritte, anhand derer Azure AD ermittelt, ob Sie Zugriff auf eine Verwaltungsressource haben. Verwenden Sie diese Informationen, um Zugriffsprobleme zu beheben.

1. Ein Benutzer (oder Dienstprinzipal) ruft ein Token für den Microsoft Graph- oder Azure AD Graph-Endpunkt ab.
1. Der Benutzer sendet mithilfe des ausgestellten Tokens über Microsoft Graph oder Azure AD Graph einen API-Aufruf an Azure Active Directory (Azure AD).
1. Abhängig von der jeweiligen Situation führt Azure AD eine der folgenden Aktionen aus:
   - Wertet die Rollenmitgliedschaften des Benutzers basierend auf dem [wids-Anspruch](../../active-directory-b2c/access-tokens.md) im Zugriffstoken des Benutzers aus.
   - Ruft alle für den Benutzer entweder direkt oder über eine Gruppenmitgliedschaft geltenden Rollenzuweisungen für die Ressource ab, für die die Aktion ausgeführt wird.
1. Azure AD ermittelt, ob die Aktion im API-Aufruf in den Rollen enthalten ist, die dem Benutzer für diese Ressource zugewiesen sind.
1. Wenn der Benutzer keine Rolle mit der Aktion als angefordertem Bereich besitzt, wird kein Zugriff gewährt. Andernfalls wird der Zugriff gewährt.

## <a name="role-assignment"></a>Rollenzuweisung

Eine Rollenzuweisung ist eine Azure AD-Ressource, die einem *Benutzer* eine *Rollendefinition* in einem bestimmten *Bereich* zuordnet, um Zugriff auf Azure AD-Ressourcen zu gewähren. Der Zugriff wird durch Erstellen einer Rollenzuweisung erteilt und durch Entfernen einer Rollenzuweisung widerrufen. Eine Rollenzuweisung besteht im Kern aus drei Elementen:

- Ein Azure AD-Benutzer
- Rollendefinition
- Ressourcenumfang

Zum [Erstellen von Rollenzuweisungen](custom-create.md) können Sie das Azure-Portal, Azure AD PowerShell oder die Graph-API verwenden. Sie können auch [die Rollenzuweisungen auflisten](view-assignments.md).

Das folgende Diagramm zeigt ein Beispiel für eine Rollenzuweisung. In diesem Beispiel wurde Chris Green die benutzerdefinierte Rolle „App-Registrierungsadministrator“ für den Bereich der Contoso Widget Builder-App-Registrierung zugewiesen. Diese Zuweisung erteilt Chris nur für diese spezifische App-Registrierung die Berechtigungen der Rolle „App-Registrierungsadministrator“.

![Die Rollenzuweisung definiert, wie Berechtigungen erzwungen werden. Sie besteht aus drei Teilen.](./media/custom-overview/rbac-overview.png)

### <a name="security-principal"></a>Sicherheitsprinzipal

Ein Sicherheitsprinzipal stellt den Benutzer dar, dem Zugriff auf Azure AD-Ressourcen zugewiesen werden soll. Ein Benutzer ist eine Person, die über ein Benutzerprofil in Azure Active Directory verfügt.

### <a name="role"></a>Role

Eine Rollendefinition oder Rolle ist eine Sammlung von Berechtigungen. Eine Rollendefinition listet die Vorgänge auf, die für Azure AD-Ressourcen ausgeführt werden können, z. B. Lesen, Schreiben, Aktualisieren und Löschen. Azure AD umfasst zwei Arten von Rollen:

- Von Microsoft erstellte integrierte Rollen, die nicht geändert werden können.
- Von Ihrer Organisation erstellte und verwaltete benutzerdefinierte Rollen

### <a name="scope"></a>`Scope`

Ein Bereich ist die Beschränkung zulässiger Aktionen auf eine bestimmte Azure AD-Ressource im Rahmen einer Rollenzuweisung. Beim Zuweisen einer Rolle können Sie einen Bereich angeben, der den Zugriff des Administrators auf eine bestimmte Ressource beschränkt. Wenn Sie z. B. einem Entwickler eine benutzerdefinierte Rolle – jedoch nur zum Verwalten einer bestimmten Anwendungsregistrierung – zuweisen möchten, können Sie die entsprechende Anwendungsregistrierung als Bereich in die Rollenzuweisung einschließen.

## <a name="license-requirements"></a>Lizenzanforderungen

Die Nutzung integrierter Rollen in Azure AD ist kostenlos, während für benutzerdefinierte Rollen eine Azure AD Premium P1-Lizenz erforderlich ist. Um die richtige Lizenz für Ihre Anforderungen zu ermitteln, lesen Sie [Vergleich der allgemein verfügbaren Features der Editionen Free und Premium](https://www.microsoft.com/security/business/identity-access-management/azure-ad-pricing).

## <a name="next-steps"></a>Nächste Schritte

- [Grundlegendes zu Rollen in Azure Active Directory](concept-understand-roles.md)
- Erstellen von benutzerdefinierten Rollenzuweisungen über das [Azure-Portal, Azure AD PowerShell und die Graph-API](custom-create.md)
- [Auflisten von Rollenzuweisungen](view-assignments.md)