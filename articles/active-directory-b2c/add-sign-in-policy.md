---
title: Einrichten eines Anmeldeflows
titleSuffix: Azure Active Directory B2C
description: Hier erfahren Sie, wie Sie einen Anmeldeflow in Azure Active Directory B2C einrichten.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/12/2021
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 2956f0ffb562214477249da3198ebbe42ef9bb45
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2021
ms.locfileid: "98660368"
---
# <a name="set-up-a-sign-in-flow-in-azure-active-directory-b2c"></a>Einrichten eines Anmeldeflows in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

## <a name="sign-in-flow-overview"></a>Übersicht über den Anmeldeflow

Die Anmelderichtlinie ermöglicht Benutzern Folgendes: 

* Benutzer können sich mit einem lokalen Azure AD B2C-Konto anmelden.
* Registrieren oder Anmelden mit einem Social Media-Konto
* Zurücksetzen von Kennwörtern
* Benutzer können sich nicht für ein lokales Azure AD B2C-Konto registrieren. Zum Erstellen eines Kontos kann ein Administrator die [Microsoft Graph-API](microsoft-graph-operations.md) verwenden.

![Profilbearbeitungsflow](./media/add-sign-in-policy/sign-in-user-flow.png)

## <a name="prerequisites"></a>Voraussetzungen

Wenn dies noch nicht erfolgt ist, [registrieren Sie eine Webanwendung in Azure Active Directory B2C](tutorial-register-applications.md).

::: zone pivot="b2c-user-flow"

## <a name="create-a-sign-in-user-flow"></a>Erstellen eines Benutzerflows für die Anmeldung

Gehen Sie wie folgt vor, um eine Anmelderichtlinie hinzuzufügen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie unter **Richtlinien** die Option **Benutzerflows** und dann **Neuer Benutzerflow** aus.
1. Wählen Sie auf der Seite **Benutzerflow erstellen** den Benutzerflow **Anmeldung** aus.
1. Wählen Sie unter **Version auswählen** die Option **Empfohlen** und dann **Erstellen** aus. (Weitere Informationen zu [Benutzerflowversionen](user-flow-versions.md))
1. Geben Sie unter **Name** einen Namen für den Benutzerflow ein. Beispiel: *signupsignin1*.
1. Klicken Sie unter **Identitätsanbieter** auf **Email sign-in** (E-Mail-Anmeldung).
1. Wählen Sie für **Anwendungsansprüche** die Ansprüche und Attribute aus, die an Ihre Anwendung gesendet werden sollen. Beispiel: Wählen Sie **Mehr anzeigen** aus, und wählen Sie dann Attribute und Ansprüche für **Anzeigename**, **Vorname**, **Nachname** und **Objekt-ID des Benutzers** aus. Klicken Sie auf **OK**.
1. Klicken Sie auf **Erstellen**, um den Benutzerflow hinzuzufügen. Dem Namen wird automatisch das Präfix *B2C_1* vorangestellt.

### <a name="test-the-user-flow"></a>Testen des Benutzerflows

1. Wählen Sie den von Ihnen erstellten Benutzerflow aus, um die entsprechende Übersichtsseite zu öffnen, und wählen Sie dann **Benutzerflow ausführen** aus.
1. Wählen Sie für **Anwendung** die Webanwendung *webapp1* aus, die Sie zuvor registriert haben. Als **Antwort-URL** sollte `https://jwt.ms` angezeigt werden.
1. Klicken Sie auf **Benutzerflow ausführen**.
1. Sie sollten sich mit dem von Ihnen (mit der Microsoft Graph-API) erstellten Konto ohne den Registrierungslink anmelden können. Das zurückgegebene Token enthält die von Ihnen ausgewählten Ansprüche.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="remove-the-sign-up-link"></a>Entfernen des Registrierungslinks

Das technische Profil **SelfAsserted-LocalAccountSignin-Email** ist ein [sebstbestätigtes Profil](self-asserted-technical-profile.md), das beim Registrierungs- oder Anmeldeflow aufgerufen wird. Um den Registrierungslink zu entfernen, legen Sie die `setting.showSignupLink`-Metadaten auf `false` fest. Überschreiben Sie in der Erweiterungsdatei die technischen Profile in SelfAsserted-LocalAccountSignin-Email. 

1. Öffnen Sie die Erweiterungsdatei Ihrer Richtlinie. Beispiel: _`SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`**_.
1. Suchen Sie das Element `ClaimsProviders`. Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
1. Fügen Sie dem `ClaimsProviders`-Element die folgenden Anspruchsanbieter hinzu:

    ```xml
    <ClaimsProvider>
      <DisplayName>Local Account</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
          <Metadata>
            <Item Key="setting.showSignupLink">false</Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Fügen Sie innerhalb des `<BuildingBlocks>`-Elements die folgende [ContentDefinition](contentdefinitions.md) hinzu, um auf den Daten-URI der Version 1.2.0 (oder höher) zu verweisen:

    ```XML
    <ContentDefinitions>
     <ContentDefinition Id="api.localaccountsignup">
        <DataUri>urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.2.0</DataUri>
      </ContentDefinition>
    </ContentDefinitions>
    ```

## <a name="update-and-test-your-policy"></a>Aktualisieren und Testen Ihrer Richtlinie

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD-Mandanten enthält, indem Sie im oberen Menü den **Verzeichnis- und Abonnementfilter** und dann das Verzeichnis auswählen, das Ihren Azure AD-Mandanten enthält.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **App-Registrierungen**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Framework für die Identitätsfunktion** aus.
1. Wählen Sie **Benutzerdefinierte Richtlinie hochladen** aus, und laden Sie dann die beiden geänderten Richtliniendateien hoch.
1. Wählen Sie die hochgeladene Anmelderichtlinie aus, und klicken Sie auf die Schaltfläche **Jetzt ausführen**.
1. Sie sollten sich mit dem von Ihnen (mit der Microsoft Graph-API) erstellten Konto ohne den Registrierungslink anmelden können.

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

* [Hinzufügen eines Identitätsanbieters zu Ihrem Azure Active Directory B2C-Mandanten](add-identity-provider.md)
* [Einrichten eines Kennwortzurücksetzungsflows in Azure Active Directory B2C](add-password-reset-policy.md)
