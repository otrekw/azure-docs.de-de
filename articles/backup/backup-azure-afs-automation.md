---
title: Sichern von Azure Files mit PowerShell
description: In diesem Artikel erfahren Sie, wie Sie Azure Files mit dem Azure Backup-Dienst und PowerShell sichern.
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: 865cfc6daa7568236b0306ba591b42a9f7704dd4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82101177"
---
# <a name="back-up-azure-files-with-powershell"></a>Sichern von Azure Files mit PowerShell

In diesem Artikel wird beschrieben, wie Sie mit Azure PowerShell und einem Recovery Services-Tresor von [Azure Backup](backup-overview.md) eine Azure Files-Dateifreigabe sichern.

In diesem Artikel wird Folgendes erläutert:

> [!div class="checklist"]
>
> * Einrichten von PowerShell und Registrieren des Azure Recovery Services-Anbieters
> * Erstellen Sie einen Recovery Services-Tresor.
> * Konfigurieren der Sicherung für eine Azure-Dateifreigabe
> * Ausführen eines Sicherungsauftrags

## <a name="before-you-start"></a>Vorbereitung

* [Lesen Sie weitere Informationen](backup-azure-recovery-services-vault-overview.md) zu Recovery Services-Tresoren.
* Sehen Sie sich die PowerShell-Objekthierarchie für Recovery Services an.

## <a name="recovery-services-object-hierarchy"></a>Recovery Services-Objekthierarchie

Die Objekthierarchie ist im folgenden Diagramm zusammengefasst.

