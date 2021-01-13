---
title: Vorläufiges Löschen für virtuelle Computer
description: Erfahren Sie, wie das vorläufige Löschen für virtuelle Computer Sicherungen sicherer macht.
ms.topic: conceptual
ms.date: 04/30/2020
ms.custom: references_regions
ms.openlocfilehash: a8b70d4c8240d096c19e5a8d7449921557b8896c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89022240"
---
# <a name="soft-delete-for-virtual-machines"></a>Vorläufiges Löschen für virtuelle Computer

Vorläufiges Löschen für VMs schützt die Sicherungen Ihrer VMs vor unbeabsichtigtem Löschen. Selbst nachdem die Sicherungen gelöscht wurden, bleiben sie für weitere 14 Tage im Zustand „vorläufig gelöscht“ erhalten.

> [!NOTE]
> Mit dem vorläufigen Löschen werden nur gelöschte Sicherungsdaten geschützt. Wenn eine VM ohne Sicherung gelöscht wird, bleiben die Daten beim Feature „Vorläufiges Löschen“ nicht erhalten. Alle Ressourcen sollten per Azure Backup geschützt werden, um die vollständige Resilienz sicherzustellen.
>

## <a name="supported-regions"></a>Unterstützte Regionen

Vorläufiges Löschen wird derzeit in den folgenden Regionen unterstützt: „USA, Westen-Mitte“, „Asien, Osten“, „Kanada, Mitte“, „Kanada, Osten“, „Frankreich, Mitte“, „Frankreich, Süden“, „Südkorea, Mitte“, „Südkorea, Süden“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „Australien, Osten“, „Australien, Südosten“, „Europa, Norden“, „USA, Westen“, „USA, Westen 2“, „USA, Mitte“, „Asien, Südosten“, „USA, Norden-Mitte“, „USA, Süden-Mitte“, „Japan, Osten“, „Japan, Westen“, „Indien, Süden“, „Indien, Mitte“, „Indien, Westen“, „USA, Osten 2“, „Schweiz, Norden“, „Schweiz, Westen“, „Norwegen, Westen“, „Norwegen, Osten“ sowie in allen nationalen Regionen.

## <a name="soft-delete-for-vms-using-azure-portal"></a>Vorläufiges Löschen für VMs über das Azure-Portal

1. Der Sicherungsvorgang muss beendet werden, um die Sicherungsdaten einer VM zu löschen. Navigieren Sie im Azure-Portal zu Ihrem Recovery Services-Tresor, klicken Sie mit der rechten Maustaste auf das Sicherungselement, und wählen Sie **Sicherung beenden** aus.

   ![Screenshot: Sicherungselemente im Azure-Portal](./media/backup-azure-security-feature-cloud/backup-stopped.png)

2. Im folgenden Fenster erhalten Sie die Möglichkeit, die Sicherungsdaten zu löschen oder zu speichern. Wenn Sie die Option **Sicherungsdaten löschen** und dann **Sicherung beenden** wählen, wird die VM-Sicherung nicht endgültig gelöscht. Stattdessen werden die Sicherungsdaten 14 Tage lang im Zustand „Vorläufig gelöscht“ aufbewahrt. Wenn Sie die Option **Sicherungsdaten löschen** auswählen, wird eine E-Mail-Benachrichtigung an die konfigurierte E-Mail-ID gesendet. Hiermit wird der Benutzer darüber informiert, dass die Aufbewahrungsdauer für die Sicherungsdaten um 14 Tage verlängert wurde. Darüber hinaus wird am 12. Tag eine E-Mail-Benachrichtigung mit dem Hinweis gesendet, dass zwei weitere Tage verbleiben, um die gelöschten Daten wiederherzustellen. Das endgültige Löschen wird erst am 15. Tag durchgeführt, und es wird eine abschließende E-Mail-Benachrichtigung gesendet, um den Benutzer über die endgültige Löschung der Daten zu informieren.

   ![Screenshot: „Sicherung beenden“ im Azure-Portal](./media/backup-azure-security-feature-cloud/delete-backup-data.png)

