---
title: Überprüfen des Verschlüsselungsstatus unter Linux
description: In diesem Artikel finden Sie Anweisungen zum Überprüfen des Verschlüsselungsstatus auf Plattform- und Betriebssystemebene.
author: kailashmsft
ms.service: security
ms.topic: article
ms.author: kaib
ms.date: 03/11/2020
ms.custom: seodec18
ms.openlocfilehash: 0aaa32c46d915eafffcfac9d95cfdd3a24d4086d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80123420"
---
# <a name="how-to-verify-encryption-status-for-linux"></a>Überprüfen des Verschlüsselungsstatus unter Linux 

**Dieses Szenario gilt für ADE-Erweiterungen mit einem und zwei Durchläufen.**  
Das Ziel dieser Dokumentation ist es, den Verschlüsselungsstatus einer VM (virtueller Computer) mithilfe verschiedener Methoden zu überprüfen.

### <a name="environment"></a>Environment

- Linux-Distributionen

### <a name="procedure"></a>Verfahren

Eine VM wurde mit einem oder zwei Durchläufen verschlüsselt.

Der Verschlüsselungsstatus kann während oder nach der Verschlüsselung anhand verschiedener Methoden überprüft werden.

>[!NOTE] 
>Im Verlauf der Dokumentation werden Variablen verwendet. Achten Sie darauf, dass Sie die Werte entsprechend ersetzen.

### <a name="verification"></a>Überprüfung

Die Überprüfung kann über das Portal, PowerShell, die Azure CLI oder über das Betriebssystem der VM durchgeführt werden. 

Diese Überprüfung können Sie durchführen, indem Sie die Datenträger überprüfen, die einer bestimmten VM angefügt sind. 

Alternativ können Sie die Verschlüsselungseinstellungen auf jedem einzelnen Datenträger abfragen, um in Erfahrung zu bringen, ob sie angefügt oder nicht angefügt sind.

Im Folgenden werden die verschiedenen Überprüfungsmethoden aufgeführt:

## <a name="using-the-portal"></a>Verwenden des Portals

Überprüfen Sie den Verschlüsselungsstatus im Abschnitt „Erweiterungen“ im Azure-Portal.

Im Abschnitt **Erweiterungen** wird die ADE-Erweiterung aufgeführt. 

Klicken Sie auf diese Erweiterung, und sehen Sie sich die **Statusmeldung** an. Diese gibt den aktuellen Verschlüsselungsstatus an:

![Überprüfung im Portal 1](./media/disk-encryption/verify-encryption-linux/portal-check-001.png)

In der Liste der Erweiterungen wird die entsprechende Version de ADE-Erweiterung angezeigt. Version 0.x entspricht ADE mit zwei Durchläufen und Version 1.x entspricht ADE mit einem Durchlauf.

Weitere Details finden Sie, indem Sie auf die Erweiterung und dann auf *Detaillierten Status anzeigen* klicken.

Daraufhin wird ein ausführlicherer Status des Verschlüsselungsprozesses im JSON-Format angezeigt:

![Überprüfung im Portal 2](./media/disk-encryption/verify-encryption-linux/portal-check-002.png)

![Überprüfung im Portal 3](./media/disk-encryption/verify-encryption-linux/portal-check-003.png)

Eine weitere Möglichkeit zum Überprüfen des Verschlüsselungsstatus besteht darin, den Abschnitt **Datenträger** zu überprüfen.

![Überprüfung im Portal 4](./media/disk-encryption/verify-encryption-linux/portal-check-004.png)

>[!NOTE] 
> Dieser Status deutet darauf hin, dass die Datenträger über gestempelte Verschlüsselungseinstellungen verfügen, aber das bedeutet nicht, dass sie tatsächlich auf Betriebssystemebene verschlüsselt wurden. Die Datenträger werden entwurfsgemäß erst gestempelt und später verschlüsselt. Wenn der Verschlüsselungsprozess fehlschlägt, werden die Datenträger womöglich gestempelt, aber nicht verschlüsselt. Sie können die Verschlüsselung der Datenträger auf der Betriebssystemebene wiederholt prüfen, um sicherzustellen, ob die Datenträger wirklich verschlüsselt wurden.

