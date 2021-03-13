---
title: Microsoft Enterprise SSO-Plug-In für Apple-Geräte
titleSuffix: Microsoft identity platform | Azure
description: Enthält eine Beschreibung des Azure Active Directory-SSO-Plug-Ins von Microsoft für iOS-, iPadOS- und macOS-Geräte.
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
ms.openlocfilehash: 96fbf23128896f23beee70a6b3d32b4b87a454ea
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102427095"
---
# <a name="microsoft-enterprise-sso-plug-in-for-apple-devices-preview"></a>Microsoft Enterprise SSO-Plug-In für Apple-Geräte (Vorschau)

>[!IMPORTANT]
> Dieses Feature [!INCLUDE [PREVIEW BOILERPLATE](../../../includes/active-directory-develop-preview.md)]

Das *Microsoft Enterprise SSO-Plug-In für Apple-Geräte* ermöglicht einmaliges Anmelden (Single Sign-On, SSO) für Azure Active Directory (Azure AD)-Konten auf macOS-, iOS- und iPadOS-Geräten und für alle Anwendungen, die das Feature [Enterprise Single Sign-On](https://developer.apple.com/documentation/authenticationservices) von Apple unterstützen. Dazu zählen auch ältere Anwendungen, auf die sich Ihr Unternehmen möglicherweise verlässt und die noch nicht die neuesten Identitätsbibliotheken oder Protokolle unterstützen. Microsoft hat bei der Entwicklung dieses Plug-Ins eng mit Apple zusammengearbeitet, um die Nutzbarkeit Ihrer Anwendung zu erhöhen, während gleichzeitig der bestmögliche Schutz durch Apple und Microsoft gewährt wird.

Das Enterprise SSO-Plug-In ist derzeit als integriertes Feature der folgenden Apps verfügbar:

* [Microsoft Authenticator](../user-help/user-help-auth-app-overview.md): iOS, iPadOS
* Microsoft Intune-[Unternehmensportal](/mem/intune/apps/apps-company-portal-macos): macOS

## <a name="features"></a>Features

Mit dem Microsoft Enterprise SSO-Plug-In für Apple-Geräte ergeben sich für Sie die folgenden Vorteile:

- SSO für Azure AD-Konten über alle Anwendungen hinweg, die das Feature „Enterprise Single Sign-On“ von Apple unterstützen.
- Kann für jede MDM-Lösung (Mobile Device Management, Verwaltung mobiler Geräte) aktiviert werden.
- Weitet einmaliges Anmelden (SSO) auf Anwendungen aus, die noch keine Microsoft Identity Platform-Bibliotheken verwenden.
- Weitet einmaliges Anmelden (SSO) aus Anwendungen aus, die OAuth2, OpenID Connect und SAML verwenden.

## <a name="requirements"></a>Anforderungen

Für die Nutzung des Microsoft Enterprise SSO-Plug-Ins für Apple-Geräte gelten die folgenden Anforderungen:

- Das Gerät muss Apps **unterstützen**, für die das Microsoft Enterprise SSO-Plug-In für Apple-Geräte **installiert** ist, und auf dem Gerät muss eine solche App verfügbar sein:
  - iOS 13.0 und höher: [Microsoft Authenticator-App](../user-help/user-help-auth-app-overview.md)
  - iPadOS 13.0 und höher: [Microsoft Authenticator-App](../user-help/user-help-auth-app-overview.md)
  - macOS 10.15 und höher: [Intune-Unternehmensportal-App](/mem/intune/user-help/enroll-your-device-in-intune-macos-cp)
- Das Gerät muss für die **Verwaltung mobiler Geräte (MDM) registriert sein** (z. B. mit Microsoft Intune).
- Die Konfiguration muss **per Pushvorgang auf das Gerät übertragen** werden, um das Enterprise SSO-Plug-In auf dem Gerät zu aktivieren. Diese Sicherheitseinschränkung wird von Apple gefordert.

### <a name="ios-requirements"></a>iOS-Anforderungen:
- iOS 13.0 oder höher muss auf dem Gerät installiert sein.
- Eine Microsoft-Anwendung, über die das Microsoft Enterprise SSO-Plug-In für Apple-Geräte bereitgestellt wird, muss auf dem Gerät installiert sein. Während der öffentlichen Vorschauphase ist dies die [Microsoft Authenticator-App](/intune/user-help/user-help-auth-app-overview.md).


### <a name="macos-requirements"></a>macOS-Anforderungen:
- macOS 10.15 oder höher muss auf dem Gerät installiert sein. 
- Eine Microsoft-Anwendung, über die das Microsoft Enterprise SSO-Plug-In für Apple-Geräte bereitgestellt wird, muss auf dem Gerät installiert sein. Während der öffentlichen Vorschauphase ist dies die [Intune-Unternehmensportal-App](/intune/user-help/enroll-your-device-in-intune-macos-cp.md).

## <a name="enable-the-sso-plug-in-with-mobile-device-management-mdm"></a>Aktivieren des SSO-Plug-Ins mit der mobilen Geräteverwaltung (Mobile Device Management, MDM)

### <a name="microsoft-intune-configuration"></a>Microsoft Intune-Konfiguration

Wenn Sie Microsoft Intune als MDM-Dienst verwenden, können Sie die integrierten Konfigurationsprofileinstellungen verwenden, um das Microsoft Enterprise SSO-Plug-In zu aktivieren.

Konfigurieren Sie zunächst im Konfigurationsprofil die Einstellungen für die [SSO-App-Erweiterung](/mem/intune/configuration/device-features-configure#single-sign-on-app-extension), und [weisen Sie das Profil einem Benutzer oder einer Gerätegruppe zu](/mem/intune/configuration/device-profile-assign) (falls es nicht bereits zugewiesen ist).

Die Profileinstellungen, mit denen das SSO-Plug-In aktiviert wird, werden automatisch auf die Geräte der Gruppe angewendet, wenn sich die einzelnen Geräte das nächste Mal bei Microsoft Intune anmelden.

### <a name="manual-configuration-for-other-mdm-services"></a>Manuelle Konfiguration für andere MDM-Dienste

Wenn Sie Microsoft Intune nicht für die Verwaltung mobiler Geräte verwenden, konfigurieren Sie mithilfe der folgenden Parameter das Microsoft Enterprise SSO-Plug-In für Apple-Geräte.

#### <a name="ios-settings"></a>iOS-Einstellungen:

- **Erweiterungs-ID**: `com.microsoft.azureauthenticator.ssoextension`
- **Team-ID**: (dieses Feld wird für iOS nicht benötigt)

#### <a name="macos-settings"></a>macOS-Einstellungen:

- **Erweiterungs-ID**: `com.microsoft.CompanyPortalMac.ssoextension`
- **Team-ID**: `UBF8T346G9`

#### <a name="common-settings"></a>Allgemeine Einstellungen:

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
  
### <a name="additional-configuration-options"></a>Zusätzliche Konfigurationsoptionen
Zusätzliche Konfigurationsoptionen können hinzugefügt werden, um die SSO-Funktionalität auf weitere Apps auszuweiten.

#### <a name="enable-sso-for-apps-that-dont-use-a-microsoft-identity-platform-library"></a>Aktivieren des einmaligen Anmeldens (SSO) für Apps, die keine Microsoft Identity Platform-Bibliothek verwenden

Mit dem SSO-Plug-In können alle Anwendungen vom einmaligen Anmelden (SSO) partizipieren, auch wenn sie nicht mit einem Microsoft-SDK wie Microsoft Authentication Library (MSAL) entwickelt wurden.

Das SSO-Plug-In wird automatisch auf Geräten installiert, auf denen die Microsoft Authenticator-App (iOS und iPadOS) oder die Intune-Unternehmensportal-App (macOS) heruntergeladen wurde und die bei Ihrer Organisation registriert wurden. Ihre Organisation verwendet die Authenticator-App aktuell wahrscheinlich für Szenarien wie Multi-Factor Authentication, kennwortlose Authentifizierung und bedingten Zugriff. Sie kann mithilfe eines beliebigen MDM-Anbieters für Ihre Anwendungen aktiviert werden, auch wenn Microsoft das Konfigurieren in Microsoft Endpoint Manager von Intune vereinfacht hat. Zum Konfigurieren dieser Anwendungen für die Verwendung des SSO-Plug-Ins wird eine Zulassungsliste verwendet.

>[!IMPORTANT]
> Es werden nur Apps unterstützt, die native Apple-Netzwerktechnologien oder -Webansichten verwenden. Wenn eine Anwendung ihre eigene Implementierung der Netzwerkschicht beinhaltet, wird das Microsoft Enterprise SSO-Plug-In nicht unterstützt.  

Konfigurieren Sie mithilfe der folgenden Parameter das Microsoft Enterprise SSO-Plug-In für Apps, die keine Microsoft Identity Platform-Bibliothek verwenden:

Bereitstellen einer Liste mit bestimmten Apps:

- **Schlüssel**: `AppAllowList`
- **Typ:** `String`
- **Value**: Kommagetrennte Liste mit den Bundle-IDs der Anwendungen, die für das einmalige Anmelden (SSO) zugelassen sind.
- **Beispiel**: `com.contoso.workapp, com.contoso.travelapp`

Bereitstellen einer Präfixliste (alternativ):
- **Schlüssel**: `AppPrefixAllowList`
- **Typ:** `String`
- **Value**: Kommagetrennte Liste mit den Paket-IDs (Präfixen) der Anwendungen, die für einmaliges Anmelden (SSO) zugelassen sind. Beachten Sie, dass dadurch alle Apps, die mit einem bestimmten Präfix beginnen, für einmaliges Anmelden (SSO) zugelassen sind.
- **Beispiel**: `com.contoso., com.fabrikam.`

[Mit Zustimmung versehene Apps](./application-consent-experience.md), denen der MDM-Administrator die Genehmigung für die SSO-Teilnahme erteilt hat, können automatisch ein Token für den Endbenutzer erhalten. Daher ist es wichtig, der Positivliste nur vertrauenswürdige Anwendungen hinzuzufügen. 

>[!NOTE]
> Sie brauchen dieser Liste keine Anwendungen hinzuzufügen, die MSAL oder ASWebAuthenticationSession verwenden. Diese Anwendungen sind standardmäßig aktiviert. 

##### <a name="how-to-discover-app-bundle-identifiers-on-ios-devices"></a>Ermitteln der Paket-IDs von Anwendungen auf iOS-Geräten

Apple bietet keine einfache Möglichkeit, Paket-IDs im App Store zu ermitteln. Zum Ermitteln der Paket-IDs der Apps, für die einmaliges Anmelden verwendet werden soll, wenden Sie sich am einfachsten an Ihren Anbieter oder den App-Entwickler. Wenn diese Option nicht verfügbar ist, können Sie Ihre MDM-Konfiguration verwenden, um die Paket-IDs zu ermitteln. 

Aktivieren Sie vorübergehend in Ihrer MDM-Konfiguration das folgende Flag:

- **Schlüssel**: `admin_debug_mode_enabled`
- **Typ:** `Integer`
- **Value**: 1 oder 0

Wenn dieses Flag für die Anmeldung bei iOS-Apps auf dem Gerät gesetzt ist, sollten Sie dafür die Paket-ID ermitteln. Öffnen Sie die Microsoft Authenticator-App, und wählen Sie dann „Hilfe“ -> „Protokolle senden“ -> „Protokolle anzeigen“ aus. 

Suchen Sie in der Protokolldatei nach der folgenden Zeile:

`[ADMIN MODE] SSO extension has captured following app bundle identifiers:`

Diese sollte alle Paket-IDs der Anwendungen enthalten, die für die SSO-Erweiterung sichtbar sind. Anschließend können Sie diese IDs zum Konfigurieren des einmaligen Anmeldens (SSO) für diese Apps verwenden. 

#### <a name="allow-user-to-sign-in-from-unknown-applications-and-the-safari-browser"></a>Erlauben von Benutzeranmeldungen über unbekannte Anwendungen und den Safari-Browser

Standardmäßig stellt das Microsoft Enterprise SSO-Plug-In einmaliges Anmelden (SSO) für autorisierte Apps nur dann bereit, wenn sich ein Benutzer über eine App angemeldet hat, die eine Microsoft Identity Platform-Bibliothek wie ADAL oder MSAL verwendet. Das Microsoft Enterprise SSO-Plug-In kann auch freigegebene Anmeldeinformationen abrufen, wenn es beim Erwerb eines neuen Tokens von einer anderen App aufgerufen wird, die eine Microsoft Identity Platform-Bibliothek verwendet.

Durch das Aktivieren des Flags `browser_sso_interaction_enabled` kann eine App, die keine Microsoft Identity Platform-Bibliothek verwendet, das anfängliche Bootstrapping ausführen und freigegebene Anmeldeinformationen abrufen. Auch der Safari-Browser kann auf diese Weise das anfängliche Bootstrapping ausführen und freigegebene Anmeldeinformationen abrufen. Wenn das Microsoft Enterprise SSO-Plug-In noch nicht über freigegebene Anmeldeinformationen verfügt, versucht es jedes Mal Anmeldeinformationen abzurufen, wenn von einer Azure AD-URL im Safari-Browser, von ASWebAuthenticationSession, von SafariViewController oder von einer anderen zugelassenen nativen Anwendung eine Anmeldung angefordert wird.  

- **Schlüssel**: `browser_sso_interaction_enabled`
- **Typ:** `Integer`
- **Value**: 1 oder 0

Bei macOS ist diese Einstellung erforderlich, um eine einheitlichere Umgebung für alle Apps zu erhalten. Bei iOS und IPadOS ist diese Einstellung nicht erforderlich, da die meisten Apps die Microsoft Authenticator-App für die Anmeldung verwenden. Wenn Sie jedoch einige Anwendungen haben, die Microsoft Authenticator unter iOS oder IpadOS nicht verwenden, wird durch dieses Flag die Umgebung verbessert. Daher empfehlen wir, die Einstellung zu aktivieren. Diese Einstellung ist standardmäßig deaktiviert.

#### <a name="disable-asking-for-mfa-on-initial-bootstrapping"></a>Deaktivieren der MFA-Anforderung beim anfänglichen Bootstrapping

Beim anfänglichen Bootstrapping und Abrufen freigegebener Anmeldeinformationen fordert das Microsoft Enterprise SSO-Plug-In standardmäßig den Benutzer immer zur mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) auf, auch wenn dies für die aktuelle Anwendung, die der Benutzer gestartet hat, nicht erforderlich ist. Dies ist der Fall, damit die freigegebenen Anmeldeinformationen problemlos für alle weiteren Anwendungen verwendet werden können und der Benutzer bei späterem MFA-Bedarf nicht erneut aufgefordert werden muss. Dadurch erhält der Benutzer auf dem Gerät weniger Aufforderungen, was im Allgemeinen eine gute Entscheidung ist.

Durch Aktivieren von `browser_sso_disable_mfa` wird diese Einstellung deaktiviert, und der Benutzer wird nur dann aufgefordert, wenn MFA für eine Anwendung oder Ressource erforderlich ist. 

- **Schlüssel**: `browser_sso_disable_mfa`
- **Typ:** `Integer`
- **Value**: 1 oder 0

Wir empfehlen, dieses Flag deaktiviert zu lassen, da dadurch der Benutzer seltener Aufforderungen auf dem Gerät erhält. Wenn Ihre Organisation selten MFA verwendet, könnten Sie das Flag aktivieren. Wir empfehlen jedoch stattdessen, häufiger MFA zu verwenden. Aus diesem Grund ist das Flag standardmäßig deaktiviert.

#### <a name="disable-oauth2-application-prompts"></a>Deaktivieren von Eingabeaufforderungen durch OAuth2-Anwendungen

Das Microsoft Enterprise SSO-Plug-In ermöglicht einmaliges Anmelden (SSO), indem es an Netzwerkanforderungen, die von zulässigen Anwendungen stammen, freigegebene Anmeldeinformationen anfügt. Einige OAuth2-Anwendungen können jedoch fälschlicherweise Benutzereingabeaufforderungen auf der Protokollebene erzwingen. In diesem Fall werden Sie feststellen, dass freigegebene Anmeldeinformationen für diese Apps ignoriert werden, und der Benutzer wird aufgefordert, sich anzumelden, auch wenn das Microsoft Enterprise SSO-Plug-In für andere Anwendungen funktioniert.  

Wenn Sie das `disable_explicit_app_prompt`-Flag aktivieren, wird die Fähigkeit von nativen und Webanwendungen, eine Endbenutzereingabeaufforderung auf der Protokollebene zu erzwingen und SSO zu umgehen, eingeschränkt.

- **Schlüssel**: `disable_explicit_app_prompt`
- **Typ:** `Integer`
- **Value**: 1 oder 0

Wir empfehlen, dieses Flag zu aktivieren, um in allen Apps eine einheitlichere Darstellung zu erzielen. Diese Einstellung ist standardmäßig deaktiviert. 

#### <a name="enable-sso-through-cookies-for-specific-application"></a>Aktivieren von SSO über Cookies für eine bestimmte Anwendung

Eine kleine Anzahl von Apps ist möglicherweise nicht mit der SSO-Erweiterung kompatibel. Insbesondere bei Apps mit erweiterten Netzwerkeinstellungen können unerwartete Probleme auftreten, wenn sie für einmaliges Anmelden (SSO) aktiviert werden. (Es kann z. B. ein Fehler angezeigt werden, dass die Netzwerkanforderung abgebrochen oder unterbrochen wurde.) 

Wenn bei der Anmeldung mit der im Abschnitt `Enable SSO for apps that don't use MSAL` beschriebenen Methode Probleme auftreten, könnten Sie eine alternative Konfiguration für diese Apps ausprobieren. 

Verwenden Sie die folgenden Parameter, um das Microsoft Enterprise SSO-Plug-In für Apple-Geräte zu konfigurieren:

- **Schlüssel**: `AppCookieSSOAllowList`
- **Typ:** `String`
- **Value**: Kommagetrennte Liste mit den Paket-IDs (Präfixen) der Anwendungen, die für einmaliges Anmelden (SSO) zugelassen sind. Beachten Sie, dass dadurch alle Apps, die mit einem bestimmten Präfix beginnen, für einmaliges Anmelden (SSO) zugelassen sind.
- **Beispiel**: `com.contoso.myapp1, com.fabrikam.myapp2`

Beachten Sie, dass Anwendungen, die auf diese Weise für einmaliges Anmelden (SSO) aktiviert wurden, sowohl zur `AppCookieSSOAllowList` als auch zur `AppPrefixAllowList` hinzugefügt werden müssen.

Wir empfehlen, diese Option nur für Anwendungen auszuprobieren, bei denen unerwartete Anmeldefehler auftreten. 

#### <a name="use-intune-for-simplified-configuration"></a>Verwenden von Intune zum Vereinfachen der Konfiguration

Wie bereits erwähnt, können Sie Microsoft Intune als MDM-Dienst verwenden, um die Konfiguration des Microsoft Enterprise SSO-Plug-Ins zu vereinfachen. Dazu gehören auch das Aktivieren des Plug-Ins und das Hinzufügen älterer Apps zu einer Zulassungsliste, damit sie für SSO zugelassen werden. Weitere Informationen finden Sie in der [Dokumentation zur Intune-Konfiguration](/intune/configuration/ios-device-features-settings).

## <a name="using-the-sso-plug-in-in-your-application"></a>Verwenden des SSO-Plug-Ins in Ihrer Anwendung

Version 1.1.0 (und höher) der [Microsoft Authentication Library (MSAL) für Apple-Geräte](https://github.com/AzureAD/microsoft-authentication-library-for-objc) unterstützt das Microsoft Enterprise SSO-Plug-In für Apple-Geräte. Es empfiehlt sich, auf diesem Weg Unterstützung für das Microsoft Enterprise SSO-Plug-In hinzuzufügen und sicherzustellen, dass Sie die umfangreichen Funktionen von Microsoft Identity Platform in vollem Umfang nutzen können.

Wenn Sie eine Anwendung für Mitarbeiter im Kundenkontakt oder vergleichbare Szenarien erstellen, finden Sie weitere Informationen zum Setup des Features unter [Modus für gemeinsam genutzte Geräte für iOS-Geräte](msal-ios-shared-devices.md).

## <a name="how-the-sso-plug-in-works"></a>Funktionsweise des SSO-Plug-Ins

Das Microsoft Enterprise SSO-Plug-In basiert auf dem [Enterprise Single Sign-On-Framework von Apple](https://developer.apple.com/documentation/authenticationservices/asauthorizationsinglesignonprovider?language=objc). In das Framework integrierte Identitätsanbieter können den Netzwerkdatenverkehr für ihre Domänen abfangen und die Verarbeitung dieser Anforderungen verbessern oder ändern. Das SSO-Plug-In kann z. B. eine zusätzliche Benutzeroberfläche anzeigen, um Anmeldeinformationen von Endbenutzern sicher zu erfassen, MFA anzufordern oder automatisch Token für die Anwendung bereitzustellen.

Native Anwendungen können außerdem benutzerdefinierte Vorgänge implementieren und direkt mit dem SSO-Plug-In kommunizieren.
Weitere Informationen zum SSO-Framework erhalten Sie im [WWDC-Video von 2019 von Apple](https://developer.apple.com/videos/play/tech-talks/301/).

### <a name="applications-that-use-a-microsoft-identity-platform-library"></a>Anwendungen mit Microsoft Identity Platform-Bibliothek

Version 1.1.0 (und höher) der [Microsoft Authentication Library (MSAL) für Apple-Geräte](https://github.com/AzureAD/microsoft-authentication-library-for-objc) unterstützt das Microsoft Enterprise SSO-Plug-In für Apple-Geräte nativ für Geschäfts-, Schul- oder Unikonten. 

Es ist keine besondere Konfiguration erforderlich, wenn Sie [alle empfohlenen Schritte](./quickstart-v2-ios.md) befolgen und das standardmäßige [Umleitungs-URI-Format](./redirect-uris-ios.md) verwenden. Bei der Ausführung auf einem Gerät, auf dem das SSO-Plug-In vorhanden ist, ruft MSAL das Plug-In automatisch für alle interaktiven und automatischen Tokenanforderungen sowie für Kontoenumerations- und Kontoentfernungsvorgänge auf. Da MSAL das native SSO-Plug-In-Protokoll implementiert, das auf benutzerdefinierten Vorgängen basiert, bietet dieses Setup die komfortabelste native Umgebung für den Endbenutzer. 

Wenn das SSO-Plug-In von MDM nicht aktiviert wird, aber die Microsoft Authenticator-App auf dem Gerät vorhanden ist, verwendet MSAL stattdessen die Microsoft Authenticator-App für alle interaktiven Tokenanforderungen. Das SSO-Plug-In gibt SSO für die Microsoft Authenticator-App frei.

### <a name="applications-that-dont-use-a-microsoft-identity-platform-library"></a>Anwendungen ohne Microsoft Identity Platform-Bibliothek

Anwendungen, die keine Microsoft Identity Platform-Bibliothek wie MSAL verwenden, können trotzdem vom einmaligen Anmelden (SSO) profitieren, wenn sie von einem Administrator explizit zur Zulassungsliste hinzugefügt werden. 

In diesen Apps sind keine Codeänderungen erforderlich, solange die folgenden Bedingungen erfüllt sind:

- Die Anwendung verwendet Apple-Frameworks, um Netzwerkanforderungen (z. B. [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview), [NSURLSession](https://developer.apple.com/documentation/foundation/nsurlsession)) auszuführen. 
- Die Anwendung verwendet Standardprotokolle für die Kommunikation mit Azure AD (z. B. OAuth2, SAML, WS-Verbund).
- Die Anwendung erfasst auf der nativen Benutzeroberfläche keine Benutzernamen und Kennwörter in Klartext.

In diesem Fall wird SSO bereitgestellt, wenn die Anwendung eine Netzwerkanforderung erstellt und einen Webbrowser öffnet, um den Benutzer anzumelden. Wenn ein Benutzer zu einer Azure AD-Anmelde-URL umgeleitet wird, überprüft das SSO-Plug-In die URL und prüft, ob für diese URL SSO-Anmeldeinformationen verfügbar sind. Wenn diese vorhanden sind, übergibt das SSO-Plug-In die SSO-Anmeldeinformationen an Azure AD. Azure AD autorisiert dann die Anwendung, die Netzwerkanforderung abzuschließen, ohne den Benutzer zur Eingabe seiner Anmeldeinformationen aufzufordern. Wenn das Gerät Azure AD bekannt ist, übergibt das SSO-Plug-In außerdem das Gerätezertifikat, um die gerätebasierte bedingte Zugriffsüberprüfung zu erfüllen. 

Zur Unterstützung des einmaligen Anmeldens (SSO) für Apps ohne MSAL implementiert das SSO-Plug-In ein dem Windows-Browser-Plug-In vergleichbares Protokoll, das unter [Was ist ein primäres Aktualisierungstoken?](../devices/concept-primary-refresh-token.md#browser-sso-using-prt) beschrieben ist. 

Im Vergleich zu MSAL-basierten Apps ist die Funktionsweise des SSO-Plug-Ins für Apps ohne MSAL transparenter, da die von den Apps bereitgestellte vorhandene Browseranmeldeumgebung integriert wird. Dem Endbenutzer wird die vertraute Umgebung angezeigt, und er hat zudem den Vorteil, dass er nicht in jeder Anwendung weitere Anmeldungen vornehmen muss. Anstatt die native Kontoauswahl anzuzeigen, fügt das SSO-Plug-In beispielsweise der webbasierten Kontoauswahlumgebung SSO-Sitzungen hinzu. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Modus für gemeinsam genutzte Geräte unter iOS finden Sie im Artikel [Modus für gemeinsam genutzte Geräte für iOS-Geräte](msal-ios-shared-devices.md).
