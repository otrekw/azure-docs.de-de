---
title: Hinzufügen eines Geschäfts-, Schul- oder Unikontos zur Microsoft Authenticator-App – Azure AD
description: Fügen Sie der Microsoft Authenticator-App Ihr Geschäfts-, Schul- oder Unikonto hinzu, um während der zweistufigen Überprüfung Ihre Identität zu bestätigen.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 11/15/2020
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: 04c9bc429d9663f7ac36b6ba8f40abf225eb71c6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97359109"
---
# <a name="add-your-work-or-school-account-to-the-microsoft-authenticator-app"></a>Hinzufügen Ihres Geschäfts-, Schul- oder Unikontos in der Microsoft Authenticator-App

Wenn Ihre Organisation eine zweistufige Überprüfung verwendet, können Sie Ihr Geschäfts-, Schul- oder Unikonto für die Verwendung der Microsoft Authenticator-App als eine der Überprüfungsmethoden einrichten.

>[!Important]
>Bevor Sie Ihr Konto hinzufügen können, müssen Sie die Microsoft Authenticator-App herunterladen und installieren. Wenn Sie dies noch nicht getan haben, führen Sie die im Artikel [Herunterladen und Installieren der App](user-help-auth-app-download-install.md) aufgeführten Schritte aus.

## <a name="add-your-work-or-school-account"></a>Hinzufügen Ihres Geschäfts-, Schul- oder Unikontos

Sie können Ihr Geschäfts-, Schul- oder Unikonto zur Microsoft Authenticator-App hinzufügen, indem Sie einen der folgenden Schritte ausführen:

- Anmelden mit den Anmeldeinformationen Ihres Geschäfts-, Schul- oder Unikontos
- Scannen eines QR-Codes

### <a name="sign-in-with-your-credentials"></a>Mit Ihren Anmeldeinformationen anmelden

>[!Note]
>Diese Funktion kann nur von Benutzern verwendet werden, deren Administratoren die Anmeldung per Telefon mithilfe der Authenticator-App aktiviert haben.

So fügen Sie ein Konto hinzu, indem Sie sich mit Ihren Anmeldeinformationen bei Ihrem Geschäfts-, Schul- oder Unikonto anmelden:

1. Öffnen Sie die Microsoft Authenticator-App, wählen Sie die Schaltfläche **+** aus, und tippen Sie auf **Geschäfts-, Schul- oder Unikonto hinzufügen**. Wählen Sie **Anmelden**.

1. Geben Sie die Anmeldeinformationen Ihres Geschäfts-, Schul- oder Unikontos ein. Wenn Sie über einen befristeten Zugriffspass (Temporary Access Pass, TAP) verfügen, können Sie diesen für die Anmeldung verwenden. Zu diesem Zeitpunkt können Sie den Vorgang aufgrund einer der folgenden Bedingungen möglicherweise nicht fortsetzen:

   - Wenn Sie nicht über genügend Authentifizierungsmethoden für Ihr Konto verfügen, um ein sicheres Authentifizierungstoken zu erhalten, können Sie den Vorgang zum Hinzufügen eines Kontos nicht fortsetzen.

   - Wenn die Meldung `You might be signing in from a location that is restricted by your admin` angezeigt wird, sind Sie gesperrt, und ein Administrator muss Sie unter [Sicherheitsinformation](https://mysignins.microsoft.com/security-info) entsperren.

   - Wenn Sie nicht von Ihrem Administrator für die Anmeldung per Telefon mit der Authenticator-App gesperrt wurden, können Sie die Geräteregistrierung durchführen, um die kennwortlose Anmeldung per Telefon sowie Azure Multi-Factor Authentication (MFA) einzurichten.

1. Zu diesem Zeitpunkt werden Sie unter Umständen aufgefordert, einen von Ihrer Organisation bereitgestellten QR-Code zu scannen, um ein lokales Multi-Factor Authentication-Konto in der App einzurichten. Dies ist nur erforderlich, wenn Ihre Organisation einen lokalen MFA-Server verwendet.

1. Tippen Sie auf Ihrem Gerät auf das Konto, und überprüfen Sie in der Vollbildansicht, ob Ihre Kontoinformationen korrekt sind und ein zugehöriger sechsstelliger Prüfcode vorhanden ist. Um die Sicherheit zu erhöhen, wird der Prüfcode alle 30 Sekunden geändert, um zu verhindern, dass jemand einen Code mehrmals verwendet.

## <a name="sign-in-with-a-qr-code"></a>Anmelden mit einem QR-Code

Gehen Sie folgendermaßen vor, um ein Konto durch Scannen eines QR-Codes hinzuzufügen:

1. Navigieren Sie auf Ihrem Computer zur Seite **Zusätzliche Sicherheitsüberprüfung**.

   >[!Note]
   >Wenn die Seite **Zusätzliche Sicherheitsüberprüfung** nicht angezeigt wird, hat Ihr Administrator möglicherweise die Sicherheitsinformation (Vorschau) aktiviert. Befolgen Sie in diesem Fall die Anweisungen im Abschnitt [Einrichten der Sicherheitsinformation zur Verwendung einer Authentifikator-App](security-info-setup-auth-app.md). Ist dies nicht der Fall, bitten Sie den Helpdesk Ihrer Organisation um Unterstützung. Weitere Informationen zu Sicherheitsinformationen finden Sie unter [Einrichten Ihrer Sicherheitsinformation über eine Anmeldeaufforderung](security-info-setup-signin.md).

1. Aktivieren Sie das Kontrollkästchen neben der Authenticator-App, und wählen Sie dann **Konfigurieren** aus. Die Seite **Mobile App konfigurieren** wird angezeigt.

   ![Bildschirm, der einen QR-Code enthält](./media/user-help-auth-app-add-work-school-account/auth-app-barcode.png)

1. Öffnen Sie die Microsoft Authenticator-App, und wählen Sie das Pluszeichen ![Auswählen des Pluszeichens auf einem iOS- oder Android-Gerät](media/user-help-auth-app-add-work-school-account/plus-icon.png) und anschließend **Konto hinzufügen** aus. Wählen Sie dann **Geschäfts-, Schul- oder Unikonto** und anschließend **QR-Code scannen** aus.
   Wenn Sie kein Konto in der Authenticator-App eingerichtet haben, wird eine große blaue Schaltfläche mit der Aufschrift **Konto hinzufügen** angezeigt.

Wenn Sie nicht dazu aufgefordert werden, Ihre Kamera zum Scannen eines QR-Codes zu verwenden, stellen Sie in den Einstellungen Ihres Telefons sicher, dass die Authenticator-App Zugriff auf die Telefonkamera hat.

## <a name="next-steps"></a>Nächste Schritte

- Nachdem Ihre Konten der App hinzugefügt wurden, können Sie sich unter Verwendung der Authenticator-App auf Ihrem Gerät anmelden. Weitere Informationen finden Sie unter [Anmelden mithilfe der App](user-help-auth-app-sign-in.md).

- Für iOS-Geräte können Sie Ihre Kontoanmeldeinformationen und zugehörige App-Einstellungen, etwa die Reihenfolge Ihrer Konten, in der Cloud sichern. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von Kontoanmeldeinformationen mithilfe der Microsoft Authenticator-App](user-help-auth-app-backup-recovery.md).
