---
title: 'Häufig gestellte Fragen: Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern'
description: In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zum Sichern von SAP HANA-Datenbanken mit dem Azure Backup-Dienst.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: d9d10e38885ba814045d8476b83671153feb7b8c
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "77919684"
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

### <a name="will-backups-work-after-migrating-sap-hana-from-10-to-20"></a>Funktionieren Sicherungen nach der Migration von SAP HANA aus Version 1.0 zu 2.0?

Weitere Informationen finden Sie in [diesem Abschnitt](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot#upgrading-from-sap-hana-10-to-20) des Leitfadens zur Problembehandlung.

## <a name="restore"></a>Restore

### <a name="why-cant-i-see-the-hana-system-i-want-my-database-to-be-restored-to"></a>Warum kann ich das HANA-System nicht sehen, in dem meine Datenbank wieder hergestellt werden soll?

Überprüfen Sie, ob alle Voraussetzungen für die Wiederherstellung in der SAP HANA-Zielinstanz erfüllt sind. Weitere Informationen finden Sie unter [Voraussetzungen: Wiederherstellen von SAP HANA-Datenbanken in einer Azure-VM](https://docs.microsoft.com/azure/backup/sap-hana-db-restore#prerequisites).

### <a name="why-is-the-overwrite-db-restore-failing-for-my-database"></a>Warum schlägt die Wiederherstellungsoption zum Überschreiben der Datenbank für meine Datenbank fehl?

Stellen Sie sicher, dass die Option **Überschreiben erzwingen** beim Wiederherstellen ausgewählt ist.

### <a name="why-do-i-see-the-source-and-target-systems-for-restore-are-incompatible-error"></a>Warum wird der Fehler „Quell- und Zielsystem für die Wiederherstellung sind inkompatibel“ angezeigt?

Lesen Sie den SAP HANA-Hinweis [1642148](https://launchpad.support.sap.com/#/notes/1642148), um zu ermitteln, welche Wiederherstellungstypen derzeit unterstützt werden.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie [SAP HANA-Datenbanken auf virtuellen Azure-Computern gesichert werden](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database).
