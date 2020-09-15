---
title: Schutz vor versehentlichem Löschen für Azure-Dateifreigaben
description: Erfahren Sie, wie Sie mit dem vorläufigen Löschen Ihre Azure-Dateifreigaben vor versehentlichem Löschen schützen können.
ms.topic: conceptual
ms.date: 02/02/2020
ms.custom: references_regions
ms.openlocfilehash: 52a116320e07c25d4ee7f00b8063ca15faeb8560
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89179911"
---
# <a name="accidental-delete-protection-for-azure-file-shares-using-azure-backup"></a>Schutz vor versehentlichem Löschen für Azure-Dateifreigaben mit Azure Backup

Zum Schutz vor Cyberangriffen und versehentlichem Löschen wird [vorläufiges Löschen](../storage/files/storage-files-prevent-file-share-deletion.md) für alle Dateifreigaben in einem Speicherkonto aktiviert, wenn Sie die Sicherung für eine Dateifreigabe im entsprechenden Speicherkonto konfigurieren. Beim vorläufigen Löschen werden die Inhalte und Wiederherstellungspunkte (Momentaufnahmen) der Dateifreigabe für einen Zeitraum von mindestens 14 zusätzlichen Tagen beibehalten. Dies ermöglicht die Wiederherstellung von Dateifreigaben ohne Datenverlust, selbst wenn ein böswilliger Akteur die Dateifreigabe löscht.  Vorläufiges Löschen wird für Storage Standard- und Storage Premium-Konten unterstützt. Die Einstellung wird durch Azure Backup für alle Speicherkonten aktiviert, auf denen gesicherte Dateifreigaben gehostet werden.

Im folgenden Flussdiagramm sind die unterschiedlichen Schritte und Zustände eines Sicherungselements bei aktiviertem vorläufigem Löschen für Dateifreigaben in einem Speicherkonto dargestellt:

 ![Flussdiagramm für vorläufiges Löschen](./media/soft-delete-afs/soft-delete-flow-chart.png)

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="when-will-soft-delete-be-enabled-for-file-shares-in-my-storage-account"></a>Wann wird vorläufiges Löschen für Dateifreigaben in meinem Speicherkonto aktiviert?

Wenn Sie die Sicherung zum ersten Mal für eine beliebige Dateifreigabe in einem Speicherkonto konfigurieren, aktiviert der Azure Backup-Dienst vorläufiges Löschen für alle Dateifreigaben im entsprechenden Speicherkonto.

### <a name="can-i-configure-the-number-of-days-for-which-my-snapshots-and-restore-points-will-be-retained-in-soft-deleted-state-after-i-delete-the-file-share"></a>Kann ich konfigurieren, wie viele Tage meine Momentaufnahmen und Wiederherstellungspunkte nach dem Löschen der Dateifreigabe im vorläufig gelöschten Zustand aufbewahrt werden?

Ja, Sie können die Beibehaltungsdauer nach Ihren Anforderungen festlegen. In [diesem Dokument](../storage/files/storage-files-enable-soft-delete.md?tabs=azure-portal) werden die Schritte zum Konfigurieren der Beibehaltungsdauer erläutert. Bei Speicherkonten mit gesicherten Dateifreigaben sollte die Einstellung für die Beibehaltung mindestens 14 Tage betragen.

### <a name="does-azure-backup-reset-my-retention-setting-because-i-configured-it-to-less-than-14-days"></a>Setzt Azure Backup meine Einstellung für die Beibehaltung zurück, wenn ich sie auf weniger als 14 Tage festgelegt habe?

Aus Sicherheitsgründen wird empfohlen, bei Speicherkonten mit gesicherten Dateifreigaben eine Aufbewahrungsdauer von mindestens 14 Tagen einzurichten. Wenn Azure Backup bei einem Sicherungsauftrag auf eine Einstellung von weniger als 14 Tagen stößt, wird die Einstellung daher jedes Mal auf 14 Tage zurückgesetzt.

### <a name="what-is-the-cost-incurred-during-the-retention-period"></a>Welche Kosten fallen während der Beibehaltungsdauer an?

Während des Zeitraums im vorläufig gelöschten Zustand bleiben die Kosten für die geschützte Instanz und die Speicherung der Momentaufnahme unverändert.  Außerdem wird Ihnen die genutzte Kapazität zum normalen Tarif für Standard-Dateifreigaben und zum Momentaufnahme-Speichertarif für Premium-Dateifreigaben in Rechnung gestellt.

### <a name="can-i-perform-a-restore-operation-when-my-data-is-in-soft-deleted-state"></a>Kann ich einen Wiederherstellungsvorgang durchführen, wenn sich meine Daten im vorläufig gelöschten Zustand befinden?

