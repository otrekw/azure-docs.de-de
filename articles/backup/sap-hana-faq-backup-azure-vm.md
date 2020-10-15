---
title: 'Häufig gestellte Fragen: Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern'
description: In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zum Sichern von SAP HANA-Datenbanken mit dem Azure Backup-Dienst.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: dcbf1bf6b39b2afa3fb5aaf2a7f18c5d0e8e4afb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86513505"
---
# <a name="frequently-asked-questions--back-up-sap-hana-databases-on-azure-vms"></a>Häufig gestellte Fragen: Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern

In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zum Sichern von SAP HANA-Datenbanken mit dem Azure Backup-Dienst.

## <a name="backup"></a>Backup

### <a name="how-many-full-backups-are-supported-per-day"></a>Wie viele vollständige Sicherungen werden pro Tag unterstützt?

Wir unterstützen nur eine vollständige Sicherung pro Tag. Es ist nicht möglich, am selben Tag eine differenzielle Sicherung und eine vollständige Sicherung auszulösen.

### <a name="do-successful-backup-jobs-create-alerts"></a>Erstellen erfolgreiche Sicherungsaufträge Warnungen?

Nein. Erfolgreiche Sicherungsaufträge generieren keine Warnungen. Warnungen werden nur für Sicherungsaufträge gesendet, bei denen ein Fehler aufgetreten ist. Ausführliche Informationen zum Verhalten von Portalwarnungen finden Sie [hier](./backup-azure-monitoring-built-in-monitor.md). Falls Sie aber daran interessiert sind, Benachrichtigungen auch für erfolgreiche Aufträge zu erhalten, können Sie [Azure Monitor](./backup-azure-monitoring-use-azuremonitor.md) verwenden.

### <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>Werden geplante Sicherungsaufträge im Menü „Sicherungsaufträge“ angezeigt?

Im Menü „Sicherungsaufträge“ werden nur Ad-hoc-Sicherungsaufträge angezeigt. Verwenden Sie für geplante Aufträge [Azure Monitor](./backup-azure-monitoring-use-azuremonitor.md).

### <a name="are-future-databases-automatically-added-for-backup"></a>Werden zukünftige Datenbanken für die Durchführung von Sicherungen automatisch hinzugefügt?

Nein. Dies wird derzeit nicht unterstützt.

### <a name="if-i-delete-a-database-from-an-instance-what-will-happen-to-the-backups"></a>Was passiert mit den Sicherungen, wenn ich eine Datenbank aus einer Instanz lösche?

Wenn eine Datenbank aus einer SAP HANA-Instanz gelöscht wird, wird weiterhin versucht, Sicherungen der Datenbank durchzuführen. Dies bedeutet auch, dass die gelöschte Datenbank unter **Sicherungselemente** als fehlerhaft angezeigt wird und weiterhin geschützt ist.
Die richtige Vorgehensweise zum Beenden des Schutzes dieser Datenbank besteht darin, hierfür die Option zum **Beenden der Sicherung bei gelöschten Daten** zu verwenden.

### <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-the-behavior-be"></a>Welches Verhalten ergibt sich, wenn ich den Namen der Datenbank ändere, nachdem sie geschützt wurde?

Eine umbenannte Datenbank wird wie eine neue Datenbank behandelt. Diese Situation wird daher vom Dienst so behandelt, als ob die Datenbank nicht gefunden wurde, und für die Sicherungen tritt ein Fehler auf. Die umbenannte Datenbank wird als neue Datenbank angezeigt und muss für Schutz konfiguriert werden.

### <a name="what-are-the-prerequisites-to-back-up-sap-hana-databases-on-an-azure-vm"></a>Was sind die Voraussetzungen für die Sicherung von SAP HANA-Datenbanken auf einer Azure-VM?

