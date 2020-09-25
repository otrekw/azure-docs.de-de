---
title: Einschränkungen für Umleitungs-URI/Antwort-URL | Azure
titleSuffix: Microsoft identity platform
description: Beschrieben werden die Einschränkungen, die für das Format des Umleitungs-URIs (Antwort-URL) gelten, das von der Microsoft Identity-Plattform erzwungen wird.
author: SureshJa
ms.author: sureshja
manager: CelesteDG
ms.date: 08/07/2020
ms.topic: conceptual
ms.subservice: develop
ms.custom: aaddev
ms.service: active-directory
ms.reviewer: lenalepa, manrath
ms.openlocfilehash: bd6f88db2b55a5f0f445659e4b5ef609d3e146e9
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90030309"
---
# <a name="redirect-uri-reply-url-restrictions-and-limitations"></a>Einschränkungen für Umleitungs-URI/Antwort-URL

Ein Umleitungs-URI oder eine Antwort-URL definiert den Ort, an den der Autorisierungsserver den Benutzer leitet, sobald die App erfolgreich autorisiert und ein Autorisierungscode oder Zugriffstoken erteilt wurde. Der Autorisierungsserver sendet den Code oder das Token an den Umleitungs-URI. Daher ist es wichtig, dass Sie beim App-Registrierungsvorgang den richtigen Ort registrieren.

 Für Umleitungs-URIs gelten die folgenden Einschränkungen:

* Der Umleitungs-URI muss mit dem Schema `https` beginnen.

* Beim Umleitungs-URI wird die Groß-/Kleinschreibung beachtet. Die Groß-/Kleinschreibung muss der Groß-/Kleinschreibung des URL-Pfads Ihrer ausgeführten Anwendung entsprechen. Wenn der Pfad für Ihre Anwendung z. B. `.../abc/response-oidc` als Bestandteil enthält, dürfen Sie im Umleitungs-URI nicht `.../ABC/response-oidc` angeben. Weil der Webbrowser bei Pfaden die Groß-/Kleinschreibung beachtet, werden Cookies, die `.../abc/response-oidc` zugeordnet sind, möglicherweise ausgeschlossen, wenn eine Umleitung an die anders geschriebene (nicht übereinstimmende) URL `.../ABC/response-oidc` erfolgt.

## <a name="maximum-number-of-redirect-uris"></a>Maximale Anzahl von Umleitungs-URIs

Die folgende Tabelle zeigt die maximale Anzahl von Umleitungs-URIs, die Sie einer App-Registrierung in Microsoft Identity Plattform hinzufügen können.

| Angemeldete Konten | Maximale Anzahl von Umleitungs-URIs | BESCHREIBUNG |
|--------------------------|---------------------------------|-------------|
| Geschäfts-, Schul- oder Unikonten von Microsoft in einem beliebigen Azure Active Directory (Azure AD)-Mandanten eines Unternehmens | 256 | Das Feld `signInAudience` im Anwendungsmanifest ist entweder auf *AzureADMyOrg* oder *AzureADMultipleOrgs* eingestellt |
| Persönliche Konten sowie Geschäfts-, Schul- und Unikonten von Microsoft | 100 | Das Feld `signInAudience` im Anwendungsmanifest ist auf *AzureADandPersonalMicrosoftAccount* eingestellt |

## <a name="maximum-uri-length"></a>Maximale URI-Länge

Für jeden Umleitungs-URI, den Sie einer App-Registrierung hinzufügen, können Sie maximal 256 Zeichen verwenden.

## <a name="supported-schemes"></a>Unterstützte Schemas

Das Azure Active Directory (Azure AD)-Anwendungsmodell unterstützt derzeit bei Apps, die Geschäfts-, Schul- oder Unikonten in einem Azure AD-Mandanten eines Unternehmens anmelden, sowohl das HTTP- als auch das HTTPS-Schema. Diese Kontotypen werden im Feld `signInAudience` des Anwendungsmanifests durch die Werte `AzureADMyOrg` und `AzureADMultipleOrgs` angegeben. Bei Apps, die persönliche Microsoft-Konten *und* Geschäfts-, Schul- oder Unikonten anmelden (d. h., `signInAudience` ist auf `AzureADandPersonalMicrosoftAccount` festgelegt), ist nur das HTTPS-Schema zulässig.

