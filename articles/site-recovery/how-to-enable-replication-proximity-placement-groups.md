---
title: Replizieren von Azure-VMs, die in Näherungsplatzierungsgruppen ausgeführt werden
description: Erfahren Sie, wie Sie Azure-VMs, die in Näherungsplatzierungsgruppen ausgeführt werden, mithilfe von Azure Site Recovery replizieren.
author: Sharmistha-Rai
manager: gaggupta
ms.topic: how-to
ms.date: 05/25/2020
ms.openlocfilehash: 204ac3be46ac7ba0e1ea96e50379ca417b1299ce
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83847632"
---
# <a name="replicate-azure-virtual-machines-running-in-proximity-placement-groups-to-another-region"></a>Replizieren von virtuellen Azure-Computern, die in Näherungsplatzierungsgruppen ausgeführt werden

In diesem Artikel wird beschrieben, wie virtuelle Computer, die in einer Näherungsplatzierungsgruppe ausgeführt werden, in eine sekundäre Region repliziert werden und wie ein Failover bzw. Failback ausgeführt wird.

[Näherungsplatzierungsgruppen](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups-portal) sind eine logische Gruppierungsfunktion von virtuellen Azure-Computern, die Sie verwenden können, um die Netzwerklatenz zwischen virtuellen Computern zu verringern, die Ihren Anwendungen zugeordnet sind. Wenn die VMs innerhalb derselben Näherungsplatzierungsgruppe bereitgestellt werden, sind sie physisch so nah wie möglich beieinander. Näherungsplatzierungsgruppen eignen sich besonders, um die Anforderungen von latenzsensiblen Workloads zu erfüllen.

## <a name="disaster-recovery-with-proximity-placement-groups"></a>Notfallwiederherstellung mit Näherungsplatzierungsgruppen

In einem typischen Szenario können Ihre virtuellen Computer in einer Näherungsplatzierungsgruppe ausgeführt werden, um die Netzwerklatenz zwischen den verschiedenen Ebenen Ihrer Anwendung zu vermeiden. Obwohl dies eine optimale Netzwerklatenz der Anwendung ermöglicht, möchten Sie diese Anwendung dennoch mithilfe von Site Recovery gegen alle Ausfälle auf Regionsebene schützen. Mit Site Recovery werden die Daten von einer Region in eine andere Azure-Region repliziert, und die Computer werden bei einem Ausfall in der Notfallwiederherstellungsregion aktiviert.

## <a name="considerations"></a>Überlegungen

- Der beste Ansatz ist ein Failover/Failback der virtuellen Computer in eine Näherungsplatzierungsgruppe. Wenn die VM während des Failovers/Failbacks jedoch nicht innerhalb der Näherungsplatzierungsgruppe hochgefahren werden kann, wird trotzdem ein Failover/Failback durchgeführt, und virtuelle Computer werden außerhalb einer Näherungsplatzierungsgruppe erstellt.
-  Wenn eine Verfügbarkeitsgruppe an eine Näherungsplatzierungsgruppe fixiert ist und während des Failover/Failbacks VMs in der Verfügbarkeitsgruppe eine Zuordnungsbeschränkung aufweisen, werden die virtuellen Computer außerhalb sowohl der Verfügbarkeitsgruppe als auch der Näherungsplatzierungsgruppe erstellt.
-  Site Recovery für Näherungsplatzierungsgruppen wird für nicht verwaltete Datenträger nicht unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

