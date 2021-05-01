---
title: Anzeigen und Suchen Ihrer letzten Anmeldeaktivität auf der Seite „Meine Anmeldungen“ – Azure Active Directory | Microsoft-Dokumentation
description: Details zur Anzeige und Suche Ihrer letzten Anmeldeaktivität auf der Seite „Meine Anmeldungen“ im Portal „Mein Konto“.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: rhicock
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 04/22/2021
ms.author: curtand
ms.openlocfilehash: 593ec6588e2f55cb9f379dcfac437956bcabc10b
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107951322"
---
# <a name="view-and-search-your-recent-sign-in-activity-from-the-my-sign-ins-page"></a>Anzeigen und Suchen Ihrer letzten Anmeldeaktivität auf der Seite „Meine Anmeldungen“

Sie können die letzten Anmeldeaktivitäten für Ihr Geschäfts-, Schul- oder Unikonto auf der Seite **Meine Anmeldungen** im Portal **Mein Konto** einsehen. Durch die Überprüfung Ihres Anmeldeverlaufs können Sie feststellen, ob ungewöhnliche Aktivitäten stattgefunden haben. Dabei wird Ihnen Folgendes angezeigt:

- Ob jemand versucht, Ihr Kennwort zu erraten.
- Ob sich ein Angreifer erfolgreich bei ihrem Konto angemeldet hat und an welchem Standort die Anmeldung erfolgt ist.
- Auf welche Apps der Angreifer versucht hat, zuzugreifen.

>[!Note]
> Wenn bei der Anmeldung mit einem persönlichen Microsoft-Konto ein Fehler auftritt, können Sie sich trotzdem mit dem Domänennamen für Ihre Organisation (z. B. „contoso.com“) oder der **Mandanten-ID** Ihrer Organisation, die Sie von Ihrem Administrator erhalten, über eine der folgenden URLs anmelden:
>
>   - `https://myaccount.microsoft.com?tenantId=<your_domain_name>`
>   - `https://myaccount.microsoft.com?tenant=<your_tenant_ID>`

## <a name="view-your-recent-sign-in-activity"></a>Anzeigen Ihrer letzten Anmeldeaktivität

1. Melden Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto an, und rufen Sie die Seite https://myaccount.microsoft.com/ auf.

1. Wählen Sie im linken Navigationsbereich **Meine Anmeldungen** aus, oder wählen Sie im Block **Meine Anmeldungen** den Link **Letzte Aktivität überprüfen** aus.

    ![Seite „Mein Konto“ mit hervorgehobenen Links für die letzte Aktivität](media/my-account-portal/my-account-portal-sign-ins.png)

1. Erweitern Sie alle Anmeldeelemente, und überprüfen Sie diese. Stellen Sie sicher, dass Sie alle erkennen. Wenn Sie ein Anmeldeelement finden, das Ihnen unbekannt ist, ändern Sie Ihr Kennwort, um das Konto vor einer Sicherheitsgefährdung zu schützen.

    ![Seite „Letzte Aktivität“ mit erweiterten Anmeldedetails](media/my-account-portal-sign-ins-page/recent-activity.png)

### <a name="if-you-see-a-successful-sign-in"></a>Wenn eine erfolgreiche Anmeldung angezeigt wird

