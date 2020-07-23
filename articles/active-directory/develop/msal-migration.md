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
ms.date: 06/16/2020
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: f84e479c4780437b09ab463781edbb52d8dfdfaa
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86255624"
---
# <a name="migrate-applications-to-microsoft-authentication-library-msal"></a>Migrieren von Anwendungen zu Microsoft Authentication Library (MSAL)

Viele Entwickler haben Anwendungen mit Azure Active Directory Authentication Library (ADAL) entwickelt und bereitgestellt. Jetzt empfehlen wir die Verwendung von Microsoft Authentication Library (MSAL) für die Authentifizierung und Autorisierung von Azure AD-Entitäten.

Die Vorteile von MSAL gegenüber ADAL:

- Sie können eine größere Anzahl von Identitäten authentifizieren:
  - Azure AD-Identitäten
  - Microsoft-Konten
  - Social Media- und lokale Konten bei Verwendung von Azure AD B2C
- Ihre Benutzer erhalten die besten Funktionen für einmalige Anmelden (Single Sign-On, SSO).
- Ihre Anwendung kann die inkrementelle Zustimmung aktivieren.
- Die Unterstützung des bedingten Zugriffs ist einfacher.
- Sie profitieren von Innovationen. Da sich alle Microsoft-Entwicklungsaufgaben nun auf MSAL konzentrieren, werden keine neuen Features in ADAL implementiert.

**MSAL wird jetzt als Authentifizierungsbibliothek für Microsoft Identity Platform empfohlen**.

## <a name="migration-guidance"></a>Hinweise zur Migration

Die folgenden Artikel unterstützen Sie bei der Migration zu MSAL:

- [Migrieren zu MSAL.Android](migrate-android-adal-msal.md)
- [Migrieren zu MSAL.iOS/macOS](migrate-objc-adal-msal.md)
- [Migrieren zu MSAL Java](migrate-adal-msal-java.md)
- [Migrieren zu MSAL.js](msal-compare-msal-js-and-adal-js.md)
- [Migrieren zu MSAL.NET](msal-net-migration.md)
- [Migrieren zu MSAL Python](migrate-python-adal-msal.md)
- [Migrieren von Xamarin-Apps mithilfe von Brokern zu MSAL.NET](msal-net-migration-ios-broker.md)

## <a name="frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ)

__F: Wird ADAL als veraltet markiert?__  
A: Ja. Ab dem 30. Juni 2020 werden ADAL keine neuen Features mehr hinzugefügt. Bis zum 30. Juni 2022 stellen wir weiterhin wichtige Sicherheitspatches für ADAL bereit.

__F: Woher weiß ich, welche meiner Apps ADAL verwendet?__  
A: Wenn Sie über den Quellcode für die Anwendung verfügen, können Sie anhand der oben genannten Artikel zur Migration ermitteln, welche Bibliothek die App verwendet und wie die Migration zu MSAL erfolgen kann. Wenn Sie keinen Zugriff auf den Quellcode Ihrer Anwendung haben, können Sie [eine Supportanfrage öffnen](developer-support-help-options.md#open-a-support-request), um eine Liste der registrierten Anwendungen mit der jeweils verwendeten Bibliothek zu erhalten.

__F: Funktionieren meine vorhandenen ADAL-Apps auch weiterhin?__  
A: Ihre vorhandenen Apps funktionieren auch weiterhin ohne Änderungen. Wenn geplant ist, die Anwendungen über den 30. Juni 2022 hinaus auszuführen, sollten Sie ein Update auf MSAL in Erwägung ziehen, um weiterhin die Sicherheit zu gewährleisten. Eine Migration zu MSAL ist jedoch nicht erforderlich, um die vorhandene Funktionalität aufrechtzuerhalten.

__F: Warum sollte ich in den Umstieg auf MSAL investieren?__  
A: MSAL enthält neue Features, die nicht in ADAL enthalten sind. Dazu zählen die inkrementelle Zustimmung, Einmaliges Anmelden (SSO) und die Tokencacheverwaltung. Im Gegensatz zu ADAL erhält MSAL auch nach dem 30. Juni 2022 weiterhin Sicherheitspatches. [Weitere Informationen](msal-overview.md)

__F: Wird ein Tool veröffentlicht, mit dem ich meine Apps von ADAL auf MSAL umstellen kann?__  
A: Nein. Bei den Unterschieden zwischen den Bibliotheken wären dedizierte Ressourcen für die Entwicklung und Wartung des Tools erforderlich, die so in die Verbesserung von MSAL fließen können. Wir stellen jedoch die obigen Artikel zur Migration bereit, um Ihnen bei der Durchführung der erforderlichen Änderungen in Ihrer Anwendung zu helfen.

__F: Wie funktioniert MSAL mit AD FS?__  
A: MSAL.NET unterstützt bestimmte Szenarien für die Authentifizierung bei AD FS 2019. Wenn Ihre App Token direkt aus einer früheren Version von AD FS abrufen muss, sollten Sie ADAL beibehalten. [Weitere Informationen](msal-net-adfs-support.md)

__F: Wie erhalte ich Hilfe bei der Migration meiner Anwendung?__  
A: Weitere Informationen finden Sie im Abschnitt [Hinweise zur Migration](#migration-guidance) in diesem Artikel. Wenn Sie nach dem Lesen der Anleitung für Ihre App-Plattform noch Fragen haben, können Sie auf Stack Overflow einen Beitrag mit dem Tag `[adal-deprecation]` veröffentlichen oder im GitHub-Repository der Bibliothek ein Problem öffnen. Links zum Repository der einzelnen Bibliotheken finden Sie im Übersichtsartikel zu MSAL im Abschnitt [Sprachen und Frameworks](msal-overview.md#languages-and-frameworks).

## <a name="next-steps"></a>Nächste Schritte

- [Aktualisieren Ihrer Anwendungen für die Verwendung von Microsoft Authentication Library und der Microsoft Graph-API](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/update-your-applications-to-use-microsoft-authentication-library/ba-p/1257363)
- [Übersicht über Microsoft Identity Platform](v2-overview.md)
- [Überprüfen der MSAL-Codebeispiele](sample-v2-code.md)
