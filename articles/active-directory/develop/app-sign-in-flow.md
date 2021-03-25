---
title: App-Anmeldeflow mit Microsoft Identity Platform | Azure
titleSuffix: Microsoft identity platform
description: Lernen Sie den Anmeldeflow von Web-, Desktop- und mobilen Apps in Microsoft Identity Platform kennen.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/18/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: 1f9f330ab140fa66b5a66a112c47ca2a68ba56bf
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98755700"
---
# <a name="app-sign-in-flow-with-the-microsoft-identity-platform"></a>App-Anmeldeflow mit Microsoft Identity Platform | Azure

In diesem Thema wird der grundlegende Anmeldeflow für Web-, Desktop- und mobile Apps mit Microsoft Identity Platform erläutert. Informationen zu Anmeldeszenarien, die von Microsoft Identity Platform unterstützt werden, finden Sie unter [Authentifizierungsflows und App-Szenarien](authentication-flows-app-scenarios.md).

## <a name="web-app-sign-in-flow"></a>Anmeldeflow für Web-Apps

Wenn ein Benutzer im Browser zu einer Web-App navigiert, passiert Folgendes:

* Die Web-App ermittelt, ob der Benutzer bereits authentifiziert wurde.
* Falls der Benutzer nicht authentifiziert wurde, delegiert die Web-App den Vorgang an Azure AD, damit der Benutzer angemeldet wird. Diese Anmeldung ist mit der Richtlinie der Organisation konform. Dies kann bedeuten, dass der Benutzer zum Eingeben seiner Anmeldeinformationen aufgefordert wird, indem die [mehrstufige Authentifizierung](../authentication/concept-mfa-howitworks.md) (manchmal auch als zweistufige Authentifizierung oder 2FA bezeichnet) oder ein Verfahren ganz ohne Kennwort (z. B. Windows Hello) verwendet wird.
* Der Benutzer wird aufgefordert, seine Einwilligung für den Zugriff zu erteilen, den die Client-App benötigt. Aus diesem Grund müssen Client-Apps bei Azure AD registriert werden. Microsoft Identity Platform kann dann Token für den Zugriff bereitstellen, für den der Benutzer seine Einwilligung erteilt hat.

Nach der erfolgreichen Authentifizierung des Benutzers:

* Microsoft Identity Platform sendet ein Token an die Web-App.
* Es wird ein Cookie gespeichert, das der Domäne von Azure AD, in der die Identität des Benutzers im Cookiebehälter des Browsers enthalten ist, zugeordnet ist. Wenn eine App den Browser das nächste Mal zum Navigieren zum Autorisierungsendpunkt von Microsoft Identity Platform verwendet, stellt der Browser das Cookie bereit, sodass sich der Benutzer nicht erneut anmelden muss. Auf diese Weise wird auch einmaliges Anmelden (SSO) ermöglicht. Das Cookie wird von Azure AD erstellt und ist nur für Azure AD verwendbar.
* Anschließend wird das Token von der Web-App überprüft. Wenn die Überprüfung erfolgreich ist, zeigt die Web-App die geschützte Seite an und speichert im Cookiebehälter des Browsers ein Sitzungscookie. Wenn der Benutzer zu einer anderen Seite navigiert, verfügt die Web-App aufgrund des Sitzungscookies über die Information, dass der Benutzer authentifiziert wurde.

Das folgende Sequenzdiagramm enthält eine Zusammenfassung dieser Interaktion:

![Authentifizierungsprozess für Web-Apps](media/authentication-scenarios/web-app-how-it-appears-to-be.png)

### <a name="how-a-web-app-determines-if-the-user-is-authenticated"></a>Ermittlung der Authentifizierung des Benutzers durch die Web-App

Web-App-Entwickler können angeben, ob für alle oder nur für bestimmte Seiten eine Authentifizierung erforderlich ist. In ASP.NET/ASP.NET Core wird hierfür beispielsweise das Attribut `[Authorize]` den Controlleraktionen hinzugefügt.

Dieses Attribut bewirkt, dass für ASP.NET überprüft wird, ob ein Sitzungscookie mit der Identität des Benutzers vorhanden ist. Falls kein Cookie vorhanden ist, leitet ASP.NET die Authentifizierung an den angegebenen Identitätsanbieter um. Wenn Azure AD als Identitätsanbieter verwendet wird, leitet die Web-App die Authentifizierung an `https://login.microsoftonline.com` um, und es wird ein Dialogfeld für die Anmeldung angezeigt.

