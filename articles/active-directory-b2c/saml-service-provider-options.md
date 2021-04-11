---
title: Konfigurieren der Optionen für SAML-Dienstanbieter
title-suffix: Azure Active Directory B2C
description: Konfigurieren der Optionen für den SAML-Dienstanbieter in Azure Active Directory B2C
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/15/2021
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 09cfdd026105a34db976118f38b011e2c4578a24
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103470779"
---
# <a name="options-for-registering-a-saml-application-in-azure-ad-b2c"></a>Optionen zum Registrieren einer SAML-Anwendung in Azure AD B2C

In diesem Artikel werden die Konfigurationsoptionen beschrieben, die verfügbar sind, wenn Azure Active Directory (Azure AD B2C) mit Ihrer SAML-Anwendung verbunden wird.

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-user-flow"

[!INCLUDE [active-directory-b2c-limited-to-custom-policy](../../includes/active-directory-b2c-limited-to-custom-policy.md)]

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="encrypted-saml-assertions"></a>Verschlüsselte SAML-Assertionen

Wenn Ihre Anwendung erwartet, dass SAML-Assertionen in einem verschlüsselten Format vorliegen, müssen Sie sicherstellen, dass in der Azure AD B2C-Richtlinie die Verschlüsselung aktiviert ist.

In Azure AD B2C wird das auf einem öffentlichen Schlüssel basierende Zertifikat des Dienstanbieters verwendet, um die SAML-Assertion zu verschlüsseln. Der öffentliche Schlüssel muss auf dem Metadatenendpunkt der SAML-Anwendung vorhanden sein, und der Schlüsseldeskriptor (KeyDescriptor) „use“ muss auf „Encryption“ festgelegt werden. Dies ist im folgenden Beispiel dargestellt:

```xml
<KeyDescriptor use="encryption">
  <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
    <X509Data>
      <X509Certificate>valid certificate</X509Certificate>
    </X509Data>
  </KeyInfo>
</KeyDescriptor>
```

