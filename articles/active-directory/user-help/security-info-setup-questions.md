---
title: Einrichten von Sicherheitsfragen als Überprüfungsmethode – Azure AD
description: Erfahren Sie, wie Sie Ihre Seite mit den Sicherheitsinformationen (Vorschau) einrichten, um Ihre Identität mithilfe vordefinierter Sicherheitsfragen als Überprüfungsmethode zu verifizieren.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 02/13/2019
ms.author: curtand
ms.openlocfilehash: 0c55f15d12ddc51573f59f56c742a8cac525af0b
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83744431"
---
# <a name="set-up-security-questions-as-your-verification-method"></a>Einrichten von Sicherheitsfragen als Überprüfungsmethode

Mit diesen Schritten können Sie Ihre Methode für die Kennwortzurücksetzung hinzufügen. Nachdem Sie die Ersteinrichtung abgeschlossen haben, können Sie zur Seite **Sicherheitsinformation** zurückkehren, um Sicherheitsinformationen hinzuzufügen, zu aktualisieren oder zu löschen.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Note]
>Wenn die Option für Sicherheitsfragen nicht angezeigt wird, ist es möglich, dass Ihre Organisation die Verwendung dieser Option nicht als Kennwortzurücksetzungsmethode zulässt. In diesem Fall müssen Sie eine andere Methode auswählen oder sich an den Helpdesk Ihrer Organisation wenden, um weitere Hilfe zu erhalten.

## <a name="security-verification-versus-password-reset-authentication"></a>Authentifizierung mittels Sicherheitsüberprüfung und Kennwortzurücksetzung im Vergleich

Sicherheitsinformationsmethoden werden sowohl für die zweistufige Sicherheitsüberprüfung als auch die Kennwortzurücksetzung verwendet. Es können jedoch nicht alle Methoden für beide Verfahren verwendet werden.

| Methode | Syntaxelemente |
| ------ | -------- |
| Authenticator-App | Authentifizierung mittels zweistufiger Überprüfung und Kennwortzurücksetzung. |
| Textnachrichten | Authentifizierung mittels zweistufiger Überprüfung und Kennwortzurücksetzung. |
| Telefonanrufe | Authentifizierung mittels zweistufiger Überprüfung und Kennwortzurücksetzung. |
| Sicherheitsschlüssel | Authentifizierung mittels zweistufiger Überprüfung und Kennwortzurücksetzung. |
| E-Mail-Konto | Nur Authentifizierung mittels Kennwortzurücksetzung. Sie müssen eine andere Methode für die zweistufige Überprüfung auswählen. |
| Sicherheitsfragen | Nur Authentifizierung mittels Kennwortzurücksetzung. Sie müssen eine andere Methode für die zweistufige Überprüfung auswählen. |

## <a name="set-up-your-security-questions-from-the-security-info-page"></a>Einrichten Ihrer Sicherheitsfragen auf der Seite „Sicherheitsinformation“

Je nach den Einstellungen Ihrer Organisation können Sie als eine Ihrer Methoden für Sicherheitsinformationen einige Sicherheitsfragen auswählen und beantworten. Der Administrator legt die Anzahl von Sicherheitsfragen fest, die Sie auswählen und beantworten müssen.

Wenn Sie Sicherheitsfragen verwenden, wird empfohlen, diese in Verbindung mit einer anderen Methode zu verwenden. Sicherheitsfragen können weniger sicher als andere Methoden sein, da einige Personen unter Umständen die Antworten auf Ihre Fragen kennen.

> [!Note]
> Sicherheitsfragen werden privat und sicher in einem Benutzerobjekt im Verzeichnis gespeichert und können nur während der Registrierung von Ihnen beantwortet werden. Es gibt keine Möglichkeit für Ihren Administrator, Ihre Fragen oder Antworten zu lesen oder zu ändern.
>
> Wird die Option für Sicherheitsfragen nicht angezeigt, lässt Ihre Organisation unter Umständen die Nutzung von Sicherheitsfragen für die Überprüfung nicht zu. In diesem Fall müssen Sie eine andere Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.
>
> Administratorkonten ist es nicht gestattet, Sicherheitsfragen als Methode zum Zurücksetzen des Kennworts zu verwenden. Wenn Sie als Konto der Administratorebene angemeldet sind, werden diese Optionen nicht angezeigt.

### <a name="to-set-up-your-security-questions"></a>So richten Sie Ihre Sicherheitsfragen ein

1. Melden Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto an, und rufen Sie die Seite https://myaccount.microsoft.com/ auf.

    ![Seite „Mein Profil“ mit hervorgehobenen Links zu Sicherheitsinformationen](media/security-info/securityinfo-myprofile-page.png)

