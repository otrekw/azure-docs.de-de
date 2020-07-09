---
title: Hinzufügen von Google als Identitätsanbieter für B2B – Azure AD
description: Erstellen eines Verbunds mit Google, um Gastbenutzern die Anmeldung bei Ihren Azure AD-Apps mit ihren eigenen Gmail-Konten zu ermöglichen
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 05/11/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4b895054f8fa81526bf72cadd2fea1a3691d758
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85387115"
---
# <a name="add-google-as-an-identity-provider-for-b2b-guest-users"></a>Hinzufügen von Google als Identitätsanbieter für B2B-Gastbenutzer

Durch die Einrichtung eines Verbunds mit Google können Sie eingeladenen Benutzern ermöglichen, sich bei Ihren freigegebenen Apps und Ressourcen mit ihren eigenen Gmail-Konten anzumelden, ohne dass sie Microsoft-Konten (Microsoft Accounts, MSAs) erstellen müssen. 

> [!NOTE]
> Der Google-Verbund wurde speziell für Gmail-Benutzer konzipiert. Um einen Verbund mit G Suite-Domänen herzustellen, verwenden Sie das [Feature „Direkter Verbund“](direct-federation.md).

## <a name="what-is-the-experience-for-the-google-user"></a>Wie läuft der Vorgang für Google-Benutzer ab?
Wenn Sie eine Einladung an einen Google Gmail-Benutzer senden, sollte der Gastbenutzer auf Ihre freigegebenen Apps oder Ressourcen über einen Link, der den Mandantenkontext enthält, zugreifen. Der Ablauf hängt davon ab, ob er bereits bei Google angemeldet ist:
  - Wenn der Gastbenutzer nicht bei Google angemeldet ist, wird er aufgefordert, sich bei Google anzumelden.
  - Wenn der Gastbenutzer bereits bei Google angemeldet ist, wird er aufgefordert, das Konto auszuwählen, das er verwenden möchte. Er muss das Konto auswählen, das Sie für die Einladung verwendet haben.

Wenn dem Gastbenutzer ein Fehler aufgrund eines zu langen Headers angezeigt wird, sollte er versuchen, seine Cookies zu löschen. Er kann auch ein privates oder Inkognitofenster öffnen und sich erneut anzumelden.

![Screenshot der Google-Anmeldeseite](media/google-federation/google-sign-in.png)

## <a name="limitations"></a>Einschränkungen

Teams unterstützt Google-Gastbenutzer vollständig auf allen Geräten. Google-Benutzer können sich über einen allgemeinen Endpunkt wie `https://teams.microsoft.com` bei Teams anmelden.

Die allgemeinen Endpunkte anderer Anwendungen unterstützen Google-Benutzer möglicherweise nicht. Google-Gastbenutzer müssen sich über einen Link anmelden, der Mandanteninformationen enthält. Hier finden Sie einige Beispiele:
  * `https://myapps.microsoft.com/?tenantid=<your tenant id>`
  * `https://portal.azure.com/<your tenant id>`
  * `https://myapps.microsoft.com/<your verified domain>.onmicrosoft.com`

   Wenn Google-Gastbenutzer versuchen, einen Link wie `https://myapps.microsoft.com` oder `https://portal.azure.com` zu verwenden, erhalten sie eine Fehlermeldung.

Sie können Google-Gastbenutzern auch einen direkten Link zu einer Anwendung oder Ressource zur Verfügung stellen, sofern dieser Link Ihre Mandanteninformationen enthält, z. B. `https://myapps.microsoft.com/signin/Twitter/<application ID?tenantId=<your tenant ID>`. 

## <a name="step-1-configure-a-google-developer-project"></a>Schritt 1: Konfigurieren eines Google-Entwicklerprojekts
Erstellen Sie zunächst ein neues Projekt in der Google Developers Console, um eine Client-ID und einen geheimen Clientschlüssel abzurufen. Beides fügen Sie später in Azure AD hinzu. 
1. Wechseln Sie zur Google-APIs unter https://console.developers.google.com, und melden Sie sich mit Ihrem Google-Konto an. Es wird empfohlen, ein freigegebenes Google-Teamkonto zu verwenden.
2. Erstellen eines neuen Projekts: Wählen Sie auf dem Dashboard **Projekt erstellen** und dann **Erstellen** aus. Geben Sie auf der Seite „Neues Projekt“ einen **Projektnamen** ein, und wählen Sie dann **Erstellen** aus.
   
   ![Screenshot mit einer Seite „Neues Projekt“ für Google](media/google-federation/google-new-project.png)

3. Stellen Sie sicher, dass das neue Projekt im Menü „Projekt“ ausgewählt ist. Wählen Sie dann unter **APIs und Dienste**  die Option**OAuth-Zustimmungsbildschirm** aus.

4. Wählen Sie **Extern** und anschließend **Erstellen** aus. 
5. Geben Sie im **OAuth-Zustimmungsbildschirm** einen **Anwendungsnamen** ein. (Behalten Sie die restlichen Einstellungen bei.)

   ![Screenshot der Option „Google-OAuth-Einwilligungsbildschirm“](media/google-federation/google-oauth-consent-screen.png)

6. Scrollen Sie zum Abschnitt **Authorized domains** (Autorisierte Domänen), und geben Sie „microsoftonline.com“ ein.

   ![Screenshot des Abschnitts „Autorisierte Domänen“](media/google-federation/google-oauth-authorized-domains.png)

7. Wählen Sie **Speichern** aus.

8. Wählen Sie **Anmeldeinformationen** aus. Wählen Sie im Menü **Anmeldedaten erstellen** die Option **OAuth-Client-ID** aus.

   ![Screenshot der Google-API-Option zum Erstellen von Anmeldeinformationen](media/google-federation/google-api-credentials.png)

