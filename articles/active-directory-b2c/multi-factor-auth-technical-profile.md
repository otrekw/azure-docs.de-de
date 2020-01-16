---
title: Technische Azure MFA-Profile in benutzerdefinierten Richtlinien
titleSuffix: Azure AD B2C
description: Benutzerdefinierter Richtlinienverweis für technische Profile von Azure Multi-Factor Authentication (MFA) in Azure AD B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: a8aaea6b2afb4d89e6e667edba0eeba2f4ddcca8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475056"
---
# <a name="define-an-azure-mfa-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>Definieren eines technischen Azure MFA-Profils in einer benutzerdefinierten Azure AD B2C-Richtlinie

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) bietet Unterstützung für die Überprüfung einer Telefonnummer mithilfe von Azure Multi-Factor Authentication (MFA). Verwenden Sie dieses technische Profil, um einen Code zu generieren und an eine Telefonnummer zu senden, und überprüfen Sie anschließend den Code.

Das technische Azure MFA-Profil gibt möglicherweise auch eine Fehlermeldung zurück. Sie können die Azure MFA-Integration mithilfe eines **technischen Validierungsprofils** entwerfen. Das technische Validierungsprofil ruft den Azure MFA-Dienst auf. Mit dem technischen Validierungsprofil werden die vom Benutzer bereitgestellten Daten überprüft, bevor die User Journey fortgesetzt wird. Mit dem technischen Validierungsprofil wird eine Fehlermeldung auf einer Seite mit Selbstbestätigung angezeigt.

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="protocol"></a>Protocol

Das **Name**-Attribut des **Protocol**-Elements muss auf `Proprietary` festgelegt werden. Das **handler**-Attribut muss den vollqualifizierten Namen der Protokollhandlerassembly, die von Azure AD B2C verwendet wird, enthalten:

```
Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
```

Das folgende Beispiel zeigt ein technisches Azure MFA-Profil:

```XML
<TechnicalProfile Id="AzureMfa-SendSms">
    <DisplayName>Send Sms</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    ...
```

## <a name="send-sms"></a>Senden einer SMS

Der erste Modus dieses technischen Profils besteht darin, einen Code zu generieren und zu senden. Für diesen Modus können folgende Optionen konfiguriert werden:

### <a name="input-claims"></a>Eingabeansprüche

Das **InputClaims**-Element enthält eine Liste von Ansprüchen, die an Azure MFA gesendet werden sollen. Sie können auch den Namen Ihres Anspruchs dem im technischen MFA-Profil definierten Namen zuordnen.

| ClaimReferenceId | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| userPrincipalName | Ja | Der Bezeichner für den Benutzer, dem die Telefonnummer gehört. |
| phoneNumber | Ja | Die Telefonnummer, an die ein SMS-Code gesendet werden soll. |
| companyName | Nein |Der Unternehmensname in der SMS. Wenn nichts angegeben ist, wird der Name der Anwendung verwendet. |
| locale | Nein | Das Gebietsschema der SMS. Wenn nichts angegeben ist, wird das Browsergebietsschema des Benutzers verwendet. |

Das **InputClaimsTransformations**-Element enthält ggf. eine Sammlung von **InputClaimsTransformation**-Elementen, die zum Ändern der Eingabeansprüche oder zum Generieren neuer verwendet werden, bevor es an den Azure MFA-Dienst gesendet wird.

### <a name="output-claims"></a>Ausgabeansprüche

Der Azure MFA-Protokollanbieter gibt keine **OutputClaims**-Elemente zurück. Daher ist es nicht erforderlich, Ausgabeansprüche anzugeben. Sie können jedoch Ansprüche, die nicht vom Azure MFA-Identitätsanbieter zurückgegeben wurden, einfügen, sofern Sie das `DefaultValue`-Attribut festlegen.

Das **OutputClaimsTransformations**-Element darf eine Sammlung von **OutputClaimsTransformation**-Elementen, die zum Ändern der Ausgabeansprüche oder zum Generieren neuer verwendet werden, enthalten.

### <a name="metadata"></a>Metadaten

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| Vorgang | Ja | Muss **OneWaySMS** sein.  |
| UserMessageIfInvalidFormat | Nein | Benutzerdefinierte Fehlermeldung, wenn die angegebene Telefonnummer keine gültige Telefonnummer ist. |
| UserMessageIfCouldntSendSms | Nein | Benutzerdefinierte Fehlermeldung, wenn die angegebene Telefonnummer keine SMS-Nachrichten akzeptiert. |
| UserMessageIfServerError | Nein | Benutzerdefinierte Fehlermeldung, wenn der Server einen internen Fehler festgestellt hat. |

### <a name="return-an-error-message"></a>Zurückgeben einer Fehlermeldung

