---
title: Worum handelt es sich bei der Seite „Zusätzliche Sicherheitsüberprüfung“? – Azure AD
description: Informationen zum Aufrufen der Seite „Zusätzliche Sicherheitsüberprüfung“ für die zweistufige Überprüfung
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 05/28/2020
ms.author: curtand
ms.openlocfilehash: e5b07f8f7ae766d110c87a495a3e1623b815e526
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88795982"
---
# <a name="what-is-the-additional-verification-page"></a>Worum handelt es sich bei der Seite „Zusätzliche Sicherheitsüberprüfung“?

Ihre Organisation ergreift zusätzliche Maßnahmen, um Ihre Identität bei der Anmeldung sicherzustellen. Diese zusätzliche Sicherheitsüberprüfung wird auch als zweistufige Überprüfung bezeichnet. Sie besteht aus einer Kombination aus Benutzername, Kennwort und einem mobilen Gerät oder Telefon. Wenn Sie lediglich die zweistufige Überprüfung für ein Microsoft-Konto wie alain@outlook.com deaktivieren möchten, verwenden Sie die Anweisungen unter [Aktivieren oder Deaktivieren der Überprüfung in zwei Schritten für Ihr Microsoft-Konto](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off).

<center>

![Bild konzeptioneller Authentifizierungsmethoden](../authentication/media/concept-mfa-howitworks/methods.png)</center>

Die zweistufige Überprüfung ist sicherer als ein einfaches Kennwort, da sie auf zwei Formen der Authentifizierung beruht:

- Etwas, das Sie kennen, wie Ihr Kennwort.
- Etwas, das Sie besitzen, wie ein Telefon oder ein anderes Gerät, das Sie bei sich tragen.

Die zweistufige Überprüfung kann dazu beitragen, Hacker davon abzuhalten, sich für Sie auszugeben. Selbst wenn sie Ihr Kennwort kennen, stehen die Chancen gut, dass sie nicht auch über Ihr Gerät verfügen.

>[!Important]
>Wenn Sie Administrator sind und Informationen zum Aktivieren der zweistufigen Überprüfung für Ihre Mitarbeiter oder andere Benutzer benötigen, sehen Sie sich die [Dokumentation zur Azure Active Directory-Authentifizierung](../authentication/index.yml) an. Dieser Artikel richtet sich an Benutzer, die die zweistufige Überprüfung mit einem Geschäfts-, Schul- oder Unikonto (z. B. alain@contoso.com) verwenden möchten.

## <a name="who-decides-if-you-use-this-feature"></a>Wer entscheidet über die Verwendung dieser Funktion?

Wer entscheidet, ob Sie die zweistufige Überprüfung verwenden, hängt davon ab, welche Art von Konto Sie verwenden:

- **Geschäfts-, Schul- oder Unikonto.** Wenn Sie ein Geschäfts-, Schul- oder Uni-Konto verwenden (z. B. alain@contoso.com), entscheidet Ihre Organisation, ob Sie die zweistufige Überprüfung verwenden. Sie gibt zudem vor, welche Überprüfungsmethode anzuwenden ist. Da Ihre Organisation die Verwendung des Features vorgeschrieben hat, kann es nicht individuell deaktiviert werden.

