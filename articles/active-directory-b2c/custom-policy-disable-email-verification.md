---
title: Deaktivieren der E-Mail-Überprüfung bei der Kundenregistrierung mit einer benutzerdefinierten Richtlinie
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie die E-Mail-Überprüfung bei der Benutzerregistrierung in Azure Active Directory B2C deaktivieren.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 51cb46d3ce1b74681c2ee3e53104cc57e73f1c5d
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84192229"
---
# <a name="disable-email-verification-during-customer-sign-up-using-a-custom-policy-in-azure-active-directory-b2c"></a>Deaktivieren der E-Mail-Überprüfung bei der Kundenregistrierung in Azure Active Directory B2C mit einer benutzerdefinierten Richtlinie

[!INCLUDE [disable email verification intro](../../includes/active-directory-b2c-disable-email-verification.md)]

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte unter [Erste Schritte mit benutzerdefinierten Richtlinien in Azure Active Directory B2C](custom-policy-get-started.md) aus. Sie sollten eine funktionierende benutzerdefinierte Richtlinie für die Registrierung und Anmeldung mit Social Media- und lokalen Konten haben.

## <a name="add-the-metadata-to-the-self-asserted-technical-profile"></a>Hinzufügen der Metadaten zum selbstbestätigten technischen Profil

Das technische Profil **LocalAccountSignUpWithLogonEmail** ist ein [sebstbestätigtes Profil](self-asserted-technical-profile.md), das beim Registrierungsflow aufgerufen wird. Um die E-Mail-Überprüfung zu deaktivieren, stellen Sie die Metadaten `EnforceEmailVerification` auf „false“ ein. Überschreiben Sie die technischen Profile „LocalAccountSignUpWithLogonEmail“ in der Erweiterungsdatei. 

1. Öffnen Sie die Erweiterungsdatei Ihrer Richtlinie. Beispiel: <em>`SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** </em>.
1. Suchen Sie das Element `ClaimsProviders`. Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
1. Fügen Sie dem `ClaimsProviders`-Element die folgenden Anspruchsanbieter hinzu:

```XML
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
      <Metadata>
        <Item Key="EnforceEmailVerification">false</Item>
      </Metadata>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="test-the-custom-policy"></a>Testen der benutzerdefinierten Richtlinie

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD-Mandanten enthält, indem Sie im oberen Menü den **Verzeichnis- und Abonnementfilter** und dann das Verzeichnis auswählen, das Ihren Azure AD-Mandanten enthält.
3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **App-Registrierungen**, und wählen Sie dann diese Option aus.
4. Wählen Sie **Framework für die Identitätsfunktion** aus.
5. Wählen Sie **Benutzerdefinierte Richtlinie hochladen** aus, und laden Sie dann die beiden geänderten Richtliniendateien hoch.
2. Wählen Sie die hochgeladene Registrierungs- oder Anmelderichtlinie aus, und klicken Sie auf die Schaltfläche **Jetzt ausführen**.
3. Sie sollten sich ohne Überprüfung mit einer E-Mail-Adresse registrieren können.


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [selbstbestätigten technischen Profilen](self-asserted-technical-profile.md) finden Sie in der IEF-Referenz.
