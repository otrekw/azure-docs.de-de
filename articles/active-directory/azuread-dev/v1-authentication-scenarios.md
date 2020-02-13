---
title: Azure AD für Entwickler (v1.0) | Azure
description: Erlernen Sie die Grundlagen der Authentifizierung für Azure AD für Entwickler (v1.0), z. B. das App-Modell, die API, die Bereitstellung und die gängigsten Authentifizierungsszenarien.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 0c84e7d0-16aa-4897-82f2-f53c6c990fd9
ms.service: active-directory
ms.subservice: azuread-dev
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/14/2019
ms.author: ryanwi
ms.reviewer: saeeda, sureshja, hirsin
ms.custom: aaddev
ms.openlocfilehash: 07bdc8af5a3bc82f0fc5aad52648a9524a2dc87f
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77163399"
---
# <a name="what-is-authentication"></a>Was ist Authentifizierung?

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

*Authentifizierung* ist die Anforderung ordnungsgemäßer Anmeldeinformationen von einer Partei. Sie bildet die Grundlage für die Erstellung eines Sicherheitsprinzipals für die Identitäts- und Zugriffssteuerung. Vereinfacht ausgedrückt, ist es der Vorgang, mit dem Sie den Nachweis erbringen, dass Sie derjenige sind, für den Sie sich ausgeben. Authentifizierung wird manchmal verkürzt als AuthN bezeichnet.

*Autorisierung* ist die Gewährung einer Handlungsberechtigung für einen authentifizierten Sicherheitsprinzipal. Sie gibt an, auf welche Daten Sie zugreifen dürfen und welche Aktionen Sie damit ausführen können. Autorisierung wird manchmal verkürzt als AuthZ bezeichnet.

Azure Active Directory für Entwickler (v1.0) (Azure AD) vereinfacht die Authentifizierung für Anwendungsentwickler durch die Bereitstellung von Identity-as-a-Service, durch Unterstützung branchenüblicher Protokolle wie OAuth 2.0 und OpenID Connect sowie durch Open-Source-Bibliotheken für verschiedene Plattformen, sodass Sie schnell mit der Programmierung beginnen können.

Im Programmiermodell von Azure AD sind im Wesentlichen zwei Anwendungsfälle vorgesehen:

* Im Rahmen einer OAuth 2.0-Autorisierungsgewährung: Wenn der Ressourcenbesitzer die Clientanwendung autorisiert und dem Client dadurch Zugriff auf Ressourcen des Ressourcenbesitzers gewährt.
* Im Rahmen des Ressourcenzugriffs durch den Client: Gemäß Implementierung durch den Ressourcenserver unter Verwendung der Werte des Anspruchs aus dem Zugriffstoken, die als Grundlage für Zugriffssteuerungsentscheidungen herangezogen werden.

## <a name="authentication-basics-in-azure-ad"></a>Azure AD-Authentifizierungsgrundlagen

Betrachten Sie das einfachste Szenario, bei dem eine Identität erforderlich ist: Ein Benutzer in einem Webbrowser muss sich gegenüber einer Webanwendung authentifizieren. Im folgenden Diagramm wird dieses Szenario veranschaulicht:

![Übersicht über die Anmeldung bei Webanwendungen](./media/v1-authentication-scenarios/auth-basics-microsoft-identity-platform.svg)

Folgendes müssen Sie über die verschiedenen im Diagramm gezeigten Komponenten wissen:

