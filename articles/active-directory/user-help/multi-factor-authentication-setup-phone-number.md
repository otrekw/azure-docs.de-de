---
title: 'Einrichten eines mobilen Geräts als Methode für die zweistufige Überprüfung: Azure Active Directory | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie ein mobiles Gerät als Methode für die zweistufige Überprüfung einrichten.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 08/12/2019
ms.author: curtand
ms.openlocfilehash: 76809a41400502ec48c2dd674f8976bb168ec9f9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "83735989"
---
# <a name="set-up-a-mobile-device-as-your-two-factor-verification-method"></a>Einrichten eines mobilen Geräts als Methode für die zweistufige Überprüfung

Sie können Ihr mobiles Gerät als Methode für die zweistufige Überprüfung einrichten. Ihr Mobiltelefon kann entweder eine SMS mit einem Prüfcode oder einen Telefonanruf empfangen.

>[!Note]
> Ist die Option für das Authentifizierungstelefon abgeblendet, lässt Ihre Organisation unter Umständen die Nutzung einer Telefonnummer oder SMS für die Überprüfung nicht zu. In diesem Fall müssen Sie eine andere Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.

## <a name="set-up-your-mobile-device-to-use-a-text-message-as-your-verification-method"></a>Einrichten Ihres mobilen Geräts zur Verwendung einer SMS als Überprüfungsmethode

1. Wählen Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** die Option **Telefon für Authentifizierung** im Bereich **Schritt 1: Wie dürfen wir Sie kontaktieren?** die Option Telefon für Authentifizierung aus. Wählen Sie anschließend in der Dropdownliste Ihr Land oder Ihre Region aus, und geben Sie die Telefonnummer Ihres mobilen Geräts ein.

2. Wählen Sie im Bereich **Methode** die Option **Code per SMS an mich senden** und anschließend **Weiter** aus.

    ![Seite „Zusätzliche Sicherheitsüberprüfung“ mit Authentifizierungstelefon und SMS](media/multi-factor-authentication-verification-methods/multi-factor-authentication-text-message.png)

3. Geben Sie den Prüfcode aus der SMS von Microsoft im Bereich **Schritt 2: Wir haben eine SMS an folgende Nummer geschickt** ein, und wählen Sie dann **Bestätigen** aus.

    ![Seite „Zusätzliche Sicherheitsüberprüfung“ mit Authentifizierungstelefon und SMS](media/multi-factor-authentication-verification-methods/multi-factor-authentication-text-message-test.png)

4. Kopieren Sie im Bereich **Schritt 3: Vorhandene Anwendungen weiterhin verwenden** das angegebene App-Kennwort, und speichern Sie es an einem sicheren Ort.

    ![Bereich für App-Kennwörter der Seite „Zusätzliche Sicherheitsüberprüfung“](media/multi-factor-authentication-verification-methods/multi-factor-authentication-app-passwords.png)

    >[!Note]
    >Informationen zur Verwendung des App-Kennworts mit älteren Apps finden Sie unter [Verwalten von App-Kennwörtern für die zweistufige Überprüfung](multi-factor-authentication-end-user-app-passwords.md). App-Kennwörter sind nur erforderlich, wenn Sie weiterhin ältere Apps verwenden, die die zweistufige Überprüfung nicht unterstützen.

5. Wählen Sie **Fertig**aus.

## <a name="set-up-your-mobile-device-to-receive-a-phone-call"></a>Einrichten Ihres mobilen Geräts zum Empfangen eines Telefonanrufs

1. Wählen Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** die Option **Telefon für Authentifizierung** im Bereich **Schritt 1: Wie dürfen wir Sie kontaktieren?** die Option Telefon für Authentifizierung aus. Wählen Sie anschließend in der Dropdownliste Ihr Land oder Ihre Region aus, und geben Sie die Telefonnummer Ihres mobilen Geräts ein.

2. Wählen Sie im Bereich **Methode** die Option **Anrufen** und anschließend **Weiter** aus.

    ![Seite „Zusätzliche Sicherheitsüberprüfung“ mit Authentifizierungstelefon und Telefonanruf](media/multi-factor-authentication-verification-methods/multi-factor-authentication-phone-call.png)

3. Sie erhalten einen Telefonanruf von Microsoft, in dem Sie aufgefordert werden, die Rautetaste auf Ihrem mobilen Gerät zu drücken, um Ihre Identität zu bestätigen.

    ![Testen der angegebenen Telefonnummer](media/multi-factor-authentication-verification-methods/multi-factor-authentication-phone-call-test.png)

4. Kopieren Sie im Bereich **Schritt 3: Vorhandene Anwendungen weiterhin verwenden** das angegebene App-Kennwort, und speichern Sie es an einem sicheren Ort.

    ![Bereich für App-Kennwörter der Seite „Zusätzliche Sicherheitsüberprüfung“](media/multi-factor-authentication-verification-methods/multi-factor-authentication-app-passwords.png)

    >[!Note]
    >Informationen zur Verwendung des App-Kennworts mit älteren Apps finden Sie unter [Verwalten von App-Kennwörtern für die zweistufige Überprüfung](multi-factor-authentication-end-user-app-passwords.md). App-Kennwörter sind nur erforderlich, wenn Sie weiterhin ältere Apps verwenden, die die zweistufige Überprüfung nicht unterstützen.

5. Wählen Sie **Fertig**aus.

## <a name="next-steps"></a>Nächste Schritte

Nach der Einrichtung der Methode für die zweistufige Überprüfung können Sie weitere Methoden hinzufügen, Ihre Einstellungen und App-Kennwörter verwalten, sich anmelden oder Hilfe bei allgemeinen Problemen im Zusammenhang mit der zweistufigen Überprüfung anfordern.

- [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md)

- [Verwalten von App-Kennwörtern](multi-factor-authentication-end-user-app-passwords.md)

- [Anmelden per zweistufiger Überprüfung](multi-factor-authentication-end-user-signin.md)

- [Hilfe bei der zweistufigen Überprüfung](multi-factor-authentication-end-user-troubleshoot.md)