2. Wählen Sie im linken Navigationsbereich den Eintrag **Sicherheitsinformation** oder den entsprechenden Link im Block **Sicherheitsinformation** aus, und klicken Sie dann auf der Seite **Sicherheitsinformation** auf **Methode hinzufügen**.

    ![Seite „Sicherheitsinformation“ mit hervorgehobener Option „Methode hinzufügen“](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Wählen Sie auf der Seite **Methode hinzufügen** in der Dropdownliste die Option **Sicherheitsfragen** aus, und wählen Sie dann **Hinzufügen**.

    ![Feld „Methode hinzufügen“ mit ausgewählten Sicherheitsfragen](media/security-info/securityinfo-myprofile-addquestions.png)

4. Wählen Sie auf der Seite **Sicherheitsfragen** Ihre Sicherheitsfragen aus, und beantworten Sie diese. Wählen Sie dann **Speichern**.

    ![Telefonnummer hinzufügen und Telefonanrufe wählen](media/security-info/securityinfo-myprofile-securityquestions.png)

    Ihre Sicherheitsinformationen werden aktualisiert, und Sie können bei der Kennwortzurücksetzung Sicherheitsfragen zur Bestätigung Ihrer Identität verwenden.

## <a name="delete-security-questions-from-your-security-info-methods"></a>Sicherheitsfragen aus Ihren Methoden für Sicherheitsinformationen löschen

Wenn Sie Ihre Sicherheitsfragen nicht mehr als Methode für Sicherheitsinformationen verwenden möchten, können Sie sie von der Seite **Sicherheitsinformation** entfernen.

>[!Important]
>Wenn Sie Ihre Sicherheitsfragen versehentlich löschen, gibt es keine Möglichkeit, diesen Vorgang rückgängig zu machen. In diesem Fall müssen Sie die Methode mit den Schritten im Abschnitt [Sicherheitsfragen einrichten](#set-up-your-security-questions-from-the-security-info-page) dieses Artikels erneut hinzufügen.

### <a name="to-delete-your-security-questions"></a>So löschen Sie Ihre Sicherheitsfragen

1. Klicken Sie auf der Seite **Sicherheitsinformation** neben der Option **Sicherheitsfragen** auf den Link **Löschen**.

    ![Link zum Löschen der E-Mail-Methode aus den Sicherheitsinformationen](media/security-info/securityinfo-myprofile-questionsdelete.png)

2. Klicken Sie im Bestätigungsfeld auf **Ja**, um Ihre **Sicherheitsfragen** zu löschen. Nachdem Ihre Sicherheitsfragen gelöscht wurden, wird die Methode aus den Sicherheitsinformationen entfernt und auf der Seite **Sicherheitsinformation** nicht mehr angezeigt.

## <a name="additional-security-info-methods"></a>Weitere Methoden für Sicherheitsinformationen

Basierend auf der Aktion, die Sie ausführen möchten, stehen zusätzliche Möglichkeiten zur Verfügung, wie Ihre Organisation sich mit Ihnen in Verbindung setzen kann, um Ihre Identität zu überprüfen. Die Optionen lauten:

- **Authentifikator-App:** Sie können eine Authentifikator-App herunterladen und verwenden, um entweder eine Genehmigungsbenachrichtigung oder einen nach dem Zufallsprinzip generierten Genehmigungscode für die zweistufige Überprüfung oder die Kennwortzurücksetzung zu erhalten. Ausführliche Anweisungen zum Einrichten und Verwenden der Microsoft Authenticator-App finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung einer Authentifikator-App](security-info-setup-auth-app.md).

- **SMS an ein mobiles Gerät:** Geben Sie die Nummer Ihres mobilen Geräts an, und Sie erhalten eine SMS mit dem Code, den Sie für die zweistufige Überprüfung oder die Kennwortzurücksetzung verwenden. Ausführliche Anweisungen dazu, wie Sie Ihre Identität mit einer Textnachricht (SMS) bestätigen, finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung von SMS](security-info-setup-text-msg.md).

- **Anruf bei einem mobilen Gerät oder einer geschäftlichen Telefonnummer:** Geben Sie die Nummer Ihres mobilen Geräts an, und erhalten Sie einen Telefonanruf für die zweistufige Überprüfung oder die Kennwortzurücksetzung. Ausführliche Anweisungen dazu, wie Sie Ihre Identität mit einer Telefonnummer bestätigen, finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung von Telefonanrufen](security-info-setup-phone-number.md).

- **Sicherheitsschlüssel:** Registrieren Sie Ihren Microsoft-kompatiblen Sicherheitsschlüssel, und verwenden Sie ihn zusammen mit einer PIN für die zweistufige Überprüfung oder die Kennwortzurücksetzung. Unter [Einrichten der Sicherheitsinformationen zur Verwendung eines Sicherheitsschlüssels (Vorschau)](security-info-setup-security-key.md) erfahren Sie Schritt für Schritt, wie Sie Ihre Identität mithilfe eines Sicherheitsschlüssels bestätigen.

- **E-Mail-Adresse:** Geben Sie Ihre Geschäfts-, Schul- oder Uni-E-Mail-Adresse an, um eine E-Mail für die Kennwortzurücksetzung zu erhalten. Diese Option ist für die zweistufige Überprüfung nicht verfügbar. Ausführliche Anweisungen zum Einrichten Ihrer E-Mail-Adresse finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung einer E-Mail-Adresse](security-info-setup-email.md).

    >[!Note]
    >Wenn einige dieser Optionen fehlen, sind die entsprechenden Methoden in Ihrer Organisation wahrscheinlich nicht zugelassen. In diesem Fall müssen Sie eine verfügbare Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

- Setzen Sie Ihr Kennwort zurück, wenn Sie es verloren oder vergessen haben. Verwenden Sie dazu das [Portal für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/), oder führen Sie die Schritte im Artikel [Reset your work or school password](active-directory-passwords-update-your-own-password.md) (Zurücksetzen des Kennworts eines Geschäfts-, Schul- oder Unikontos) aus.

- Der Artikel [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) enthält Tipps zur Problembehandlung bei Anmeldeproblemen.
