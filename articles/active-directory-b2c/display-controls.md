---
title: Referenz zu Anzeigesteuerelementen
titleSuffix: Azure AD B2C
description: Referenz zu Anzeigesteuerelementen für Azure AD B2C Verwendung von Anzeigesteuerelementen zur Anpassung von Benutzerjourneys, die in benutzerdefinierten Richtlinien definiert sind
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/10/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 4998fb19e42e123edd57bfcf10931d594ac4cb44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188731"
---
# <a name="display-controls"></a>Anzeigesteuerelemente

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Ein **Anzeigesteuerelement** ist ein Benutzeroberflächenelement mit spezieller Funktionalität, das mit dem Back-End-Dienst von Azure Active Directory B2C (Azure AD B2C) interagiert. Es ermöglicht dem Benutzer das Durchführen von Aktionen auf der Seite, die ein [technisches Validierungsprofil](validation-technical-profile.md) auf dem Back-End aufrufen. Anzeigesteuerelemente werden auf der Seite angezeigt, und ein [selbstbestätigtes technisches Profil](self-asserted-technical-profile.md) verweist auf diese.

In der folgenden Abbildung wird eine selbstbestätigte Registrierungsseite mit zwei Anzeigesteuerelementen veranschaulicht, die eine primäre und sekundäre E-Mail-Adresse überprüfen.

