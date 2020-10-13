---
title: SSO-Sitzungen in Azure Active Directory B2C | Microsoft-Dokumentation
description: Hier finden Sie eine Übersicht über das Konfigurieren des Sitzungsverhaltens in Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 0004c874a2011a78bb5cfe67ff0a840224d47bbb
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91258964"
---
# <a name="azure-ad-b2c-session"></a>Azure AD B2C-Sitzung

Mit dem einmaligen Anmelden (Single Sign-On, SSO) ist für Sicherheit und Komfort gesorgt, wenn sich Benutzer anwendungsübergreifend in Azure Active Directory B2C (Azure AD B2C) anmelden. In diesem Artikel werden die Methoden für das einmalige Anmelden beschrieben, die in Azure AD B2C verwendet werden. Sie erhalten zudem Hilfestellung beim Auswählen der am besten geeigneten SSO-Methode zum Konfigurieren Ihrer Anwendungen.

Mit der einmaligen Anmeldung melden sich Benutzer einmal mit einem einzigen Konto an und erhalten Zugriff auf mehrere Anwendungen. Dabei kann es sich unabhängig von Plattform oder Domänennamen um eine mobile Anwendung bzw. eine Web- oder Single-Page-Anwendung handeln.

Wenn sich der Benutzer anfänglich bei einer Anwendung anmeldet, speichert Azure AD B2C eine cookiebasierte Sitzung. Bei nachfolgenden Authentifizierungsanforderungen liest und überprüft Azure AD B2C die cookiebasierte Sitzung und gibt ein Zugriffstoken aus, ohne den Benutzer zur erneuten Anmeldung aufzufordern. Wenn die cookiebasierte Sitzung abläuft oder ungültig wird, wird der Benutzer zur erneuten Anmeldung aufgefordert.  

## <a name="sso-session-types"></a>SSO-Sitzungstypen

Die Integration mit Azure AD B2C beinhaltet drei SSO-Sitzungstypen:

- **Azure AD B2C**: Sitzung wird von Azure AD B2C verwaltet
- **Verbundidentitätsanbieter**: Sitzung wird vom Identitätsanbieter (z. B. Facebook, Salesforce oder Microsoft-Konto) verwaltet
- **Anwendung**: Sitzung wird von der mobilen Anwendung bzw. der Web- oder Single-Page-Anwendung verwaltet

![SSO-Sitzung](media/session-overview/sso-session-types.png)

### <a name="azure-ad-b2c-sso"></a>SSO mit Azure AD B2C 

Wenn sich ein Benutzer erfolgreich mit einem lokalen oder Social Media-Konto authentifiziert, speichert Azure AD B2C eine cookiebasierte Sitzung im Browser des Benutzers. Das Cookie wird unter dem Domänennamen des Azure AD B2C-Mandanten gespeichert, z. B. `https://contoso.b2clogin.com`.

Wenn ein Benutzer sich anfänglich mit einem Verbundkonto anmeldet und sich während des Sitzungszeitfensters (Gültigkeitsdauer) bei derselben oder einer anderen App anmeldet, versucht Azure AD B2C, ein neues Zugriffstoken vom Verbundidentitätsanbieter abzurufen. Wenn die Verbundidentitätsanbieter-Sitzung abgelaufen ist oder ungültig wird, fordert der Verbundidentitätsanbieter den Benutzer zur Eingabe seiner Anmeldeinformationen auf. Wenn die Sitzung noch aktiv ist (oder wenn sich der Benutzer mit einem lokalen statt einem Verbundkonto angemeldet hat), autorisiert Azure AD B2C den Benutzer und eliminiert weitere Eingabeaufforderungen.

Sie können das Sitzungsverhalten konfigurieren. Dazu zählen auch die Gültigkeitsdauer der Sitzung und die Art, wie Azure AD B2C die Sitzung über Richtlinien und Anwendungen freigibt.

### <a name="federated-identity-provider-sso"></a>SSO mit Verbundidentitätsanbieter

Ein Social Media- oder Unternehmensidentitätsanbieter verwaltet seine eigene Sitzung. Das Cookie wird unter dem Domänennamen des Identitätsanbieters gespeichert, z. B. `https://login.salesforce.com`. Die Verbundidentitätsanbieter-Sitzung wird nicht von Azure AD B2C gesteuert. Das Sitzungsverhalten wird stattdessen vom Verbundidentitätsanbieter bestimmt. 

Nehmen Sie das folgende Szenario als Beispiel:

1. Ein Benutzer meldet sich bei Facebook an, um seinen Feed zu überprüfen.
2. Zu einem späteren Zeitpunkt öffnet der Benutzer Ihre Anwendung und startet den Anmeldevorgang. Die Anwendung leitet den Benutzer zu Azure AD B2C um, um den Anmeldevorgang abzuschließen.
3. Auf der Registrierungs- oder Anmeldeseite von Azure AD B2C wählt der Benutzer die Anmeldung mit seinem Facebook-Konto aus. Der Benutzer wird zu Facebook umgeleitet. Wenn auf Facebook eine aktive Sitzung vorhanden ist, wird der Benutzer nicht aufgefordert, seine Anmeldeinformationen einzugeben, sondern wird sofort mit einem Facebook-Token zu Azure AD B2C umgeleitet.

### <a name="application-sso"></a>Anwendungs-SSO

Eine mobile Anwendung bzw. eine Web- oder Single-Page-Anwendung kann durch OAuth-Zugriff, ID-Token oder SAML-Token geschützt werden. Wenn ein Benutzer versucht, auf eine geschützte Ressource in der App zuzugreifen, prüft die App, ob seitens der Anwendung eine aktive Sitzung vorhanden ist. Wenn keine App-Sitzung vorhanden oder die Sitzung abgelaufen ist, leitet die App den Benutzer zur Azure AD B2C-Anmeldeseite.

Die Anwendungssitzung kann eine cookiebasierte Sitzung sein, die unter dem Domänennamen der Anwendung gespeichert wird, z. B. `https://contoso.com`. Mobile Anwendungen speichern die Sitzung möglicherweise anders, verwenden aber einen ähnlichen Ansatz.

## <a name="azure-ad-b2c-session-configuration"></a>Konfiguration der Azure AD B2C-Sitzung

### <a name="session-scope"></a>Bereich einer Sitzung

Die Azure AD B2C-Sitzung kann mit den folgenden Bereichen konfiguriert werden:

- **Mandant**: Dies ist die Standardeinstellung. Mit dieser Einstellung können mehrere Anwendungen und Benutzerflows in Ihrem B2C-Mandanten die gleiche Benutzersitzung gemeinsam nutzen. Beispiel: Sobald sich ein Benutzer bei einer Anwendung angemeldet hat, kann er sich auch beim Zugriff auf eine andere Anwendung nahtlos anmelden.
- **Anwendung**: Mit dieser Einstellung können Sie eine Benutzersitzung ausschließlich für eine Anwendung beibehalten, unabhängig von anderen Anwendungen. Beispiel: Diese Einstellung können Sie verwenden, wenn sich der Benutzer bei Contoso Pharmacy anmelden soll, auch wenn er sich bereits bei Contoso Groceries angemeldet hat.
- **Richtlinie**: Mit dieser Einstellung können Sie eine Benutzersitzung ausschließlich für einen Benutzerflow beibehalten, unabhängig von den Anwendungen, die diesen verwenden. Beispiel: Wenn sich der Benutzer bereits angemeldet und einen Multi-Factor Authentication-Schritt (MFA) abgeschlossen hat, kann er Zugriff auf Bestandteile mit höherer Sicherheitsstufe von mehreren Anwendungen erhalten, solange die an den Benutzerflow gebundene Sitzung nicht abläuft.
- **Deaktiviert**: Diese Einstellung zwingt den Benutzer, bei jeder Ausführung der Richtlinie den gesamten Benutzerflow zu durchlaufen.

### <a name="session-life-time"></a>Gültigkeitsdauer der Sitzung

Die **Gültigkeitsdauer der Sitzung** ist die Zeitspanne, in der das Azure AD B2C-Sitzungscookie nach erfolgreicher Authentifizierung im Browser des Benutzers gespeichert wird. Für die Gültigkeitsdauer der Sitzung können Sie einen Wert zwischen 15 und 720 Minuten festlegen.

### <a name="keep-me-signed-in"></a>Angemeldet bleiben

Mit der Funktion [Angemeldet bleiben](custom-policy-keep-me-signed-in.md) wird die Gültigkeitsdauer der Sitzung durch die Verwendung eines permanenten Cookies verlängert. Die Sitzung bleibt auch nach Schließen und erneutem Öffnen des Browsers aktiv. Die Sitzung wird nur widerrufen, wenn sich der Benutzer abmeldet. Die Funktion „Angemeldet bleiben“ gilt nur für die Anmeldung mit lokalen Konten.