![Recovery Services-Objekthierarchie](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

[Sehen Sie sich die ](/powershell/module/az.recoveryservices)Cmdlet-Referenz zu **Az.RecoveryServices** in der Azure-Bibliothek an.

## <a name="set-up-and-install"></a>Einrichten und Installieren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Richten Sie PowerShell wie folgt ein:

1. [Laden Sie die neueste Version von Azure PowerShell](/powershell/azure/install-az-ps) herunter. Version 1.0.0 ist die erforderliche Mindestversion.

    > [!WARNING]
    > Die erforderliche PS-Mindestversion für die Azure-Dateifreigabesicherung ist **Az.RecoveryServices 2.6.0**. Führen Sie ein Upgrade Ihrer Version durch, um Probleme mit den vorhandenen Skripts zu vermeiden. Installieren Sie die Mindestversion mit den folgenden PowerShell-Befehlen:

    ```powershell
    Install-module -Name Az.RecoveryServices -RequiredVersion 2.6.0
    ```

2. Ermitteln Sie die PowerShell-Cmdlets für Azure Backup mit dem folgenden Befehl:

    ```powershell
    Get-Command *azrecoveryservices*
    ```

3. Sehen Sie sich die Aliase und Cmdlets für Azure Backup, Azure Site Recovery und den Recovery Services-Tresor an. Unten sehen Sie eine Beispielanzeige. Dies ist keine vollständige Liste der Cmdlets.

    ![Liste der Recovery Services-Cmdlets](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

4. Melden Sie sich mit **Connect-AzAccount** bei Ihrem Azure-Konto an.
5. Auf der daraufhin angezeigten Webseite werden Sie aufgefordert, Ihre Kontoanmeldeinformationen einzugeben.

    * Alternativ können Sie Ihre Kontoanmeldeinformationen als Parameter im Cmdlet **Connect-AzAccount** mit **-Credential** angeben.
    * Wenn Sie als CSP-Partner für einen Mandanten tätig sind, geben Sie den Kunden mit dessen Mandanten-ID oder primären Mandantendomänennamen als Mandanten an. Beispiel: **Connect-AzAccount -Tenant** fabrikam.com.

6. Da ein Konto mehrere Abonnements enthalten kann, müssen Sie das Abonnement, das Sie verwenden möchten, dem Konto zuordnen.

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

7. Falls Sie Azure Backup zum ersten Mal verwenden, registrieren Sie den Azure Recovery Services-Anbieter für Ihr Abonnement mithilfe des Cmdlets **Register-AzResourceProvider**.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

8. Überprüfen Sie, ob die Anbieter erfolgreich registriert wurden:

    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

9. Vergewissern Sie sich in der Befehlsausgabe, dass sich der Wert für **RegistrationState** in **Registered** ändert. Ist dies nicht der Fall, führen Sie das Cmdlet **Register-AzResourceProvider** erneut aus.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Der Recovery Services-Tresor ist eine Resource Manager-Ressource. Deshalb müssen Sie ihn in eine Ressourcengruppe einfügen. Sie können eine vorhandene Ressourcengruppe verwenden oder eine Ressourcengruppe mit dem Cmdlet **New-AzResourceGroup** erstellen. Wenn Sie eine Ressourcengruppe erstellen, geben Sie den Namen und den Speicherort dafür an.

Führen Sie die nachstehenden Schritte aus, um einen Recovery Services-Tresors zu erstellen.

1. Ein Tresor wird in einer Ressourcengruppe platziert. Erstellen Sie mit [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-1.4.0) eine neue Ressourcengruppe, wenn noch keine vorhanden ist. In diesem Beispiel wird eine neue Ressourcengruppe in der Region „USA, Westen“ erstellt.

   ```powershell
   New-AzResourceGroup -Name "test-rg" -Location "West US"
   ```

2. Verwenden Sie das Cmdlet [New-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/New-AzRecoveryServicesVault?view=azps-1.4.0) zum Erstellen des Tresors. Geben Sie denselben Speicherort für den Tresor an, der für die Ressourcengruppe verwendet wurde.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```

3. Geben Sie den für den Tresorspeicher zu verwendenden Redundanztyp an.

   * Sie können [lokal redundanten Speicher](../storage/common/storage-redundancy-lrs.md) oder [georedundanten Speicher](../storage/common/storage-redundancy-grs.md) verwenden.
   * Im folgenden Beispiel wird die Option **-BackupStorageRedundancy** für den Befehl [Set-AzRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty) für **testvault** auf den Wert **GeoRedundant** festgelegt.

     ```powershell
     $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
     Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
     ```

### <a name="view-the-vaults-in-a-subscription"></a>Anzeigen von Tresoren in einem Abonnement

Wenn Sie alle Tresore des Abonnements anzeigen möchten, verwenden Sie [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0).

```powershell
Get-AzRecoveryServicesVault
```

Die Ausgabe sieht in etwa wie folgt aus: Beachten Sie, dass die zugeordnete Ressourcengruppe und der Speicherort angegeben werden.

```powershell
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

### <a name="set-the-vault-context"></a>Festlegen des Tresorkontexts

Speichern Sie das Tresorobjekt in einer Variablen, und legen Sie den Tresorkontext fest.

* Viele Azure Backup-Cmdlets erfordern das Recovery Services-Tresorobjekt als Eingabe. Daher ist es sinnvoll, das Tresorobjekt in einer Variablen zu speichern.
* Der Tresorkontext ist der Datentyp, der im Tresor geschützt wird. Legen Sie ihn mit [Set-AzRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext?view=azps-1.4.0) fest. Der festgelegte Kontext gilt für alle nachfolgenden Cmdlets.

Im folgenden Beispiel wird der Tresorkontext für **testvault** festgelegt.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

### <a name="fetch-the-vault-id"></a>Abrufen der Tresor-ID

Es ist geplant, die Tresorkontexteinstellung gemäß den Azure PowerShell-Richtlinien als veraltet zu kennzeichnen. Stattdessen können Sie die Tresor-ID speichern oder abrufen und an relevante Befehle übergeben. Wenn Sie also den Tresorkontext nicht festgelegt haben oder den Befehl so angeben möchten, dass er für einen bestimmten Tresor ausgeführt wird, übergeben Sie die Tresor-ID wie folgt als „-vaultID“ an alle relevanten Befehle:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol -VaultID $vaultID
```

## <a name="configure-a-backup-policy"></a>Konfigurieren einer Sicherungsrichtlinie

Eine Sicherungsrichtlinie gibt den Zeitplan für Sicherungen an und legt fest, wie lange Wiederherstellungspunkte der Sicherung beibehalten werden sollen:

* Eine Sicherungsrichtlinie ist mindestens einer Aufbewahrungsrichtlinie zugeordnet. Eine Aufbewahrungsrichtlinie definiert, wie lange ein Wiederherstellungspunkt gespeichert werden soll, bevor er gelöscht wird.
* Zeigen Sie den Aufbewahrungszeitraum der Standardsicherungsrichtlinie mit [Get-AzRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject?view=azps-1.4.0) an.
* Zeigen Sie den Zeitplan der Standardsicherungsrichtlinie mit [Get-AzRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject?view=azps-1.4.0) an.
* Mit dem Cmdlet [New-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0) erstellen Sie eine neue Sicherungsrichtlinie. Dazu geben Sie die Zeitplan- und Aufbewahrungsrichtlinienobjekte ein.

Eine Startzeit wird standardmäßig im Schedule Policy Object (Zeitplanrichtlinienobjekt) definiert. Verwenden Sie das folgende Beispiel, um die Startzeit in die gewünschte Startzeit zu ändern. Die gewünschte Startzeit sollte ebenfalls in UTC angegeben werden. Beim nachstehenden Beispiel wird vorausgesetzt, dass die gewünschte Startzeit für tägliche Sicherungen 01:00 Uhr UTC ist.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$UtcTime = Get-Date -Date "2019-03-20 01:30:00Z"
$UtcTime = $UtcTime.ToUniversalTime()
$schpol.ScheduleRunTimes[0] = $UtcTime
```

> [!IMPORTANT]
> Sie können die Startzeit nur in 30-Minuten-Einheiten angeben. Im obigen Beispiel kann sie nur „01:00:00“ oder „02:30:00“ lauten. „01:15:00“ kann nicht als Startzeit angegeben werden.

Im folgenden Beispiel werden die Zeitplanrichtlinie und die Aufbewahrungsrichtlinie in Variablen gespeichert. Anschließend werden diese Variablen als Parameter für eine neue Richtlinie (**NewAFSPolicy**) verwendet. Die Richtlinie **NewAFSPolicy**führt eine tägliche Sicherung aus und bewahrt sie 30 Tage auf.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

Die Ausgabe sieht in etwa wie folgt aus:

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2019 1:30:00 AM
```

## <a name="enable-backup"></a>Aktivieren der Sicherung

Nachdem Sie die Sicherungsrichtlinie definiert haben, können Sie den Schutz für die Azure-Dateifreigabe mit der Richtlinie aktivieren.

### <a name="retrieve-a-backup-policy"></a>Abrufen einer Sicherungsrichtlinie

Rufen Sie zuerst das entsprechende Richtlinienobjekt mit [Get-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0) ab. Verwenden Sie dieses Cmdlet zum Abrufen einer bestimmten Richtlinie oder zum Anzeigen der Richtlinien, die einem bestimmten Workloadtyp zugeordnet sind.

#### <a name="retrieve-a-policy-for-a-workload-type"></a>Abrufen einer Richtlinie für einen Workloadtyp

Im folgenden Beispiel werden Richtlinien für den Workloadtyp **AzureFiles** abgerufen.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

Die Ausgabe sieht in etwa wie folgt aus:

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```

> [!NOTE]
> Die Zeitzone für das Feld **BackupTime** in PowerShell ist die koordinierte Weltzeit (UTC). Wenn der Zeitpunkt der Sicherung im Azure-Portal angezeigt wird, wird die Uhrzeit auf Ihre lokale Zeitzone eingestellt.

### <a name="retrieve-a-specific-policy"></a>Abrufen einer bestimmten Richtlinie

Die folgende Richtlinie ruft die Sicherungsrichtlinie mit dem Namen **dailyafs** ab.

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

### <a name="enable-backup-and-apply-policy"></a>Aktivieren der Sicherung und Anwenden der Richtlinie

Aktivieren Sie den Schutz mit [Enable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0). Nachdem die Richtlinie dem Tresor zugeordnet wurde, werden Sicherungen entsprechend dem Richtlinienzeitplan ausgelöst.

Im folgenden Beispiel wird der Schutz für die Azure-Dateifreigabe **testAzureFileShare** im Speicherkonto **testStorageAcct** mit der Richtlinie **dailyafs** aktiviert.

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

Der Befehl wartet, bis der Auftrag zum Konfigurieren des Schutzes beendet ist, und gibt eine ähnliche Ausgabe wie gezeigt zurück.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

## <a name="important-notice---backup-item-identification-for-afs-backups"></a>Wichtiger Hinweis: Identifizierung von Sicherungselementen für AFS-Sicherungen

In diesem Abschnitt wird eine wichtige Änderung bei der AFS-Sicherung als Vorbereitung für die allgemeine Verfügbarkeit erläutert.

Beim Aktivieren der Sicherung für AFS gibt der Benutzer den benutzerfreundlichen Dateifreigabenamen als Entitätsnamen ein. Dann wird ein Sicherungselement erstellt. Der „Name“ des Sicherungselements ist ein eindeutiger Bezeichner, der im Azure Backup-Dienst erstellt wird. Der Bezeichner enthält in der Regel den benutzerfreundlichen Namen. In Bezug auf das wichtige Szenario des vorläufigen Löschens, bei dem eine Dateifreigabe gelöscht werden und eine andere Freigabe mit dem gleichen Namen erstellt werden kann, wird als eindeutige Identität in Azure nun jedoch eine ID anstelle eines benutzerfreundlichen Namens verwendet. Um die eindeutige Identität oder den eindeutigen Namen der einzelnen Elemente zu ermitteln, führen Sie einfach den Befehl ```Get-AzRecoveryServicesBackupItem``` mit den entsprechenden Filtern für backupManagementType und WorkloadType aus, um alle relevanten Elemente abzurufen, und sehen Sie sich dann das Feld „Name“ im zurückgegebenen PS-Objekt oder der zurückgegebenen PS-Antwort an. Es wird immer empfohlen, Elemente aufzulisten und dann ihren eindeutigen Namen aus dem Feld „Name“ in der Antwort abzurufen. Verwenden Sie diesen Wert, um die Elemente mit dem Parameter „Name“ zu filtern. Verwenden Sie andernfalls den Parameter „FriendlyName“, um das Element mit seinem benutzerfreundlichen Namen oder Bezeichner abzurufen.

> [!WARNING]
> Stellen Sie für AFS-Sicherungen sicher, dass die PS-Version auf die Mindestversion für Az.RecoveryServices 2.6.0 aktualisiert wird. Mit dieser Version ist der Filter „friendlyName“ für den Befehl ```Get-AzRecoveryServicesBackupItem``` verfügbar. Übergeben Sie den Namen der Azure-Dateifreigabe an den Parameter „friendlyName“. Wenn Sie den Namen der Azure-Dateifreigabe an den Parameter „Name“ übergeben, löst diese Version eine Warnung aus, dass dieser Anzeigename an den Parameter „friendlyName“ übergeben werden muss. Wenn Sie diese Mindestversion nicht installieren, kann dies zu Fehlern bei vorhandenen Skripts führen. Installieren Sie die Mindestversion von PowerShell mit dem folgenden Befehl.

```powershell
Install-module -Name Az.RecoveryServices -RequiredVersion 2.6.0
```

## <a name="trigger-an-on-demand-backup"></a>Auslösen einer bedarfsgesteuerten Sicherung

Verwenden Sie [Backup-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem?view=azps-1.4.0), um eine bedarfsgesteuerte Sicherung für eine geschützte Azure-Dateifreigabe auszuführen.

1. Rufen Sie mit [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-Azrecoveryservicesbackupcontainer) das Speicherkonto aus dem Container im Tresor ab, in dem sich die Sicherungsdaten befinden.
2. Zum Starten eines Sicherungsauftrags rufen Sie mit [Get-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem) Informationen zu der Azure-Dateifreigabe ab.
3. Führen Sie mit [Backup-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/backup-Azrecoveryservicesbackupitem) eine bedarfsgesteuerte Sicherung aus.

Führen Sie die bedarfsgesteuerte Sicherung wie folgt aus:

```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -FriendlyName "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

Der Befehl gibt einen Auftrag mit einer ID zur Nachverfolgung zurück, wie im folgenden Beispiel gezeigt wird.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

Momentaufnahmen von Azure-Dateifreigaben werden beim Erstellen von Sicherungen verwendet. Daher ist der Auftrag in der Regel abgeschlossen, bis der Befehl diese Ausgabe zurückgibt.

### <a name="using-a-runbook-to-schedule-backups"></a>Verwenden eines Runbooks zum Planen von Sicherungen

Wenn Sie nach Beispielskripts suchen, können Sie mit einem Azure Automation-Runbook auf das [Beispielskript auf GitHub](https://github.com/Azure-Samples/Use-PowerShell-for-long-term-retention-of-Azure-Files-Backup) verweisen.

>[!NOTE]
> Die Azure-Sicherungsrichtlinie für Dateifreigaben unterstützt jetzt die Sicherung mit täglicher/wöchentlicher/monatlicher oder jährlicher Aufbewahrung.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich im Azure-Portal über die [Sicherung von Azure Files-Dateifreigaben](backup-afs.md).
