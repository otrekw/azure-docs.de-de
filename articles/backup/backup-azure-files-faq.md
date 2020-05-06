---
title: Häufig gestellte Fragen zum Sichern von Azure Files
description: In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zum Schützen von Azure-Dateifreigaben mit dem Azure Backup-Dienst.
ms.date: 04/22/2020
ms.topic: conceptual
ms.openlocfilehash: d7b19fd11e6784a188a18f6a613eef5ff4f77764
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82105640"
---
# <a name="questions-about-backing-up-azure-files"></a>Fragen zum Sichern von Azure Files

In diesem Artikel werden allgemeine Fragen zum Sichern von Azure Files beantwortet. Einige Antworten enthalten Links zu Artikeln mit umfassenderen Informationen. Außerdem können Sie Fragen zum Azure Backup-Dienst im [Diskussionsforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup)stellen.

Verwenden Sie die Links auf der rechten Seite unter **In diesem Artikel**, um sich einen Überblick über die Abschnitte dieses Artikels zu verschaffen.

## <a name="configuring-the-backup-job-for-azure-files"></a>Konfigurieren des Sicherungsauftrags für Azure Files

### <a name="why-cant-i-see-some-of-my-storage-accounts-that-i-want-to-protect-which-contain-valid-azure-file-shares"></a>Warum werden einige Speicherkonten nicht angezeigt, die geschützt werden sollen und gültige Azure-Dateifreigaben enthalten?

