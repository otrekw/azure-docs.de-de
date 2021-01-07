---
title: Einrichten der Registrierung und Anmeldung mit einem ID.me-Konto
titleSuffix: Azure AD B2C
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit ID.me-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/17/2020
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 6877a82d81b46ac2ffda8d0f0bf3fb0d7b337ff3
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97663516"
---
# <a name="set-up-sign-up-and-sign-in-with-a-idme-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem ID.me-Konto mithilfe von Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-user-flow"

[!INCLUDE [active-directory-b2c-limited-to-custom-policy](../../includes/active-directory-b2c-limited-to-custom-policy.md)]

::: zone-end

::: zone pivot="b2c-custom-policy"


## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [active-directory-b2c-customization-prerequisites-custom-policy](../../includes/active-directory-b2c-customization-prerequisites-custom-policy.md)]


## <a name="create-an-idme-application"></a>Erstellen eines ID.me-Kontos

Um ein ID.me-Konto als Identitätsanbieter in Azure Active Directory B2C (Azure AD B2C) verwenden zu können, müssen Sie in den [ID.me-Entwicklerressourcen für API und SDK](https://developers.id.me/) eine Anwendung erstellen. Wenn Sie noch kein ID.me-Entwicklerkonto haben, können Sie sich auf [https://developers.id.me/registration/new](https://developers.id.me/registration/new) registrieren.

1. Melden Sie sich mit den Anmeldeinformationen Ihres ID.me-Kontos bei den [ID.me-Entwicklerressourcen für API und SDK](https://developers.id.me/) an.
1. Wählen Sie **Meine Anwendungen anzeigen** und dann **Weiter** aus.
1. Wählen Sie **Neu erstellen** aus.
    1. Geben Sie einen **Namen** und einen **Anzeigenamen** ein.
    1. Geben Sie `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` als **Umleitungs-URI** ein. Ersetzen Sie `your-tenant-name` durch den Namen Ihres Mandanten. 
1. Klicken Sie auf **Continue**(Weiter).
1. Kopieren Sie die Werte für **Client ID** und **Client Secret**. Sie benötigen beide Angaben, um den Identitätsanbieter Ihrem Mandanten hinzuzufügen.

## <a name="create-a-policy-key"></a>Erstellen eines Richtlinienschlüssels

Sie müssen den geheimen Clientschlüssel speichern, den Sie zuvor in Ihrem Azure AD B2C-Mandanten notiert haben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Mandanten enthält.
3. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
4. Wählen Sie auf der Seite „Übersicht“ die Option **Framework für die Identitätsfunktion** aus.
5. Klicken Sie erst auf **Richtlinienschlüssel** und anschließend auf **Hinzufügen**.
6. Klicken Sie unter **Optionen** auf `Manual`.
7. Geben Sie einen **Namen** für den Richtlinienschlüssel ein. Beispiel: `IdMeSecret`. Dem Namen Ihres Schlüssels wird automatisch das Präfix `B2C_1A_` hinzugefügt.
8. Geben Sie im Feld **Geheimnis** den geheimen Clientschlüssel ein, den Sie zuvor notiert haben.
9. Wählen Sie für **Schlüsselverwendung** die Option `Signature` aus.
10. Klicken Sie auf **Erstellen**.

## <a name="add-a-claims-provider"></a>Hinzufügen eines Anspruchsanbieters

Wenn Sie möchten, dass sich Benutzer mit einem ID.me-Konto anmelden, müssen Sie das Konto als Anspruchsanbieter definieren, mit dem Azure AD B2C über einen Endpunkt kommunizieren kann. Der Endpunkt bietet eine Reihe von Ansprüchen, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde.

Sie können ein ID.me-Konto als Anspruchsanbieter definieren, indem Sie es in der Erweiterungsdatei Ihrer Richtlinie dem **ClaimsProviders**-Element hinzufügen.

1. Öffnen Sie die Datei *TrustFrameworkExtensions.xml*.
2. Suchen Sie nach dem Element **ClaimsProviders**. Falls das Element nicht vorhanden sein sollte, fügen Sie es unter dem Stammelement hinzu.
3. Fügen Sie ein neues **ClaimsProvider**-Element wie folgt hinzu:

    ```xml
    <ClaimsProvider>
      <Domain>id.me</Domain>
      <DisplayName>ID.me</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="IdMe-OAUTH2">
          <DisplayName>IdMe</DisplayName>
          <Protocol Name="OAuth2" />
          <Metadata>
            <Item Key="ProviderName">api.id.me</Item>
            <Item Key="authorization_endpoint">https://api.id.me/oauth/authorize</Item>
            <Item Key="AccessTokenEndpoint">https://api.id.me/oauth/token</Item>
            <Item Key="ClaimsEndpoint">https://api.id.me/api/public/v2/attributes.json</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="scope">openid alumni</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <!-- Update the Client ID below to the Application ID -->
            <Item Key="client_id">Your ID.me application ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_IdMeSecret"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="uuid" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="fname" />
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lname" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="me.id.com" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateDisplayNameFromFirstNameAndLastName" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Legen Sie **client_id** auf die Anwendungs-ID aus der Anwendungsregistrierung fest.
5. Speichern Sie die Datei .

### <a name="add-the-claims-transformations"></a>Hinzufügen von Ansprüchen von Transformationen

Als Nächstes benötigen zum Erstellen des displayName-Anspruchs eine Anspruchstransformation. Fügen Sie im `<ClaimsTransformations>`-Element in `<BuildingBlocks>` die folgenden Anspruchstransformation hinzu. 

```xml
 <ClaimsTransformations>
  <ClaimsTransformation Id="CreateDisplayNameFromFirstNameAndLastName" TransformationMethod="FormatStringMultipleClaims">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="inputClaim1" />
      <InputClaim ClaimTypeReferenceId="surName" TransformationClaimType="inputClaim2" />
    </InputClaims>
    <InputParameters>
      <InputParameter Id="stringFormat" DataType="string" Value="{0} {1}" />
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="displayName" TransformationClaimType="outputClaim" />
    </OutputClaims>
   </ClaimsTransformation>
</ClaimsTransformations>
```

### <a name="upload-the-extension-file-for-verification"></a>Hochladen der Erweiterungsdatei zur Überprüfung

Nun haben Sie Ihre Richtlinie so konfiguriert, dass Azure AD B2C mit Ihrem ID.me-Konto kommunizieren kann. Versuchen Sie, die Erweiterungsdatei Ihrer Richtlinie hochzuladen, um sich zu vergewissern, dass soweit keine Probleme vorliegen.

1. Wählen Sie in Ihrem Azure AD B2C-Mandanten auf der Seite **Benutzerdefinierte Richtlinien** die Option **Richtlinie hochladen** aus.
2. Aktivieren Sie **Richtlinie überschreiben, sofern vorhanden**, navigieren Sie dann zur Datei *TrustFrameworkExtensions.xml*, und wählen Sie die Datei aus.
3. Klicken Sie auf **Hochladen**.

## <a name="register-the-claims-provider"></a>Registrieren des Anspruchsanbieters

An dieser Stelle wurde der Identitätsanbieter eingerichtet, ist jedoch auf keinem der Registrierungs-/Anmeldebildschirme vorhanden. Um ihn verfügbar zu machen, erstellen Sie ein Duplikat einer vorhandenen User Journey-Vorlage und ändern diese dann so, dass sie auch ID.me als Identitätsanbieter aufweist.

1. Öffnen Sie die Datei *TrustFrameworkBase.xml* aus dem Starter Pack.
2. Suchen und kopieren Sie den gesamten Inhalt des **UserJourney**-Elements, das `Id="SignUpOrSignIn"` enthält.
3. Öffnen Sie die Datei *TrustFrameworkExtensions.xml*, und suchen Sie nach dem **UserJourneys**-Element. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
4. Fügen Sie den gesamten Inhalt des kopierten **UserJourney**-Element als untergeordnetes Element des **UserJourneys**-Elements ein.
5. Benennen Sie die ID der User Journey um. Beispiel: `SignUpSignInIdMe`.

### <a name="display-the-button"></a>Anzeigen der Schaltfläche

Das **ClaimsProviderSelection**-Element entspricht einer Schaltfläche für einen Identitätsanbieter auf einem Registrierungs- oder Anmeldebildschirm. Wenn Sie ein **ClaimsProviderSelection**-Element für ein ID.me-Konto hinzufügen, wird eine neue Schaltfläche angezeigt, wenn ein Benutzer zu der Seite gelangt.

1. Suchen Sie nach dem **OrchestrationStep**-Element, das `Order="1"` in der User Journey enthält, die Sie erstellt haben.
2. Fügen Sie unter **ClaimsProviderSelects** das folgende Element hinzu. Legen Sie den Wert von **TargetClaimsExchangeId** auf einen geeigneten Wert (z.B. auf `IdMeExchange`) fest:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="IdMeExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion

Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall soll bei der Aktion Azure AD B2C mit dem ID.me-Konto kommunizieren, um ein Token zu empfangen.

1. Suchen Sie nach dem **OrchestrationStep**-Element, das `Order="2"` in der User Journey enthält.
2. Fügen Sie das folgende **ClaimsExchange**-Element hinzu, und stellen Sie dabei sicher, dass Sie den gleichen Wert für die ID verwenden, den Sie für **TargetClaimsExchangeId** verwendet haben:

    ```xml
    <ClaimsExchange Id="IdMeExchange" TechnicalProfileReferenceId="IdMe-OAUTH2" />
    ```

    Ändern Sie den Wert von **TechnicalProfileReferenceId** in die ID des technischen Profils, das Sie zuvor erstellt haben. Beispiel: `IdMe-OAUTH2`.

3. Speichern Sie die Datei *TrustFrameworkExtensions.xml*, und laden Sie die Datei zur Überprüfung erneut hoch.

## <a name="update-and-test-the-relying-party-file"></a>Aktualisieren und Testen der Datei der vertrauenden Seite

Aktualisieren Sie als Nächstes die Datei der vertrauenden Seite, mit der die erstellte User Journey initiiert wird.

1. Erstellen Sie in Ihrem Arbeitsverzeichnis eine Kopie der Datei *SignUpOrSignIn.xml*, und benennen Sie sie um. Benennen Sie die Datei um, z. B. in *SignUpSignInIdMe.xml*.
1. Öffnen Sie die neue Datei, und aktualisieren Sie den Wert des Attributs **PolicyId** für **TrustFrameworkPolicy** mit einem eindeutigen Wert. Beispiel: `SignUpSignIdMe`.
1. Aktualisieren Sie den Wert von **PublicPolicyUri** mit dem URI für die Richtlinie. Beispiel: `http://contoso.com/B2C_1A_signup_signin_IdMe`
1. Ändern Sie den Wert des Attributs **ReferenceId** im Element **DefaultUserJourney** so, dass er der ID der neuen User Journey entspricht, die Sie erstellt haben (SignUpSignInIdMe).
1. Speichern Sie die Änderungen, und laden Sie die Datei hoch.
1. Wählen Sie unter **Benutzerdefinierte Richtlinien** die Richtlinie **B2C_1A_signup_signin** aus.
1. Wählen Sie für **Anwendung auswählen** die Webanwendung *testapp1* aus, die Sie zuvor registriert haben. Als **Antwort-URL** sollte `https://jwt.ms` angezeigt werden.
1. Wählen Sie **Jetzt ausführen** und dann „ID.me“ aus, um sich mit ID.me anzumelden und die benutzerdefinierte Richtlinie zu testen.

::: zone-end