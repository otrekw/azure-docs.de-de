---
title: Autorisieren des Zugriffs auf Azure App Configuration mittels Azure Active Directory
description: Aktivieren von RBAC zur Autorisierung des Zugriffs auf Ihre Azure App Configuration-Instanz
author: lisaguthrie
ms.author: lcozzens
ms.date: 02/13/2020
ms.topic: conceptual
ms.service: azure-app-configuration
ms.openlocfilehash: 18fa1b60b15b7eef96efa8dcc4fbf9cd7c4dc7f7
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77473198"
---
# <a name="authorize-access-to-azure-app-configuration-using-azure-active-directory"></a>Autorisieren des Zugriffs auf Azure App Configuration mittels Azure Active Directory
Azure App Configuration unterstützt die Verwendung von Azure Active Directory (Azure AD) zum Autorisieren von Anforderungen an App Configuration-Instanzen.  Azure AD gestattet Ihnen die Verwendung der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC), um einem Sicherheitsprinzipal Berechtigungen zu erteilen.  Ein Sicherheitsprinzipal kann ein Benutzer oder ein [Anwendungsdienstprinzipal](../active-directory/develop/app-objects-and-service-principals.md) sein.  Weitere Informationen zu Rollen und Rollenzuweisungen finden Sie unter [Grundlegendes zu verschiedenen Rollen](../role-based-access-control/overview.md).

## <a name="overview"></a>Übersicht
Vom Sicherheitsprinzipal (einem Benutzer oder einer Anwendung) gesendete Anforderungen für den Zugriff auf eine App Configuration-Ressource müssen autorisiert werden.  Mit Azure AD ist der Zugriff auf eine Ressource ein zweistufiger Prozess.
1. Die Identität des Sicherheitsprinzipals wird authentifiziert, und ein OAuth 2.0-Token wird zurückgegeben.  Der Ressourcenname zum Anfordern eines Tokens lautet `https://login.microsoftonline.com/{tenantID}`, wobei `{tenantID}` der Azure Active Directory-Mandanten-ID entspricht, zu der der Dienstprinzipal gehört.
2. Das Token wird als Teil einer Anforderung an den App Configuration-Dienst übergeben, um den Zugriff auf die angegebene Ressource zu autorisieren.

Für den Authentifizierungsschritt ist es erforderlich, dass eine Anwendungsanforderung zur Laufzeit ein OAuth 2.0-Zugriffstoken enthält.  Wenn eine Anwendung in einer Azure-Entität, z. B. einer Azure Functions-App, einer Azure-Web-App oder einem virtuellen Azure-Computer, ausgeführt wird, kann der Zugriff auf die Ressourcen über eine verwaltete Identität erfolgen.  Informationen zum Authentifizieren von Anforderungen, die von einer verwalteten Identität an Azure App Configuration übermittelt werden, finden Sie unter [Authentifizieren des Zugriffs auf Azure App Configuration-Ressourcen mit Azure Active Directory und verwalteten Identitäten für Azure-Ressourcen](howto-integrate-azure-managed-service-identity.md).

Für den Autorisierungsschritt ist es erforderlich, dem Sicherheitsprinzipal mindestens eine RBAC-Rolle zuzuweisen. Azure App Configuration stellt RBAC-Rollen bereit, die Berechtigungssätze für App Configuration-Ressourcen umfassen. Die einem Sicherheitsprinzipal zugewiesenen Rollen bestimmen die dem Prinzipal bereitgestellten Berechtigungen. Weitere Informationen zu RBAC-Rollen finden Sie unter [Integrierte RBAC-Rollen für Azure App Configuration](#built-in-rbac-roles-for-azure-app-configuration). 

## <a name="assign-rbac-roles-for-access-rights"></a>Zuweisen von RBAC-Rollen für Zugriffsrechte
Azure Active Directory (Azure AD) autorisiert Rechte für den Zugriff auf abgesicherte Ressourcen über die [rollenbasierte Zugriffssteuerung (RBAC)](../role-based-access-control/overview.md).

Wenn einem Azure AD-Sicherheitsprinzipal eine RBAC-Rolle zugewiesen wird, gewährt Azure diesem Sicherheitsprinzipal Zugriff auf diese Ressourcen. Der Umfang des Zugriffs ist auf die App Configuration-Ressource beschränkt. Ein Azure AD-Sicherheitsprinzipal kann ein Benutzer, ein Anwendungsdienstprinzipal oder eine [verwaltete Identität für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) sein.

## <a name="built-in-rbac-roles-for-azure-app-configuration"></a>Integrierte RBAC-Rollen für Azure App Configuration
Azure stellt die folgenden integrierten Rollen für die rollenbasierte Zugriffssteuerung zum Autorisieren des Zugriffs auf App Configuration-Daten mittels Azure AD und OAuth bereit:

- Azure App Configuration-Datenbesitzer: Verwenden Sie diese Rolle, um App Configuration-Ressourcen Lese-/Schreibzugriff zu gewähren.
- Azure App Configuration-Datenleser: Verwenden Sie diese Rolle, um App Configuration-Ressourcen Lesezugriff zu gewähren.
- Mitwirkender: Verwenden Sie diese Rolle, um dem Dienst Administratorzugriff zu gewähren, ohne Zugriff auf die in der App Configuration-Instanz gespeicherten Daten zu gewähren.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung [verwalteter Identitäten](howto-integrate-azure-managed-service-identity.md) zum Verwalten Ihres App Configuration-Diensts.