Sie müssen die vorläufig gelöschte Dateifreigabe zuerst wiederherstellen, um Wiederherstellungsvorgänge auszuführen. Mit dem Vorgang zum Aufheben des Löschvorgangs wird die Dateifreigabe zurück in den gesicherten Zustand versetzt, aus dem Sie sie jederzeit wiederherstellen können. Um zu erfahren, wie Sie die Dateifreigabe aus dem gelöschten Zustand wiederherstellen, besuchen Sie [diesen Link](../storage/files/storage-files-enable-soft-delete.md?tabs=azure-portal#restore-soft-deleted-file-share), oder sehen Sie sich das [Skript zum Wiederherstellen einer Dateifreigabe](./scripts/backup-powershell-script-undelete-file-share.md) an.

### <a name="how-can-i-purge-the-data-of-a-file-share-in-a-storage-account-that-has-at-least-one-protected-file-share"></a>Wie kann ich die Daten einer Dateifreigabe in einem Speicherkonto bereinigen, das mindestens eine geschützte Dateifreigabe enthält?

Wenn eines Ihrer Speicherkonten mindestens eine geschützte Dateifreigabe enthält, bedeutet dies, dass vorläufiges Löschen für alle Dateifreigaben in diesem Konto aktiviert ist und Ihre Daten nach dem Löschvorgang 14 Tage lang aufbewahrt werden. Wenn Sie die Daten jedoch sofort entfernen und nicht beibehalten möchten, führen Sie die folgenden Schritte aus:

1. Wenn Sie die Dateifreigabe beim Aktivieren des vorläufigen Löschens bereits gelöscht haben, heben Sie den Löschvorgang der Dateifreigabe zuerst über das [Azure Files-Portal](../storage/files/storage-files-enable-soft-delete.md?tabs=azure-portal#restore-soft-deleted-file-share) oder mit dem [Skript zum Wiederherstellen einer Dateifreigabe](./scripts/backup-powershell-script-undelete-file-share.md) auf.
2. Deaktivieren Sie das vorläufige Löschen von Dateifreigaben in Ihrem Speicherkonto, indem Sie die Schritte in [diesem Dokument](../storage/files/storage-files-enable-soft-delete.md?tabs=azure-portal#disable-soft-delete) ausführen.
3. Löschen Sie nun die Dateifreigabe, deren Inhalt Sie sofort entfernen möchten.

>[!NOTE]
>Sie sollten Schritt 2 ausführen, bevor der nächste geplante Sicherungsauftrag für die geschützte Dateifreigabe in Ihrem Speicherkonto ausgeführt wird. Wenn der Sicherungsauftrag ausgeführt wird, aktiviert er das vorläufige Löschen für alle Dateifreigaben im Speicherkonto erneut.

>[!WARNING]
>Nach dem Deaktivieren des vorläufigen Löschens in Schritt 2 ist jeder Löschvorgang für Dateifreigaben ein dauerhafter Löschvorgang. Wenn Sie die gesicherte Dateifreigabe versehentlich löschen, nachdem Sie das vorläufige Löschen deaktiviert haben, gehen somit alle Momentaufnahmen verloren, und die Daten können nicht mehr wiederhergestellt werden.

### <a name="in-the-context-of-a-file-shares-soft-delete-setting-what-changes-does-azure-backup-do-when-i-unregister-a-storage-account"></a>Welche Änderungen führt Azure Backup im Zusammenhang mit der Einstellung für vorläufiges Löschen einer Dateifreigabe durch, wenn ich die Registrierung eines Speicherkontos aufhebe?

Zum Zeitpunkt der Aufhebung der Registrierung prüft Azure Backup die Einstellung für die Beibehaltungsdauer von Dateifreigaben. Wenn sie einen anderen Wert als 14 Tage aufweist, bleibt die Beibehaltung unverändert. Wenn die Beibehaltung jedoch 14 Tage beträgt, wird dies von Azure Backup als aktiviert betrachtet, sodass vorläufiges Löschen bei der Aufhebung der Registrierung deaktiviert wird. Wenn Sie die Registrierung des Speicherkontos aufheben, die Beibehaltungsdauer jedoch unverändert lassen möchten, aktivieren Sie sie erneut über den Bereich des Speicherkontos, nachdem Sie die Aufhebung der Registrierung abgeschlossen haben. Informationen zu den Konfigurationsschritten finden Sie unter [diesem Link](../storage/files/storage-files-enable-soft-delete.md?tabs=azure-portal#restore-soft-deleted-file-share).

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit dem [Sichern von Azure-Dateifreigaben in einem Recovery Services-Tresor](backup-afs.md) vertraut.
