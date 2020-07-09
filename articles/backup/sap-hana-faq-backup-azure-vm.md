---
title: 'Häufig gestellte Fragen: Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern'
description: In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zum Sichern von SAP HANA-Datenbanken mit dem Azure Backup-Dienst.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 08e0eaf5f744ebb0ada07a944f627cc1ff1ac496
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84248803"
---
# <a name="frequently-asked-questions--back-up-sap-hana-databases-on-azure-vms"></a>Häufig gestellte Fragen: Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern

In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zum Sichern von SAP HANA-Datenbanken mit dem Azure Backup-Dienst.

## <a name="backup"></a>Backup

### <a name="how-many-full-backups-are-supported-per-day"></a>Wie viele vollständige Sicherungen werden pro Tag unterstützt?

Wir unterstützen nur eine vollständige Sicherung pro Tag. Es ist nicht möglich, eine differenzielle Sicherung und eine vollständige Sicherung am selben Tag auszulösen.

### <a name="do-successful-backup-jobs-create-alerts"></a>Erstellen erfolgreiche Sicherungsaufträge Warnungen?

Nein. Erfolgreiche Sicherungsaufträge generieren keine Warnungen. Warnungen werden nur für Sicherungsaufträge gesendet, bei denen ein Fehler aufgetreten ist. Ausführliche Informationen zum Verhalten von Portalwarnungen finden Sie [hier](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor). Wenn Sie jedoch daran interessiert sind, Benachrichtigungen auch für erfolgreiche Aufträge zu erhalten, können Sie [Azure Monitor](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor) verwenden.

### <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>Werden geplante Sicherungsaufträge im Menü „Sicherungsaufträge“ angezeigt?

Im Menü „Sicherungsaufträge“ werden nur Ad-hoc-Sicherungsaufträge angezeigt. Verwenden Sie für geplante Aufträge [Azure Monitor](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor).

### <a name="are-future-databases-automatically-added-for-backup"></a>Werden zukünftige Datenbanken für die Durchführung von Sicherungen automatisch hinzugefügt?

Nein, dies wird derzeit nicht unterstützt.

### <a name="if-i-delete-a-database-from-an-instance-what-will-happen-to-the-backups"></a>Was passiert mit den Sicherungen, wenn ich eine Datenbank aus einer Instanz lösche?

Wenn eine Datenbank aus einer SAP HANA-Instanz gelöscht wird, wird weiterhin versucht, Sicherungen der Datenbank durchzuführen. Dies bedeutet auch, dass die gelöschte Datenbank unter **Sicherungselemente** als fehlerhaft angezeigt wird und weiterhin geschützt ist.
Die richtige Vorgehensweise zum Beenden des Schutzes dieser Datenbank besteht darin, hierfür die Option zum **Beenden der Sicherung bei gelöschten Daten** zu verwenden.

### <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-the-behavior-be"></a>Welches Verhalten ergibt sich, wenn ich den Namen der Datenbank ändere, nachdem sie geschützt wurde?

Eine umbenannte Datenbank wird wie eine neue Datenbank behandelt. Diese Situation wird daher vom Dienst so behandelt, als ob die Datenbank nicht gefunden wurde, und für die Sicherungen tritt ein Fehler auf. Die umbenannte Datenbank wird als neue Datenbank angezeigt und muss für Schutz konfiguriert werden.

### <a name="what-are-the-prerequisites-to-back-up-sap-hana-databases-on-an-azure-vm"></a>Was sind die Voraussetzungen für die Sicherung von SAP HANA-Datenbanken auf einer Azure-VM?

