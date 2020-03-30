---
title: Notfallwiederherstellung für virtuelle Hyper-V-Computer mit Azure Site Recovery und PowerShell
description: Automatisieren der Notfallwiederherstellung von Hyper-V-VMs in Azure mit dem Azure Site Recovery-Dienst über PowerShell und Azure Resource Manager.
author: sujayt
manager: rochakm
ms.topic: article
ms.date: 01/10/2020
ms.author: sutalasi
ms.openlocfilehash: 6499c986bef965848303ee9833fd59f5e3f0889c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79229166"
---
# <a name="set-up-disaster-recovery-to-azure-for-hyper-v-vms-using-powershell-and-azure-resource-manager"></a>Einrichten der Notfallwiederherstellung in Azure für Hyper-V-VMs mithilfe von PowerShell und Azure Resource Manager

[Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung, indem Replikation, Failover und Wiederherstellung virtueller Azure-Computer und lokaler virtueller Computer sowie physischer Server aufeinander abgestimmt werden.

Dieser Artikel beschreibt, wie Sie Windows PowerShell zusammen mit Azure Resource Manager verwenden, um Hyper-V-VMs in Azure zu replizieren. Das Beispiel in diesem Artikel zeigt, wie Sie einen auf einem Hyper-V-Host ausgeführten einzelnen virtuellen Computer in Azure replizieren.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell stellt Cmdlets zum Verwalten von Azure mit Windows PowerShell zur Verfügung. Mit PowerShell-Cmdlets für Site Recovery, die mit Azure PowerShell für den Azure Resource Manager verfügbar sind, können Sie Ihre Server in Azure schützen und wiederherstellen.

Sie müssen kein PowerShell-Experte sein, um diesen Artikel verwenden zu können. Sie müssen jedoch mit den grundlegenden Konzepten wie Modulen, Cmdlets und Sitzungen vertraut sein. Weitere Informationen finden Sie in der [PowerShell-Dokumentation](/powershell) und unter [Verwenden von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md).

> [!NOTE]
> Microsoft-Partner im CSP-Programm (Cloud Solution Provider, Cloud-Lösungsanbieter) können den Schutz der Server ihrer Kunden in den entsprechenden CSP-Abonnements (Mandantenabonnements) konfigurieren und verwalten.

## <a name="before-you-start"></a>Vorbereitung

Stellen Sie sicher, dass diese Voraussetzungen erfüllt werden:

- Ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung. Darüber hinaus können Sie sich über die [Preisgestaltung für Azure Site Recovery-Manager](https://azure.microsoft.com/pricing/details/site-recovery/)informieren.
- Azure PowerShell. Informationen zu dieser Version und wie Sie diese installieren, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps).

Darüber hinaus gelten für das in diesem Artikel beschriebene Beispiel die folgenden Voraussetzungen:

- Ein Hyper-V-Host unter Windows Server 2012 R2 oder Microsoft Hyper-V Server 2012 R2 mit mindestens einem virtuellen Computer. Hyper-V-Server müssen entweder direkt oder über einen Proxy mit dem Internet verbunden sein.
- Die virtuellen Computer, die Sie replizieren möchten, sollten [diese Voraussetzungen](hyper-v-azure-support-matrix.md#replicated-vms) erfüllen.

## <a name="step-1-sign-in-to-your-azure-account"></a>Schritt 1: Anmelden bei Ihrem Azure-Konto

1. Öffnen Sie eine PowerShell-Konsole, und führen Sie den folgenden Befehl aus, um sich bei Ihrem Azure-Konto anzumelden. Das Cmdlet ruft eine Webseite auf, die Sie zur Eingabe Ihrer Kontoanmeldeinformationen auffordert: `Connect-AzAccount`.
   - Alternativ können Sie Ihre Kontoanmeldeinformationen als Parameter `Connect-AzAccount`Credential**im Cmdlet** angeben.
   - Wenn Sie als CSP-Partner für einen Mandanten tätig sind, geben Sie den Kunden mit dessen Mandanten-ID oder primären Mandantendomänennamen als Mandanten an. Beispiel: `Connect-AzAccount -Tenant "fabrikam.com"`
1. Da ein Konto mehrere Abonnements enthalten kann, müssen Sie das Abonnement, das Sie verwenden möchten, dem Konto zuordnen:

   ```azurepowershell
   Set-AzContext -Subscription $SubscriptionName
   ```

1. Stellen Sie mit den folgenden Befehlen sicher, dass Ihr Abonnement für die Verwendung der Azure-Anbieter für Recovery Services und Site Recovery registriert ist:

   ```azurepowershell
   Get-AzResourceProvider -ProviderNamespace  Microsoft.RecoveryServices
   ```

1. Vergewissern Sie sich in der Befehlsausgabe, dass **RegistrationState** auf **Registered** festgelegt ist. Anschließend können Sie mit Schritt 2 fortfahren. Wenn dies nicht der Fall ist, müssen Sie den fehlenden Anbieter in Ihrem Abonnement registrieren, indem Sie die folgenden Befehle ausführen:

   ```azurepowershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.RecoveryServices
   ```

1. Überprüfen Sie mithilfe der folgenden Befehle, ob die Anbieter erfolgreich registriert wurden:

   ```azurepowershell
   Get-AzResourceProvider -ProviderNamespace  Microsoft.RecoveryServices
   ```

## <a name="step-2-set-up-the-vault"></a>Schritt 2: Einrichten des Tresors

1. Erstellen Sie eine Azure Resource Manager-Ressourcengruppe, in der der Tresor erstellt werden soll, oder verwenden Sie eine vorhandene Ressourcengruppe. Erstellen Sie wie folgt eine neue Ressourcengruppe. Die Variable `$ResourceGroupName` enthält den Namen der zu erstellenden Ressourcengruppe, und die Variable „$Geo“ enthält die Azure-Region, in der die Ressourcengruppe erstellt wird (z. B. „Brasilien, Süden“).

   ```azurepowershell
   New-AzResourceGroup -Name $ResourceGroupName -Location $Geo
   ```

1. Führen Sie das Cmdlet `Get-AzResourceGroup` aus, um eine Liste der Ressourcengruppen in Ihrem Abonnement abzurufen.
1. Erstellen Sie einen neuen Azure Recovery Services-Tresor wie folgt:

   ```azurepowershell
   $vault = New-AzRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>
   ```

Sie können eine Liste der vorhandenen Tresore mit dem Cmdlet `Get-AzRecoveryServicesVault` abrufen.

## <a name="step-3-set-the-recovery-services-vault-context"></a>Schritt 3: Festlegen des Kontexts des Recovery Services-Tresors

Legen Sie den Tresorkontext wie folgt fest:

```azurepowershell
Set-AzRecoveryServicesAsrVaultContext -Vault $vault
```

## <a name="step-4-create-a-hyper-v-site"></a>Schritt 4: Erstellen eines Hyper-V-Standorts

1. Erstellen Sie einen neuen Hyper-V-Standort wie folgt:

   ```azurepowershell
   $sitename = "MySite"                #Specify site friendly name
   New-AzRecoveryServicesAsrFabric -Type HyperVSite -Name $sitename
   ```

1. Dieses Cmdlet startet einen Site Recovery-Auftrag, um den Standort zu erstellen, und gibt ein Site Recovery-Auftragsobjekt zurück. Warten Sie, bis der Auftrag abgeschlossen ist, und überprüfen Sie, ob der Auftrag erfolgreich abgeschlossen wurde.
1. Verwenden Sie das Cmdlet `Get-AzRecoveryServicesAsrJob` zum Abrufen des Auftragsobjekts, und überprüfen Sie den aktuellen Status des Auftrags.
1. Generieren Sie wie folgt einen Registrierungsschlüssel für den Standort, und laden Sie ihn herunter:

   ```azurepowershell
   $SiteIdentifier = Get-AzRecoveryServicesAsrFabric -Name $sitename | Select-Object -ExpandProperty SiteIdentifier
   $path = Get-AzRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename
   ```

1. Kopieren Sie den heruntergeladenen Schlüssel in den Hyper-V-Host. Sie benötigen den Schlüssel, um den Hyper-V-Host am Standort zu registrieren.

## <a name="step-5-install-the-provider-and-agent"></a>Schritt 5: Installieren des Anbieters und des Agents

1. Laden Sie bei [Microsoft](https://aka.ms/downloaddra)das Installationsprogramm für die neueste Version des Anbieters herunter.
1. Führen Sie das Installationsprogramm auf dem Hyper-V-Host aus.
1. Fahren Sie am Ende der Installation mit dem Registrierungsschritt fort.
1. Geben Sie den heruntergeladenen Schlüssel an, wenn Sie dazu aufgefordert werden, und schließen Sie die Registrierung des Hyper-V-Hosts ab.
1. Überprüfen Sie wie folgt, ob der Hyper-V-Host am Standort registriert wurde:

   ```azurepowershell
   $server = Get-AzRecoveryServicesAsrFabric -Name $siteName | Get-AzRecoveryServicesAsrServicesProvider -FriendlyName $server-friendlyname
   ```

Wenn Sie einen Hyper-V Core Server betreiben, laden Sie die Setupdatei herunter, und führen Sie diese Schritte aus:

1. Extrahieren Sie die Dateien aus _AzureSiteRecoveryProvider.exe_ in ein lokales Verzeichnis, indem Sie den folgenden Befehl ausführen:

   ```console
   AzureSiteRecoveryProvider.exe /x:. /q
   ```

1. Führen Sie den folgenden Befehl aus:

   ```console
   .\setupdr.exe /i
   ```

   Die Ergebnisse werden in _%ProgramData%\ASRLogs\DRASetupWizard.log_ protokolliert.

1. Registrieren Sie den Server mithilfe dieses Befehls:

   ```console
   cd  C:\Program Files\Microsoft Azure Site Recovery Provider\DRConfigurator.exe" /r /Friendlyname "FriendlyName of the Server" /Credentials "path to where the credential file is saved"
   ```

## <a name="step-6-create-a-replication-policy"></a>Schritt 6: Erstellen einer Replikationsrichtlinie

Bevor Sie beginnen, muss sich das angegebene Speicherkonto in derselben Azure-Region wie der Tresor befinden, und die Georeplikation muss aktiviert sein.

1. Erstellen Sie eine Replikationsrichtlinie wie folgt:

   ```azurepowershell
   $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
   $PolicyName = “replicapolicy”
   $Recoverypoints = 6                    #specify the number of recovery points
   $storageaccountID = Get-AzStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select-Object -ExpandProperty Id

   $PolicyResult = New-AzRecoveryServicesAsrPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds -NumberOfRecoveryPointsToRetain $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID
   ```

1. Überprüfen Sie den zurückgegebenen Auftrag, um sicherzustellen, dass die Replikationsrichtlinie erfolgreich erstellt wurde.

1. Rufen Sie den Schutzcontainer, der dem Standort entspricht, wie folgt ab:

   ```azurepowershell
   $protectionContainer = Get-AzRecoveryServicesAsrProtectionContainer
   ```

1. Ordnen Sie den Schutzcontainers der Replikationsrichtlinie wie folgt zu:

   ```azurepowershell
   $Policy = Get-AzRecoveryServicesAsrPolicy -FriendlyName $PolicyName
   $associationJob = New-AzRecoveryServicesAsrProtectionContainerMapping -Name $mappingName -Policy $Policy -PrimaryProtectionContainer $protectionContainer[0]
   ```

1. Warten Sie, bis der Zuordnungsauftrag erfolgreich abgeschlossen wurde.

1. Dient zum Abrufen der Schutzcontainerzuordnung.

   ```azurepowershell
   $ProtectionContainerMapping = Get-AzRecoveryServicesAsrProtectionContainerMapping -ProtectionContainer $protectionContainer
   ```

## <a name="step-7-enable-vm-protection"></a>Schritt 7: Aktivieren des Schutzes des virtuellen Computers

1. Rufen Sie das schützbare Element, das dem zu schützenden virtuellen Computer entspricht, wie folgt ab:

   ```azurepowershell
   $VMFriendlyName = "Fabrikam-app"          #Name of the VM
   $ProtectableItem = Get-AzRecoveryServicesAsrProtectableItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
   ```

1. Schützen Sie den virtuellen Computer. Wenn an den geschützten virtuellen Computer mehrere Datenträger angefügt sind, geben Sie den Betriebssystemdatenträger mit dem Parameter **OSDiskName** an.

   ```azurepowershell
   $OSType = "Windows"          # "Windows" or "Linux"
   $DRjob = New-AzRecoveryServicesAsrReplicationProtectedItem -ProtectableItem $VM -Name $VM.Name -ProtectionContainerMapping $ProtectionContainerMapping -RecoveryAzureStorageAccountId $StorageAccountID -OSDiskName $OSDiskNameList[$i] -OS $OSType -RecoveryResourceGroupId $ResourceGroupID
   ```

1. Warten Sie, bis die virtuellen Computer nach der ersten Replikation einen geschützten Zustand erreicht haben. Abhängig von Faktoren wie der Menge der zu replizierenden Daten und der verfügbare Upstreambandbreite zu Azure kann dies länger dauern. Die Aufträge „State“ und „StateDescription“ werden wie folgt aktualisiert, sobald der virtuelle Computer einen geschützten Zustand erreicht:

   ```console
   PS C:\> $DRjob = Get-AzRecoveryServicesAsrJob -Job $DRjob

   PS C:\> $DRjob | Select-Object -ExpandProperty State
   Succeeded

   PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
   Completed
   ```

1. Aktualisieren Sie die Wiederherstellungseigenschaften (z. B. die Größe der VM-Rolle) und das Azure-Netzwerk, an das die NIC des virtuellen Computers nach dem Failover angefügt werden soll.

   ```console
   PS C:\> $nw1 = Get-AzVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

   PS C:\> $VMFriendlyName = "Fabrikam-App"

   PS C:\> $rpi = Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

   PS C:\> $UpdateJob = Set-AzRecoveryServicesAsrReplicationProtectedItem -InputObject $rpi -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

   PS C:\> $UpdateJob = Get-AzRecoveryServicesAsrJob -Job $UpdateJob

   PS C:\> $UpdateJob | Select-Object -ExpandProperty state

   PS C:\> Get-AzRecoveryServicesAsrJob -Job $job | Select-Object -ExpandProperty state

   Succeeded
   ```

> [!NOTE]
> Führen Sie die folgenden Schritte mithilfe von Az PowerShell 3.3.0 oder höher aus, wenn Sie die Replikation auf einem CMK-aktivierten verwalteten Datenträger in Azure durchführen möchten:
>
> 1. Aktivieren des Failovers auf verwaltete Datenträger durch Aktualisieren von VM-Eigenschaften
> 1. Verwenden des Cmdlets `Get-AzRecoveryServicesAsrReplicationProtectedItem` zum Abrufen der Datenträger-ID für jeden Datenträger des geschützten Elements
> 1. Erstellen Sie mit dem Cmdlet `New-Object "System.Collections.Generic.Dictionary``2[System.String,System.String]"` ein Wörterbuchobjekt, das die Zuordnung der Datenträger-ID zum Datenträgerverschlüsselungssatz enthalten soll. Diese Datenträgerverschlüsselungssätze müssen von Ihnen in der Zielregion vorab erstellt werden.
> 1. Aktualisieren Sie die VM-Eigenschaften mithilfe des Cmdlets `Set-AzRecoveryServicesAsrReplicationProtectedItem`, indem Sie das Wörterbuchobjekt im Parameter **DiskIdToDiskEncryptionSetMap** übergeben.

## <a name="step-8-run-a-test-failover"></a>Schritt 8: Ausführen eines Testfailovers

1. Führen Sie ein Testfailover wie folgt aus:

   ```azurepowershell
   $nw = Get-AzVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

   $rpi = Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

   $TFjob = Start-AzRecoveryServicesAsrTestFailoverJob -ReplicationProtectedItem $VM -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
   ```

1. Stellen Sie sicher, dass die Test-VM in Azure erstellt wird. Der Testfailoverauftrag wird nach dem Erstellen des virtuellen Testcomputers in Azure angehalten.
1. Führen Sie zum Bereinigen und Abschließen des Testfailovers Folgendes aus:

   ```azurepowershell
   $TFjob = Start-AzRecoveryServicesAsrTestFailoverCleanupJob -ReplicationProtectedItem $rpi -Comment "TFO done"
   ```

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr](/powershell/module/az.recoveryservices) über Azure Site Recovery mit PowerShell-Cmdlets für Azure Resource Manager.
