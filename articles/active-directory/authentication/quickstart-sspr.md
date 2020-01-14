---
title: 'Schnellstart: Self-Service-Kennwortzurücksetzung in Azure AD'
description: In dieser Schnellstartanleitung lernen Sie, wie Sie eine Konfiguration der Self-Service-Kennwortzurücksetzung in Azure AD durchführen, damit Benutzer ihre eigenen Kennwörter zurücksetzen können und so der Arbeitsaufwand der IT-Abteilung reduziert wird.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: quickstart
ms.date: 12/10/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: a168f9bf58c4942fc0b76b9ffefc2b32b5bfbe5a
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75549362"
---
# <a name="quickstart-configure-azure-active-directory-self-service-password-reset"></a>Schnellstart: Konfigurieren der Self-Service-Kennwortzurücksetzung in Azure Active Directory

In dieser Schnellstartanleitung konfigurieren Sie die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) in Azure Active Directory (AD), damit Benutzer ihre Kennwörter zurücksetzen oder ihre Konten entsperren können. Mit SSPR können Benutzer ihre eigenen Anmeldeinformationen ohne Unterstützung durch Helpdesk oder Administrator zurücksetzen. So können Benutzer wieder auf Ihr Konto zuzugreifen, ohne auf zusätzlichen Support warten zu müssen.

> [!IMPORTANT]
> In dieser Schnellstartanleitung erfahren Sie, wie ein Administrator die Self-Service-Kennwortzurücksetzung aktivieren kann. Wenn Sie bereits als Endbenutzer für die Self-Service-Kennwortzurücksetzung registriert sind und wieder zu Ihrem Konto zurückkehren müssen, gehen Sie zu https://aka.ms/sspr.
>
> Wenn Ihr IT-Team die Möglichkeit zum Zurücksetzen Ihres eigenen Kennworts nicht aktiviert hat, wenden Sie sich an den Helpdesk, um weitere Unterstützung zu erhalten.

## <a name="prerequisites"></a>Voraussetzungen

* Ein funktionierender Azure AD-Mandant mit mindestens einer aktivierten Testlizenz.
    * Erstellen Sie ggf. [ein kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Ein Konto mit Berechtigungen vom Typ „Globaler Administrator“.
* Ein Testbenutzer ohne Administratorrechte mit einem Ihnen bekannten Kennwort, wie z. B. *testuser*.
    * Wenn Sie einen Benutzer erstellen müssen, finden Sie weitere Informationen unter [Schnellstart: Hinzufügen neuer Benutzer in Azure Active Directory](../add-users-azure-active-directory.md) weiter.
* Eine Pilotgruppe, mit der getestet werden kann, ob der Testbenutzer ohne Administratorrechte Mitglied ist, z. B. *SSPR-Test-Group*.
    * Wenn Sie eine Gruppe erstellen müssen, finden Sie weitere Informationen unter [Erstellen einer Gruppe und Hinzufügen von Mitgliedern in Azure Active Directory](../active-directory-groups-create-azure-portal.md).

## <a name="enable-self-service-password-reset"></a>Aktivieren der Self-Service-Kennwortzurücksetzung

[Diesen Prozess als Video auf YouTube anzeigen](https://youtu.be/Pa0eyqjEjvQ)

1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Azure Active Directory** und dann **Kennwortzurücksetzung** aus.

1. Wählen Sie auf der Seite **Eigenschaften** unter **Self-Service-Kennwortzurücksetzung aktiviert** die Option **Aktiviert** aus.
1. Wählen Sie **Gruppe auswählen** und dann Ihre Pilotgruppe aus, die Sie im Abschnitt „Voraussetzungen“ dieses Artikels erstellt haben, z. B. *SSPR-Test-Group*. Wählen Sie **Speichern** aus, wenn Sie so weit sind.
1. Wählen Sie auf der Seite **Authentifizierungsmethoden** Folgendes und dann **Speichern** aus:
    * Anzahl von erforderlichen Methoden zum Zurücksetzen: **1**
    * Für Benutzer verfügbare Methoden:
        * **Code der mobilen App**
        * **E-Mail**

    > [!div class="mx-imgBorder"]
    > ![Auswählen der Authentifizierungsmethoden für SSPR][Authentication]

4. Wählen Sie auf der Seite **Registrierung** Folgendes und dann **Speichern** aus:
   * Erzwingen der Registrierung für Benutzer bei der Anmeldung: **Ja**
   * Festlegen der Anzahl von Tagen, bevor Benutzer aufgefordert werden, ihre Authentifizierungsinformationen erneut zu bestätigen: **365**

## <a name="test-self-service-password-reset"></a>Testen der Self-Service-Kennwortzurücksetzung

Nun können Sie Ihre SSPR-Konfiguration mit einem Testbenutzer testen, der Teil der Gruppe ist, die Sie im vorherigen Abschnitt ausgewählt haben, z. B. *testuser*. Da Microsoft für Azure-Administratorkonten strenge Anforderungen an die Authentifizierung stellt, kann das Testen mit einem Administratorkonto zu einem anderen Ergebnis führen. Weitere Informationen zur Richtlinie für Administratorkennwörter finden Sie im Artikel zu [Kennwortrichtlinien](concept-sspr-policy.md).

1. Öffnen Sie ein neues Browserfenster im InPrivate- oder Inkognitomodus, und navigieren Sie zu [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup).
2. Melden Sie sich als Testbenutzer ohne Administratorrechte an, z. B. *testuser*, und registrieren Sie Ihre Telefonnummer für die Authentifizierung.
3. Wählen Sie abschließend die Schaltfläche **Sieht gut aus** aus, und schließen Sie das Browserfenster.
4. Öffnen Sie ein neues Browserfenster im InPrivate- oder Inkognitomodus, und navigieren Sie zu [https://aka.ms/sspr](https://aka.ms/sspr).
5. Geben Sie die Benutzer-ID des Testbenutzers ohne Administratorrechte, z. B. *testuser*, und die CAPTCHA-Zeichen ein, und wählen Sie anschließend **Weiter** aus.
6. Führen Sie die Schritte für die Verifizierung aus, um Ihr Kennwort zurückzusetzen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Um die Self-Service-Kennwortzurücksetzung zu deaktivieren, suchen Sie im Azure-Portal nach **Azure Active Directory**, und wählen Sie die Option aus. Wählen Sie **Eigenschaften** > **Kennwortzurücksetzung** und dann unter **Self-Service-Kennwortzurücksetzung aktiviert** die Option **Keine** aus. Wählen Sie **Speichern** aus, wenn Sie so weit sind.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie die Self-Service-Kennwortzurücksetzung für Ihre reinen Cloudbenutzer konfigurieren können. Fahren Sie mit dem Leitfaden für den Rollout fort, um sich darüber zu informieren, wie Sie einen ausführlicheren Rollout durchführen.

> [!div class="nextstepaction"]
> [Erfolgreicher Rollout der Self-Service-Kennwortzurücksetzung](howto-sspr-deployment.md)

[Authentication]: ./media/quickstart-sspr/sspr-authentication-methods.png "Verfügbare Azure AD-Authentifizierungsmethoden und erforderliche Menge"

<!-- INTERNAL LINKS -->
[register-sspr]: ../user-help/active-directory-passwords-reset-register.md
[reset-password]: ../user-help/active-directory-passwords-update-your-own-password.md
