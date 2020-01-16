---
title: Serverseitige Verschlüsselung von Azure Managed Disks – PowerShell
description: Azure Storage schützt Ihre Daten, indem der Dienst diese im Ruhezustand verschlüsselt, bevor diese auf Storage-Clustern gespeichert werden. Sie können von Microsoft verwaltete Schlüssel für die Verschlüsselung Ihrer verwalteten Datenträger nutzen, oder Sie können mit vom Kunden verwalteten Schlüsseln die Verschlüsselung mit Ihren eigenen Schlüsseln verwalten.
author: roygara
ms.date: 12/13/2019
ms.topic: conceptual
ms.author: rogarana
ms.service: virtual-machines-windows
ms.subservice: disks
ms.openlocfilehash: c84c3b5b7e299cdd83f10746adb31ec539c2badd
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75460086"
---
# <a name="server-side-encryption-of-azure-managed-disks"></a>Serverseitige Verschlüsselung von Azure Managed Disks

Verwaltete Azure-Datenträger verschlüsseln Daten standardmäßig automatisch, wenn die Daten in der Cloud gespeichert werden. Die serverseitige Verschlüsselung schützt Ihre Daten und unterstützt Sie beim Einhalten der Sicherheits- und Complianceanforderungen Ihrer Organisation. Daten in verwalteten Azure-Datenträgern werden transparent mit 256-Bit-[AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) – einer der stärksten verfügbaren Blockverschlüsselungen – FIPS 140-2-konform ver- und entschlüsselt.   

Die Verschlüsselung wirkt sich nicht auf die Leistung verwalteter Datenträger aus. Für die Verschlüsselung werden keine zusätzlichen Kosten in Rechnung gestellt.

