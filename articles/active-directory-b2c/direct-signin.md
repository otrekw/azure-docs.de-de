---
title: Einrichten einer direkten Anmeldung mit Azure Active Directory B2C | Microsoft-Dokumentation
description: Erfahren Sie, wie der Anmeldename aufgefüllt oder direkt zu einem sozialen Netzwerk als Identitätsanbieter umgeleitet wird.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/14/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 35e8efa269ab72477b06e86824d368d0a3dced03
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103197327"
---
# <a name="set-up-direct-sign-in-using-azure-active-directory-b2c"></a>Einrichten einer direkten Anmeldung mit Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

Beim Einrichten der Anmeldung für Ihre Anwendung mithilfe von Azure Active Directory (AD) B2C können Sie den Anmeldenamen auffüllen oder sich direkt bei einem bestimmten sozialen Netzwerk als Identitätsanbieter anmelden, wie z.B. bei einem Facebook-, LinkedIn- oder Microsoft-Konto.

## <a name="prepopulate-the-sign-in-name"></a>Auffüllen des Anmeldenamens

Während einer User Journey zur Anmeldung kann eine Anwendung der vertrauenden Seite auf einen bestimmten Benutzer oder Domänennamen ausgerichtet werden. Bei der Ausrichtung auf einen Benutzer kann eine Anwendung in der Autorisierungsanforderung den Abfrageparameter `login_hint` mit dem Anmeldenamen des Benutzers angeben. Azure AD B2C füllt den Anmeldenamen automatisch auf, während der Benutzer nur das Kennwort angeben muss.

![Seite für Anmeldung/Registrierung mit hervorgehobenem Abfrageparameter „login_hint“ in der URL](./media/direct-signin/login-hint.png)

Der Benutzer kann den Wert im Textfeld für die Anmeldung ändern.

::: zone pivot="b2c-custom-policy"

Um einen Parameter für Anmeldehinweise zu unterstützen, überschreiben Sie das technische `SelfAsserted-LocalAccountSignin-Email`-Profil. Legen Sie im Abschnitt `<InputClaims>` den „DefaultValue“ des „signInName“-Anspruchs auf `{OIDC:LoginHint}` fest. Die Variable `{OIDC:LoginHint}` enthält den Wert des Parameters `login_hint`. Azure AD B2C liest den Wert des „signInName“-Anspruchs und füllt das Textfeld „signInName“ auf.

```xml
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
      <InputClaims>
        <!-- Add the login hint value to the sign-in names claim type -->
        <InputClaim ClaimTypeReferenceId="signInName" DefaultValue="{OIDC:LoginHint}" />
      </InputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

::: zone-end

## <a name="redirect-sign-in-to-a-social-provider"></a>Umleiten einer Anmeldung zu einem Anbieter sozialer Netzwerke

Wenn Sie die User Journey für die Anmeldung bei Ihrer Anwendung so konfiguriert haben, dass Konten für soziale Netzwerke inbegriffen sind, wie z.B. Facebook, LinkedIn oder Google, können Sie den Parameter `domain_hint` angeben. Dieser Abfrageparameter enthält einen Hinweis für Azure AD B2C zu dem sozialen Netzwerk als Identitätsanbieter, das für die Anmeldung verwendet werden sollte. Wenn in der Anwendung beispielsweise `domain_hint=facebook.com` angegeben ist,erfolgt die Anmeldung direkt auf der Anmeldeseite von Facebook.

![Seite für Anmeldung/Registrierung mit hervorgehobenem Abfrageparameter „domain_hint“ in der URL](./media/direct-signin/domain-hint.png)

::: zone pivot="b2c-user-flow"

Der Parameter für den Domänenhinweis in der Abfragezeichenfolge kann auf eine der folgenden Domänen festgelegt werden:

- amazon.com
- facebook.com
- github.com
- google.com
- linkedin.com
- microsoft.com
- qq.com
- twitter.com
- wechat.com
- weibo.com 
- Informationen zu [OpenID Connect für generische Identitätsanbieter](identity-provider-generic-openid-connect.md) finden Sie unter [Domänenhinweis](identity-provider-generic-openid-connect.md#response-mode).

::: zone-end

::: zone pivot="b2c-custom-policy"

Sie können den Domänennamen mithilfe des XML-Elements `<Domain>domain name</Domain>` von einem beliebigen `<ClaimsProvider>` konfigurieren, um Domänenhinweisparameter zu unterstützen.

```xml
<ClaimsProvider>
    <!-- Add the domain hint value to the claims provider -->
    <Domain>facebook.com</Domain>
    <DisplayName>Facebook</DisplayName>
    <TechnicalProfiles>
    ...
```

::: zone-end