![Beispiel: gerendertes Anzeigesteuerelement](media/display-controls/display-control-email.png)

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

 Im Abschnitt [Metadaten](self-asserted-technical-profile.md#metadata) eines [selbstbestätigten technischen Profils](self-asserted-technical-profile.md) muss für das [ContentDefinition](contentdefinitions.md)-Element, auf das verwiesen wird, `DataUri` auf Seitenvertragsversion 2.0.0 oder höher festgelegt sein. Beispiel:

```XML
<ContentDefinition Id="api.selfasserted">
  <LoadUri>~/tenant/default/selfAsserted.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:selfasserted:2.0.0</DataUri>
  ...
```

## <a name="defining-display-controls"></a>Definieren von Anzeigesteuerelementen

Das **DisplayControl**-Element enthält die folgenden Attribute:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| Id | Ja | Ein Bezeichner, der für das Anzeigesteuerelement verwendet wird. Auf diesen kann [verwiesen](#referencing-display-controls) werden. |
| UserInterfaceControlType | Ja | Der Typ des Anzeigesteuerelements. Derzeit wird [VerificationControl](display-control-verification.md) unterstützt. |

Das **DisplayControl**-Element enthält die folgenden Elemente:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ----------- | ----------- |
| InputClaims | 0:1 | **InputClaims** wird verwendet, um den Wert der Ansprüche, die vom Benutzer erfasst werden sollen, vorab auszufüllen. |
| DisplayClaims | 0:1 | **DisplayClaims** wird verwendet, um die Ansprüche darzustellen, die vom Benutzer erfasst werden sollen. |
| OutputClaims | 0:1 | **OutputClaims** wird verwendet, um die Ansprüche darzustellen, die temporär für **DisplayControl** gespeichert werden sollen. |
| Aktionen | 0:1 | **Actions** wird verwendet, um die technischen Validierungsprofile aufzulisten, die für Benutzeraktionen am Front-End aufgerufen werden sollen. |

### <a name="input-claims"></a>Eingabeansprüche

In einem Anzeigesteuerelement können Sie **InputClaims**-Elemente verwenden, um den Wert von Ansprüchen vorab auszufüllen, die vom Benutzer der Seite erfasst werden sollen. **InputClaimsTransformations**-Elemente können im selbstbestätigten technischen Profil definiert werden, das auf dieses Anzeigesteuerelement verweist.

Im folgenden Beispiel wird die E-Mail-Adresse, die überprüft werden soll, mit der bereits vorhandenen Adresse vorab aufgefüllt.

```XML
<DisplayControl Id="emailControl" UserInterfaceControlType="VerificationControl">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="emailAddress" />
  </InputClaims>
  ...
```

### <a name="display-claims"></a>Anzeigeansprüche

Jede Art von Anzeigesteuerelement erfordert andere Anzeigeansprüche, [Ausgabeansprüche](#output-claims) und [Aktionen](#display-control-actions), die ausgeführt werden müssen.

Ähnlich wie die **Anzeigeansprüche** in einem [selbstbestätigten technischen Profil](self-asserted-technical-profile.md#display-claims) sind die Anzeigeansprüche die Ansprüche, die im Anzeigesteuerelement vom Benutzer erfasst werden sollen. Das **ClaimType**-Element, auf das verwiesen wird, muss das **UserInputType**-Element für einen von Azure AD B2C unterstützten Benutzereingabetyp angeben, wie etwa `TextBox` oder `DropdownSingleSelect`. Wenn der Wert eines Anzeigeanspruchs für ein **Action**-Element erforderlich ist, legen Sie das **Required**-Attribut auf `true` fest, damit der Benutzer einen Wert für diesen Anzeigeanspruch angeben muss.

Bestimmte Anzeigeansprüche sind für bestimmte Arten von Anzeigesteuerelementen erforderlich. **VerificationCode** ist beispielsweise für **VerificationControl**-Anzeigesteuerelemente erforderlich. Verwenden Sie das-Attribut **ControlClaimType**, um anzugeben, welches DisplayClaim-Element für den erforderlichen Anspruch festgelegt ist. Beispiel:

```XML
<DisplayClaim ClaimTypeReferenceId="otpCode" ControlClaimType="VerificationCode" Required="true" />
```

### <a name="output-claims"></a>Ausgabeansprüche

Die **Ausgabeansprüche** eines Anzeigesteuerelements werden nicht an den nächsten Orchestrierungsschritt gesendet. Sie werden nur für die aktuelle Sitzung gespeichert, in der das Anzeigesteuerelement verwendet wird. Diese temporären Ansprüche können von den verschiedenen Aktionen desselben Anzeigesteuerelements gemeinsam genutzt werden.

Verwenden Sie das **OutputClaims**-Element des selbstbestätigten technischen Profils, das auf dieses Anzeigesteuerelement verweist, um die Ausgabeansprüche an den nächsten Orchestrierungsschritt weiterzugeben.

### <a name="display-control-actions"></a>Actions-Anzeigesteuerelement

Die **Actions**-Elemente eines Anzeigesteuerelements sind Prozeduren, die im Back-End von Azure AD B2C auftreten, wenn ein Benutzer clientseitig (also im Browser) bestimmte Aktionen durchführt. Dazu zählen beispielsweise die Validierungen, die durchgeführt werden müssen, wenn der Benutzer auf eine Schaltfläche auf der Seite klickt.

Eine Aktion definiert eine Liste mit **technischen Validierungsprofilen**. Diese werden zum Validieren einiger oder aller Anzeigeansprüche des Anzeigesteuerelements verwendet. Das technische Validierungsprofil überprüft die Benutzereingabe und kann einen Fehler an den Benutzer zurückgeben. Sie können **ContinueOnError**, **ContinueOnSuccess** und **Preconditions** so im Action-Element des Anzeigesteuerelements verwenden, wie diese in [technischen Validierungsprofilen](validation-technical-profile.md) in einem selbstbestätigten technischen Profil verwendet werden.

Im folgenden Beispiel wird ein Code abhängig von der Auswahl des Benutzers im **mfaType**-Anspruch per E-Mail oder SMS gesendet.

```XML
<Action Id="SendCode">
  <ValidationClaimsExchange>
    <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="AzureMfa-SendSms">
      <Preconditions>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
          <Value>mfaType</Value>
          <Value>email</Value>
          <Action>SkipThisValidationTechnicalProfile</Action>
        </Precondition>
      </Preconditions>
    </ValidationClaimsExchangeTechnicalProfile>
    <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="AadSspr-SendEmail">
      <Preconditions>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
          <Value>mfaType</Value>
          <Value>phone</Value>
          <Action>SkipThisValidationTechnicalProfile</Action>
        </Precondition>
      </Preconditions>
    </ValidationClaimsExchangeTechnicalProfile>
  </ValidationClaimsExchange>
</Action>
```

## <a name="referencing-display-controls"></a>Verweisen auf Anzeigesteuerelemente

Auf Anzeigesteuerelemente wird in den [Anzeigeansprüchen](self-asserted-technical-profile.md#display-claims) des [selbstbestätigten technischen Profils](self-asserted-technical-profile.md) verwiesen.

Beispiel:

```XML
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
  ...
  <DisplayClaims>
    <DisplayClaim DisplayControlReferenceId="emailVerificationControl" />
    <DisplayClaim DisplayControlReferenceId="PhoneVerificationControl" />
    <DisplayClaim ClaimTypeReferenceId="displayName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="givenName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="surName" Required="true" />
```