## <a name="using-powershell"></a>PowerShell

Sie können den **allgemeinen** Verschlüsselungsstatus einer verschlüsselten VM mithilfe der folgenden PowerShell-Befehle überprüfen:

```azurepowershell
   $VMNAME="VMNAME"
   $RGNAME="RGNAME"
   Get-AzVmDiskEncryptionStatus -ResourceGroupName  ${RGNAME} -VMName ${VMNAME}
```
![Überprüfung in PowerShell 1](./media/disk-encryption/verify-encryption-linux/verify-status-ps-01.png)

Sie können die Verschlüsselungseinstellungen der einzelnen Datenträger mithilfe der folgenden PowerShell-Befehle erfassen:

### <a name="single-pass"></a>Einzeldurchlauf
Bei Verschlüsselungen mit einem Durchlauf werden die Verschlüsselungseinstellungen auf jedem Datenträger (Betriebssystem und Daten) gestempelt. Sie können die Verschlüsselungseinstellungen des Betriebssystemdatenträger bei Verschlüsselungen mit einem Durchlauf wie folgt erfassen:

``` powershell
$RGNAME = "RGNAME"
$VMNAME = "VMNAME"

$VM = Get-AzVM -Name ${VMNAME} -ResourceGroupName ${RGNAME}  
 $Sourcedisk = Get-AzDisk -ResourceGroupName ${RGNAME} -DiskName $VM.StorageProfile.OsDisk.Name
 Write-Host "============================================================================================================================================================="
 Write-Host "Encryption Settings:"
 Write-Host "============================================================================================================================================================="
 Write-Host "Enabled:" $Sourcedisk.EncryptionSettingsCollection.Enabled
 Write-Host "Version:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettingsVersion
 Write-Host "Source Vault:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SourceVault.Id
 Write-Host "Secret URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SecretUrl
 Write-Host "Key URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.KeyEncryptionKey.KeyUrl
 Write-Host "============================================================================================================================================================="
```
![Überprüfung über das Betriebssystem bei Einzeldurchlauf 01](./media/disk-encryption/verify-encryption-linux/verify-os-single-ps-001.png)

Wenn der Datenträger über keine gestempelten Verschlüsselungseinstellungen verfügt, ist die Ausgabe wie im Folgenden gezeigt leer:

![Verschlüsselungseinstellungen des Betriebssystems 2](./media/disk-encryption/verify-encryption-linux/os-encryption-settings-2.png)

Erfassen der Datenträgerverschlüsselungseinstellungen:

```azurepowershell
$RGNAME = "RGNAME"
$VMNAME = "VMNAME"

$VM = Get-AzVM -Name ${VMNAME} -ResourceGroupName ${RGNAME}
 clear
 foreach ($i in $VM.StorageProfile.DataDisks|ForEach-Object{$_.Name})
 {
 Write-Host "============================================================================================================================================================="
 Write-Host "Encryption Settings:"
 Write-Host "============================================================================================================================================================="
 Write-Host "Checking Disk:" $i
 $Disk=(Get-AzDisk -ResourceGroupName ${RGNAME} -DiskName $i)
 Write-Host "Encryption Enable: " $Sourcedisk.EncryptionSettingsCollection.Enabled
 Write-Host "Encryption KeyEncryptionKey: " $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.KeyEncryptionKey.KeyUrl;
 Write-Host "Encryption DiskEncryptionKey: " $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SecretUrl;
 Write-Host "============================================================================================================================================================="
 }
```
![Überprüfen der Datenträgerverschlüsselung mit einem Durchlauf in PowerShell 001](./media/disk-encryption/verify-encryption-linux/verify-data-single-ps-001.png)

### <a name="dual-pass"></a>Zwei Durchläufe
Bei zwei Durchläufen werden die Verschlüsselungseinstellungen im VM-Modell gestempelt und nicht auf jedem einzelnen Datenträger.

