---
title: „Angemeldet bleiben“ in Azure Active Directory B2C
description: Erfahren Sie, wie Sie die Option „Angemeldet bleiben“ in Azure Active Directory B2C festlegen.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: de5dd051804f3a0a7d1b0d32b998262af13e8926
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85389189"
---
# <a name="enable-keep-me-signed-in-kmsi-in-azure-active-directory-b2c"></a>Aktivieren von „Angemeldet bleiben“ in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Sie können die Funktion „Angemeldet bleiben“ (Keep Me Signed In, KMSI) für Benutzer Ihrer webbasierten und nativen Anwendungen aktivieren, die über lokale Konten in Ihrem Azure AD B2C-Verzeichnis (Azure Active Directory B2C) verfügen. Dieses Feature gewährt wiederkehrenden Benutzern Zugriff auf Ihre Anwendung, ohne dass sie zur erneuten Eingabe von Benutzername und Kennwort aufgefordert werden. Dieser Zugriff wird widerrufen, wenn sich der Benutzer abmeldet.

Benutzer sollten diese Option nicht auf öffentlichen Computern aktivieren.

![Beispiel für eine Seite zur Registrierung/Anmeldung mit dem Kontrollkästchen „Angemeldet bleiben“](./media/custom-policy-keep-me-signed-in/kmsi.PNG)

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure AD B2C-Mandant, der die Anmeldung lokaler Konten zulässt. KMSI wird für externe Identitätsanbieterkonten nicht unterstützt.
- Führen Sie die Schritte unter [Erste Schritte mit benutzerdefinierten Richtlinien in Azure Active Directory B2C](custom-policy-get-started.md) aus.

## <a name="configure-the-page-identifier"></a>Konfigurieren des Seitenbezeichners

Legen Sie zum Aktivieren von KMSI das ContentDefinitions-Element `DataUri` auf [Seitenbezeichner](contentdefinitions.md#datauri) `unifiedssp` und die [Seitenversion](page-layout.md) auf *1.1.0* oder höher fest.

1. Öffnen Sie die Erweiterungsdatei Ihrer Richtlinie. Beispiel: <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em>. Die Erweiterungsdatei ist eine der Richtliniendateien im Starter Pack für benutzerdefinierte Richtlinien, das Sie im unter „Voraussetzungen“ genannten Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](custom-policy-get-started.md) abgerufen haben sollten.
1. Suchen Sie nach dem Element **BuildingBlocks**. Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
1. Fügen Sie das **ContentDefinitions**-Element dem Element **BuildingBlocks** der Richtlinie hinzu.

    Ihre benutzerdefinierte Richtlinie sollte wie der folgende Codeausschnitt aussehen:

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0</DataUri>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

## <a name="add-the-metadata-to-the-self-asserted-technical-profile"></a>Hinzufügen der Metadaten zum selbstbestätigten technischen Profil

Um das Kontrollkästchen KMSI zur Registrierungs-und Anmeldeseite hinzuzufügen, legen Sie die `setting.enableRememberMe`-Metadaten auf TRUE fest. Überschreiben Sie in der Erweiterungsdatei die technischen Profile in SelfAsserted-LocalAccountSignin-Email.

1. Suchen Sie das ClaimsProviders-Element. Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
1. Fügen Sie dem ClaimsProviders-Element die folgenden Anspruchsanbieter hinzu:

```xml
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
      <Metadata>
        <Item Key="setting.enableRememberMe">True</Item>
      </Metadata>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

1. Speichern Sie die Erweiterungsdatei.

## <a name="configure-a-relying-party-file"></a>Konfigurieren einer Datei der vertrauenden Seite

Aktualisieren Sie als Nächstes die Datei der vertrauenden Seite, mit der die erstellte User Journey initiiert wird.

1. Öffnen Sie Ihre benutzerdefinierte Richtliniendatei. Beispiel: *SignUpOrSignin.xml*.
1. Fügen Sie dem Knoten `<RelyingParty>` einen untergeordneten Knoten `<UserJourneyBehaviors>` hinzu, falls dieser noch nicht vorhanden ist. Er muss direkt nach `<DefaultUserJourney ReferenceId="User journey Id" />` angeordnet werden. Beispiel: `<DefaultUserJourney ReferenceId="SignUpOrSignIn" />`.
1. Fügen Sie den folgenden Knoten als untergeordnetes Element des `<UserJourneyBehaviors>`-Elements hinzu.

    ```xml
    <UserJourneyBehaviors>
      <SingleSignOn Scope="Tenant" KeepAliveInDays="30" />
      <SessionExpiryType>Absolute</SessionExpiryType>
      <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
    </UserJourneyBehaviors>
    ```

    - **SessionExpiryType**: Gibt an, wie die Sitzung durch die in `SessionExpiryInSeconds` und `KeepAliveInDays` angegebene Zeit verlängert wird. Der Wert `Rolling` (Standardwert) gibt an, dass die Sitzung jedes Mal verlängert wird, wenn der Benutzer eine Authentifizierung vornimmt. Der Wert `Absolute` gibt an, dass der Benutzer nach dem angegebenen Zeitraum zur erneuten Authentifizierung gezwungen wird.

    - **SessionExpiryInSeconds**: Die Lebensdauer der Sitzungscookies, wenn *Angemeldet bleiben* nicht aktiviert ist bzw. ein Benutzer *Angemeldet bleiben* nicht aktiviert. Die Sitzung wird nach Ablauf des unter `SessionExpiryInSeconds` angegebenen Werts beendet, oder der Browser wird geschlossen.

    - **KeepAliveInDays**: Die Lebensdauer der Sitzungscookies, wenn *Angemeldet bleiben* aktiviert ist bzw. der Benutzer *Angemeldet bleiben* aktiviert.  Der Wert von `KeepAliveInDays` hat Vorrang vor dem Wert `SessionExpiryInSeconds` und gibt den Ablaufzeitpunkt für die Sitzung vor. Auch wenn ein Benutzer den Browser schließt und später erneut öffnet, kann er sich im Hintergrund anmelden, solange dies innerhalb des unter „KeepAliveInDays“ angegebenen Zeitraums erfolgt.

    Weitere Informationen finden Sie unter [Verhalten der User Journey](relyingparty.md#userjourneybehaviors).

Es wird empfohlen, den Wert von „SessionExpiryInSeconds“ auf einen kurzen Zeitraum (1200 Sekunden) und den Wert von „KeepAliveInDays“ auf einen relativ langen Zeitraum (30 Tage) festzulegen. Dies ist im folgenden Beispiel dargestellt:

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <UserJourneyBehaviors>
    <SingleSignOn Scope="Tenant" KeepAliveInDays="30" />
    <SessionExpiryType>Absolute</SessionExpiryType>
    <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
  </UserJourneyBehaviors>
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
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```

## <a name="test-your-policy"></a>Testen Ihrer Richtlinie

1. Speichern Sie die Änderungen, und laden Sie die Datei dann hoch.
1. Um die benutzerdefinierte Richtlinie zu testen, die Sie hochgeladen haben, wechseln Sie im Azure-Portal zur Seite „Richtlinie“, und wählen Sie dann **Jetzt ausführen** aus.
1. Geben Sie Ihren **Benutzernamen** und Ihr **Kennwort** ein. Aktivieren Sie **Angemeldet bleiben**, und klicken Sie dann auf **Anmelden**.
1. Wechseln Sie zurück zum Azure-Portal. Wechseln Sie zur Seite „Richtlinie“, und wählen Sie dann **Kopieren** aus, um die Anmelde-URL zu kopieren.
1. Entfernen Sie in der Adressleiste des Browsers den Abfragezeichenfolgen-Parameter `&prompt=login`, der den Benutzer zur Eingabe seiner Anmeldeinformationen für diese Anforderung zwingt.
1. Klicken Sie im Browser auf **Los**. Azure AD B2C stellt nun ein Zugriffstoken aus, ohne Sie zur erneuten Anmeldung aufzufordern. 

## <a name="next-steps"></a>Nächste Schritte

Die Beispielrichtlinie finden Sie [hier](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).
