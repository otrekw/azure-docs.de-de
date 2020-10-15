---
title: Verwenden von Azure AD für nicht im App-Katalog aufgeführten Anwendungen
description: Erfahren Sie, wie Sie Apps integrieren, die nicht im Azure AD-Katalog aufgeführt sind.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 07/27/2020
ms.author: kenwith
ms.reviewer: arvinh,luleon
ms.openlocfilehash: 262c3a7c8a668ef25d092b5d523743e0ece89cc3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88641142"
---
# <a name="using-azure-ad-for-applications-not-listed-in-the-app-gallery"></a>Verwenden von Azure AD für nicht im App-Katalog aufgeführten Anwendungen

Im Schnellstart [App hinzufügen](add-application-portal.md) erfahren Sie, wie Sie Ihrem Azure AD-Mandanten eine App hinzufügen.

Zusätzlich zu den Optionen im [Azure AD-Anwendungskatalog](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) haben Sie die Möglichkeit, eine **Nicht-Kataloganwendung** hinzuzufügen. 

## <a name="capabilities-for-apps-not-listed-in-the-azure-ad-gallery"></a>Funktionen für Apps, die nicht im Azure AD-Katalog aufgeführt sind

Sie können in Ihrer Organisation bereits vorhandene Anwendungen oder Anwendungen eines Anbieters hinzufügen, der noch nicht im Azure AD-Katalog gelistet ist. Abhängig von Ihrem [Lizenzvertrag](https://azure.microsoft.com/pricing/details/active-directory/) sind die folgenden Funktionen verfügbar:

- Self-Service-Integration von Anwendungen, die [Security Assertion Markup Language (SAML) 2.0](https://wikipedia.org/wiki/SAML_2.0)-Identitätsanbieter unterstützen (vom Dienstanbieter (SP) oder vom Identitätsanbieter (IdP) initiiert)
- Self-Service-Integration für Webanwendungen, die über eine HTML-basierte Anmeldeseite mit [kennwortbasierter einmaliger Anmeldung (Single Sign-On, SSO)](sso-options.md#password-based-sso)
- Self-Service-Verbindung für Anwendungen, die das [SCIM-Protokoll (System for Cross-Domain Identity Management) verwenden](../app-provisioning/use-scim-to-provision-users-and-groups.md)
- Möglichkeit zum Hinzufügen von Links zu Anwendungen im [Office 365-App-Startfeld](https://www.microsoft.com/microsoft-365/blog/2014/10/16/organize-office-365-new-app-launcher-2/) oder in [Meine Apps](sso-options.md#linked-sign-on)

Wenn Sie nach einer Anleitung für Entwickler zum Integrieren von benutzerdefinierten Apps in Azure AD suchen, finden Sie diese unter [Authentifizierungsszenarios für Azure AD](../develop/authentication-scenarios.md). Wenn Sie eine App entwickeln, die ein modernes Protokoll wie [OpenID Connect/OAuth](../develop/active-directory-v2-protocols.md) zum Authentifizieren von Benutzern verwendet, können Sie die App im Azure-Portal über die Oberfläche für [App-Registrierungen](../develop/quickstart-register-app.md) bei der Microsoft Identity Platform registrieren.

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstartserie zur App-Verwaltung](view-applications-portal.md)