3. Während dieser 14 Tage wird die vorläufig gelöschte VM im Recovery Services-Tresor mit dem roten Symbol für „Vorläufig gelöscht“ daneben angezeigt.

   ![Screenshot: Vorläufig gelöschte VM im Azure-Portal](./media/backup-azure-security-feature-cloud/vm-soft-delete.png)

   > [!NOTE]
   > Wenn der Tresor vorläufig gelöschte Sicherungselemente enthält, kann er zu diesem Zeitpunkt nicht gelöscht werden. Versuchen Sie den Tresor erst zu löschen, nachdem die Sicherungselemente endgültig gelöscht wurden und der Tresor kein Element mit dem Status „Vorläufig gelöscht“ mehr enthält.

4. Das Löschen muss zuerst wieder rückgängig gemacht werden, um die vorläufig gelöschte VM wiederherstellen zu können. Wählen Sie zum Rückgängigmachen des Löschvorgangs den vorläufig gelöschten virtuellen Computer und anschließend **Wiederherstellen** aus.

   ![Screenshot: Wiederherstellen der VM im Azure-Portal](./media/backup-azure-security-feature-cloud/choose-undelete.png)

   Es wird ein Fenster mit einer Warnung angezeigt, dass bei Auswahl der Option „Wiederherstellen“ alle Wiederherstellungspunkte für die VM wiederhergestellt werden und für die Durchführung eines Wiederherstellungsvorgangs verfügbar sind. Für die VM gilt der Status „Beendigung des Schutzes mit Beibehaltung der Daten“. Die Sicherungen sind angehalten, und die Sicherungsdaten werden ohne zeitliche Beschränkung aufbewahrt, ohne dass eine Sicherungsrichtlinie gilt.

   ![Screenshot: Bestätigung der VM-Wiederherstellung im Azure-Portal](./media/backup-azure-security-feature-cloud/undelete-vm.png)

   An diesem Punkt können Sie die VM auch wiederherstellen, indem Sie für den ausgewählten Wiederherstellungspunkt die Option **Virtuellen Computer wiederherstellen** auswählen.  

   ![Screenshot: Option „Virtuellen Computer wiederherstellen“ im Azure-Portal](./media/backup-azure-security-feature-cloud/restore-vm.png)

   > [!NOTE]
   > Der Garbage Collector zum Bereinigen abgelaufener Wiederherstellungspunkte wird erst ausgeführt, nachdem der Benutzer den Vorgang **Sicherung fortsetzen** durchgeführt hat.

5. Nachdem der Wiederherstellungsprozess abgeschlossen ist, wird der Status auf „Sicherung beenden mit Daten beibehalten“ zurückgesetzt, und anschließend können Sie die Option **Sicherung fortsetzen** auswählen. Mit **Sicherung fortsetzen** wird das Sicherungselement wieder in den aktiven Zustand versetzt, und es wird eine vom Benutzer ausgewählte Sicherungsrichtlinie zugeordnet, mit der die Sicherungs- und Aufbewahrungszeitpläne definiert werden.

   ![Screenshot: Option „Sicherung fortsetzen“ im Azure-Portal](./media/backup-azure-security-feature-cloud/resume-backup.png)

## <a name="soft-delete-for-vms-using-azure-powershell"></a>Vorläufiges Löschen für VMs über Azure PowerShell

> [!IMPORTANT]
> Die erforderliche Mindestversion von Az.RecoveryServices für die Verwendung des vorläufigen Löschens mit Azure PowerShell ist 2.2.0. Verwenden Sie ```Install-Module -Name Az.RecoveryServices -Force```, um die aktuelle Version herunterzuladen.

Wie oben für das Azure-Portal erläutert, ist die Abfolge der Schritte bei der Verwendung von Azure PowerShell identisch.

### <a name="delete-the-backup-item-using-azure-powershell"></a>Löschen des Sicherungselements mit Azure PowerShell

