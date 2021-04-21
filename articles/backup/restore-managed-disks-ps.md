---
title: Wiederherstellen von Azure-Managed Disks über Azure PowerShell
description: Erfahren Sie, wie Sie Azure Managed Disks über das Azure-Portal wiederherstellen können.
ms.topic: conceptual
ms.date: 03/26/2021
ms.openlocfilehash: c6625b43c313d45d4b295dd406e29a2b1d85b387
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2021
ms.locfileid: "107520036"
---
# <a name="restore-azure-managed-disks-using-azure-powershell"></a>Wiederherstellen von Azure-Managed Disks über Azure PowerShell

In diesem Artikel wird erklärt, wie Sie [Azure Managed Disks](../virtual-machines/managed-disks-overview.md) aus einem von Azure Backup erstellten Wiederherstellungspunkt wiederherstellen.

Derzeit wird die Wiederherstellungsoption „Wiederherstellung am ursprünglichen Speicherort“, bei der die Wiederherstellung die vorhandene Instanz auf dem Quelldatenträger, von dem die Sicherungen erstellt wurden, ersetzt, nicht unterstützt. Sie können eine Wiederherstellung aus einem Wiederherstellungspunkt vornehmen, um einen neuen Datenträger entweder in derselben Ressourcengruppe wie der Quelldatenträger, von dem die Sicherungen erstellt wurden, oder in einer beliebigen anderen Ressourcengruppe zu erstellen. Dies wird als „Wiederherstellung an einem anderen Speicherort“ bezeichnet und hilft, sowohl den Quelldatenträger als auch den wiederhergestellten (neuen) Datenträger zu behalten.

In diesem Artikel lernen Sie Folgendes:

- Wiederherstellen zum Erstellen eines neuen Datenträgers

- Nachverfolgen des Status des Wiederherstellungsvorgangs

In den Beispielen wird auf einen vorhandenen Sicherungstresor „TestBkpVault“ unter der Ressourcengruppe „TestBkpVaultRG“ verwiesen.

```azurepowershell-interactive
$TestBkpVault = Get-AzDataProtectionBackupVault -VaultName TestBkpVault -ResourceGroupName "testBkpVaultRG"
```

## <a name="restore-to-create-a-new-disk"></a>Wiederherstellen zum Erstellen eines neuen Datenträgers

### <a name="setting-up-permissions"></a>Einrichten von Berechtigungen

Der Sicherungstresor verwendet die verwaltete Identität für den Zugriff auf andere Azure-Ressourcen. Um die Wiederherstellung aus einer Sicherung vornehmen, erfordert die verwaltete Identität des Sicherungstresors einen Satz von Berechtigungen für die Ressourcengruppe, in der der Datenträger wiederhergestellt werden soll.

Der Sicherungstresor verwendet eine systemseitig zugewiesene verwaltete Identität, die auf eine Ressource beschränkt und an den Lebenszyklus dieser Ressource gebunden ist. Sie können der verwalteten Identität mithilfe der rollenbasierten Zugriffssteuerung von Azure (Azure RBAC) Berechtigungen erteilen. Eine verwaltete Identität ist ein spezieller Dienstprinzipal, der nur zusammen mit Azure-Ressourcen verwendet werden kann. Informieren Sie sich ausführlicher über [verwaltete Identitäten](../active-directory/managed-identities-azure-resources/overview.md).

