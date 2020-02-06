---
title: Einrichten der Hyper-V-Notfallwiederherstellung (mit VMM) an einem sekundären Standort mit Azure Site Recovery/PowerShell
description: Dieser Artikel beschreibt, wie Sie die Notfallwiederherstellung von Hyper-V-VMs, die sich in VMM-Clouds befinden, in einem sekundären VMM-Standort mithilfe von Azure Site Recovery und PowerShell einrichten.
services: site-recovery
author: sujayt
manager: rochakm
ms.topic: article
ms.date: 1/10/2020
ms.author: sutalasi
ms.openlocfilehash: a46bca5c5c55338f8bea7e1ff370f92ce6a2d577
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76841045"
---
# <a name="set-up-disaster-recovery-of-hyper-v-vms-to-a-secondary-site-by-using-powershell-resource-manager"></a>Einrichten der Notfallwiederherstellung von Hyper-V-VMs in einen sekundären Standort mithilfe von PowerShell (Resource Manager)

In diesem Artikel wird beschrieben, wie Sie die Schritte für die Replikation von Hyper-V-VMs in System Center Virtual Machine Manager-Clouds in eine Virtual Machine Manager-Cloud an einem sekundären lokalen Standort mit [Azure Site Recovery](site-recovery-overview.md) automatisieren.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Überprüfen Sie [Architektur und Komponenten des Szenarios](hyper-v-vmm-architecture.md).
- Überprüfen Sie die [Supportanforderungen](site-recovery-support-matrix-to-sec-site.md) für alle Komponenten.
- Stellen Sie sicher, dass die Virtual Machine Manager-Server und Hyper-V-Hosts den [Anforderungen für die Unterstützung](site-recovery-support-matrix-to-sec-site.md) entsprechen.
- Überprüfen Sie, ob die VMs, die Sie replizieren möchten, die Anforderungen für die [Unterstützung replizierter Computer](site-recovery-support-matrix-to-sec-site.md) erfüllen.

## <a name="prepare-for-network-mapping"></a>Bereiten Sie sich auf die Netzwerkzuordnung vor

Die [Netzwerkzuordnung](hyper-v-vmm-network-mapping.md) ist die Zuordnung zwischen lokalen Virtual Machine Manager-VM-Netzwerken in Quell- und Zielclouds. Die Zuordnung ermöglicht Folgendes:

- Nach dem Failover werden Verbindungen von VMs mit geeigneten Ziel-VM-Netzwerken hergestellt.
- Replikat-VMs werden optimal auf Ziel-Hyper-V-Hostservern platziert.
- Wenn Sie die Netzwerkzuordnung nicht konfigurieren, werden Replikat-VMs nach einem Failover nicht mit einem VM-Netzwerk verbunden.

Bereiten Sie Virtual Machine Manager wie folgt vor:

- Stellen Sie sicher, dass [logische Virtual Machine Manager-Netzwerke](https://docs.microsoft.com/system-center/vmm/network-logical) auf den Virtual Machine Manager-Quell- und -Zielservern vorhanden sind:
  - Das logische Netzwerk auf dem Quellserver sollte der Quellcloud zugeordnet sein, in der sich Hyper-V-Hosts befinden.
  - Das logische Netzwerk auf dem Zielserver sollte der Zielcloud zugeordnet werden.
- Stellen Sie sicher, dass [VM-Netzwerke](https://docs.microsoft.com/system-center/vmm/network-virtual) auf den Virtual Machine Manager-Quell- und -Zielservern vorhanden sind. VM-Netzwerke sollten an jedem Standort mit dem logischen Netzwerk verknüpft werden.
- Stellen Sie die Verbindung der virtuellen Computer auf den Hyper-V-Quellhosts mit dem VM-Quellnetzwerk her.

## <a name="prepare-for-powershell"></a>Vorbereiten von PowerShell

Stellen Sie sicher, dass Azure PowerShell zur Verwendung bereit ist:

- Wenn Sie PowerShell bereits verwenden, aktualisieren Sie auf Version 0.8.10 oder höher. [Erfahren Sie mehr](/powershell/azureps-cmdlets-docs) über das Einrichten von PowerShell.
- Überprüfen Sie nach Abschluss der Einrichtung und Konfiguration von PowerShell die [Dienst-Cmdlets](/powershell/azure/overview).
- Weitere Informationen zur Verwendung der Parameterwerte, Eingaben und Ausgaben in PowerShell finden Sie in der Anleitung [Getting started with Azure PowerShell](/powershell/azure/get-started-azureps) (Erste Schritte mit Azure PowerShell).

## <a name="set-up-a-subscription"></a>Einrichten eines Abonnements

1. Melden Sie sich in PowerShell bei Ihrem Azure-Konto an.

   ```azurepowershell
   $UserName = "<user@live.com>"
   $Password = "<password>"
   $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
   $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
   Connect-AzAccount #-Credential $Cred
   ```

1. Rufen Sie über die Abonnement-IDs eine Liste Ihrer Abonnements ab. Notieren Sie die ID des Abonnements, in dem Sie den Recovery Services-Tresor erstellen möchten.

   ```azurepowershell
   Get-AzSubscription
   ```

1. Legen Sie das Abonnement für den Tresor fest.

   ```azurepowershell
   Set-AzContext –SubscriptionID <subscriptionId>
   ```

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

1. Erstellen Sie eine Azure Resource Manager-Ressourcengruppe, falls noch keine vorhanden ist.

   ```azurepowershell
   New-AzResourceGroup -Name #ResourceGroupName -Location #location
   ```

1. Erstellen Sie einen neuen Recovery Services-Tresor. Speichern Sie das Tresorobjekt zu späteren Verwendung in einer Variablen.

   ```azurepowershell
   $vault = New-AzRecoveryServicesVault -Name #vaultname -ResourceGroupName #ResourceGroupName -Location #location
   ```

   Sie können das Tresorobjekt nach seiner Erstellung mit dem Cmdlet `Get-AzRecoveryServicesVault` abrufen.

## <a name="set-the-vault-context"></a>Festlegen des Tresorkontexts

1. Rufen Sie einen vorhandenen Tresor ab.

   ```azurepowershell
   $vault = Get-AzRecoveryServicesVault -Name #vaultname
   ```

1. Legen Sie den Tresorkontext fest.

   ```azurepowershell
   Set-AzRecoveryServicesAsrVaultContext -Vault $vault
   ```

## <a name="install-the-site-recovery-provider"></a>Installieren des Site Recovery-Anbieters

1. Erstellen Sie auf dem Virtual Machine Manager-Computer ein Verzeichnis, indem Sie den folgenden Befehl ausführen:

   ```azurepowershell
   New-Item -Path C:\ASR -ItemType Directory
   ```

1. Extrahieren Sie die Dateien mithilfe der heruntergeladenen Anbietersetupdatei.

   ```console
   pushd C:\ASR\
   .\AzureSiteRecoveryProvider.exe /x:. /q
   ```

1. Installieren Sie den Anbieter, und warten Sie, bis die Installation abgeschlossen wurde.

   ```console
   .\SetupDr.exe /i
   $installationRegPath = "HKLM:\Software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
   do
   {
     $isNotInstalled = $true;
     if(Test-Path $installationRegPath)
     {
       $isNotInstalled = $false;
     }
   }While($isNotInstalled)
   ```

1. Registrieren Sie den Server im Tresor.

   ```console
   $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
   pushd $BinPath
   $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice
   ```

## <a name="create-and-associate-a-replication-policy"></a>Erstellen und Zuordnen einer Replikationsrichtlinie

1. Erstellen Sie wie folgt eine Replikationsrichtlinie (in diesem Fall für Hyper-V 2012 R2):

   ```azurepowershell
   $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
   $PolicyName = “replicapolicy”
   $RepProvider = HyperVReplica2012R2
   $Recoverypoints = 24                    #specify the number of hours to retain recovery points
   $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
   $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
   $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
   $InitialRepMethod = "Online" #options are "Online" or "Offline"

   $policyresult = New-AzRecoveryServicesAsrPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -NumberOfRecoveryPointsToRetain $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod
   ```

   > [!NOTE]
   > Die Virtual Machine Manager-Cloud kann Hyper-V-Hosts enthalten, auf denen verschiedene Versionen von Windows Server ausgeführt werden, die Replikationsrichtlinie gilt jedoch für eine bestimmte Betriebssystemversion. Wenn Sie über verschiedene Hosts verfügen, die unter unterschiedlichen Betriebssystemversionen ausgeführt werden, erstellen Sie separate Replikationsrichtlinien für jedes System. Wenn Sie beispielsweise fünf Hosts unter Windows Server 2012 und drei Hosts unter Windows Server 2012 R2 haben, erstellen Sie zwei Replikationsrichtlinien. Für jeden Betriebssystemtyp erstellen Sie eine Richtlinie.

1. Rufen Sie den primären Schutzcontainer (primäre Virtual Machine Manager-Cloud) und den Schutzcontainer für die Wiederherstellung (Virtual Machine Manager-Cloud für die Wiederherstellung) ab.

   ```azurepowershell
   $PrimaryCloud = "testprimarycloud"
   $primaryprotectionContainer = Get-AzRecoveryServicesAsrProtectionContainer -FriendlyName $PrimaryCloud;

   $RecoveryCloud = "testrecoverycloud"
   $recoveryprotectionContainer = Get-AzRecoveryServicesAsrProtectionContainer -FriendlyName $RecoveryCloud;
   ```

1. Rufen Sie die Replikationsrichtlinie, die Sie erstellt haben, mithilfe des Anzeigenamens ab.

   ```azurepowershell
   $policy = Get-AzRecoveryServicesAsrPolicy -FriendlyName $policyname
   ```

1. Starten Sie die Zuordnung des Schutzcontainers (Virtual Machine Manager-Cloud) zur Replikationsrichtlinie.

   ```azurepowershell
   $associationJob  = New-AzRecoveryServicesAsrProtectionContainerMapping -Policy $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
   ```

1. Warten Sie, bis der Richtlinienzuordnungsauftrag abgeschlossen wurde. Mit dem folgenden PowerShell-Codeausschnitt können Sie überprüfen, ob der Auftrag abgeschlossen wurde:

   ```azurepowershell
   $job = Get-AzRecoveryServicesAsrJob -Job $associationJob

   if($job -eq $null -or $job.StateDescription -ne "Completed")
   {
     $isJobLeftForProcessing = $true;
   }
   ```

1. Nachdem die Verarbeitung des Auftrags abgeschlossen wurde, führen Sie den folgenden Befehl aus:

   ```azurepowershell
   if($isJobLeftForProcessing)
   {
     Start-Sleep -Seconds 60
   }
   While($isJobLeftForProcessing)
   ```

Um den Abschluss des Vorgangs zu überprüfen, führen Sie die Schritte in [Überwachen der Aktivität](#monitor-activity) aus.

##  <a name="configure-network-mapping"></a>Konfigurieren der Netzwerkzuordnung

1. Verwenden Sie diesen Befehl, um Server für den aktuellen Tresor abzurufen. Der Befehl speichert die Site Recovery-Server in der Arrayvariable `$Servers`.

   ```azurepowershell
   $Servers = Get-AzRecoveryServicesAsrFabric
   ```

1. Führen Sie den folgenden Befehl aus, um die Netzwerke für den Virtual Machine Manager-Quellserver und den Virtual Machine Manager-Zielserver abzurufen.

   ```azurepowershell
   $PrimaryNetworks = Get-AzRecoveryServicesAsrNetwork -Name $Servers[0]

   $RecoveryNetworks = Get-AzRecoveryServicesAsrNetwork -Name $Servers[1]
   ```

   > [!NOTE]
   > Der Virtual Machine Manager-Quellserver kann der erste oder zweite Server im Serverarray sein. Überprüfen Sie die Namen der Virtual Machine Manager-Server, und rufen Sie die Netzwerke entsprechend ab.

1. Dieses Cmdlet erstellt eine Zuordnung zwischen dem primären Netzwerk und dem Wiederherstellungsnetzwerk. Es gibt das primäre Netzwerk als erstes Element von `$PrimaryNetworks` an. Es gibt das Wiederherstellungsnetzwerk als erstes Element von `$RecoveryNetworks` an.

   ```azurepowershell
   New-AzRecoveryServicesAsrNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]
   ```

## <a name="enable-protection-for-vms"></a>Aktivieren des Schutzes für virtuelle Computer

Nach der korrekten Konfiguration der Server, Clouds und Netzwerke aktivieren Sie den Schutz für VMs in der Cloud.

1. Führen Sie den folgenden Befehl aus, um den Schutzcontainer abzurufen und den Schutz zu aktivieren:

   ```azurepowershell
   $PrimaryProtectionContainer = Get-AzRecoveryServicesAsrProtectionContainer -FriendlyName $PrimaryCloudName
   ```

1. Rufen Sie die Schutzentität (VM) wie folgt ab:

   ```azurepowershell
   $protectionEntity = Get-AzRecoveryServicesAsrProtectableItem -FriendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
   ```

1. Aktivieren Sie die Replikation für die VM.

   ```azurepowershell
   $jobResult = New-AzRecoveryServicesAsrReplicationProtectedItem -ProtectableItem $protectionentity -ProtectionContainerMapping $policy -VmmToVmm
   ```

> [!NOTE]
> Führen Sie die folgenden Schritte mithilfe von Az PowerShell 3.3.0 oder höher aus, wenn Sie Replikation auf CMK-aktivierte verwaltete Datenträger in Azure durchführen möchten:
>
> 1. Aktivieren des Failovers auf verwaltete Datenträger durch Aktualisieren von VM-Eigenschaften
> 1. Verwenden des Cmdlets `Get-AzRecoveryServicesAsrReplicationProtectedItem` zum Abrufen der Datenträger-ID für jeden Datenträger des geschützten Elements
> 1. Erstellen Sie mit dem Cmdlet `New-Object "System.Collections.Generic.Dictionary``2[System.String,System.String]"` ein Wörterbuchobjekt, das die Zuordnung der Datenträger-ID zum Datenträgerverschlüsselungssatz enthalten soll. Diese Datenträgerverschlüsselungssätze müssen von Ihnen in der Zielregion vorab erstellt werden.
> 1. Aktualisieren Sie die VM-Eigenschaften mithilfe des Cmdlets `Set-AzRecoveryServicesAsrReplicationProtectedItem`, indem Sie das Wörterbuchobjekt im Parameter **DiskIdToDiskEncryptionSetMap** übergeben.

## <a name="run-a-test-failover"></a>Ausführen eines Testfailovers

Führen Sie zum Testen Ihrer Bereitstellung ein Testfailover für eine einzelne VM aus. Sie können auch einen Wiederherstellungsplan erstellen, der mehrere VMs enthält, und ein Testfailover für den Plan ausführen. Das Test-Failover simuliert Ihre Failover- und Wiederherstellungsmechanismen in einem isolierten Netzwerk.

1. Rufen Sie die VM ab, zu der das Failover für die VMs ausgeführt wird.

   ```azurepowershell
   $Servers = Get-AzRecoveryServicesASRFabric
   $RecoveryNetworks = Get-AzRecoveryServicesAsrNetwork -Name $Servers[1]
   ```

1. Führen Sie ein Testfailover aus.

   Für einen einzelnen virtuellen Computer

   ```azurepowershell
   $protectionEntity = Get-AzRecoveryServicesAsrProtectableItem -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

   $jobIDResult = Start-AzRecoveryServicesAsrTestFailoverJob -Direction PrimaryToRecovery -ReplicationProtectedItem $protectionEntity -VMNetwork $RecoveryNetworks[1]
   ```

   Für einen Wiederherstellungsplan

   ```azurepowershell
   $recoveryplanname = "test-recovery-plan"

   $recoveryplan = Get-AzRecoveryServicesAsrRecoveryPlan -FriendlyName $recoveryplanname

   $jobIDResult = Start-AzRecoveryServicesAsrTestFailoverJob -Direction PrimaryToRecovery -RecoveryPlan $recoveryplan -VMNetwork $RecoveryNetworks[1]
   ```

Um den Abschluss des Vorgangs zu überprüfen, führen Sie die Schritte in [Überwachen der Aktivität](#monitor-activity) aus.

## <a name="run-planned-and-unplanned-failovers"></a>Ausführen geplanter und ungeplanter Failover

1. Führen Sie ein geplantes Failover aus.

   Für einen einzelnen virtuellen Computer

   ```azurepowershell
   $protectionEntity = Get-AzRecoveryServicesAsrProtectableItem -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

   $jobIDResult = Start-AzRecoveryServicesAsrPlannedFailoverJob -Direction PrimaryToRecovery -ReplicationProtectedItem $protectionEntity
   ```

   Für einen Wiederherstellungsplan

   ```azurepowershell
   $recoveryplanname = "test-recovery-plan"

   $recoveryplan = Get-AzRecoveryServicesAsrRecoveryPlan -FriendlyName $recoveryplanname

   $jobIDResult = Start-AzRecoveryServicesAsrPlannedFailoverJob -Direction PrimaryToRecovery -RecoveryPlan $recoveryplan
   ```

1. Führen Sie ein ungeplantes Failover aus.

   Für einen einzelnen virtuellen Computer

   ```azurepowershell
   $protectionEntity = Get-AzRecoveryServicesAsrProtectableItem -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

   $jobIDResult = Start-AzRecoveryServicesAsrUnplannedFailoverJob -Direction PrimaryToRecovery -ReplicationProtectedItem $protectionEntity
   ```

   Für einen Wiederherstellungsplan

   ```azurepowershell
   $recoveryplanname = "test-recovery-plan"

   $recoveryplan = Get-AzRecoveryServicesAsrRecoveryPlan -FriendlyName $recoveryplanname

   $jobIDResult = Start-AzRecoveryServicesAsrUnplannedFailoverJob -Direction PrimaryToRecovery -RecoveryPlan $recoveryplan
   ```

## <a name="monitor-activity"></a>Überwachen der Aktivität

Verwenden Sie die folgenden Befehle zum Überwachen der Failoveraktivität. Warten Sie zwischen Aufträgen, bis die Verarbeitung abgeschlossen wurde.

```azurepowershell
Do
{
    $job = Get-AzRecoveryServicesAsrJob -TargetObjectId $associationJob.JobId;
    Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
    if($job -eq $null -or $job.StateDescription -ne "Completed")
    {
        $isJobLeftForProcessing = $true;
    }

if($isJobLeftForProcessing)
    {
        Start-Sleep -Seconds 60
    }
}While($isJobLeftForProcessing)
```

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr](/powershell/module/az.recoveryservices) über Site Recovery mit PowerShell-Cmdlets für Resource Manager.
