---
title: 'Azure AD: Registrieren von Authentifizierungsinformationen zum Zurücksetzen Ihres Kennworts'
description: Registrieren Sie Ihre Überprüfungsmethodeninformationen für die Self-Service-Kennwortzurücksetzung in Azure AD, damit Sie Ihr Kennwort ohne Administratorhilfe zurücksetzen können.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.assetid: ''
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 01/15/2020
ms.author: curtand
ms.openlocfilehash: 3deb59b04f3c0ba90e270bc064e3ece3c21229b3
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83739605"
---
# <a name="register-your-verification-method-info-to-reset-your-own-password"></a>Registrieren Ihrer Überprüfungsmethode zum Zurücksetzen Ihres Kennworts

Wenn Sie das Kennwort für Ihr Geschäfts-, Schul -oder Unikonto vergessen haben, von Ihrem Unternehmen keines erhalten haben oder aus Ihrem Konto ausgesperrt wurden, können Sie Ihre Sicherheitsinformation und Ihr mobiles Gerät verwenden, um das Kennwort Ihres Geschäfts-, Schul -oder Unikontos zurückzusetzen.

Ihr Administrator muss dieses Feature aktivieren, damit Sie Ihre Informationen registrieren und Ihr Kennwort zurücksetzen können. Wenn die Option **Kennwort vergessen** nicht angezeigt wird, bedeutet dies, dass Ihr Administrator das Feature für Ihre Organisation nicht aktiviert hat. Ist dies Ihrer Meinung nach nicht richtig, wenden Sie sich an den Helpdesk.

>[!Important]
>Dieser Artikel richtet sich an Benutzer, die versuchen, sich für die Self-Service-Kennwortzurücksetzung zu registrieren. Dies bedeutet, dass Sie das Kennwort für Ihr Geschäfts-, Schul- oder Unikonto (z. B. alain@contoso.com) ohne die Hilfe Ihres Administrators zurücksetzen können. Wenn Sie ein Administrator sind und nach Informationen zum Aktivieren der Self-Service-Kennwortzurücksetzung für Ihre Mitarbeiter oder andere Benutzer suchen, lesen Sie [Bereitstellen von Self-Service-Kennwortzurücksetzung in Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment) und weitere Artikel zu diesem Thema.

## <a name="set-up-your-password-reset-verification-method"></a>Einrichten Ihrer Überprüfungsmethode für die Kennwortzurücksetzung

