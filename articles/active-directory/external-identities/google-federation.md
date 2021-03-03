---
title: Hinzufügen von Google als Identitätsanbieter für B2B – Azure AD
description: Erstellen eines Verbunds mit Google, um Gastbenutzern die Anmeldung bei Ihren Azure AD-Apps mit ihren eigenen Gmail-Konten zu ermöglichen
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 03/02/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9e7ec5569dd0de3b0535c3b0e3b3304848a5207
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101653317"
---
# <a name="add-google-as-an-identity-provider-for-b2b-guest-users"></a>Hinzufügen von Google als Identitätsanbieter für B2B-Gastbenutzer

Durch die Einrichtung eines Verbunds mit Google können Sie eingeladenen Benutzern ermöglichen, sich bei Ihren freigegebenen Apps und Ressourcen mit ihren eigenen Gmail-Konten anzumelden, ohne dass sie Microsoft-Konten erstellen müssen.

Nachdem Sie Google als Anmeldeoption für Ihre Anwendung hinzugefügt haben, können Benutzer auf der **Anmeldeseite** einfach die E-Mail-Adresse eingeben, mit der sie sich bei Google anmelden. Alternativ können sie zuerst **Anmeldeoptionen** und dann **Mit Google anmelden** auswählen. In beiden Fällen werden sie zur Authentifizierung an die Google-Anmeldeseite umgeleitet.

![Anmeldeoptionen für Google-Benutzer](media/google-federation/sign-in-with-google-overview.png)

> [!NOTE]
> Der Google-Verbund wurde speziell für Gmail-Benutzer konzipiert. Um einen Verbund mit G Suite-Domänen einzurichten, verwenden Sie den [direkten Verbund](direct-federation.md).