Weisen Sie die relevanten Berechtigungen für die vom System zugewiesene verwaltete Identität des Tresors der Zielressourcengruppe zu, in der die Datenträger wieder hergestellt/erstellt werden, wie [hier](restore-managed-disks.md#restore-to-create-a-new-disk)erwähnt.

### <a name="fetching-the-relevant-recovery-point"></a>Der relevante Wiederherstellungspunkt wird abgerufen.

Rufen Sie mithilfe des Befehls [Get-AzDataProtectionBackupInstance](/powershell/module/az.dataprotection/get-azdataprotectionbackupinstance?view=azps-5.7.0&preserve-view=true) alle Instanzen ab, und identifizieren Sie die relevante Instanz.

```azurepowershell-interactive
$AllInstances = Get-AzDataProtectionBackupInstance -ResourceGroupName "testBkpVaultRG" -VaultName $TestBkpVault.Name
```

Sie können auch **Az.Resourcegraph** und den Befehl [Search-AzDataProtectionBackupInstanceInAzGraph](/powershell/module/az.dataprotection/search-azdataprotectionbackupinstanceinazgraph?view=azps-5.7.0&preserve-view=true) verwenden, um Instanzen in vielen Tresoren und Abonnements zu durchsuchen.

```azurepowershell-interactive
$AllInstances = Search-AzDataProtectionBackupInstanceInAzGraph -ResourceGroupName "testBkpVaultRG" -VaultName $TestBkpVault.Name -DatasourceType AzureDisk -ProtectionStatus ProtectionConfigured
```

Rufen Sie den entsprechenden Wiederherstellungspunkt ab, sobald die Instanz identifiziert wurde.

```azurepowershell-interactive
$rp = Get-AzDataProtectionRecoveryPoint -ResourceGroupName "testBkpVaultRG" -VaultName $TestBkpVault.Name -BackupInstanceName $AllInstances[2].BackupInstanceName
```

### <a name="preparing-the-restore-request"></a>Die Wiederherstellungsanforderung wird vorbereitet

Erstellen Sie die ARM-ID des neuen Datenträgers, der mit der Zielressourcengruppe erstellt werden soll, dem die Berechtigungen wie [oben](#setting-up-permissions)beschrieben zugewiesen wurden, und den erforderlichen Datenträgernamen. Ein Datenträger kann beispielsweise unter einer Ressourcengruppe **targetrg** einen anderen Abonnement den Namen **PSTestDisk2** haben.

```azurepowershell-interactive
$targetDiskId = /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx/resourceGroups/targetrg/providers/Microsoft.Compute/disks/PSTestDisk2
```

Verwenden Sie den Befehl [Initialize-AzDataProtectionRestoreRequest](/powershell/module/az.dataprotection/initialize-azdataprotectionrestorerequest?view=azps-5.7.0&preserve-view=true), um die Wiederherstellungsanforderung mit allen relevanten Details vorzubereiten.

```azurepowershell-interactive
$restorerequest = Initialize-AzDataProtectionRestoreRequest -DatasourceType AzureDisk -SourceDataStore OperationalStore -RestoreLocation $TestBkpVault.Location  -RestoreType AlternateLocation -TargetResourceId $targetDiskId -RecoveryPoint $rp[0].Name
```

### <a name="trigger-the-restore"></a>Auslösen des Wiederherstellungsvorgangs

Verwenden Sie den Befehl [Start-AzDataProtectionBackupInstanceRestore](/powershell/module/az.dataprotection/start-azdataprotectionbackupinstancerestore?view=azps-5.7.0&preserve-view=true), um die Wiederherstellung mit der oben vorbereiteten Anforderung zu initiieren.

```azurepowershell-interactive
Start-AzDataProtectionBackupInstanceRestore -BackupInstanceName $AllInstances[2].BackupInstanceName -ResourceGroupName "testBkpVaultRG" -VaultName $TestBkpVault.Name -Parameter $restorerequest
```

## <a name="tracking-job"></a>Aufträge nachverfolgen

Verfolgen Sie alle Aufträge mithilfe des Befehls [Get-AzDataProtectionJob](/powershell/module/az.dataprotection/get-azdataprotectionjob?view=azps-5.7.0&preserve-view=true). Sie können alle Aufträge auflisten und ein bestimmtes Auftragsdetail abrufen.

Sie können auch **Az.ResourceGraph** verwenden, um alle Aufträge für alle Sicherungstresore zu überprüfen. Verwenden Sie den Befehl [Search-AzDataProtectionJobInAzGraph](/powershell/module/az.dataprotection/search-azdataprotectionjobinazgraph?view=azps-5.7.0&preserve-view=true), um den relevanten Auftrag zu erhalten, der in jedem Sicherungstresor vorhanden sein kann.

```azurepowershell-interactive
$job = Search-AzDataProtectionJobInAzGraph -Subscription $sub -ResourceGroupName "testBkpVaultRG" -Vault $TestBkpVault.Name -DatasourceType AzureDisk -Operation OnDemandBackup
```

## <a name="next-steps"></a>Nächste Schritte

- [Häufig gestellte Fragen zu Azure Disk Backup](disk-backup-faq.yml)
