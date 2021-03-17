---
title: Unterstützung der Zugriffsebene „Archiv“ (Vorschau)
description: Informationen zur Unterstützung der Zugriffsebene „Archiv“ für Azure Backup
ms.topic: conceptual
ms.date: 02/18/2021
ms.openlocfilehash: 6c597d640f24dc4c680bfd5db16f9df09017ee54
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102609851"
---
# <a name="archive-tier-support-preview"></a>Unterstützung der Zugriffsebene „Archiv“ (Vorschau)

Kunden setzen zum Speichern von Sicherungsdaten, darunter auch Sicherungsdaten für die langfristige Aufbewahrung, auf Azure Backup. Die Anforderungen an die Aufbewahrungsdauer sind dabei in den Konformitätsregeln des jeweiligen Unternehmens definiert. In den meisten Fällen erfolgt nur selten ein Zugriff auf die älteren Sicherungsdaten, die ausschließlich zu Konformitätszwecken gespeichert werden.

Azure Backup unterstützt neben Momentaufnahmen und der Standardzugriffsebene auch die Sicherung von Wiederherstellungspunkten für die langfristige Aufbewahrung auf der Zugriffsebene „Archiv“.

## <a name="scope-for-preview"></a>Umfang der Vorschau

Unterstützte Workloads:

- Virtuelle Azure-Computer
  - Nur monatliche und jährliche Wiederherstellungspunkte. Tägliche und wöchentliche Wiederherstellungspunkte werden nicht unterstützt.
  - Alter: mindestens drei (3) Monate auf der Tresorstandardebene
  - Restliche Aufbewahrungsdauer: mindestens sechs (6) Monate
  - Keine aktiven täglichen und wöchentlichen Abhängigkeiten
- SQL Server auf virtuellen Azure-Computern
  - Nur vollständige Wiederherstellungspunkte. Protokolle und differenzielle Sicherungen werden nicht unterstützt.
  - Alter: mindestens 45 Tage auf der Tresorstandardebene
  - Restliche Aufbewahrungsdauer: mindestens sechs (6) Monate
  - Keine Abhängigkeiten

Unterstützte Clients:

- Die Funktion wird mithilfe von PowerShell bereitgestellt.

