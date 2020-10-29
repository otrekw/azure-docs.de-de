---
title: Übersicht über das Protokoll für die Azure Active Directory-Authentifizierung und -Synchronisierung
description: Architekturleitfaden zum Erreichen dieses Authentifizierungsmusters
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d881dc3fe3e3caa1058cf97834735910b0de1d9
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92113999"
---
# <a name="azure-active-directory-integrations-with-legacy-authentication-and-synchronization-protocols"></a>Azure Active Directory-Integrationen mit älteren Authentifizierungs- und Synchronisierungsprotokollen

Microsoft Azure Active Directory (Azure AD) ermöglicht die Integration mit zahlreichen Authentifizierungs- und Synchronisierungsprotokollen. Die Authentifizierungsintegrationen ermöglichen es Ihnen, Azure AD und zugehörige Sicherheits- und Verwaltungsfunktionen mit wenigen oder gar keinen Änderungen an Ihren Anwendungen zu verwenden, die ältere Authentifizierungsmethoden verwenden. Die Synchronisierungsintegrationen ermöglichen das Synchronisieren von Benutzer- und Gruppendaten mit Azure AD und die anschließende Verwendung von Azure AD-Verwaltungsfunktionen. Einige Synchronisierungsmuster ermöglichen darüber hinaus die automatisierte Bereitstellung.

## <a name="authentication-patterns"></a>Authentifizierungsmuster

In der folgenden Tabelle sind die Authentifizierungsmuster und ihre Funktionen dargestellt. Wählen Sie den Namen eines Authentifizierungsmusters aus, um Folgendes anzuzeigen:

* Eine detaillierte Beschreibung

* Einsatzgebiete

* Architekturdiagramm

* Eine Erläuterung der Systemkomponenten

* Links zum Implementieren der Integration

 

| Authentifizierungsmuster| Authentifizierung| Authorization| Multi-Factor Authentication| Bedingter Zugriff |
| - |- | - | - | - |
| [Headerbasierte Authentifizierung](auth-header-based.md)|![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png) |
| [LDAP-Authentifizierung](auth-ldap.md)| ![Häkchen](./media/authentication-patterns/check.png)| | |  |
| [OAuth 2.0-Authentifizierung](auth-oauth2.md)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png) |
| [OIDC-Authentifizierung](auth-oidc.md)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png) |
| [Kennwortbasierte SSO-Authentifizierung](auth-password-based-sso.md )| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png) |
| [RADIUS-Authentifizierung]( auth-radius.md)| ![Häkchen](./media/authentication-patterns/check.png)| | ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png) |
| [Remotedesktop-Gatewaydienste](auth-remote-desktop-gateway.md)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png) |
| [SAML-Authentifizierung](auth-saml.md)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png) |
| [Windows-Authentifizierung: Eingeschränkte Kerberos-Delegierung](auth-kcd.md)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png) |


 
## <a name="synchronization-patterns"></a>Synchronisierungsmuster

In der folgenden Tabelle sind die Synchronisierungsmuster und ihre Funktionen dargestellt. Wählen Sie den Namen eines Musters aus, um Folgendes anzuzeigen:

* Eine detaillierte Beschreibung

* Einsatzgebiete

* Architekturdiagramm

* Eine Erläuterung der Systemkomponenten

* Links zum Implementieren der Integration



| Synchronisierungsmuster| Verzeichnissynchronisierung| Benutzerbereitstellung |
| - | - | - |
| [Verzeichnissynchronisierung](sync-directory.md)| ![Häkchen](./media/authentication-patterns/check.png)|  |
| [LDAP-Synchronisierung](sync-ldap.md)| ![Häkchen](./media/authentication-patterns/check.png)|  |
| [SCIM-Synchronisierung](sync-scim.md)| ![Häkchen](./media/authentication-patterns/check.png)| ![Häkchen](./media/authentication-patterns/check.png) |

