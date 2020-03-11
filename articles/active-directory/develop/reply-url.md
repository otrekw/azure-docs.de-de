---
title: Einschränkungen für Umleitungs-URI und Antwort-URL – Microsoft Identity Platform | Azure
description: 'Antwort-URLs/Umleitungs-URls: Einschränkungen'
author: SureshJa
ms.author: sureshja
manager: CelesteDG
ms.date: 06/29/2019
ms.topic: conceptual
ms.subservice: develop
ms.custom: aaddev
ms.service: active-directory
ms.reviewer: lenalepa, manrath
ms.openlocfilehash: 8fdc64632be8b5fcb3dca8de2ee833fef25719fe
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77656737"
---
# <a name="redirect-urireply-url-restrictions-and-limitations"></a>Umleitungs-URI/Antwort-URL: Einschränkungen

Ein Umleitungs-URI oder eine Antwort-URL definiert den Ort, an den der Autorisierungsserver den Benutzer leitet, sobald die App erfolgreich autorisiert und ein Autorisierungscode oder Zugriffstoken erteilt wurde. Der Code oder das Token ist im Umleitungs-URI oder Antworttoken enthalten. Daher ist es wichtig, dass Sie beim App-Registrierungsvorgang den richtigen Ort registrieren.

 Für Antwort-URLs gelten die folgenden Einschränkungen:

    * Die Antwort-URL muss mit dem Schema `https` beginnen.
    * Bei der Antwort-URL muss die Groß-/Kleinschreibung beachtet werden. Die Groß-/Kleinschreibung muss der Groß-/Kleinschreibung des URL-Pfads Ihrer ausgeführten Anwendung entsprechen. Wenn Ihre Anwendung z. B. als Teil des Pfads `.../abc/response-oidc` enthält, geben Sie in der Antwort-URL nicht `.../ABC/response-oidc` an. Weil der Webbrowser bei Pfaden die Groß-/Kleinschreibung beachtet, werden Cookies, die `.../abc/response-oidc` zugeordnet sind, möglicherweise ausgeschlossen, wenn eine Umleitung an die anders geschriebene (nicht übereinstimmende) URL `.../ABC/response-oidc` erfolgt.
    
## <a name="maximum-number-of-redirect-uris"></a>Maximale Anzahl von Umleitungs-URIs

Die folgende Tabelle zeigt die maximale Anzahl von Umleitungs-URIs, die Sie beim Registrieren Ihrer App hinzufügen können.

| Angemeldete Konten | Maximale Anzahl von Umleitungs-URIs | BESCHREIBUNG |
|--------------------------|---------------------------------|-------------|
| Geschäfts-, Schul- oder Unikonten von Microsoft in einem beliebigen Azure Active Directory (Azure AD)-Mandanten eines Unternehmens | 256 | Das Feld `signInAudience` im Anwendungsmanifest ist entweder auf *AzureADMyOrg* oder *AzureADMultipleOrgs* eingestellt |
| Persönliche Konten sowie Geschäfts-, Schul- und Unikonten von Microsoft | 100 | Das Feld `signInAudience` im Anwendungsmanifest ist auf *AzureADandPersonalMicrosoftAccount* eingestellt |

## <a name="maximum-uri-length"></a>Maximale URI-Länge

Sie können maximal 256 Zeichen für jeden Umleitungs-URI verwenden, den Sie einer App-Registrierung hinzufügen.

## <a name="supported-schemes"></a>Unterstützte Schemas
Das Azure AD-Anwendungsmodell unterstützt derzeit sowohl das HTTP- als auch das HTTPS-Schema für Apps, die Geschäfts-, Schul- oder Unikonten von Microsoft in einem beliebigen Azure Active Directory-Mandanten (Azure AD) des Unternehmens anmelden. Das Feld `signInAudience` im Anwendungsmanifest ist entweder auf *AzureADMyOrg* oder *AzureADMultipleOrgs* eingestellt. Bei Apps, die persönliche Konten sowie Geschäfts-, Schul- oder Unikonten von Microsoft anmelden (`signInAudience` ist auf *AzureADandPersonalMicrosoftAccount* eingestellt), ist nur das HTTPS-Schema zulässig.

> [!NOTE]
> In der neuen Umgebung [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908) können Entwickler auf der Benutzeroberfläche keine URIs mit HTTP-Schema hinzufügen. Das Hinzufügen von HTTP-URIs wird bei Apps, die Geschäfts-, Schul- oder Unikonten anmelden, nur über den App-Manifest-Editor unterstützt. In Zukunft werden bei neuen Apps keine HTTP-Schemas im Umleitungs-URI mehr verwendet werden können. Ältere Anwendungen, die HTTP-Schemas in Umleitungs-URIs enthalten, funktionieren jedoch weiterhin. Entwickler müssen HTTPS-Schemas in Umleitungs-URIs verwenden.

