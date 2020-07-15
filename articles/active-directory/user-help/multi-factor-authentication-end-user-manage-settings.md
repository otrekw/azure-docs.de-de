---
title: Ändern der zweistufigen Überprüfungsmethode und der Einstellungen – Azure Active Directory
description: Auf der Seite „Zusätzliche Sicherheitsüberprüfung“ erfahren Sie, wie Sie die Sicherheitsüberprüfungsmethode und die Einstellungen für Ihr Geschäfts-, Schul- oder Unikonto ändern können.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: richagi
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 07/06/2020
ms.author: curtand
ms.openlocfilehash: 259468d0d3b04b354e782529cefa2149974c9ca0
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2020
ms.locfileid: "85983304"
---
# <a name="change-your-two-factor-verification-method-and-settings"></a>Ändern der zweistufigen Überprüfungsmethode und der Einstellungen

Nachdem Sie Ihre Methoden für die Sicherheitsüberprüfung für Ihre Geschäfts-, Schul- oder Unikonto eingerichtet haben, können Sie alle zugehörigen Details aktualisieren. Folgendes ist möglich:

- Standardmethode für die Sicherheitsüberprüfung

- Details der Sicherheitsüberprüfungsmethode, z. B. eine Telefonnummer

- Einrichten einer Authenticator-App oder Löschen eines Geräts aus der Authenticator-App

## <a name="using-the-additional-security-verification-page"></a>Verwenden der Seite „Zusätzliche Sicherheitsüberprüfung“