Löschen Sie das Sicherungselement mithilfe des PowerShell-Cmdlets [Disable-AzRecoveryServicesBackupProtection](/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupprotection).

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $myBkpItem -RemoveRecoveryPoints -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           DeleteBackupData     Completed            12/5/2019 12:44:15 PM     12/5/2019 12:44:50 PM     0488c3c2-accc-4a91-a1e0-fba09a67d2fb
```

Der DeleteState des Sicherungselements ändert sich von „NotDeleted“ in „ToBeDeleted“. Die Sicherungsdaten werden 14 Tage lang aufbewahrt. Wenn Sie den Löschvorgang rückgängig machen möchten, sollten Sie den Vorgang zum Rückgängigmachen des Löschens ausführen.

### <a name="undoing-the-deletion-operation-using-azure-powershell"></a>Rückgängigmachen des Löschvorgangs mithilfe von Azure PowerShell

Rufen Sie zuerst das relevante Sicherungselement ab, das sich im Zustand für vorläufiges Löschen befindet (d. h., dass das Löschen dieses Elements bevorsteht).

```powershell

Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID | Where-Object {$_.DeleteState -eq "ToBeDeleted"}

Name                                     ContainerType        ContainerUniqueName                      WorkloadType         ProtectionStatus     HealthStatus         DeleteState
----                                     -------------        -------------------                      ------------         ----------------     ------------         -----------
VM;iaasvmcontainerv2;selfhostrg;AppVM1    AzureVM             iaasvmcontainerv2;selfhostrg;AppVM1       AzureVM              Healthy              Passed               ToBeDeleted

$myBkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID -Name AppVM1
```

Führen Sie dann den Vorgang zum Rückgängigmachen des Löschens mithilfe des PowerShell-Cmdlets [Undo-AzRecoveryServicesBackupItemDeletion](/powershell/module/az.recoveryservices/undo-azrecoveryservicesbackupitemdeletion) aus.

```powershell
Undo-AzRecoveryServicesBackupItemDeletion -Item $myBKpItem -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           Undelete             Completed            12/5/2019 12:47:28 PM     12/5/2019 12:47:40 PM     65311982-3755-46b5-8e53-c82ea4f0d2a2
```

Der DeleteState des Sicherungselements wird auf „NotDeleted“ zurückgesetzt. Der Schutz bleibt jedoch weiterhin unterbrochen. [Setzen Sie die Sicherung fort](./backup-azure-vms-automation.md#change-policy-for-backup-items), um den Schutz erneut zu aktivieren.

## <a name="soft-delete-for-vms-using-rest-api"></a>Vorläufiges Löschen für VMs über die REST-API

- Löschen Sie die Sicherungen mithilfe der REST-API, wie [hier](backup-azure-arm-userestapi-backupazurevms.md#stop-protection-and-delete-data) erläutert.
- Wenn Sie diese Löschvorgänge rückgängig machen möchten, finden Sie [hier](backup-azure-arm-userestapi-backupazurevms.md#undo-the-deletion) die entsprechenden Schritte.

## <a name="how-to-disable-soft-delete"></a>Deaktivieren des vorläufigen Löschens

Es wird davon abgeraten, dieses Feature zu deaktivieren. Sie sollten das vorläufige Löschen nur dann deaktivieren, wenn Sie planen, Ihre geschützten Elemente in einen neuen Tresor zu verschieben, und nicht die 14 Tage warten können, die für das Löschen und erneute Schützen erforderlich sind (z. B. in einer Testumgebung). Anweisungen zum Deaktivieren des vorläufigem Löschens finden Sie unter [Aktivieren und Deaktivieren des vorläufigen Löschens](backup-azure-security-feature-cloud.md#enabling-and-disabling-soft-delete).

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die [häufig gestellten Fragen](backup-azure-security-feature-cloud.md#frequently-asked-questions) zum vorläufigen Löschen.
- Sehen Sie sich die [Übersicht über Sicherheitsfeatures in Azure Backup](security-overview.md) an.
