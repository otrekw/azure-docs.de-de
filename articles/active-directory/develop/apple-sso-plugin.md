---
title: Microsoft Enterprise SSO-Plug-In für Apple-Geräte
titleSuffix: Microsoft identity platform | Azure
description: Erfahren Sie mehr über das Azure Active Directory-SSO-Plug-In für iOS-, iPadOS- und macOS-Geräte.
services: active-directory
author: brandwe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/15/2020
ms.author: brandwe
ms.reviewer: brandwe
ms.custom: aaddev
ms.openlocfilehash: eb9a6e1f3044492b09dac3fb3168a9bd26aeff0f
ms.sourcegitcommit: bb9a6c6e9e07e6011bb6c386003573db5c1a4810
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110494610"
---
# <a name="microsoft-enterprise-sso-plug-in-for-apple-devices-preview"></a>Microsoft Enterprise SSO-Plug-In für Apple-Geräte (Vorschau)

>[!IMPORTANT]
> Dieses Feature [!INCLUDE [PREVIEW BOILERPLATE](../../../includes/active-directory-develop-preview.md)]

Das *Microsoft Enterprise SSO-Plug-In für Apple-Geräte* ermöglicht einmaliges Anmelden (Single Sign-On, SSO) für Azure Active Directory-Konten (Azure AD) auf macOS-, iOS- und iPadOS-Geräten und für alle Anwendungen, die das Feature [Enterprise Single Sign-On](https://developer.apple.com/documentation/authenticationservices) von Apple unterstützen. Das Plug-In ermöglicht einmaliges Anmelden (SSO) auch für ältere Anwendungen, auf die Ihr Unternehmen möglicherweise angewiesen ist, die aber noch nicht die neuesten Identitätsbibliotheken oder -protokolle unterstützen. Microsoft hat bei der Entwicklung dieses Plug-Ins eng mit Apple zusammengearbeitet, um die Nutzbarkeit Ihrer Anwendung zu erhöhen, während gleichzeitig bestmöglicher Schutz gewährt wird.

Das Enterprise SSO-Plug-In ist derzeit ein in die folgenden Apps integriertes Feature:

* [Microsoft Authenticator](../user-help/user-help-auth-app-overview.md): iOS, iPadOS
* Microsoft Intune-[Unternehmensportal](/mem/intune/apps/apps-company-portal-macos): macOS

## <a name="features"></a>Features

Mit dem Microsoft Enterprise SSO-Plug-In für Apple-Geräte ergeben sich für Sie die folgenden Vorteile:

- SSO für Azure AD-Konten und alle Anwendungen, die das Feature „Enterprise SSO“ von Apple unterstützen
- Kann für jede MDM-Lösung (Mobile Device Management, Verwaltung mobiler Geräte) aktiviert werden
- Weitet einmaliges Anmelden (SSO) auf Anwendungen aus, die noch keine Microsoft Identity Platform-Bibliotheken nutzen
- Weitet einmaliges Anmelden (SSO) aus Anwendungen aus, die OAuth2, OpenID Connect und SAML nutzen

## <a name="requirements"></a>Anforderungen

Für das Microsoft Enterprise SSO-Plug-In für Apple-Geräte gelten folgende Anforderungen:

- Das Gerät muss Apps *unterstützen*, für die das Microsoft Enterprise SSO-Plug-In für Apple-Geräte installiert ist, und auf dem Gerät muss eine solche App verfügbar sein:
  - Ab iOS 13.0: [Microsoft Authenticator-App](../user-help/user-help-auth-app-overview.md)
  - Ab iPadOS 13.0: [Microsoft Authenticator-App](../user-help/user-help-auth-app-overview.md)
  - Ab macOS 10.15: [Intune-Unternehmensportal-App](/mem/intune/user-help/enroll-your-device-in-intune-macos-cp)
- Das Gerät muss *in MDM registriert* sein, z. B. über Microsoft Intune.
- Die Konfiguration muss *per Push auf das Gerät übertragen* werden, um das Enterprise SSO-Plug-In zu aktivieren. Diese Sicherheitseinschränkung wird von Apple verlangt.

### <a name="ios-requirements"></a>iOS-Anforderungen:
- iOS 13.0 oder höher muss auf dem Gerät installiert sein.
- Eine Microsoft-Anwendung, über die das Microsoft Enterprise SSO-Plug-In für Apple-Geräte bereitgestellt wird, muss auf dem Gerät installiert sein. Während der öffentlichen Vorschauphase ist dies die [Microsoft Authenticator-App](../user-help/user-help-auth-app-overview.md).


### <a name="macos-requirements"></a>macOS-Anforderungen:
- macOS 10.15 oder höher muss auf dem Gerät installiert sein. 
- Eine Microsoft-Anwendung, über die das Microsoft Enterprise SSO-Plug-In für Apple-Geräte bereitgestellt wird, muss auf dem Gerät installiert sein. Während der öffentlichen Vorschauphase ist dies die [Intune-Unternehmensportal-App](/mem/intune/user-help/enroll-your-device-in-intune-macos-cp).

## <a name="enable-the-sso-plug-in"></a>Aktivieren des SSO-Plug-Ins

Aktivieren Sie das SSO-Plug-In mithilfe von MDM und folgender Informationen.
### <a name="microsoft-intune-configuration"></a>Microsoft Intune-Konfiguration

Wenn Sie Microsoft Intune als MDM-Dienst verwenden, können Sie das Microsoft Enterprise SSO-Plug-In mithilfe der integrierten Konfigurationsprofileinstellungen aktivieren:

1. Konfigurieren Sie die Einstellungen der [SSO-App-Erweiterung](/mem/intune/configuration/device-features-configure#single-sign-on-app-extension) eines Konfigurationsprofils. 
1. Wenn das Profil nicht bereits zugewiesen ist, [weisen Sie das Profil einer Benutzer- oder Gerätegruppe zu](/mem/intune/configuration/device-profile-assign).

Die Profileinstellungen, mit denen das SSO-Plug-In aktiviert wird, werden automatisch auf die Geräte der Gruppe angewendet, wenn sich die einzelnen Geräte das nächste Mal bei Microsoft Intune anmelden.

### <a name="manual-configuration-for-other-mdm-services"></a>Manuelle Konfiguration für andere MDM-Dienste

Wenn Sie nicht mit Intune für MDM arbeiten, können Sie eine erweiterbare Nutzlast für SSO-Profile für Apple-Geräte konfigurieren. Verwenden Sie die folgenden Parameter, um das Microsoft Enterprise SSO-Plug-In und dessen Konfigurationsoptionen zu konfigurieren.

iOS-Einstellungen:

- **Erweiterungs-ID**: `com.microsoft.azureauthenticator.ssoextension`
- **Team-ID**: Dieses Feld wird für iOS nicht benötigt.

macOS-Einstellungen:

- **Erweiterungs-ID**: `com.microsoft.CompanyPortalMac.ssoextension`
- **Team-ID**: `UBF8T346G9`

Allgemeine Einstellungen:

- **Typ:** Redirect
  - `https://login.microsoftonline.com`
  - `https://login.microsoft.com`
  - `https://sts.windows.net`
  - `https://login.partner.microsoftonline.cn`
  - `https://login.chinacloudapi.cn`
  - `https://login.microsoftonline.de`
  - `https://login.microsoftonline.us`
  - `https://login.usgovcloudapi.net`
  - `https://login-us.microsoftonline.com`
  
### <a name="more-configuration-options"></a>Weitere Konfigurationsoptionen
Sie können weitere Konfigurationsoptionen hinzufügen, um SSO-Funktionalität auf andere Apps auszuweiten.

#### <a name="enable-sso-for-apps-that-dont-use-a-microsoft-identity-platform-library"></a>Aktivieren des einmaligen Anmeldens (SSO) für Apps, die keine Microsoft Identity Platform-Bibliothek verwenden

Dank des SSO-Plug-Ins können alle Anwendungen am einmaligen Anmelden (SSO) teilnehmen, auch wenn sie nicht mit einem Microsoft SDK wie Microsoft Authentication Library (MSAL) entwickelt wurden.

Das SSO-Plug-In wird automatisch auf Geräten installiert, die folgende Voraussetzungen erfüllen:
* Die Authenticator-App wurde für iOS oder iPadOS oder die Intune-Unternehmensportal-App für macOS heruntergeladen.
* Das Gerät wurde in Ihrer Organisation registriert. 

Ihre Organisation verwendet die Authenticator-App wahrscheinlich für Szenarien wie mehrstufige Authentifizierung (MFA), kennwortlose Authentifizierung und bedingten Zugriff. Mithilfe eines MDM-Anbieters können Sie das SSO-Plug-In für Ihre Anwendungen aktivieren. Microsoft hat das Konfigurieren des Plug-Ins innerhalb von Microsoft Endpoint Manager in Intune vereinfacht. Eine Positivliste dient zum Konfigurieren dieser Anwendungen für das SSO-Plug-In.

>[!IMPORTANT]
> Das Microsoft Enterprise SSO-Plug-In unterstützt nur Anwendungen, die native Apple-Netzwerktechnologien oder Webansichten nutzen. Es unterstützt keine Anwendungen mit eigener Implementierung der Netzwerkschicht.  

Konfigurieren Sie mithilfe der folgenden Parameter das Microsoft Enterprise SSO-Plug-In für Apps, die keine Microsoft Identity Platform-Bibliothek verwenden.

Stellen Sie eine Liste bestimmter Anwendungen mithilfe dieser Parameter bereit:

- **Schlüssel**: `AppAllowList`
- **Typ:** `String`
- **Wert**: Durch Komma getrennte Liste mit den Bundle-IDs der Anwendungen, die für einmaliges Anmelden (SSO) zugelassen sind.
- **Beispiel**: `com.contoso.workapp, com.contoso.travelapp`

Stellen Sie mithilfe dieser Parameter eine Liste von Präfixen bereit:
- **Schlüssel**: `AppPrefixAllowList`
- **Typ:** `String`
- **Wert**: Durch Komma getrennte Liste mit den Bundle-ID-Präfixen der Anwendungen, die für einmaliges Anmelden (SSO) zugelassen sind. Dieser Parameter ermöglicht allen Anwendungen, die mit einem bestimmten Präfix beginnen, einmaliges Anmelden.
- **Beispiel**: `com.contoso., com.fabrikam.`

[Mit Einwilligung versehene Apps](./application-consent-experience.md), denen der MDM-Administrator einmaliges Anmelden erlaubt hat, können automatisch ein Token für den Endbenutzer erhalten. Fügen Sie der Positivliste daher nur vertrauenswürdige Anwendungen hinzu. 

>[!NOTE]
> Sie müssen Anwendungen, die mit MSAL oder ASWebAuthenticationSession arbeiten, nicht zur Liste der Anwendungen hinzufügen, für die einmaliges Anmelden aktiviert ist. Diese Anwendungen sind standardmäßig aktiviert. 

##### <a name="find-app-bundle-identifiers-on-ios-devices"></a>Ermitteln der App Bundle-IDs auf iOS-Geräten

Apple bietet keine einfache Möglichkeit, Bundle-IDs aus dem App Store abzurufen. Die einfachste Möglichkeit zur Ermittlung der Bundle-IDs der Apps, für die Sie einmaliges Anmelden aktivieren möchten, ist die Rückfrage beim Anbieter oder Entwickler der App. Wenn das nicht möglich ist, können Sie die Bundle-IDs in Ihrer MDM-Konfiguration ermitteln: 

1. Aktivieren Sie in Ihrer MDM-Konfiguration vorübergehend folgendes Flag:

    - **Schlüssel**: `admin_debug_mode_enabled`
    - **Typ:** `Integer`
    - **Value**: 1 oder 0
1. Wenn dieses Flag aktiviert ist, melden Sie sich bei iOS-Apps auf dem Gerät an, für das Sie die Bundle-ID benötigen. 
1. Wählen Sie in der Authenticator-App **Hilfe** > **Protokolle senden** > **Protokolle anzeigen** aus. 
1. Suchen Sie in der Protokolldatei folgende Zeile: `[ADMIN MODE] SSO extension has captured following app bundle identifiers`. Diese Zeile muss alle App Bundle-IDs erfassen, die für die SSO-Erweiterung sichtbar sind. 

Konfigurieren Sie einmaliges Anmelden für die Apps mithilfe der Bundle-IDs. 

#### <a name="allow-users-to-sign-in-from-unknown-applications-and-the-safari-browser"></a>Erlauben von Benutzeranmeldungen über unbekannte Anwendungen und den Safari-Browser

Standardmäßig stellt das Microsoft Enterprise SSO-Plug-In einmaliges Anmelden (SSO) für autorisierte Apps nur dann bereit, wenn sich ein Benutzer über eine App angemeldet hat, die eine Microsoft Identity Platform-Bibliothek wie MSAL oder ADAL (Azure Active Directory Authentication Library) verwendet. Das Microsoft Enterprise SSO-Plug-In kann auch freigegebene Anmeldeinformationen abrufen, wenn es beim Beziehen eines neuen Tokens von einer anderen App aufgerufen wird, die eine Microsoft Identity Platform-Bibliothek verwendet.

Wenn Sie das Flag `browser_sso_interaction_enabled` aktivieren, können Apps, die keine Microsoft Identity Platform-Bibliothek verwenden, das anfängliche Bootstrapping durchführen und freigegebene Anmeldeinformationen abrufen. Auch der Safari-Browser kann das anfängliche Bootstrapping ausführen und freigegebene Anmeldeinformationen abrufen. 

Wenn das Microsoft Enterprise SSO-Plug-In noch nicht über freigegebene Anmeldeinformationen verfügt, versucht es jedes Mal Anmeldeinformationen abzurufen, sobald von einer Azure AD-URL im Safari-Browser, von ASWebAuthenticationSession, von SafariViewController oder von einer anderen zugelassenen nativen Anwendung eine Anmeldung angefordert wird. 

Aktivieren Sie das Flag mit diesen Parametern: 

- **Schlüssel**: `browser_sso_interaction_enabled`
- **Typ:** `Integer`
- **Value**: 1 oder 0

macOS benötigt diese Einstellung, um in allen Apps eine einheitliche Umgebung zu bieten. iOS und iPadOS benötigen diese Einstellung nicht, da die meisten Apps die Authenticator-App für die Anmeldung nutzen. Wir empfehlen jedoch, diese Einstellung zu aktivieren, denn wenn einige Ihrer Anwendungen die Authenticator-App unter iOS oder iPadOS nicht nutzen, wird dieses Flag die Benutzererfahrung verbessern. Die Einstellung ist standardmäßig deaktiviert.

#### <a name="disable-asking-for-mfa-during-initial-bootstrapping"></a>Deaktivieren der Aufforderung der mehrstufigen Authentifizierung beim anfänglichen Bootstrapping

Standardmäßig fordert das Microsoft Enterprise SSO-Plug-In den Benutzer während des anfänglichen Bootstrappings und beim Abrufen freigegebener Anmeldeinformationen stets zur mehrstufigen Authentifizierung auf. Der Benutzer wird zur mehrstufigen Authentifizierung aufgefordert, auch wenn sie für die Anwendung, die der Benutzer geöffnet hat, nicht erforderlich ist. Durch dieses Verhalten können die freigegebenen Anmeldeinformationen problemlos in allen anderen Anwendungen verwendet werden, ohne dass der Benutzer aufgefordert werden muss, wenn die mehrstufige Authentifizierung später erforderlich ist. Da der Benutzer insgesamt weniger Aufforderungen erhält, empfiehlt sich diese Einstellung im Allgemeinen.

Wenn Sie `browser_sso_disable_mfa` aktivieren, wird die mehrstufige Authentifizierung während des anfänglichen Bootstrappings und beim Abrufen der freigegebenen Anmeldeinformationen deaktiviert. In diesem Fall wird der Benutzer nur aufgefordert, wenn die mehrstufige Authentifizierung von einer Anwendung oder Ressource verlangt wird. 

Aktivieren Sie das Flag mit diesen Parametern:

- **Schlüssel**: `browser_sso_disable_mfa`
- **Typ:** `Integer`
- **Value**: 1 oder 0

Es wird empfohlen, dieses Flag deaktiviert zu lassen, da es die Anzahl der Anmeldeaufforderungen für Benutzer reduziert. Wenn die mehrstufige Authentifizierung in Ihrer Organisation kaum eine Rolle spielt, können Sie das Flag aktivieren. Es wird jedoch empfohlen, der mehrstufigen Authentifizierung mehr Bedeutung einzuräumen. Aus diesem Grund ist das Flag standardmäßig deaktiviert.

#### <a name="disable-oauth-2-application-prompts"></a>Deaktivieren von Eingabeaufforderungen durch OAuth2-Anwendungen

Das Microsoft Enterprise SSO-Plug-In ermöglicht einmaliges Anmelden, indem es an Netzwerkanforderungen, die von zulässigen Anwendungen stammen, freigegebene Anmeldeinformationen anfügt. Einige OAuth2-Anwendungen können auf Protokollebene jedoch fälschlicherweise Endbenutzeraufforderungen erzwingen. Wenn dieses Problem auftritt, werden Sie auch feststellen, dass freigegebene Anmeldeinformationen für diese Apps ignoriert werden. Ihr Benutzer wird zur Anmeldung aufgefordert, obwohl das Microsoft Enterprise SSO-Plug-In für andere Anwendungen funktioniert.  

Durch Aktivieren des Flags `disable_explicit_app_prompt` wird die Fähigkeit sowohl nativer Anwendungen als auch von Webanwendungen eingeschränkt, eine Aufforderung des Endbenutzers auf Protokollebene zu erzwingen und SSO zu umgehen. Aktivieren Sie das Flag mit diesen Parametern:

- **Schlüssel**: `disable_explicit_app_prompt`
- **Typ:** `Integer`
- **Value**: 1 oder 0

Wir empfehlen, dieses Flag zu aktivieren, um in allen Apps eine einheitliche Benutzererfahrung zu erzielen. Standardmäßig ist es deaktiviert. 

#### <a name="enable-sso-through-cookies-for-a-specific-application"></a>Aktivieren von SSO über Cookies für eine bestimmte Anwendung

Einige wenige Apps sind möglicherweise nicht mit der SSO-Erweiterung kompatibel. Insbesondere bei Apps mit erweiterten Netzwerkeinstellungen können unerwartete Probleme auftreten, wenn sie für einmaliges Anmelden aktiviert werden. Es kann z. B. der Fehler gemeldet werden, dass die Netzwerkanforderung abgebrochen oder unterbrochen wurde. 

Wenn Sie Probleme haben, sich mit der im Abschnitt [Anwendungen, die MSAL nicht verwenden](#applications-that-dont-use-msal) beschriebenen Methode anzumelden, probieren Sie es mit einer anderen Konfiguration. Konfigurieren Sie das Plug-In mit diesen Parametern:

- **Schlüssel**: `AppCookieSSOAllowList`
- **Typ:** `String`
- **Value**: Kommagetrennte Liste mit den Paket-IDs (Präfixen) der Anwendungen, die für einmaliges Anmelden (SSO) zugelassen sind. Alle Apps, die mit den aufgeführten Präfixen beginnen, sind für einmaliges Anmelden aktiviert.
- **Beispiel**: `com.contoso.myapp1, com.fabrikam.myapp2`

Anwendungen, die mithilfe dieser Einrichtung für einmaliges Anmelden aktiviert werden, müssen sowohl `AppCookieSSOAllowList` als auch `AppPrefixAllowList` hinzugefügt werden.

Probieren Sie diese Konfiguration nur für Anwendungen aus, bei denen es zu unerwarteten Anmeldefehlern kommt. 

#### <a name="use-intune-for-simplified-configuration"></a>Verwenden von Intune zum Vereinfachen der Konfiguration

Sie können mit Intune als MDM-Dienst die Konfiguration des Microsoft Enterprise SSO-Plug-Ins vereinfachen. Sie können z. B. mithilfe von Intune das Plug-In aktivieren und alte Anwendungen einer Positivliste hinzufügen, damit einmaliges Anmelden für sie möglich ist. 

Weitere Informationen finden Sie in der [Dokumentation zur Intune-Konfiguration](/intune/configuration/ios-device-features-settings).

## <a name="use-the-sso-plug-in-in-your-application"></a>Verwenden des SSO-Plug-Ins in Ihrer Anwendung

[MSAL für Apple-Geräte](https://github.com/AzureAD/microsoft-authentication-library-for-objc) unterstützt ab Version 1.1.0 das Microsoft Enterprise SSO-Plug-In für Apple-Geräte. Dies ist die empfohlene Methode zum Hinzufügen von Unterstützung für das Microsoft Enterprise SSO-Plug-In. Sie stellt sicher, dass Sie den vollen Funktionsumfang der Microsoft Identity Platform nutzen können.

Wenn Sie eine Anwendung für Mitarbeiter mit Kundenkontakt entwickeln, finden Sie unter [Modus für gemeinsam genutzte Geräte für iOS-Geräte](msal-ios-shared-devices.md) weitere Informationen zur Einrichtung.

## <a name="understand-how-the-sso-plug-in-works"></a>Grundlegendes zur Funktionsweise des SSO-Plug-Ins

Das Microsoft Enterprise SSO-Plug-In basiert auf dem [Enterprise SSO-Framework von Apple](https://developer.apple.com/documentation/authenticationservices/asauthorizationsinglesignonprovider?language=objc). Am Framework teilnehmende Identitätsanbieter können Netzwerkdatenverkehr für ihre Domänen abfangen und die Verarbeitung dieser Anforderungen verbessern oder ändern. Das SSO-Plug-In kann z. B. zusätzliche Benutzeroberflächen anzeigen, um Anmeldeinformationen von Endbenutzern sicher zu erfassen, die mehrstufige Authentifizierung anzufordern oder automatisch Token für die Anwendung bereitzustellen.

Native Anwendungen können außerdem benutzerdefinierte Vorgänge implementieren und direkt mit dem SSO-Plug-In kommunizieren. Weitere Informationen finden Sie in diesem [Video von der Worldwide Developer Conference von Apple aus dem Jahr 2019](https://developer.apple.com/videos/play/tech-talks/301/).

### <a name="applications-that-use-msal"></a>Anwendungen, die MSAL verwenden

[MSAL für Apple-Geräte](https://github.com/AzureAD/microsoft-authentication-library-for-objc) unterstützt ab Version 1.1.0 das Microsoft Enterprise SSO-Plug-In für Apple-Geräte nativ für Geschäfts-, Schul- oder Unikonten. 

Es ist keine besondere Konfiguration erforderlich, wenn Sie [alle empfohlenen Schritte](./quickstart-v2-ios.md) befolgen und das standardmäßige [Umleitungs-URI-Format](./redirect-uris-ios.md) verwenden. Auf Geräten mit dem SSO-Plug-In ruft MSAL dieses automatisch für alle interaktiven und unbeaufsichtigten Anforderungen von Token auf. Es wird auch für die Kontoenumeration und Vorgänge zum Entfernen von Konten aufgerufen. Da MSAL ein natives SSO-Plug-In-Protokoll implementiert, das auf benutzerdefinierten Vorgängen basiert, bietet diese Einrichtung die komfortabelste native Endbenutzerumgebung. 

Wenn das SSO-Plug-In von MDM nicht aktiviert wird, aber die Microsoft Authenticator-App auf dem Gerät vorhanden ist, verwendet MSAL für interaktive Tokenanforderungen stattdessen die Microsoft Authenticator-App. Das SSO-Plug-In gibt SSO für die Authenticator-App frei.

### <a name="applications-that-dont-use-msal"></a>Anwendungen, die MSAL nicht verwenden

Anwendungen, die keine Microsoft Identity Platform-Bibliothek wie MSAL verwenden, können trotzdem einmaliges Anmelden nutzen, wenn sie von einem Administrator explizit zur Positivliste hinzugefügt werden. 

Sie müssen den Code in diesen Apps nicht ändern, solange die folgenden Bedingungen erfüllt sind:

- Die Anwendung verwendet für Netzwerkanforderungen Apple-Frameworks. Zu diesen Frameworks gehören z. B. [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) und [NSURLSession](https://developer.apple.com/documentation/foundation/nsurlsession). 
- Die Anwendung verwendet Standardprotokolle zur Kommunikation mit Azure AD. Zu diesen Protokollen gehören beispielsweise OAuth 2, SAML und WS-Verbund.
- Die Anwendung erfasst auf der nativen Benutzeroberfläche keine Benutzernamen und Kennwörter in Klartext.

In diesem Fall wird SSO bereitgestellt, wenn die Anwendung eine Netzwerkanforderung erstellt und einen Webbrowser öffnet, um den Benutzer anzumelden. Wenn ein Benutzer zu einer Azure AD-Anmelde-URL umgeleitet wird, überprüft das SSO-Plug-In die URL und ob für diese URL SSO-Anmeldeinformationen verfügbar sind. Falls ja, übergibt das SSO-Plug-In die Anmeldeinformationen an Azure AD. Azure AD autorisiert dann die Anwendung zum Erfüllen der Netzwerkanforderung, ohne den Benutzer zur Eingabe von Anmeldeinformationen aufzufordern. Wenn das Gerät Azure AD bekannt ist, übergibt das SSO-Plug-In außerdem das Gerätezertifikat, um die gerätebasierte Überprüfung auf bedingten Zugriff zu erfüllen. 

Zur Unterstützung des einmaligen Anmeldens für Apps ohne MSAL implementiert das SSO-Plug-In ein dem Windows-Browser-Plug-In vergleichbares Protokoll, das unter [Was ist ein primäres Aktualisierungstoken?](../devices/concept-primary-refresh-token.md#browser-sso-using-prt) beschrieben ist. 

Im Vergleich zu MSAL-basierten Apps verhält sich das SSO-Plug-in für nicht-MSAL-basierte Apps transparenter. Es integriert sich in die bestehende Browser-Anmeldeerfahrung, die Apps bieten. 

Der Benutzer sieht die gewohnte Oberfläche und muss sich nicht in jeder Anwendung neu anmelden. Anstatt die native Kontoauswahl anzuzeigen, fügt das SSO-Plug-In beispielsweise der webbasierten Kontoauswahlumgebung SSO-Sitzungen hinzu. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zum [Modus für gemeinsam genutzte Geräte für iOS-Geräte](msal-ios-shared-devices.md).
