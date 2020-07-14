---
title: Häufig gestellte Fragen zum Sichern von Azure Files
description: In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zum Schützen von Azure-Dateifreigaben mit dem Azure Backup-Dienst.
ms.date: 04/22/2020
ms.topic: conceptual
ms.openlocfilehash: ded1551dad1be34c116e61b9bf59f372169bca5c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84488697"
---
# <a name="questions-about-backing-up-azure-files"></a>Fragen zum Sichern von Azure Files

In diesem Artikel werden allgemeine Fragen zum Sichern von Azure Files beantwortet. Einige Antworten enthalten Links zu Artikeln mit umfassenderen Informationen. Sie können auch auf der [Frageseite von Microsoft Q&A (Fragen und Antworten)](https://docs.microsoft.com/answers/topics/azure-backup.html) Fragen zum Azure Backup-Dienst posten.

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

Wenn sich die Dateifreigabe im Status „Vorläufig gelöscht“ befindet, müssen Sie zuerst den Löschvorgang der Dateifreigabe rückgängig machen, um den Wiederherstellungsvorgang auszuführen. Durch das Rückgängigmachen des Löschvorgangs wird die Dateifreigabe wieder in den aktiven Status versetzt, der eine Wiederherstellung zu einem beliebigen Zeitpunkt ermöglicht. Um zu erfahren, wie Sie die Dateifreigabe aus dem gelöschten Zustand wiederherstellen, besuchen Sie [diesen Link](https://docs.microsoft.com/azure/storage/files/storage-files-enable-soft-delete?tabs=azure-portal#restore-soft-deleted-file-share), oder sehen Sie sich das [Skript zum Wiederherstellen einer Dateifreigabe](./scripts/backup-powershell-script-undelete-file-share.md) an. Wenn die Dateifreigabe dauerhaft gelöscht wird, können Sie Inhalte und Momentaufnahmen nicht wiederherstellen.

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

### <a name="what-is-the-impact-on-existing-recovery-points-and-snapshots-when-i-modify-the-backup-policy-for-an-azure-file-share-to-switch-from-daily-policy-to-gfs-policy"></a>Welche Auswirkung hat es auf vorhandene Wiederherstellungspunkte und Momentaufnahmen, wenn ich die Backup-Richtlinie für eine Azure-Dateifreigabe von „Tägliche Richtlinie“ in „GFS-Richtlinie“ ändere?

Bei der Umstellung von einer täglichen Richtlinie auf eine GFS-Richtlinie (mit wöchentlicher/monatlicher/jährlicher Aufbewahrung) ergibt sich das folgende Verhalten:

- **Aufbewahrung**: Wenn Sie beim Ändern der Richtlinie die wöchentliche, monatliche bzw. jährliche Aufbewahrung hinzufügen, werden alle zukünftigen Wiederherstellungspunkte, die im Rahmen der geplanten Sicherung erstellt werden, gemäß der neuen Richtlinie mit Tags gekennzeichnet. Alle vorhandenen Wiederherstellungspunkte werden weiterhin als tägliche Wiederherstellungspunkte angesehen und daher nicht mit Tags als „Wöchentlich“, „Monatlich“ bzw. „Jährlich“ gekennzeichnet.

- **Bereinigung von Momentaufnahmen und Wiederherstellungspunkten**:

  - Bei einer Erweiterung der täglichen Aufbewahrung wird das Ablaufdatum der vorhandenen Wiederherstellungspunkte gemäß dem Wert für die tägliche Aufbewahrung aktualisiert, der in der neuen Richtlinie konfiguriert ist.
  - Bei einer Reduzierung der täglichen Aufbewahrung werden die vorhandenen Wiederherstellungspunkte und Momentaufnahmen so gekennzeichnet, dass sie beim nächsten Bereinigungslauf gelöscht werden (basierend auf dem Wert für die tägliche Aufbewahrung, der in der neuen Richtlinie konfiguriert ist). Das Löschen wird dann entsprechend durchgeführt.

Hier ist ein Beispiel für diesen Vorgang angegeben:

#### <a name="existing-policy-p1"></a>Vorhandene Richtlinie [P1]

|Aufbewahrungstyp |Zeitplan |Aufbewahrung  |
|---------|---------|---------|
|Täglich    |    Jeden Tag um 20 Uhr    |  100 Tage       |

#### <a name="new-policy-modified-p1"></a>Neue Richtlinie [P1 geändert]

| Aufbewahrungstyp | Zeitplan                       | Aufbewahrung |
| -------------- | ------------------------------ | --------- |
| Täglich          | Jeden Tag um 21 Uhr              | 50 Tage   |
| Wöchentlich         | Sonntags um 21 Uhr              | 3 Wochen   |
| Monatlich        | Am letzten Montag um 21 Uhr         | 1 Monat   |
| Jährlich         | Im Januar am dritten Sonntag um 21 Uhr | 4 Jahre   |

#### <a name="impact"></a>Auswirkung

1. Das Ablaufdatum der vorhandenen Wiederherstellungspunkte wird gemäß dem Wert für tägliche Aufbewahrung angepasst, der in der neuen Richtlinie enthalten ist (50 Tage). Daher werden alle Wiederherstellungspunkte, die älter als 50 Tage sind, als zu löschend gekennzeichnet.

2. Die vorhandenen Wiederherstellungspunkte werden basierend auf der neuen Richtlinie nicht als „Wöchentlich“, „Monatlich“ bzw. „Jährlich“ gekennzeichnet.

3. Alle zukünftigen Sicherungen werden gemäß dem neuen Zeitplan ausgelöst (um 21 Uhr).

4. Das Ablaufdatum aller zukünftigen Wiederherstellungspunkte wird anhand der neuen Richtlinie ausgerichtet.

>[!NOTE]
>Die Richtlinienänderungen wirken sich nur auf die Wiederherstellungspunkte aus, die im Rahmen der geplanten Ausführung des Sicherungsauftrags erstellt werden. Bei bedarfsgesteuerten Sicherungen richtet sich die Aufbewahrung nach dem Wert für **Aufbewahren bis**, der bei der Durchführung des Sicherungsvorgangs angegeben wurde.

### <a name="what-is-the-impact-on-existing-recovery-points-when-i-modify-an-existing-gfs-policy"></a>Welche Auswirkung ergibt sich für die vorhandenen Wiederstellungspunkte, wenn ich eine vorhandene GFS-Richtlinie ändere?

Wenn eine neue Richtlinie auf Dateifreigaben angewendet wird, werden alle zukünftigen geplanten Sicherungen gemäß dem Zeitplan erstellt, der in der geänderten Richtlinie konfiguriert ist.  Die Aufbewahrung aller vorhandenen Wiederherstellungspunkte basiert auf den neuen konfigurierten Werten für die Aufbewahrung. Wenn die Aufbewahrung erweitert wird, werden vorhandene Wiederherstellungspunkte so gekennzeichnet, dass sie gemäß der neuen Richtlinie aufbewahrt werden. Bei einer Reduzierung der Aufbewahrung werden sie für die Löschung im Rahmen des nächsten Bereinigungsauftrags gekennzeichnet und dann entsprechend gelöscht.

Hier ist ein Beispiel für diesen Vorgang angegeben:

#### <a name="existing-policy-p2"></a>Vorhandene Richtlinie [P2]

| Aufbewahrungstyp | Zeitplan           | Aufbewahrung |
| -------------- | ------------------ | --------- |
| Täglich          | Jeden Tag um 20 Uhr | 50 Tage   |
| Wöchentlich         | Montags um 20 Uhr  | 3 Wochen   |

#### <a name="new-policy-modified-p2"></a>Neue Richtlinie [P2 geändert]

| Aufbewahrungstyp | Zeitplan               | Aufbewahrung |
| -------------- | ---------------------- | --------- |
| Täglich          | Jeden Tag um 21 Uhr     | 10 Tage   |
| Wöchentlich         | Montags um 21 Uhr      | Zwei Wochen   |
| Monatlich        | Am letzten Montag um 21 Uhr | 2 Monate  |

#### <a name="impact-of-change"></a>Auswirkung der Änderung

1. Das Ablaufdatum der vorhandenen täglichen Wiederherstellungspunkte wird am neuen Wert für die tägliche Aufbewahrung ausgerichtet (zehn Tage). Dies bedeutet, dass alle täglichen Wiederherstellungspunkte gelöscht werden, die älter als zehn Tage sind.

2. Das Ablaufdatum der vorhandenen wöchentlichen Wiederherstellungspunkte wird am neuen Wert für die wöchentliche Aufbewahrung ausgerichtet (zwei Wochen). Dies bedeutet, dass alle wöchentlichen Wiederherstellungspunkte gelöscht werden, die älter als zwei Wochen sind.

3. Die monatlichen Wiederherstellungspunkte werden erst bei zukünftigen Sicherungen anhand der Konfiguration der neuen Richtlinie erstellt.

4. Das Ablaufdatum aller zukünftigen Wiederherstellungspunkte wird anhand der neuen Richtlinie ausgerichtet.

>[!NOTE]
>Die Richtlinienänderungen wirken sich nur auf die Wiederherstellungspunkte aus, die im Rahmen des geplanten Sicherungsvorgangs erstellt werden. Bei bedarfsgesteuerten Sicherungen richtet sich die Aufbewahrung nach dem Wert für **Aufbewahren bis**, der bei der Durchführung des Sicherungsvorgangs angegeben wurde.

## <a name="next-steps"></a>Nächste Schritte

- [Behandeln von Problemen beim Sichern von Azure-Dateifreigaben](troubleshoot-azure-files.md)
