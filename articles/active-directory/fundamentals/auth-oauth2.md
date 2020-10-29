---
title: OAUTH 2.0-Authentifizierung mit Azure Active Directory
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
ms.openlocfilehash: ea22c4e5b363eaa3ecc2a736dfef714666310062
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92113930"
---
# <a name="oauth-20-authentication-with-azure-active-directory"></a>OAuth 2.0-Authentifizierung mit Azure Active Directory

OAuth 2.0 ist ein Branchenprotokoll für die Autorisierung. Es ermöglicht einem Benutzer, eingeschränkten Zugriff auf die geschützten Ressourcen zu gewähren. OAuth ist speziell für die Verwendung mit HTTP (Hypertext Transfer-Protokoll) konzipiert und trennt die Rolle des Clients vom Ressourcenbesitzer. Der Client fordert den Zugriff auf die Ressourcen an, die vom Ressourcenbesitzer gesteuert und vom Ressourcenserver gehostet werden. Der Ressourcenserver gibt Zugriffstoken mit der Genehmigung des Ressourcenbesitzers aus. Der Client verwendet die Zugriffstoken für den Zugriff auf die geschützten Ressourcen, die vom Ressourcenserver gehostet werden. 

OAuth 2.0 hängt direkt mit OpenID Connect (OIDC) zusammen. OIDC ist eine Autorisierungs- und Autorisierungsebene, die auf OAuth 2.0 basiert, und ist daher nicht mit OAuth 1.0 abwärtskompatibel. Azure Active Directory (Azure AD) unterstützt alle OAuth 2.0-Flows. 

## <a name="use-when"></a>Zu verwenden in folgenden Fällen:

Für Rich Client-Szenarien und Szenarien mit modernen Apps sowie den RESTful-Web-API-Zugriff.

![Diagramm der Architektur](./media/authentication-patterns/oauth.png)

## <a name="components-of-system"></a>Komponenten des Systems

* **Benutzer:** Der Benutzer fordert einen Dienst von der Webanwendung (Web-App) an. Der Benutzer ist in der Regel der Ressourcenbesitzer, der die Daten besitzt und die Möglichkeit hat, Clients den Zugriff auf die Daten oder die Ressource zu gewähren. 

* **Webbrowser:** Der Webbrowser, mit dem der Benutzer interagiert, ist der OAuth-Client. 

* **Web-App** : Die Web-App oder der Ressourcenserver ist der Ort, an dem die Ressourcen oder die Daten abgelegt sind. Er vertraut auf die sichere Authentifizierung und Autorisierung des OAuth-Clients durch den Autorisierungsserver. 

* **Azure AD** : Azure AD ist der Autorisierungsserver, der auch als Identitätsanbieter (Identity Provider, IdP) bezeichnet wird. Er verarbeitet auf sichere Weise alles im Zusammenhang mit den Informationen des Benutzers, dessen Zugriff und den Vertrauensstellungen. Er ist für die Ausgabe der Token zuständig, mit denen der Zugriff auf Ressourcen gewährt und widerrufen wird.

## <a name="implement-oauth-20-with-azure-ad"></a>Implementieren von OAuth 2.0 mit Azure AD

* [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list) 

* [OAuth 2.0- und OpenID Connect-Protokolle auf der Microsoft Identity Platform](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols) 

* [Anwendungstypen und OAuth2](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types) 

 
