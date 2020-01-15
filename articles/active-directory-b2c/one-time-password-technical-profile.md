---
title: Aktivieren von Einmalkennwortüberprüfung
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie ein Einmalkennwort (One-Time Password, OTP) mithilfe von benutzerdefinierten Azure AD B2C-Richtlinien einrichten.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/10/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: c50edb03fe849c70596c0bfb3cdc2dafa15f136f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475052"
---
# <a name="define-a-one-time-password-technical-profile-in-an-azure-ad-b2c-custom-policy"></a>Definieren eines technischen Einmalkennwortprofils in einer benutzerdefinierten Azure AD B2C-Richtlinie

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) bietet Unterstützung für die Verwaltung der Generierung und Überprüfung eines Einmalkennworts. Verwenden Sie ein technisches Profil, um einen Code zu generieren, und überprüfen Sie diesen Code später.

Das technische Einmalkennwortprofil kann bei der Codeüberprüfung auch eine Fehlermeldung zurückgeben. Entwerfen Sie die Integration in das Einmalkennwort, indem Sie ein **technisches Validierungsprofil** verwenden. Ein technisches Validierungsprofil ruft das technische Einmkennwortprofil auf, um einen Code zu überprüfen. Mit dem technischen Validierungsprofil werden die vom Benutzer bereitgestellten Daten überprüft, bevor die User Journey fortgesetzt wird. Mit dem technischen Validierungsprofil wird eine Fehlermeldung auf einer Seite mit Selbstbestätigung angezeigt.

## <a name="protocol"></a>Protocol

Das **Name**-Attribut des **Protocol**-Elements muss auf `Proprietary` festgelegt werden. Das **handler**-Attribut muss den vollqualifizierten Namen der Protokollhandlerassembly, die von Azure AD B2C verwendet wird, enthalten:

```XML
Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
```

Das folgende Beispiel zeigt ein technisches Einmalkennwortprofil:

```XML
<TechnicalProfile Id="VerifyCode">
  <DisplayName>Validate user input verification code</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="generate-code"></a>Generieren von Code

Der erste Modus dieses technischen Profils besteht darin, einen Code zu generieren. Im Folgenden finden Sie die Optionen, die für diesen Modus konfiguriert werden können.

### <a name="input-claims"></a>Eingabeansprüche

Das **InputClaims**-Element enthält eine Liste der Ansprüche, die zum Senden an den Anbieter des Einmalkennwortprotokolls erforderlich sind. Sie können auch den Namen Ihres Anspruchs dem unten definierten Namen zuordnen.

| ClaimReferenceId | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| Bezeichner | Ja | Der Bezeichner zur Identifizierung des Benutzers, der den Code später überprüfen muss. Er wird häufig als Bezeichner des Ziels verwendet, an das der Code übermittelt wird, z.B. eine E-Mail-Adresse oder Telefonnummer. |

Das **InputClaimsTransformations**-Element darf eine Sammlung von **InputClaimsTransformation**-Elementen enthalten, die zum Ändern der Eingabeansprüche oder zum Generieren neuer Ansprüche verwendet werden, bevor das Senden an den Anbieter des Einmalkennwortprotokolls erfolgt.

### <a name="output-claims"></a>Ausgabeansprüche

Das **OutputClaims**-Element enthält eine Liste der Ansprüche, die vom Anbieter des Einmalkennwortprotokolls generiert werden. Sie können auch den Namen Ihres Anspruchs dem unten definierten Namen zuordnen.

| ClaimReferenceId | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| otpGenerated | Ja | Der generierte Code, dessen Sitzung von Azure AD B2C verwaltet wird. |

Das **OutputClaimsTransformations**-Element darf eine Sammlung von **OutputClaimsTransformation**-Elementen, die zum Ändern der Ausgabeansprüche oder zum Generieren neuer verwendet werden, enthalten.

### <a name="metadata"></a>Metadaten

Die folgenden Einstellungen können verwendet werden, um die Codegenerierung und -wartung zu konfigurieren:

| Attribut | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| CodeExpirationInSeconds | Nein | Die Zeit in Sekunden bis zum Ablauf des Codes. Mindestwert: `60`, Maximalwert: `1200`, Standardwert: `600`. |
| CodeLength | Nein | Die Länge des Codes. Standardwert: `6`. |
| CharacterSet | Nein | Der Zeichensatz für den Code, der für die Verwendung in einem regulären Ausdruck formatiert ist. Beispiel: `a-z0-9A-Z`. Standardwert: `0-9`. Der Zeichensatz muss mindestens zehn verschiedene Zeichen enthalten, die im angegebenen Zeichensatz enthalten sind. |
| NumRetryAttempts | Nein | Die Anzahl der Überprüfungsversuche, bevor der Code als ungültig betrachtet wird. Standardwert: `5`. |
| ReuseSameCode | Nein | Gibt an, ob ein doppelter Code angegeben werden soll, anstatt einen neuen Code zu generieren, wenn der angegebene Code noch nicht abgelaufen und gültig ist. Standardwert: `false`. |

### <a name="returning-error-message"></a>Zurückgegebene Fehlermeldung

Es wird keine Fehlermeldung für den Codegenerierungsmodus zurückgegeben.

### <a name="example"></a>Beispiel

Das folgende Beispiel `TechnicalProfile` wird zum Erstellen eines Codes verwendet:

```XML
<TechnicalProfile Id="GenerateCode">
    <DisplayName>Generate Code</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="Operation">GenerateCode</Item>
        <Item Key="CodeExpirationInSeconds">600</Item>
        <Item Key="CodeLength">6</Item>
        <Item Key="CharacterSet">0-9</Item>
        <Item Key="NumRetryAttempts">5</Item>
        <Item Key="ReuseSameCode">false</Item>
    </Metadata>
    <InputClaims>
        <InputClaim ClaimTypeReferenceId="identifier" PartnerClaimType="identifier" />
    </InputClaims>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="otpGenerated" PartnerClaimType="otpGenerated" />
    </OutputClaims>
