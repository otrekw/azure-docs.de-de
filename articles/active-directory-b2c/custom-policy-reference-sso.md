---
title: Sitzungsverwaltung für einmaliges Anmelden mit benutzerdefinierten Richtlinien
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie SSO-Sitzungen mithilfe benutzerdefinierter Richtlinien in Azure AD B2C verwalten.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/09/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 80cf0d101a29de7fca9d4dd36e188a500d35e290
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79225486"
---
# <a name="single-sign-on-session-management-in-azure-active-directory-b2c"></a>Sitzungsverwaltung für einmaliges Anmelden in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Mithilfe der Sitzungsverwaltung für einmaliges Anmelden (Single Sign-On, SSO) in Azure Active Directory B2C (Azure AD B2C) kann ein Administrator die Interaktion mit einem Benutzer steuern, nachdem dieser bereits authentifiziert wurde. Der Administrator kann z. B. steuern, ob die Auswahl von Identitätsanbietern angezeigt wird, oder ob lokale Kontodetails erneut eingegeben werden müssen. In diesem Artikel wird das Konfigurieren der Einstellungen für einmaliges Anmelden für Azure AD B2C beschrieben.

Die SSO-Sitzungsverwaltung besteht aus zwei Teilen. Der erste Teil befasst sich mit den direkten Benutzerinteraktionen mit Azure AD B2C und der zweite Teil behandelt Benutzerinteraktionen mit externen Parteien, z. B. mit Facebook. Azure AD B2C setzt SSO-Sitzungen nicht außer Kraft oder umgeht diese, die von externen Parteien abgehalten werden. Stattdessen wird die Route über Azure AD B2C zur externen Partei „gespeichert“, wodurch vermieden wird, dass der Benutzer erneut aufgefordert werden muss, seinen Identitätsanbieter für soziale Netzwerke oder Unternehmen auszuwählen. Die ultimative SSO-Entscheidung verbleibt der externen Partei.

Die SSO-Sitzungsverwaltung verwendet dieselbe Semantik wie jedes andere technische Profil in benutzerdefinierten Richtlinien. Wenn ein Orchestrierungsschritt ausgeführt wird, wird das technische Profil, das dem Schritt zugeordnet ist, nach einem `UseTechnicalProfileForSessionManagement`-Verweis abgefragt. Falls vorhanden, wird der referenzierte SSO-Sitzungsanbieter überprüft, um festzustellen, ob der Benutzer ein Sitzungsteilnehmer ist. In diesem Fall wird der SSO-Sitzungsanbieter dazu verwendet, um die Sitzung wieder aufzufüllen. Wenn die Ausführung eines Orchestrierungsschritts abgeschlossen ist, wird entsprechend der Anbieter verwendet, um Informationen in der Sitzung zu speichern, wenn ein SSO-Sitzungsanbieter angegeben wurde.

Azure AD B2C hat eine Reihe von SSO-Sitzungsanbietern definiert, die verwendet werden können:

* NoopSSOSessionProvider
* DefaultSSOSessionProvider
* ExternalLoginSSOSessionProvider
* SamlSSOSessionProvider

SSO Verwaltungsklassen werden mithilfe des `<UseTechnicalProfileForSessionManagement ReferenceId="{ID}" />`-Elements eines technischen Profils angegeben.

## <a name="input-claims"></a>Eingabeansprüche

Das `InputClaims`-Element ist leer oder nicht vorhanden.

## <a name="persisted-claims"></a>Persistente Ansprüche

Ansprüche, die an die Anwendung zurückgegeben oder von Vorbedingungen in nachfolgenden Schritten verwendet werden müssen, sollten in der Sitzung gespeichert oder durch einen Lesevorgang vom Profil des Benutzers im Verzeichnis ergänzt werden. Die Verwendung persistenter Ansprüche stellt sicher, dass Ihre Authentifizierungsmethoden bei fehlenden Ansprüchen nicht fehlschlagen. Verwenden Sie das `<PersistedClaims>`-Element des technischen Profils, um Ansprüche in der Sitzung hinzuzufügen. Wenn der Anbieter dazu verwendet wird, um die Sitzung erneut aufzufüllen, werden die bestehen gebliebenen Ansprüche zum Anspruchsbehälter hinzugefügt.

## <a name="output-claims"></a>Ausgabeansprüche

`<OutputClaims>` dienen zum Abrufen von Ansprüchen aus der Sitzung.

## <a name="session-providers"></a>Sitzungsanbieter

### <a name="noopssosessionprovider"></a>NoopSSOSessionProvider