Sie können die folgenden Befehle verwenden, um zu überprüfen, ob die Verschlüsselungseinstellungen mit zwei Durchläufen gestempelt wurden:

```azurepowershell
$RGNAME = "RGNAME"
$VMNAME = "VMNAME"

$vm = Get-AzVm -ResourceGroupName ${RGNAME} -Name ${VMNAME};
$Sourcedisk = Get-AzDisk -ResourceGroupName ${RGNAME} -DiskName $VM.StorageProfile.OsDisk.Name
clear
Write-Host "============================================================================================================================================================="
Write-Host "Encryption Settings:"
Write-Host "============================================================================================================================================================="
Write-Host "Enabled:" $Sourcedisk.EncryptionSettingsCollection.Enabled
Write-Host "Version:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettingsVersion
Write-Host "Source Vault:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SourceVault.Id
Write-Host "Secret URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SecretUrl
Write-Host "Key URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.KeyEncryptionKey.KeyUrl
Write-Host "============================================================================================================================================================="
```
![Überprüfung bei zwei Durchläufen in PowerShell 1](./media/disk-encryption/verify-encryption-linux/verify-dual-ps-001.png)

### <a name="unattached-disks"></a>Nicht angefügte Datenträger

Überprüfen Sie die Verschlüsselungseinstellungen von Datenträgern, die nicht an eine VM angefügt sind.

### <a name="managed-disks"></a>Verwaltete Datenträger
```powershell
$Sourcedisk = Get-AzDisk -ResourceGroupName ${RGNAME} -DiskName ${TARGETDISKNAME}
Write-Host "============================================================================================================================================================="
Write-Host "Encryption Settings:"
Write-Host "============================================================================================================================================================="
Write-Host "Enabled:" $Sourcedisk.EncryptionSettingsCollection.Enabled
Write-Host "Version:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettingsVersion
Write-Host "Source Vault:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SourceVault.Id
Write-Host "Secret URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.DiskEncryptionKey.SecretUrl
Write-Host "Key URL:" $Sourcedisk.EncryptionSettingsCollection.EncryptionSettings.KeyEncryptionKey.KeyUrl
Write-Host "============================================================================================================================================================="
```
## <a name="using-az-cli"></a>Verwenden der Azure CLI

Sie können den **allgemeinen** Verschlüsselungsstatus einer verschlüsselten VM mithilfe der folgenden Azure CLI-Befehle überprüfen:

```bash
VMNAME="VMNAME"
RGNAME="RGNAME"
az vm encryption show --name ${VMNAME} --resource-group ${RGNAME} --query "substatus"
```
![Allgemeine Überprüfung mit der CLI ](./media/disk-encryption/verify-encryption-linux/verify-gen-cli.png)

### <a name="single-pass"></a>Einzeldurchlauf
Sie können die Verschlüsselungseinstellungen der einzelnen Datenträger mithilfe der folgenden Azure CLI-Befehle überprüfen:

```bash
az vm encryption show -g ${RGNAME} -n ${VMNAME} --query "disks[*].[name, statuses[*].displayStatus]"  -o table
```

![Datenverschlüsselungseinstellungen](./media/disk-encryption/verify-encryption-linux/data-encryption-settings-2.png)

>[!IMPORTANT]
> Wenn der Datenträger über keine gestempelten Verschlüsselungseinstellungen verfügt, wird „Disk is not encrypted“ (Der Datenträger ist nicht verschlüsselt) angezeigt.

Detaillierter Status und Verschlüsselungseinstellungen:

Betriebssystemdatenträger:

```bash
RGNAME="RGNAME"
VMNAME="VNAME"

disk=`az vm show -g ${RGNAME} -n ${VMNAME} --query storageProfile.osDisk.name -o tsv`
for disk in $disk; do \
echo "============================================================================================================================================================="
echo -ne "Disk Name: "; az disk show -g ${RGNAME} -n ${disk} --query name -o tsv; \
echo -ne "Encryption Enabled: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.enabled -o tsv; \
echo -ne "Version: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettingsVersion -o tsv; \
echo -ne "Disk Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].diskEncryptionKey.secretUrl -o tsv; \
echo -ne "key Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].keyEncryptionKey.keyUrl -o tsv; \
echo "============================================================================================================================================================="
done
```

