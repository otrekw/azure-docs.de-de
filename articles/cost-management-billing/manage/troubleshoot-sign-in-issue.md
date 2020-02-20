---
title: Beheben von Problemen bei der Anmeldung für ein Azure-Abonnement
description: Hilft bei der Behebung von Problemen, aufgrund derer Sie sich nicht beim Azure-Portal oder Azure-Kontocenter anmelden können.
author: v-miegge
ms.reviewer: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.topic: troubleshooting
ms.date: 02/12/2020
ms.author: v-miegge
ms.openlocfilehash: 40a645fe4b74043d84dc770f15d5e3c3ec907f02
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77199482"
---
# <a name="troubleshoot-azure-subscription-sign-in-issues"></a>Beheben von Problemen bei der Anmeldung für ein Azure-Abonnement

Dieser Leitfaden hilft bei der Behebung von Problemen, aufgrund derer Sie sich nicht beim Azure-Portal oder Azure-Kontocenter anmelden können.

## <a name="issues"></a>Probleme

### <a name="page-hangs-in-the-loading-status"></a>Seite hängt im Ladestatus

Wenn Ihr Internetbrowser hängt, können Sie nacheinander die folgenden Schritte ausprobieren, bis Sie auf das Azure-Portal zugreifen können.

- Aktualisieren Sie die Seite.
- Verwenden Sie einen anderen Internetbrowser.
- Verwenden Sie den privaten Modus in Ihrem Browser. Für Internet Explorer: Klicken Sie auf **Extras** > **Sicherheit** > **InPrivate-Browsen**, und navigieren Sie dann zum [Azure-Portal](https://portal.azure.com/) oder [Azure-Kontocenter](https://account.azure.com/Subscriptions).

### <a name="you-are-automatically-signed-in-as-a-different-user"></a>Sie werden automatisch als ein anderer Benutzer angemeldet

Dieses Problem kann auftreten, wenn Sie mehrere Benutzerkonten in einem Internetbrowser verwenden.

Um dieses Problem zu lösen, probieren Sie eine der folgenden Methoden aus:

- Löschen Sie Cache und Internetcookies. Klicken Sie in Internet Explorer auf **Extras** > **Internetoptionen** > **Löschen**. Stellen Sie sicher, dass die Kontrollkästchen für temporäre Dateien, Cookies, Kennwort und Verlauf aktiviert sind, und klicken Sie dann auf „Löschen“.
- Setzen Sie die Internet Explorer-Einstellungen zurück, um alle persönlichen Einstellungen wiederherstellen, die Sie vorgenommen haben. Klicken Sie hierzu auf **Extras** > **Internetoptionen** > **Erweitert**. Wählen Sie das Feld **Persönliche Einstellungen löschen** und dann **Zurücksetzen** aus.
- Verwenden Sie den privaten Modus in Ihrem Browser. Für Internet Explorer:  Klicken Sie auf **Extras** > **Sicherheit** > **InPrivate-Browsen**, und navigieren Sie dann zum [Azure-Portal](https://portal.azure.com/) oder [Azure-Kontocenter](https://account.azure.com/Subscriptions).

### <a name="i-can-sign-in-but-i-see-no-subscriptions-found"></a>Ich kann mich zwar anmelden, aber es wird *Keine Abonnements gefunden* angezeigt

Dieses Problem tritt auf, wenn Sie das falsche Verzeichnis ausgewählt haben oder Ihr Konto nicht über ausreichende Berechtigungen verfügt.

**Szenario 1:** Die Fehlermeldung wird im [Azure-Portal](https://portal.azure.com/) angezeigt.

So beheben Sie dieses Problem:

- Vergewissern Sie sich, dass das richtige Azure-Verzeichnis ausgewählt ist, indem Sie oben rechts auf Ihr Konto klicken.
- Wenn das richtige Azure-Verzeichnis ausgewählt ist, die Fehlermeldung jedoch immer noch ausgegeben wird, [lassen Sie Ihr Konto als Besitzer hinzufügen](add-change-subscription-administrator.md).

**Szenario 2:** Die Fehlermeldung wird im [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions) angezeigt.

Überprüfen Sie, ob das Konto, das Sie verwendet haben, das des Kontoadministrators ist. Gehen Sie folgendermaßen vor, um zu überprüfen, wer der Kontoadministrator ist:

1. Melden Sie sich bei der [Ansicht „Abonnements“ im Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) an.

2. Wählen Sie das zu überprüfende Abonnement aus, und sehen Sie unter **Einstellungen** nach.

3. Wählen Sie **Eigenschaften** aus. Der Kontoadministrator des Abonnements wird im Feld **Kontoadministrator** angezeigt.

## <a name="additional-help-resources"></a>Zusätzliche Hilferessourcen

Weitere Artikel für die Problembehandlung bei Azure-Abrechnung und -Abonnements

- [Karte abgelehnt](troubleshoot-declined-card.md)
- [Probleme bei der Registrierung für ein Abonnement](troubleshoot-azure-sign-up.md)
- [Keine Abonnements gefunden](no-subscriptions-found.md)
- [Enterprise-Kostenansicht deaktiviert](enterprise-mgmt-grp-troubleshoot-cost-view.md)

## <a name="contact-us-for-help"></a>Setzen Sie sich mit uns in Verbindung, um Hilfe zu erhalten.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Nächste Schritte

- [Dokumentation zur Azure-Abrechnung](../../billing/index.md)