9. Wählen Sie unter **Anwendungstyp** die Option **Webanwendung** aus, und geben Sie dann unter **Autorisierte Weiterleitungs-URIs** die folgenden URIs ein:
   - `https://login.microsoftonline.com` 
   - `https://login.microsoftonline.com/te/<directory id>/oauth2/authresp` <br>(Dabei ist `<directory id>` Ihre Verzeichnis-ID.)
   
     > [!NOTE]
     > Sie finden Ihre Verzeichnis-ID unter https://portal.azure.com. Wählen Sie dort unter **Azure Active Directory** die Option **Eigenschaften** aus, und kopieren Sie die **Verzeichnis-ID**.

   ![Screenshot des Abschnitts „Autorisierte Umleitungs-URIs“](media/google-federation/google-create-oauth-client-id.png)

10. Klicken Sie auf **Erstellen**. Kopieren Sie die Client-ID und den geheimen Clientschlüssel, die Sie verwenden möchten, um den Identitätsanbieter im Azure AD-Portal hinzuzufügen.

   ![Screenshot der OAuth-Client-ID und des Clientgeheimnisses](media/google-federation/google-auth-client-id-secret.png)

## <a name="step-2-configure-google-federation-in-azure-ad"></a>Schritt 2: Konfigurieren des Google-Verbunds in Azure AD 
Nun legen Sie die Google-Client-ID und den geheimen Clientschlüssel entweder durch Eingabe im Azure AD-Portal oder mithilfe von PowerShell fest. Testen Sie unbedingt die Konfiguration des Google-Verbunds, indem Sie sich selbst unter Verwendung einer Gmail-Adresse einladen und versuchen, die Einladung mit dem eingeladenen Google-Konto einzulösen. 

#### <a name="to-configure-google-federation-in-the-azure-ad-portal"></a>So konfigurieren Sie den Google-Verbund im Azure AD-Portal 
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Wählen Sie im linken Bereich **Azure Active Directory** aus. 
2. Wählen Sie **Externe Identitäten** aus.
3. Wählen Sie **Alle Identitätsanbieter** aus, und klicken Sie dann auf die Schaltfläche **Google**.
4. Geben Sie einen Namen ein. Geben Sie dann die Client-ID und den geheimen Clientschlüssel ein, die Sie zuvor erhalten haben. Wählen Sie **Speichern** aus. 

   ![Screenshot der Seite „Google als Identitätsanbieter hinzufügen“](media/google-federation/google-identity-provider.png)

#### <a name="to-configure-google-federation-by-using-powershell"></a>So konfigurieren Sie den Google-Verbund mithilfe von PowerShell
1. Installieren Sie die neueste Version des Azure AD PowerShell für Graph-Moduls ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Führen Sie den folgenden Befehl aus: `Connect-AzureAD`.
3. Melden Sie sich an der Anmeldeaufforderung mit dem verwalteten globalen Administratorkonto an.  
4. Führen Sie den folgenden Befehl aus: 
   
   `New-AzureADMSIdentityProvider -Type Google -Name Google -ClientId [Client ID] -ClientSecret [Client secret]`
 
   > [!NOTE]
   > Verwenden Sie die Client-ID und den geheimen Clientschlüssel der App, die Sie hier erstellt haben: „Schritt 1: Konfigurieren eines Google-Entwicklerprojekts“. Weitere Informationen finden Sie im Artikel zu [New-AzureADMSIdentityProvider](https://docs.microsoft.com/powershell/module/azuread/new-azureadmsidentityprovider?view=azureadps-2.0-preview). 
 
## <a name="how-do-i-remove-google-federation"></a>Wie entferne ich einen Google Verbund?
Sie können Ihre Google-Verbundeinrichtung löschen. Wenn Sie dies tun, können Google-Gastbenutzer, die bereits seine Einladung eingelöst haben, sich nicht mehr anmelden. Sie können ihnen aber erneut Zugriff auf Ihre Ressourcen erteilen, indem Sie sie aus dem Verzeichnis löschen und erneut einladen. 
 
### <a name="to-delete-google-federation-in-the-azure-ad-portal"></a>So löschen Sie den Google-Verbund im Azure AD-Portal 
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Wählen Sie im linken Bereich **Azure Active Directory** aus. 
2. Wählen Sie **Externe Identitäten** aus.
3. Wählen Sie **Alle Identitätsanbieter** aus.
4. Wählen Sie in der Zeile **Google** das Kontextmenü ( **...** ) aus, und wählen Sie dann **Löschen** aus. 
   
   ![Screenshot der Option „Löschen“ für den Identitätsanbieter für soziale Netzwerke](media/google-federation/google-social-identity-providers.png)

1. Wählen Sie **Ja** aus, um den Löschvorgang zu bestätigen. 

### <a name="to-delete-google-federation-by-using-powershell"></a>So löschen Sie den Google-Verbund mithilfe von PowerShell 
1. Installieren Sie die neueste Version des Azure AD PowerShell für Graph-Moduls ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Führen Sie `Connect-AzureAD` aus.  
4. Melden Sie sich an der Anmeldeaufforderung mit dem verwalteten globalen Administratorkonto an.  
5. Geben Sie den folgenden Befehl ein:

    `Remove-AzureADMSIdentityProvider -Id Google-OAUTH`

   > [!NOTE]
   > Weitere Informationen finden Sie unter [Remove-AzureADMSIdentityProvider](https://docs.microsoft.com/powershell/module/azuread/Remove-AzureADMSIdentityProvider?view=azureadps-2.0-preview). 