Legen Sie im [technischen Profil der vertrauenden Seite](relyingparty.md#technicalprofile) das Metadatenelement **WantsEncryptedAssertion** auf `true` fest, um Azure AD B2C das Senden verschlüsselter Assertionen zu ermöglichen. Sie können auch den zum Verschlüsseln der SAML-Assertion verwendeten Algorithmus konfigurieren.

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="SAML2"/>
    <Metadata>
      <Item Key="WantsEncryptedAssertions">true</Item>
    </Metadata>
   ..
  </TechnicalProfile>
</RelyingParty>
```

### <a name="encryption-method"></a>Verschlüsselungsmethode

Legen Sie den Metadatenschlüssel `DataEncryptionMethod` auf der vertrauenden Seite fest, um die Verschlüsselungsmethode zu konfigurieren, die zum Verschlüsseln der SAML-Assertionsdaten verwendet wird. Mögliche Werte sind `Aes256` (Standard), `Aes192`, `Sha512` oder `Aes128`. Die Metadaten bestimmen den Wert des `<EncryptedData>`-Elements in der SAML-Antwort.

Legen Sie den Metadatenschlüssel `KeyEncryptionMethod` auf der vertrauenden Seite fest, um die Methode für die Verschlüsselung der Kopie des Schlüssels zu konfigurieren, der zum Verschlüsseln der SAML-Assertionsdaten verwendet wurde. Mögliche Werte sind `Rsa15` (Standardwert) – RSA PKCS-Algorithmus (Public Key Cryptography Standard), Version 1.5, und `RsaOaep` – RSA OAEP-Verschlüsselungsalgorithmus (Optimal Asymmetric Encryption Padding).  Die Metadaten bestimmen den Wert des `<EncryptedKey>`-Elements in der SAML-Antwort.

Im folgenden Beispiel wird der Abschnitt `EncryptedAssertion` einer SAML-Assertion veranschaulicht. Die Methode für die Datenverschlüsselung lautet `Aes128`, und als Methode für den verschlüsselten Schlüssel wird `Rsa15` verwendet.

```xml
<saml:EncryptedAssertion>
  <xenc:EncryptedData xmlns:xenc="http://www.w3.org/2001/04/xmlenc#"
    xmlns:dsig="http://www.w3.org/2000/09/xmldsig#" Type="http://www.w3.org/2001/04/xmlenc#Element">
    <xenc:EncryptionMethod Algorithm="http://www.w3.org/2001/04/xmlenc#aes128-cbc" />
    <dsig:KeyInfo>
      <xenc:EncryptedKey>
        <xenc:EncryptionMethod Algorithm="http://www.w3.org/2001/04/xmlenc#rsa-1_5" />
        <xenc:CipherData>
          <xenc:CipherValue>...</xenc:CipherValue>
        </xenc:CipherData>
      </xenc:EncryptedKey>
    </dsig:KeyInfo>
    <xenc:CipherData>
      <xenc:CipherValue>...</xenc:CipherValue>
    </xenc:CipherData>
  </xenc:EncryptedData>
</saml:EncryptedAssertion>
```

Sie können das Format der verschlüsselten Assertionen ändern. Legen Sie den Metadatenschlüssel `UseDetachedKeys` auf der vertrauenden Seite fest, um das Verschlüsselungsformat zu konfigurieren. Mögliche Werte sind `true` oder `false` (Standardwert). Wenn der Wert auf `true` festgelegt ist, wird die verschlüsselte Assertion von den getrennten Schlüsseln als untergeordnetes Element von `EncrytedAssertion` hinzugefügt (und nicht von `EncryptedData`).

Konfigurieren Sie die Verschlüsselungsmethode und das Format, und verwenden Sie die Metadatenschlüssel innerhalb des [technischen Profils der vertrauenden Seite](relyingparty.md#technicalprofile):

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="SAML2"/>
    <Metadata>
      <Item Key="DataEncryptionMethod">Aes128</Item>
      <Item Key="KeyEncryptionMethod">Rsa15</Item>
      <Item Key="UseDetachedKeys">false</Item>
    </Metadata>
   ..
  </TechnicalProfile>
</RelyingParty>
```

## <a name="identity-provider-initiated-flow"></a>Vom Identitätsanbieter initiierter Flow

Wenn Ihre Anwendung eine SAML-Assertion erwartet, ohne dass zuerst eine SAML-Authentifizierungsanforderung an den Identitätsanbieter gesendet wird, müssen Sie Azure AD B2C für den vom Identitätsanbieter initiierten Flow konfigurieren.

Beim vom Identitätsanbieter initiierten Flow wird der Anmeldevorgang vom Identitätsanbieter (Azure AD B2C) initiiert, der eine nicht angeforderte SAML-Antwort an den Dienstanbieter (Ihre Anwendung der vertrauenden Seite) sendet.

Wir unterstützen derzeit keine Szenarien, in denen der initiierende Identitätsanbieter ein externer Identitätsanbieter im Verbund mit Azure AD B2C ist, z. B. [AD FS](identity-provider-adfs.md) oder [Salesforce](identity-provider-salesforce-saml.md). Es besteht nur Unterstützung für die lokale Azure AD B2C-Kontoauthentifizierung.

Um den vom Identitätsanbieter initiierten Flow zu aktivieren, legen Sie im [technischen Profil der vertrauenden Seite](relyingparty.md#technicalprofile) das Metadatenelement **IdpInitiatedProfileEnabled** auf `true` fest.

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="SAML2"/>
    <Metadata>
      <Item Key="IdpInitiatedProfileEnabled">true</Item>
    </Metadata>
   ..
  </TechnicalProfile>
</RelyingParty>
```

Verwenden Sie die folgende URL, um einen Benutzer über den vom Identitätsanbieter initiierten Flow anzumelden oder zu registrieren:

```
https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/<policy-name>/generic/login?EntityId=app-identifier-uri 
```

Ersetzen Sie die folgenden Werte:

* **tenant-name** durch den Namen Ihres Mandanten
* **policy-name** durch den Namen der SAML-Richtlinie für die vertrauende Seite
* **app-identifier-uri** mit den `identifierUris` in der Metadatendatei, z. B. `https://contoso.onmicrosoft.com/app-name`

### <a name="sample-policy"></a>Beispielrichtlinie

Wir stellen eine vollständige Beispielrichtlinie zur Verfügung, die Sie zum Testen mit der SAML-Test-App verwenden können.

1. Laden Sie die [Anmeldebeispielrichtlinie „SAML-SP-initiated“](https://github.com/azure-ad-b2c/saml-sp/tree/master/policy/SAML-SP-Initiated) herunter.
1. Aktualisieren Sie `TenantId` gemäß Ihrem Mandantennamen, z. B. *contoso.b2clogin.com*.
1. Behalten Sie den Richtliniennamen *B2C_1A_signup_signin_saml* bei.

## <a name="saml-response-signature-algorithm"></a>Signaturalgorithmus für SAML-Antwort

Sie können den zum Signieren der SAML-Assertion verwendeten Signaturalgorithmus konfigurieren. Mögliche Werte sind `Sha256`, `Sha384`, `Sha512` oder `Sha1`. Stellen Sie sicher, dass für das technische Profil und die Anwendung der gleiche Signaturalgorithmus genutzt wird. Verwenden Sie nur den Algorithmus, den Ihr Zertifikat unterstützt.

Konfigurieren Sie den Signaturalgorithmus mit dem Metadatenschlüssel `XmlSignatureAlgorithm` im Metadatenelement auf der vertrauenden Seite.

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="SAML2"/>
    <Metadata>
      <Item Key="XmlSignatureAlgorithm">Sha256</Item>
    </Metadata>
   ..
  </TechnicalProfile>
</RelyingParty>
```

## <a name="saml-response-lifetime"></a>Lebensdauer der SAML-Antwort

Sie können konfigurieren, wie lange die SAML-Antwort gültig bleibt. Legen Sie die Lebensdauer fest, indem Sie das Metadatenelement `TokenLifeTimeInSeconds` im technischen Profil des SAML-Tokenausstellers verwenden. Mit diesem Wert wird die Anzahl von Sekunden angegeben, die ab dem Zeitstempel `NotBefore`, der beim Ausstellen des Tokens berechnet wird, verstreichen können. Standardmäßig beträgt die Lebensdauer 300 Sekunden (5 Minuten).

```xml
<ClaimsProvider>
  <DisplayName>Token Issuer</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Saml2AssertionIssuer">
      <DisplayName>Token Issuer</DisplayName>
      <Protocol Name="SAML2"/>
      <OutputTokenFormat>SAML2</OutputTokenFormat>
      <Metadata>
        <Item Key="TokenLifeTimeInSeconds">400</Item>
      </Metadata>
      ...
    </TechnicalProfile>
```

## <a name="saml-response-valid-from-skew"></a>Gültigkeit der SAML-Antwort in Bezug auf die Abweichung

Sie können die Zeitabweichung konfigurieren, die auf den Zeitstempel `NotBefore` der SAML-Antwort angewendet wird. Mit dieser Konfiguration wird sichergestellt, dass die SAML-Assertion auch bei nicht synchronen Zeiten zwischen zwei Plattformen als gültig angesehen wird, sofern sie innerhalb dieser Zeitabweichung liegt.

Legen Sie die Zeitabweichung fest, indem Sie das Metadatenelement `TokenNotBeforeSkewInSeconds` im technischen Profil des SAML-Tokenausstellers verwenden. Der Wert für die Abweichung wird in Sekunden angegeben (Standardwert: 0). Der Höchstwert ist 3.600 (eine Stunde).

Beispiel mit Festlegung von `TokenNotBeforeSkewInSeconds` auf `120` Sekunden:

- Das Token wird um 13:05:10 UTC ausgestellt.
- Das Token ist ab 13:03:10 UTC gültig.

```xml
<ClaimsProvider>
  <DisplayName>Token Issuer</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Saml2AssertionIssuer">
      <DisplayName>Token Issuer</DisplayName>
      <Protocol Name="SAML2"/>
      <OutputTokenFormat>SAML2</OutputTokenFormat>
      <Metadata>
        <Item Key="TokenNotBeforeSkewInSeconds">120</Item>
      </Metadata>
      ...
    </TechnicalProfile>
```

## <a name="remove-milliseconds-from-date-and-time"></a>Entfernen von Millisekunden für Datum und Uhrzeit

Sie können angeben, ob die Millisekunden aus datetime-Werten in der SAML-Antwort entfernt werden (hierzu gehören „IssueInstant“, „NotBefore“, „NotOnOrAfter“ und „AuthnInstant“). Legen Sie zum Entfernen der Millisekunden den Metadatenschlüssel `RemoveMillisecondsFromDateTime
` auf der vertrauenden Seite fest. Mögliche Werte: `false` (Standard) oder `true`.

```xml
<ClaimsProvider>
  <DisplayName>Token Issuer</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Saml2AssertionIssuer">
      <DisplayName>Token Issuer</DisplayName>
      <Protocol Name="SAML2"/>
      <OutputTokenFormat>SAML2</OutputTokenFormat>
      <Metadata>
        <Item Key="RemoveMillisecondsFromDateTime">true</Item>
      </Metadata>
      ...
    </TechnicalProfile>
```

## <a name="azure-ad-b2c-issuer-id"></a>Azure AD B2C-Aussteller-ID

Wenn Sie mehrere SAML-Anwendungen verwenden, die von unterschiedlichen `entityID`-Werten abhängen, können Sie den Wert `issueruri` in der Datei der vertrauenden Seite überschreiben. Kopieren Sie zum Überschreiben des Aussteller-URI das technische Profil mit der ID „Saml2AssertionIssuer“ aus der Basisdatei, und überschreiben Sie den Wert `issueruri`.

> [!TIP]
> Kopieren Sie den Abschnitt `<ClaimsProviders>` aus der Basisdatei, und bewahren Sie diese Elemente im Anspruchsanbieter auf: `<DisplayName>Token Issuer</DisplayName>`, `<TechnicalProfile Id="Saml2AssertionIssuer">` und `<DisplayName>Token Issuer</DisplayName>`.
 
Beispiel:

```xml
   <ClaimsProviders>   
    <ClaimsProvider>
      <DisplayName>Token Issuer</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Saml2AssertionIssuer">
          <DisplayName>Token Issuer</DisplayName>
          <Metadata>
            <Item Key="IssuerUri">customURI</Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
  </ClaimsProviders>
  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpInSAML" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="SAML2" />
      <Metadata>
     …
```

## <a name="session-management"></a>Sitzungsverwaltung

Sie können die Sitzung zwischen Azure AD B2C und der SAML-Anwendung der vertrauenden Seite verwalten, indem Sie das Element `UseTechnicalProfileForSessionManagement` und [SamlSSOSessionProvider](custom-policy-reference-sso.md#samlssosessionprovider) verwenden.

## <a name="force-users-to-re-authenticate"></a>Erneutes Authentifizieren von Benutzern erzwingen 

Um die erneute Authentifizierung von Benutzern zu erzwingen, kann die Anwendung das `ForceAuthn`-Attribut in die SAML-Authentifizierungsanforderung einschließen. Das `ForceAuthn`-Attribut ist ein boolescher Wert. Wenn diese Einstellung auf „true“ festgelegt ist, wird die Benutzersitzung bei Azure AD B2C ungültig, und der Benutzer wird gezwungen, sich erneut zu authentifizieren. Die folgende SAML-Authentifizierungsanforderung veranschaulicht, wie das `ForceAuthn`-Attribut auf „true“ festgelegt wird. 


```xml
<samlp:AuthnRequest 
       Destination="https://contoso.b2clogin.com/contoso.onmicrosoft.com/B2C_1A_SAML2_signup_signin/samlp/sso/login"
       ForceAuthn="true" ...>
    ...
</samlp:AuthnRequest>
```

## <a name="debug-the-saml-protocol"></a>Debuggen des SAML-Protokolls

Zum Konfigurieren und Debuggen der Integration mit Ihrem Dienstanbieter können Sie eine Browsererweiterung für das SAML-Protokoll verwenden, z. B. die [SAML-DevTools-Erweiterung](https://chrome.google.com/webstore/detail/saml-devtools-extension/jndllhgbinhiiddokbeoeepbppdnhhio) für Chrome, [SAML-tracer](https://addons.mozilla.org/es/firefox/addon/saml-tracer/) für Firefox oder [Entwicklertools für Edge oder IE](https://techcommunity.microsoft.com/t5/microsoft-sharepoint-blog/gathering-a-saml-token-using-edge-or-ie-developer-tools/ba-p/320957).

Mit diesen Tools können Sie die Integration zwischen Ihrer Anwendung und Azure AD B2C überprüfen. Beispiel:

* Überprüfen Sie, ob die SAML-Anforderung eine Signatur enthält, und bestimmen Sie, welcher Algorithmus zum Anmelden für die Autorisierungsanforderung verwendet wird.
* Überprüfen Sie, ob Azure AD B2C eine Fehlermeldung zurückgibt.
* Überprüfen Sie, ob der Assertionsabschnitt verschlüsselt ist.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die weiteren [Informationen zum SAML-Protokoll auf der OASIS-Website](https://www.oasis-open.org/).
- Rufen Sie die SAML-Test-Web-App im [GitHub-Repository der Community in Azure AD B2C](https://github.com/azure-ad-b2c/saml-sp-tester) ab.

<!-- LINKS - External -->
[samltest]: https://aka.ms/samltestapp

::: zone-end
