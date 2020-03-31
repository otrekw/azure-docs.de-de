---
title: Häufig gestellte Fragen zu Überwachungswarnungen und Berichten
description: In diesem Artikel finden Sie Antworten auf häufige Fragen zu den Berichten von Azure Backup-Überwachungswarnungen und Azure Backup.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 07/08/2019
ms.openlocfilehash: f5be97458ba658f315c31ae34e540842b64e3ec4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76989568"
---
# <a name="azure-backup-monitoring-alert---faq"></a>Azure Backup-Überwachungswarnungen – häufig gestellte Fragen

In diesem Artikel werden häufig gestellte Fragen zur Azure Backup-Überwachung und zur Berichterstellung beantwortet.

## <a name="configure-azure-backup-reports"></a>Konfigurieren von Azure Backup-Berichten

### <a name="how-do-i-check-if-reporting-data-has-started-flowing-into-a-log-analytics-la-workspace"></a>Wie überprüfe ich, ob bereits Berichtsdaten an einen Log Analytics-Arbeitsbereich (LA) übermittelt werden?

Navigieren Sie zum LA-Arbeitsbereich, den Sie konfiguriert haben, navigieren Sie zum Menüelement **Protokolle**, und führen Sie die Abfrage „CoreAzureBackup | take 1“ aus. Wenn ein Datensatz zurückgegeben wird, bedeutet dies, dass Daten an den Arbeitsbereich übertragen wurden. Der erste Datenpush kann bis zu 24 Stunden dauern.

### <a name="what-is-the-frequency-of-data-push-to-an-la-workspace"></a>Wie oft erfolgt ein Datenpush an einen LA-Arbeitsbereich?

Die Diagnosedaten aus dem Tresor werden mit einer gewissen Verzögerung in den Log Analytics-Arbeitsbereich übertragen. Jedes Ereignis erreicht den Log Analytics-Arbeitsbereich 20 bis 30 Minuten nach dem Push aus dem Recovery Services-Tresor. Hier finden Sie weitere Details zu dieser Verzögerung:

* Lösungsübergreifend werden die im Sicherungsdienst integrierten Warnungen sofort nach ihrer Erstellung gepusht. Daher werden sie normalerweise nach 20 bis 30 Minuten im Log Analytics-Arbeitsbereich angezeigt.
* Lösungsübergreifend werden bedarfsgesteuerte Sicherungs- und Wiederherstellungsaufträge gepusht, sobald sie abgeschlossen sind.
* Die geplanten Sicherungsaufträge aus allen Lösungen (außer SQL-Sicherung) werden gepusht, sobald sie abgeschlossen sind.
* Da Protokollsicherungen alle 15 Minuten auftreten können, werden für SQL-Sicherungen Informationen für alle abgeschlossenen geplanten Sicherungsaufträge (einschließlich Protokolle) als Batch gesammelt und alle 6 Stunden gepusht.
* Alle anderen Informationen wie Sicherungselemente, Richtlinien, Wiederherstellungspunkte, Speicher usw. werden lösungsübergreifend mindestens einmal täglich gepusht.
* Eine Änderung der Sicherungskonfiguration (wie einer Änderung der Richtlinie oder Bearbeitungsrichtlinie) löst einen Push aller zugehörigen Sicherungsinformationen aus.

### <a name="how-long-can-i-retain-reporting-data"></a>Wie lange kann ich Berichtsdaten aufbewahren?

Nachdem Sie einen LA-Arbeitsbereich erstellt haben, können Sie die Daten für maximal 2 Jahre aufbewahren. Standardmäßig werden die Daten in einem LA-Arbeitsbereich 31 Tage aufbewahrt.

### <a name="will-i-see-all-my-data-in-reports-after-i-configure-the-la-workspace"></a>Werden nach dem Konfigurieren des LA-Arbeitsbereichs alle meine Daten in Berichten angezeigt?

 Alle Daten, die nach der Konfiguration der Diagnoseeinstellungen generiert werden, werden an den LA-Arbeitsbereich gepusht und stehen in den Berichten zur Verfügung. Aufträge, die sich in Bearbeitung befinden, werden nicht mithilfe von Push für die Berichterstellung übertragen. Wenn ein Auftrag abgeschlossen wird oder zu einem Fehler führt, wird er an die Berichte gesendet.

### <a name="can-i-view-reports-across-vaults-and-subscriptions"></a>Kann ich Berichte für verschiedene Tresore und Abonnements anzeigen?

Ja, Sie können Berichte über Tresore, Abonnements und Regionen hinweg anzeigen. Ihre Daten können sich in einem einzelnen LA-Arbeitsbereich oder in einer Gruppe von LA-Arbeitsbereichen befinden.

### <a name="can-i-view-reports-across-tenants"></a>Kann ich Berichte mandantenübergreifend anzeigen?

Wenn Sie ein [Azure Lighthouse](https://azure.microsoft.com/services/azure-lighthouse/)-Benutzer mit delegiertem Zugriff auf die Abonnements oder LA-Arbeitsbereiche Ihrer Kunden sind, können Sie in den Berichten von Backup Daten zu allen Ihren Mandanten anzeigen.

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>Wie lange dauert es, bis der Auftragsstatus des Azure Backup-Agents im Portal angezeigt wird?

Es kann bis zu 15 Minuten dauern, bis der Status des Azure Backup-Agent-Auftrags im Azure-Portal angezeigt wird.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Wie lange dauert es, bis eine Warnung ausgelöst wird, wenn ein Sicherungsauftrag nicht erfolgreich ist?

Innerhalb von 20 Minuten nach dem Azure-Sicherungsfehler wird eine Warnung ausgelöst.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>Kann es vorkommen, dass eine E-Mail nicht gesendet wird, wenn Benachrichtigungen konfiguriert sind?

Ja. In den folgenden Situationen werden keine Benachrichtigungen gesendet.

* Stündliche Benachrichtigungen wurden konfiguriert, und eine Warnung wird ausgelöst und innerhalb dieser Stunde gelöst.
* Ein Auftrag wurde abgebrochen.
* Bei einem zweiten Sicherungsauftrag tritt ein Fehler auf, weil der ursprüngliche Sicherungsauftrag noch ausgeführt wird.

## <a name="recovery-services-vault"></a>Recovery Services-Tresor

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>Wie lange dauert es, bis der Auftragsstatus des Azure Backup-Agents im Portal angezeigt wird?

Es kann bis zu 15 Minuten dauern, bis der Status des Azure Backup-Agent-Auftrags im Azure-Portal angezeigt wird.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Wie lange dauert es, bis eine Warnung ausgelöst wird, wenn ein Sicherungsauftrag nicht erfolgreich ist?

Innerhalb von 20 Minuten nach dem Azure-Sicherungsfehler wird eine Warnung ausgelöst.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>Kann es vorkommen, dass eine E-Mail nicht gesendet wird, wenn Benachrichtigungen konfiguriert sind?

Ja. In den folgenden Situationen werden keine Benachrichtigungen gesendet:

* Stündliche Benachrichtigungen wurden konfiguriert, und eine Warnung wird ausgelöst und innerhalb dieser Stunde gelöst.
* Ein Auftrag wurde abgebrochen.
* Bei einem zweiten Sicherungsauftrag tritt ein Fehler auf, weil der ursprüngliche Sicherungsauftrag noch ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die anderen häufig gestellten Fragen:

* [Gängige Fragen](backup-azure-vm-backup-faq.md) zu Azure VM-Sicherungen
* [Gängige Fragen](backup-azure-file-folder-backup-faq.md) zum Azure Backup-Agent
