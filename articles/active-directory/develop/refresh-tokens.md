---
title: 'Microsoft Identity Platform: Aktualisierungstoken | Azure'
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie mehr über Aktualisierungstoken, die von der Azure AD ausgegeben werden.
services: active-directory
author: SHERMANOUKO
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 05/25/2021
ms.author: shermanouko
ms.reviewer: mmacy, hirsin
ms.custom: aaddev, identityplatformtop40, fasttrack-edit
ms.openlocfilehash: 8b8bda71c637419dbd5b2bf7b181288abd9b965c
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112075239"
---
# <a name="microsoft-identity-platform-refresh-tokens"></a>Microsoft Identity Platform: Aktualisierungstoken

Wenn ein Client ein Zugriffstoken für den Zugriff auf eine geschützte Ressource abruft, erhält er auch ein Aktualisierungstoken. Das Aktualisierungstoken wird verwendet, um neue Zugriffs-/Aktualisierungstoken-Paare abzurufen, wenn das aktuelle Zugriffstoken abläuft. Aktualisierungstoken werden auch verwendet, um zusätzliche Zugriffstoken für andere Ressourcen abzurufen. Aktualisierungstoken sind an eine Kombination aus Benutzer und Client gebunden, aber nicht an eine Ressource oder einen Mandanten. Dies ermöglicht es einem Client, mithilfe eines Aktualisierungstoken Zugriffstoken für eine beliebige Kombination von Ressourcen und Mandanten abzurufen, für die er dazu berechtigt ist. Aktualisierungstoken werden verschlüsselt und können nur von Microsoft Identity Platform gelesen werden.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie sich diesen Artikel durchlesen, sollten Sie die folgenden Artikel durchgehen:

* Microsoft Identity Platform – [ID-Token](id-tokens.md)
* Microsoft Identity Platform-[Zugriffstoken](access-tokens.md)

## <a name="refresh-token-lifetime"></a>Lebensdauer von Aktualisierungstoken

Aktualisierungstoken haben eine deutlich längere Lebensdauer als Zugriffstoken. Die Standardlebensdauer für die Token beträgt 90 Tage,und sie ersetzen sich selbst bei jeder Verwendung durch ein neues Token. Das bedeutet, dass immer dann, wenn ein Aktualisierungstoken zum Abrufen eines neuen Zugriffstokens verwendet wird, auch ein neues Aktualisierungstoken ausgegeben wird. Microsoft Identity Platform widerruft keine alten Aktualisierungstoken, wenn damit neue Zugriffstoken abgerufen werden. Löschen Sie das alte Aktualisierungstoken dauerhaft, nachdem Sie ein neues Token abgerufen haben. Aktualisierungstoken müssen wie Zugriffstoken oder Anwendungsanmeldeinformationen sicher gespeichert werden. 

## <a name="refresh-token-expiration"></a>Ablauf von Aktualisierungstoken

Aktualisierungstoken können aufgrund von Timeouts und Sperrungen jederzeit widerrufen werden. Ihre App muss in diesem Fall Ablehnungen durch den Anmeldedienst ordnungsgemäß behandeln. Dazu wird der Benutzer an eine interaktive Anmeldeaufforderung zur erneuten Anmeldung weitergeleitet. 

### <a name="token-timeouts"></a>Tokenzeitüberschreitungen

