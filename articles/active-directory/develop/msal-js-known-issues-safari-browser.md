---
title: Bekannte Probleme mit dem Safari-Browser (MSAL.js) | Azure
titleSuffix: Microsoft identity platform
description: Enthält Informationen zu bekannten Problemen bei der Verwendung der Microsoft Authentication Library für JavaScript (MSAL.js) mit dem Safari-Browser.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: troubleshooting
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: edb995e31c2872c1541e29fee09dd66aafc8f9e2
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "76696111"
---
# <a name="known-issues-on-safari-browser-with-msaljs"></a>Bekannte Probleme im Safari-Browser mit „MSAL.js“ 

## <a name="silent-token-renewal-on-safari-12-and-itp-20"></a>Automatische Tokenverlängerung unter Safari 12 und ITP 2.0

Die Betriebssysteme Apple iOS 12 und MacOS 10.14 haben eine Version des [Safari 12-Browsers](https://developer.apple.com/safari/whats-new/) enthalten. Aus Sicherheits- und Datenschutzgründen verfügt Safari 12 über [Intelligent Tracking Prevention 2.0](https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/). Hiermit wird im Wesentlichen erreicht, dass der Browser das Festlegen der Cookies von Drittanbietern verhindert. Bei ITP 2.0 werden außerdem Cookies, die von Identitätsanbietern festgelegt werden, als Cookies von Drittanbietern behandelt.

### <a name="impact-on-msaljs"></a>Auswirkung auf „MSAL.js“

Für „MSAL.js“ wird ein ausgeblendeter IFrame verwendet, um die automatische Tokenbeschaffung und -erneuerung im Rahmen von `acquireTokenSilent`-Aufrufen durchzuführen. Für die automatischen Tokenanforderungen muss der IFrame über Zugriff auf die authentifizierte Benutzersitzung verfügen, die durch die von Azure AD festgelegten Cookies repräsentiert wird. Da der Zugriff auf diese Cookies durch ITP 2.0 verhindert wird, ist für „MSAL.js“ die automatische Beschaffung und Erneuerung von Token nicht möglich, und es treten `acquireTokenSilent`-Fehler auf.

Derzeit gibt es keine Lösung für dieses Problem, und wir evaluieren gerade zusammen mit der Community für Standards mögliche Optionen.

### <a name="work-around"></a>Problemumgehung

Die ITP-Einstellung ist auf dem Safari-Browser standardmäßig aktiviert. Sie können diese Einstellung unter **Einstellungen** -> **Datenschutz** über die Option **Websiteübergreifende Nachverfolgung blockieren** deaktivieren.

![Safari-Einstellung](./media/msal-js-known-issue-safari-browser/safari.png)

Sie müssen die `acquireTokenSilent`-Fehler mit einem interaktiven Aufruf zur Tokenbeschaffung verarbeiten, wobei der Benutzer zum Anmelden aufgefordert wird.
Ein möglicher Ansatz zur Vermeidung von wiederholten Anmeldungen ist das Behandeln des `acquireTokenSilent`-Fehlers und das Bereitstellen einer Option für Benutzer, mit der vor dem Fortfahren mit dem interaktiven Aufruf die ITP-Einstellung in Safari deaktiviert werden kann. Nachdem die Einstellung deaktiviert wurde, sollten die nachfolgenden automatischen Tokenerneuerungen erfolgreich verlaufen.