- **Persönliches Microsoft-Konto.** Sie können die zweistufige Überprüfung auch für Ihre persönlichen Microsoft-Konten einrichten (z. B. alain@outlook.com). Sie können es jederzeit aktivieren oder deaktivieren, indem Sie die einfachen Anweisungen in [Aktivieren oder Deaktivieren der Überprüfung in zwei Schritten für Ihr Microsoft-Konto](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off) verwenden.

    >[!Note]
    >Wenn andere Probleme bei der zweistufigen Überprüfung und Ihrem persönlichen Microsoft-Konto auftreten, gibt es weitere Vorschläge unter [Verwenden der zweistufigen Überprüfung für Ihr Microsoft-Konto](https://support.microsoft.com/help/12408/microsoft-account-how-to-use-two-step-verification).

## <a name="open-the-additional-security-verification-page"></a>Öffnen der Seite „Zusätzliche Sicherheitsüberprüfung“

Nachdem Ihre Organisation die zweistufige Überprüfung aktiviert hat, werden Sie bei jeder Anmeldung aufgefordert, weitere Informationen einzugeben, um Ihr Konto zu schützen.

![Aufforderung zur Eingabe weiterer Informationen](media/multi-factor-authentication-verification-methods/multi-factor-authentication-initial-prompt.png)

### <a name="to-access-the-additional-security-verification-page"></a>So greifen Sie auf die Seite „Zusätzliche Sicherheitsüberprüfung“ zu

1. Wählen Sie in der Aufforderung **Weitere Informationen erforderlich** die Option **Weiter** aus.

    Die Seite **Zusätzliche Sicherheitsüberprüfung** wird angezeigt.

2. Wählen Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** die zweistufige Überprüfungsmethode aus, mit der überprüft werden soll, ob Sie die Person sind, für den Sie sich ausgeben, wenn Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto anmelden. Sie können Folgendes auswählen:

    | Kontaktmethode | Beschreibung |
    | --- | --- |
    | Mobile App | <ul><li>**Benachrichtigungen zur Überprüfung empfangen**: Mit dieser Option wird eine Benachrichtigung an die Authenticator-App auf Ihrem Smartphone oder Tablet gesendet. Überprüfen Sie die Benachrichtigung, und wählen Sie in der App **Authentifizieren** aus, wenn Sie den Zugriff zulassen möchten. Ihr Unternehmen oder Ihre Bildungseinrichtung erfordert möglicherweise die Eingabe einer PIN, bevor Sie sich authentifizieren.</li><li>**Prüfcode verwenden**: In diesem Modus generiert die App einen Überprüfungscode, der alle 30 Sekunden aktualisiert wird. Geben Sie auf dem Anmeldebildschirm den aktuellen Prüfcode ein.<br>Die Microsoft Authenticator-App ist für [Android](https://go.microsoft.com/fwlink/?linkid=866594) und [iOS](https://go.microsoft.com/fwlink/?linkid=866594) verfügbar.</li></ul> |
    | Authentifizierungstelefon | <ul><li>**Telefonanruf**: Das Telefon mit der von Ihnen angegebenen Telefonnummer empfängt einen automatisierten Anruf. Nehmen Sie den Anruf an, und drücken Sie die Rautetaste (#) auf der Telefontastatur, um sich zu authentifizieren.</li><li>**SMS**: Es wird eine SMS gesendet, die einen Prüfcode enthält. Je nachdem, wozu Sie in der SMS aufgefordert werden, antworten Sie auf die SMS, oder geben Sie im Anmeldungsfenster den angegebenen Überprüfungscode ein.</li></ul> |
    | Bürotelefon | Das Telefon mit der von Ihnen angegebenen Telefonnummer empfängt einen automatisierten Anruf. Nehmen Sie den Anruf an, und drücken Sie die Rautetaste (#) auf der Telefontastatur, um sich zu authentifizieren. |

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** eine zweistufige Überprüfungsmethode ausgewählt haben, müssen Sie diese einrichten:

- [Einrichten Ihres mobilen Geräts als Überprüfungsmethode](multi-factor-authentication-setup-phone-number.md)

- [Einrichten Ihrer Bürotelefonnummer als Überprüfungsmethode](multi-factor-authentication-setup-office-phone.md)

- [Einrichten der Microsoft Authenticator-App als Überprüfungsmethode](multi-factor-authentication-setup-auth-app.md)

## <a name="related-resources"></a>Zugehörige Ressourcen

- [Anmelden per zweistufiger Überprüfung](multi-factor-authentication-end-user-signin.md)

- [Hilfe bei der zweistufigen Überprüfung](multi-factor-authentication-end-user-troubleshoot.md)