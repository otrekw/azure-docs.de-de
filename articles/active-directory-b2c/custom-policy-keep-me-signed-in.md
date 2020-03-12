---
title: „Angemeldet bleiben“ in Azure Active Directory B2C
description: Erfahren Sie, wie Sie die Option „Angemeldet bleiben“ in Azure Active Directory B2C festlegen.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/27/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 9a27487fa69888b02883c3d9a2151887f41afc45
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78189377"
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

1. Öffnen Sie die Erweiterungsdatei Ihrer Richtlinie. Beispiel: <em>`SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** </em>. Die Erweiterungsdatei ist eine der Richtliniendateien im Starter Pack für benutzerdefinierte Richtlinien, das Sie im unter „Voraussetzungen“ genannten Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](custom-policy-get-started.md) abgerufen haben sollten.
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

1. Speichern Sie die Erweiterungsdatei.



## <a name="configure-a-relying-party-file"></a>Konfigurieren einer Datei der vertrauenden Seite

Aktualisieren Sie als Nächstes die Datei der vertrauenden Seite, mit der die erstellte User Journey initiiert wird.

1. Öffnen Sie Ihre benutzerdefinierte Richtliniendatei. Beispiel: *SignUpOrSignin.xml*.
1. Fügen Sie dem Knoten `<RelyingParty>` einen untergeordneten Knoten `<UserJourneyBehaviors>` hinzu, falls dieser noch nicht vorhanden ist. Er muss direkt nach `<DefaultUserJourney ReferenceId="User journey Id" />` angeordnet werden. Beispiel: `<DefaultUserJourney ReferenceId="SignUpOrSignIn" />`.
1. Fügen Sie den folgenden Knoten als untergeordnetes Element des `<UserJourneyBehaviors>`-Elements hinzu.

    ```XML
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

```XML
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

4. Speichern Sie Ihre Änderungen, und laden Sie die Datei dann hoch.
5. Um die benutzerdefinierte Richtlinie zu testen, die Sie hochgeladen haben, wechseln Sie im Azure-Portal zur Richtlinienseite, und klicken Sie dann auf **Jetzt ausführen**.

Die Beispielrichtlinie finden Sie [hier](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).