Sehen Sie in der [Unterstützungsmatrix für die Sicherung von Azure-Dateifreigaben](azure-file-share-support-matrix.md) nach, um sicherzustellen, dass das Speicherkonto einem der unterstützten Speicherkontotypen angehört. Es ist ferner möglich, dass das Speicherkonto, nach dem Sie suchen, bereits geschützt oder bei einem anderen Tresor registriert ist. [Heben Sie die Registrierung des Speicherkontos beim Tresor auf](manage-afs-backup.md#unregister-a-storage-account), um das Speicherkonto in anderen Tresoren für den Schutz zu ermitteln.

### <a name="why-cant-i-see-some-of-my-azure-file-shares-in-the-storage-account-when-im-trying-to-configure-backup"></a>Warum werden beim Konfigurieren der Sicherung einige Azure-Dateifreigaben im Speicherkonto nicht angezeigt?

Überprüfen Sie, ob die Azure-Dateifreigabe bereits in demselben Recovery Services-Tresor geschützt wird oder vor Kurzem gelöscht wurde.

### <a name="can-i-protect-file-shares-connected-to-a-sync-group-in-azure-files-sync"></a>Kann ich Dateifreigaben schützen, die mit einer Synchronisierungsgruppe in Azure Files Sync verbunden sind?

Ja. Der Schutz von Azure-Dateifreigaben, die mit Synchronisierungsgruppen verbunden sind, ist aktiviert.

### <a name="when-trying-to-back-up-file-shares-i-clicked-on-a-storage-account-for-discovering-the-file-shares-in-it-however-i-didnt-protect-them-how-do-i-protect-these-file-shares-with-any-other-vault"></a>Beim Sichern von Dateifreigaben habe ich auf ein Speicherkonto geklickt, um die darin enthaltenen Dateifreigaben zu ermitteln. Ich habe sie jedoch nicht geschützt. Wie schütze ich diese Dateifreigaben mit einem anderen Tresor?

Wenn Sie beim Sichern ein Speicherkonto auswählen, um die darin enthaltenen Dateifreigaben zu ermitteln, wird das Speicherkonto bei dem Tresor registriert, über den der Vorgang ausgeführt wird. Falls Sie die Dateifreigaben bei einem anderen Tresor registrieren möchten, [heben Sie die Registrierung des ausgewählten Speicherkontos bei diesem Tresor auf](manage-afs-backup.md#unregister-a-storage-account).

### <a name="can-i-change-the-vault-to-which-i-back-up-my-file-shares"></a>Kann ich den Tresor ändern, in dem ich meine Dateifreigaben sichere?

Ja. Sie müssen jedoch im verknüpften Tresor [den Schutz einer Dateifreigabe beenden](manage-afs-backup.md#stop-protection-on-a-file-share), die [Registrierung dieses Speicherkontos aufheben](manage-afs-backup.md#unregister-a-storage-account) und es dann über einen anderen Tresor schützen.

### <a name="how-many-azure-file-shares-can-i-protect-in-a-vault"></a>Wie viele Azure-Dateifreigaben kann ich in einem Tresor schützen?

Sie können Azure-Dateifreigaben aus bis zu 50 Speicherkonten pro Tresor schützen. Sie können auch bis zu 200 Azure-Dateifreigaben in einem einzelnen Tresor schützen.

### <a name="can-i-protect-two-different-file-shares-from-the-same-storage-account-to-different-vaults"></a>Kann ich zwei verschiedene Dateifreigaben im gleichen Speicherkonto in verschiedenen Tresoren schützen?

Nein. Alle Dateifreigaben in einem Speicherkonto können nur durch denselben Tresor geschützt werden.

## <a name="backup"></a>Backup

### <a name="what-should-i-do-if-my-backups-start-failing-due-to-the-maximum-limit-reached-error"></a>Wie ist vorzugehen, wenn Sicherungen aufgrund des erreichten Grenzwerts Fehler aufzuweisen beginnen?

Für eine Dateifreigabe können jeweils bis zu 200 Momentaufnahmen vorhanden sein. In die Berechnung dieses Grenzwerts werden auch Momentaufnahmen einbezogen, die mit Azure Backup erstellt werden. Dies wird durch Ihre Richtlinie definiert. Falls für Ihre Sicherungen nach dem Erreichen des Grenzwerts Fehler auftreten, sollten Sie bedarfsgesteuerte Momentaufnahmen löschen, damit die Erstellung von Sicherungen wieder erfolgreich ist.

## <a name="restore"></a>Restore

### <a name="can-i-recover-from-a-deleted-azure-file-share"></a>Kann die Wiederherstellung auf der Grundlage einer gelöschten Azure-Dateifreigabe erfolgen?

Beim Löschen einer Azure-Dateifreigabe wird die Liste mit den Sicherungen angezeigt, die gelöscht werden, und Sie werden zum Bestätigen aufgefordert. Zurzeit kann eine gelöschte Azure-Dateifreigabe nicht wiederhergestellt werden.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-an-azure-file-share"></a>Kann ich Sicherungen wiederherstellen, wenn ich den Schutz für eine Azure-Dateifreigabe beendet habe?

Ja. Wenn Sie beim Beenden des Schutzes die Option **Sicherungsdaten beibehalten** ausgewählt haben, können Sie alle vorhandenen Wiederherstellungspunkte wiederherstellen.

### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>Was geschieht, wenn ich einen laufenden Wiederherstellungsauftrag abbreche?

Wenn ein laufender Wiederherstellungsauftrag abgebrochen wird, wird der Wiederherstellungsvorgang beendet, und alle vor dem Abbruch wiederhergestellten Dateien verbleiben ohne Rollbacks am konfigurierten Ziel (ursprünglicher oder alternativer Speicherort).

## <a name="manage-backup"></a>Verwalten von Sicherungen

### <a name="can-i-use-powershell-to-configuremanagerestore-backups-of-azure-file-shares"></a>Kann ich PowerShell zum Konfigurieren/Verwalten/Wiederherstellen von Sicherungen von Azure-Dateifreigaben verwenden?

Ja. Informationen hierzu finden Sie in der ausführlichen Dokumentation unter [diesem Link](backup-azure-afs-automation.md).

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-them"></a>Kann ich auf die von Azure Backup erstellten Momentaufnahmen zugreifen und sie einbinden?

Auf alle von Azure Backup erstellten Momentaufnahmen kann im Portal, über PowerShell oder mithilfe der CLI über die Funktion zum Anzeigen von Momentaufnahmen zugegriffen werden. Weitere Informationen zu Azure Files-Freigabemomentaufnahmen finden Sie unter [Übersicht über Freigabemomentaufnahmen für Azure Files](../storage/files/storage-snapshots-files.md).

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups"></a>Welche maximale Aufbewahrungsdauer kann ich für Sicherungen konfigurieren?

Ausführliche Informationen zur maximalen Aufbewahrung finden Sie in der [Unterstützungsmatrix](azure-file-share-support-matrix.md). Azure Backup führt eine Echtzeitberechnung der Anzahl von Momentaufnahmen durch, wenn Sie beim Konfigurieren der Sicherungsrichtlinie die Aufbewahrungswerte eingeben. Sobald die Anzahl der Momentaufnahmen, die Ihren definierten Aufbewahrungswerten entsprechen, 200 überschreitet, zeigt das Portal eine Warnung an, die Sie auffordert, Ihre Aufbewahrungswerte anzupassen. Dadurch wird die maximale Anzahl Momentaufnahmen, die von Azure Files für eine beliebige Dateifreigabe unterstützt werden, zu keiner Zeit überschritten.

### <a name="what-happens-when-i-change-the-backup-policy-for-an-azure-file-share"></a>Was passiert, wenn ich die Sicherungsrichtlinie für eine Azure-Dateifreigabe ändere?

Wenn eine neue Richtlinie auf Dateifreigaben angewendet wird, werden der Zeitplan und die Aufbewahrung der neuen Richtlinie beachtet. Bei einer Ausweitung der Aufbewahrung werden bereits vorhandene Wiederherstellungspunkte markiert, um sie gemäß der neuen Richtlinie aufzubewahren. Bei einer Verkürzung der Aufbewahrung werden sie im Rahmen der nächsten Bereinigung gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen Bereichen von Azure Backup finden Sie in diesen weiteren Artikeln mit häufig gestellten Fragen zu Sicherungen:

- [Häufig gestellte Fragen zum Recovery Services-Tresor](backup-azure-backup-faq.md)
- [Häufig gestellte Fragen zur Azure-VM-Sicherung](backup-azure-vm-backup-faq.md)
- [Häufig gestellte Fragen zur Sicherung von Dateien/Ordnern mit dem Azure Backup-Agent](backup-azure-file-folder-backup-faq.md)