* Azure AD ist der Identitätsanbieter. Der Identitätsanbieter ist für die Überprüfung der Identität von Benutzern und Anwendungen zuständig, die im Verzeichnis einer Organisation vorhanden sind, und bei erfolgreicher Authentifizierung dieser Benutzer und Anwendungen auch für die Ausgabe von Sicherheitstoken.
* Jede Anwendung, die ihre Authentifizierung in Azure AD auslagern möchte, muss in Azure Active Directory (Azure AD) registriert werden. Azure AD registriert die App im Verzeichnis und identifiziert sie eindeutig.
* Mit den Open-Source-Authentifizierungsbibliotheken von Azure AD können sich die Entwickler um die Protokolldetails kümmern und die Authentifizierung für Sie vereinfachen. Weitere Informationen finden Sie in den [v2.0-Authentifizierungsbibliotheken](../develop/reference-v2-libraries.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) und [v1.0-Authentifizierungsbibliotheken](active-directory-authentication-libraries.md) für Microsoft Identity Platform.
* Nach der Authentifizierung eines Benutzers muss die Anwendung das Sicherheitstoken des Benutzers überprüfen, um sicherzustellen, dass die Authentifizierung erfolgreich war. Es sind Schnellstarts, Tutorials und Codebeispiele in einer Vielzahl von Sprachen und Frameworks verfügbar, die zeigen, wie die Anwendung vorgehen muss.
  * Um eine App schnell zu erstellen und Funktionen wie das Abrufen von Token, Aktualisieren von Token, Anmelden eines Benutzers, Anzeigen einiger Benutzerinformationen und vieles mehr hinzuzufügen, lesen Sie den Abschnitt **Schnellstarts** in der Dokumentation.
  * Ausführlichere, szenariobasierte Verfahren für wichtige Authentifizierungsentwickleraufgaben wie das Abrufen von Zugriffstoken und deren Verwendung in Aufrufen der Microsoft Graph-API und anderer APIs, die Implementierung der Anmeldung bei Microsoft mit einer traditionellen webbrowsergestützten App unter Verwendung von OpenID Connect und mehr finden Sie im Abschnitt **Tutorials** der Dokumentation.
  * Um Codebeispiele herunterzuladen, navigieren Sie zu [GitHub](https://github.com/Azure-Samples?q=active-directory).
* Der Fluss von Anforderungen und Antworten für den Authentifizierungsprozess ergibt sich aus dem von Ihnen verwendeten Authentifizierungsprotokoll (etwa OAuth 2.0, OpenID Connect, WS-Federation oder SAML 2.0). Weitere Informationen zu Protokollen finden Sie im Abschnitt **Konzepte > Authentifizierungsprotokoll** der Dokumentation.

Im Beispielszenario oben können Sie die Apps gemäß diesen beiden Rollen klassifizieren:

* Apps, die sicher auf Ressourcen zugreifen müssen
* Apps, die die Rolle der Ressource selbst übernehmen

### <a name="how-each-flow-emits-tokens-and-codes"></a>Ausgabe von Token und Codes für die einzelnen Abläufe

Je nach Art Ihres Clients können die Authentifizierungsflows (einer oder mehrere) verwendet werden, die von Azure AD unterstützt werden. Mit diesen Abläufen können verschiedene Token (ID-Token, Aktualisierungstoken, Zugriffstoken) und Autorisierungscodes erstellt werden, und es sind unterschiedliche Token erforderlich. Dieses Diagramm enthält eine Übersicht:

|Flow | Erforderlich | id_token | Zugriffstoken | Aktualisierungstoken | Autorisierungscode | 
|-----|----------|----------|--------------|---------------|--------------------|
|[Autorisierungscodeflow](v1-protocols-oauth-code.md) | | x | x | x | x|  
|[Impliziter Flow](v1-oauth2-implicit-grant-flow.md) | | x        | x    |      |                    |
|[Hybrid-OIDC-Ablauf](v1-protocols-openid-connect-code.md#get-access-tokens)| | x  | |          |            x   |
|[Einlösung des Aktualisierungstokens](v1-protocols-oauth-code.md#refreshing-the-access-tokens) | Aktualisierungstoken | x | x | x| |
|[„Im Auftrag von“-Ablauf](v1-oauth2-on-behalf-of-flow.md) | Zugriffstoken| x| x| x| |
|[Clientanmeldeinformationen](v1-oauth2-client-creds-grant-flow.md) | | | x (nur App)| | |

Für Token, die im impliziten Modus ausgestellt werden, gilt eine Längenbeschränkung, weil sie per URL zurück an den Browser übergeben werden (`response_mode` ist hierbei `query` oder `fragment`).  Für einige Browser gilt eine Größenbeschränkung für die URL, die in die Browserleiste eingefügt werden kann. Es tritt ein Fehler auf, wenn die URL zu lang ist.  Diese Token verfügen daher nicht über Ansprüche der Art `groups` oder `wids`. 

Da Sie nun einen Überblick über die Grundlagen haben, lesen Sie weiter, um das App-Identitätsmodell und die API zu verstehen, um zu erfahren, wie die Bereitstellung in Azure AD funktioniert, und um Links zu detaillierten Informationen zu den allgemeinen Szenarien abzurufen, die Azure AD unterstützt.

## <a name="application-model"></a>Anwendungsmodell

Azure AD stellt Anwendungen dar, die einem bestimmten Modell folgen, das zwei Hauptfunktionen erfüllen soll:

* **Identifizieren der App anhand der von ihr unterstützten Authentifizierungsprotokolle**: Dies beinhaltet die Aufzählung aller Bezeichner, URLs, Geheimnisse und verwandten Informationen, die zur Authentifizierungszeit benötigt werden. Auf Azure AD trifft dabei Folgendes zu:

    * Enthält alle Daten, die zur Unterstützung der Authentifizierung zur Laufzeit erforderlich sind.
    * Enthält alle Daten, um zu entscheiden, auf welche Ressourcen eine App zugreifen muss und ob und unter welchen Umständen eine bestimmte Anforderung erfüllt werden soll.
    * Stellt die Infrastruktur für die Implementierung der App-Bereitstellung innerhalb des Mandanten des App-Entwicklers und für andere Azure AD-Mandanten bereit.

* **Behandeln der Zustimmung des Benutzers während der Tokenanforderungszeit und Ermöglichen der dynamischen mandantenübergreifenden Bereitstellung von Apps**. Hierbei übernimmt Azure AD die folgenden Aufgaben:

    * Ermöglicht Benutzern und Administratoren, der App dynamisch die Zustimmung zum Zugriff auf Ressourcen in ihrem Namen zu erteilen oder zu verweigern.
    * Ermöglicht es Administratoren, letztendlich zu entscheiden, welche Aktionen Apps ausführen und welche Benutzer bestimmte Apps verwenden dürfen und wie auf die Verzeichnisressourcen zugegriffen wird.

In Azure AD beschreibt ein **Anwendungsobjekt** eine Anwendung als eine abstrakte Entität. Entwickler arbeiten mit Anwendungen. Zum Zeitpunkt der Bereitstellung verwendet Azure AD ein angegebenes Anwendungsobjekt als Blaupause zum Erstellen eines **Dienstprinzipals**, der eine konkrete Instanz einer Anwendung in einem Verzeichnis oder einem Mandanten darstellt. Es ist der Dienstprinzipal, der definiert, welche Aktionen die App in einem bestimmten Zielverzeichnis ausführen darf, wer sie verwenden kann, auf welche Ressourcen sie Zugriff besitzt usw. Azure AD erstellt einen Dienstprinzipal aus einem Anwendungsobjekt über **Zustimmung**.

Die folgende Abbildung zeigt einen vereinfachten Azure AD-Bereitstellungsablauf, der durch Zustimmung gesteuert wird.  Darin gibt es zwei Mandanten (A und B), wobei Mandant A Besitzer der Anwendung ist, und Mandant B die Anwendung über einen Dienstprinzipal instanziiert.  

![Vereinfachter Bereitstellungsablauf, der durch Zustimmung gesteuert wird](./media/v1-authentication-scenarios/simplified-provisioning-flow-consent-driven.svg)

In diesem Bereitstellungsablauf geschieht Folgendes:

1. Ein Benutzer von Mandant B versucht, sich mit der App anzumelden, und der Autorisierungsendpunkt fordert ein Token für die Anwendung an.
1. Die Anmeldeinformationen des Benutzers werden abgerufen und für die Authentifizierung überprüft.
1. Der Benutzer wird aufgefordert, dem Zugriff auf den Mandanten B für die App zuzustimmen.
1. Azure AD verwendet das Anwendungsobjekt in Mandant A als Blaupause für das Erstellen eines Dienstprinzipals in Mandant B.
1. Der Benutzer erhält das angeforderte Token.

Sie können diesen Vorgang beliebig oft für andere Mandanten (C, D, usw.) wiederholen. Der Mandant A enthält die Blaupause für die App (Anwendungsobjekt). Benutzer und Administratoren aller anderen Mandanten, in denen der App die Zustimmung erteilt wird, behalten durch das entsprechende Dienstprinzipalobjekt in jedem Mandaten die Kontrolle darüber, welche Aktionen die Anwendung ausführen darf. Weitere Informationen finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Microsoft Identity Platform](../develop/app-objects-and-service-principals.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json).

## <a name="claims-in-azure-ad-security-tokens"></a>Ansprüche in Sicherheitstokens von Azure AD

Von Azure AD ausgestellte Sicherheitstoken (Zugriffs- und ID-Token) enthalten Ansprüche oder Assertionen von Informationen zum authentifizierten Antragsteller. Anwendungen können Ansprüche für verschiedene Aufgaben verwenden, z.B. für die folgenden:

* Überprüfen des Tokens
* Identifizieren des Verzeichnismandanten des Antragstellers
* Anzeigen der Benutzerinformationen
* Bestimmen der Autorisierung des Antragstellers

Welche Ansprüche in einem Sicherheitstoken enthalten sind, hängt von der Art des Tokens, von der Art der Anmeldeinformationen für die Benutzerauthentifizierung sowie von der Anwendungskonfiguration ab.

Die folgende Tabelle enthält eine kurze Beschreibung der einzelnen Anspruchsarten, die von Azure AD ausgegeben werden. Weitere Informationen finden Sie finden Sie in den Themen zu den [Zugriffstoken](../develop/access-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) und [ID-Token](../develop/id-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json), die von Azure AD ausgestellt werden.

| Anspruch | BESCHREIBUNG |
| --- | --- |
| Anwendungs-ID | Die Anwendung, die das Token verwendet. |
| Zielgruppe | Die Empfängerressource, für die das Token vorgesehen ist. |
| Application Authentication Context Class Reference (Kontextklassenreferenz für die Anwendungsauthentifizierung) | Die Art der Clientauthentifizierung (öffentlicher oder vertraulicher Client). |
| Authentifizierungszeitpunkt | Datum und Uhrzeit der Authentifizierung. |
| Authentifizierungsmethode | Die Authentifizierungsmethode für den Antragsteller des Tokens (Kennwort, Zertifikat oder Ähnliches). |
| First Name (Vorname) | Der Vorname des Benutzers gemäß Angabe in Azure AD. |
| Gruppen | Objekt-IDs von Azure AD-Gruppen, denen der Benutzer angehört. |
| Identitätsanbieter | Der Identitätsanbieter, der den Antragsteller des Tokens authentifiziert hat. |
| Ausgestellt um | Die Zeit, zu der das Token ausgestellt wurde (wird häufig für die Tokenaktualität verwendet). |
| Issuer (Aussteller) | Der STS, der das Token ausgestellt hat, sowie der Azure AD-Mandant. |
| Last Name (Nachname) | Der Nachname des Benutzers gemäß Angabe in Azure AD. |
| Name | Ein lesbarer Wert, der Aufschluss über den Antragsteller des Tokens gibt. |
| ObjectID | Ein unveränderlicher, eindeutiger Bezeichner des Antragstellers in Azure AD. |
| Rollen | Anzeigenamen von Azure AD-Anwendungsrollen, die dem Benutzer erteilt wurden. |
| `Scope` | Die Berechtigungen, die der Clientanwendung gewährt wurden. |
| Subject | Der Prinzipal, für den das Token Informationen bestätigt. |
| Mandanten-ID | Ein unveränderlicher, eindeutiger Bezeichner des Verzeichnismandanten, der das Token ausgestellt hat. |
| Tokengültigkeitsdauer | Das Zeitintervall, für das ein Token gültig ist. |
| Benutzerprinzipalname | Der Benutzerprinzipalname des Antragstellers. |
| Version | Die Versionsnummer des Tokens. |

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die [in Microsoft Identity Platform unterstützten Anwendungstypen und Szenarien](app-types.md).