Wie unter [Metadaten](#metadata)beschrieben, können Sie die Fehlermeldung anpassen, die dem Benutzer für verschiedene Fehlerfälle angezeigt wird. Sie können diese Nachrichten weiter lokalisieren, indem Sie das Gebietsschema mit einem Präfix versehen. Beispiel:

```XML
<Item Key="en.UserMessageIfInvalidFormat">Invalid phone number.</Item>
```

### <a name="example-send-an-sms"></a>Beispiel: Senden einer SMS

Das folgende Beispiel zeigt ein technisches Azure MFA-Profil, das zum Senden eines Code per SMS verwendet wird.

```XML
<TechnicalProfile Id="AzureMfa-SendSms">
    <DisplayName>Send Sms</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="Operation">OneWaySMS</Item>
    </Metadata>
    <InputClaimsTransformations>
        <InputClaimsTransformation ReferenceId="CombinePhoneAndCountryCode" />
        <InputClaimsTransformation ReferenceId="ConvertStringToPhoneNumber" />
    </InputClaimsTransformations>
    <InputClaims>
        <InputClaim ClaimTypeReferenceId="userPrincipalName" />
        <InputClaim ClaimTypeReferenceId="fullPhoneNumber" PartnerClaimType="phoneNumber" />
    </InputClaims>
</TechnicalProfile>
```

## <a name="verify-code"></a>Code überprüfen

Der zweite Modus dieses technischen Profils besteht darin, einen Code zu überprüfen. Für diesen Modus können folgende Optionen konfiguriert werden:

### <a name="input-claims"></a>Eingabeansprüche

Das **InputClaims**-Element enthält eine Liste von Ansprüchen, die an Azure MFA gesendet werden sollen. Sie können auch den Namen Ihres Anspruchs dem im technischen MFA-Profil definierten Namen zuordnen.

| ClaimReferenceId | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- | ----------- |
| phoneNumber| Ja | Dieselbe Telefonnummer, die zuvor zum Senden eines Codes verwendet wurde. Außerdem wird sie verwendet, um eine Telefonüberprüfungssitzung zu suchen. |
| verificationCode  | Ja | Der Prüfcode, der vom zu überprüfenden Benutzer bereitgestellt wird. |

Das **InputClaimsTransformations**-Element enthält ggf. eine Sammlung von **InputClaimsTransformation**-Elementen, die zum Ändern der Eingabeansprüche oder zum Generieren neuer verwendet werden, bevor der Azure MFA-Dienst aufgerufen wird.

### <a name="output-claims"></a>Ausgabeansprüche

Der Azure MFA-Protokollanbieter gibt keine **OutputClaims**-Elemente zurück. Daher ist es nicht erforderlich, Ausgabeansprüche anzugeben. Sie können jedoch Ansprüche, die nicht vom Azure MFA-Identitätsanbieter zurückgegeben wurden, einfügen, sofern Sie das `DefaultValue`-Attribut festlegen.

Das **OutputClaimsTransformations**-Element darf eine Sammlung von **OutputClaimsTransformation**-Elementen, die zum Ändern der Ausgabeansprüche oder zum Generieren neuer verwendet werden, enthalten.

## <a name="metadata"></a>Metadaten

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| Vorgang | Ja | Muss **Überprüfen** sein. |
| UserMessageIfInvalidFormat | Nein | Benutzerdefinierte Fehlermeldung, wenn die angegebene Telefonnummer keine gültige Telefonnummer ist. |
| UserMessageIfWrongCodeEntered | Nein | Benutzerdefinierte Fehlermeldung, wenn der für die Überprüfung eingegebene Code falsch ist. |
| UserMessageIfMaxAllowedCodeRetryReached | Nein | Benutzerdefinierte Fehlermeldung, wenn der Benutzer zu oft versucht hat, einen Prüfcode auszuführen. |
| UserMessageIfThrottled | Nein | Benutzerdefinierte Fehlermeldung, wenn der Benutzer gedrosselt ist. |
| UserMessageIfServerError | Nein | Benutzerdefinierte Fehlermeldung, wenn der Server einen internen Fehler festgestellt hat. |

### <a name="return-an-error-message"></a>Zurückgeben einer Fehlermeldung

Wie unter [Metadaten](#metadata)beschrieben, können Sie die Fehlermeldung anpassen, die dem Benutzer für verschiedene Fehlerfälle angezeigt wird. Sie können diese Nachrichten weiter lokalisieren, indem Sie das Gebietsschema mit einem Präfix versehen. Beispiel:

```XML
<Item Key="en.UserMessageIfWrongCodeEntered">Wrong code has been entered.</Item>
```

### <a name="example-verify-a-code"></a>Beispiel: Überprüfen eines Codes

Das folgende Beispiel zeigt ein technisches Azure MFA-Profil, das zum Überprüfen des Codes verwendet wird:

```XML
<TechnicalProfile Id="AzureMfa-VerifySms">
    <DisplayName>Verify Sms</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureMfaProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="Operation">Verify</Item>
    </Metadata>
    <InputClaims>
        <InputClaim ClaimTypeReferenceId="phoneNumber" PartnerClaimType="phoneNumber" />
        <InputClaim ClaimTypeReferenceId="verificationCode" />
    </InputClaims>
</TechnicalProfile>
```