Wie der Name besagt, ist dieser Anbieter untätig. Dieser Anbieter kann zum Unterdrücken des SSO-Verhaltens für ein bestimmtes technisches Profil verwendet werden. Das folgende technische `SM-Noop`-Profil ist im [Starter Pack für benutzerdefinierte Richtlinien](custom-policy-get-started.md#custom-policy-starter-pack) enthalten.

```XML
<TechnicalProfile Id="SM-Noop">
  <DisplayName>Noop Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.NoopSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="defaultssosessionprovider"></a>DefaultSSOSessionProvider

Dieser Anbieter kann zum Speichern von Ansprüchen in einer Sitzung verwendet werden. Auf diesen Anbieter wird in der Regel in einem technischen Profil verwiesen, über das lokale Konten verwaltet werden. Das folgende technische `SM-AAD`-Profil ist im [Starter Pack für benutzerdefinierte Richtlinien](custom-policy-get-started.md#custom-policy-starter-pack) enthalten.

```XML
<TechnicalProfile Id="SM-AAD">
  <DisplayName>Session Mananagement Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="objectId" />
    <PersistedClaim ClaimTypeReferenceId="signInName" />
    <PersistedClaim ClaimTypeReferenceId="authenticationSource" />
    <PersistedClaim ClaimTypeReferenceId="identityProvider" />
    <PersistedClaim ClaimTypeReferenceId="newUser" />
    <PersistedClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" />
  </PersistedClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectIdFromSession" DefaultValue="true"/>
  </OutputClaims>
</TechnicalProfile>
```

Das folgende technische `SM-MFA`-Profil ist im [Starter Pack für benutzerdefinierte Richtlinien](custom-policy-get-started.md#custom-policy-starter-pack) `SocialAndLocalAccountsWithMfa`enthalten. Dieses technische Profil verwaltet die Multi-Factor Authentication-Sitzung.

```XML
<TechnicalProfile Id="SM-MFA">
  <DisplayName>Session Mananagement Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="Verified.strongAuthenticationPhoneNumber" />
  </PersistedClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isActiveMFASession" DefaultValue="true"/>
  </OutputClaims>
</TechnicalProfile>
```

### <a name="externalloginssosessionprovider"></a>ExternalLoginSSOSessionProvider

Dieser Anbieter dient zum Unterdrücken des Bildschirms „Identitätsanbieter auswählen“. Er wird in der Regel in einem technischen Profil für einen externen Identitätsanbieter referenziert, z. B. Facebook. Das folgende technische `SM-SocialLogin`-Profil ist im [Starter Pack für benutzerdefinierte Richtlinien](custom-policy-get-started.md#custom-policy-starter-pack) enthalten.

```XML
<TechnicalProfile Id="SM-SocialLogin">
  <DisplayName>Session Mananagement Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="AlwaysFetchClaimsFromProvider">true</Item>
  </Metadata>
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="AlternativeSecurityId" />
  </PersistedClaims>
</TechnicalProfile>
```

#### <a name="metadata"></a>Metadaten

| attribute | Erforderlich | BESCHREIBUNG|
| --- | --- | --- |
| AlwaysFetchClaimsFromProvider | Nein | Derzeit nicht verwendet, kann ignoriert werden. |

### <a name="samlssosessionprovider"></a>SamlSSOSessionProvider

Dieser Anbieter wird in Azure AD B2C zum Verwalten der SAML-Sitzungen zwischen einer Anwendung der vertrauenden Seite oder einem SAML-Verbundidentitätsanbieter verwendet. Wenn der SSO-Anbieter zum Speichern einer SAML-Identitätsanbietersitzung verwendet wird, muss `RegisterServiceProviders` auf `false` eingestellt sein. Das folgende technische `SM-Saml-idp`-Profil wird vom [technischen SAML-Profil](saml-technical-profile.md) verwendet.

```XML
<TechnicalProfile Id="SM-Saml-idp">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="RegisterServiceProviders">false</Item>
  </Metadata>
</TechnicalProfile>
```

Wenn der Anbieter zum Speichern der B2C-SAML-Sitzung verwendet wird, muss `RegisterServiceProviders` auf `true` eingestellt sein. Für die Abmeldung von der SAML-Sitzung ist erforderlich, dass `SessionIndex` und `NameID` abgeschlossen sind.

Das folgende technische `SM-Saml-idp`-Profil wird vom [technischen Profil des SAML-Ausstellers](saml-issuer-technical-profile.md) verwendet.

```XML
<TechnicalProfile Id="SM-Saml-sp">
  <DisplayName>Session Management Provider</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>
</TechnicalProfile>
```
#### <a name="metadata"></a>Metadaten

| attribute | Erforderlich | BESCHREIBUNG|
| --- | --- | --- |
| IncludeSessionIndex | Nein | Derzeit nicht verwendet, kann ignoriert werden.|
| RegisterServiceProviders | Nein | Gibt an, dass der Anbieter alle SAML-Dienstanbieter registrieren soll, die eine Assertion ausgestellt haben. Mögliche Werte: `true` (Standard) oder `false`.|



