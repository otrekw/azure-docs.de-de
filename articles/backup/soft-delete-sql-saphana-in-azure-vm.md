---
title: Vorläufiges Löschen für SQL-Server auf virtuellen Azure-Computern und SAP HANA in Azure-VM-Workloads
description: Erfahren Sie, wie das vorläufige Löschen für SQL-Server auf virtuellen Azure-Computern und SAP HANA in Azure-VM-Workloads Sicherungen noch sicherer macht.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 2a442997d426ff0bf4c74b0b45f7657cc0593b82
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91254294"
---
# <a name="soft-delete-for-sql-server-in-azure-vm-and-sap-hana-in-azure-vm-workloads"></a>Vorläufiges Löschen für SQL-Server auf virtuellen Azure-Computern und SAP HANA in Azure-VM-Workloads

Azure Backup bietet nun vorläufiges Löschen für SQL-Server auf virtuellen Azure-Computern und SAP HANA in Azure-VM-Workloads. Diese Funktion ergänzt das bereits unterstützte [Szenario für vorläufiges Löschen virtueller Azure-Computer](soft-delete-virtual-machines.md).

[Vorläufiges Löschen](backup-azure-security-feature-cloud.md) ist ein Sicherheitsfeature für den Schutz von Sicherungsdaten auch nach dem Löschen. Mit dem vorläufigen Löschen werden selbst beim Löschen der Sicherung einer Datenbank durch einen böswilligen Akteur oder beim versehentlichen Löschen der Sicherungsdaten diese noch 14 Tage lang aufbewahrt. Dies ermöglicht die Wiederherstellung dieses Sicherungselements ohne Datenverluste. Für die zusätzlichen 14 Tage der Aufbewahrung von Sicherungsdaten mit dem Status „Vorläufiges Löschen“ fallen für Kunden keine Kosten an.

## <a name="soft-delete-for-sql-server-in-azure-vm-using-azure-portal"></a>Vorläufiges Löschen für SQL-Server auf einer Azure-VM über das Azure-Portal

>[!NOTE]
>Diese Anweisungen gelten auch für SAP HANA auf Azure-VMs.

1. Der Sicherungsvorgang muss zunächst beendet werden, damit die Sicherungsdaten einer Datenbank auf einem SQL-Server gelöscht werden können. Navigieren Sie im Azure-Portal zu Ihrem Recovery Services-Tresor und dann zu dem Sicherungselement, und wählen Sie **Sicherung beenden** aus.

   ![Sicherung beenden](./media/soft-delete-sql-saphana-in-azure-vm/stop-backup.png)

2. Im folgenden Fenster erhalten Sie die Möglichkeit, die Sicherungsdaten zu löschen oder zu speichern. Wenn Sie **Sicherungsdaten löschen** auswählen, wird die Datenbanksicherung nicht endgültig gelöscht. Stattdessen werden die Sicherungsdaten 14 Tage lang im Zustand „Vorläufig gelöscht“ aufbewahrt. Der Löschvorgang wird bis zum 15. Tag zurückgestellt. Die Benutzer erhalten reguläre Warnungs-E-Mails am 1., 12. und 15. Tag, die über den Sicherungszustand der Datenbank informieren.

   ![Löschen von Sicherungsdaten](./media/soft-delete-sql-saphana-in-azure-vm/delete-backup-data.png)

3. Während dieser 14 Tage wird das vorläufig gelöschte Element im Recovery Services-Tresor mit einem roten Symbol gekennzeichnet, mit dem der Status „Vorläufig gelöscht“ angezeigt wird.

   ![Vorläufig gelöschte Elemente](./media/soft-delete-sql-saphana-in-azure-vm/soft-deleted-items.png)

4. Das Löschen muss zunächst wieder rückgängig gemacht werden, um die vorläufig gelöschte Datenbank wiederherstellen zu können. Wählen Sie zum Rückgängigmachen des Löschvorgangs die vorläufig gelöschte Datenbank und anschließend **Wiederherstellen** aus.

   ![Rückgängigmachen des Löschvorgangs einer Datenbank](./media/soft-delete-sql-saphana-in-azure-vm/undelete-database.png)

   Es wird ein Fenster mit einer Warnung angezeigt, dass bei Auswahl der Option „Wiederherstellen“ alle Wiederherstellungspunkte für die Datenbank wiederhergestellt werden und für die Durchführung eines Wiederherstellungsvorgangs verfügbar sind. Für das Sicherungselement gilt der Status „Beendigung des Schutzes mit Beibehaltung der Daten“. Die Sicherungen sind angehalten, und die Sicherungsdaten werden ohne zeitliche Beschränkung aufbewahrt, ohne dass eine Sicherungsrichtlinie gilt.

   ![Warnung zum Rückgängigmachen des Löschvorgangs](./media/soft-delete-sql-saphana-in-azure-vm/undelete-warning.png)

