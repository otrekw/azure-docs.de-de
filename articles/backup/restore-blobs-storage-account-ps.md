---
title: Wiederherstellen von Blobs mithilfe von Azure PowerShell
description: Erfahren Sie, wie Sie Azure-Blobs mithilfe von Azure PowerShell für beliebige Zeitpunkte wiederherstellen.
ms.topic: conceptual
ms.date: 05/05/2021
ms.openlocfilehash: 653c3ce6fbfea4d890c3dc3e52fb1032a72b7eab
ms.sourcegitcommit: 2cb7772f60599e065fff13fdecd795cce6500630
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108803060"
---
# <a name="restore-azure-blobs-to-point-in-time-using-azure-powershell"></a>Durchführen von Zeitpunktwiederherstellungen für Azure-Blobs mithilfe von Azure PowerShell

In diesem Artikel wird beschrieben, wie [Blobs](blob-backup-overview.md) mithilfe von Azure PowerShell für einen beliebigen Zeitpunkt wiederhergestellt werden können.

> [!IMPORTANT]
> Unterstützung für Azure-Blobs ist ab Version Az 5.9.0 verfügbar.

> [!IMPORTANT]
> Bevor Sie mit der Wiederherstellung von Azure-Blobs mithilfe von Azure PowerShell fortfahren, machen Sie sich mit einigen [wichtigen Punkten](blob-restore.md#before-you-start) vertraut.

In diesem Artikel lernen Sie Folgendes:

- Wiederherstellen von Azure-Blobs für einen bestimmten Zeitpunkt

- Nachverfolgen des Status des Wiederherstellungsvorgangs

In den Beispielen wird auf einen vorhandenen Sicherungstresor _TestBkpVault_ unter der Ressourcengruppe _testBkpVaultRG_ verwiesen.

```azurepowershell-interactive
$TestBkpVault = Get-AzDataProtectionBackupVault -VaultName TestBkpVault -ResourceGroupName "testBkpVaultRG"
```

## <a name="restoring-azure-blobs-within-a-storage-account"></a>Wiederherstellen von Azure-Blobs in einem Speicherkonto

### <a name="fetching-the-valid-time-range-for-restore"></a>Abrufen des gültigen Zeitbereichs für die Wiederherstellung

Da die operative Sicherung für Blobs fortlaufend erfolgt, gibt es keine eindeutigen Punkte, anhand derer eine Wiederherstellung durchgeführt werden kann. Stattdessen muss der gültige Zeitbereich abgerufen werden, in dem Blobs für einen beliebigen Zeitpunkt wiederhergestellt werden können. In diesem Beispiel suchen wir nach gültigen Zeitbereichen für die Wiederherstellung innerhalb der letzten 30 Tage.

```azurepowershell-interactive
$startDate = (Get-Date).AddDays(-30)
$endDate = Get-Date
```

Rufen Sie zunächst mithilfe des Befehls [Get-AzDataProtectionBackupInstance](/powershell/module/az.dataprotection/get-azdataprotectionbackupinstance?view=azps-5.9.0&preserve-view=true) alle Instanzen ab, und identifizieren Sie die relevante Instanz.

```azurepowershell-interactive
$AllInstances = Get-AzDataProtectionBackupInstance -ResourceGroupName "testBkpVaultRG" -VaultName $TestBkpVault.Name
```

Sie können auch Az.Resourcegraph und den Befehl [Search-AzDataProtectionBackupInstanceInAzGraph](/powershell/module/az.dataprotection/search-azdataprotectionbackupinstanceinazgraph?view=azps-5.9.0&preserve-view=true) verwenden, um Instanzen in vielen Tresoren und Abonnements zu durchsuchen.

```azurepowershell-interactive
$AllInstances = Search-AzDataProtectionBackupInstanceInAzGraph -ResourceGroupName "testBkpVaultRG" -VaultName $TestBkpVault.Name -DatasourceType AzureBlob -ProtectionStatus ProtectionConfigured
```

Rufen Sie nach der Identifizierung der Instanz mit dem Befehl [Find-AzDataProtectionRestorableTimeRange](/powershell/module/az.dataprotection/find-azdataprotectionrestorabletimerange) den relevanten Wiederherstellungsbereich ab.

```azurepowershell-interactive
Find-AzDataProtectionRestorableTimeRange -ResourceGroupName "testBkpVaultRG" -VaultName $TestBkpVault.Name -BackupInstanceName $AllInstances[2].BackupInstanceName -StartTime $startDate -endTime $endDate

EndTime    : 2021-04-24T08:57:36.4149422Z
ObjectType : RestorableTimeRange
StartTime  : 2021-03-25T14:27:31.0000000Z

$DesiredPIT = (Get-Date -Date "2021-04-23T02:47:02.9500000Z")
```

### <a name="preparing-the-restore-request"></a>Die Wiederherstellungsanforderung wird vorbereitet

Sobald der Zeitpunkt für die Wiederherstellung feststeht, gibt es mehrere Optionen für die Wiederherstellung. Verwenden Sie den Befehl [Initialize-AzDataProtectionRestoreRequest](/powershell/module/az.dataprotection/initialize-azdataprotectionrestorerequest?view=azps-5.9.0&preserve-view=true), um die Wiederherstellungsanforderung mit allen relevanten Details vorzubereiten.

#### <a name="restoring-all-the-blobs-to-a-point-in-time"></a>Durchführen einer Zeitpunktwiederherstellung für alle Blobs

Bei dieser Option werden alle Blockblobs im Speicherkonto wiederhergestellt, indem ein Rollback zum ausgewählten Zeitpunkt durchgeführt wird. Bei Speicherkonten, die große Datenmengen enthalten oder eine hohe Änderungsrate aufweisen, kann der Vorgang längere Zeit in Anspruch nehmen.

```azurepowershell-interactive
$restorerequest = Initialize-AzDataProtectionRestoreRequest -DatasourceType AzureBlob -SourceDataStore OperationalStore -RestoreLocation $TestBkpVault.Location  -RestoreType OriginalLocation -PointInTime (Get-Date -Date "2021-04-23T02:47:02.9500000Z") -BackupInstance $AllInstances[2]
```

#### <a name="restoring-selected-containers"></a>Wiederherstellen ausgewählter Container

Bei dieser Option können Sie bis zu 10 Container durchsuchen und auswählen, die wiederhergestellt werden sollen.

```azurepowershell-interactive
$restorerequest = Initialize-AzDataProtectionRestoreRequest -DatasourceType AzureBlob -SourceDataStore OperationalStore -RestoreLocation $TestBkpVault.Location  -RestoreType OriginalLocation -PointInTime (Get-Date -Date "2021-04-23T02:47:02.9500000Z") -BackupInstance $AllInstances[2] -ItemLevelRecovery -ContainersList "abc","xyz"
```

#### <a name="restoring-containers-using-a-prefix-match"></a>Wiederherstellen von Containern mithilfe eines Präfixabgleichs

Bei dieser Option können Sie eine Teilmenge von Blobs über einen Präfixabgleich wiederherstellen. Sie können bis zu 10 lexikografische Bereiche von Blobs innerhalb eines einzelnen Containers oder aus mehreren Containern wiederherstellen, um diese Blobs auf einen Zustand zu einem bestimmten Zeitpunkt zurückzusetzen. Beachten Sie dabei Folgendes:

- Sie können einen Schrägstrich (/) verwenden, um den Containernamen vom Blobpräfix zu trennen.
- Der Anfang des angegebenen Bereichs wird mit eingeschlossen, das Ende des angegebenen Bereichs wird jedoch ausgeschlossen.

[Erfahren Sie mehr](blob-restore.md#use-prefix-match-for-restoring-blobs) über die Verwendung von Präfixen zum Wiederherstellen von Blobbereichen.

```azurepowershell-interactive
$restorerequest = Initialize-AzDataProtectionRestoreRequest -DatasourceType AzureBlob -SourceDataStore OperationalStore -RestoreLocation $TestBkpVault.Location  -RestoreType OriginalLocation -PointInTime (Get-Date -Date "2021-04-23T02:47:02.9500000Z") -BackupInstance $AllInstances[2] -ItemLevelRecovery -FromPrefixPattern "containerabc/aaa","containerabc/ccc" -ToPrefixPattern "containerabc/bbb","containerabc/ddd"
```

### <a name="trigger-the-restore"></a>Auslösen des Wiederherstellungsvorgangs

Verwenden Sie den Befehl [Start-AzDataProtectionBackupInstanceRestore](/powershell/module/az.dataprotection/start-azdataprotectionbackupinstancerestore?view=azps-5.9.0&preserve-view=true), um die Wiederherstellung mit der oben vorbereiteten Anforderung zu initiieren.

```azurepowershell-interactive
Start-AzDataProtectionBackupInstanceRestore -BackupInstanceName $AllInstances[2].BackupInstanceName -ResourceGroupName "testBkpVaultRG" -VaultName $TestBkpVault.Name -Parameter $restorerequest
```

## <a name="tracking-job"></a>Aufträge nachverfolgen

Verfolgen Sie alle Aufträge mithilfe des Befehls [Get-AzDataProtectionJob](/powershell/module/az.dataprotection/get-azdataprotectionjob?view=azps-5.9.0&preserve-view=true) nach. Sie können alle Aufträge auflisten und ein bestimmtes Auftragsdetail abrufen.

Sie können auch Az.ResourceGraph verwenden, um alle Aufträge für alle Sicherungstresore zu überprüfen. Verwenden Sie den Befehl [Search-AzDataProtectionJobInAzGraph](/powershell/module/az.dataprotection/search-azdataprotectionjobinazgraph?view=azps-5.9.0&preserve-view=true), um den relevanten Auftrag zu erhalten, der in jedem Sicherungstresor vorhanden sein kann.

```azurepowershell-interactive
$job = Search-AzDataProtectionJobInAzGraph -Subscription $sub -ResourceGroupName "testBkpVaultRG" -Vault $TestBkpVault.Name -DatasourceType AzureBlob -Operation Restore
```

## <a name="next-steps"></a>Nächste Schritte

[Übersicht über die Sicherung von Azure-Blobs](blob-backup-overview.md)
