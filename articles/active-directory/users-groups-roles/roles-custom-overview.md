---
title: Benutzerdefinierte Administratorrollen in Azure Active Directory | Microsoft-Dokumentation
description: 'Vorschau: Benutzerdefinierte Azure AD-Rollen zur Delegierung der Identitätsverwaltung. Verwalten von Azure-Rollen im Azure-Portal, mit PowerShell oder über die Graph-API.'
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: overview
ms.date: 09/11/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e962bea893e8a658b876aabf417e64b11a31dfc1
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90033505"
---
# <a name="custom-administrator-roles-in-azure-active-directory-preview"></a>Benutzerdefinierte Administratorrollen in Azure Active Directory (Vorschau)

In diesem Artikel werden die Grundlagen der benutzerdefinierten Azure AD-Rollen in Azure Active Directory (Azure AD) mit rollenbasierter Zugriffssteuerung und Ressourcenbereichen beschrieben. Durch benutzerdefinierte Azure AD-Rollen werden die zugrunde liegenden Berechtigungen der [integrierten Rollen](directory-assign-admin-roles.md) angezeigt, sodass Sie eigene benutzerdefinierte Rollen erstellen und organisieren können. Dadurch können Sie den Zugriff bei Bedarf differenzierter gewähren als mit integrierten Rollen. Dieses erste Release der benutzerdefinierten Azure AD-Rollen umfasst die Funktion, eine Rolle zum Zuweisen von Berechtigungen zum Verwalten von App-Registrierungen zu erstellen. Im Laufe der Zeit werden weitere Berechtigungen für Organisationsressourcen wie Unternehmensanwendungen, Benutzer und Geräte hinzugefügt.  

Darüber hinaus unterstützen benutzerdefinierte Azure AD-Rollen zusätzlich zu den eher herkömmlichen organisationsweiten Zuweisungen auch Zuweisungen auf Ressourcenbasis. Dieser Ansatz bietet Ihnen die Möglichkeit, Zugriff zum Verwalten einiger Ressourcen (z. B. einer App-Registrierung) zu erteilen, ohne Zugriff auf alle Ressourcen (alle App-Registrierungen) zu gewähren.

Die rollenbasierte Zugriffssteuerung ist eine öffentliche Previewfunktion von Azure AD und mit jedem kostenpflichtigen Azure AD-Lizenzplan verfügbar. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="understand-azure-ad-role-based-access-control"></a>Grundlegendes zur rollenbasierten Zugriffssteuerung von Azure AD

Das Erteilen von Berechtigungen mithilfe benutzerdefinierter Azure AD-Rollen ist ein zweistufiger Prozess, bei dem eine benutzerdefinierte Rollendefinition erstellt und dann mithilfe einer Rollenzuweisung zugewiesen wird. Eine benutzerdefinierte Rollendefinition ist eine Sammlung von Berechtigungen, die Sie aus einer vordefinierten Liste hinzufügen. Bei diesen Berechtigungen handelt es sich um die gleichen Berechtigungen, die in den integrierten Rollen verwendet werden.  

Nachdem Sie Ihre Rollendefinition erstellt haben, können Sie diese durch Erstellen einer Rollenzuweisung einem Benutzer zuweisen. Eine Rollenzuweisung erteilt einem Benutzer die Berechtigungen in einer Rollendefinition für einen bestimmten Bereich. Durch diesen zweistufigen Prozess können Sie eine einzelne Rollendefinition erstellen und dann für verschiedene Bereiche mehrmals zuweisen. Ein Bereich definiert die Gruppe von Azure AD-Ressourcen, auf die das Rollenmitglied Zugriff hat. Der gängigste Bereich ist der organisationsweite Bereich. Eine benutzerdefinierte Rolle kann für den organisationsweiten Bereich zugewiesen werden. Dies bedeutet, dass das Rollenmitglied über die Rollenberechtigungen für alle Ressourcen in der Organisation verfügt. Eine benutzerdefinierte Rolle kann auch für einen Objektbereich zugewiesen werden. Ein Beispiel für einen Objektbereich ist eine einzelne Anwendung. Die gleiche Rolle kann einem Benutzer für alle Anwendungen in der Organisation und dann einem anderen Benutzer nur für die App der Contoso-Spesenabrechnungen zugewiesen werden.  

Die integrierten und benutzerdefinierten Rollen in Azure AD funktionieren auf ähnliche Weise wie die [rollenbasierte Zugriffssteuerung in Azure (Azure RBAC)](../../role-based-access-control/overview.md). Der [Unterschied zwischen diesen beiden rollenbasierten Zugriffssteuerungssystemen](../../role-based-access-control/rbac-and-directory-admin-roles.md) besteht darin, dass die Azure-RBAC den Zugriff auf Azure-Ressourcen wie virtuelle Computer oder Speicher mithilfe der Azure-Ressourcenverwaltung steuert und benutzerdefinierte Azure AD-Rollen den Zugriff auf Azure AD-Ressourcen mithilfe der Graph-API steuern. Beide Systeme nutzen das Konzept von Rollendefinitionen und Rollenzuweisungen.

### <a name="how-azure-ad-determines-if-a-user-has-access-to-a-resource"></a>So ermittelt Azure AD, ob ein Benutzer Zugriff auf eine Ressource hat