Wenn Ihre Organisation Ihnen bestimmte Schritte zum Aktivieren und Verwalten der zweistufigen Überprüfung zur Verfügung gestellt hat, sollten Sie zuerst diese Anweisungen befolgen. Andernfalls finden Sie die Einstellungen Ihrer Methode für die Sicherheitsüberprüfung auf der Seite [Zusätzliche Sicherheitsüberprüfung](https://docs.microsoft.com/azure/active-directory/user-help/multi-factor-authentication-end-user-first-time).

>[!Note]
>Wenn die auf Ihrem Bildschirm angezeigten Optionen nicht den in diesem Artikel behandelten Inhalten entsprechen, hat Ihr Administrator die Benutzeroberfläche **Sicherheitsinformation (Vorschau)** aktiviert, oder Ihre Organisation hat ein eigenes benutzerdefiniertes Portal bereitgestellt. Weitere Informationen zur neuen Benutzeroberfläche „Sicherheitsinformation“ finden Sie unter [Übersicht über die Sicherheitsinformationen (Vorschau)](user-help-security-info-overview.md). Um weitere Informationen zum benutzerdefinierten Portal Ihrer Organisation zu erhalten, wenden Sie sich an den Helpdesk Ihrer Organisation.

### <a name="to-get-to-the-additional-security-verification-page"></a>So gelangen Sie zur Seite „Zusätzliche Sicherheitsüberprüfung“

Sie können diesen Link [Seite „Zusätzliche Sicherheitsüberprüfung“](https://account.activedirectory.windowsazure.com/proofup.aspx?proofup=1) verwenden.

![Seite „Zusätzliche Sicherheitsüberprüfung“ mit Details zu den verfügbaren Methoden für die Sicherheitsüberprüfung](./media/multi-factor-authentication-end-user-manage-settings/mfa-security-verification-page.png)

Sie können auch die Seite **Zusätzliche Sicherheitsüberprüfung** aufrufen, indem Sie die folgenden Schritte ausführen:

1. Melden Sie sich bei [https://myapps.microsoft.com](https://myapps.microsoft.com) an.

1. Wählen Sie oben rechts Ihren Kontonamen aus, und wählen Sie dann **Profil**.

1. Klicken Sie auf **Zusätzliche Sicherheitsüberprüfung**.  

    ![Meine Apps-Link zur Seite „Zusätzliche Sicherheitsüberprüfung“](./media/multi-factor-authentication-end-user-manage-settings/mfa-myapps-link.png)

>[!Note]
>Informationen zum Verwenden des Abschnitts **App-Kennwörter** der Seite **Zusätzliche Sicherheitsüberprüfung** finden Sie unter [Verwalten von App-Kennwörtern für die zweistufige Überprüfung](multi-factor-authentication-end-user-app-passwords.md). App-Kennwörter sollten nur für Apps verwendet werden, die keine zweistufige Überprüfung unterstützen.

## <a name="change-your-default-security-verification-method"></a>Ändern Ihrer Standardmethode für die Sicherheitsüberprüfung

Nachdem Sie sich mit Ihrem Benutzernamen und Kennwort bei Ihrem Geschäfts-, Schul- oder Unikonto angemeldet haben, wird Ihnen Ihre ausgewählte Methode für die Sicherheitsüberprüfung automatisch angezeigt. Abhängig von den Anforderungen Ihrer Organisation kann es sich hierbei um einen Benachrichtigungs- oder Überprüfungscode für eine Authentifikator-App, eine SMS oder einen Telefonanruf handeln.

Wenn Sie die von Ihnen verwendete Standardmethode für die Sicherheitsüberprüfung ändern möchten, können Sie dies hier tun.

### <a name="to-change-your-default-security-verification-method"></a>So ändern Sie Ihre Standardmethode für die Sicherheitsüberprüfung

1. Wählen Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** in der Liste **Welche Option bevorzugen Sie?** die gewünschte Methode aus. Ihnen werden zwar alle Optionen angezeigt, Sie können aber nur diejenigen auswählen, die Ihnen von Ihrer Organisation zur Verfügung gestellt werden.

    - **Mich durch die App benachrichtigen**: Sie werden von der Authenticator-App darüber informiert, dass eine Aufforderung zur Überprüfung auf Sie wartet.

    - **Mein Authentifizierungstelefon anrufen**: Sie erhalten einen Anruf auf Ihrem Mobilgerät, in dem Sie aufgefordert werden, Ihre Informationen zu bestätigen.

    - **Textcode an mein Authentifizierungstelefon**: Sie erhalten auf Ihrem Mobilgerät eine SMS mit einem Überprüfungscode. Diesen Code müssen Sie bei der Überprüfungsaufforderung für Ihr Geschäfts-, Schul- oder Unikonto eingeben.

    - **Meine geschäftliche Rufnummer anrufen**: Sie erhalten einen Anruf auf Ihrem geschäftlichen Telefonanschluss, in dem Sie aufgefordert werden, Ihre Informationen zu bestätigen.

    - **Prüfcode aus der App verwenden**: Sie verwenden Ihre Authentifikator-App, um einen Überprüfungscode abzurufen, den Sie in die Überprüfungsaufforderung für Ihr Geschäfts-, Schul- oder Unikonto eingeben.

2. Wählen Sie **Speichern** aus.

## <a name="add-or-change-your-phone-number"></a>Hinzufügen oder Ändern Ihrer Telefonnummer

Auf der Seite **Zusätzliche Sicherheitsüberprüfung** können Sie neue Telefonnummern hinzufügen oder vorhandene Nummern ändern.

>[!Important]
>Es wird dringend empfohlen, eine zweite Telefonnummer hinzuzufügen, um zu verhindern, dass Sie aus Ihrem Konto ausgesperrt sind, wenn Ihr primäres Telefon verloren geht oder gestohlen wird oder wenn Sie ein neues Telefon verwenden und nicht mehr über Ihre ursprüngliche primäre Telefonnummer verfügen.

### <a name="to-change-your-phone-numbers"></a>So ändern Sie Ihre Telefonnummern

1. Aktualisieren Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** im Abschnitt **Wie möchten Sie antworten?** die Telefonnummer für Ihr **Authentifizierungstelefon** (Ihr primäres mobiles Gerät) und Ihr **Bürotelefon**.

1. Wählen Sie das Feld neben der Option **Alternative Telefonnummer für Authentifizierung** aus, und geben Sie eine sekundäre Telefonnummer ein, unter der Sie Anrufe empfangen können, falls Sie nicht auf Ihr primäres Telefon zugreifen können.

1. Wählen Sie **Speichern** aus.

## <a name="add-a-new-account-to-the-microsoft-authenticator-app"></a>Hinzufügen eines neuen Kontos zur Microsoft Authenticator-App

Sie können Ihr Geschäfts-, Schul- oder Unikonto in der Microsoft Authenticator-App für [Android](https://play.google.com/store/apps/details?id=com.azure.authenticator) oder [iOS](https://apps.apple.com/app/microsoft-authenticator/id983156458) einrichten.

Wenn Sie Ihr Geschäfts-, Schul- oder Unikonto bereits in der Microsoft Authenticator-App eingerichtet haben, müssen Sie dies nicht erneut tun.

1. Wählen Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** im Abschnitt **Wie möchten Sie antworten?** die Option **Authenticator-App einrichten** aus.

    ![Einrichten Ihres Geschäfts-, Schul- oder Unikontos in der Microsoft Authenticator-App](./media/multi-factor-authentication-end-user-manage-settings/mfa-security-verification-page-auth-app.png)

1. Befolgen Sie die Anweisungen auf dem Bildschirm (u. a. müssen Sie den QR-Code mit dem Mobilgerät scannen), und wählen Sie dann **Weiter** aus.

    Sie werden aufgefordert, eine Benachrichtigung über die Microsoft Authenticator-App zu genehmigen, um Ihre Informationen zu bestätigen.

1. Wählen Sie **Speichern** aus.

## <a name="delete-your-account-or-device-from-the-microsoft-authenticator-app"></a>Löschen Ihres Kontos oder Ihres Geräts aus der Microsoft Authenticator-App

Sie können Ihr Konto aus der Microsoft Authenticator-App löschen, und Sie können Ihr Gerät aus Ihrem Geschäfts-, Schul- oder Unikonto löschen. In der Regel löschen Sie ein Gerät, wenn Sie ein verlorenes, gestohlenes oder altes Gerät dauerhaft aus Ihrem Konto entfernen möchten. Das Löschen eines Kontos erfolgt meist, um Verbindungsprobleme zu beheben oder eine Kontoänderung umzusetzen, z. B. aufgrund eines neuen Benutzernamens.

### <a name="to-delete-your-device-from-your-work-or-school-account"></a>So löschen Sie Ihr Gerät aus Ihrem Geschäfts-, Schul- oder Unikonto

1. Wählen Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** im Abschnitt **Wie möchten Sie antworten?** die Schaltfläche **Authenticator-App einrichten** aus.

1. Wählen Sie **Speichern** aus.

### <a name="to-delete-your-account-from-the-microsoft-authenticator-app"></a>So löschen Sie Ihr Konto aus der Microsoft Authenticator-App

Wählen Sie in der Microsoft Authenticator-App die Schaltfläche **Löschen** neben dem Gerät aus, das Sie löschen möchten.

## <a name="turn-on-two-factor-verification-prompts-on-a-trusted-device"></a>Aktivieren von Aufforderungen der zweistufigen Überprüfung auf einem vertrauenswürdigen Gerät

Abhängig von den Einstellungen Ihrer Organisation steht möglicherweise ein Kontrollkästchen **Die nächsten X Tage nicht erneut fragen** zur Verfügung, wenn Sie die zweistufige Überprüfung in Ihrem Browser ausführen. Wenn Sie diese Option ausgewählt haben, um keine Aufforderungen für die zweistufige Überprüfung mehr zu erhalten, und Ihr Gerät dann verloren geht oder möglicherweise kompromittiert wurde, sollten Sie die Aufforderungen wieder aktivieren, um Ihr Konto besser zu schützen. Sie müssen die Aufforderungen für all Ihre Geräte gleichzeitig aktivieren. Leider können Sie die Aufforderungen nicht nur für ein bestimmtes Gerät aktivieren.

### <a name="to-turn-two-factor-verification-prompts-back-on-for-your-devices"></a>So aktivieren Sie die Aufforderungen für die zweistufige Überprüfung wieder für Ihre Geräte

Wählen Sie auf der Seite [**Zusätzliche Sicherheitsüberprüfung**](#to-get-to-the-additional-security-verification-page) die Option **Multi-Factor Authentication auf Geräten wiederherstellen, die zuvor als vertrauenswürdig eingestuft worden sind** aus. Bei der nächsten Anmeldung bei einem Gerät werden Sie aufgefordert, die zweistufige Überprüfung durchzuführen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Einstellungen für die zweistufige Überprüfung hinzugefügt oder aktualisiert haben, können Sie Ihre App-Kennwörter verwalten, sich anmelden oder Hilfe bei allgemeinen Problemen im Zusammenhang mit der zweistufigen Überprüfung erhalten.

- [Verwalten Sie App-Kennwörter für die zweistufige Überprüfung](multi-factor-authentication-end-user-app-passwords.md) für alle Apps, die die zweistufige Überprüfung nicht unterstützen.

- [Anmelden per zweistufiger Überprüfung](multi-factor-authentication-end-user-signin.md)

- [Beheben gängiger Probleme bei der zweistufigen Überprüfung](multi-factor-authentication-end-user-troubleshoot.md)