5. An diesem Punkt können Sie die Daten auch wiederherstellen, indem Sie für das ausgewählte vorläufig gelöschte Sicherungselement die Option **Wiederherstellen** auswählen.

   ![Wiederherstellen eines virtuellen Computers](./media/soft-delete-sql-saphana-in-azure-vm/restore-vm.png)

6. Nachdem der Wiederherstellungsprozess abgeschlossen ist, wird der Status auf „Sicherung beenden mit Daten beibehalten“ zurückgesetzt, und anschließend können Sie die Option **Sicherung fortsetzen** auswählen. Mit **Sicherung fortsetzen** wird das Sicherungselement wieder in den aktiven Zustand versetzt, und es wird eine vom Benutzer ausgewählte Sicherungsrichtlinie zugeordnet, mit der die Sicherungs- und Aufbewahrungszeitpläne definiert werden.

   ![Sicherung fortsetzen](./media/soft-delete-sql-saphana-in-azure-vm/resume-backup.png)

## <a name="soft-delete-for-sql-server-in-vm-using-azure-powershell"></a>Vorläufiges Löschen für SQL-Server auf einer VM mit Azure PowerShell

>[!NOTE]
>Die erforderliche Mindestversion von Az.RecoveryServices für die Verwendung des vorläufigen Löschens mit Azure PowerShell ist 2.2.0. Verwenden Sie `Install-Module -Name Az.RecoveryServices -Force`, um die aktuelle Version herunterzuladen.

Die Abfolge der Schritte bei Verwendung von Azure PowerShell ist mit der oben für das Azure-Portal erläuterten identisch.

### <a name="delete-the-backup-item-using-azure-powershell"></a>Löschen des Sicherungselements mit Azure PowerShell

Löschen Sie das Sicherungselement mithilfe des PowerShell-Cmdlets [Disable-AzRecoveryServicesBackupProtection](/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupprotection).

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $myBkpItem -RemoveRecoveryPoints -VaultId $myVaultID -Force
```

Der **DeleteState** des Sicherungselements ändert sich von **NotDeleted** in **ToBeDeleted**. Die Sicherungsdaten werden 14 Tage lang aufbewahrt. Wenn Sie den Löschvorgang rückgängig machen möchten, sollten Sie den Vorgang zum Rückgängigmachen des Löschens ausführen.

### <a name="undoing-the-deletion-operation-using-azure-powershell"></a>Rückgängigmachen des Löschvorgangs mithilfe von Azure PowerShell

Rufen Sie zuerst das relevante Sicherungselement ab, das sich im Zustand für vorläufiges Löschen befindet (d. h., dass das Löschen dieses Elements bevorsteht).

```powershell
Get-AzRecoveryServicesBackupItem -BackupManagementType AzureWorkload -WorkloadType SQLDataBase -VaultId $myVaultID | Where-Object {$_.DeleteState -eq "ToBeDeleted"}

$myBkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureWorkload -WorkloadType SQLDataBase -VaultId $myVaultID -Name AppVM1
```

Führen Sie dann den Vorgang zum Rückgängigmachen des Löschens mithilfe des PowerShell-Cmdlets [Undo-AzRecoveryServicesBackupItemDeletion](/powershell/module/az.recoveryservices/undo-azrecoveryservicesbackupitemdeletion) aus.

```powershell
Undo-AzRecoveryServicesBackupItemDeletion -Item $myBKpItem -VaultId $myVaultID -Force
```

Der **DeleteState** des Sicherungselements wird auf **NotDeleted** zurückgesetzt. Der Schutz bleibt jedoch weiterhin unterbrochen. Setzen Sie die Sicherung fort, um den Schutz erneut zu aktivieren.

## <a name="how-to-disable-soft-delete"></a>Deaktivieren des vorläufigen Löschens

Es wird davon abgeraten, dieses Feature zu deaktivieren. Sie sollten das vorläufige Löschen nur dann deaktivieren, wenn Sie planen, Ihre geschützten Elemente in einen neuen Tresor zu verschieben, und nicht die 14 Tage warten können, die für das Löschen und erneute Schützen erforderlich sind (z. B. in einer Testumgebung). Anweisungen zum Deaktivieren des vorläufigem Löschens finden Sie unter [Aktivieren und Deaktivieren des vorläufigen Löschens](backup-azure-security-feature-cloud.md#enabling-and-disabling-soft-delete).

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die [häufig gestellten Fragen](backup-azure-security-feature-cloud.md#frequently-asked-questions) zum vorläufigen Löschen.
- Sehen Sie sich die [Übersicht über Sicherheitsfeatures in Azure Backup](security-overview.md) an.
