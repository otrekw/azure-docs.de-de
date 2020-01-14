---
title: Hinzufügen von Authentifizierung zu Ihren mobilen Apps mit Visual Studio App Center und Azure-Diensten
description: Erfahren Sie mehr über Dienste wie Visual Studio App Center, die beim Einrichten von Benutzerauthentifizierung unterstützen und es mobilen Anwendungen ermöglichen, sich über Social Media-Konten, Azure Active Directory und benutzerdefinierte Authentifizierung zu authentifizieren.
author: elamalani
ms.assetid: 34a8a070-2222-4faf-9090-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: bc7d8d4aaec6ebe27a0f8d2ecc11ca408266f7ef
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453242"
---
# <a name="add-authentication-and-manage-user-identities-in-your-mobile-apps"></a>Hinzufügen von Authentifizierung und Verwalten von Benutzeridentitäten in Ihren mobilen Apps

Mit einer Sicht auf die Benutzer und deren Verhalten in Ihrer Anwendung werden Sie als Entwickler in die Lage versetzt, die Benutzer besser einzubinden, indem Sie maßgeschneiderte Umgebungen für sie erstellen. Unabhängig davon, ob Sie ein Anwendungsentwickler sind, der eine Zusammenarbeitsanwendung für Benutzer in Ihrer Organisation erstellt, oder ob Sie die nächste Plattform für soziale Netzwerke erstellen, Sie benötigen eine Möglichkeit, Benutzer zu authentifizieren und Benutzeridentitäten zu verwalten. Ein Identitätsverwaltungsdienst ist eines der wichtigsten Features eines Dienstes für ein mobiles Back-End.

Mit den folgenden Diensten können Sie Benutzerauthentifizierung in ihren mobilen Apps aktivieren.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Auth](/appcenter/auth/) ist ein cloudbasierter Identitätsverwaltungsdienst, den Entwicklern zum Authentifizieren von Benutzern und zum Verwalten von Benutzeridentitäten verwenden können. App Center Auth ist auch in andere Teile von Visual Studio App Center integriert. Entwickler können die Benutzeridentität dazu nutzen, ein [Anzeigen von Benutzerdaten](/appcenter/data/index) in anderen Diensten zu ermöglichen und sogar [Pushbenachrichtigungen an Benutzer anstatt an einzelne Geräte zu senden](/appcenter/push/push-to-user#app-center-auth-set-identity). 

**Wichtige Features**
- Unterstützt von Azure Active Directory B2C (Azure AD B2C). 
    - Für große Unternehmen geeignet.
    - Hochverfügbar.
    - Sicherer und globaler Dienst.
- Verwenden Ihrer eigenen Identität (BYOI) und die Möglichkeit zur Verwendung anderer beliebter Identitäts- und Zugriffsverwaltungsanbieter wie Auth0 und Firebase.
- Azure Active Directory-Unterstützung.
    - Verbinden von vorhandenen Azure AD-Mandanten. 
    - Ermöglichen von Authentifizierung bei einer Unternehmensdomäne.
    - Verwalten von Zugriff auf sensible Daten.
- Einfache Benutzerumgebung und magische SDK-Erfahrung, indem die Microsoft-Authentifizierungsbibliothek in das Visual App Center-SDK eingebunden wird.
- Plattformunterstützung für iOS, Android, Xamarin und React Native.

**Referenzen**
- [Registrieren bei Visual Studio App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs) 
- [Erste Schritte mit App Center Auth](/appcenter/auth/)

## <a name="azure-active-directory-b2c"></a>Azure Active Directory B2C
[Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/) ist ein Dienst für die Business-to-Consumer-Identitätsverwaltung (B2C), den Entwickler für die Authentifizierung ihrer Kunden verwenden können. Mit diesem White-Label-Dienst können Entwickler anpassen und steuern, wie Benutzer sicher mit ihren Web-, Desktop-, mobilen oder Einzelseitenanwendungen interagieren. Mit Azure AD B2C können Benutzer Registrierungs- und Anmeldevorgänge ausführen, Kennwörter zurücksetzen und Profile bearbeiten. Azure AD B2C implementiert eine Form von OpenID Connect- und OAuth 2.0-Protokollen. 

**Wichtige Features**
- Sicheres Authentifizieren von Kunden über deren bevorzugten Identitätsanbieter.
- Verwalten von Kundenidentität- und -zugriff.
- Erhalten von Anmeldeunterstützung für soziale Medien wie Facebook, GitHub, Google, LinkedIn, Twitter, WeChat und Weibo.
- Verwenden von Standardprotokollen wie OpenID Connect oder SAML für das Herstellen von Verbindungen mit Ihren Benutzerkonten, um die Identitätsverwaltung auf zahlreichen Plattformen zu ermöglichen.
- Bereitstellen von markengebundenen Registrierungs- und Anmeldeerfahrungen.
- Einfache Integration in CRM-Datenbanken, Marktanalysetools und Systeme zur Kontoüberprüfung.
- Erfassen von Anmelde-, Einstellungs- und Konvertierungsdaten für Kunden.

**Referenzen**
- [Azure portal](https://portal.azure.com/)
- [Azure AD B2C-Dokumentation](/azure/active-directory-b2c/)
- [Schnellstarts](/azure/active-directory-b2c/active-directory-b2c-quickstarts-web-app)
- [Beispiele](/azure/active-directory-b2c/code-samples)

## <a name="azure-active-directory"></a>Azure Active Directory
[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) ist der cloudbasierte Identitäts- und Zugriffsverwaltungsdienst von Microsoft, über den Ihre Mitarbeiter sich anmelden und zugreifen können auf:
- Externe Ressourcen wie Microsoft Office 365, das Azure-Portal und Tausende andere SaaS-Anwendungen (Software-as-a-Service).
- Interne Ressourcen (beispielsweise Apps im Netzwerk/Intranet Ihres Unternehmens oder selbst entwickelte Cloud-Apps Ihrer Organisation)

**Wichtige Features**
- Nahtloser, äußerst sicherer Zugriff, indem Benutzer mit den Anwendungen verbunden werden, die sie benötigen.
- Umfassender Identitätsschutz und erhöhte Sicherheit für Identitäten und Zugriff auf Basis von Benutzer-, Standort-, Geräte-, Daten- und Anwendungskontext.
- Tausende von bereits integrierten Apps für sowohl kommerzielle als auch benutzerdefinierte Anwendungen, etwa Office 365, Salesforce.com und Box.
- Die Möglichkeit zur Verwaltung von Zugriff im großen Stil.

**Referenzen**
- [Azure portal](https://portal.azure.com/)
- [Was ist Azure AD?](/azure/active-directory/fundamentals/active-directory-whatis)
- [Erste Schritte mit Azure Active Directory](/azure/active-directory/fundamentals/active-directory-whatis)
- [Schnellstarts](/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)