1. Öffnen Sie auf Ihrem Gerät den Webbrowser, und navigieren Sie zur [Seite für die Sicherheitsinformationen](https://account.activedirectory.windowsazure.com/PasswordReset/Register.aspx?regref=ssprsetup).

2. Abhängig davon, wie Ihr Administrator Ihre Organisation eingerichtet hat, steht Ihnen mindestens eine der folgenden Optionen zur Verfügung, die Sie als Sicherheitsüberprüfungsmethode einrichten können. Wenn mehrere Optionen verfügbar sind, wird dringend empfohlen, mehr als eine als Sicherheitsüberprüfungsmethode zu verwenden, falls eine Ihrer Methoden nicht mehr verfügbar ist.

    - **Authentifizierungs-App:** Wählen Sie diese Option aus, um die Microsoft Authenticator-App oder eine andere Authentifikator-App als Sicherheitsüberprüfungsmethode zu verwenden. Weitere Informationen zum Einrichten der App finden Sie unter [Einrichten der Microsoft Authenticator-App als Überprüfungsmethode](security-info-setup-auth-app.md).

    - **SMS-Nachrichten:** Wählen Sie diese Option aus, um SMS-Nachrichten an Ihr mobiles Gerät zu senden. Weitere Informationen zum Einrichten von SMS finden Sie unter [Einrichten von SMS-Nachrichten als Überprüfungsmethode](security-info-setup-text-msg.md).

    - **Telefonanrufe:** Wählen Sie diese Option aus, um einen Telefonanruf an Ihre registrierte Telefonnummer zu erhalten. Weitere Informationen zum Einrichten von Telefonanrufen finden Sie unter [Einrichten einer Telefonnummer als Überprüfungsmethode](security-info-setup-phone-number.md).

    - **Sicherheitsschlüssel:** Wählen Sie diese Option aus, um einen Microsoft-kompatiblen Sicherheitsschlüssel zu verwenden. Weitere Informationen finden Sie unter [Einrichten eines Sicherheitsschlüssels als Überprüfungsmethode](security-info-setup-security-key.md).

    - **E-Mail-Adresse:** Wählen Sie diese Option aus, um eine alternative E-Mail-Adresse anzugeben, die ohne Eingabe eines Kennworts verwendet werden kann. Diese Option funktioniert nur für die Kennwortzurücksetzung und nicht als Sicherheitsüberprüfungsmethode. Weitere Informationen zum Einrichten einer E-Mail-Adresse finden Sie unter [Einrichten einer E-Mail-Adresse als Überprüfungsmethode](security-info-setup-email.md).

    - **Sicherheitsfragen:** Wählen Sie diese Option aus, um von Ihrem Administrator festgelegte vordefinierte Sicherheitsfragen einzurichten und zu beantworten. Diese Option funktioniert nur für die Kennwortzurücksetzung und nicht als Sicherheitsüberprüfungsmethode. Weitere Informationen zu Sicherheitsfragen finden Sie unter [Einrichten von Sicherheitsfragen als Überprüfungsmethode](security-info-setup-questions.md).

3. Wählen Sie nach dem Auswählen und Einrichten Ihrer Methoden die Option **Fertig stellen** aus, um den Vorgang abzuschließen.

    > [!Note]
    > Für Ihre Telefonnummer oder E-Mail-Adresse hinzugefügte Informationen werden nicht für das globale Verzeichnis Ihrer Organisation freigegeben. Diese Informationen sind nur für Sie und Ihren Administrator sichtbar. Die Antworten auf Ihre Sicherheitsfragen sind nur für Sie selbst sichtbar.

## <a name="common-problems-and-their-solutions"></a>Häufige Probleme und Lösungen

 Hier finden Sie einige häufige Fehler und die passenden Lösungen:

| Fehlermeldung |  Mögliche Lösung |
| --- | --- | --- |
| Wenden Sie sich an Ihren Administrator.<br>Es wurde ermittelt, dass das Kennwort Ihres Benutzerkontos nicht von Microsoft verwaltet wird. Ihr Kennwort kann daher nicht automatisch zurückgesetzt werden.<br>Weitere Unterstützung erhalten Sie vom IT-Team.| Wenn Sie diese Fehlermeldung nach der Eingabe Ihrer Benutzer-ID erhalten, bedeutet dies, dass Ihre Organisation Ihr Kennwort intern verwaltet und Sie Ihr Kennwort nicht über den Link **Sie können nicht auf Ihr Konto zugreifen?** zurücksetzen können. Wenn Ihr Kennwort in dieser Situation zurückgesetzt werden soll, müssen Sie den Helpdesk oder Administrator Ihrer Organisation um Hilfe bitten. |
| Ihr Konto ist nicht für die Kennwortzurücksetzung aktiviert.<br>Ihr IT-Team hat Ihr Konto nicht zur Verwendung mit diesem Dienst eingerichtet.<br>Wenn Sie möchten, können wir einen Administrator in Ihrer Organisation kontaktieren, um Ihr Kennwort für Sie zurückzusetzen. | Wenn Sie diese Fehlermeldung nach der Eingabe Ihrer Benutzer-ID erhalten, bedeutet dies, dass Ihre Organisation die Kennwortzurücksetzungsfunktion nicht aktiviert hat oder Sie die Funktion nicht verwenden dürfen. Wenn Ihr Kennwort in dieser Situation zurückgesetzt werden soll, müssen Sie den Link zur **Kontaktaufnahme mit einem Administrator** auswählen. Nachdem Sie auf den Link geklickt haben, wird eine E-Mail an den Helpdesk oder Administrator Ihrer Organisation gesendet, in der er darüber informiert wird, dass Sie Ihr Kennwort zurücksetzen möchten. |
| Das Konto konnte nicht überprüft werden.<br>Wenn Sie möchten, können wir einen Administrator in Ihrer Organisation kontaktieren, um Ihr Kennwort für Sie zurückzusetzen. | Wenn Sie diese Fehlermeldung nach der Eingabe Ihrer Benutzer-ID erhalten, bedeutet dies, dass Ihre Organisation die Kennwortzurücksetzung aktiviert hat und Sie die Funktion zwar verwenden können, sich aber nicht für den Dienst registriert haben. In diesem Fall müssen Sie sich an den Helpdesk oder Administrator Ihrer Organisation wenden, damit Ihr Kennwort zurückgesetzt werden kann. Informationen dazu, wie Sie sich für die Kennwortzurücksetzung registrieren, nachdem Sie Ihr Gerät wieder verwenden können, finden Sie im obigen Prozess in diesem Artikel. |

## <a name="next-steps"></a>Nächste Schritte

- [Ändern Ihres Kennworts mithilfe der Self-Service-Kennwortzurücksetzung](active-directory-passwords-update-your-own-password.md)

- [Seite für Sicherheitsinformationen](https://mysignins.microsoft.com/security-info)

- [Kennwortzurücksetzungsportal](https://passwordreset.microsoftonline.com/)

- Falls Sie sich nicht bei Ihrem Microsoft-Konto anmelden können, klicken Sie [hier](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).