Informationen finden Sie in den Abschnitten zu den [Voraussetzungen](tutorial-backup-sap-hana-db.md#prerequisites) und den [Aufgaben des Vorregistrierungsskripts](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does).

### <a name="what-permissions-should-be-set-so-azure-can-back-up-sap-hana-databases"></a>Welche Berechtigungen sollten festgelegt werden, damit SAP HANA-Datenbanken von Azure gesichert werden können?

Wenn Sie das Skript vor der Registrierung ausführen, werden die erforderlichen Berechtigungen festgelegt, damit Azure SAP HANA-Datenbanken sichern kann. Weitere Informationen zu den Aufgaben des Vorregistrierungsskripts finden Sie [hier](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does).

### <a name="will-backups-work-after-migrating-sap-hana-from-sdc-to-mdc"></a>Funktionieren Sicherungen nach der Migration von SAP HANA von SDC auf MDC?

Weitere Informationen finden Sie in [diesem Abschnitt](./backup-azure-sap-hana-database-troubleshoot.md#sdc-to-mdc-upgrade-with-a-change-in-sid) des Leitfadens zur Problembehandlung.

### <a name="can-azure-hana-backup-be-set-up-against-a-virtual-ip-load-balancer-and-not-a-virtual-machine"></a>Kann die HANA-Sicherung in Azure für eine virtuelle IP-Adresse (Lastenausgleich) und nicht einen virtuellen Computer eingerichtet werden?

Derzeit besteht keine Möglichkeit, die Lösung nur für eine virtuelle IP-Adresse einzurichten. Es wird ein virtueller Computer zum Ausführen der Lösung benötigt.

### <a name="how-can-i-move-an-on-demand-backup-to-the-local-file-system-instead-of-the-azure-vault"></a>Wie kann ich eine bedarfsgesteuerte Sicherung in das lokale Dateisystem anstatt in den Azure-Tresor verschieben?

1. Warten Sie, bis die aktuell ausgeführte Sicherung für die gewünschte Datenbank abgeschlossen ist (überprüfen Sie in Studio, ob der Vorgang abgeschlossen ist).
1. Deaktivieren Sie Protokollsicherungen, und legen Sie die Katalogsicherung mithilfe der folgenden Schritte auf **Dateisystem** für die gewünschte Datenbank fest:
1. Doppelklicken Sie auf **SYSTEMDB** -> **Konfiguration** -> **Datenbank auswählen** -> **Filter (Protokoll)** .
    1. Legen Sie „enable_auto_log_backup“ auf **No** fest.
    1. Legen Sie „catalog_backup_using_backint“ auf **False** fest.
1. Erstellen Sie eine bedarfsgesteuerte Sicherung (vollständig/differenziell/inkrementell) für die gewünschte Datenbank, und warten Sie, bis die Vorgänge für die Sicherung und die Katalogsicherung abgeschlossen sind.
1. Legen Sie „enable_auto_log_backup“ auf **Yes** fest, wenn auch die Protokollsicherungen in das Dateisystem verschoben werden sollen.
1. Kehren Sie zu den vorherigen Einstellungen zurück, damit Sicherungen in den Azure-Tresor übertragen werden können:
    1. Legen Sie „enable_auto_log_backup“ auf **Yes** fest.
    1. Legen Sie „catalog_backup_using_backint“ auf **True** fest.

>[!NOTE]
>Beim Verschieben von Sicherungen in das lokale Dateisystem und Zurückwechseln zum Azure-Tresor kann es zu einer Unterbrechung der Protokollkette im Tresor kommen. Hierdurch wird eine vollständige Sicherung ausgelöst, nach deren erfolgreichem Abschluss mit dem Sichern der Protokolle begonnen wird.

### <a name="how-can-i-use-sap-hana-backup-with-my-hana-replication-set-up"></a>Wie kann ich die SAP HANA-Sicherung mit meiner Einrichtung der HANA-Replikation nutzen?

Derzeit ist Azure Backup nicht in der Lage, eine HSR-Einrichtung zu verstehen. Dies bedeutet, dass der primäre und sekundäre Knoten der HSR als zwei einzelne VMs behandelt werden, die nicht in Verbindung stehen. Zuerst müssen Sie die Sicherung auf dem primären Knoten konfigurieren. Wenn ein Failover ausgeführt wird, muss die Sicherung auf dem sekundären Knoten konfiguriert werden (der nun zum primären Knoten wird). Es erfolgt kein automatisches Failover der Sicherung auf den anderen Knoten.

Zum Sichern von Daten vom aktiven (primären) Knoten zu einem beliebigen Zeitpunkt können Sie einen **Wechsel des Schutzes** auf den sekundären Knoten durchführen, der nach dem Failover zum primären Knoten geworden ist.

Führen Sie diese Schritte aus, um diesen **Wechsel des Schutzes** durchzuführen:

- [Beenden des Schutzes](sap-hana-db-manage.md#stop-protection-for-an-sap-hana-database) (mit Beibehaltung der Daten) auf dem primären Knoten
- Ausführen des [Vorregistrierungsskripts](https://aka.ms/scriptforpermsonhana) auf dem sekundären Knoten
- [Ermitteln der Datenbanken](tutorial-backup-sap-hana-db.md#discover-the-databases) auf dem sekundären Knoten und [Konfigurieren der entsprechenden Sicherungen](tutorial-backup-sap-hana-db.md#configure-backup)

Diese Schritte müssen nach jedem Failover manuell ausgeführt werden. Sie können diese Schritte nicht nur im Azure-Portal ausführen, sondern auch per Befehlszeile/HTTP REST. Für die Automatisierung dieser Schritte können Sie ein Azure-Runbook verwenden.

Hier ist ein ausführliches Beispiel für den **Wechsel des Schutzes** angegeben:

In diesem Beispiel verfügen Sie über zwei Knoten: Knoten 1 (primär) und Knoten 2 (sekundär) in der HSR-Einrichtung.  Sicherungen werden auf Knoten 1 konfiguriert. Wie bereits erwähnt, sollten Sie noch nicht versuchen, Sicherungen auf Knoten 2 zu konfigurieren.

Wenn das erste Failover ausgeführt wird, wird Knoten 2 zum primären Knoten. Dies ergibt folgende Szenarien:

1. Beenden Sie den Schutz auf Knoten 1 (vorheriger primärer Knoten) mit der Option zur Beibehaltung der Daten.
1. Führen Sie das Vorregistrierungsskript auf Knoten 2 (nun der primäre Knoten) aus.
1. Ermitteln Sie die Datenbanken auf Knoten 2, weisen Sie eine Sicherungsrichtlinie zu, und konfigurieren Sie die Sicherungen.

Anschließend wird auf Knoten 2 eine erste vollständige Sicherung ausgelöst, und nach deren Abschluss beginnen die Protokollsicherungen.

Beim nächsten Failover wird Knoten 1 wieder zum primären Knoten und Knoten 2 zum sekundären Knoten. Wiederholen Sie nun den Vorgang:

1. Beenden Sie den Schutz von Knoten 2 mit der Option zur Beibehaltung der Daten.
1. Führen Sie das Vorregistrierungsskript auf Knoten 1 (der wieder zum primären Knoten geworden ist) aus.
1. [Setzen Sie dann die Sicherungen auf Knoten 1 fort](sap-hana-db-manage.md#resume-protection-for-an-sap-hana-database), indem Sie die erforderliche Richtlinie nutzen (da die Sicherungsvorgänge zuvor für Knoten 1 beendet wurden).

Anschließend wird auf Knoten 1 erneut eine vollständige Sicherung ausgelöst, und nach deren Abschluss beginnen die Protokollsicherungen.

## <a name="restore"></a>Restore

### <a name="why-cant-i-see-the-hana-system-i-want-my-database-to-be-restored-to"></a>Warum kann ich das HANA-System nicht sehen, in dem meine Datenbank wieder hergestellt werden soll?

Überprüfen Sie, ob alle Voraussetzungen für die Wiederherstellung in der SAP HANA-Zielinstanz erfüllt sind. Weitere Informationen finden Sie unter [Voraussetzungen: Wiederherstellen von SAP HANA-Datenbanken in einer Azure-VM](./sap-hana-db-restore.md#prerequisites).

### <a name="why-is-the-overwrite-db-restore-failing-for-my-database"></a>Warum schlägt die Wiederherstellungsoption zum Überschreiben der Datenbank für meine Datenbank fehl?

Stellen Sie sicher, dass die Option **Überschreiben erzwingen** beim Wiederherstellen ausgewählt ist.

### <a name="why-do-i-see-the-source-and-target-systems-for-restore-are-incompatible-error"></a>Warum wird der Fehler „Quell- und Zielsystem für die Wiederherstellung sind inkompatibel“ angezeigt?

Lesen Sie den SAP HANA-Hinweis [1642148](https://launchpad.support.sap.com/#/notes/1642148), um zu ermitteln, welche Wiederherstellungstypen derzeit unterstützt werden.

### <a name="can-i-use-a-backup-of-a-database-running-on-sles-to-restore-to-an-rhel-hana-system-or-vice-versa"></a>Kann ich eine Sicherung einer laufenden Datenbank auf SLES verwenden, um sie in einem RHEL HANA-System wiederherzustellen (oder umgekehrt)?

Ja. Sie können Streamingsicherungen, die auf einer auf SLES laufenden HANA-Datenbank ausgelöst wurden, verwenden, um sie auf einem RHEL HANA-System wiederherzustellen (und umgekehrt). Das heißt, dass die betriebssystemübergreifende Wiederherstellung mithilfe von Streamingsicherungen möglich ist. Sie müssen aber sicherstellen, dass sowohl das HANA-System, auf dem Sie die Wiederherstellung durchführen möchten, als auch das HANA-System, das für die Wiederherstellung verwendet wird, gemäß SAP für die Wiederherstellung kompatibel sind. Lesen Sie SAP HANA-Hinweis [1642148](https://launchpad.support.sap.com/#/notes/1642148), um zu erfahren, welche Wiederherstellungstypen kompatibel sind.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie [SAP HANA-Datenbanken auf virtuellen Azure-Computern gesichert werden](./backup-azure-sap-hana-database.md).
