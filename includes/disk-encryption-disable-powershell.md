---
title: Datei einfügen
description: Datei einfügen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/19/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 517bde8d365793906c24d0f242d4adc1b6241e7c
ms.sourcegitcommit: 8651d19fca8c5f709cbb22bfcbe2fd4a1c8e429f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112083747"
---
Sie können die Verschlüsselung mit Azure PowerShell, der Azure CLI oder einer Resource Manager-Vorlage deaktivieren. Das Deaktivieren der Datenträgerverschlüsselung auf einem virtuellen Windows-Computer funktioniert nicht wie erwartet, wenn sowohl das Betriebssystem als auch Datenträger verschlüsselt wurden. Deaktivieren Sie stattdessen die Verschlüsselung auf allen Datenträgern.

- **Deaktivieren der Datenträgerverschlüsselung mit Azure PowerShell:** Verwenden Sie das Cmdlet [Disable-AzVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption), um die Verschlüsselung zu deaktivieren. 
     ```azurepowershell-interactive
     Disable-AzVMDiskEncryption -ResourceGroupName 'MyVirtualMachineResourceGroup' -VMName 'MySecureVM' -VolumeType "all"
     ```

- **Deaktivieren der Verschlüsselung mit der Azure CLI:** Verwenden Sie den Befehl [az vm encryption disable](/cli/azure/vm/encryption#az_vm_encryption_disable), um die Verschlüsselung zu deaktivieren. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MyVirtualMachineResourceGroup" --volume-type "all"
     ```
- **Deaktivieren der Verschlüsselung mit einer Resource Manager-Vorlage:** 

    1. Klicken Sie in der Vorlage unter [Disable disk encryption on running Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.compute/decrypt-running-windows-vm-without-aad) (Deaktivieren der Datenträgerverschlüsselung auf einer ausgeführten Windows-VM) auf **Deploy to Azure** (In Azure bereitstellen).
    2. Wählen Sie das Abonnement, die Ressourcengruppe, den Standort, die VM, den Volumetyp, die rechtlichen Bedingungen und die Vereinbarung aus.
    3.  Klicken Sie auf **Kaufen**, um die Datenträgerverschlüsselung auf einer ausgeführten Windows-VM zu deaktivieren. 