Um Umleitungs-URIs mit einem HTTP-Schema zu App-Registrierungen hinzuzufügen, die Geschäfts-, Schul- oder Unikonten anmelden, müssen Sie im Azure-Portal unter [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908) den Anwendungsmanifest-Editor verwenden. Auch wenn Sie mit dem Manifest-Editor einen HTTP-basierten Umleitungs-URI festlegen können, empfehlen wir *dringend*, für Ihre Umleitungs-URIs das HTTPS-Schema zu verwenden.

## <a name="localhost-exceptions"></a>Ausnahmen für Localhost

Gemäß [RFC 8252, Abschnitte 8.3](https://tools.ietf.org/html/rfc8252#section-8.3) und [7.3](https://tools.ietf.org/html/rfc8252#section-7.3), gelten für die Umleitungs-URIs „loopback“ und „localhost“ zwei Besonderheiten:

1. `http`-URI-Schemas sind akzeptabel, da die Umleitung das Gerät niemals verlässt. Daher ist beides akzeptabel:
    - `http://127.0.0.1/myApp`
    - `https://127.0.0.1/myApp`
1. Aufgrund kurzlebiger Portbereiche, die häufig von nativen Anwendungen benötigt werden, wird die Portkomponente (z. B. `:5001` oder `:443`) beim Abgleich eines Umleitungs-URI ignoriert. Folglich werden alle als gleichwertig betrachtet:
    - `http://127.0.0.1/MyApp`
    - `http://127.0.0.1:1234/MyApp`
    - `http://127.0.0.1:5000/MyApp`
    - `http://127.0.0.1:8080/MyApp`

Aus Entwicklersicht bedeutet dies Folgendes:

* Registrieren Sie nicht mehrere Umleitungs-URIs, wenn sich nur der Port unterscheidet. Der Anmeldeserver wählt willkürlich einen Umleitungs-URI aus und verwendet das diesem zugeordnete Verhalten (z. B. entsprechend dem Umleitungstyp `web`, `native` oder `spa`).
* Wenn Sie mehrere Umleitungs-URIs für Localhost registrieren möchten, um während der Entwicklung verschiedene Flows zu testen, unterscheiden Sie diese mithilfe der *path*-Komponente des URI. Beispielsweise stimmt `http://127.0.0.1/MyWebApp` nicht mit `http://127.0.0.1/MyNativeApp` überein.
* Gemäß RFC-Anleitungen sollten Sie `localhost` nicht in einem Umleitungs-URI verwenden. Verwenden Sie stattdessen die tatsächliche Loopback-IP-Adresse `127.0.0.1`. Dadurch wird verhindert, dass Ihre App durch falsch konfigurierte Firewalls oder umbenannte Netzwerkschnittstellen fehlerhaft wird.

    Um anstelle von „localhost“ das `http`-Schema mit der Loopback Adresse (127.0.0.1) zu verwenden, müssen Sie das [Anwendungsmanifest](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest#replyurls-attribute) bearbeiten. 

    Die IPv6-Loopback Adresse (`[::1]`) wird derzeit nicht unterstützt.

## <a name="restrictions-on-wildcards-in-redirect-uris"></a>Einschränkungen für Platzhalter in Umleitungs-URIs

Platzhalter in URIs wie `https://*.contoso.com` sind möglicherweise bequem, sind jedoch aufgrund von Sicherheitsrisiken zu vermeiden. Gemäß der OAuth 2.0-Spezifikation ([RFC 6749, Abschnitt 3.1.2](https://tools.ietf.org/html/rfc6749#section-3.1.2)) muss ein Umleitungsendpunkt-URI ein absoluter URI sein.

URIs mit Platzhalter werden derzeit in App-Registrierungen, die für die Anmeldung von persönlichen Microsoft-Konten und von Geschäfts-, Schul- oder Unikonten konfiguriert sind, nicht unterstützt. URIs mit Platzhalter sind jedoch zulässig bei Apps, die nur für die Anmeldung von Geschäfts-, Schul- oder Unikonten bei einem Azure AD-Mandanten in einer Organisation konfiguriert sind.

Um Umleitungs-URIs mit Platzhalter zu App-Registrierungen hinzuzufügen, die Geschäfts-, Schul- oder Unikonten anmelden, müssen Sie im Azure-Portal unter [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908) den Anwendungsmanifest-Editor verwenden. Auch wenn Sie mit dem Manifest-Editor einen Umleitungs-URI mit Platzhalter festlegen können, empfehlen wir *dringend*, [RFC 6749, Abschnitt 3.1.2](https://tools.ietf.org/html/rfc6749#section-3.1.2) zu beachten und ausschließlich absolute URIs zu verwenden.

Wenn die Anzahl der in Ihrem Szenario erforderlichen Umleitungs-URIs den zulässigen Höchstwert überschreitet, sollten Sie anstelle eines Umleitungs-URIs mit Platzhalter den [folgenden Ansatz](#use-a-state-parameter) in Betracht ziehen.

### <a name="use-a-state-parameter"></a>Verwenden eines Statusparameters

Wenn Sie mehrere Unterdomänen haben und in Ihrem Szenario Benutzer nach erfolgreicher Authentifizierung wieder auf die Ausgangsseite umgeleitet werden müssen, kann die Verwendung eines Statusparameters hilfreich sein.

Dieser Ansatz ermöglicht Folgendes:

1. Erstellen eines „gemeinsamen“ Umleitungs-URIs pro Anwendung, um die vom Autorisierungsendpunkt empfangenen Sicherheitstoken zu verarbeiten.
1. Ihre Anwendung kann anwendungsspezifische Parameter (z. B. URL der Unterdomäne, aus welcher der Benutzer stammt, oder andere Parameter wie Brandinginformationen) im Statusparameter senden. Die Verwendung eines Statusparameters bietet Schutz vor der websiteübergreifenden Anforderungsfälschung (CSRF) gemäß [RFC 6749, Abschnitt 10.12](https://tools.ietf.org/html/rfc6749#section-10.12).
1. Die anwendungsspezifischen Parameter enthalten alle Informationen, die die Anwendung benötigt, um die richtige Umgebung für den Benutzer zu rendern (d.h., den entsprechenden Anwendungsstatus zu erstellen). Der Azure AD-Autorisierungsendpunkt entfernt HTML aus dem Statusparameter. Stellen Sie also sicher, dass Sie in diesem Parameter keinen HTML-Inhalt übergeben.
1. Wenn Azure AD eine Antwort an den „gemeinsamen“ Umleitungs-URI sendet, wird gleichzeitig der Statusparameter an die Anwendung zurückgesendet.
1. Die Anwendung kann dann anhand des Werts im Statusparameter bestimmen, an welche URL der Benutzer weitergeleitet werden soll. Stellen Sie eine Überprüfung in Bezug auf CSRF-Schutz sicher.

> [!WARNING]
> Dieser Ansatz ermöglicht einem kompromittierten Client, die im Statusparameter gesendeten zusätzlichen Parameter zu ändern, wodurch der Benutzer zu einer anderen URL umgeleitet wird. Dieser [Open Redirect](https://tools.ietf.org/html/rfc6819#section-4.2.4)-Angriff (Sicherheitsrisiko durch offene Umleitung) ist in RFC 6819 beschrieben. Daher muss der Client diese Parameter schützen, indem er den Status verschlüsselt oder auf andere Weise verifiziert, z. B. durch Überprüfen des Domänennamens im Umleitungs-URI anhand des Tokens.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das [Anwendungsmanifest](reference-app-manifest.md) der App-Registrierung.
