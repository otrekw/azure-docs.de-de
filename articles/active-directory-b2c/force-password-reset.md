---
title: Konfigurieren eines Flows zum Erzwingen der Kennwortzurücksetzung in Azure AD B2C
titleSuffix: Azure AD B2C
description: Es wird beschrieben, wie Sie in Azure Active Directory B2C einen Flow zum Erzwingen der Kennwortzurücksetzung einrichten.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/03/2021
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 0477153b466702bec0fa2d5221fee1e054d06314
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102033764"
---
# <a name="set-up-a-force-password-reset-flow-in-azure-active-directory-b2c"></a>Einrichten eines Flows zum Erzwingen der Kennwortzurücksetzung in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

Als Administrator können Sie [das Kennwort für einen Benutzer zurücksetzen](manage-users-portal.md#reset-a-users-password), wenn der Benutzer sein Kennwort vergessen hat. Es kann auch sein, dass Sie die Kennwortzurücksetzung erzwingen möchten. In diesem Artikel wird beschrieben, wie Sie in diesen Szenarien eine Kennwortzurücksetzung erzwingen.

## <a name="overview"></a>Übersicht

Wenn ein Administrator das Kennwort eines Benutzers über das Azure-Portal zurücksetzt, wird der Wert des Attributs [forceChangePasswordNextSignIn](user-profile-attributes.md#password-profile-property) auf `true` festgelegt.

Während des [Anmeldungs- und Registrierungsablaufs](add-sign-up-and-sign-in-policy.md) wird der Wert dieses Attributs überprüft. Nachdem der Benutzer die Anmeldung durchgeführt hat, muss er sein Kennwort zurücksetzen, wenn das Attribut auf `true` festgelegt ist. Anschließend wird der Wert des Attributs wieder auf `false` festgelegt.

![Flow zum Erzwingen der Kennwortzurücksetzung](./media/force-password-reset/force-password-reset-flow.png)

Der Flow für die Kennwortzurücksetzung wird für lokale Konten in Azure AD B2C verwendet, bei denen für die Anmeldung eine [E-Mail-Adresse](identity-provider-local.md#email-sign-in) oder ein [Benutzername](identity-provider-local.md#username-sign-in) und ein Kennwort verwendet werden.

### <a name="force-a-password-reset-after-90-days"></a>Erzwingen einer Kennwortzurücksetzung nach 90 Tagen

Als Administrator können Sie [MS Graph](microsoft-graph-operations.md) verwenden, um die Gültigkeitsdauer eines Benutzerkennworts auf 90 Tage festzulegen. Nach 90 Tagen wird der Wert des Attributs [forceChangePasswordNextSignIn](user-profile-attributes.md#password-profile-property) automatisch auf `true` festgelegt. Weitere Informationen zum Festlegen einer Kennwortablaufrichtlinie für einen Benutzer finden Sie unter [Attribut „passwordPolicies“/Kennwortrichtlinienattribut](user-profile-attributes.md#password-policy-attribute).

Nachdem eine Kennwortablaufrichtlinie festgelegt wurde, müssen Sie auch den Flow zum Erzwingen der Kennwortzurücksetzung konfigurieren. Dies ist in diesem Artikel beschrieben.  

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="configure-your-policy"></a>Konfigurieren der Richtlinie

::: zone pivot="b2c-user-flow"

Gehen Sie wie folgt vor, um die Einstellung **Erzwungene Kennwortzurücksetzung** in einem Flow für die Registrierung oder Anmeldung zu aktivieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie **Benutzerflows** aus.
1. Wählen Sie den (**empfohlenen**) Benutzerflow für die Registrierung/Anmeldung oder nur für die Anmeldung aus, den Sie anpassen möchten.
1. Wählen Sie im linken Menü unter **Einstellungen** die Option **Eigenschaften** aus.
1. Wählen Sie unter **Kennwortkomplexität** die Option **Erzwungene Kennwortzurücksetzung** aus.
1. Wählen Sie **Speichern** aus.

### <a name="test-the-user-flow"></a>Testen des Benutzerflows

1. Melden Sie sich als Benutzer- oder Kennwortadministrator beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zu den verfügbaren Rollen finden Sie unter [Zuweisen von Administratorrollen in Azure AD](../active-directory/roles/permissions-reference.md#all-roles).
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie **Benutzer** aus. Suchen Sie nach dem Benutzer, den Sie zum Testen der Kennwortzurücksetzung verwenden möchten, und wählen Sie ihn aus. Wählen Sie anschließend die Option **Kennwort zurücksetzen** aus.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie **Benutzerflows** aus.
1. Wählen Sie einen (**empfohlenen**) Benutzerflow für die Registrierung oder Anmeldung aus, den Sie testen möchten.
1. Wählen Sie **Benutzerflow ausführen** aus.
1. Wählen Sie für **Anwendung** die Webanwendung *webapp1* aus, die Sie zuvor registriert haben. Als **Antwort-URL** sollte `https://jwt.ms` angezeigt werden.
1. Wählen Sie **Benutzerflow ausführen** aus.
1. Melden Sie sich mit dem Benutzerkonto an, für das Sie das Kennwort zurücksetzen.
1. Nun müssen Sie das Kennwort für den Benutzer ändern. Ändern Sie das Kennwort, und klicken Sie auf **Weiter**. Das Token wird an `https://jwt.ms` zurückgegeben und sollte Ihnen angezeigt werden.

::: zone-end

::: zone pivot="b2c-custom-policy"

1. Verwenden Sie das Beispiel für das Erzwingen der Kennwortzurücksetzung auf [GitHub](https://github.com/azure-ad-b2c/samples/tree/master/policies/force-password-reset).
1. Ersetzen Sie in jeder Datei die Zeichenfolge `yourtenant` durch den Namen Ihres Azure AD B2C-Mandanten. Wenn der Name des B2C-Mandanten z.B. *contosob2c* lautet, werden alle Instanzen von `yourtenant.onmicrosoft.com` zu `contosob2c.onmicrosoft.com`.
1. Laden Sie die Richtliniendateien in der folgenden Reihenfolge hoch: die Erweiterungsrichtlinie `TrustFrameworkExtensionsCustomForcePasswordReset.xml`und dann die Richtlinie für die vertrauende Seite `SignUpOrSigninCustomForcePasswordReset.xml`.

### <a name="test-the-policy"></a>Testen der Richtlinie

1. Melden Sie sich als Benutzer- oder Kennwortadministrator beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zu den verfügbaren Rollen finden Sie unter [Zuweisen von Administratorrollen in Azure AD](../active-directory/roles/permissions-reference.md#all-roles).
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie **Benutzer** aus. Suchen Sie nach dem Benutzer, den Sie zum Testen der Kennwortzurücksetzung verwenden möchten, und wählen Sie ihn aus. Wählen Sie anschließend die Option **Kennwort zurücksetzen** aus.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie unter **Richtlinien** die Option **Identity Experience Framework** aus.
1. Wählen Sie die Richtlinie `B2C_1A_signup_signin_Custom_ForcePasswordReset` aus, um sie zu öffnen. 
1. Wählen Sie für **Anwendung** eine Webanwendung aus, die Sie [zuvor registriert haben](troubleshoot-custom-policies.md#troubleshoot-the-runtime). Als **Antwort-URL** sollte `https://jwt.ms` angezeigt werden.
1. Wählen Sie die Schaltfläche **Jetzt ausführen** aus.
1. Melden Sie sich mit dem Benutzerkonto an, für das Sie das Kennwort zurücksetzen.
1. Nun müssen Sie das Kennwort für den Benutzer ändern. Ändern Sie das Kennwort, und klicken Sie auf **Weiter**. Das Token wird an `https://jwt.ms` zurückgegeben und sollte Ihnen angezeigt werden.

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

Richten Sie eine [Self-Service-Kennwortzurücksetzung](add-password-reset-policy.md) ein.