![OSSingleCLI](./media/disk-encryption/verify-encryption-linux/os-single-cli.png)

Datenträger:

```bash
RGNAME="RGNAME"
VMNAME="VMNAME"
az vm encryption show --name ${VMNAME} --resource-group ${RGNAME} --query "substatus"

for disk in `az vm show -g ${RGNAME} -n ${VMNAME} --query storageProfile.dataDisks[].name -o tsv`; do \
echo "============================================================================================================================================================="; \
echo -ne "Disk Name: "; az disk show -g ${RGNAME} -n ${disk} --query name -o tsv; \
echo -ne "Encryption Enabled: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.enabled -o tsv; \
echo -ne "Version: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettingsVersion -o tsv; \
echo -ne "Disk Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].diskEncryptionKey.secretUrl -o tsv; \
echo -ne "key Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].keyEncryptionKey.keyUrl -o tsv; \
echo "============================================================================================================================================================="
done
```

![Datenträgerüberprüfung bei einem Durchlauf über die CLI ](./media/disk-encryption/verify-encryption-linux/data-single-cli.png)

### <a name="dual-pass"></a>Zwei Durchläufe

``` bash
az vm encryption show --name ${VMNAME} --resource-group ${RGNAME} -o table
```

![Allgemeine Überprüfung bei zwei Durchläufen über die CLI](./media/disk-encryption/verify-encryption-linux/verify-gen-dual-cli.png) Sie können die Verschlüsselungseinstellungen auch im Speicherprofil des VM-Modells auf dem Betriebssystemdatenträger überprüfen:

```bash
disk=`az vm show -g ${RGNAME} -n ${VMNAME} --query storageProfile.osDisk.name -o tsv`
for disk in $disk; do \
echo "============================================================================================================================================================="; \
echo -ne "Disk Name: "; az disk show -g ${RGNAME} -n ${disk} --query name -o tsv; \
echo -ne "Encryption Enabled: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.enabled -o tsv; \
echo -ne "Version: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettingsVersion -o tsv; \
echo -ne "Disk Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].diskEncryptionKey.secretUrl -o tsv; \
echo -ne "key Encryption Key: "; az disk show -g ${RGNAME} -n ${disk} --query encryptionSettingsCollection.encryptionSettings[].keyEncryptionKey.keyUrl -o tsv; \
echo "============================================================================================================================================================="
done
```

![Überprüfung des VM-Profils bei zwei Durchläufen über die CLI ](./media/disk-encryption/verify-encryption-linux/verify-vm-profile-dual-cli.png)

### <a name="unattached-disks"></a>Nicht angefügte Datenträger

Überprüfen Sie die Verschlüsselungseinstellungen von Datenträgern, die nicht an eine VM angefügt sind.

### <a name="managed-disks"></a>Verwaltete Datenträger

```bash
RGNAME="RGNAME"
TARGETDISKNAME="DISKNAME"
echo "============================================================================================================================================================="
echo -ne "Disk Name: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query name -o tsv; \
echo -ne "Encryption Enabled: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.enabled -o tsv; \
echo -ne "Version: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettingsVersion -o tsv; \
echo -ne "Disk Encryption Key: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettings[].diskEncryptionKey.secretUrl -o tsv; \
echo -ne "key Encryption Key: "; az disk show -g ${RGNAME} -n ${TARGETDISKNAME} --query encryptionSettingsCollection.encryptionSettings[].keyEncryptionKey.keyUrl -o tsv; \
echo "============================================================================================================================================================="
```
### <a name="unmanaged-disks"></a>Nicht verwaltete Datenträger

Nicht verwaltete Datenträger sind VHD-Dateien, die als Seitenblobs in Azure-Speicherkonten gespeichert sind.

Sie müssen Folgendes angeben, um die Details zu einem spezifischen Datenträger abzurufen:

Die ID des Speicherkontos, das den Datenträger enthält
Eine Verbindungszeichenfolge für das spezifische Speicherkonto
Der Name des Containers, in dem der Datenträger gespeichert wird
Der Name des Datenträgers

Mit dem folgenden Befehl werden alle IDs für all Ihre Speicherkonten aufgeführt:

```bash
az storage account list --query [].[id] -o tsv
```
Die Speicherkonto-IDs werden im folgenden Format aufgeführt:

/subscriptions/\<Abonnement-ID>/resourceGroups/\<Name der Ressourcengruppe>/providers/Microsoft.Storage/storageAccounts/\<Name des Speicherkontos>

Wählen Sie die entsprechende ID aus, und speichern Sie sie in einer Variable:
```bash
id="/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name>"
```
Verbindungszeichenfolge.

Mit dem folgenden Befehl wird die Verbindungszeichenfolge für ein bestimmtes Speicherkonto abgerufen und in einer Variable gespeichert:

```bash
ConnectionString=$(az storage account show-connection-string --ids $id --query connectionString -o tsv)
```

Der Containername

Mit dem folgenden Befehl werden alle Container in einem Speicherkonto aufgeführt:
```bash
az storage container list --connection-string $ConnectionString --query [].[name] -o tsv
```
Der Container, der für Datenträger verwendet wird, trägt normalerweise den Namen „vhds“.

Speichern Sie den Containernamen in einer Variable: 
```bash
ContainerName="name of the container"
```

Der Name des Datenträgers

Verwenden Sie den folgenden Befehl, um alle Blobs eines bestimmten Containers aufzuführen:
```bash 
az storage blob list -c ${ContainerName} --connection-string $ConnectionString --query [].[name] -o tsv
```
Wählen Sie den abzufragenden Datenträger aus, und speichern Sie seinen Namen in einer Variable.
```bash
DiskName="diskname.vhd"
```
Fragen Sie die Datenträgerverschlüsselungseinstellungen ab:
```bash
az storage blob show -c ${ContainerName} --connection-string ${ConnectionString} -n ${DiskName} --query metadata.DiskEncryptionSettings
```

## <a name="from-the-os"></a>Methode zur Überprüfung über das Betriebssystem
Überprüfen Sie, ob die Datenträgerpartitionen verschlüsselt sind (und der Betriebssystemdatenträger nicht).

Wenn eine Partition bzw. ein Datenträger verschlüsselt ist, wird er mit dem Typ **crypt** angezeigt. Wenn keine Verschlüsselung vorliegt, wird der Typ **part/disk** angezeigt.

``` bash
lsblk
```

![Verschlüsselte Betriebssystemebene ](./media/disk-encryption/verify-encryption-linux/verify-os-crypt-layer.png)

Mit der folgenden Variante des lsblk-Befehls können Sie weitere Details abrufen. 

Daraufhin wird die Ebene des Typs **crypt** angezeigt, die von der Erweiterung eingebunden wird.

Im folgenden Beispiel werden logische Volumes und normale Datenträger mit „**crypto\_LUKS FSTYPE**“ veranschaulicht.

```bash
lsblk -o NAME,TYPE,FSTYPE,LABEL,SIZE,RO,MOUNTPOINT
```
![Verschlüsselte Betriebssystemebene 2](./media/disk-encryption/verify-encryption-linux/verify-os-crypt-layer-2.png)

Sie können einen zusätzlichen Schritt ausführen, indem Sie auch überprüfen, ob der Datenträger über geladene Schlüssel verfügt.

``` bash
cryptsetup luksDump /dev/VGNAME/LVNAME
```

``` bash
cryptsetup luksDump /dev/sdd1
```

Außerdem können Sie überprüfen, welche DM-Geräte als verschlüsselt aufgeführt werden.

```bash
dmsetup ls --target crypt
```

## <a name="next-steps"></a>Nächste Schritte

- [Azure Disk Encryption-Problembehandlung](disk-encryption-troubleshooting.md)
