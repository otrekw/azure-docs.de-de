---
title: Verwalten der Sicherungen von Azure-Dateifreigaben mit PowerShell
description: Erfahren Sie, wie Sie über PowerShell Azure-Dateifreigaben, die durch den Azure Backup-Dienst gesichert wurden, verwalten und überwachen.
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 6ee5fb92e4a66a9d6db66514f966c3650d3a4f13
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83201973"
---
# <a name="manage-azure-file-share-backups-with-powershell"></a>Verwalten der Sicherungen von Azure-Dateifreigaben mit PowerShell

In diesem Artikel wird beschrieben, wie Sie über PowerShell Azure-Dateifreigaben, die durch den Azure Backup-Dienst gesichert wurden, verwalten und überwachen.

> [!WARNING]
> Stellen Sie für AFS-Sicherungen sicher, dass die PS-Version auf die Mindestversion für Az.RecoveryServices 2.6.0 aktualisiert wird. Weitere Informationen finden Sie im [Abschnitt](backup-azure-afs-automation.md#important-notice-backup-item-identification) zur Anforderung für diese Änderung.

## <a name="modify-the-protection-policy"></a>Ändern der Schutzrichtlinie

Verwenden Sie [Enable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0), um die Sicherungsrichtlinie für die Azure-Dateifreigabe anzupassen. Geben Sie das relevante Sicherungselement und die neue Sicherungsrichtlinie an.

Im folgenden Beispiel wird die Schutzrichtlinie von **testAzureFS** von **dailyafs** in **monthlyafs** geändert.

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="track-backup-and-restore-jobs"></a>Nachverfolgen von Sicherungs- und Wiederherstellungsaufträgen

Bei bedarfsgesteuerten Sicherungs- und Wiederherstellungsvorgängen wird ein Auftrag zusammen mit einer ID wie [oben](backup-azure-afs-automation.md#trigger-an-on-demand-backup) gezeigt zurückgegeben. Verwenden Sie das Cmdlet [Get-AzRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob?view=azps-1.4.0), um den Fortschritt des Auftrags nachzuverfolgen und Details abzurufen.

```powershell
$job = Get-AzRecoveryServicesBackupJob -JobId 00000000-6c46-496e-980a-3740ccb2ad75 -VaultId $vaultID

 $job | fl


IsCancellable        : False
IsRetriable          : False
ErrorDetails         : {Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureFileShareJobErrorInfo}
ActivityId           : 00000000-5b71-4d73-9465-8a4a91f13a36
JobId                : 00000000-6c46-496e-980a-3740ccb2ad75
Operation            : Restore
Status               : Failed
WorkloadName         : testAFS
StartTime            : 12/10/2018 9:56:38 AM
EndTime              : 12/10/2018 11:03:03 AM
Duration             : 01:06:24.4660027
BackupManagementType : AzureStorage

$job.ErrorDetails

 ErrorCode ErrorMessage                                          Recommendations
 --------- ------------                                          ---------------
1073871825 Microsoft Azure Backup encountered an internal error. Wait for a few minutes and then try the operation again. If the issue persists, please contact Microsoft support.
```

## <a name="stop-protection-on-a-file-share"></a>Beenden des Schutzes für eine Dateifreigabe

Der Schutz für Azure-Dateifreigaben kann auf zwei Arten beendet werden:

* Beenden aller zukünftigen Sicherungsaufträge und *Löschen* aller Wiederherstellungspunkte
* Beenden aller zukünftigen Sicherungsaufträge unter *Beibehaltung* der Wiederherstellungspunkte

Unter Umständen fallen für die Aufbewahrung der Wiederherstellungspunkte im Speicher Gebühren an, da die von Azure Backup erstellten zugrundeliegenden Momentaufnahmen beibehalten werden. Der Vorteil der Beibehaltung von Wiederherstellungspunkten ist, dass Sie die Dateifreigabe bei Bedarf später wiederherstellen können. Informationen zu den Kosten, die durch die Beibehaltung der Wiederherstellungspunkte entstehen, finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/storage/files/). Wenn Sie alle Wiederherstellungspunkte löschen, kann die Dateifreigabe nicht mehr wiederhergestellt werden.

## <a name="stop-protection-and-retain-recovery-points"></a>Beenden des Schutzes und Beibehalten der Wiederherstellungspunkte

Wenn Sie den Schutz beenden, aber die Daten beibehalten möchten, können Sie das Cmdlet [Disable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupprotection?view=azps-3.3.0) verwenden.

Im folgenden Beispiel wird der Schutz für die Dateifreigabe *afsfileshare* beendet, aber alle Wiederherstellungspunkte werden beibehalten:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "afstesting" -Name "afstest" | select -ExpandProperty ID
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureStorage -WorkloadType AzureFiles -Name "afsfileshare" -VaultId $vaultID
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $vaultID
```

```output
WorkloadName     Operation         Status         StartTime                 EndTime                   JobID
------------     ---------         ------         ---------                 -------                   -----
afsfileshare     DisableBackup     Completed      1/26/2020 2:43:59 PM      1/26/2020 2:44:21 PM      98d9f8a1-54f2-4d85-8433-c32eafbd793f
```

Das Auftrags-ID-Attribut in der Ausgabe entspricht der ID des Auftrags, der vom Sicherungsdienst für Ihren Vorgang zum Beenden des Schutzes erstellt wird. Mit dem Cmdlet [Get-AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob?view=azps-3.3.0) können Sie den Status eines Auftrags nachverfolgen.

## <a name="stop-protection-without-retaining-recovery-points"></a>Beenden des Schutzes ohne Beibehaltung von Wiederherstellungspunkten

Um den Schutz ohne Beibehaltung der Wiederherstellungspunkte zu beenden, verwenden Sie das Cmdlet [Disable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupprotection?view=azps-3.3.0) und fügen den Parameter **-RemoveRecoveryPoints** hinzu.

Im folgenden Beispiel wird der Schutz für die Dateifreigabe *afsfileshare* beendet, ohne Wiederherstellungspunkte beizubehalten:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "afstesting" -Name "afstest" | select -ExpandProperty ID
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureStorage -WorkloadType AzureFiles -Name "afsfileshare" -VaultId $vaultID
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $vaultID -RemoveRecoveryPoints
```

```output
WorkloadName     Operation            Status         StartTime                 EndTime                   JobID
------------     ---------            ------         ---------                 -------                   -----
afsfileshare     DeleteBackupData     Completed      1/26/2020 2:50:57 PM      1/26/2020 2:51:39 PM      b1a61c0b-548a-4687-9d15-9db1cc5bcc85
```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über die [Verwaltung von Sicherungen von Azure-Dateifreigaben](manage-afs-backup.md) im Azure-Portal.