Im Folgenden finden Sie die allgemeinen Schritte, anhand derer Azure AD ermittelt, ob Sie Zugriff auf eine Verwaltungsressource haben. Verwenden Sie diese Informationen, um Zugriffsprobleme zu beheben.

1. Ein Benutzer (oder Dienstprinzipal) ruft ein Token für den Microsoft Graph- oder Azure AD Graph-Endpunkt ab.

1. Der Benutzer sendet mithilfe des ausgestellten Tokens über Microsoft Graph oder Azure AD Graph einen API-Aufruf an Azure Active Directory (Azure AD).

1. Abhängig von der jeweiligen Situation führt Azure AD eine der folgenden Aktionen aus:

    - Wertet die Rollenmitgliedschaften des Benutzers basierend auf dem [wids-Anspruch](../develop/access-tokens.md) im Zugriffstoken des Benutzers aus.
    - Ruft alle für den Benutzer entweder direkt oder über eine Gruppenmitgliedschaft geltenden Rollenzuweisungen für die Ressource ab, für die die Aktion ausgeführt wird.

1. Azure AD ermittelt, ob die Aktion im API-Aufruf in den Rollen enthalten ist, die dem Benutzer für diese Ressource zugewiesen sind.
1. Wenn der Benutzer keine Rolle mit der Aktion als angefordertem Bereich besitzt, wird kein Zugriff gewährt. Andernfalls wird der Zugriff gewährt.

### <a name="role-assignments"></a>Rollenzuweisungen

Eine Rollenzuweisung ist das Objekt, das einem Benutzer eine Rollendefinition in einem bestimmten Bereich anfügt, um Zugriff auf Azure AD-Ressourcen zu gewähren. Der Zugriff wird durch Erstellen einer Rollenzuweisung erteilt und durch Entfernen einer Rollenzuweisung widerrufen. Eine Rollenzuweisung besteht im Kern aus drei Elementen:

- Benutzer (eine Person, die über ein Benutzerprofil in Azure Active Directory verfügt)
- Rollendefinition
- Ressourcenumfang

Zum [Erstellen von Rollenzuweisungen](roles-create-custom.md) können Sie das Azure-Portal, Azure AD PowerShell oder die Graph-API verwenden. Sie können auch [die Zuweisungen für eine benutzerdefinierte Rolle anzeigen](roles-view-assignments.md#view-the-assignments-of-a-role).

Das folgende Diagramm zeigt ein Beispiel für eine Rollenzuweisung. In diesem Beispiel wurde Chris Green die benutzerdefinierte Rolle „App-Registrierungsadministrator“ für den Bereich der Contoso Widget Builder-App-Registrierung zugewiesen. Diese Zuweisung erteilt Chris nur für diese spezifische App-Registrierung die Berechtigungen der Rolle „App-Registrierungsadministrator“.

![Die Rollenzuweisung definiert, wie Berechtigungen erzwungen werden. Sie besteht aus drei Teilen.](./media/roles-custom-overview/rbac-overview.png)

### <a name="security-principal"></a>Sicherheitsprinzipal

Ein Sicherheitsprinzipal stellt den Benutzer dar, dem Zugriff auf Azure AD-Ressourcen zugewiesen werden soll. Ein *Benutzer* ist eine Person, die über ein Benutzerprofil in Azure Active Directory verfügt.

### <a name="role"></a>Role

Eine Rollendefinition oder Rolle ist eine Sammlung von Berechtigungen. Eine Rollendefinition listet die Vorgänge auf, die für Azure AD-Ressourcen ausgeführt werden können, z. B. Lesen, Schreiben, Aktualisieren und Löschen. Azure AD umfasst zwei Arten von Rollen:

- Von Microsoft erstellte integrierte Rollen, die nicht geändert werden können.
- Von Ihrer Organisation erstellte und verwaltete benutzerdefinierte Rollen

### <a name="scope"></a>`Scope`

Ein Bereich ist die Beschränkung zulässiger Aktionen auf eine bestimmte Azure AD-Ressource im Rahmen einer Rollenzuweisung. Beim Zuweisen einer Rolle können Sie einen Bereich angeben, der den Zugriff des Administrators auf eine bestimmte Ressource beschränkt. Wenn Sie z. B. einem Entwickler eine benutzerdefinierte Rolle – jedoch nur zum Verwalten einer bestimmten Anwendungsregistrierung – zuweisen möchten, können Sie die entsprechende Anwendungsregistrierung als Bereich in die Rollenzuweisung einschließen.

  > [!Note]
  > Benutzerdefinierte Rollen können im Verzeichnisbereich und im Ressourcenbereich zugewiesen werden. Noch können sie nicht im Bereich von Verwaltungseinheiten zugewiesen werden.
  > Integrierte Rollen können im Verzeichnisbereich und in einigen Fällen im Bereich von Verwaltungseinheiten zugewiesen werden. Sie können noch nicht im Azure AD-Ressourcenbereich zugewiesen werden.

## <a name="required-license-plan"></a>Erforderlicher Lizenzplan

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Nächste Schritte

- Erstellen von benutzerdefinierten Rollenzuweisungen über das [Azure-Portal, Azure AD PowerShell und die Graph-API](roles-create-custom.md)
- [Anzeigen der Zuweisungen für eine benutzerdefinierte Rolle](roles-view-assignments.md#view-assignments-of-single-application-scope)