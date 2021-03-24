---
title: Sichern einer Azure-Dateifreigabe mithilfe von PowerShell
description: In diesem Artikel erfahren Sie, wie Sie eine Azure Files-Dateifreigabe mit dem Azure Backup-Dienst und PowerShell sichern.
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: 948931764769bc967b88e7942b7e8384b0f93dff
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "87077005"
---
# <a name="back-up-an-azure-file-share-by-using-powershell"></a>Sichern einer Azure-Dateifreigabe mithilfe von PowerShell

In diesem Artikel wird beschrieben, wie Sie mit Azure PowerShell und einem Recovery Services-Tresor von [Azure Backup](backup-overview.md) eine Azure Files-Dateifreigabe sichern.

In diesem Artikel wird Folgendes erläutert:

> [!div class="checklist"]
>
> * Einrichten von PowerShell und Registrieren des Recovery Services-Anbieters
> * Erstellen Sie einen Recovery Services-Tresor.
> * Konfigurieren der Sicherung für eine Azure-Dateifreigabe
> * Ausführen eines Sicherungsauftrags

## <a name="before-you-start"></a>Vorbereitung

* [Lesen Sie weitere Informationen](backup-azure-recovery-services-vault-overview.md) zu Recovery Services-Tresoren.
* Sehen Sie sich die [Referenz zum Cmdlet Az.RecoveryServices](/powershell/module/az.recoveryservices) in der Azure-Bibliothek an.
* Sehen Sie sich die folgende PowerShell-Objekthierarchie für Recovery Services an:

  ![Recovery Services-Objekthierarchie](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

## <a name="set-up-powershell"></a>Einrichten von PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Richten Sie PowerShell wie folgt ein:

1. [Laden Sie die neueste Version von Azure PowerShell herunter](/powershell/azure/install-az-ps).

    > [!NOTE]
    > Die erforderliche PowerShell-Mindestversion für die Sicherung von Azure-Dateifreigaben ist Az.RecoveryServices 2.6.0. Wenn Sie die neueste oder zumindest die Mindestversion nutzen, können Sie Probleme mit vorhandenen Skripts vermeiden. Installieren Sie die Mindestversion mit dem folgenden PowerShell-Befehl:
    >
    > ```powershell
    > Install-module -Name Az.RecoveryServices -RequiredVersion 2.6.0
    > ```

2. Ermitteln Sie die PowerShell-Cmdlets für Azure Backup mit dem folgenden Befehl:

    ```powershell
    Get-Command *azrecoveryservices*
    ```

3. Sehen Sie sich die Aliase und Cmdlets für Azure Backup, Azure Site Recovery und den Recovery Services-Tresor an. Unten sehen Sie eine Beispielanzeige. Dies ist keine vollständige Liste der Cmdlets.

    ![Liste der Recovery Services-Cmdlets](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

4. Melden Sie sich mithilfe von **Connect-AzAccount** bei Ihrem Azure-Konto an.
5. Auf der daraufhin angezeigten Webseite werden Sie aufgefordert, Ihre Kontoanmeldeinformationen einzugeben.

    Alternativ können Sie Ihre Kontoanmeldeinformationen als Parameter im Cmdlet **Connect-AzAccount** mit **-Credential** angeben.

    Wenn Sie als CSP-Partner für einen Mandanten tätig sind, geben Sie den Kunden als Mandanten an. Geben Sie dabei seine Mandanten-ID oder den primären Domänennamen des Mandanten an. Beispiel: **Connect-AzAccount -Tenant "fabrikam.com"** .

6. Da ein Konto mehrere Abonnements enthalten kann, müssen Sie das Abonnement, das Sie verwenden möchten, dem Konto zuordnen:

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

7. Falls Sie Azure Backup zum ersten Mal verwenden, registrieren Sie den Azure Recovery Services-Anbieter für Ihr Abonnement mithilfe des Cmdlets **Register-AzResourceProvider**:

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

8. Überprüfen Sie, ob die Anbieter erfolgreich registriert wurden:

    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

9. Vergewissern Sie sich in der Befehlsausgabe, dass sich der Wert für **RegistrationState** in **Registered** ändert. Ist dies nicht der Fall, führen Sie das Cmdlet **Register-AzResourceProvider** erneut aus.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Der Recovery Services-Tresor ist eine Resource Manager-Ressource. Deshalb müssen Sie ihn einer Ressourcengruppe hinzufügen. Sie können eine vorhandene Ressourcengruppe verwenden oder mit dem Cmdlet **New-AzResourceGroup** eine Ressourcengruppe erstellen. Wenn Sie eine Ressourcengruppe erstellen, geben Sie Name und Speicherort an.

Führen Sie die nachstehenden Schritte aus, um einen Recovery Services-Tresors zu erstellen:

1. Erstellen Sie mit [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine neue Ressourcengruppe, wenn noch keine vorhanden ist. In diesem Beispiel wird eine Ressourcengruppe in der Region „USA, Westen“ erstellt:

   ```powershell
   New-AzResourceGroup -Name "test-rg" -Location "West US"
   ```

1. Verwenden Sie das Cmdlet [New-AzRecoveryServicesVault](/powershell/module/az.recoveryservices/new-azrecoveryservicesvault) zum Erstellen des Tresors. Geben Sie für den Tresor den Speicherort an, den Sie auch für die Ressourcengruppe ausgewählt haben.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```

### <a name="view-the-vaults-in-a-subscription"></a>Anzeigen von Tresoren in einem Abonnement

Verwenden Sie [Get-AzRecoveryServicesVault](/powershell/module/az.recoveryservices/get-azrecoveryservicesvault), um alle Tresore des Abonnements anzuzeigen:

```powershell
Get-AzRecoveryServicesVault
```

Die Ausgabe sieht in etwa wie folgt aus: Beachten Sie, dass in der Ausgabe die zugeordnete Ressourcengruppe und der zugehörige Standort angegeben sind.

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

Viele Azure Backup-Cmdlets erfordern das Recovery Services-Tresorobjekt als Eingabe. Daher ist es sinnvoll, das Tresorobjekt in einer Variablen zu speichern.

Der Tresorkontext ist der Datentyp, der im Tresor geschützt wird. Legen Sie ihn mit [Set-AzRecoveryServicesVaultContext](/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext) fest. Der festgelegte Kontext gilt für alle nachfolgenden Cmdlets.

Im folgenden Beispiel wird der Tresorkontext für **testvault** festgelegt:

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

### <a name="fetch-the-vault-id"></a>Abrufen der Tresor-ID

Es ist geplant, die Tresorkontexteinstellung gemäß den Azure PowerShell-Richtlinien als veraltet zu kennzeichnen. Stattdessen können Sie die Tresor-ID speichern oder abrufen und an relevante Befehle übergeben. Wenn Sie den Tresorkontext nicht festgelegt haben oder den Befehl so angeben möchten, dass er für einen bestimmten Tresor ausgeführt wird, übergeben Sie die Tresor-ID wie folgt als `-vaultID` an alle relevanten Befehle:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol -VaultID $vaultID
```

## <a name="configure-a-backup-policy"></a>Konfigurieren einer Sicherungsrichtlinie

Eine Sicherungsrichtlinie gibt den Zeitplan für Sicherungen an und legt fest, wie lange Wiederherstellungspunkte der Sicherung beibehalten werden sollen.

Eine Sicherungsrichtlinie ist mindestens einer Aufbewahrungsrichtlinie zugeordnet. Eine Aufbewahrungsrichtlinie definiert, wie lange ein Wiederherstellungspunkt gespeichert werden soll, bevor er gelöscht wird. Sie können Sicherungen mit einer täglichen, wöchentlichen, monatlichen oder jährlichen Aufbewahrungsdauer konfigurieren.

Im folgenden finden Sie einige Cmdlets für Sicherungsrichtlinien:

* Zeigen Sie den Aufbewahrungszeitraum der Standardsicherungsrichtlinie mit [Get-AzRecoveryServicesBackupRetentionPolicyObject](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject) an.
* Zeigen Sie den Zeitplan der Standardsicherungsrichtlinie mit [Get-AzRecoveryServicesBackupSchedulePolicyObject](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject) an.
* Mit dem Cmdlet [New-AzRecoveryServicesBackupProtectionPolicy](/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy) erstellen Sie eine neue Sicherungsrichtlinie. Dazu geben Sie die Zeitplan- und Aufbewahrungsrichtlinienobjekte an.

Eine Startzeit wird standardmäßig im SchedulePolicyObject definiert. Verwenden Sie das folgende Beispiel, um die Startzeit in die gewünschte Startzeit zu ändern. Die gewünschte Startzeit muss in UTC-Zeit (Universal Coordinated Time) angegeben werden. Beim nachstehenden Beispiel wird vorausgesetzt, dass die gewünschte Startzeit für tägliche Sicherungen 01:00 Uhr UTC ist.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$UtcTime = Get-Date -Date "2019-03-20 01:30:00Z"
$UtcTime = $UtcTime.ToUniversalTime()
$schpol.ScheduleRunTimes[0] = $UtcTime
```

> [!IMPORTANT]
> Sie können die Startzeit nur in 30-Minuten-Einheiten angeben. Im vorherigen Beispiel kann sie nur 01:00:00 oder 02:30:00 sein. 01:15:00 kann nicht als Startzeit angegeben werden.

Im folgenden Beispiel werden die Zeitplanrichtlinie und die Aufbewahrungsrichtlinie in Variablen gespeichert. Anschließend werden diese Variablen als Parameter für eine neue Richtlinie (**NewAFSPolicy**) verwendet. Die Richtlinie **NewAFSPolicy** führt eine tägliche Sicherung aus und bewahrt sie 30 Tage auf.

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

Nachdem Sie die Sicherungsrichtlinie definiert haben, können Sie den Schutz für die Azure-Dateifreigabe mithilfe der Richtlinie aktivieren.

### <a name="retrieve-a-backup-policy"></a>Abrufen einer Sicherungsrichtlinie

Rufen Sie zuerst das entsprechende Richtlinienobjekt mit [Get-AzRecoveryServicesBackupProtectionPolicy](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy) ab. Mit diesem Cmdlet können Sie die mit einem Workloadtyp verknüpften Richtlinien anzeigen oder eine bestimmte Richtlinie abrufen.

#### <a name="retrieve-a-policy-for-a-workload-type"></a>Abrufen einer Richtlinie für einen Workloadtyp

Im folgenden Beispiel werden Richtlinien für den Workloadtyp **AzureFiles** abgerufen:

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
> Die Zeitzone des Felds **BackupTime** in PowerShell ist UTC. Wenn der Zeitpunkt der Sicherung im Azure-Portal angezeigt wird, wird die Uhrzeit auf Ihre lokale Zeitzone eingestellt.

#### <a name="retrieve-a-specific-policy"></a>Abrufen einer bestimmten Richtlinie

Die folgende Richtlinie ruft die Sicherungsrichtlinie mit dem Namen **dailyafs** ab:

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

### <a name="enable-protection-and-apply-the-policy"></a>Aktivieren des Schutzes und Anwenden der Richtlinie

Aktivieren Sie den Schutz mit [Enable-AzRecoveryServicesBackupProtection](/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection). Nachdem die Richtlinie dem Tresor zugeordnet wurde, werden Sicherungen entsprechend dem Richtlinienzeitplan ausgelöst.

Im folgenden Beispiel wird der Schutz für die Azure-Dateifreigabe **testAzureFileShare** im Speicherkonto **testStorageAcct** mit der Richtlinie **dailyafs** aktiviert:

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

Der Befehl wartet, bis der Auftrag zum Konfigurieren des Schutzes abgeschlossen ist, und liefert eine Ausgabe ähnlich dem folgenden Beispiel:

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

Weitere Informationen zum Abrufen einer Liste von Dateifreigaben für ein Speicherkonto finden Sie in [diesem Artikel](/powershell/module/az.storage/get-azstorageshare).

## <a name="important-notice-backup-item-identification"></a>Wichtiger Hinweis: Bestimmen von Sicherungselementen

In diesem Abschnitt wird eine wichtige Änderung bei Sicherungen von Azure-Dateifreigaben in Vorbereitung auf die allgemeine Verfügbarkeit dargelegt.

Wenn Sie eine Sicherung für Azure-Dateifreigaben aktivieren, gibt der Benutzer dem Kunden einen Dateifreigabenamen als Entitätsnamen an. Daraufhin wird ein Sicherungselement erstellt. Der Name des Sicherungselements ist ein eindeutiger Bezeichner, den der Azure Backup-Dienst erstellt. Der Bezeichner ist in der Regel ein benutzerfreundlicher Name. Doch für das Szenario des vorläufigen Löschens, bei dem eine Dateifreigabe gelöscht und eine andere Freigabe mit dem gleichen Namen erstellt werden kann, ist der eindeutige Bezeichner einer Azure-Dateifreigabe nun eine ID.

Um die eindeutige ID jedes Elements zu ermitteln, führen Sie den Befehl **Get-AzRecoveryServicesBackupItem** mit den entsprechenden Filtern für **backupManagementType** und **WorkloadType** aus, um alle relevanten Elemente abzurufen. Beobachten Sie dann das Namensfeld im zurückgegebenen PowerShell-Objekt bzw. in der PowerShell-Antwort.

Es wird immer empfohlen, Elemente aufzulisten und dann ihren eindeutigen Namen aus dem Feld „Name“ in der Antwort abzurufen. Verwenden Sie diesen Wert, um die Elemente mit dem Parameter *Name* zu filtern. Verwenden Sie andernfalls den Parameter *FriendlyName*, um das Element mit seiner ID abzurufen.

> [!IMPORTANT]
> Stellen Sie sicher, dass PowerShell auf die Mindestversion (Az.RecoveryServices 2.6.0) für Sicherungen von Azure-Dateifreigaben aktualisiert wird. Bei dieser Version ist der Filter *FriendlyName* für den Befehl **Get-AzRecoveryServicesBackupItem** verfügbar.
>
> Übergeben Sie den Namen der Azure-Dateifreigabe an den Parameter *friendlyName*. Wenn Sie den Namen der Dateifreigabe an den Parameter *Name* übergeben, löst diese Version die Warnung aus, dass dieser Name an den Parameter *friendlyName* übergeben werden muss.
>
> Wenn Sie diese Mindestversion nicht installieren, kann dies zu Fehlern bei vorhandenen Skripts führen. Installieren Sie die Mindestversion von PowerShell mit dem folgenden Befehl:
>
>```powershell
>Install-module -Name Az.RecoveryServices -RequiredVersion 2.6.0
>```

## <a name="trigger-an-on-demand-backup"></a>Auslösen einer bedarfsgesteuerten Sicherung

Verwenden Sie [Backup-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem), um eine bedarfsgesteuerte Sicherung für eine geschützte Azure-Dateifreigabe auszuführen:

1. Rufen Sie mit [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-Azrecoveryservicesbackupcontainer) das Speicherkonto aus dem Container im Tresor ab, in dem sich die Sicherungsdaten befinden.
2. Zum Starten eines Sicherungsauftrags rufen Sie mit [Get-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem) Informationen zu der Azure-Dateifreigabe ab.
3. Führen Sie mit [Backup-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/backup-Azrecoveryservicesbackupitem) eine bedarfsgesteuerte Sicherung aus.

Führen Sie die bedarfsgesteuerte Sicherung wie folgt aus:

```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -FriendlyName "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

Der Befehl gibt einen Auftrag mit einer ID zur Nachverfolgung zurück, wie im folgenden Beispiel gezeigt wird:

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

Momentaufnahmen von Azure-Dateifreigaben werden verwendet, während die Sicherungen erstellt werden. Normalerweise ist der Auftrag beendet, wenn der Befehl diese Ausgabe zurückgibt.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das [Sichern von Azure-Dateifreigaben im Azure-Portal](backup-afs.md).
* Informationen zum Planen von Sicherungen mit einem Azure Automation-Runbook finden Sie im [Beispielskript auf GitHub](https://github.com/Azure-Samples/Use-PowerShell-for-long-term-retention-of-Azure-Files-Backup).
