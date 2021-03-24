---
title: Hinzufügen eines persönlichen Microsoft-Kontos zur Microsoft Authenticator-App – Azure AD
description: Fügen Sie der Microsoft Authenticator-App persönliche Microsoft-Konten (z. B. für Outlook.com oder Xbox LIVE) hinzu, um Ihre Identität mit der zweistufigen Überprüfung zu überprüfen.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 04/08/2020
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: ff83a9a4e4bfd4c27dd3f8d3f212e489c3772eb4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "83741627"
---
# <a name="add-personal-microsoft-accounts-to-the-microsoft-authenticator-app"></a>Hinzufügen persönlicher Microsoft-Konten zur Microsoft Authenticator-App

Fügen Sie Ihre persönlichen Microsoft-Konten (z.B. für Outlook.com oder Xbox LIVE) zur Microsoft Authenticator-App hinzu – sowohl für die standardmäßige zweistufige Überprüfung als auch für die kennwortlose Anmeldung per Telefon.

- **Standardmäßige zweistufige Überprüfung** Geben Sie Ihren Benutzernamen und Ihr Kennwort auf dem Gerät ein, bei dem Sie sich anmelden möchten. Wählen Sie anschließend aus, ob die Microsoft Authenticator-App eine Benachrichtigung senden soll, oder ob Sie den zugehörigen Prüfcode aus dem Bildschirm **Konten** der Microsoft Authenticator-App kopieren möchten.

- **Anmeldung ohne Kennwort** Geben Sie den Benutzernamen Ihres persönlichen Microsoft-Kontos auf dem Gerät ein, bei dem Sie sich anmelden möchten. Verwenden Sie dann Ihr mobiles Gerät, um Ihre Identität per Fingerabdruck, Gesichtserkennung oder PIN zu bestätigen. Bei dieser Methode müssen Sie kein Kennwort eingeben.

>[!Important]
>Bevor Sie Ihr Konto hinzufügen können, müssen Sie die Microsoft Authenticator-App herunterladen und installieren. Wenn Sie dies noch nicht getan haben, führen Sie die im Artikel [Herunterladen und Installieren der App](user-help-auth-app-download-install.md) aufgeführten Schritte aus.

Sie können Ihr persönliches Microsoft-Konto hinzufügen, indem Sie zunächst die zweistufige Überprüfung aktivieren und dann das Konto zur App hinzufügen. Sie müssen die zweistufige Überprüfung nicht aktivieren, nur um die kennwortlose Anmeldung per Telefon für Ihr Konto zu verwenden. Es wird jedoch dringend empfohlen, die zweistufige Überprüfung für zusätzliche Kontosicherheit zu aktivieren.

## <a name="turn-on-two-factor-verification"></a>Aktivieren der zweistufigen Überprüfung

1. Wechseln Sie auf Ihrem Computer zur Seite [Grundlegendes zur Sicherheit](https://account.microsoft.com/security), und melden Sie sich mit Ihrem persönlichen Microsoft-Konto an. Beispiel: alain@outlook.com.

2. Klicken Sie unten auf der Seite **Grundlegendes zur Sicherheit** auf den Link **Weitere Sicherheitsoptionen**.

    ![Seite „Grundlegendes zur Sicherheit“ mit hervorgehobenem Link „Weitere Sicherheitsoptionen“](./media/user-help-auth-app-add-personal-ms-account/more-security-options-link.png)

3. Wechseln Sie zum Abschnitt **Zweistufige Überprüfung**, und aktivieren Sie das Feature, indem Sie es auf **Ein** festlegen. Sie können das Feature hier auch deaktivieren, wenn Sie es nicht mehr mit Ihrem persönlichen Konto verwenden möchten.

## <a name="add-your-microsoft-account-to-the-app"></a>Hinzufügen Ihres Microsoft-Kontos zur App

1. Öffnen Sie die Microsoft Authenticator-App auf Ihrem mobilen Gerät.

1. Wählen Sie in Android über das Symbol **Anpassen und steuern** in der oberen rechten Ecke die Option **Konto hinzufügen** aus.

    ![Android-Konto – Auswahlseiten](media/user-help-auth-app-add-personal-ms-account/customize-and-control-icon.png)

    Wählen Sie in iOS das Pluszeichen oben rechts aus.

    ![iOS-Version der Kontoauswahlumgebung](media/user-help-auth-app-add-personal-ms-account/customize-and-control-icon-ios.png)

1. Wählen Sie auf der Seite **Konto hinzufügen** die Option **Persönliches Konto** aus.

1. Wählen Sie **Bei Microsoft anmelden** aus, um Ihr Konto hinzuzufügen. Sofern verfügbar, kann auch ein QR-Code verwendet werden. Sie können Ihr Konto aber jederzeit hinzufügen, indem Sie sich mit Ihrem Benutzernamen und Kennwort anmelden.

    ![Wählen Sie entweder ein Microsoft-Konto aus, oder scannen Sie einen QR-Code, sofern ein solcher Code verfügbar ist.](media/user-help-auth-app-add-personal-ms-account/add-account-android.png)

1. Melden Sie sich über die entsprechende E-Mail-Adresse (z.B. alain@outlook.com) bei Ihrem persönlichen Konto an, und wählen Sie dann **Weiter** aus.

    >[!Note]
    >Wenn Sie kein persönliches Microsoft-Konto haben, können Sie [hier](https://account.microsoft.com/account/Account?refd=www.bing.com&ru=https%3A%2F%2Faccount.microsoft.com%2F%3Frefd%3Dwww.bing.com&destrt=home-index) ein Konto erstellen.

1. Geben Sie Ihr Kennwort ein, und wählen Sie dann **Anmelden** aus. Ihr persönliches Konto wird zur Microsoft Authenticator-App hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

- Nachdem Ihre Konten der App hinzugefügt wurden, können Sie sich unter Verwendung der Authenticator-App auf Ihrem Gerät anmelden. Weitere Informationen finden Sie unter [Anmelden mithilfe der App](user-help-auth-app-sign-in.md).

- Wenn Sie Probleme beim Abrufen des Prüfcodes für Ihr persönliches Microsoft-Konto haben, lesen Sie im Artikel **Sicherheitsinformation und Prüfcodes zum Microsoft-Konto** den Abschnitt [Problembehandlung beim Prüfcode](https://support.microsoft.com/help/12428/microsoft-account-security-info-verification-codes).

- Für iOS-Geräte können Sie Ihre Kontoanmeldeinformationen und zugehörige App-Einstellungen, etwa die Reihenfolge Ihrer Konten, in der Cloud sichern. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von Kontoanmeldeinformationen mithilfe der Microsoft Authenticator-App](user-help-auth-app-backup-recovery.md).
