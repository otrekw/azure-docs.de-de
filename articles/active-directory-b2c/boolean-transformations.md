---
title: Beispiele für die Transformation von booleschen Ansprüchen für benutzerdefinierte Richtlinien
titleSuffix: Azure AD B2C
description: Hier finden Sie Beispiele für die Transformation von booleschen Ansprüchen für das IEF-Schema (Identity Experience Framework) von Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/03/2020
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: f0d6d74271cc4ff0be4a653b389cc70ad5c56ef9
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2020
ms.locfileid: "76983077"
---
# <a name="boolean-claims-transformations"></a>Transformationen von booleschen Ansprüchen

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dieser Artikel enthält Beispiele für die Verwendung von Transformationen von booleschen Ansprüchen des Identity Experience Framework-Schemas in Azure Active Directory B2C (Azure AD B2C). Weitere Informationen finden Sie unter [ClaimsTransformations](claimstransformations.md).

## <a name="andclaims"></a>AndClaims

Führt einen And-Vorgang für zwei boolesche Eingabeansprüche aus und legt den Ausgabeanspruch mit dem Ergebnis des Vorgangs fest.

| Element  | TransformationClaimType  | Datentyp  | Notizen |
|-------| ------------------------ | ---------- | ----- |
| InputClaim | inputClaim1 | boolean | Der erste auszuwertende Anspruchstyp |
| InputClaim | inputClaim2  | boolean | Der zweite auszuwertende Anspruchstyp |
|OutputClaim | outputClaim | boolean | Die Anspruchstypen, die erstellt werden, nachdem diese Anspruchstransformation aufgerufen wurde (TRUE oder FALSE). |

Die folgende Anspruchstransformation veranschaulicht, wie Sie einen And-Vorgang für zwei boolesche Anspruchstypen (`isEmailNotExist` und `isSocialAccount`) durchführen. `true` wird für den Ausgabeanspruch `presentEmailSelfAsserted` festgelegt, wenn beide Eingabeansprüche den Wert `true` aufweisen. In einem Orchestrierungsschritt können Sie eine Vorbedingung nur verwenden, um eine Seite mit Selbstbestätigung im Voraus einzustellen, wenn eine E-Mail-Adresse eines Social Media-Kontos leer ist.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="AndClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isEmailNotExist" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="isSocialAccount" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="presentEmailSelfAsserted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
    - **inputClaim1**: TRUE
    - **inputClaim2**: FALSE
- Ausgabeansprüche:
    - **outputClaim**: FALSE


## <a name="assertbooleanclaimisequaltovalue"></a>AssertBooleanClaimIsEqualToValue

Überprüft, ob die booleschen Werte von zwei Ansprüchen identisch sind und löst eine Ausnahme aus, wenn sie es nicht sind.

| Element | TransformationClaimType  | Datentyp  | Notizen |
| ---- | ------------------------ | ---------- | ----- |
| inputClaim | inputClaim | boolean | Der Anspruchstyp, der bestätigt werden soll. |
| InputParameter |valueToCompareTo | boolean | Der Wert, der verglichen werden soll (TRUE oder FALSE). |

Die Anspruchstransformation **AssertBooleanClaimIsEqualToValue** wird immer über ein [technisches Validierungsprofil](validation-technical-profile.md) ausgeführt, das von einem [selbstbestätigten technischen Profil](self-asserted-technical-profile.md) aufgerufen wird. Die Metadaten des selbstbestätigten technischen Profils **UserMessageIfClaimsTransformationBooleanValueIsNotEqual** steuern die Fehlermeldung, die das technische Profil dem Benutzer anzeigt.

![Ausführung von AssertStringClaimsAreEqual](./media/boolean-transformations/assert-execution.png)

Die folgende Anspruchstransformation veranschaulicht, wie Sie den Wert eines booleschen Anspruchstyps mit einem `true`-Wert überprüfen. Wenn der Wert des Anspruchstyps `accountEnabled` FALSE ist, wird eine Fehlermeldung ausgelöst.

```XML
<ClaimsTransformation Id="AssertAccountEnabledIsTrue" TransformationMethod="AssertBooleanClaimIsEqualToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="accountEnabled" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="valueToCompareTo" DataType="boolean" Value="true" />
  </InputParameters>
</ClaimsTransformation>
```