### <a name="how-a-web-app-delegates-sign-in-to-the-microsoft-identity-platform-and-obtains-a-token"></a>Delegieren der Anmeldung an Microsoft Identity Platform und Abrufen eines Tokens durch die Web-App

Die Benutzerauthentifizierung erfolgt über den Browser. Für das OpenID-Protokoll werden die üblichen HTTP-Protokollnachrichten verwendet.

* Die Web-App sendet „HTTP 302 (Umleitung)“ an den Browser, um Microsoft Identity Platform verwenden zu können.
* Nachdem der Benutzer authentifiziert wurde, sendet Microsoft Identity Platform mithilfe einer Umleitung über den Browser das Token an die Web-App.
* Die Umleitung durch die Web-App erfolgt in Form eines Umleitungs-URIs. Dieser Umleitungs-URI wird unter dem Azure AD-Anwendungsobjekt registriert. Es kann mehrere Umleitungs-URIs geben, weil die Anwendung ggf. unter mehreren URLs bereitgestellt wird. Daher muss die Web-App auch den zu verwendenden Umleitungs-URI angeben.
* Azure AD überprüft, ob der von der Web-App gesendete Umleitungs-URI einer der registrierten URIs für die App ist.

## <a name="desktop-and-mobile-app-sign-in-flow"></a>Anmeldeflow für Desktop- und mobile Apps

Der oben beschriebene Ablauf gilt mit geringfügigen Abweichungen auch für Desktop- und mobile Anwendungen.

Für Desktop- und mobile Anwendungen kann für die Authentifizierung ein eingebettetes Websteuerelement oder ein Systembrowser verwendet werden. Im folgenden Diagramm ist dargestellt, wie eine Desktop- oder mobile App die Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) verwendet, um Zugriffstoken abzurufen und Web-APIs aufzurufen.

![Darstellung einer Desktop-App](media/authentication-scenarios/desktop-app-how-it-appears-to-be.png)

MSAL verwendet einen Browser, um Token abzurufen. Wie bei Web-Apps wird die Authentifizierung an Microsoft Identity Platform delegiert.

Da von Azure AD das gleiche Identitätscookie wie für Web-Apps im Browser gespeichert wird, gilt Folgendes: Wenn die native oder mobile App den Systembrowser verwendet, wird unmittelbar SSO mit der entsprechenden Web-App ermöglicht.

Standardmäßig wird für die MSAL der Systembrowser genutzt. Eine Ausnahme stellen .NET Framework-Desktopanwendungen dar, bei denen ein eingebettetes Steuerelement verwendet wird, um eine stärker integrierte Benutzeroberfläche bereitzustellen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Themen zu den Grundlagen von Authentifizierung und Autorisierung:

* Unter [Authentifizierung im Vergleich zu Autorisierung](authentication-vs-authorization.md) lernen Sie die grundlegenden Konzepte von Authentifizierung und Autorisierung in Microsoft Identity Platform kennen.
* Unter [Sicherheitstoken](security-tokens.md) erfahren Sie, wie Zugriffstoken, Aktualisierungstoken und ID-Token bei Authentifizierung und Autorisierung verwendet werden.
* Unter [Anwendungsmodell](application-model.md) erhalten Sie Informationen zum Registrierungsvorgang für Ihre Anwendung, damit diese in Microsoft Identity Platform integriert werden kann.

Weitere Informationen zum App-Anmeldeflow:

* In [Authentifizierungsflows und App-Szenarien](authentication-flows-app-scenarios.md) finden Sie weitere Informationen zu anderen Szenarien für die Authentifizierung von Benutzern, die von Microsoft Identity Platform unterstützt werden.
* Weitere Informationen zu den Microsoft-Bibliotheken, mit denen Sie für Microsoft-Konten, Azure AD-Konten und Azure AD B2C-Benutzer geeignete Anwendungen entwickeln können – alles mit einem zentralen, optimierten Programmiermodell –, finden Sie unter [MSAL-Bibliotheken](msal-overview.md).
