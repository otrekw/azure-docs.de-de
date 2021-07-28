---
title: Beheben von Problemen bei der Anmeldung für ein Azure-Abonnement
description: Hilft bei der Behebung von Problemen, die das Anmelden beim Azure-Portal verhindern.
services: cost-management-billing
author: v-miegge
manager: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: troubleshooting
ms.date: 05/13/2021
ms.author: v-miegge
ms.openlocfilehash: 009a29a4e296c8aa03163dd954cd242eec040027
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110470744"
---
# <a name="troubleshoot-azure-subscription-sign-in-issues"></a>Beheben von Problemen bei der Anmeldung für ein Azure-Abonnement

Dieser Leitfaden hilft bei der Behebung von Problemen, die das Anmelden beim Azure-Portal verhindern.

> [!NOTE]
> Wenn Sie Probleme bei der Registrierung für ein neues Azure-Konto haben, helfen Ihnen die Informationen unter [Beheben von Problemen beim Registrieren eines neuen Kontos im Azure-Portal oder im Azure-Kontocenter](./troubleshoot-azure-sign-up.md) weiter.

## <a name="page-hangs-in-the-loading-status"></a>Seite hängt im Ladestatus

Wenn Ihr Internetbrowser hängt, können Sie nacheinander die folgenden Schritte ausprobieren, bis Sie auf das Azure-Portal zugreifen können.

- Aktualisieren Sie die Seite.
- Verwenden Sie einen anderen Internetbrowser.
- Verwenden Sie den privaten Modus in Ihrem Browser:

   - **Microsoft Edge:** Öffnen Sie **Einstellungen** (drei Punkte neben Ihrem Profilbild), und wählen Sie die Option **Neues InPrivate-Fenster** aus. Navigieren Sie dann zum [Azure-Portal](https://portal.azure.com/), und melden Sie sich an. 
   - **Chrome:** Wählen Sie den Modus **Inkognito** aus.
   - **Safari:** Wählen Sie **Datei** und dann **Neues privates Fenster** aus.

- Löschen Sie den Cache und die Internetcookies:

   - **Microsoft Edge:** Öffnen Sie **Einstellungen**, und wählen Sie **Datenschutz und Dienste** aus. Führen Sie die Schritte unter **Browserdaten löschen** aus. Vergewissern Sie sich, dass die Kontrollkästchen für **Browserverlauf**, **Downloadverlauf** und **Zwischengespeicherte Bilder und Dateien** aktiviert sind, und wählen Sie dann die Option **Löschen** aus.
   - **Chrome:** Wählen Sie **Einstellungen** und unter **Datenschutz und Sicherheit** dann **Browserdaten löschen** aus.

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>Sie werden automatisch als ein anderer Benutzer angemeldet

Dieses Problem kann auftreten, wenn Sie mehrere Benutzerkonten in einem Internetbrowser verwenden.

Um dieses Problem zu lösen, probieren Sie eine der folgenden Methoden aus:

- Löschen Sie Cache und Internetcookies.

   - **Microsoft Edge:** Öffnen Sie **Einstellungen**, und wählen Sie **Datenschutz und Dienste** aus. Führen Sie die Schritte unter **Browserdaten löschen** aus. Vergewissern Sie sich, dass die Kontrollkästchen für **Browserverlauf**, **Downloadverlauf**, **Cookies** und **Zwischengespeicherte Bilder und Dateien** aktiviert sind, und wählen Sie dann die Option **Löschen** aus.
   - **Chrome:** Wählen Sie **Einstellungen** und unter **Datenschutz und Sicherheit** dann **Browserdaten löschen** aus.
- Setzen Sie Ihre Browsereinstellungen auf die Standardwerte zurück.
- Verwenden Sie den privaten Modus in Ihrem Browser. 
   - **Microsoft Edge:** Öffnen Sie **Einstellungen** (drei Punkte neben Ihrem Profilbild), und wählen Sie die Option **Neues InPrivate-Fenster** aus. Navigieren Sie dann zum [Azure-Portal](https://portal.azure.com/), und melden Sie sich an. 
   - **Chrome:** Wählen Sie den Modus **Inkognito** aus.
   - **Safari:** Wählen Sie **Datei** und dann **Neues privates Fenster** aus.

## <a name="i-can-sign-in-but-i-see-the-error-no-subscriptions-found"></a>Ich kann mich zwar anmelden, aber der Fehler „Keine Abonnements gefunden“ wird angezeigt

Dieses Problem tritt auf, wenn Sie das falsche Verzeichnis ausgewählt haben oder Ihr Konto nicht über ausreichende Berechtigungen verfügt.

**Szenario:** Sie erhalten die Fehlermeldung bei der Anmeldung beim [Azure-Portal](https://portal.azure.com/).

So beheben Sie dieses Problem:

- Vergewissern Sie sich, dass das richtige Azure-Verzeichnis ausgewählt ist, indem Sie oben rechts Ihr Konto auswählen.
- Wenn das richtige Azure-Verzeichnis ausgewählt ist und die Fehlermeldung trotzdem noch ausgegeben wird, [sollten Sie Ihr Konto als Besitzer hinzufügen lassen](./add-change-subscription-administrator.md).

## <a name="additional-help-resources"></a>Zusätzliche Hilferessourcen

Weitere Artikel für die Problembehandlung bei Azure-Abrechnung und -Abonnements

- [Karte abgelehnt](./troubleshoot-declined-card.md)
- [Probleme bei der Registrierung für ein Abonnement](./troubleshoot-azure-sign-up.md)
- [Keine Abonnements gefunden](./no-subscriptions-found.md)
- [Enterprise-Kostenansicht deaktiviert](./enterprise-mgmt-grp-troubleshoot-cost-view.md)
- [Dokumentation zur Azure-Abrechnung](../index.yml)

## <a name="contact-us-for-help"></a>Setzen Sie sich mit uns in Verbindung, um Hilfe zu erhalten.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).