>[!NOTE]
>Die Unterstützung der Zugriffsebene „Archiv“ für Azure VMs und SQL Server-Instanzen auf Azure-VMs befindet sich in einer eingeschränkten öffentlichen Vorschau mit begrenzten Anmeldungen. Um sich für die Archiv-Unterstützung anzumelden, verwenden Sie diesen [Link](https://aka.ms/ArchivePreviewInterestForm).

## <a name="get-started-with-powershell"></a>Erste Schritte mit PowerShell

1. Führen Sie in PowerShell den folgenden Befehl aus:
  
    ```azurepowershell
    install-module -name Az.RecoveryServices -Repository PSGallery -RequiredVersion 4.0.0-preview -AllowPrerelease -force
    ```

1. Stellen Sie mithilfe des Cmdlets [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) eine Verbindung mit Azure her.
1. Melden Sie sich bei Ihrem Abonnement an:

   `Set-AzContext -Subscription "SubscriptionName"`

1. Tresor abrufen:

    `$vault =  Get-AzRecoveryServicesVault -ResourceGroupName "rgName" -Name "vaultName"`

1. Liste der Sicherungselemente abrufen:

    `$BackupItemList = Get-AzRecoveryServicesBackupItem -vaultId $vault.ID -BackupManagementType "AzureVM/AzureWorkload" -WorkloadType "AzureVM/MSSQL"`

1. Rufen Sie das Sicherungselement ab.

    - Gehen Sie bei virtuellen Azure-Computern wie folgt vor:

        `$bckItm = $BackupItemList | Where-Object {$_.Name -match '<vmName>'}`

    - Gehen Sie bei SQL Server auf virtuellen Azure-Computern wie folgt vor:

        `$bckItm = $BackupItemList | Where-Object {$_.Name -match '<dbName>' -and $_.ContainerName -match '<vmName>'}`

## <a name="use-powershell"></a>Verwenden von PowerShell

### <a name="check-archivable-recovery-points"></a>Überprüfen von archivierbaren Wiederherstellungspunkten

```azurepowershell
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -VaultId $vault.ID -Item $bckItm  -IsReadyForMove $true -TargetTier VaultArchive
```

Damit werden alle Wiederherstellungspunkte aufgelistet, die einem bestimmten Sicherungselement zugeordnet sind, das in das Archiv verschoben werden kann.

### <a name="check-why-a-recovery-point-cannot-be-moved-to-archive"></a>Überprüfen, warum ein Wiederherstellungspunkt nicht in das Archiv verschoben werden kann

```azurepowershell
$rp[0].RecoveryPointMoveReadinessInfo["ArchivedRP"]
```

Dabei entspricht `$rp[0]` dem Wiederherstellungspunkt, der nicht archiviert werden kann und den Sie überprüfen möchten.

Beispielausgabe:

```output
IsReadyForMove  AdditionalInfo
--------------  --------------
False           Recovery-Point Type is not eligible for archive move as it is already moved to archive tier
```

### <a name="check-recommended-set-of-archivable-points-only-for-azure-vms"></a>Überprüfen des empfohlenen Satzes archivierbarer Punkte (nur für virtuelle Azure-Computer)

Die einem virtuellen Computer zugeordneten Wiederherstellungspunkte sind naturgemäß inkrementell. Wenn ein bestimmter Wiederherstellungspunkt in das Archiv verschoben wird, wird er zuerst in eine vollständige Sicherung konvertiert und dann archiviert. Daher richten sich die mit dem Verschieben ins Archiv verbundenen Kosteneinsparungen nach der Änderungsrate der Datenquelle.

Aus diesem Grund stellt Azure Backup einen empfohlenen Satz von Wiederherstellungspunkten bereit, die bei einer gemeinsamen Verschiebung zu Kosteneinsparungen führen können.

>[!NOTE]
>Die Kosteneinsparungen richten sich nach einer Vielzahl von Bedingungen und sind häufig nicht einmal bei zwei Instanzen identisch.

```azurepowershell
$RecommendedRecoveryPointList = Get-AzRecoveryServicesBackupRecommendedArchivableRPGroup -Item $bckItm -VaultId $vault.ID
```

### <a name="move-to-archive"></a>Verschieben in das Archiv

```azurepowershell
Move-AzRecoveryServicesBackupRecoveryPoint -VaultId $vault.ID -RecoveryPoint $rp[2] -SourceTier VaultStandard -DestinationTier VaultArchive
```

Mit diesem Befehl wird ein archivierbarer Wiederherstellungspunkt in das Archiv verschoben. Zurückgegeben wird ein Auftrag, der zum Nachverfolgen des Verschiebevorgangs, sowohl im Portal als auch mit PowerShell, verwendet werden kann.

### <a name="view-archived-recovery-points"></a>Anzeigen archivierter Wiederherstellungspunkte

Mit diesem Befehl werden alle archivierten Wiederherstellungspunkte zurückgegeben.

```azurepowershell
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -VaultId $vault.ID -Item $bckItm -Tier VaultArchive
```

### <a name="restore-with-powershell"></a>Wiederherstellen mit PowerShell

Für Wiederherstellungspunkte im Archiv stellt Azure Backup eine integrierte Wiederherstellungsmethode bereit.

Bei der integrierten Wiederherstellung handelt es sich um einen zweistufigen Vorgang. Zuerst werden die im Archiv gespeicherten Wiederherstellungspunkte aktiviert und temporär für einen Zeitraum von 10 bis 30 Tagen (auch als Aktivierungsdauer bezeichnet) auf der Tresorstandardebene gespeichert. Der Standardwert beträgt 15 Tage. Bei der Aktivierung gibt es zwei unterschiedliche Prioritäten: Standardpriorität und hohe Priorität. Weitere Informationen hierzu finden Sie unter [Aktivierungspriorität](https://docs.microsoft.com/azure/storage/blobs/storage-blob-rehydration#rehydrate-an-archived-blob-to-an-online-tier).

>[!NOTE]
>
>- Die einmal ausgewählte Aktivierungsdauer kann nicht geändert werden, und die aktivierten Wiederherstellungspunkte befinden sich während der Aktivierungsdauer auf der Standardzugriffsebene.
>- Der zusätzliche Aktivierungsschritt verursacht Kosten.

Weitere Informationen zu den verschiedenen Wiederherstellungsmethoden für virtuelle Azure-Computer finden Sie unter [Wiederherstellen eines virtuellen Azure-Computers mit PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm).

```azurepowershell
Restore-AzRecoveryServicesBackupItem -VaultLocation $vault.Location -RehydratePriority "Standard" -RehydrateDuration 15 -RecoveryPoint $rp -StorageAccountName "SampleSA" -StorageAccountResourceGroupName "SArgName" -TargetResourceGroupName $vault.ResourceGroupName -VaultId $vault.ID
```

Zum Wiederherstellen von SQL Server führen Sie [diese Schritte](backup-azure-sql-automation.md#restore-sql-dbs) aus. Die zusätzlich erforderlichen Parameter sind **RehydrationPriority** (Aktivierungspriorität) und **RehydrationDuration** (Aktivierungsdauer).

### <a name="view-jobs-from-powershell"></a>Anzeigen von Aufträgen in PowerShell

Verwenden Sie das folgende PowerShell-Cmdlet, um die Verschiebungs- und Wiederherstellungsaufträge anzuzeigen:

```azurepowershell
Get-AzRecoveryServicesBackupJob -VaultId $vault.ID
```

## <a name="use-the-portal"></a>Verwenden des Portals

### <a name="check-archived-recovery-point"></a>Überprüfen archivierter Wiederherstellungspunkte

Sie können jetzt alle Wiederherstellungspunkte anzeigen, die in das Archiv verschoben wurden.

![Alle Wiederherstellungspunkte.](./media/archive-tier-support/restore-points.png)

### <a name="restore-in-the-portal"></a>Wiederherstellen im Portal

Bei Wiederherstellungspunkten, die ins Archiv verschoben wurden, müssen Sie bei der Wiederherstellung die Parameter für die Aktivierungsdauer und die Aktivierungspriorität hinzufügen.

![Wiederherstellen im Azure-Portal.](./media/archive-tier-support/restore-in-portal.png)

### <a name="view-jobs-in-the-portal"></a>Anzeigen von Aufträgen im Portal

![Anzeigen von Aufträgen im Portal.](./media/archive-tier-support/view-jobs-portal.png)

### <a name="modify-protection"></a>Ändern des Schutzes

Es gibt zwei Möglichkeiten, den Schutz für eine Datenquelle zu ändern:

- Bearbeiten einer vorhandenen Richtlinie
- Schützen der Datenquelle durch eine neue Richtlinie

In beiden Fällen wird die neue Richtlinie auf alle älteren Wiederherstellungspunkte, die sich auf der Standard- und der Archivebene befinden, angewendet. Möglicherweise werden durch Änderung der Richtlinie ältere Wiederherstellungspunkte gelöscht.

Für Wiederherstellungspunkte, die ins Archiv verschoben werden, unterliegen für einen Zeitraum von 180 Tagen einer Gebühr für vorzeitiges Löschen. Die Gebühren werden anteilmäßig abgerechnet. Beim Löschen eines Wiederherstellungspunkts, der sich keine 180 Tage im Archiv befunden hat, werden Kosten für 180 Tage abzüglich der Anzahl der Tage auf der Standardebene in Rechnung gestellt.

Für Wiederherstellungspunkte, die sich nicht mindestens sechs Monate im Archiv befunden haben, wird beim Löschen eine Gebühr für vorzeitiges Löschen fällig.

## <a name="stop-protection-and-delete-data"></a>Beenden des Schutzes und Löschen der Daten

Durch das Beenden des Schutzes und das Löschen der Daten werden alle Wiederherstellungspunkte gelöscht. Bei Wiederherstellungspunkten im Archiv, die sich keine 180 Tage auf der Archivebene befunden haben, verursacht ein Löschvorgang Kosten für vorzeitiges Löschen.

## <a name="error-codes-and-troubleshooting-steps"></a>Fehlercodes und Schritte zur Problembehandlung

Es gibt mehrere Fehlercodes, die auftreten, wenn ein Wiederherstellungspunkt nicht in das Archiv verschoben werden kann.

### <a name="recoverypointtypenoteligibleforarchive"></a>RecoveryPointTypeNotEligibleForArchive

**Fehlermeldung**: Der Typ des Wiederherstellungspunkts ist nicht für eine Verschiebung in das Archiv berechtigt.

**Beschreibung**: Dieser Fehlercode wird angezeigt, wenn der ausgewählte Typ des Wiederherstellungspunkts nicht berechtigt ist, in das Archiv verschoben zu werden.

**Empfohlene Aktion**: Überprüfen Sie [hier](#scope-for-preview) die Berechtigung des Wiederherstellungspunkts.

### <a name="recoverypointhaveactivedependencies"></a>RecoveryPointHaveActiveDependencies

**Fehlermeldung**: Der Wiederherstellungspunkt enthält aktive Abhängigkeiten für die Wiederherstellung und ist nicht berechtigt, in das Archiv verschoben zu werden.

**Beschreibung**: Der ausgewählte Wiederherstellungspunkt enthält aktive Abhängigkeiten und kann daher nicht in das Archiv verschoben werden.

**Empfohlene Aktion**: Überprüfen Sie [hier](#scope-for-preview) die Berechtigung des Wiederherstellungspunkts.

### <a name="minlifespaninstandardrequiredforarchive"></a>MinLifeSpanInStandardRequiredForArchive

**Fehlermeldung**: Der Wiederherstellungspunkt ist nicht berechtigt, in das Archiv verschoben zu werden, weil die bisherige Lebensdauer auf der Tresorstandardebene nicht der erforderlichen Mindestdauer entspricht.

**Beschreibung**: Der Wiederherstellungspunkt muss bei virtuellen Azure-Computern mindestens drei Monate und bei SQL Server mindestens 45 Tage auf der Standardzugriffsebene gespeichert gewesen sein.

**Empfohlene Aktion**: Überprüfen Sie [hier](#scope-for-preview) die Berechtigung des Wiederherstellungspunkts.

### <a name="minremaininglifespaninarchiverequired"></a>MinRemainingLifeSpanInArchiveRequired

**Fehlermeldung**: Die restliche Lebensdauer des Wiederherstellungspunkts liegt unter dem erforderlichen Mindestwert.

**Beschreibung**:Die erforderliche Mindestlebensdauer für einen Wiederherstellungspunkt, die zum Verschieben in das Archiv berechtigt, beträgt sechs Monate.

**Empfohlene Aktion**: Überprüfen Sie [hier](#scope-for-preview) die Berechtigung des Wiederherstellungspunkts.

### <a name="usererrorrecoverypointalreadyinarchivetier"></a>UserErrorRecoveryPointAlreadyInArchiveTier

**Fehlermeldung**: Der Wiederherstellungspunkt ist nicht berechtigt, in das Archiv verschoben zu werden, weil er bereits auf die Archivebene verschoben wurde.

**Beschreibung**: Der ausgewählte Wiederherstellungspunkt befindet sich bereits im Archiv. Daher ist er nicht berechtigt, in das Archiv verschoben zu werden.

### <a name="usererrordatasourcetypeisnotsupportedforrecommendationapi"></a>UserErrorDatasourceTypeIsNotSupportedForRecommendationApi

**Fehlermeldung**: Der Datenquellentyp ist für die Empfehlungs-API nicht berechtigt.

**Beschreibung**: Die Empfehlungs-API ist nur für virtuelle Azure-Computer geeignet. Dies trifft nicht auf den ausgewählten Datenquellentyp zu.

### <a name="usererrorrecoverypointalreadyrehydrated"></a>UserErrorRecoveryPointAlreadyRehydrated

**Fehlermeldung**: Der Wiederherstellungspunkt wurde bereits aktiviert. Die Aktivierung ist für diesen Wiederherstellungspunkt nicht zulässig.

**Beschreibung**: Der ausgewählte Wiederherstellungspunkt wurde bereits aktiviert.

### <a name="usererrorrecoverypointisnoteligibleforarchivemove"></a>UserErrorRecoveryPointIsNotEligibleForArchiveMove

**Fehlermeldung**: Der Wiederherstellungspunkt ist nicht berechtigt, in das Archiv verschoben zu werden.

**Beschreibung**:Der ausgewählte Wiederherstellungspunkt ist nicht berechtigt, in das Archiv verschoben zu werden.

### <a name="usererrorrecoverypointnotrehydrated"></a>UserErrorRecoveryPointNotRehydrated

**Fehler** **meldung**: Der Wiederherstellungspunkt im Archiv ist nicht aktiviert. Wiederholen Sie die Wiederherstellung, nachdem die Aktivierung des Wiederherstellungspunkts im Archiv abgeschlossen ist.

**Beschreibung**: Der Wiederherstellungspunkt ist nicht aktiviert. Versuchen Sie, die Wiederherstellung nach dem Aktivieren des Wiederherstellungspunkts auszuführen.

### <a name="usererrorrecoverypointrehydrationnotallowed"></a>UserErrorRecoveryPointRehydrationNotAllowed

**Fehler** **meldung**: Die Aktivierung wird nur für Wiederherstellungspunkte im Archiv unterstützt.

**Beschreibung**: Für den ausgewählten Wiederherstellungspunkt ist keine Aktivierung zulässig.

### <a name="usererrorrecoverypointrehydrationalreadyinprogress"></a>UserErrorRecoveryPointRehydrationAlreadyInProgress

**Fehlermeldung**: Die Aktivierung des Wiederherstellungspunkts im Archiv wird bereits durchgeführt.

**Beschreibung**: Die Aktivierung des ausgewählten Wiederherstellungspunkts wird bereits durchgeführt.

### <a name="rpmovenotsupportedduetoinsufficientretention"></a>RPMoveNotSupportedDueToInsufficientRetention

**Fehlermeldung**: Der Wiederherstellungspunkt kann nicht auf die Archivebene verschoben werden, da die von der Richtlinie vorgegebene Aufbewahrungsdauer nicht ausreicht.

**Empfohlene Aktion**: Aktualisieren Sie die Richtlinie für das geschützte Element mit der entsprechenden Aufbewahrungseinstellung, und wiederholen Sie dann den Vorgang.

### <a name="rpmovereadinesstobedetermined"></a>RPMoveReadinessToBeDetermined

**Fehlermeldung**: Es wird noch ermittelt, ob dieser Wiederherstellungspunkt verschoben werden kann.

**Beschreibung**: Es muss noch ermittelt werden, ob der Wiederherstellungspunkt verschoben werden kann.

**Empfohlene Aktion**: Warten Sie einen Moment, und wiederholen Sie dann den Vorgang.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="what-will-happen-to-archive-recovery-points-if-i-stop-protection-and-retain-data"></a>Was passiert mit den Wiederherstellungspunkten im Archiv, wenn ich den Schutz beende und die Daten behalte?

Der Wiederherstellungspunkt bleibt dauerhaft im Archiv gespeichert. Weitere Informationen finden Sie unter [Auswirkungen auf Wiederherstellungspunkte beim Beenden des Schutzes](manage-recovery-points.md#impact-of-stop-protection-on-recovery-points).

## <a name="next-steps"></a>Nächste Schritte

- [Preise für Azure Backup](azure-backup-pricing.md)