1. Stellen Sie sicher, dass Sie über das PowerShell-Modul „Az“ verfügen. Wenn Sie PowerShell installieren oder aktualisieren müssen, befolgen Sie die Anweisungen unter [Handbuch zum Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="set-up-site-recovery-for-virtual-machines-in-proximity-placement-group"></a>Einrichten von Site Recovery für Virtual Machines in einer Näherungsplatzierungsgruppe

### <a name="azure-to-azure"></a>Azure zu Azure

1. [Melden](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#sign-in-to-your-microsoft-azure-subscription) Sie sich bei Ihrem Azure-Konto an, und legen Sie Ihr Abonnement fest.
2. Rufen Sie die Details des virtuellen Computers, den Sie replizieren möchten, wie [hier](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#get-details-of-the-virtual-machine-to-be-replicated) beschrieben ab.
3. [Erstellen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-recovery-services-vault) Sie Ihren Recovery Services-Tresor, und [legen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#set-the-vault-context) Sie den Tresorkontext fest.
4. Bereiten Sie den Tresor für die Replikation des virtuellen Computers vor. Dies umfasst das Erstellen eines [Service Fabric-Objekts](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-site-recovery-fabric-object-to-represent-the-primary-source-region) sowohl für die primäre Region als auch für die Wiederherstellungsregionen.
5. [Erstellen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-site-recovery-protection-container-in-the-primary-fabric) Sie einen Site Recovery-Schutzcontainer im primären Fabric und den Wiederherstellungsfabrics.
6. [Erstellen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-replication-policy) Sie eine Replikationsrichtlinie.
7. Erstellen Sie eine Schutzcontainerzuordnung zwischen dem primären Container und dem Wiederherstellungsschutzcontainer. Verwenden Sie dazu [diese](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-protection-container-mapping-between-the-primary-and-recovery-protection-container) Schritte sowie eine Schutzcontainerzuordnung für Failback, wie [hier](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-protection-container-mapping-for-failback-reverse-replication-after-a-failover) beschrieben.
8. Erstellen Sie ein Cachespeicherkonto mit den [hier](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-cache-storage-account-and-target-storage-account) beschriebenen Schritten.
9. Erstellen Sie die erforderlichen Netzwerkzuordnungen, wie [hier](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-network-mappings) beschrieben.
10. Verwenden Sie das folgende PowerShell-Cmdlet, um den virtuellen Azure-Computer mit verwalteten Datenträgern zu replizieren: 

```azurepowershell
#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration)

#OsDisk
$OSdiskId = $vm.StorageProfile.OsDisk.ManagedDisk.Id
$RecoveryOSDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType
$RecoveryReplicaDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType

$OSDiskReplicationConfig = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $EastUSCacheStorageAccount.Id ` -DiskId $OSdiskId -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType ` -RecoveryTargetDiskAccountType $RecoveryOSDiskAccountType -RecoveryProximityPlacementGroupId $recPpg.Id

# Data disk
$datadiskId1 = $vm.StorageProfile.DataDisks[0].ManagedDisk.Id
$RecoveryReplicaDiskAccountType = $vm.StorageProfile.DataDisks[0].ManagedDisk.StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[0].ManagedDisk.StorageAccountType

$DataDisk1ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id ` -DiskId $datadiskId1 -RecoveryResourceGroupId $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType $RecoveryReplicaDiskAccountType ` -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.

$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk1ReplicationConfig

#Start replication by creating replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.

$TempASRJob = New-AzRecoveryServicesAsrReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId -RecoveryProximityPlacementGroupId $recPpg.Id
```
Sobald der Replikationsvorgang erfolgreich gestartet wurde, werden Daten des virtuellen Computers in die Wiederherstellungsregion repliziert.

Zu Beginn des Replikationsprozesses wird zunächst durch Seeding eine Kopie der zu replizierenden Datenträger des virtuellen Computers in die Wiederherstellungsregion übernommen. Diese Phase wird als erste Replikationsphase bezeichnet.

Sobald die erste Replikation abgeschlossen ist, wird in die Phase der differenziellen Synchronisierung gewechselt. An diesem Punkt ist der virtuelle Computer geschützt, und es kann ein Testfailover ausgeführt werden. Nach Abschluss der ersten Replikation wechselt der Replikationsstatus des replizierten Elements, das den virtuellen Computer darstellt, in den Zustand „Geschützt“.

Überwachen Sie den Replikationsstatus und die Replikationsintegrität für den virtuellen Computer, indem Sie Details des entsprechenden replikationsgeschützten Elements abrufen. 

```azurepowershell
Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $PrimaryProtContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```

11. Zum Ausführen eines Testfailovers und Überprüfen und Bereinigen des Testfailovers führen Sie [diese](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#do-a-test-failover-validate-and-cleanup-test-failover) Schritte aus.
12. Führen Sie ein Failover mit den [hier](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#fail-over-to-azure) beschriebenen Schritten aus.
13. Verwenden Sie das folgende PowerShell-Cmdlet, um den Schutz wiederherzustellen und ein Failback in die Quellregion auszuführen:

```azurepowershell
#Create Cache storage account for replication logs in the primary region
$WestUSCacheStorageAccount = New-AzStorageAccount -Name "a2acachestoragewestus" -ResourceGroupName "A2AdemoRG" -Location 'West US' -SkuName Standard_LRS -Kind Storage


#Use the recovery protection container, new cache storage account in West US and the source region VM resource group 
Update-AzRecoveryServicesAsrProtectionDirection -ReplicationProtectedItem $ReplicationProtectedItem -AzureToAzure -ProtectionContainerMapping $WusToEusPCMapping -LogStorageAccountId $WestUSCacheStorageAccount.Id -RecoveryResourceGroupID $sourceVMResourcegroup.ResourceId -RecoveryProximityPlacementGroupId $vm.ProximityPlacementGroup.Id
```
14. Führen Sie zum Deaktivieren der Replikation die [hier](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#disable-replication) beschriebenen Schritte aus.

### <a name="vmware-to-azure"></a>VMware zu Azure

1. Stellen Sie sicher, dass Sie die Schritte unter [Vorbereiten lokaler VMware-Server](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises) für die Notfallwiederherstellung in Azure ausgeführt haben.
2. Melden Sie sich bei Ihrem Azure-Konto an, und legen Sie Ihr Abonnement fest, wie [hier](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#log-into-azure) beschrieben.
3. [Richten Sie einen Recovery Services-Tresor ein](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#set-up-a-recovery-services-vault), und [legen Sie den Tresorkontext fest](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#set-the-vault-context).
4. [Überprüfen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#validate-vault-registration) Sie die Tresorregistrierung.
5. [Erstellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#create-a-replication-policy) Sie eine Replikationsrichtlinie.
6. [Fügen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#add-a-vcenter-server-and-discover-vms) Sie einen vCenter-Server hinzu, ermitteln Sie virtuelle Computer, und [erstellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#create-storage-accounts-for-replication) Sie Speicherkonten für die Replikation.
7. Um virtuelle VMware-Computer zu replizieren, überprüfen Sie hier die Details, und verwenden Sie das folgende PowerShell-Cmdlet:

```azurepowershell
#Get the target resource group to be used
$ResourceGroup = Get-AzResourceGroup -Name "VMwareToAzureDrPs"

#Get the target virtual network to be used
$RecoveryVnet = Get-AzVirtualNetwork -Name "ASR-vnet" -ResourceGroupName "asrrg" 

#Get the protection container mapping for replication policy named ReplicationPolicy
$PolicyMap = Get-AzRecoveryServicesAsrProtectionContainerMapping -ProtectionContainer $ProtectionContainer | where PolicyFriendlyName -eq "ReplicationPolicy"

#Get the protectable item corresponding to the virtual machine CentOSVM1
$VM1 = Get-AzRecoveryServicesAsrProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM1"

# Enable replication for virtual machine CentOSVM1 using the Az.RecoveryServices module 2.0.0 onwards to replicate to managed disks
# The name specified for the replicated item needs to be unique within the protection container. Using a random GUID to ensure uniqueness
$Job_EnableReplication1 = New-AzRecoveryServicesAsrReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM1 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -logStorageAccountId $LogStorageAccount.Id -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1" -RecoveryProximityPlacementGroupId $recPpg.Id
```
8. Sie können den Replikationsstatus und die Replikationsintegrität des virtuellen Computers mit dem Cmdlet Get-ASRReplicationProtectedItem überprüfen.

```azurepowershell
Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $ProtectionContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```
9. Konfigurieren Sie die Failovereinstellungen, indem Sie die [hier](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#configure-failover-settings) beschriebenen Schritte ausführen.
10. [Führen Sie ein Testfailover durch](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#run-a-test-failover). 
11. Führen Sie das Failover auf Azure mithilfe [dieser](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#fail-over-to-azure) Schritte aus.

### <a name="hyper-v-to-azure"></a>Hyper-V in Azure

1. Stellen Sie sicher, dass Sie die Schritte unter [Vorbereiten lokaler Hyper-V-Server](https://docs.microsoft.com/azure/site-recovery/hyper-v-prepare-on-premises-tutorial) für die Notfallwiederherstellung in Azure ausgeführt haben.
2. [Melden Sie sich bei Azure an](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-1-sign-in-to-your-azure-account).
3. [Richten Sie den Tresor ein](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-2-set-up-the-vault), und [richten Sie den Tresorkontext ein](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-3-set-the-recovery-services-vault-context).
4. [Erstellen](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-4-create-a-hyper-v-site) Sie eine Hyper-V-Site.
5. [Installieren](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-5-install-the-provider-and-agent) Sie den Anbieter und den Agent.
6. [Erstellen](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-6-create-a-replication-policy) Sie eine Replikationsrichtlinie.
7. Aktivieren Sie die Replikation mithilfe der folgenden Schritte: 
    
    a. Rufen Sie das schützbare Element, das dem zu schützenden virtuellen Computer entspricht, wie folgt ab:

    ```azurepowershell
    $VMFriendlyName = "Fabrikam-app"          #Name of the VM
    $ProtectableItem = Get-AzRecoveryServicesAsrProtectableItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
    ```
    b. Schützen Sie den virtuellen Computer. Wenn an den geschützten virtuellen Computer mehrere Datenträger angefügt sind, geben Sie den Betriebssystemdatenträger mit dem Parameter OSDiskName an.
    
    ```azurepowershell
    $OSType = "Windows"          # "Windows" or "Linux"
    $DRjob = New-AzRecoveryServicesAsrReplicationProtectedItem -ProtectableItem $VM -Name $VM.Name -ProtectionContainerMapping $ProtectionContainerMapping -RecoveryAzureStorageAccountId   $StorageAccountID -OSDiskName $OSDiskNameList[$i] -OS $OSType -RecoveryResourceGroupId $ResourceGroupID -RecoveryProximityPlacementGroupId $recPpg.Id
    ```
    c. Warten Sie, bis die virtuellen Computer nach der ersten Replikation einen geschützten Zustand erreicht haben. Abhängig von Faktoren wie der Menge der zu replizierenden Daten und der verfügbare Upstreambandbreite zu Azure kann dies länger dauern. Die Aufträge „State“ und „StateDescription“ werden wie folgt aktualisiert, sobald der virtuelle Computer einen geschützten Zustand erreicht: 
    
    ```azurepowershell
    $DRjob = Get-AzRecoveryServicesAsrJob -Job $DRjob
    $DRjob | Select-Object -ExpandProperty State

    $DRjob | Select-Object -ExpandProperty StateDescription
    ```
    d. Aktualisieren Sie die Wiederherstellungseigenschaften (z. B. die Größe der VM-Rolle) und das Azure-Netzwerk, an das die NIC des virtuellen Computers nach dem Failover angefügt werden soll.

    ```azurepowershell
    $nw1 = Get-AzVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

    $VMFriendlyName = "Fabrikam-App"

    $rpi = Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

    $UpdateJob = Set-AzRecoveryServicesAsrReplicationProtectedItem -InputObject $rpi -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

    $UpdateJob = Get-AzRecoveryServicesAsrJob -Job $UpdateJob

    $UpdateJob | Select-Object -ExpandProperty state

    Get-AzRecoveryServicesAsrJob -Job $job | Select-Object -ExpandProperty state
    ```
8. Führen Sie ein [Testfailover](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-8-run-a-test-failover) durch.


## <a name="next-steps"></a>Nächste Schritte

Um den erneuten Schutz und das Failback für VMware zu Azure auszuführen, führen Sie die [hier](https://docs.microsoft.com/azure/site-recovery/vmware-azure-prepare-failback) beschriebenen Schritte aus.

Um ein Failover für Hyper-V zu Azure auszuführen, führen Sie die Schritte aus, die [hier](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) beschrieben werden, und für die Ausführung eines Failback verwenden Sie die [hier](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-failback) beschriebenen Schritte.

Weitere Informationen finden Sie unter [Failover in Site Recovery](site-recovery-failover.md).
