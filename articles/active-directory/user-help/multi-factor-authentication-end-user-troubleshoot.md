---
title: Allgemeine Probleme mit der zweistufigen Kontoauthentifizierung – Azure AD
description: Lösungen für einige häufiger auftretende Probleme mit der zweistufigen Überprüfung in Verbindung mit Ihrem Geschäfts-, Schul- oder Unikonto.
services: active-directory
author: curtand
manager: daveba
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: troubleshooting
ms.date: 02/20/2020
ms.author: curtand
ms.reviewer: kexia
ms.openlocfilehash: 7dfd15f6270827382a6456231e8b1699986cd5f6
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78191366"
---
# <a name="common-problems-with-two-factor-verification-and-your-work-or-school-account"></a>Allgemeine Probleme mit der zweistufigen Überprüfung in Verbindung mit Ihrem Geschäfts-, Schul- oder Unikonto

Wenn Ihre Azure Active Directory (Azure AD)-Organisation die zweistufige Überprüfung aktiviert, ist für die Anmeldung bei Ihrem Geschäfts-, Schul- oder Unikonto eine Kombination aus Benutzernamen, Kennwort und einem mobilen Gerät oder Mobiltelefon erforderlich ist. Sie ist sicherer als nur ein Kennwort, da sie auf zwei Formen der Authentifizierung basiert: etwas, das Sie wissen, und etwas, das Sie bei sich tragen. Die zweistufige Überprüfung trägt dazu bei, Angriffe durch Hacker zu verhindern, die sich als Sie ausgeben. Denn auch wenn ein Hacker Ihr Kennwort weiß, ist die Wahrscheinlichkeit gering, dass er auch über Ihr Gerät verfügt.

<center>

![Bild konzeptioneller Authentifizierungsmethoden](../authentication/media/concept-mfa-howitworks/methods.png)</center>

Es gibt einige gängige Probleme bei der zweistufigen Überprüfung, die offenbar häufiger auftreten, als uns lieb ist. Wir haben diesen Artikel zusammengestellt, um einige mögliche Lösungen für die häufigsten Probleme zu bieten.

