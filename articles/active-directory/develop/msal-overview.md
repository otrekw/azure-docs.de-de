---
title: Informationen zu MSAL | Azure
titleSuffix: Microsoft identity platform
description: Die Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) ermöglicht Anwendungsentwicklern das Abrufen von Token, um geschützte Web-APIs aufzurufen. Bei diesen Web-APIs kann es sich um Microsoft Graph, andere Microsoft APIs, Web-APIs von Drittanbietern oder Ihre eigene Web-API handeln. Die MSAL unterstützt mehrere Anwendungsarchitekturen und -plattformen.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 8fbcacf4e3c26f117be4ab5b64b35f670435e6b6
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92092748"
---
# <a name="overview-of-microsoft-authentication-library-msal"></a>Übersicht über die Microsoft-Authentifizierungsbibliothek (MSAL)
Mithilfe der Microsoft-Authentifizierungsbibliothek (MSAL) können Entwickler [Token](developer-glossary.md#security-token) vom Microsoft Identity Platform-Endpunkt abrufen, um Benutzer zu authentifizieren und auf geschützte Web-APIs zuzugreifen. Sie kann verwendet werden, um sicheren Zugriff auf Microsoft Graph, andere Microsoft-APIs, Web-APIs von Drittanbietern oder Ihre eigene Web-API zu gewährleisten. MSAL unterstützt verschiedene Anwendungsarchitekturen und -plattformen einschließlich .NET, JavaScript, Java, Python, Android und iOS.

MSAL bietet zahlreiche Möglichkeiten für den Tokenabruf und stellt eine konsistente API für unterschiedliche Plattformen bereit. MSAL bietet folgende Vorteile:

* Die OAuth-Bibliotheken oder Code müssen nicht direkt in Übereinstimmung mit dem Protokoll in Ihrer Anwendung verwendet werden.
* Token werden im Namen eines Benutzers oder einer Anwendung abgerufen (sofern auf die Plattform zutreffend).
* MSAL stellt einen Tokencache bereit und aktualisiert Token, bevor sie ablaufen. Sie müssen sich nicht um den Ablauf von Token kümmern.
* Sie können angeben, welche Benutzer sich bei Ihrer Anwendung anmelden sollen (Ihre Organisation, mehrere Organisationen, Geschäfts-, Schul- und Unikonten sowie persönliche Microsoft-Konten, Identitäten in sozialen Netzwerken mit Azure AD B2C, Benutzer in Sovereign Clouds und nationalen Clouds).
* Sie können Ihre Anwendung mithilfe von Konfigurationsdateien einrichten.
* Sie können Fehler in Ihrer App mithilfe von handlungsrelevanten Ausnahmen sowie Protokoll- und Telemetriedaten beheben.

## <a name="application-types-and-scenarios"></a>Anwendungstypen und -szenarien
Mit MSAL kann ein Token von verschiedenen Anwendungstypen abgerufen werden: Webanwendungen, Web-APIs, Single-Page-Apps (JavaScript), mobilen und nativen Anwendungen sowie Daemonanwendungen und serverseitigen Anwendungen.

MSAL kann in vielen Anwendungsszenarien verwendet werden, darunter:

* [Single-Page-Anwendungen (JavaScript)](scenario-spa-overview.md)
* [Web-Apps für Benutzeranmeldungen](scenario-web-app-sign-user-overview.md)
* [Webanwendungen, die einen Benutzer anmelden und eine Web-API im Namen des Benutzers aufrufen](scenario-web-app-call-api-overview.md)
* [Zum Schutz einer Web-API, sodass nur authentifizierte Benutzer darauf zugreifen können](scenario-protected-web-api-overview.md)
* [Eine Web-API, die im Namen des angemeldeten Benutzers eine andere Downstream-Web-API aufruft](scenario-web-api-call-api-overview.md)
* [Eine Desktopanwendung, die im Namen des angemeldeten Benutzers eine Web-API aufruft](scenario-desktop-overview.md)
* [Mobile Anwendung, die eine Web-API im Namen des Benutzers aufruft, der sich interaktiv angemeldet hat](scenario-mobile-overview.md).
* [Eine Desktop-/Servicedaemonanwendung, die eine Web-API im eigenen Namen aufruft](scenario-daemon-overview.md)

## <a name="languages-and-frameworks"></a>Sprachen und Frameworks

| Bibliothek | Unterstützte Plattformen und Frameworks|
| --- | --- |
| [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)| .NET Framework, .NET Core, Xamarin Android, Xamarin iOS, Universelle Windows-Plattform|
| [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)| JavaScript-/TypeScript-Frameworks wie AngularJS, Ember.js oder Durandal.js|
| [MSAL für Android](https://github.com/AzureAD/microsoft-authentication-library-for-android)|Android|
| [MSAL für iOS und macOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|iOS und macOS|
| [MSAL Java](https://github.com/AzureAD/microsoft-authentication-library-for-java)|Windows, macOS, Linux|
| [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python)|Windows, macOS, Linux|

## <a name="differences-between-adal-and-msal"></a>Unterschiede zwischen ADAL und MSAL

Die Active Directory-Authentifizierungsbibliothek (ADAL) arbeitet mit dem Azure AD für Entwickler (v1.0)-Endpunkt zusammen, während MSAL und der Microsoft Identity Platform (v2.0)-Endpunkt integriert sind. Der v1.0-Endpunkt unterstützt Geschäftskonten, aber keine persönlichen Konten. Der v2.0-Endpunkt wird für die Zusammenführung von persönlichen Konten und Geschäftskonten von Microsoft in einem gemeinsamen Authentifizierungssystem genutzt. Darüber hinaus unterstützt MSAL auch Authentifizierungen für Azure AD B2C.

Spezifischere Informationen finden Sie in den Themen zum [Migrieren zu MSAL.NET von ADAL.NET](msal-net-migration.md) und [Migrieren zu MSAL.js von ADAL.js](msal-compare-msal-js-and-adal-js.md).