## <a name="restrictions-using-a-wildcard-in-uris"></a>Einschränkungen bei Verwendung eines Platzhalters in URIs

Platzhalter-URIs (z.B. `https://*.contoso.com`) sind praktisch, sollten aber vermieden werden. Die Verwendung von Platzhaltern im Umleitungs-URI hat Auswirkungen auf die Sicherheit. Gemäß der OAuth 2.0-Spezifikation ([RFC 6749, Abschnitt 3.1.2](https://tools.ietf.org/html/rfc6749#section-3.1.2)) muss ein Umleitungsendpunkt-URI ein absoluter URI sein. 

Das Azure AD-Anwendungsmodell unterstützt keine Platzhalter-URIs für Apps, die für die Anmeldung von persönlichen Konten sowie Geschäfts-, Schul- oder Unikonten von Microsoft konfiguriert sind. Platzhalter-URIs sind jedoch zulässig bei Apps, die aktuell für die Anmeldung von Geschäfts-, Schul- oder Unikonten bei einem Azure AD-Mandanten in einer Organisation konfiguriert sind. 
 
> [!NOTE]
> In der neuen Umgebung [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908) können Entwickler auf der Benutzeroberfläche keine Platzhalter-URIs hinzufügen. Das Hinzufügen von Platzhalter-URIs für Apps, die Geschäfts-, Schul- oder Unikonten anmelden, wird nur über den App-Manifest-Editor unterstützt. In Zukunft werden bei neuen Apps im Umleitungs-URI keine Platzhalter mehr verwendet werden können. Ältere Anwendungen, die Platzhalter in Umleitungs-URIs enthalten, funktionieren jedoch weiterhin.

Wenn die Anzahl der in Ihrem Szenario erforderlichen Umleitungs-URIs den zulässigen Höchstwert überschreitet, sollten Sie anstelle eines Umleitungs-URIs mit Platzhalter den folgenden Ansatz wählen.

### <a name="use-a-state-parameter"></a>Verwenden eines Statusparameters

Wenn Sie eine Reihe von Subdomänen haben und in Ihrem Szenario Benutzer nach erfolgreicher Authentifizierung wieder auf die Startseite umgeleitet werden müssen, kann die Verwendung eines Statusparameters hilfreich sein. 

Dieser Ansatz ermöglicht Folgendes:

1. Erstellen eines „gemeinsamen“ Umleitungs-URIs pro Anwendung, um die vom Autorisierungsendpunkt empfangenen Sicherheitstoken zu verarbeiten.
1. Ihre Anwendung kann anwendungsspezifische Parameter (z.B. URL der Subdomäne, aus welcher der Benutzer stammt, oder andere Informationen wie Brandinginformationen) im Statusparameter senden. Die Verwendung eines Statusparameters bietet Schutz vor der websiteübergreifenden Anforderungsfälschung (CSRF) gemäß [RFC 6749, Abschnitt 10.12](https://tools.ietf.org/html/rfc6749#section-10.12). 
1. Die anwendungsspezifischen Parameter enthalten alle Informationen, die die Anwendung benötigt, um die richtige Umgebung für den Benutzer zu rendern (d.h., den entsprechenden Anwendungsstatus zu erstellen). Der Azure AD-Autorisierungsendpunkt entfernt HTML aus dem Statusparameter. Stellen Sie also sicher, dass Sie in diesem Parameter keinen HTML-Inhalt übergeben.
1. Wenn Azure AD eine Antwort an den „gemeinsamen“ Umleitungs-URI sendet, wird gleichzeitig der Statusparameter an die Anwendung zurückgesendet.
1. Die Anwendung kann dann anhand des Werts im Statusparameter bestimmen, an welche URL der Benutzer weitergeleitet werden soll. Stellen Sie eine Überprüfung in Bezug auf CSRF-Schutz sicher.

> [!NOTE]
> Dieser Ansatz ermöglicht einem kompromittierten Client, die im Statusparameter gesendeten zusätzlichen Parameter zu ändern, wodurch der Benutzer zu einer anderen URL umgeleitet wird. Dieser [Open Redirect](https://tools.ietf.org/html/rfc6819#section-4.2.4)-Angriff (Sicherheitsrisiko durch offene Umleitung) ist in RFC 6819 beschrieben. Daher muss der Client diese Parameter schützen, indem er den Status verschlüsselt oder auf andere Weise verifiziert, z.B. durch Überprüfen des Domänennamens im Umleitungs-URI anhand des Tokens.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Anwendungsmanifest](reference-app-manifest.md)
