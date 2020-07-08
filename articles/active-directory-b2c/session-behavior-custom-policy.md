---
title: Konfigurieren des Sitzungsverhaltens mit benutzerdefinierten Richtlinien – Azure Active Directory B2C | Microsoft-Dokumentation
description: Konfigurieren des Sitzungsverhaltens mit benutzerdefinierten Richtlinien in Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: a2f20a4521efe2806c4bc66e4612b99caf84382a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85385262"
---
# <a name="configure-session-behavior-using-custom-policies-in-azure-active-directory-b2c"></a>Konfigurieren des Sitzungsverhaltens mit benutzerdefinierten Richtlinien in Azure Active Directory B2C

Mithilfe der [Sitzungsverwaltung für einmaliges Anmelden](session-overview.md) (Single Sign-On, SSO) in Azure Active Directory B2C (Azure AD B2C) kann ein Administrator die Interaktion mit einem Benutzer steuern, nachdem dieser bereits authentifiziert wurde. Der Administrator kann z. B. steuern, ob die Auswahl von Identitätsanbietern angezeigt wird oder ob Kontodetails erneut eingegeben werden müssen. In diesem Artikel wird das Konfigurieren der Einstellungen für einmaliges Anmelden für Azure AD B2C beschrieben.

## <a name="session-behavior-properties"></a>Eigenschaften des Sitzungsverhaltens

Sie können die folgenden Eigenschaften zum Verwalten von Webanwendungssitzungen verwenden:

- **Lebensdauer der Web-App-Sitzung (Minuten)** : Die Gültigkeitsdauer von Azure AD B2C-Sitzungscookies, die nach erfolgreicher Authentifizierung des Benutzers im Browser gespeichert werden
    - Standardwert = 86400 Sekunden (1440 Minuten).
    - Mindestwert (inkl.) = 900 Sekunden (15 Minuten).
    - Höchstwert (inkl.) = 86400 Sekunden (1440 Minuten).
- **Timeout für Web-App-Sitzung**: [Sitzungsablauftyp](session-overview.md#session-expiry-type), *Parallel* oder *Absolut*. 
- **SSO-Konfiguration**: [Sitzungsbereich](session-overview.md#session-scope) des SSO-Verhaltens über verschiedene Apps und Benutzerflows in Ihrem Azure AD B2C-Mandanten hinweg. 

## <a name="configure-the-properties"></a>Konfigurieren der Eigenschaften

Zum Ändern Ihres Sitzungsverhaltens und der SSO-Konfigurationen ist es erforderlich, dass Sie im [RelyingParty](relyingparty.md)-Element ein **UserJourneyBehaviors**-Element hinzufügen.  Das **UserJourneyBehaviors**-Element muss unmittelbar nach **DefaultUserJourney** eingefügt werden. Das **UserJourneyBehavors**-Element sollte wie in folgendem Beispiel aussehen:

```xml
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```

## <a name="single-sign-out"></a>Einmaliges Abmelden

### <a name="configure-the-applications"></a>Konfigurieren der Anwendungen

Wenn Sie den Benutzer zum Azure AD B2C-Abmeldeendpunkt (für OAuth2- und SAML-Protokolle) umleiten, löscht Azure AD B2C die Sitzung des Benutzers im Browser.  Um das [einmalige Abmelden](session-overview.md#single-sign-out) zuzulassen, legen Sie im Azure-Portal die `LogoutUrl` für die Anwendung fest:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
1. Wählen Sie Ihre Azure AD B2C-Instanz aus, indem Sie in der rechten oberen Ecke der Seite auf Ihr Konto klicken.
1. Wählen Sie im Menü auf der linken Seite **Azure AD B2C** > **App-Registrierungen** und dann Ihre Anwendung aus.
1. Wählen Sie **Einstellungen** > **Eigenschaften** aus, und suchen Sie das Textfeld **Abmelde-URL**. 

### <a name="configure-the-token-issuer"></a>Konfigurieren des Tokenausstellers 

Um das einmalige Abmelden zu unterstützen, müssen die technischen Profile des Tokenausstellers für JWT und SAML Folgendes angeben:

- Protokollname, z. B. `<Protocol Name="OpenIdConnect" />`
- Verweis auf das technische Profil für die Sitzung, z. B. `UseTechnicalProfileForSessionManagement ReferenceId="SM-OAuth-issuer" />`.

Das folgende Beispiel veranschaulicht die JWT- und SAML-Tokenaussteller mit einmaligem Abmelden:

```xml
<ClaimsProvider>
  <DisplayName>Local Account SignIn</DisplayName>
  <TechnicalProfiles>
    <!-- JWT Token Issuer -->
    <TechnicalProfile Id="JwtIssuer">
      <DisplayName>JWT token Issuer</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <OutputTokenFormat>JWT</OutputTokenFormat>
      ...    
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-OAuth-issuer" />
    </TechnicalProfile>

    <!-- Session management technical profile for OIDC based tokens -->
    <TechnicalProfile Id="SM-OAuth-issuer">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.OAuthSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </TechnicalProfile>

    <!--SAML token issuer-->
    <TechnicalProfile Id="Saml2AssertionIssuer">
      <DisplayName>SAML token issuer</DisplayName>
      <Protocol Name="SAML2" />
      <OutputTokenFormat>SAML2</OutputTokenFormat>
      ...
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-issuer" />
    </TechnicalProfile>

    <!-- Session management technical profile for SAML based tokens -->
    <TechnicalProfile Id="SM-Saml-issuer">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur [Azure AD B2C-Sitzung](session-overview.md).
