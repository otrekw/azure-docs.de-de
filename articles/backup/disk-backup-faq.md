---
title: Häufig gestellte Fragen zu Azure Disk Backup
description: Hier finden Sie Antworten auf häufig gestellte Fragen zu Azure Disk Backup.
ms.topic: conceptual
ms.date: 01/07/2021
ms.openlocfilehash: 4c4c9f4b8388fed95a19c49b705981b9b9bce2e0
ms.sourcegitcommit: 6628bce68a5a99f451417a115be4b21d49878bb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2021
ms.locfileid: "98556746"
---
# <a name="frequently-asked-questions-about-azure-disk-backup-in-preview"></a>Häufig gestellte Fragen zu Azure Disk Backup (Vorschau)

>[!IMPORTANT]
>Die Vorschauversion von Azure Disk Backup wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Informationen zur regionalen Verfügbarkeit finden Sie in der [Unterstützungsmatrix](disk-backup-support-matrix.md).
>
>[Füllen Sie dieses Formular aus](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR1vE8L51DIpDmziRt_893LVUNFlEWFJBN09PTDhEMjVHS05UWFkxUlUzUS4u), wenn Sie sich für die Vorschau registrieren möchten.

In diesem Artikel werden häufig gestellte Fragen zu Azure Disk Backup beantwortet. Weitere Informationen zur regionalen Verfügbarkeit von [Azure Disk Backup](disk-backup-overview.md), zu unterstützten Szenarien und zu Einschränkungen finden Sie in der [Unterstützungsmatrix](disk-backup-support-matrix.md).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="can-i-back-up-the-disk-using-the-azure-disk-backup-solution-if-the-same-disk-is-backed-up-using-azure-virtual-machine-backup"></a>Kann ich einen Datenträger mithilfe der Azure Disk Backup-Lösung sichern, wenn derselbe Datenträger bereits mit Azure Backup für VMs gesichert wird?

Azure Backup bietet eine parallele Unterstützung für die Sicherung verwalteter Datenträger mithilfe der Sicherungslösungen für Datenträger und [Azure-VMs](backup-azure-vms-introduction.md). Dies ist hilfreich, wenn Sie einmal pro Tag eine anwendungskonsistente Sicherung von VMs und gleichzeitig häufiger absturzkonsistente Sicherungen des Betriebssystemdatenträgers oder eines bestimmten anderen Datenträgers durchführen möchten, ohne die Leistung der Anwendung in der Produktion zu beeinträchtigen.

### <a name="how-do-i-find-the-snapshot-resource-group-that-i-used-to-configure-backup-for-a-disk"></a>Wie ermittle ich die Momentaufnahme-Ressourcengruppe, die ich zum Konfigurieren der Sicherung für einen Datenträger verwendet habe?

Auf dem Bildschirm **Sicherungsinstanz** finden Sie ein Feld mit der Momentaufnahme-Ressourcengruppe im Abschnitt **Zusammenfassung**. Sie können die Sicherungsinstanz des entsprechenden Datenträgers im Backup Center oder im Sicherungstresor suchen und auswählen.

![Feld mit der Momentaufnahme-Ressourcengruppe](./media/disk-backup-faq/snapshot-resource-group.png)

### <a name="what-is-a-snapshot-resource-group"></a>Was ist eine Momentaufnahme-Ressourcengruppe?

