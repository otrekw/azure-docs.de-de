---
title: Telefonregistrierung und -anmeldung mit benutzerdefinierten Richtlinien (Vorschau)
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie mit benutzerdefinierten Richtlinien in Azure Active Directory B2C Einmalkennwörter (One-Time Passwords, OTP) in Textnachrichten an die Telefone Ihrer Anwendungsbenutzer senden.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: eadac0e973b361b1fdee63dcc9cfa848a0b2bacb
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78183957"
---
# <a name="set-up-phone-sign-up-and-sign-in-with-custom-policies-in-azure-ad-b2c-preview"></a>Einrichten von Telefonregistrierung und -anmeldung mit benutzerdefinierten Richtlinien in Azure AD B2C (Vorschau)

Die Telefonregistrierung und -anmeldung in Azure Active Directory B2C (Azure AD B2C) ermöglicht es Ihren Benutzern, sich mit einem Einmalkennwort (One-Time Password, OTP) bei Ihren Anwendungen zu registrieren und anzumelden, das in einer Textnachricht an ihr Telefon gesendet wird. Mithilfe von Einmalkennwörtern können Sie das Risiko minimieren, dass Ihre Benutzer ihre Kennwörter vergessen oder versehentlich offenlegen.

Befolgen Sie die Schritte in diesem Artikel, um Ihren Kunden mithilfe benutzerdefinierter Richtlinien zu ermöglichen, sich mit einem an ihr Telefon gesendeten Einmalkennwort bei Ihren Anwendungen zu registrieren und anzumelden.

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="pricing"></a>Preise