Die Funktion „Angemeldet bleiben“ hat Vorrang vor der Gültigkeitsdauer der Sitzung. Wenn die Funktion „Angemeldet bleiben“ aktiviert ist und der Benutzer sie auswählt, gibt diese Funktion vor, wann die Sitzung abläuft. 

### <a name="session-expiry-type"></a>Sitzungsablauftyp

Der **Sitzungsablauftyp** gibt an, wie eine Sitzung durch die Einstellung der Gültigkeitsdauer der Sitzung oder die Festlegung der Funktion „Angemeldet bleiben“ verlängert wird.

- **Rollierend**: Gibt an, dass die Sitzung jedes Mal verlängert wird, wenn der Benutzer eine cookiebasierte Authentifizierung ausführt (Standardeinstellung).
- **Absolut**: Gibt an, dass sich der Benutzer nach dem angegebenen Zeitraum erneut authentifizieren muss.

## <a name="sign-out"></a>Abmeldung

Wenn Sie den Benutzer bei der Anwendung abmelden möchten, reicht es nicht aus, die Cookies der Anwendung zu löschen oder die Sitzung mit dem Benutzer auf andere Weise zu beenden. Sie müssen den Benutzer für die Abmeldung zu Azure AD B2C umleiten. Ansonsten kann sich der Benutzer möglicherweise erneut bei Ihrer Anwendung authentifizieren, ohne seine Anmeldeinformationen erneut einzugeben.

Auf die Abmeldeanforderung reagiert Azure AD B2C wie folgt:

1. Die cookiebasierte Azure AD B2C-Sitzung wird für ungültig erklärt.
1. Azure AD B2C versucht, den Benutzer vom Verbundidentitätsanbieter abzumelden:
   - OpenId Connect, wenn der bekannte Konfigurationsendpunkt des Identitätsanbieters eine `end_session_endpoint`-Adresse angibt.
   - SAML, wenn die Metadaten des Identitätsanbieters die `SingleLogoutService`-Adresse enthalten.
1. Optional nimmt Azure AD B2C die Abmeldung bei anderen Anwendungen vor. Weitere Informationen finden Sie im Abschnitt [Einmaliges Abmelden](#single-sign-out).

Bei der Abmeldung wird zwar der SSO-Status des Benutzers bei Azure AD B2C gelöscht, der Benutzer wird möglicherweise jedoch nicht von seiner Sitzung beim Social Media-Identitätsanbieter abgemeldet. Wenn der Benutzer bei einer nachfolgenden Anmeldung den gleichen Identitätsanbieter auswählt, kann er sich möglicherweise ohne Eingabe seiner Anmeldeinformationen erneut authentifizieren. Wenn sich ein Benutzer von der Anwendung abmelden möchte, bedeutet dies nicht unbedingt, dass er sich auch vollständig von seinem Facebook-Konto abmelden möchte. Wenn jedoch lokale Konten verwendet werden, wird die Sitzung des Benutzers ordnungsgemäß beendet.

### <a name="single-sign-out"></a>Einmaliges Abmelden 


> [!NOTE]
> Diese Funktion ist auf [benutzerdefinierte Richtlinien](custom-policy-overview.md) beschränkt.

Wenn Sie den Benutzer zum Azure AD B2C-Abmeldeendpunkt (für OAuth2- und SAML-Protokolle) umleiten, löscht Azure AD B2C die Sitzung des Benutzers im Browser. Allerdings kann der Benutzer weiterhin bei anderen Anwendungen angemeldet sein, die Azure AD B2C für die Authentifizierung verwenden. Damit der Benutzer gleichzeitig auch bei diesen Anwendungen abgemeldet werden kann, sendet Azure AD B2C eine HTTP GET-Anforderung an die registrierte `LogoutUrl` aller Anwendungen, bei denen der Benutzer aktuell angemeldet ist.


Anwendungen müssen auf diese Anforderung antworten, indem sie alle Sitzungen löschen, mit denen der Benutzer identifiziert wird, und eine `200`-Antwort zurückgeben. Wenn Sie das einmalige Abmelden in Ihrer Anwendung unterstützen möchten, müssen Sie eine `LogoutUrl` im Code Ihrer Anwendung implementieren. 

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum [Konfigurieren des Sitzungsverhaltens in Benutzerflows](session-behavior.md).
- Informationen zum [Konfigurieren des Sitzungsverhaltens in benutzerdefinierten Richtlinien](session-behavior-custom-policy.md).