Das `login-NonInteractive`technische Validierungsprofil ruft die `AssertAccountEnabledIsTrue`-Anspruchstransformation auf.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertAccountEnabledIsTrue" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

Das selbstbestätigte technische Profil ruft das technische Validierungsprofil **login-NonInteractive** auf.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationBooleanValueIsNotEqual">Custom error message if account is disabled.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
    - **inputClaim**: FALSE
    - **valueToCompareTo**: TRUE
- Ergebnis: Fehler wird ausgelöst.

## <a name="comparebooleanclaimtovalue"></a>CompareBooleanClaimToValue

Überprüft, ob der boolesche Wert eines Anspruchs `true` oder `false` entspricht, und gibt das Ergebnis der Komprimierung zurück. 

| Element | TransformationClaimType  | Datentyp  | Notizen |
| ---- | ------------------------ | ---------- | ----- |
| inputClaim | inputClaim | boolean | Der Anspruchstyp, der bestätigt werden soll. |
| InputParameter |valueToCompareTo | boolean | Der Wert, der verglichen werden soll (TRUE oder FALSE). |
| OutputClaim | inputClaim | boolean | Der Anspruchstyp, der erstellt wird, nachdem diese Anspruchstransformation aufgerufen wurde. |


Die folgende Anspruchstransformation veranschaulicht, wie Sie den Wert eines booleschen Anspruchstyps mit einem `true`-Wert überprüfen. Wenn der Wert des `IsAgeOver21Years`-Anspruchstyps `true` entspricht, gibt die Anspruchstransformation `true`, andernfalls `false` zurück.

```XML
<ClaimsTransformation Id="AssertAccountEnabled" TransformationMethod="CompareBooleanClaimToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="IsAgeOver21Years" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="valueToCompareTo" DataType="boolean" Value="true" />
  </InputParameters>
  <OutputClaims>
      <OutputClaim  ClaimTypeReferenceId="accountEnabled" TransformationClaimType="compareResult"/>
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
    - **inputClaim**: FALSE
- Eingabeparameter:
    - **valueToCompareTo**: TRUE
- Ausgabeansprüche:
    - **compareResult**: false 



## <a name="notclaims"></a>NotClaims

Führt einen Not-Vorgang für den booleschen Eingabeanspruch durch und legt den Ausgabeanspruch mit dem Ergebnis des Vorgangs fest.

| Element | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | boolean | Der auszuführende Anspruch |
| OutputClaim | outputClaim | boolean | Die Anspruchstypen, die erstellt werden, nachdem die Anspruchstransformation aufgerufen wurde (TRUE oder FALSE). |

Verwenden Sie diese Anspruchstransformation, um die logische Negation für einen Anspruch auszuführen.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="NotClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="inputClaim" />
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
    - **inputClaim**: FALSE
- Ausgabeansprüche:
    - **outputClaim**: true

## <a name="orclaims"></a>OrClaims

Berechnet einen Or-Vorgang für zwei boolesche Eingabeansprüche und legt den Ausgabeanspruch mit dem Ergebnis des Vorgangs fest.

| Element | TransformationClaimType | Datentyp | Notizen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | boolean | Der erste auszuwertende Anspruchstyp |
| InputClaim | inputClaim2 | boolean | Der zweite auszuwertende Anspruchstyp |
| OutputClaim | outputClaim | boolean | Die Anspruchstypen, die erstellt werden, nachdem diese Anspruchstransformation aufgerufen wurde (TRUE oder FALSE). |

Die folgende Anspruchstransformation veranschaulicht, wie Sie einen `Or`-Vorgang für zwei boolesche Anspruchstypen durchführen. Im Orchestrierungsschritt können Sie eine Vorbedingung verwenden, um eine Seite mit Selbstbestätigung im Voraus einzustellen, wenn einer der Ansprüche den Wert `true` aufweist.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="OrClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isLastTOSAcceptedNotExists" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="isLastTOSAcceptedGreaterThanNow" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="presentTOSSelfAsserted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
</ClaimsTransformation>
```

### <a name="example"></a>Beispiel

- Eingabeansprüche:
    - **inputClaim1**: TRUE
    - **inputClaim2**: FALSE
- Ausgabeansprüche:
    - **outputClaim**: true