Mit der [Konfiguration der Tokengültigkeitsdauer](active-directory-configurable-token-lifetimes.md#refresh-and-session-token-lifetime-policy-properties) kann die Lebensdauer von Aktualisierungstoken verringert oder verlängert werden. Mit dieser Einstellung wird geändert, wie lange ein ungenutztes Aktualisierungstoken gültig bleibt. Stellen Sie sich beispielsweise ein Szenario vor, in dem ein Benutzer eine App 90 Tage lang nicht öffnet. Wenn die App versucht, dieses über 90 Tage alte Aktualisierungstoken zu verwenden, wird sie feststellen, dass es abgelaufen ist. Darüber hinaus kann ein Administrator verlangen, dass in regelmäßigen Abständen zweite Faktoren verwendet werden, sodass sich der Benutzer in bestimmten Intervallen manuell anmelden muss. Zu diesen Szenarien gehören:

* Inaktivität: Aktualisierungstoken sind nur für den durch `MaxInactiveTime` bestimmten Zeitraum gültig.  Wird das Token innerhalb dieses Zeitraums nicht verwendet (und durch das neue Token ersetzt), kann es nicht mehr verwendet werden.
* Veraltung der Sitzung: Wenn `MaxAgeSessionMultiFactor` oder `MaxAgeSessionSingleFactor` auf einen anderen Wert als die Standardeinstellung (Until-revoked) festgelegt wurden, ist eine erneute Authentifizierung erforderlich, nachdem der in „MaxAgeSession*“ festgelegte Zeitraum abgelaufen ist.  Diese Einstellung wird verwendet, um zu erzwingen, dass sich Benutzer in regelmäßigen Abständen mit einem ersten oder zweiten Faktor erneut authentifizieren. 
* Beispiele:
  * Für den Mandanten ist ein MaxInactiveTime-Wert von fünf Tagen festgelegt, und der Benutzer ist eine Woche lang im Urlaub. Daher hat Azure AD seit sieben Tagen keine neue Tokenanforderung vom Benutzer erhalten. Bei der nächsten Anforderung eines neuen Tokens durch den Benutzer wurde das Aktualisierungstoken widerrufen, sodass der Benutzer seine Anmeldeinformationen erneut eingeben muss.
  * Für eine vertrauliche Anwendung gilt ein `MaxAgeSessionMultiFactor`-Wert von einem Tag. Ein Benutzer muss die MFA über eine interaktive Eingabeaufforderung erneut ausführen, wenn er sich nach einem Tag erneut anmeldet. Beispiel: Ein Benutzer meldet sich am Montag und nach 25 Stunden am Dienstag an. 

Nicht alle Aktualisierungstoken folgen den Regeln, die in der Richtlinie für die Tokengültigkeitsdauer festgelegt sind. Insbesondere in [Single-Page-Apps](reference-third-party-cookies-spas.md) verwendete Aktualisierungstoken sind stets auf 24 Stunden Aktivität beschränkt, als würde für sie eine `MaxAgeSessionSingleFactor`-Richtlinie von 24 Stunden gelten. 

### <a name="revocation"></a>Widerruf

Aktualisierungstoken können vom Server aufgrund einer Änderung der Anmeldeinformationen oder aufgrund einer Benutzeraktion oder Administratoraktion widerrufen werden.  Aktualisierungstoken werden in zwei Klassen unterteilt: Token, die für vertrauliche Clients ausgestellt werden (die Spalte ganz rechts), und Token, die für öffentliche Clients (alle anderen Spalten) ausgestellt werden.

| Change | Kennwortbasiertes Cookie | Kennwortbasiertes Token | Nicht kennwortbasiertes Cookie | Nicht kennwortbasiertes Token | Vertrauliches Clienttoken |
|---|-----------------------|----------------------|---------------------------|--------------------------|---------------------------|
| Kennwort läuft ab | Bleibt aktiv | Bleibt aktiv | Bleibt aktiv | Bleibt aktiv | Bleibt aktiv |
| Kennwort wird vom Benutzer geändert | Widerrufen | Widerrufen | Bleibt aktiv | Bleibt aktiv | Bleibt aktiv |
| Benutzer verwendet SSPR | Widerrufen | Widerrufen | Bleibt aktiv | Bleibt aktiv | Bleibt aktiv |
| Administrator setzt Kennwort zurück | Widerrufen | Widerrufen | Bleibt aktiv | Bleibt aktiv | Bleibt aktiv |
| Benutzer widerruft Aktualisierungstoken [über PowerShell](/powershell/module/azuread/revoke-azureadsignedinuserallrefreshtoken) | Widerrufen | Widerrufen | Widerrufen | Widerrufen | Widerrufen |
| Administrator widerruft alle Aktualisierungstoken für einen Benutzer [über PowerShell](/powershell/module/azuread/revoke-azureaduserallrefreshtoken) | Widerrufen | Widerrufen |Widerrufen | Widerrufen | Widerrufen |
| Einmaliges Abmelden [im Web](v2-protocols-oidc.md#single-sign-out) | Widerrufen | Bleibt aktiv | Widerrufen | Bleibt aktiv | Bleibt aktiv |

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die [konfigurierbare Tokengültigkeitsdauer](active-directory-configurable-token-lifetimes.md).
* Weitere Informationen zu primären Aktualisierungstoken finden Sie unter [Was ist ein primäres Aktualisierungstoken (Primary Refresh Token, PRT)?](../devices/concept-primary-refresh-token.md).