>[!Important]
>Administratoren finden in der [Azure AD-Dokumentation](https://docs.microsoft.com/azure/active-directory) weitere Informationen zum Einrichten und Verwalten der Azure AD-Umgebung.
>
>Dieser Inhalt ist außerdem nur für die Verwendung mit Ihrem Geschäfts-, Schul- oder Unikonto vorgesehen, also dem Konto, das Ihnen von Ihrer Organisation zur Verfügung gestellt wird (z. B. alain@contoso.com). Bei Problemen mit der zweistufigen Überprüfung in Verbindung mit Ihrem persönlichen Microsoft-Konto, also dem Konto, das Sie für sich selbst eingerichtet haben (z. B. danielle@outlook.com) lesen Sie den Artikel [Aktivieren oder Deaktivieren der Überprüfung in zwei Schritten für Ihr Microsoft-Konto](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off).

## <a name="i-dont-have-my-mobile-device-with-me"></a>Ich habe mein mobiles Gerät nicht bei mir

Das kommt vor. Sie haben Ihr mobiles Gerät zu Hause gelassen und können es jetzt nicht verwenden, um Ihre Identität zu bestätigen. Wenn Sie zuvor eine andere Methode zur Anmeldung bei Ihrem Konto hinzugefügt haben, z. B. Ihr Bürotelefon, sollten Sie diese Methode jetzt verwenden können. Wenn Sie keine zusätzliche Überprüfungsmethode hinzugefügt haben, müssen Sie sich an den Helpdesk Ihrer Organisation wenden und sich beim Zugriff auf Ihr Konto helfen lassen.

### <a name="to-sign-in-to-your-work-or-school-account-using-another-verification-method"></a>So melden Sie sich mit einer anderen Überprüfungsmethode bei Ihrem Geschäfts-, Schul- oder Unikonto an

1. Melden Sie sich bei Ihrem Konto an, wählen Sie aber auf der Seite **Zweistufige Überprüfung** den Link **Auf andere Weise anmelden** aus.

    ![Ändern der Überprüfungsmethode für die Anmeldung](./media/multi-factor-authentication-end-user-troubleshoot/two-factor-auth-signin-another-way.png)

    Wenn Sie den Link **Auf andere Weise anmelden** nicht sehen, bedeutet dies, dass Sie keine anderen Überprüfungsmethoden eingerichtet haben. Wenden Sie sich an Ihren Administrator, damit er Ihnen bei der Anmeldung bei Ihrem Konto hilft.

2. Wählen Sie Ihre alternative Überprüfungsmethode aus, und fahren Sie mit dem zweistufigen Überprüfungsprozess fort.

## <a name="i-lost-my-mobile-device-or-it-was-stolen"></a>Ich habe mein mobiles Gerät verloren, oder es wurde gestohlen

Wenn Sie Ihr mobiles Gerät verloren haben oder es gestohlen wurde, können Sie sich entweder mit einer anderen Methode anmelden oder den Helpdesk Ihrer Organisation bitten, Ihre Einstellungen zu löschen. Es wird dringend empfohlen, den Helpdesk Ihrer Organisation zu informieren, wenn Sie Ihr Smartphone verloren haben oder es gestohlen wurde, damit Ihr Konto entsprechend aktualisiert werden kann. Nachdem Ihre Einstellungen gelöscht wurden, werden Sie bei der nächsten Anmeldung aufgefordert, sich für die [zweistufige Überprüfung zu registrieren](multi-factor-authentication-end-user-first-time.md).

## <a name="im-not-getting-the-verification-code-sent-to-my-mobile-device"></a>Ich empfange nicht den Überprüfungscode, der an mein mobiles Gerät gesendet wurde

Das Nichtempfangen des Überprüfungscodes ist ein gängiges Problem, das typischerweise mit Ihrem Mobilgerät und seinen Einstellungen zusammenhängt. Sie können Folgendes versuchen:

Versuchen Sie Folgendes | Info und Anleitung
--------- | ------------
Starten Sie Ihr mobiles Gerät neu | Gelegentlich benötigt Ihr Gerät nur einen Neustart. Beim Neustart Ihres Geräts werden alle Hintergrundprozesse oder -dienste beendet, die derzeit ausgeführt werden und Probleme verursachen könnten. Außerdem werden die Kernkomponenten Ihres Geräts aktualisiert und neu gestartet, falls sie irgendwann einmal abgestürzt sein sollten.
Überprüfen Sie, ob Ihre Sicherheitsinformationen richtig sind | Stellen Sie sicher, dass die Informationen zu Ihrer Sicherheitsüberprüfungsmethode zutreffend sind, insbesondere Ihre Telefonnummern. Wenn Sie eine falsche Telefonnummer eingeben, landen alle Ihre Benachrichtigungen bei dieser falschen Nummer. Glücklicherweise kann dieser Benutzer mit den Benachrichtigungen nichts anfangen, aber es wird Ihnen auch nicht helfen, sich bei Ihrem Konto anzumelden. Um sicherzustellen, dass Ihre Informationen richtig sind, lesen Sie die Anweisungen im Artikel [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md).
Überprüfen Sie, ob Benachrichtigungen aktiviert sind | Vergewissern Sie sich, dass auf Ihrem Mobilgerät Benachrichtigungen aktiviert sind und dass Sie eine Benachrichtigungsmethode ausgewählt haben, die es ermöglicht, dass über Anrufe, Ihre Authentifizierungs-App und Ihre Messaging-App (für SMS) sichtbare Benachrichtigungen an Ihr Mobilgerät gesendet werden.
Stellen Sie sicher, dass Sie über ein Gerätesignal und eine Internetverbindung verfügen | Vergewissern Sie sich, dass Ihre Telefonanrufe und SMS-Nachrichten zu Ihrem mobilen Gerät gelangen. Bitten Sie einen Freund, Sie anzurufen und Ihnen eine SMS zu schicken, um sicherzustellen, dass beides möglich ist. Falls nicht, überprüfen Sie zunächst, ob Ihr Mobilgerät eingeschaltet ist. Wenn Ihr Gerät eingeschaltet ist, Sie aber immer noch keinen Anruf bzw. keine SMS erhalten, liegt höchstwahrscheinlich ein Problem mit Ihrem Mobilfunknetz vor, weshalb Sie sich an Ihren Mobilfunkanbieter wenden müssen. Wenn Sie häufig signalbedingte Probleme haben, empfiehlt sich die Installation und Verwendung der [Microsoft Authenticator-App](user-help-auth-app-download-install.md) auf Ihrem mobilen Gerät. Diese App kann zufällige Sicherheitscodes für die Anmeldung generieren, ohne ein Mobilfunksignal oder eine Internetverbindung vorauszusetzen.
Deaktivieren Sie „Nicht stören“ | Stellen Sie sicher, dass das Funktionsmerkmal **Nicht stören** für Ihr mobiles Gerät deaktiviert ist. Wenn dieses Funktionsmerkmal aktiviert ist, sind keine Benachrichtigungen auf Ihrem Mobilgerät möglich. In der Bedienungsanleitung Ihres Mobilgeräts finden Sie Anweisungen, wie Sie dieses Funktionsmerkmal deaktivieren können.
Aufheben der Sperre von Telefonnummern | In den USA stammen Anrufe von Microsoft von folgenden Telefonnummern: + 1 (866) 539 4191, + 1 (855) 330 8653 und + 1 (877) 668 6536.
Überprüfen Sie akkubezogene Einstellungen | Dies scheint auf den ersten Blick etwas seltsam zu sein, aber wenn Sie Ihre Akkuroptimierung so eingerichtet haben, dass weniger genutzte Anwendungen nicht im Hintergrund aktiv bleiben, ist Ihr Benachrichtigungssystem höchstwahrscheinlich betroffen. Um dieses Problem zu beheben, deaktivieren Sie die Akkuoptimierung für Ihre Authentifizierungs- und Messaging-App, und versuchen Sie dann, sich erneut bei Ihrem Konto anzumelden.

## <a name="im-not-getting-prompted-for-my-second-verification-information"></a>Ich werde nicht zur Eingabe meiner zweiten Überprüfungsinformation aufgefordert

Wenn Sie sich mit Ihrem Benutzernamen und Kennwort bei Ihrem Geschäfts-, Schul- oder Unikonto angemeldet haben, aber nicht zur Eingabe der zusätzlichen Informationen zur Sicherheitsüberprüfung aufgefordert wurden, haben Sie möglicherweise Ihr Gerät noch nicht eingerichtet. Ihr Mobilgerät muss entsprechend eingerichtet sein, damit es mit Ihrer speziellen zusätzlichen Sicherheitsüberprüfungsmethode funktioniert. Um sicherzustellen, dass Sie Ihr Mobilgerät eingeschaltet haben und es für die Verwendung mit Ihrer Überprüfungsmethode verfügbar ist, lesen Sie den Artikel [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md). Wenn Sie wissen, dass Sie Ihr Gerät oder Ihr Konto nicht eingerichtet haben, können Sie dies jetzt tun, indem Sie die Schritte im Artikel [Einrichten meines Kontos für die zweistufige Überprüfung](multi-factor-authentication-end-user-first-time.md) ausführen.

## <a name="i-got-a-new-phone-number-and-i-want-to-add-it"></a>Ich habe eine neue Telefonnummer und möchte diese hinzufügen

Wenn Sie eine neue Telefonnummer haben, müssen Sie die Details Ihrer Sicherheitsüberprüfungsmethode aktualisieren, damit Überprüfungsaufforderungen an die richtige Stelle geleitet werden. Um die Überprüfungsmethode zu aktualisieren, führen Sie die Schritte im Abschnitt **Hinzufügen oder Ändern Ihrer Telefonnummer** des Artikels [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md#add-or-change-your-phone-number) aus.

## <a name="i-got-a-new-mobile-device-and-i-want-to-add-it"></a>Ich habe ein neues mobiles Gerät und möchte dieses hinzufügen

Wenn Sie ein neues Mobilgerät haben, müssen Sie es so einrichten, dass es mit der zweistufigen Überprüfung funktioniert. Dies ist ein mehrstufiger Prozess:

1. Richten Sie Ihr Gerät für Ihr Geschäfts-, Schul- oder Unikonto ein, indem Sie die Schritte im Artikel [Einrichten meines Kontos für die zweistufige Überprüfung](multi-factor-authentication-end-user-first-time.md) ausführen.

1. Aktualisieren Sie Ihre Konto- und Geräteinformationen auf der Seite **Zusätzliche Sicherheitsüberprüfung**, indem Sie Ihr altes Gerät löschen und Ihr neues hinzufügen. Weitere Informationen finden Sie im Artikel [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md).

Optionale Schritte:

- Sie können die Microsoft Authenticator-App auf Ihr Mobilgerät herunterladen, dort installieren und einrichten, indem Sie die Schritte im Artikel [Herunterladen und Installieren der Microsoft Authenticator-App](user-help-auth-app-download-install.md) ausführen.

- Aktivieren Sie die zweistufige Überprüfung auf Ihren vertrauenswürdigen Geräten durch Ausführen der Schritte in **Erneutes Anfordern der zweistufigen Überprüfung auf einem als vertrauenswürdig markierten Gerät** des Artikels [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md#turn-on-two-factor-verification-prompts-on-a-trusted-device).

## <a name="im-having-problems-signing-in-on-my-mobile-device-while-traveling"></a>Ich habe auf Reisen Probleme bei der Anmeldung bei meinem mobilen Gerät

Möglicherweise ist es schwieriger, eine Überprüfungsmethode für mobile Geräte wie z. B. SMS zu nutzen, wenn Sie im Ausland sind. Es ist auch möglich, dass Ihr mobiles Gerät Roaminggebühren verursacht. Für diese Situation empfehlen wir Ihnen, die Microsoft Authenticator-App zu verwenden, die die Möglichkeit bietet, sich mit einem WLAN-Hotspot zu verbinden. Weitere Informationen zum Herunterladen, Installieren und Einrichten der Microsoft Authenticator-App auf Ihrem Mobilgerät finden Sie im Artikel [Herunterladen und Installieren der Microsoft Authenticator-App](user-help-auth-app-download-install.md).

## <a name="i-cant-get-my-app-passwords-to-work"></a>Meine App-Kennwörter funktionieren nicht

App-Kennwörter ersetzen normale Kennwörter für ältere Desktopanwendungen, die die zweistufige Überprüfung nicht unterstützen. Stellen Sie zunächst sicher, dass Sie das Kennwort korrekt eingegeben haben. Wenn das Problem dadurch nicht behoben wird, versuchen Sie, ein neues Kennwort für die App zu erstellen, indem Sie die Schritte im Abschnitt **Erstellen und Löschen von App-Kennwörtern im Portal „Meine Apps“** des Artikels [Verwalten von App-Kennwörtern für die zweistufige Überprüfung](multi-factor-authentication-end-user-app-passwords.md#create-and-delete-app-passwords-from-the-additional-security-verification-page) ausführen.

## <a name="i-cant-turn-two-factor-verification-off"></a>Ich kann die zweistufige Überprüfung nicht deaktivieren

Wenn Sie die zweistufige Überprüfung mit Ihrem Geschäfts-, Schul- oder Unikonto (z. B. alain@contoso.com) verwenden, bedeutet dies höchstwahrscheinlich, dass Ihre Organisation entschieden hat, dass Sie dieses zusätzliche Sicherheitsfeature verwenden müssen. Da Ihre Organisation die Verwendung des Features vorgeschrieben hat, kann es nicht individuell deaktiviert werden. Wenn Sie jedoch die zweistufige Überprüfung mit einem persönlichen Konto wie alain@outlook.comverwenden, können Sie das Feature aktivieren und deaktivieren. Anweisungen zum Steuern der zweistufigen Überprüfung für Ihre persönlichen Konten finden Sie unter [Aktivieren oder Deaktivieren der Überprüfung in zwei Schritten für Ihr Microsoft-Konto](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off).

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Ich konnte keine Lösung für mein Problem finden

Wenn Sie diese Schritte ausgeführt haben, aber weiterhin Probleme auftreten, wenden Sie sich zwecks Unterstützung an den Helpdesk Ihrer Organisation.

## <a name="related-articles"></a>Verwandte Artikel

- [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md)

- [Einrichten meines Kontos für die zweistufige Überprüfung](multi-factor-authentication-end-user-first-time.md)

- [Microsoft Authenticator-App – häufig gestellte Fragen](user-help-auth-app-faq.md)