Weitere Informationen zu den kryptografischen Modulen, die verwalteten Azure-Datenträgern zugrunde liegen, finden Sie unter [Kryptografie-API: Die nächste Generation](https://docs.microsoft.com/windows/desktop/seccng/cng-portal).

## <a name="about-encryption-key-management"></a>Informationen zur Verwaltung von Verschlüsselungsschlüsseln

Sie können von der Plattform verwaltete Schlüssel für die Verschlüsselung der verwalteten Datenträger verwenden oder die Verschlüsselung mit eigenen Schlüsseln verwalten (öffentliche Vorschauversion). Wenn Sie die Verschlüsselung mit eigenen Schlüsseln verwalten möchten, können Sie einen *vom Kunden verwalteten Schlüssel* angeben, der zum Verschlüsseln und Entschlüsseln aller Daten in verwalteten Datenträgern verwendet werden soll. 

In den folgenden Abschnitten werden die einzelnen Optionen für die Schlüsselverwaltung ausführlicher beschrieben.

## <a name="platform-managed-keys"></a>Von der Plattform verwaltete Schlüssel

Verwaltete Datenträger verwenden standardmäßig von der Plattform verwaltete Verschlüsselungsschlüssel. Seit 10. Juni 2017 werden alle neuen verwalteten Datenträger, Momentaufnahmen, Images und neuen Daten, die auf vorhandene verwaltete Datenträger geschrieben wurden, im Ruhezustand automatisch mit von der Plattform verwalteten Schlüsseln verschlüsselt. 

## <a name="customer-managed-keys-public-preview"></a>Vom Kunden verwaltete Schlüssel (öffentliche Vorschauversion)

Sie können die Verschlüsselung auf der Ebene verwalteter Datenträger mit eigenen Schlüsseln verwalten. Die serverseitige Verschlüsselung für verwaltete Datenträger mit vom Kunden verwalteten Schlüsseln bietet eine integrierte Benutzerfunktionalität mit Azure Key Vault. Sie können entweder [ihre RSA-Schlüssel](../../key-vault/key-vault-hsm-protected-keys.md) in den Schlüsseltresor importieren oder neue RSA-Schlüssel in Azure Key Vault generieren. Die Verschlüsselung und Entschlüsselung von verwalteten Azure-Datenträgern erfolgt durch [Umschlagverschlüsselung](../../storage/common/storage-client-side-encryption.md#encryption-and-decryption-via-the-envelope-technique) vollständig transparent. Daten werden mithilfe eines [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)-256 basierten Datenverschlüsselungsschlüssels (DEK) verschlüsselt, der wiederum mit Ihren Schlüsseln geschützt wird. Sie müssen den Zugriff auf verwaltete Datenträger in Ihrem Schlüsseltresor gewähren, damit Sie Ihre Schlüssel zum Verschlüsseln und Entschlüsseln des DEK verwenden können. Dies ermöglicht eine umfassende Kontrolle über Ihre Daten und Schlüssel. Sie können Ihre Schlüssel jederzeit deaktivieren oder den Zugriff auf verwaltete Datenträger widerrufen. Sie können auch die Verwendung von Verschlüsselungsschlüsseln mithilfe der Azure Key Vault-Überwachung überwachen, um sicherzustellen, dass nur verwaltete Datenträger oder andere vertrauenswürdige Azure-Dienste auf Ihre Schlüssel zugreifen.

Das folgende Diagramm zeigt, wie verwaltete Datenträger Azure Active Directory und Azure Key Vault verwenden, um Anforderungen mit dem vom Kunden verwalteten Schlüssel zu senden:

![Workflow für verwaltete Datenträger mit vom Kunden verwalteten Schlüsseln](media/disk-storage-encryption/customer-managed-keys-sse-managed-disks-workflow.png)


In der folgenden Liste wird das Diagramm ausführlicher erläutert:

1. Ein Azure Key Vault-Administrator erstellt Schlüsseltresorressourcen.
1. Der Schlüsseltresoradministrator importiert die jeweiligen RSA-Schlüssel in Key Vault oder generiert neue RSA-Schlüssel in Key Vault.
1. Dieser Administrator erstellt eine Instanz der Datenträgerverschlüsselungssatz-Ressource und gibt eine Azure Key Vault-ID sowie eine Schlüssel-URL an. Der Datenträgerverschlüsselungssatz ist eine neue Ressource, die zur Vereinfachung der Schlüsselverwaltung für verwaltete Datenträger eingeführt wurde. 
1. Beim Erstellen eines Datenträgerverschlüsselungssatzes wird eine [vom System zugewiesene verwaltete Identität](../../active-directory/managed-identities-azure-resources/overview.md) in Azure Active Directory (AD) erstellt und mit dem Datenträgerverschlüsselungssatz verknüpft. 
1. Der Azure Key Vault-Administrator erteilt dann der verwalteten Identität Berechtigungen zum Durchführen von Vorgängen im Schlüsseltresor.
1. Ein VM-Benutzer erstellt Datenträger, indem er sie dem Datenträgerverschlüsselungssatz zuordnet. Der VM-Benutzer kann auch die serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln für vorhandene Ressourcen aktivieren, indem er diese dem Datenträgerverschlüsselungssatz zuordnet. 
1. Verwaltete Datenträger verwenden die verwaltete Identität, um Anforderungen an den Azure Key Vault zu senden.
1. Zum Lesen oder Schreiben von Daten senden verwaltete Datenträger Anforderungen an Azure Key Vault, um den Datenverschlüsselungsschlüssel zu verschlüsseln (wrap) und zu entschlüsseln (unwrap), um die Daten zu verschlüsseln und zu entschlüsseln. 

Informationen zum Widerrufen von Kunden verwalteter Schlüsseln finden Sie in den Artikeln zu [Azure Key Vault-PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) und zur [Azure Key Vault-CLI](https://docs.microsoft.com/cli/azure/keyvault). Durch das Widerrufen des Zugriffs wird der Zugriff auf alle Daten im Speicherkonto blockiert, da Azure Storage keinen Zugriff mehr auf den Verschlüsselungsschlüssel hat.

### <a name="supported-scenarios-and-restrictions"></a>Unterstützte Szenarien und Einschränkungen

Während der Vorschauphase werden nur die folgenden Szenarien unterstützt:

- Erstellen eines virtuellen Computers (VM) aus einem Azure Marketplace-Image und Verschlüsseln des Betriebssystemdatenträgers mit serverseitiger Verschlüsselung unter Verwendung vom Kunden verwalteter Schlüssel.
- Erstellen eines benutzerdefinierten Image, das mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wird.
- Erstellen einer VM aus einem benutzerdefiniertes Image und Verschlüsseln des Betriebssystemdatenträgers mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln.
- Erstellen von Datenträgern, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt werden.
- Erstellen von Momentaufnahmen, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt werden.
- Erstellen von VM-Skalierungsgruppen, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt werden.

Für die Vorschauversion gelten folgende Einschränkungen:

- Nur in der Region „USA, Westen-Mitte“ verfügbar.
- Datenträger, die aus benutzerdefinierten Images erstellt werden, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wurden, müssen mit denselben vom Kunden verwalteten Schlüsseln verschlüsselt werden und sich im selben Abonnement befinden.
- Momentaufnahmen, die von Datenträgern erstellt werden, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wurden, müssen mit denselben vom Kunden verwalteten Schlüsseln verschlüsselt werden.
- Benutzerdefinierte Images, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wurden, können nicht im Katalog mit freigegebenen Images verwendet werden.
- Der Schlüsseltresor muss sich in demselben Abonnement und derselben Region wie die vom Kunden verwalteten Schlüssel befinden.
- Mit vom Kunden verwalteten Schlüsseln verschlüsselte Datenträger, Momentaufnahmen und Images können nicht in ein anderes Abonnement verschoben werden.

### <a name="setting-up-your-azure-key-vault-and-diskencryptionset"></a>Einrichten von Azure Key Vault und DiskEncryptionSet

1.  Erstellen Sie eine Azure Key Vault-Instanz und den Verschlüsselungsschlüssel.

    Beim Erstellen der Key Vault-Instanz müssen Sie vorläufiges Löschen und den Schutz vor endgültigem Löschen aktivieren. Durch vorläufiges Löschen wird sichergestellt, dass der Schlüsseltresor einen gelöschten Schlüssel für einen bestimmten Aufbewahrungszeitraum (standardmäßig 90 Tage) speichert. Der Schutz vor endgültigem Löschen stellt sicher, dass ein gelöschter Schlüssel erst nach Ablauf der Aufbewahrungsdauer dauerhaft gelöscht werden kann. Diese Einstellungen schützen Sie vor dem Verlust von Daten durch versehentliches Löschen. Diese Einstellungen sind obligatorisch, wenn ein Schlüsseltresor für die Verschlüsselung verwalteter Datenträger verwendet wird.

    ```powershell
    $ResourceGroupName="yourResourceGroupName"
    $LocationName="westcentralus"
    $keyVaultName="yourKeyVaultName"
    $keyName="yourKeyName"
    $keyDestination="Software"
    $diskEncryptionSetName="yourDiskEncryptionSetName"

    $keyVault = New-AzKeyVault -Name $keyVaultName -ResourceGroupName $ResourceGroupName -Location $LocationName -EnableSoftDelete -EnablePurgeProtection

    $key = Add-AzKeyVaultKey -VaultName $keyVaultName -Name $keyName -Destination $keyDestination  
    ```

1.  Erstellen Sie eine DiskEncryptionSet-Instanz. 
    
    ```powershell
    $desConfig=New-AzDiskEncryptionSetConfig -Location $LocationName -SourceVaultId $keyVault.ResourceId -KeyUrl $key.Key.Kid -IdentityType SystemAssigned

    $des=New-AzDiskEncryptionSet -Name $diskEncryptionSetName -ResourceGroupName $ResourceGroupName -InputObject $desConfig 
    ```

1.  Gewähren Sie der DiskEncryptionSet-Ressource Zugriff auf den Schlüsseltresor.

    ```powershell
    $identity = Get-AzADServicePrincipal -DisplayName myDiskEncryptionSet1  
     
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $des.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get
     
    New-AzRoleAssignment -ResourceName $keyVaultName -ResourceGroupName $ResourceGroupName -ResourceType "Microsoft.KeyVault/vaults" -  ObjectId $des.Identity.PrincipalId -RoleDefinitionName "Reader" 
    ```

### <a name="create-a-vm-using-a-marketplace-image-encrypting-the-os-and-data-disks-with-customer-managed-keys"></a>Erstellen einer VM mit einem Marketplace-Image, Verschlüsseln der Datenträger für Betriebssystem und Daten mit vom Kunden verwalteten Schlüsseln

```powershell
$VMLocalAdminUser = "yourVMLocalAdminUserName"
$VMLocalAdminSecurePassword = ConvertTo-SecureString <password> -AsPlainText -Force
$LocationName = "westcentralus"
$ResourceGroupName = "yourResourceGroupName"
$ComputerName = "yourComputerName"
$VMName = "yourVMName"
$VMSize = "Standard_DS3_v2"
$diskEncryptionSetName="yourdiskEncryptionSetName"
    
$NetworkName = "yourNetworkName"
$NICName = "yourNICName"
$SubnetName = "yourSubnetName"
$SubnetAddressPrefix = "10.0.0.0/24"
$VnetAddressPrefix = "10.0.0.0/16"
    
$SingleSubnet = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $SubnetAddressPrefix
$Vnet = New-AzVirtualNetwork -Name $NetworkName -ResourceGroupName $ResourceGroupName -Location $LocationName -AddressPrefix $VnetAddressPrefix -Subnet $SingleSubnet
$NIC = New-AzNetworkInterface -Name $NICName -ResourceGroupName $ResourceGroupName -Location $LocationName -SubnetId $Vnet.Subnets[0].Id
    
$Credential = New-Object System.Management.Automation.PSCredential ($VMLocalAdminUser, $VMLocalAdminSecurePassword);
    
$VirtualMachine = New-AzVMConfig -VMName $VMName -VMSize $VMSize
$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2012-R2-Datacenter' -Version latest

$diskEncryptionSet=Get-AzDiskEncryptionSet -ResourceGroupName $ResourceGroupName -Name $diskEncryptionSetName

$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name $($VMName +"_OSDisk") -DiskEncryptionSetId $diskEncryptionSet.Id -CreateOption FromImage

$VirtualMachine = Add-AzVMDataDisk -VM $VirtualMachine -Name $($VMName +"DataDisk1") -DiskSizeInGB 128 -StorageAccountType Premium_LRS -CreateOption Empty -Lun 0 -DiskEncryptionSetId $diskEncryptionSet.Id 
    
New-AzVM -ResourceGroupName $ResourceGroupName -Location $LocationName -VM $VirtualMachine -Verbose
```

### <a name="create-an-empty-disk-encrypted-using-server-side-encryption-with-customer-managed-keys-and-attach-it-to-a-vm"></a>Erstellen eines leeren Datenträgers, der mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wird, und Anfügen an eine VM

```PowerShell
$vmName = "yourVMName"
$LocationName = "westcentralus"
$ResourceGroupName = "yourResourceGroupName"
$diskName = "yourDiskName"
$diskSKU = "Premium_LRS"
$diskSizeinGiB = 30
$diskLUN = 1
$diskEncryptionSetName="yourDiskEncryptionSetName"


$vm = Get-AzVM -Name $vmName -ResourceGroupName $ResourceGroupName 

$diskEncryptionSet=Get-AzDiskEncryptionSet -ResourceGroupName $ResourceGroupName -Name $diskEncryptionSetName

$vm = Add-AzVMDataDisk -VM $vm -Name $diskName -CreateOption Empty -DiskSizeInGB $diskSizeinGiB -StorageAccountType $diskSKU -Lun $diskLUN -DiskEncryptionSetId $diskEncryptionSet.Id 

Update-AzVM -ResourceGroupName $rgName -VM $vm

```


> [!IMPORTANT]
> Von Kunden verwaltete Schlüssel basieren auf verwalteten Identitäten für Azure-Ressourcen, einem Feature von Azure Active Directory (Azure AD). Wenn Sie vom Kunden verwaltete Schlüssel konfigurieren, wird Ihren Ressourcen im Hintergrund automatisch eine verwaltete Identität zugewiesen. Wenn Sie anschließend das Abonnement, die Ressourcengruppe oder den verwalteten Datenträger von einem Azure AD-Verzeichnis in ein anderes Verzeichnis verschieben, wird die den verwalteten Datenträgern zugeordnete verwaltete Identität nicht an den neuen Mandanten übertragen, sodass vom Kunden verwaltete Schlüssel möglicherweise nicht mehr funktionieren. Weitere Informationen finden Sie unter [Übertragen eines Abonnements zwischen Azure AD-Verzeichnissen](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).

## <a name="server-side-encryption-versus-azure-disk-encryption"></a>Vergleich: Serverseitige Verschlüsselung und Azure-Datenträgerverschlüsselung

[Azure Disk Encryption](../../security/fundamentals/azure-disk-encryption-vms-vmss.md) nutzt das [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview)-Feature von Windows und das [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt)-Feature von Linux zum Verschlüsseln verwalteter Datenträger mit vom Kunden verwalteten Schlüsseln innerhalb der Gast-VM.  Die serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln bewirkt eine ADE-Verbesserung, indem Sie beliebige Betriebssystemtypen und Images für Ihre VMs verwenden können, indem Daten im Speicherdienst verschlüsselt werden.

## <a name="next-steps"></a>Nächste Schritte

- [Untersuchen der Azure Resource Manager-Vorlagen zum Erstellen verschlüsselter Datenträger mit vom Kunden verwalteten Schlüsseln](https://github.com/ramankumarlive/manageddiskscmkpreview)
- [Was ist der Azure-Schlüsseltresor?](../../key-vault/key-vault-overview.md)