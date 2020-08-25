---
title: Verwalten von SSO und der Tokenanpassung mit benutzerdefinierten Richtlinien
titleSuffix: Azure AD B2C
description: Hier erhalten Sie Informationen zum Verwalten von SSO und der Tokenanpassung mit benutzerdefinierten Richtlinien in Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 9f3cd5c3280308f6da15a52361857fa02567d595
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88505460"
---
# <a name="manage-sso-and-token-customization-using-custom-policies-in-azure-active-directory-b2c"></a>Verwalten von SSO und der Tokenanpassung mit benutzerdefinierten Richtlinien in Azure Active Directory B2C

Dieser Artikel enthält Informationen zum Verwalten der Token-, Sitzungs- und SSO-Konfiguration (Single Sign-On, einmaliges Anmelden) mithilfe von [benutzerdefinierten Richtlinien](custom-policy-overview.md) in Azure Active Directory B2C (Azure AD B2C).

## <a name="jwt-token-lifetimes-and-claims-configuration"></a>JWT-Tokengültigkeitsdauern und Anspruchskonfiguration

Zum Ändern der Einstellungen für Ihre Tokengültigkeitsdauern fügen Sie in der Datei der vertrauenden Seite für die gewünschte Richtlinie ein [ClaimsProviders](claimsproviders.md)-Element hinzu.  Das **ClaimsProviders**-Element ist dem [TrustFrameworkPolicy](trustframeworkpolicy.md)-Element untergeordnet.

Fügen Sie das ClaimsProviders-Element zwischen dem BasePolicy-Element und dem RelyingParty-Element der Datei der vertrauenden Seite ein.

Sie müssen dort die Informationen einfügen, die sich auf die Gültigkeitsdauer Ihrer Token auswirken. Die XML sieht wie im folgenden Beispiel aus:

```xml
<ClaimsProviders>
  <ClaimsProvider>
    <DisplayName>Token Issuer</DisplayName>
    <TechnicalProfiles>
      <TechnicalProfile Id="JwtIssuer">
        <Metadata>
          <Item Key="token_lifetime_secs">3600</Item>
          <Item Key="id_token_lifetime_secs">3600</Item>
          <Item Key="refresh_token_lifetime_secs">1209600</Item>
          <Item Key="rolling_refresh_token_lifetime_secs">7776000</Item>
          <Item Key="IssuanceClaimPattern">AuthorityAndTenantGuid</Item>
          <Item Key="AuthenticationContextReferenceClaimPattern">None</Item>
        </Metadata>
      </TechnicalProfile>
    </TechnicalProfiles>
  </ClaimsProvider>
</ClaimsProviders>
```

Im vorstehenden Beispiel werden die folgenden Werte festgelegt:

- **Lebensdauer des Zugriffstokens:** Der Wert für die Lebensdauer des Zugriffstokens wird mit dem Metadatenelement **token_lifetime_secs** festgelegt. Der Standardwert beträgt 3.600 Sekunden (60 Minuten).
- **Lebensdauer des ID-Tokens:** Der Wert für die Lebensdauer des ID-Tokens wird mit dem Metadatenelement **id_token_lifetime_secs** festgelegt. Der Standardwert beträgt 3.600 Sekunden (60 Minuten).
- **Lebensdauer des Aktualisierungstokens:** Der Wert für die Lebensdauer des Aktualisierungstokens wird mit dem Metadatenelement **refresh_token_lifetime_secs** festgelegt. Der Standardwert beträgt 1.209.600 Sekunden (14 Tage).
- **Lebensdauer für gleitendes Fenster des Aktualisierungstokens:** Wenn Sie eine Gültigkeitsdauer für das gleitende Fenster des Aktualisierungstokens festlegen möchten, legen Sie den Wert des Metadatenelements **rolling_refresh_token_lifetime_secs** fest. Der Standardwert ist 7.776.000 (90 Tage). Falls Sie keine Gültigkeitsdauer für das gleitende Fenster erzwingen möchten, ersetzen Sie das Element durch `<Item Key="allow_infinite_rolling_refresh_token">True</Item>`.
- **Ausstelleranspruch (iss):** Der Ausstelleranspruch (iss) wird mit dem Metadatenelement **IssuanceClaimPattern** festgelegt. Die gültigen Werte sind `AuthorityAndTenantGuid` und `AuthorityWithTfp`.
- **Festlegen des Anspruchs zur Darstellung der Richtlinien-ID:** Die Optionen zum Festlegen dieses Werts sind `TFP` (Vertrauensframework-Richtlinie) und `ACR` (Authentifizierungskontext-Referenz). Der empfohlene Wert ist `TFP`. Legen Sie für **AuthenticationContextReferenceClaimPattern** den Wert `None` fest.

    Fügen Sie im **ClaimsSchema**-Element dieses Element hinzu:

    ```xml
    <ClaimType Id="trustFrameworkPolicy">
      <DisplayName>Trust framework policy name</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
    ```

    Fügen Sie im **OutputClaims**-Element dieses Element hinzu:

    ```xml
    <OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
    ```

    Entfernen Sie für ACR das Element **AuthenticationContextReferenceClaimPattern**.

- **Anspruch „Antragsteller“:** Diese Option hat standardmäßig den Wert „ObjectID“. Ersetzen Sie die folgende Zeile, um diese Einstellung in `Not Supported` zu ändern:

    ```xml
    <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
    ```

    Durch diese Zeile:

    ```xml
    <OutputClaim ClaimTypeReferenceId="sub" />
    ```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur [Azure AD B2C-Sitzung](session-overview.md).
- Informationen zum [Konfigurieren des Sitzungsverhaltens in benutzerdefinierten Richtlinien](session-behavior-custom-policy.md).
- Referenz: [JwtIssuer](jwt-issuer-technical-profile.md).
