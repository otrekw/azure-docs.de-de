---
title: Hinzufügen von Ansprüchen und Anpassen von Benutzereingaben in benutzerdefinierten Richtlinien
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie Benutzereingaben anpassen und Ansprüche zur User Journey bei Registrierung bzw. Anmeldung in Azure Active Directory B2C hinzufügen.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 56a3478f1c0dbc05eba07a5109f5bb6ba89b79d0
ms.sourcegitcommit: 72c2da0def8aa7ebe0691612a89bb70cd0c5a436
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79079890"
---
#  <a name="add-claims-and-customize-user-input-using-custom-policies-in-azure-active-directory-b2c"></a>Hinzufügen von Ansprüchen und Anpassen von Benutzereingaben mit benutzerdefinierten Richtlinien in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel erfahren Sie, wie Sie während der User Journey für die Registrierung in Azure Active Directory B2C (Azure AD B2C) ein neues Attribut sammeln. Sie erhalten den Ort des Benutzers, konfigurieren diesen als Dropdownoption und legen fest, ob er bereitgestellt werden muss.

Sie können die anfänglichen Daten von Ihren Benutzern mithilfe der User Journey für die Registrierung bzw. Anmeldung sammeln. Weitere Ansprüche können später mit einer User Journey für die Profilbearbeitung gesammelt werden. Jedes Mal, wenn Azure AD B2C auf interaktive Weise Informationen direkt vom Benutzer sammelt, verwendet das Identity Experience Framework diese als [selbstbestätigtes technisches Profil](self-asserted-technical-profile.md). In diesem Beispiel führen Sie folgende Schritte aus:

1. Sie definieren den Anspruch „Ort“.
1. Sie fragen den Benutzer nach seinem Ort.
1. Sie speichern den Ort dauerhaft im Benutzerprofil im Azure AD B2C-Verzeichnis.
1. Sie lesen bei jeder Anmeldung den Anspruch „Ort“ aus dem Azure AD B2C-Verzeichnis.
1. Sie geben nach der Anmeldung oder Registrierung den Ort an die Anwendung der vertrauenden Seite zurück.  

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte unter [Erste Schritte mit benutzerdefinierten Richtlinien in Azure Active Directory B2C](custom-policy-get-started.md) aus. Sie sollten eine funktionierende benutzerdefinierte Richtlinie für die Registrierung und Anmeldung mit Social Media- und lokalen Konten haben.

## <a name="define-a-claim"></a>Definieren eines Anspruchs

Ein Anspruch stellt eine temporäre Speicherung von Daten während der Ausführung einer Azure AD B2C-Richtlinie bereit. Im [Anspruchsschema](claimsschema.md) deklarieren Sie Ihre Ansprüche. Die folgenden Elemente werden zum Definieren des Anspruchs verwendet:

- **DisplayName**: Zeichenfolge, mit der die Bezeichnung für den Benutzer definiert wird.
- [DataType](claimsschema.md#datatype): Typ des Anspruchs.
- **UserHelpText**: Verdeutlicht dem Benutzer, was erforderlich ist.
- [UserInputType](claimsschema.md#userinputtype): Typ der Eingabesteuerung (z. B. Textfeld, Optionsfeld, Dropdownliste oder Mehrfachauswahl).

Öffnen Sie die Erweiterungsdatei Ihrer Richtlinie. Beispiel: <em>`SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** </em>.

1. Suchen Sie nach dem Element [BuildingBlocks](buildingblocks.md). Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
1. Suchen Sie nach dem Element [ClaimsSchema](claimsschema.md). Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
1. Fügen Sie dem Element **ClaimsSchema-** Element den Anspruch „Ort“ hinzu.  

```xml
<ClaimType Id="city">
  <DisplayName>City where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

## <a name="add-a-claim-to-the-user-interface"></a>Hinzufügen eines Anspruchs zur Benutzeroberfläche

Die folgenden technischen Profile sind [selbstbestätigt](self-asserted-technical-profile.md) und werden aufgerufen, wenn von einem Benutzer eine Eingabe erwartet wird:

- **LocalAccountSignUpWithLogonEmail**: Registrierungsflow mit lokalem Konto.
- **SelfAsserted-Social**: Erstmalige Benutzeranmeldung mit Verbundkonto.
- **SelfAsserted-ProfileUpdate**: Profilbearbeitungsflow.

Um den Anspruch „Ort“ bei der Registrierung zu sammeln, muss er dem technischen Profil `LocalAccountSignUpWithLogonEmail` als Ausgabeanspruch hinzugefügt werden. Überschreiben Sie dieses technische Profil in der Erweiterungsdatei. Geben Sie die gesamte Liste von Ausgabeansprüchen an, um die Reihenfolge zu steuern, in der die Ansprüche auf dem Bildschirm angezeigt werden. Suchen Sie nach dem Element **ClaimsProviders**. Fügen Sie ein neues Element „ClaimsProvider“ wie folgt hinzu:

```xml
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <!--Local account sign-up page-->
    <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
      <OutputClaims>
       <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
       <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
       <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
       <OutputClaim ClaimTypeReferenceId="displayName" />
       <OutputClaim ClaimTypeReferenceId="givenName" />
       <OutputClaim ClaimTypeReferenceId="surName" />
       <OutputClaim ClaimTypeReferenceId="city"/>
     </OutputClaims>
   </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
<ClaimsProvider>
```

Zum Sammeln des Anspruchs „Ort“ nach der erstmaligen Anmeldung mit einem Verbundkonto muss er dem technischen Profil `SelfAsserted-Social` als Ausgabeanspruch hinzugefügt werden. Damit Benutzer von lokalen und Verbundkonten Ihre Profildaten später bearbeiten können, fügen Sie den Ausgabeanspruch dem technischen Profil `SelfAsserted-ProfileUpdate` hinzu. Überschreiben Sie diese technischen Profile in der Erweiterungsdatei. Geben Sie die gesamte Liste von Ausgabeansprüchen an, um die Reihenfolge zu steuern, in der die Ansprüche auf dem Bildschirm angezeigt werden. Suchen Sie nach dem Element **ClaimsProviders**. Fügen Sie ein neues Element „ClaimsProvider“ wie folgt hinzu:

```xml
  <DisplayName>Self Asserted</DisplayName>
  <TechnicalProfiles>
    <!--Federated account first-time sign-in page-->
    <TechnicalProfile Id="SelfAsserted-Social">
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName"/>
        <OutputClaim ClaimTypeReferenceId="givenName"/>
        <OutputClaim ClaimTypeReferenceId="surname"/>
        <OutputClaim ClaimTypeReferenceId="city"/>
      </OutputClaims>
    </TechnicalProfile>
    <!--Edit profile page-->
    <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName"/>
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="city"/>
      </OutputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="read-and-write-a-claim"></a>Lesen und Schreiben eines Anspruchs

Die folgenden technischen Profile sind [technische Active Directory-Profile](active-directory-technical-profile.md), die Daten in Azure Active Directory schreiben und daraus lesen.  
Verwenden Sie `PersistedClaims`, um Daten in das Benutzerprofil zu schreiben und `OutputClaims`, um innerhalb der entsprechenden technischen Active Directory-Profile Daten aus dem Benutzerprofil zu lesen.

Überschreiben Sie diese technischen Profile in der Erweiterungsdatei. Suchen Sie nach dem Element **ClaimsProviders**.  Fügen Sie ein neues Element „ClaimsProvider“ wie folgt hinzu:

```xml
<ClaimsProvider>
  <DisplayName>Azure Active Directory</DisplayName>
  <TechnicalProfiles>
    <!-- Write data during a local account sign-up flow. -->
    <TechnicalProfile Id="AAD-UserWriteUsingLogonEmail">
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="city"/>
      </PersistedClaims>
    </TechnicalProfile>
    <!-- Write data during a federated account first-time sign-in flow. -->
    <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="city"/>
      </PersistedClaims>
    </TechnicalProfile>
    <!-- Write data during edit profile flow. -->
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="city"/>
      </PersistedClaims>
    </TechnicalProfile>
    <!-- Read data after user authenticates with a local account. -->
    <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
      <OutputClaims>  
        <OutputClaim ClaimTypeReferenceId="city" />
      </OutputClaims>
    </TechnicalProfile>
    <!-- Read data after user authenticates with a federated account. -->
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      <OutputClaims>  
        <OutputClaim ClaimTypeReferenceId="city" />
      </OutputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="include-a-claim-in-the-token"></a>Einschließen eines Anspruchs in das Token 

Um den Anspruch „Ort“ an die Anwendung der vertrauenden Seite zurückzugeben, fügen Sie der Datei <em>`SocialAndLocalAccounts/` **`SignUpOrSignIn.xml`** </em> einen Ausgabeanspruch hinzu. Der Ausgabeanspruch wird nach einer erfolgreichen User Journey in das Token eingefügt und an die Anwendung gesendet. Ändern Sie im Abschnitt für die vertrauende Seite das Element des technischen Profils, um den Ort als Ausgabeanspruch hinzuzufügen.
 
```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surname" />
      <OutputClaim ClaimTypeReferenceId="email" />
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="identityProvider" />
      <OutputClaim ClaimTypeReferenceId="tenantId" AlwaysUseDefaultValue="true" DefaultValue="{Policy:TenantObjectId}" />
      <OutputClaim ClaimTypeReferenceId="city" DefaultValue="" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```

## <a name="test-the-custom-policy"></a>Testen der benutzerdefinierten Richtlinie

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD-Mandanten enthält, indem Sie im oberen Menü den **Verzeichnis- und Abonnementfilter** und dann das Verzeichnis auswählen, das Ihren Azure AD-Mandanten enthält.
3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **App-Registrierungen**, und wählen Sie dann diese Option aus.
4. Wählen Sie **Framework für die Identitätsfunktion** aus.
5. Wählen Sie **Benutzerdefinierte Richtlinie hochladen** aus, und laden Sie dann die beiden geänderten Richtliniendateien hoch.
2. Wählen Sie die hochgeladene Registrierungs- oder Anmelderichtlinie aus, und klicken Sie auf die Schaltfläche **Jetzt ausführen**.
3. Sie sollten sich mit einer E-Mail-Adresse registrieren können.

Der Registrierungsbildschirm sollte ähnlich wie im folgenden Screenshot aussehen:

![Screenshot der geänderten Registrierungsoption](./media/custom-policy-configure-user-input/signup-with-city-claim-dropdown-example.png)

Das Token, das an die Anwendung zurückgesendet wird, enthält den Anspruch `city`.

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
}.{
  "exp": 1583500140,
  "nbf": 1583496540,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
  "acr": "b2c_1a_signup_signin",
  "nonce": "defaultNonce",
  "iat": 1583496540,
  "auth_time": 1583496540,
  "name": "Emily Smith",
  "email": "joe@outlook.com",
  "given_name": "Emily",
  "family_name": "Smith",
  "city": "Bellevue"
  ...
}
```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das Element [ClaimsSchema](claimsschema.md) in der IEF-Referenz.
- Lesen Sie [Verwenden von benutzerdefinierten Attributen in einer benutzerdefinierten Richtlinie für die Profilbearbeitung](custom-policy-custom-attributes.md).