Einmalkennwörter werden mithilfe von SMS-Textnachrichten an Ihre Benutzer gesendet. Es fallen möglicherweise für jede gesendete Nachricht Gebühren an. Preisinformationen finden Sie im Abschnitt **Separate Gebühren** unter [Azure Active Directory B2C – Preise](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="prerequisites"></a>Voraussetzungen

Vor dem Einrichten von OTP benötigen Sie die folgenden Ressourcen.

* [Azure AD B2C-Mandant](tutorial-create-tenant.md)
* In Ihrem Mandanten [registrierte Webanwendung](tutorial-register-applications.md)
* [Benutzerdefinierte Richtlinien](custom-policy-get-started.md), die in Ihren Mandanten hochgeladen wurden

## <a name="get-the-phone-sign-up--sign-in-starter-pack"></a>Abrufen des Starter Packs für die Registrierung und Anmeldung

Aktualisieren Sie zunächst die benutzerdefinierten Richtliniendateien für die Telefonregistrierung und -anmeldung, damit diese mit Ihrem Azure AD B2C-Mandanten funktionieren.

In den folgenden Schritten wird davon ausgegangen, dass Sie die [Voraussetzungen](#prerequisites) abgeschlossen und das Repository für das [benutzerdefinierte Richtlinien-Starter Pack][starter-pack] auf Ihren lokalen Computer geklont haben.

1. Suchen Sie die [benutzerdefinierten Richtliniendateien für die Telefonregistrierung und -anmeldung][starter-pack-phone] in Ihrem lokalen Klon des Starter Pack-Repositorys, oder laden Sie sie direkt herunter. Die XML-Richtliniendateien befinden sich im folgenden Verzeichnis:

    `active-directory-b2c-custom-policy-starterpack/scenarios/`**`phone-number-passwordless`**

1. Ersetzen Sie in jeder Datei die Zeichenfolge `yourtenant` durch den Namen Ihres Azure AD B2C-Mandanten. Wenn der Name des B2C-Mandanten z.B. *contosob2c* lautet, werden alle Instanzen von `yourtenant.onmicrosoft.com` zu `contosob2c.onmicrosoft.com`.

1. Führen Sie die Schritte im Abschnitt [Hinzufügen von Anwendungs-IDs zur benutzerdefinierten Richtlinie](custom-policy-get-started.md#add-application-ids-to-the-custom-policy) von [Erste Schritte mit benutzerdefinierten Richtlinien in Azure Active Directory B2C](custom-policy-get-started.md) aus. Aktualisieren Sie in diesem Fall `/phone-number-passwordless/` **`Phone_Email_Base.xml`** mit den **Anwendungs-IDs (Client-IDs)** der beiden Anwendungen, die Sie beim Abschließen der Voraussetzungen registriert haben: *IdentityExperienceFramework* und *ProxyIdentityExperienceFramework*.

## <a name="upload-the-policy-files"></a>Hochladen der Richtliniendateien

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem Azure AD B2C-Mandanten.
1. Wählen Sie unter **Richtlinien** die Option **Identity Experience Framework** aus.
1. Wählen Sie **Benutzerdefinierte Richtlinie hochladen** aus.
1. Laden Sie die Richtliniendateien in der folgenden Reihenfolge hoch:
    1. *Phone_Email_Base.xml*
    1. *SignUpOrSignInWithPhone.xml*
    1. *SignUpOrSignInWithPhoneOrEmail.xml*
    1. *ProfileEditPhoneOnly.xml*
    1. *ProfileEditPhoneEmail.xml*
    1. *ChangePhoneNumber.xml*
    1. *PasswordResetEmail.xml*

Beim Hochladen der einzelnen Dateien fügt Azure das Präfix `B2C_1A_` hinzu.

## <a name="test-the-custom-policy"></a>Testen der benutzerdefinierten Richtlinie

1. Wählen Sie unter **Benutzerdefinierte Richtlinien** die Richtlinie **B2C_1A_SignUpOrSignInWithPhone** aus.
1. Wählen Sie unter **Anwendung auswählen** die Anwendung *webapp1* aus, die Sie beim Abschließen der Voraussetzungen registriert haben.
1. Wählen Sie für **Antwort-URL** die Option `https://jwt.ms` aus.
1. Wählen Sie **Jetzt ausführen** aus, und registrieren Sie sich mit einer E-Mail-Adresse oder Telefonnummer.
1. Wählen Sie **Jetzt ausführen** erneut aus, und melden Sie sich zur Bestätigung der richtigen Konfiguration mit demselben Konto an.

## <a name="get-user-account-by-phone-number"></a>Abrufen von Benutzerkonten anhand der Telefonnummer

Ein Benutzer, der sich mit einer Telefonnummer anmeldet, aber keine E-Mail-Adresse für die Wiederherstellung bereitstellt, wird in Ihrem Azure AD B2C-Verzeichnis mit seiner Telefonnummer als Anmeldename erfasst. Wenn der Benutzer seine Telefonnummer später ändern möchte, muss das Helpdesk- oder Supportteam zunächst sein Konto ermitteln und dann seine Telefonnummer aktualisieren.

Sie können einen Benutzer anhand der Telefonnummer (Anmeldename) mithilfe von [Microsoft Graph](manage-user-accounts-graph-api.md) ermitteln:

```http
GET https://graph.microsoft.com/v1.0/users?$filter=identities/any(c:c/issuerAssignedId eq '+{phone number}' and c/issuer eq '{tenant name}.onmicrosoft.com')
```

Beispiel:

```http
GET https://graph.microsoft.com/v1.0/users?$filter=identities/any(c:c/issuerAssignedId eq '+450334567890' and c/issuer eq 'contosob2c.onmicrosoft.com')
```

## <a name="next-steps"></a>Nächste Schritte

Sie finden das Starter Pack für die benutzerdefinierte Richtlinie für Telefonregistrierung und -anmeldung (und andere Starter Packs) auf GitHub:

[Azure-Samples/active-directory-b2c-custom-policy-starterpack/scenarios/phone-number-passwordless][starter-pack-phone]

Die Richtliniendateien des Starter Packs verwenden technische Multi-Factor Authentication-Profile und Telefonnummern-Anspruchstransformationen:

* [Definieren eines technischen Azure Multi-Factor Authentication-Profils](multi-factor-auth-technical-profile.md)
* [Definieren von Telefonnummern-Anspruchstransformationen](phone-number-claims-transformations.md)

<!-- LINKS - External -->
[starter-pack]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
[starter-pack-phone]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/phone-number-passwordless