</TechnicalProfile>
```

## <a name="verify-code"></a>Code überprüfen

Der zweite Modus dieses technischen Profils besteht darin, einen Code zu überprüfen. Im Folgenden finden Sie die Optionen, die für diesen Modus konfiguriert werden können.

### <a name="input-claims"></a>Eingabeansprüche

Das **InputClaims**-Element enthält eine Liste der Ansprüche, die zum Senden an den Anbieter des Einmalkennwortprotokolls erforderlich sind. Sie können auch den Namen Ihres Anspruchs dem unten definierten Namen zuordnen.

| ClaimReferenceId | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| Bezeichner | Ja | Der Bezeichner, der den Benutzer identifiziert, der zuvor einen Code generiert hat. Er wird häufig als Bezeichner des Ziels verwendet, an das der Code übermittelt wird, z.B. eine E-Mail-Adresse oder Telefonnummer. |
| otpToVerify | Ja | Der Überprüfungscode, der vom Benutzer bereitgestellt wird. |

Das **InputClaimsTransformations**-Element darf eine Sammlung von **InputClaimsTransformation**-Elementen enthalten, die zum Ändern der Eingabeansprüche oder zum Generieren neuer Ansprüche verwendet werden, bevor das Senden an den Anbieter des Einmalkennwortprotokolls erfolgt.

### <a name="output-claims"></a>Ausgabeansprüche

Während der Codeüberprüfung dieses Protokollanbieters werden keine Ausgabeansprüche bereitgestellt.

Das **OutputClaimsTransformations**-Element darf eine Sammlung von **OutputClaimsTransformation**-Elementen, die zum Ändern der Ausgabeansprüche oder zum Generieren neuer verwendet werden, enthalten.

### <a name="metadata"></a>Metadaten

Die folgenden Einstellungen können verwendet werden, um die Fehlermeldung zu konfigurieren, die bei einem Codeüberprüfungsfehler angezeigt wird:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| UserMessageIfSessionDoesNotExist | Nein | Die Meldung, die dem Benutzer angezeigt werden soll, wenn die Codeüberprüfungssitzung abgelaufen ist. Der Code ist entweder abgelaufen, oder der Code wurde nie für einen angegebenen Bezeichner generiert. |
| UserMessageIfMaxRetryAttempted | Nein | Die Meldung, die dem Benutzer angezeigt werden soll, wenn die maximal zulässige Anzahl von Überprüfungsversuchen überschritten wurde. |
| UserMessageIfInvalidCode | Nein | Die Meldung, die dem Benutzer angezeigt werden soll, wenn ein ungültiger Code bereitgestellt wurde. |

### <a name="returning-error-message"></a>Zurückgegebene Fehlermeldung

Wie unter [Metadaten](#metadata)beschrieben, können Sie die Fehlermeldung anpassen, die dem Benutzer für verschiedene Fehlerfälle angezeigt wird. Sie können diese Nachrichten weiter lokalisieren, indem Sie das Gebietsschema mit einem Präfix versehen. Beispiel:

```XML
<Item Key="en.UserMessageIfInvalidCode">Wrong code has been entered.</Item>
```

### <a name="example"></a>Beispiel

Das folgende Beispiel `TechnicalProfile` wird zum Überprüfen eines Codes verwendet:

```XML
<TechnicalProfile Id="VerifyCode">
    <DisplayName>Verify Code</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="Operation">VerifyCode</Item>
        <Item Key="UserMessageIfInvalidCode">Wrong code has been entered.</Item>
        <Item Key="UserMessageIfSessionDoesNotExist">Code has expired.</Item>
        <Item Key="UserMessageIfMaxRetryAttempted">You've tried too many times.</Item>
    </Metadata>
    <InputClaims>
        <InputClaim ClaimTypeReferenceId="identifier" PartnerClaimType="identifier" />
        <InputClaim ClaimTypeReferenceId="otpGenerated" PartnerClaimType="otpToVerify" />
    </InputClaims>
</TechnicalProfile>
```