Azure Disk Backup bietet Sicherungen auf Betriebsebene für verwaltete Datenträger. Das bedeutet, dass die während der geplanten und bedarfsgesteuerten Sicherungsvorgänge erstellten Momentaufnahmen in einer Ressourcengruppe in Ihrem Abonnement gespeichert werden. Azure Backup bietet eine sofortige Wiederherstellung, da die inkrementellen Momentaufnahmen in Ihrem Abonnement gespeichert sind. Diese Ressourcengruppe wird als Momentaufnahme-Ressourcengruppe bezeichnet. Weitere Informationen finden Sie unter [Konfigurieren der Sicherung](backup-managed-disks.md#configure-backup).

### <a name="why-must-the-snapshot-resource-group-be-in-same-subscription-as-that-of-the-disk-being-backed-up"></a>Warum muss sich die Momentaufnahme-Ressourcengruppe im selben Abonnement befinden wie der zu sichernde Datenträger?

Sie können keine inkrementelle Momentaufnahme für einen bestimmten Datenträger außerhalb des Abonnements dieses Datenträgers erstellen. Wählen Sie daher eine Ressourcengruppe im selben Abonnement wie der zu sichernde Datenträger aus. Weitere Informationen finden Sie unter [Inkrementelle Momentaufnahmen](https://docs.microsoft.com/azure/virtual-machines/windows/disks-incremental-snapshots-portal#restrictions) für verwaltete Datenträger.

### <a name="why-do-i-need-to-provide-role-assignments-to-be-able-to-configure-backups-perform-scheduled-and-on-demand-backups-and-restore-operations"></a>Warum müssen Rollenzuweisungen bereitgestellt werden, die berechtigt sind, Sicherungen zu konfigurieren sowie geplante und bedarfsgesteuerte Sicherungs- und Wiederherstellungsvorgänge durchzuführen?

Azure Disk Backup wendet den Ansatz der geringstmöglichen Berechtigungen für das Ermitteln, Schützen und Wiederherstellen der verwalteten Datenträger in Ihren Abonnements an. Um dies zu erreichen, verwendet Azure Backup die verwaltete Identität des [Sicherungstresors](backup-vault-overview.md), um auf andere Azure-Ressourcen zuzugreifen. Eine systemseitig zugewiesene verwaltete Identität ist auf eine Ressource beschränkt und an den Lebenszyklus dieser Ressource gebunden. Sie können der verwalteten Identität mithilfe der rollenbasierten Zugriffssteuerung von Azure (Azure RBAC) Berechtigungen erteilen. Eine verwaltete Identität ist ein spezieller Dienstprinzipal, der nur zusammen mit Azure-Ressourcen verwendet werden kann. Informieren Sie sich ausführlicher über [verwaltete Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). Der Sicherungstresor verfügt standardmäßig nicht über die Berechtigung für das Zugreifen auf den zu sichernden Datenträger, das Erstellen regelmäßiger Momentaufnahmen, das Löschen von Momentaufnahmen nach der Beibehaltungsdauer und das Wiederherstellen eines Datenträgers aus einer Sicherung. Durch das explizite Gewähren von Rollenzuweisungen für die verwaltete Identität des Sicherungstresors haben Sie die volle Kontrolle über die Verwaltung der Berechtigungen für die Ressourcen in den Abonnements.

### <a name="why-does-backup-policy-limit-the-retention-duration"></a>Warum schränkt die Sicherungsrichtlinie die Beibehaltungsdauer ein?

Azure Disk Backup verwendet inkrementelle Momentaufnahmen, die auf 200 Momentaufnahmen pro Datenträger beschränkt sind. Um zusätzlich zu den geplanten Sicherungen noch bedarfsgesteuerte Sicherungen zu ermöglichen, schränkt die Sicherungsrichtlinie die Gesamtanzahl der Sicherungen auf 180 ein. Weitere Informationen finden Sie unter [Inkrementelle Momentaufnahmen](https://docs.microsoft.com/azure/virtual-machines/windows/disks-incremental-snapshots-portal#restrictions) für verwaltete Datenträger.

### <a name="how-does-the-hourly-and-daily-backup-frequency-work-in-the-backup-policy"></a>Wie funktioniert die stündliche und tägliche Sicherungshäufigkeit in der Sicherungsrichtlinie?

Azure Disk Backup ermöglicht mehrere Sicherungen pro Tag. Wenn Sie häufigere Sicherungen benötigen, wählen Sie als Sicherungshäufigkeit **Stündlich** aus. Die Sicherungen werden basierend auf dem unter **Zeit** ausgewählten Intervall geplant. Wenn Sie z. B. **Alle vier Stunden** auswählen, werden die Sicherungen ungefähr alle 4 Stunden durchgeführt, sodass sie gleichmäßig über den Tag verteilt sind. Wenn eine Sicherung pro Tag ausreichend ist, wählen Sie als Sicherungshäufigkeit **Täglich** aus. Bei der Sicherungshäufigkeit „Täglich“ können Sie die Uhrzeit angeben, zu der die Sicherungen erstellt werden. Es muss beachtet werden, dass mit der Uhrzeit die Startzeit der Sicherung angegeben wird und nicht die Zeit, zu der die Sicherung abgeschlossen ist. Die für den Abschluss des Sicherungsvorgangs erforderliche Zeit hängt von verschiedenen Faktoren ab, einschließlich des Churns zwischen aufeinanderfolgenden Sicherungen. Azure Disk Backup ist jedoch ein agentloser Sicherungsdienst mit [inkrementellen Momentaufnahmen](https://docs.microsoft.com/azure/virtual-machines/windows/disks-incremental-snapshots-portal), die sich nicht auf die Leistung der Anwendung in der Produktion auswirken.

### <a name="why-does-the-backup-vaults-redundancy-setting-not-apply-to-the-backups-stored-in-operational-tier-the-snapshot-resource-group"></a>Warum gilt die Redundanzeinstellung des Sicherungstresors nicht für die Sicherungen, die auf der Betriebsebene gespeichert sind (die Momentaufnahme-Ressourcengruppe)?

Azure Backup verwendet [inkrementelle Momentaufnahmen](https://docs.microsoft.com/azure/virtual-machines/windows/disks-incremental-snapshots-portal#restrictions) von verwalteten Datenträgern, auf denen nur die Deltaänderungen auf den Datenträgern seit der letzten Momentaufnahme im HDD Standard-Speicher gespeichert werden. Dies ist auch unabhängig vom Speichertyp des übergeordneten Datenträgers. Für eine höhere Zuverlässigkeit werden inkrementelle Momentaufnahmen standardmäßig in zonenredundantem Speicher (ZRS) in Regionen gespeichert, die ZRS unterstützen. Derzeit unterstützt Azure Disk Backup operative Sicherungen von verwalteten Datenträgern, bei denen die Sicherungen nicht in den Speicher des Sicherungstresors kopiert werden. Die Redundanzeinstellung für den Sicherungsspeicher des Sicherungstresors gilt daher nicht für die Wiederherstellungspunkte.

### <a name="can-i-use-backup-center-to-configure-backups-and-manage-backup-instances-for-azure-disks"></a>Kann ich das Backup Center verwenden, um Sicherungen zu konfigurieren und Sicherungsinstanzen für Azure-Datenträger zu verwalten?

Ja, Azure Disk Backup ist in das [Backup Center](backup-center-overview.md) integriert, das eine **zentralisierte einheitliche Verwaltungsumgebung** in Azure bietet, mit der Unternehmen Sicherungen im großen Stil steuern, überwachen, betreiben und analysieren können. Sie können den Sicherungstresor auch zum Sichern, Wiederherstellen und Verwalten der Sicherungsinstanzen verwenden, die im Tresor geschützt werden.

### <a name="why-do-i-need-to-create-a-backup-vault-and-not-use-a-recovery-services-vault"></a>Warum muss ich einen Sicherungstresor erstellen, anstatt einen Recovery Services-Tresor zu verwenden?

Ein Sicherungstresor ist eine Speicherentität in Azure für Sicherungsdaten bestimmter neuerer, von Azure Backup unterstützter Workloads. Sie können Sicherungstresore für Sicherungsdaten von verschiedenen Azure-Diensten verwenden, z. B. von Azure Database for PostgreSQL-Servern, Azure-Datenträgern und neueren Workloads, die Azure Backup unterstützen. Sicherungstresore vereinfachen die Organisation Ihrer Sicherungsdaten und minimieren gleichzeitig den Verwaltungsaufwand. Weitere Informationen finden Sie unter [Sicherungstresore](https://docs.microsoft.com/azure/backup/backup-vault-overview).

### <a name="can-the-disk-to-be-backed-up-and-the-backup-vault-be-in-different-subscriptions"></a>Können sich der zu sichernde Datenträger und der Sicherungstresor in unterschiedlichen Abonnements befinden?

Ja, der zu sichernde verwaltete Quelldatenträger und der Sicherungstresor können sich in unterschiedlichen Abonnements befinden.

### <a name="can-the-disk-to-be-backed-up-and-the-backup-vault-be-in-different-regions"></a>Können sich der zu sichernde Datenträger und der Sicherungstresor in unterschiedlichen Regionen befinden?

Nein, derzeit müssen sich der zu sichernde verwaltete Quelldatenträger und der Sicherungstresor in derselben Region befinden.

### <a name="can-i-restore-a-disk-into-a-different-subscription"></a>Kann ich einen Datenträger in einem anderen Abonnement wiederherstellen?

Ja, Sie können den Datenträger in einem anderen Abonnement wiederherstellen als dem des verwalteten Quelldatenträgers, von dem die Sicherung erstellt wurde.

### <a name="can-i-back-up-multiple-disks-together"></a>Kann ich mehrere Datenträger gleichzeitig sichern?

Nein, Zeitpunktmomentaufnahmen von mehreren an eine VM angefügten Datenträgern werden nicht unterstützt. Weitere Informationen finden Sie unter [Konfigurieren der Sicherung](backup-managed-disks.md#configure-backup). Weitere Informationen zu Einschränkungen finden Sie in der [Unterstützungsmatrix](disk-backup-support-matrix.md).

### <a name="what-are-my-options-to-back-up-disks-across-multiple-subscriptions"></a>Welche Optionen gibt es für das Sichern von Datenträgern in mehreren Abonnements?

Derzeit können bei Verwendung des Azure-Portals zum Konfigurieren der Datenträgersicherung maximal 20 Datenträger aus demselben Abonnement gesichert werden.

### <a name="what-is-a-target-resource-group"></a>Was ist eine Zielressourcengruppe?

Sie können während eines Wiederherstellungsvorgangs das Abonnement und eine Ressourcengruppe, in der Sie den Datenträger wiederherstellen möchten, auswählen. Azure Backup erstellt neue Datenträger aus dem Wiederherstellungspunkt in der ausgewählten Ressourcengruppe. Diese wird als Zielressourcengruppe bezeichnet. Beachten Sie, dass die verwaltete Identität des Sicherungstresors eine Rollenzuweisung für die Zielressourcengruppe benötigt, die in der Lage ist, den Wiederherstellungsvorgang erfolgreich auszuführen. Weitere Informationen finden Sie in der [Dokumentation zur Wiederherstellung](restore-managed-disks.md).

### <a name="what-are-the-permissions-used-by-azure-backup-during-backup-and-restore-operation"></a>Welche Berechtigungen werden von Azure Backup während des Sicherungs- und Wiederherstellungsvorgangs verwendet?

Die folgenden Aktionen werden von der Rolle **Leser von Datenträgersicherungen** verwendet, die dem zu sichernden **Datenträger** zugewiesen ist:

Microsoft.Compute/disks/read

Microsoft.Compute/disks/beginGetAccess/action

Die folgenden Aktionen werden von der Rolle **Mitwirkender an Datenträgermomentaufnahmen** verwendet, die der **Momentaufnahme-Ressourcengruppe** zugewiesen ist:

Microsoft.Compute/snapshots/delete

Microsoft.Compute/snapshots/write

Microsoft.Compute/snapshots/read

Microsoft.Storage/storageAccounts/write

Microsoft.Storage/storageAccounts/read

Microsoft.Storage/storageAccounts/delete

Microsoft.Resources/subscriptions/resourceGroups/read

Microsoft.Storage/storageAccounts/listkeys/action

Microsoft.Compute/snapshots/beginGetAccess/action

Microsoft.Compute/snapshots/endGetAccess/action

Microsoft.Compute/disks/beginGetAccess/action

Die folgenden Aktionen werden von der Rolle **Operator für Datenträgerwiederherstellung** verwendet, die der **Zielressourcengruppe** zugewiesen ist:

Microsoft.Compute/disks/write

Microsoft.Compute/disks/read

Microsoft.Resources/subscriptions/resourceGroups/read

>[!NOTE]
>Die Berechtigungen für diese Rollen können sich in Zukunft ändern, wenn dem Azure Backup-Dienst weitere Funktionen hinzugefügt werden.

## <a name="next-steps"></a>Nächste Schritte

- [Supportmatrix für die Azure Disk-Sicherung](disk-backup-support-matrix.md)