Manchmal kann bei der Überprüfung der eigenen normalen Anmeldeaktivitäten eine erfolgreiche Anmeldung von einem unbekannten Standort, Browser oder Betriebssystem angezeigt werden. Unbekannte Anmeldungen können bedeuten, dass ein Angreifer Zugriff auf Ihr Konto erlangt hat. Wenn Sie Aktivitäten sehen, die nicht von Ihnen autorisiert wurden, wird empfohlen, Ihr Kennwort sofort zu ändern und dann zu [Sicherheitsinformation](https://mysignins.microsoft.com/security-info) zu wechseln und Ihre Sicherheitseinstellungen zu aktualisieren.

Bevor Sie festlegen, dass etwas nicht richtig ist, stellen Sie sicher, dass Sie kein falsch-positives Ergebnis sehen (wobei das Element fragwürdig aussieht, aber in Ordnung ist). So wird beispielsweise Ihr ungefährer Standort und die Zuordnung basierend auf Ihrer IP-Adresse ermittelt. Mobilfunknetze sind besonders schwer zu lokalisieren, da sie den Datenverkehr gelegentlich über entfernte Standorte leiten. Auch wenn Sie sich mit Ihrem mobilen Gerät im Bundesstaat Washington angemeldet haben, kann als Standort für die Anmeldung Kalifornien angegeben sein. Es wird dringend empfohlen, weitere Details als nur den Standort zu überprüfen. Vergewissern Sie sich, dass auch Betriebssystem, Browser und App für Sie Sinn ergeben.

### <a name="if-you-see-an-unsuccessful-sign-in"></a>Wenn eine nicht erfolgreiche Anmeldung angezeigt wird

Wenn eine nicht erfolgreiche Anmeldung angezeigt wird, kann das bedeuten, dass Sie Ihre Anmeldeinformationen falsch eingegeben haben. Es kann auch bedeuten, dass ein Angreifer versucht hat, Ihr Kennwort zu erraten. Als Reaktion auf dieses Risiko müssen Sie Ihr Kennwort nicht ändern, aber es wird empfohlen, sich für Azure AD Multi-Factor Authentication (MFA) zu registrieren. Mit Multi-Factor Authentication ist das Kennwort auch dann nicht ausreichend für den Zugriff auf Ihr Konto, wenn der Hacker es erraten hat.

![Kachel „Nicht erfolgreiche Anmeldung“](media/my-account-portal-sign-ins-page/unsuccessful.png)

Wenn eine nicht erfolgreiche Anmeldung angezeigt wird, bei der unter **Sitzungsaktivität** der Hinweis `Additional verification failed, invalid code` angegeben ist, bedeutet dies, dass Ihre primären Anmeldeinformationen für die Authentifizierung erfolgreich waren, Multi-Factor Authentication aber fehlgeschlagen ist. Das kann bedeuten, dass ein Angreifer Ihr Kennwort richtig erraten hat, aber Multi-Factor Authentication nicht durchlaufen konnte. Es wird empfohlen, Ihr Kennwort trotzdem zu ändern, da der Angreifer bereits darüber verfügen könnte, und zur Seite [Sicherheitsinformation](https://mysignins.microsoft.com/security-info) zu wechseln und Ihre Sicherheitseinstellungen zu aktualisieren.

## <a name="search-for-specific-sign-in-activity"></a>Suchen nach einer spezifischen Anmeldeaktivität

Sie können Ihre letzten Anmeldeaktivitäten anhand der verfügbaren Informationen suchen. Beispielsweise können Sie nach der aktuellen Anmeldeaktivität anhand von Betriebssystem, Standort, App usw. suchen.

1. Geben Sie auf der Seite **Letzte Aktivität überprüfen** die gesuchten Informationen in die Leiste **Suchen** ein. Geben Sie z. B. `Unsuccessful` ein, um nach allen von der App „Mein Konto“ gesammelten erfolglosen Anmeldeaktivitäten zu suchen.

2. Wählen Sie die Schaltfläche **Suche** aus, um die Suche zu starten.

    ![Seite „Letzte Aktivität“ mit hervorgehobener Suchleiste, Schaltfläche „Suche“ und den Ergebnissen](media/my-account-portal-sign-ins-page/sign-in-search.png)

### <a name="confirm-unusual-activity"></a>Bestätigen ungewöhnlicher Aktivitäten

Anmeldungen, die als ungewöhnliche Aktivitäten gekennzeichnet sind, können in der Kachel für diese Aktivität auf der Seite **Meine Anmeldungen** bestätigt werden.

![Kachel für ungewöhnliche Anmeldung zur Bestätigung, dass der Anmeldeversuch von Ihnen durchgeführt wurde](media/my-account-portal-sign-ins-page/this-wasnt-me.png)

## <a name="view-sign-in-activity-on-android"></a>Anmeldeaktivität auf Android anzeigen

Jedes Mal, wenn Sie Sicherheitsinformationen anzeigen oder ändern, müssen Sie sich bei Ihrem Arbeits- oder Schulkonto anmelden, um fortzufahren.

1. Öffnen Sie die Microsoft Authenticator-App und wählen Sie die Startseite für Ihr Arbeits- oder Schulkonto.

1. Wählen Sie **Letzte Kontoaktivität**.

    ![Startseite des Arbeits- oder Schulkontos, mit hervorgehobenem Link zu den letzten Anmeldeaktivitäten](media/my-account-portal-sign-ins-page/android-recent-activity.png)

1. Wenn die Anmeldung erfolgreich war, überprüfen Sie die einzelnen Anmeldungselemente und stellen Sie sicher, dass Sie jedes einzelne erkennen. Wenn Sie ein Anmeldeelement finden, das Ihnen unbekannt ist, ändern Sie Ihr Kennwort, um das Konto vor einer Sicherheitsgefährdung zu schützen.

    ![Seite der letzten Aktivitäten mit Anmeldedetails](media/my-account-portal-sign-ins-page/android-activity-page.png)

## <a name="change-your-password-on-android"></a>Ändern Sie Ihr Passwort auf Android

Ihr Administrator kann es Ihnen ermöglichen, Ihr Passwort über die Authenticator-App zurückzusetzen. Jedes Mal, wenn Sie sich anmelden, um Sicherheitsinformationen anzuzeigen oder zu ändern, müssen Sie sich bei Ihrem Arbeits- oder Schulkonto anmelden, um fortzufahren.

1. Öffnen Sie die Microsoft Authenticator-App und wählen Sie die Startseite für Ihr Arbeits- oder Schulkonto.

1. Wählen Sie **Kennwort ändern** aus.

    ![Startseite des Arbeits- oder Schulkontos, mit hervorgehobenem Passwort-Änderungslink](media/my-account-portal-sign-ins-page/android-change-password.png)

1. Wenn Ihre Anmeldung erfolgreich war, können Sie Ihre Passwortinformationen auf der Seite **Passwort ändern** aktualisieren.

    ![Seite mit den letzten Aktivitäten mit altem und neuem Passworteintrag](media/my-account-portal-sign-ins-page/android-password-page.png)

## <a name="update-security-info-on-android"></a>Sicherheitsinformationen auf Android aktualisieren

Jedes Mal, wenn Sie sich anmelden, um Sicherheitsinformationen anzuzeigen oder zu ändern, müssen Sie sich bei Ihrem Arbeits- oder Schulkonto anmelden, um fortzufahren.

1. Öffnen Sie die Microsoft Authenticator-App und wählen Sie die Startseite für Ihr Arbeits- oder Schulkonto.

1. Wählen Sie **Sicherheitsinfo aktualisieren**.

    ![Startseite des Arbeits- oder Schulkontos, mit hervorgehobenem Link Sicherheitsinfo aktualisieren](media/my-account-portal-sign-ins-page/android-update-security-info.png)

1. Wenn Ihre Anmeldung erfolgreich war, können Sie Ihre Sicherheitsdetails auf der **Sicherheitsinfoseite** aktualisieren.

    ![Seite der letzten Aktivitäten mit Sicherheitsinfo](media/my-account-portal-sign-ins-page/android-security-info-page.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre letzten Anmeldeinformationen angezeigt haben, können Sie:

- Anzeigen oder Verwalten der [Sicherheitsinformationen](./security-info-setup-signin.md).

- Anzeigen oder Verwalten Ihrer verbundenen [Geräte](my-account-portal-devices-page.md).

- Anzeigen oder Verwalten Ihrer [Organisationen](my-account-portal-organizations-page.md)

- Anzeigen von Informationen zur [Nutzung der datenschutzrelevanten Informationen](my-account-portal-privacy-page.md) in Ihrer Organisation.

- Ändern der [Portaleinstellungen für „Mein Konto“](my-account-portal-settings.md)