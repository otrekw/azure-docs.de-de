---
title: Einrichten der Registrierung und Anmeldung mit einem Salesforce-Konto
titleSuffix: Azure AD B2C
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Salesforce-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/07/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 69c2bd96c7aa3bb3328784bb3b5027ade4902c43
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2020
ms.locfileid: "97669226"
---
# <a name="set-up-sign-up-and-sign-in-with-a-salesforce-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem Salesforce-Konto mithilfe von Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-custom-policy"

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

::: zone-end

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]


## <a name="create-a-salesforce-application"></a>Erstellen einer Salesforce-Anwendung

Damit Sie in Azure Active Directory B2C (Azure AD B2C) ein Salesforce-Konto verwenden können, müssen Sie in Ihrem **Anwendungs-Manager** in Salesforce eine Anwendung erstellen. Weitere Informationen finden Sie unter [Konfigurieren der grundlegenden Einstellungen für verbundene Anwendungen](https://help.salesforce.com/articleView?id=connected_app_create_basics.htm) und [Aktivieren von OAuth-Einstellungen für die API-Integration](https://help.salesforce.com/articleView?id=connected_app_create_api_integration.htm).

1. [Anmelden Sie sich bei Salesforce an](https://login.salesforce.com/).
1. Klicken Sie im Menü auf **Setup**.
1.  Erweitern Sie **Anwendungen**, und wählen Sie dann **Anwendungs-Manager** aus.
1. Klicken Sie auf **Neue verbundene Anwendung**.
1. Geben Sie unter **Basic Information** (Grundlegende Informationen) Folgendes ein:
    1. **Connected App Name** (Name der verbundenen Anwendung): Der Name der verbundenen Anwendung wird im Anwendungs-Manager und auf der zugehörigen App Launcher-Kachel angezeigt. Der Name muss innerhalb Ihrer Organisation eindeutig sein. 
    1. **API-Name** 
    1. **Contact Email** (Kontakt-E-Mail-Adresse): Geben Sie hier die Kontakt-E-Mail-Adresse für Salesforce an.
1. Wählen Sie unter **API (OAuth-Einstellungen aktivieren)** die Option **OAuth-Einstellungen aktivieren** aus.
1. Geben Sie unter **Rückmeldungs-URL** `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` an. Ersetzen Sie `your-tenant-name` durch den Namen Ihres Mandanten. Bei der Eingabe Ihres Mandantennamens dürfen Sie nur Kleinbuchstaben verwenden, auch wenn der Mandant in Azure AD B2C Großbuchstaben enthält.
1. Wählen Sie unter **Selected OAuth Scopes** (Ausgewählte OAuth-Bereiche) **Zugriff auf Ihre grundlegenden Informationen (id, profile, email, address, phone)** und **Zugriff auf Ihre eindeutige Kennung zulassen (openid)** aus.
1. Wählen Sie **Geheimnis für Webserver-Flow erforderlich** aus.
1. Klicken Sie auf **ID-Token konfigurieren**, und wählen Sie anschließend **Standardanforderungen einschließen** aus.
1. Klicken Sie auf **Speichern**.
1. Kopieren Sie die Werte für **Consumer Key** (Consumerschlüssel) und **Consumer Secret** (Consumergeheimnis). Diese beiden Angaben benötigen Sie, um Salesforce als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. Das **Clientgeheimnis** ist eine wichtige Anmeldeinformation.

::: zone pivot="b2c-user-flow"

## <a name="configure-a-salesforce-account-as-an-identity-provider"></a>Konfigurieren eines Salesforce-Kontos als Identitätsanbieter

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das den Azure AD B2C-Mandanten enthält. Wählen Sie im Hauptmenü den **Verzeichnis- und Abonnementfilter** aus, und wählen Sie das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Identitätsanbieter** und dann **Neuer OpenID Connect-Anbieter** aus.
1. Geben Sie einen **Namen** ein. Geben Sie z. B. *Salesforce* ein.
1. Geben Sie für **Metadata url** (Metadaten-URL) die folgende URL ein, und ersetzen Sie dabei `{org}` durch Ihre Salesforce-Organisation:

    ```
    https://{org}.my.salesforce.com/.well-known/openid-configuration
    ```

1. Geben Sie für **Client-ID** die zuvor notierte Anwendungs-ID ein.
1. Geben Sie im Feld **Geheimer Clientschlüssel** den zuvor notierten geheimen Clientschlüssel ein.
1. Geben Sie für den **Bereich** das `openid id profile email` ein.
1. Übernehmen Sie die Standardwerte für **Antworttyp** und **Antwortmodus**.
1. (Optional) Geben Sie als **Domänenhinweis** die Zeichenfolge `contoso.com` ein. Weitere Informationen finden Sie unter [Einrichten einer direkten Anmeldung mit Azure Active Directory B2C](direct-signin.md#redirect-sign-in-to-a-social-provider).
1. Wählen Sie unter **Zuordnung von Identitätsanbieteransprüchen** die folgenden Ansprüche aus:

    - **Benutzer-ID**: *Abonnement*
    - **Anzeigename**: *name*
    - **Vorname**: *given_name*
    - **Nachname**: *family_name*
    - **E-Mail**: *preferred_username*

1. Wählen Sie **Speichern** aus.
::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-policy-key"></a>Erstellen eines Richtlinienschlüssels

Sie müssen den geheimen Clientschlüssel speichern, den Sie zuvor in Ihrem Azure AD B2C-Mandanten notiert haben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Mandanten enthält.
3. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
4. Wählen Sie auf der Seite „Übersicht“ die Option **Framework für die Identitätsfunktion** aus.
5. Klicken Sie erst auf **Richtlinienschlüssel** und anschließend auf **Hinzufügen**.
6. Klicken Sie unter **Optionen** auf `Manual`.
7. Geben Sie einen **Namen** für den Richtlinienschlüssel ein. Beispiel: `SalesforceSecret`. Dem Namen Ihres Schlüssels wird automatisch das Präfix `B2C_1A_` hinzugefügt.
8. Geben Sie im Feld **Geheimnis** den geheimen Clientschlüssel ein, den Sie zuvor notiert haben.
9. Wählen Sie für **Schlüsselverwendung** die Option `Signature` aus.
10. Klicken Sie auf **Erstellen**.

## <a name="add-a-claims-provider"></a>Hinzufügen eines Anspruchsanbieters

Wenn Sie möchten, dass Benutzer sich mit einem Salesforce-Konto anmelden, müssen Sie das Konto als Anspruchsanbieter definieren, mit dem Azure AD B2C über einen Endpunkt kommunizieren kann. Der Endpunkt bietet eine Reihe von Ansprüchen, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde.

Sie können ein Salesforce-Konto als Anspruchsanbieter definieren, indem Sie es in der Erweiterungsdatei Ihrer Richtlinie dem **ClaimsProviders**-Element hinzufügen.

1. Öffnen Sie die Datei *TrustFrameworkExtensions.xml*.
2. Suchen Sie nach dem Element **ClaimsProviders**. Falls das Element nicht vorhanden sein sollte, fügen Sie es unter dem Stammelement hinzu.
3. Fügen Sie ein neues **ClaimsProvider**-Element wie folgt hinzu:

    ```xml
    <ClaimsProvider>
      <Domain>salesforce.com</Domain>
      <DisplayName>Salesforce</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Salesforce-OIDC">
          <DisplayName>Salesforce</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <!-- Update the {org} below to your Salesforce organization -->
            <Item Key="METADATA">https://{org}.my.salesforce.com/.well-known/openid-configuration</Item>
            <Item Key="response_types">code</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="scope">openid id profile email</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <!-- Update the Client ID below to the Application ID -->
            <Item Key="client_id">Your Salesforce application ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_SalesforceSecret"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="sub" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="salesforce.com" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Legen Sie für den **METADATA**-URI `{org}` mit Ihrer Salesforce-Organisation fest.
5. Legen Sie **client_id** auf die Anwendungs-ID aus der Anwendungsregistrierung fest.
6. Speichern Sie die Datei .

### <a name="upload-the-extension-file-for-verification"></a>Hochladen der Erweiterungsdatei zur Überprüfung

Nun haben Sie Ihre Richtlinie so konfiguriert, dass Azure AD B2C mit Ihrem Salesforce-Konto kommunizieren kann. Versuchen Sie, die Erweiterungsdatei Ihrer Richtlinie hochzuladen, um sich zu vergewissern, dass soweit keine Probleme vorliegen.

1. Wählen Sie in Ihrem Azure AD B2C-Mandanten auf der Seite **Benutzerdefinierte Richtlinien** die Option **Richtlinie hochladen** aus.
2. Aktivieren Sie **Richtlinie überschreiben, sofern vorhanden**, navigieren Sie dann zur Datei *TrustFrameworkExtensions.xml*, und wählen Sie die Datei aus.
3. Klicken Sie auf **Hochladen**.

## <a name="register-the-claims-provider"></a>Registrieren des Anspruchsanbieters

An dieser Stelle wurde der Identitätsanbieter eingerichtet, ist jedoch auf keinem der Registrierungs-/Anmeldebildschirme vorhanden. Um ihn verfügbar zu machen, erstellen Sie ein Duplikat einer vorhandenen User Journey-Vorlage und ändern diese dann so, dass sie ebenfalls das Salesforce-Konto als Identitätsanbieter aufweist.

1. Öffnen Sie die Datei *TrustFrameworkBase.xml* aus dem Starter Pack.
2. Suchen und kopieren Sie den gesamten Inhalt des **UserJourney**-Elements, das `Id="SignUpOrSignIn"` enthält.
3. Öffnen Sie die Datei *TrustFrameworkExtensions.xml*, und suchen Sie nach dem **UserJourneys**-Element. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
4. Fügen Sie den gesamten Inhalt des kopierten **UserJourney**-Element als untergeordnetes Element des **UserJourneys**-Elements ein.
5. Benennen Sie die ID der User Journey um. Beispiel: `SignUpSignInSalesforce`.

### <a name="display-the-button"></a>Anzeigen der Schaltfläche

Das **ClaimsProviderSelection**-Element entspricht einer Schaltfläche für einen Identitätsanbieter auf einem Registrierungs- oder Anmeldebildschirm. Wenn Sie ein **ClaimsProviderSelection**-Element für ein Salesforce-Konto hinzufügen, wird eine neue Schaltfläche angezeigt, wenn ein Benutzer die Seite aufruft.

1. Suchen Sie nach dem **OrchestrationStep**-Element, das `Order="1"` in der User Journey enthält, die Sie erstellt haben.
2. Fügen Sie unter **ClaimsProviderSelects** das folgende Element hinzu. Legen Sie den Wert von **TargetClaimsExchangeId** auf einen geeigneten Wert (z.B. auf `SalesforceExchange`) fest:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="SalesforceExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion

Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall soll bei der Aktion Azure AD B2C mit dem Salesforce-Konto kommunizieren, um ein Token zu empfangen.

1. Suchen Sie nach dem **OrchestrationStep**-Element, das `Order="2"` in der User Journey enthält.
2. Fügen Sie das folgende **ClaimsExchange**-Element hinzu, und stellen Sie dabei sicher, dass Sie den gleichen Wert für die ID verwenden, den Sie für **TargetClaimsExchangeId** verwendet haben:

    ```xml
    <ClaimsExchange Id="SalesforceExchange" TechnicalProfileReferenceId="Salesforce-OIDC" />
    ```

    Ändern Sie den Wert von **TechnicalProfileReferenceId** in die ID des technischen Profils, das Sie zuvor erstellt haben. Beispiel: `Salesforce-OIDC`.

3. Speichern Sie die Datei *TrustFrameworkExtensions.xml*, und laden Sie die Datei zur Überprüfung erneut hoch.

::: zone-end

::: zone pivot="b2c-user-flow"

## <a name="add-salesforce-identity-provider-to-a-user-flow"></a>Hinzufügen des Salesforce-Identitätsanbieters zu einem Benutzerflow 

1. Wählen Sie in Ihrem Azure AD B2C-Mandanten die Option **Benutzerflows** aus.
1. Klicken Sie auf den Benutzerflow, dem Sie den Salesforce-Identitätsanbieter hinzufügen möchten.
1. Wählen Sie unter **Social identity providers** (Soziales Netzwerk als Identitätsanbieter) die Option **Salesforce** aus.
1. Wählen Sie **Speichern** aus.
1. Um die Richtlinie zu testen, wählen Sie **Benutzerflow ausführen** aus.
1. Wählen Sie für **Anwendung** die Webanwendung *testapp1* aus, die Sie zuvor registriert haben. Als **Antwort-URL** sollte `https://jwt.ms` angezeigt werden.
1. Klicken Sie auf **Benutzerflow ausführen**.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="update-and-test-the-relying-party-file"></a>Aktualisieren und Testen der Datei der vertrauenden Seite

Aktualisieren Sie als Nächstes die Datei der vertrauenden Seite, mit der die erstellte User Journey initiiert wird.

1. Erstellen Sie in Ihrem Arbeitsverzeichnis eine Kopie der Datei *SignUpOrSignIn.xml*, und benennen Sie sie um. Benennen Sie die Datei z.B. in *SignUpSignInSalesforce.xml* um.
1. Öffnen Sie die neue Datei, und aktualisieren Sie den Wert des Attributs **PolicyId** für **TrustFrameworkPolicy** mit einem eindeutigen Wert. Beispiel: `SignUpSignInSalesforce`.
1. Aktualisieren Sie den Wert von **PublicPolicyUri** mit dem URI für die Richtlinie. Beispiel: `http://contoso.com/B2C_1A_signup_signin_Salesforce`
1. Aktualisieren Sie den Wert des Attributs **ReferenceId** in **DefaultUserJourney** so, dass er mit der ID der neuen, von Ihnen erstellten User Journey übereinstimmt (SignUpSignSalesforce).
1. Speichern Sie die Änderungen, und laden Sie die Datei hoch.
1. Wählen Sie unter **Benutzerdefinierte Richtlinien** die Richtlinie **B2C_1A_signup_signin** aus.
1. Wählen Sie für **Anwendung auswählen** die Webanwendung *testapp1* aus, die Sie zuvor registriert haben. Als **Antwort-URL** sollte `https://jwt.ms` angezeigt werden.
1. Klicken Sie auf **Jetzt ausführen** und dann auf „Salesforce“, um sich mit Salesforce anzumelden und die benutzerdefinierte Richtlinie zu testen.

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich darüber, wie Sie das [Salesforce-Token an Ihre Anwendung übergeben](idp-pass-through-user-flow.md).
