---
title: Migrieren zu Microsoft Authentication Library (MSAL)
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über die Unterschiede zwischen Microsoft Authentication Library (MSAL) und Azure AD Authentication Library (ADAL) und deren Migration zu MSAL.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 02/27/2020
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 3af18eb09fd9906a0caaebda0b786795400467f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78165396"
---
# <a name="migrate-applications-to-microsoft-authentication-library-msal"></a>Migrieren von Anwendungen zu Microsoft Authentication Library (MSAL)

Sowohl Microsoft Authentication Library (MSAL) als auch Azure AD Authentication Library (ADAL) werden zum Authentifizieren von Azure AD-Entitäten und zum Anfordern von Token von Azure AD verwendet. Bisher haben die meisten Entwickler die Azure AD für Entwickler-Plattform (v1.0) genutzt, um Azure AD-Identitäten (Geschäfts-, Schul- und Unikonten) zu authentifizieren, indem sie unter Verwendung der Azure AD-Authentifizierungsbibliothek (ADAL) Token anforderten. Verwenden von MSAL:

- Sie können eine größere Gruppe von Microsoft-Identitäten (Azure AD-Identitäten und Microsoft-Konten sowie Social Media- und lokale Konten über Azure AD B2C) authentifizieren, da der Microsoft Identity Platform-Endpunkt verwendet wird.
- Ihre Benutzer erhalten die besten Funktionen für einmalige Anmelden (Single Sign-On, SSO).
- Sie können für Ihre Anwendung die inkrementelle Zustimmung aktivieren, und die Unterstützung des bedingten Zugriffs ist einfacher.
- Sie profitieren von der Innovation.

**MSAL wird jetzt als die Authentifizierungsbibliothek empfohlen, die mit Microsoft Identity Platform verwendet werden soll**. Für ADAL werden keine neuen Features implementiert. Der Schwerpunkt liegt auf der Verbesserung von MSAL.

In den folgenden Artikeln werden die Unterschiede zwischen der MSAL- und der ADAL-Bibliothek beschrieben. Außerdem finden Sie Unterstützung für die Migration:
- [Migrieren zu MSAL.NET](msal-net-migration.md)
- [Migrieren zu MSAL.js](msal-compare-msal-js-and-adal-js.md)
- [Migrieren zu MSAL.Android](migrate-android-adal-msal.md)
- [Migrieren zu MSAL.iOS/macOS](migrate-objc-adal-msal.md)
- [Migrieren zu MSAL Python](migrate-python-adal-msal.md)
- [Migrieren zu MSAL für Java](migrate-adal-msal-java.md)
- [Migrieren von Xamarin-Apps mithilfe von Brokern zu MSAL.NET](msal-net-migration-ios-broker.md)
