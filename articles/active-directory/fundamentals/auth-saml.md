---
title: SAML-Authentifizierung mit Azure Active Directory
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
ms.openlocfilehash: 05b13e04db8e83a8a10c2d7fe0aea202dfa3b69c
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114000"
---
# <a name="saml-authentication-with-azure-active-directory"></a>SAML-Authentifizierung mit Azure Active Directory

Security Assertion Markup Language (SAML) ist ein offener Standard für den Austausch von Authentifizierungs- und Autorisierungsdaten zwischen einem Identitätsanbieter und einem Dienstanbieter. SAML ist eine XML-basierte Markupsprache für Sicherheitsassertionen – Anweisungen, mit denen Dienstanbieter Entscheidungen zur Zugriffssteuerung treffen. 

Die SAML-Spezifikation definiert drei Rollen:

* Prinzipal, in der Regel ein Benutzer
* Identitätsanbieter (Identity Provider, IdP)
* Dienstanbieter (Service Provider, SP)


## <a name="use-when"></a>Wenn folgende Bedingungen vorliegen

Es müssen SSO-Funktionen (Single Sign-On, einmaliges Anmelden) für eine SAML-Anwendung des Unternehmens bereitgestellt werden.

Einer der wichtigsten Anwendungsfälle für SAML ist das einmalige Anmelden, insbesondere da SAML dieses Feature über Sicherheitsdomänen hinweg nutzbar macht. Allerdings gibt es auch noch andere Anwendungsfälle (so genannte Profile). 

![Architekturdiagramm](./media/authentication-patterns/saml-auth.png)

## <a name="components-of-system"></a>Komponenten des Systems

* **Benutzer:** Der Benutzer fordert einen Dienst von der Anwendung an.

* **Webbrowser** : Die Komponente, mit der der Benutzer interagiert.

* **Web-App** : Unternehmensanwendung, die SAML unterstützt und Azure AD als Identitätsanbieter verwendet.

* **Token** : Eine SAML-Assertion (auch als SAML-Token bezeichnet), die eine Reihe von Aussagen des Identitätsanbieters über die Eigenschaften und Berechtigungen des Prinzipals (Benutzers) enthält. Die Assertion bzw. das Token enthält Authentifizierungsinformationen, Attribute und Anweisungen zu Autorisierungsentscheidungen.

* **Azure AD** : Ein Identitätsanbieter der Unternehmenscloud, der einmaliges Anmelden und mehrstufige Authentifizierung für SAML-Apps bereitstellt. Dieser Anbieter synchronisiert, pflegt und verwaltet Identitätsinformationen für Benutzer und stellt gleichzeitig Authentifizierungsdienste für Anwendungen bereit, die diese benötigen. 

## <a name="implement-saml-authentication-with-azure-ad"></a>Implementieren der SAML-Authentifizierung mit Azure AD

* [Tutorials zur Integration von SaaS-Anwendungen mithilfe von Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list) 

* [Konfigurieren des SAML-basierten einmaligen Anmeldens für nicht im Katalog aufgeführte Anwendungen](https://docs.microsoft.com/azure/active-directory/manage-apps/add-non-gallery-app) 

* [Verwendung des SAML-Protokolls durch Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference)