Informationen finden Sie in den Abschnitten zu den [Voraussetzungen](tutorial-backup-sap-hana-db.md#prerequisites) und den [Aufgaben des Vorregistrierungsskripts](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does).

### <a name="what-permissions-should-be-set-for-azure-to-be-able-to-back-up-sap-hana-databases"></a>Welche Berechtigungen sollten für Azure festgelegt werden, damit SAP HANA-Datenbanken gesichert werden können?

Wenn Sie das Skript vor der Registrierung ausführen, werden die erforderlichen Berechtigungen festgelegt, damit Azure SAP HANA-Datenbanken sichern kann. Weitere Informationen zu den Aufgaben des Vorregistrierungsskripts finden Sie [hier](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does).

### <a name="will-backups-work-after-migrating-sap-hana-from-sdc-to-mdc"></a>Funktionieren Sicherungen nach der Migration von SAP HANA von SDC auf MDC?

Weitere Informationen finden Sie in [diesem Abschnitt](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot#sdc-to-mdc-upgrade-with-a-change-in-sid) des Leitfadens zur Problembehandlung.

### <a name="can-azure-hana-backup-be-set-up-against-a-virtual-ip-load-balancer-and-not-a-virtual-machine"></a>Kann die HANA-Sicherung in Azure für eine virtuelle IP-Adresse (Lastenausgleich) und nicht einen virtuellen Computer eingerichtet werden?

Zurzeit besteht keine Möglichkeit, die Lösung für eine virtuelle IP-Adresse allein einzurichten. Es wird ein virtueller Computer zum Ausführen der Lösung benötigt.

### <a name="i-have-a-sap-hana-system-replication-hsr-how-should-i-configure-backup-for-this-setup"></a>Ich verfüge über eine SAP HANA-Systemreplikation (HSR). Wie kann ich die Sicherung für dieses Setup konfigurieren?

Die primären und sekundären Knoten der HSR werden als zwei einzelne, nicht in Verbindung stehende VMs behandelt. Sie müssen die Sicherung auf dem primären Knoten konfigurieren, und wenn das Failover durchgeführt wird, müssen Sie die Sicherung auf dem sekundären Knoten konfigurieren (der nun zum primären Knoten wird). Es erfolgt kein automatisches „Failover“ der Sicherung auf den anderen Knoten.

### <a name="how-can-i-move-an-on-demand-backup-to-the-local-file-system-instead-of-the-azure-vault"></a>Wie kann ich eine bedarfsgesteuerte Sicherung in das lokale Dateisystem anstatt in den Azure-Tresor verschieben?

1. Warten Sie, bis die aktuell ausgeführte Sicherung für die gewünschte Datenbank abgeschlossen ist (überprüfen Sie in Studio, ob der Vorgang abgeschlossen ist)
1. Deaktivieren Sie Protokollsicherungen, und legen Sie die Katalogsicherung mithilfe der folgenden Schritte auf **Dateisystem** für die gewünschte Datenbank fest:
1. Doppelklicken Sie auf **SYSTEMDB** -> **Konfiguration** -> **Datenbank auswählen** -> **Filter (Protokoll)** .
    1. Legen Sie „enable_auto_log_backup“ auf **No** fest.
    1. Legen Sie „log_backup_using_backint“ auf **False** fest.
1. Erstellen Sie eine bedarfsgesteuerte Sicherung für die gewünschte Datenbank, und warten Sie, bis Sicherung und Katalogsicherung abgeschlossen sind.
1. Kehren Sie zu den vorherigen Einstellungen zurück, damit Sicherungen in den Azure-Tresor übertragen werden können:
    1. Legen Sie „enable_auto_log_backup“ auf **Yes** fest.
    1. Legen Sie „log_backup_using_backint“ auf **True** fest.

## <a name="restore"></a>Restore

### <a name="why-cant-i-see-the-hana-system-i-want-my-database-to-be-restored-to"></a>Warum kann ich das HANA-System nicht sehen, in dem meine Datenbank wieder hergestellt werden soll?

Überprüfen Sie, ob alle Voraussetzungen für die Wiederherstellung in der SAP HANA-Zielinstanz erfüllt sind. Weitere Informationen finden Sie unter [Voraussetzungen: Wiederherstellen von SAP HANA-Datenbanken in einer Azure-VM](https://docs.microsoft.com/azure/backup/sap-hana-db-restore#prerequisites).

### <a name="why-is-the-overwrite-db-restore-failing-for-my-database"></a>Warum schlägt die Wiederherstellungsoption zum Überschreiben der Datenbank für meine Datenbank fehl?

Stellen Sie sicher, dass die Option **Überschreiben erzwingen** beim Wiederherstellen ausgewählt ist.

### <a name="why-do-i-see-the-source-and-target-systems-for-restore-are-incompatible-error"></a>Warum wird der Fehler „Quell- und Zielsystem für die Wiederherstellung sind inkompatibel“ angezeigt?

Lesen Sie den SAP HANA-Hinweis [1642148](https://launchpad.support.sap.com/#/notes/1642148), um zu ermitteln, welche Wiederherstellungstypen derzeit unterstützt werden.

### <a name="can-i-use-a-backup-of-a-database-running-on-sles-to-restore-to-a-rhel-hana-system-or-vice-versa"></a>Kann ich eine Sicherung einer laufenden Datenbank auf SLES verwenden, um sie in einem RHEL Hana-System wiederherzustellen, oder umgekehrt?

Ja, Sie können Streamingsicherungen, die auf einer auf SLES laufenden HANA-Datenbank ausgelöst wurden, verwenden, um sie auf einem RHEL HANA-System wiederherzustellen und umgekehrt. Das heißt, dass die betriebssystemübergreifende Wiederherstellung mithilfe von Streamingsicherungen möglich ist. Sie müssen jedoch sicherstellen, dass sowohl HANA-System, auf dem Sie die Wiederherstellung durchführen möchten, als auch das HANA-System, das für die Wiederherstellung verwendet wird, für die Wiederherstellung gemäß SAP kompatibel sind. Lesen Sie SAP HANA-Hinweis [1642148](https://launchpad.support.sap.com/#/notes/1642148), um zu erfahren, welche Wiederherstellungstypen kompatibel sind.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie [SAP HANA-Datenbanken auf virtuellen Azure-Computern gesichert werden](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database).