> [!IMPORTANT]
> **Am 4. Januar 2021** wird Google [die Unterstützung für die WebView-Anmeldung einstellen](https://developers.googleblog.com/2020/08/guidance-for-our-effort-to-block-less-secure-browser-and-apps.html). Wenn Sie einen Google-Verbund oder die Self-Service-Registrierung mit Gmail verwenden, sollten Sie [Ihre nativen Branchenanwendungen auf Kompatibilität testen](google-federation.md#deprecation-of-webview-sign-in-support).

## <a name="what-is-the-experience-for-the-google-user"></a>Wie läuft der Vorgang für Google-Benutzer ab?

Wenn ein Google-Benutzer Ihre Einladung einlöst, hängt das Benutzererlebnis davon ab, ob er schon bei Google angemeldet ist:

- Gastbenutzer, die nicht bei Google angemeldet sind, werden aufgefordert, sich anzumelden.
- Gastbenutzer, die bereits bei Google angemeldet sind, werden aufgefordert, das zu verwendende Konto auszuwählen. Er muss das Konto auswählen, das Sie für die Einladung verwendet haben.

Gastbenutzer, bei denen ein Fehler aufgrund eines zu langen Headers angezeigt wird, können Cookies löschen oder ein privates oder Inkognito-Fenster öffnen und sich erneut anmelden.

![Screenshot der Google-Anmeldeseite](media/google-federation/google-sign-in.png)

## <a name="sign-in-endpoints"></a>Endpunkte für die Anmeldung

Google-Gastbenutzer können sich nun mithilfe eines [gemeinsamen Endpunkts](redemption-experience.md#redemption-and-sign-in-through-a-common-endpoint) (d. h. mit einer allgemeinen App-URL, die Ihren Mandantenkontext nicht enthält) bei Ihren mehrmandantenfähigen Anwendungen oder bei Microsoft-Erstanbieter-Apps anmelden. Im Folgenden finden Sie Beispiele für gemeinsame Endpunkte:

- `https://teams.microsoft.com`
- `https://myapps.microsoft.com`
- `https://portal.azure.com`

Beim Anmeldevorgang wählt der Gastbenutzer zuerst **Anmeldeoptionen** und dann **Bei einer Organisation anmelden** aus. Der Benutzer gibt dann den Namen Ihres Unternehmens ein und setzt den Vorgang mit seinen Google-Anmeldeinformationen fort.

Google-Gastbenutzer können auch Anwendungsendpunkte verwenden, die Ihre Mandanteninformationen enthalten, z. B.:

  * `https://myapps.microsoft.com/?tenantid=<your tenant ID>`
  * `https://myapps.microsoft.com/<your verified domain>.onmicrosoft.com`
  * `https://portal.azure.com/<your tenant ID>`

Sie können Google-Gastbenutzern auch einen direkten Link zu einer Anwendung oder Ressource zur Verfügung stellen und Ihre Mandanteninformationen einfügen, z. B. `https://myapps.microsoft.com/signin/Twitter/<application ID?tenantId=<your tenant ID>`.

## <a name="deprecation-of-webview-sign-in-support"></a>Einstellung der Unterstützung für die WebView-Anmeldung

Am 4. Januar 2021 wird Google die [Unterstützung für die eingebettete WebView-Anmeldung einstellen](https://developers.googleblog.com/2020/08/guidance-for-our-effort-to-block-less-secure-browser-and-apps.html). Wenn Sie einen Google-Verbund oder die [Self-Service-Registrierung mit Gmail](identity-providers.md) verwenden, sollten Sie Ihre nativen Branchenanwendungen auf Kompatibilität testen. Wenn Ihre Apps WebView-Inhalte enthalten, für die eine Authentifizierung erforderlich ist, können sich Benutzer von Google Gmail nicht mehr authentifizieren. Im Folgenden finden Sie bekannte Szenarien mit Auswirkungen auf Gmail-Benutzer:

- Windows-Apps, die eingebettete WebView-Inhalte oder WebAccountManager (WAM) unter älteren Versionen von Windows verwenden
- Andere von Ihnen entwickelte native Apps, die ein eingebettetes Browserframework für die Authentifizierung verwenden

Diese Änderung wirkt sich nicht auf Folgendes aus:

- Windows-Apps, die eingebettete WebView-Inhalte oder WebAccountManager (WAM) unter den neuesten Versionen von Windows verwenden
- Microsoft-iOS-Apps
- G Suite-Identitäten, z. B. bei Verwendung eines SAML-basierten [direkten Verbunds](direct-federation.md) mit G Suite

Wir testen weiterhin verschiedene Plattformen und Szenarien und werden diesen Artikel auf dem neuesten Stand halten.
### <a name="to-test-your-apps-for-compatibility"></a>So testen Sie Ihre Apps auf Kompatibilität

1. Ermitteln Sie anhand des [Google-Leitfadens](https://developers.googleblog.com/2020/08/guidance-for-our-effort-to-block-less-secure-browser-and-apps.html), ob Ihre Apps betroffen sind.
2. Nutzen Sie Fiddler oder ein anderes Testtool, injizieren Sie während der Anmeldung einen Header, und verwenden Sie eine externe Google-Identität, um die Anmeldung zu testen:

   1. Fügen Sie Ihren HTTP-Anforderungsheadern „Google-Accounts-Check-OAuth-Login:true“ hinzu, wenn die Anforderungen an accounts.google.com gesendet werden.
   1. Versuchen Sie sich bei der App anzumelden, indem Sie auf der Anmeldeseite unter accounts.google.com eine Gmail-Adresse eingeben.
   1. Wenn die Anmeldung eine Fehlermeldung wie „Dieser Browser oder diese App ist möglicherweise nicht sicher“ verursacht, wird die Anmeldung mit Ihren externen Google-Identitäten blockiert.

3. Beheben Sie das Problem, indem Sie eine der folgenden Aktionen ausführen:

   - Wenn Ihre Windows-App eingebettete WebView-Inhalte oder WebAccountManager (WAM) unter einer älteren Version von Windows verwendet, führen Sie ein Update auf die neueste Version von Windows durch.
   - Ändern Sie Ihre Apps so, dass diese den Systembrowser für die Anmeldung verwenden. Weitere Informationen finden Sie in der MSAL.NET-Dokumentation unter [Eingebettete Webbenutzeroberfläche im Vergleich zur System-Webbenutzeroberfläche](../develop/msal-net-web-browsers.md#embedded-vs-system-web-ui).  


## <a name="step-1-configure-a-google-developer-project"></a>Schritt 1: Konfigurieren eines Google-Entwicklerprojekts
Erstellen Sie zunächst ein neues Projekt in der Google Developers Console, um eine Client-ID und einen geheimen Clientschlüssel abzurufen. Beide fügen Sie später in Azure Active Directory (Azure AD) hinzu. 
1. Wechseln Sie zur Google-APIs unter https://console.developers.google.com, und melden Sie sich mit Ihrem Google-Konto an. Es wird empfohlen, ein freigegebenes Google-Teamkonto zu verwenden.
2. Stimmen Sie den Vertragsbedingungen zu, wenn Sie dazu aufgefordert werden.
3. Erstellen Sie ein neues Projekt: Wählen Sie in der oberen linken Ecke der Seite die Projektliste aus, und wählen Sie dann auf der Seite **Projekt auswählen** die Option **Neues Projekt** aus.
4. Geben Sie auf der Seite **Neues Projekt** dem Projekt einen Namen (z. B. **Azure AD B2B**), und wählen Sie dann **Erstellen** aus: 
   
   ![Screenshot der Seite „Neues Projekt“](media/google-federation/google-new-project.png)

4. Wählen Sie auf der Seite **APIs & Dienste** unter dem neuen Projekt die Option **Anzeigen** aus.

5. Wählen Sie auf der Karte „APIs“die Option **Go to APIs overview** (Zur API-Übersicht wechseln) aus. Wählen Sie **OAuth-Zustimmungsbildschirm** aus.

6. Wählen Sie **Extern** und anschließend **Erstellen** aus. 

7. Geben Sie im **OAuth-Einwilligungsbildschirm** einen **Anwendungsnamen** ein:

   ![Screenshot des Google-OAuth-Einwilligungsbildschirms](media/google-federation/google-oauth-consent-screen.png)

8. Scrollen Sie zum Abschnitt **Authorized domains** (Autorisierte Domänen), und geben Sie **microsoftonline.com** ein:

   ![Screenshot des Abschnitts „Authorized domains“ (Autorisierte Domänen)](media/google-federation/google-oauth-authorized-domains.PNG)

9. Wählen Sie **Speichern** aus.

10. Wählen Sie **Credentials** aus. Wählen Sie im Menü **Anmeldedaten erstellen** die Option **OAuth-Client-ID** aus:

    ![Screenshot des Google APIs-Menüs „Anmeldedaten erstellen“](media/google-federation/google-api-credentials.png)

11. Wählen Sie unter **Anwendungstyp** die Option **Webanwendung** aus. Geben Sie der Anwendung einen geeigneten Namen, z. B. **Azure AD B2B**. Geben Sie unter **Autorisierte Weiterleitungs-URIs** die folgenden URIs ein:
    - `https://login.microsoftonline.com`
    - `https://login.microsoftonline.com/te/<tenant ID>/oauth2/authresp` <br>(`<tenant ID>` ist Ihre Mandanten-ID.)
   
    > [!NOTE]
    > Ihre Mandanten-ID finden Sie im [Azure-Portal](https://portal.azure.com). Wählen Sie dort unter **Azure Active Directory** die Option **Eigenschaften** aus, und kopieren Sie die **Mandanten-ID**.

    ![Screenshot des Abschnitts „Autorisierte Umleitungs-URIs“](media/google-federation/google-create-oauth-client-id.png)

12. Klicken Sie auf **Erstellen**. Kopieren Sie die Client-ID und den geheimen Clientschlüssel. Sie verwenden diese Informationen, wenn Sie den Identitätsanbieter im Azure-Portal hinzufügen.

    ![Screenshot von OAuth-Client-ID und geheimem Clientschlüssel](media/google-federation/google-auth-client-id-secret.png)

## <a name="step-2-configure-google-federation-in-azure-ad"></a>Schritt 2: Konfigurieren des Google-Verbunds in Azure AD 
Sie legen nun die Google-Client-ID und den geheimen Clientschlüssel fest. Dazu können Sie das Azure-Portal oder PowerShell verwenden. Testen Sie unbedingt die Konfiguration des Google-Verbunds, indem Sie sich selbst einladen. Verwenden Sie eine Gmail-Adresse, und versuchen Sie, die Einladung mit Ihrem eingeladenen Google-Konto einzulösen. 

**So konfigurieren Sie den Google-Verbund im Azure-Portal** 
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Wählen Sie im linken Bereich **Azure Active Directory** aus. 
2. Wählen Sie **Externe Identitäten** aus.
3. Wählen Sie **Alle Identitätsanbieter** und dann die Schaltfläche **Google** aus.
4. Geben Sie die Client-ID und den geheimen Clientschlüssel ein, die Sie zuvor erhalten haben. Wählen Sie **Speichern** aus: 

   ![Screenshot der Seite „Google als Identitätsanbieter hinzufügen“](media/google-federation/google-identity-provider.png)

**So konfigurieren Sie den Google-Verbund mithilfe von PowerShell**
1. Installieren Sie die neueste Version des Azure AD PowerShell für Graph-Moduls ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Führen Sie den folgenden Befehl aus: `Connect-AzureAD`
3. Melden Sie sich an der Anmeldeaufforderung mit dem verwalteten globalen Administratorkonto an.  
4. Führen Sie den folgenden Befehl aus: 
   
   `New-AzureADMSIdentityProvider -Type Google -Name Google -ClientId <client ID> -ClientSecret <client secret>`
 
   > [!NOTE]
   > Verwenden Sie die Client-ID und den geheimen Clientschlüssel der App, die Sie hier erstellt haben: „Schritt 1: Konfigurieren eines Google-Entwicklerprojekts“. Weitere Informationen finden Sie unter [New-AzureADMSIdentityProvider](/powershell/module/azuread/new-azureadmsidentityprovider?view=azureadps-2.0-preview). 
 
## <a name="how-do-i-remove-google-federation"></a>Wie entferne ich einen Google Verbund?
Sie können Ihre Google-Verbundeinrichtung löschen. Wenn Sie dies tun, können sich Google-Gastbenutzer, die ihre Einladungen bereits eingelöst haben, nicht mehr anmelden. Sie können ihnen aber erneut Zugriff auf Ihre Ressourcen gewähren, indem Sie sie aus dem Verzeichnis löschen und erneut einladen. 
 
**So löschen Sie den Google-Verbund im Azure AD-Portal**
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Wählen Sie im linken Bereich **Azure Active Directory** aus. 
2. Wählen Sie **Externe Identitäten** aus.
3. Wählen Sie **Alle Identitätsanbieter** aus.
4. Wählen Sie in der Zeile **Google** die Schaltfläche mit den Auslassungspunkten ( **...** ) und dann **Löschen** aus. 
   
   ![Screenshot der Schaltfläche „Löschen“ für den Identitätsanbieter für soziale Netzwerke](media/google-federation/google-social-identity-providers.png)

1. Wählen Sie **Ja** aus, um den Löschvorgang zu bestätigen. 

**So löschen Sie den Google-Verbund mithilfe von PowerShell** 
1. Installieren Sie die neueste Version des Azure AD PowerShell für Graph-Moduls ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Führen Sie `Connect-AzureAD` aus.  
4. Melden Sie sich an der Anmeldeaufforderung mit dem verwalteten globalen Administratorkonto an.  
5. Geben Sie den folgenden Befehl ein:

    `Remove-AzureADMSIdentityProvider -Id Google-OAUTH`

   > [!NOTE]
   > Weitere Informationen finden Sie unter [Remove-AzureADMSIdentityProvider](/powershell/module/azuread/Remove-AzureADMSIdentityProvider?view=azureadps-2.0-preview).
