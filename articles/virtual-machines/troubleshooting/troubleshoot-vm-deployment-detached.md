---
title: Troubleshooting bei der Bereitstellung virtueller Computer aufgrund abgetrennter Datenträger | Microsoft-Dokumentation
description: Troubleshooting bei der Bereitstellung virtueller Computer aufgrund abgetrennter Datenträger
services: virtual-machines-windows
documentationCenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/31/2019
ms.author: vaaga
ms.openlocfilehash: e049a2b914cbf9c4f0ca0f3a1dd0281d58f881b2
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75477088"
---
# <a name="troubleshoot-virtual-machine-deployment-due-to-detached-disks"></a>Troubleshooting bei der Bereitstellung virtueller Computer aufgrund abgetrennter Datenträger

## <a name="symptom"></a>Symptom

Wenn Sie versuchen, einen virtuellen Computer zu aktualisieren, bei dem ein Fehler beim Trennen des Datenträgers aufgetreten ist, kann dieser Fehlercode auftreten.

```
Code=\"AttachDiskWhileBeingDetached\" 
Message=\"Cannot attach data disk '{disk ID}' to virtual machine '{vmName}' because the disk is currently being detached or the last detach  operation failed. Please wait until the disk is completely detached, and then try again or delete/detach the disk explicitly again\” 
```

## <a name="cause"></a>Ursache

Dieser Fehler tritt auf, wenn Sie versuchen, einen Datenträger mit fehlerhaftem letztem Trennvorgang erneut anzufügen. Die beste Möglichkeit, diesen Status zu beenden, ist das Trennen des fehlerhaften Datenträgers.

## <a name="solution-1-powershell"></a>Lösung 1: PowerShell

### <a name="step-1-get-the-virtual-machine-and-disk-details"></a>Schritt 1: Abrufen der Details zum virtuellen Computer und zum Datenträger

```azurepowershell-interactive
PS D:> $vm = Get-AzureRmVM -ResourceGroupName "Example Resource Group" -Name "ERGVM999999" 

PS D:> $vm.StorageProfile.DataDisks 
lun          : 0
name         : f94901ef-75ee-4477-9ad6-1c74da50e7ef
createOption : Attach
caching      : ReadOnly
managedDisk  : @{storageAccountType=Premium_LRS; id=/subscriptions/<subscription ID>/resourceGroups/<Resource Group name>}
diskSizeGB   : 8
toBeDetached : False 
```

### <a name="step-2-set-the-flag-for-failing-disks-to-true"></a>Schritt 2: Legen Sie das Flag für fehlerhafte Datenträger auf TRUE fest.

Rufen Sie den Arrayindex des fehlerhaften Datenträgers ab, und legen Sie das Flag **toBeDetached** für den fehlerhaften Datenträger (für den der Fehler **AttachDiskWhileBeingDetached** aufgetreten ist) auf TRUE fest. Diese Einstellung impliziert das Trennen des Datenträgers vom virtuellen Computer. Den Namen des fehlerhaften Datenträgers finden Sie in der **errorMessage**.

> !Hinweis: Die für Get- und Put-Aufrufe angegebene API-Version muss mindestens 2019-03-01 sein.

```azurepowershell-interactive
PS D:> $vm.StorageProfile.DataDisks[0].ToBeDetached = $true 
```

Alternativ können Sie diesen Datenträger auch mit dem folgenden Befehl trennen. Dies ist für Benutzer hilfreich, die API-Versionen vor dem 1. März 2019 verwenden.

```azurepowershell-interactive
PS D:> Remove-AzureRmVMDataDisk -VM $vm -Name "<disk ID>" 
```

### <a name="step-3-update-the-virtual-machine"></a>Schritt 3: Aktualisieren virtueller Computer

```azurepowershell-interactive
PS D:> Update-AzureRmVM -ResourceGroupName "Example Resource Group" -VM $vm 
```

## <a name="solution-2-rest"></a>Lösung 2: REST

### <a name="step-1-get-the-virtual-machine-payload"></a>Schritt 1: Rufen Sie die VM-Nutzlast ab.

```azurepowershell-interactive
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?$expand=instanceView&api-version=2019-03-01
```

### <a name="step-2-set-the-flag-for-failing-disks-to-true"></a>Schritt 2: Legen Sie das Flag für fehlerhafte Datenträger auf TRUE fest.

Legen Sie in der in Schritt 1 zurückgegebenen Nutzlast das Flag **toBeDetached** für den fehlerhaften Datenträger auf TRUE fest. Bitte beachten: Die für Get- und Put-Aufrufe angegebene API-Version muss mindestens `2019-03-01` sein.

**Beispiel für den Inhalt der Anforderung**

```azurepowershell-interactive
{
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_D2_v2"
    },
    "storageProfile": {
      "imageReference": {
        "sku": "2016-Datacenter",
        "publisher": "MicrosoftWindowsServer",
        "version": "latest",
        "offer": "WindowsServer"
      },
      "osDisk": {
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Standard_LRS"
        },
        "name": "myVMosdisk",
        "createOption": "FromImage"
      },
      "dataDisks": [
        {
          "diskSizeGB": 1023,
          "createOption": "Empty",
        "name": "f94901ef-75ee-4477-9ad6-1c74da50e7ef",
          "lun": 0,
          "toBeDetached": true
        },
        {
          "diskSizeGB": 1023,
          "createOption": "Empty",
          "lun": 1,
          "toBeDetached": false
        }
      ]
    },
    "osProfile": {
      "adminUsername": "{your-username}",
      "computerName": "myVM",
      "adminPassword": "{your-password}"
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/{existing-nic-name}",
          "properties": {
            "primary": true
          }
        }
      ]
    }
  }
}
```

Alternativ können Sie auch den fehlerhaften Datenträger aus der obigen Nutzlast entfernen. Dies ist für Benutzer hilfreich, die API-Versionen vor dem 1. März 2019 verwenden.

### <a name="step-3-update-the-virtual-machine"></a>Schritt 3: Aktualisieren virtueller Computer

Verwenden Sie die in Schritt 2 festgelegte Nutzlast für den Anforderungstext, und aktualisieren Sie den virtuellen Computer wie folgt:

```azurepowershell-interactive
PATCH https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2019-03-01
```

**Beispielantwort:**

```azurepowershell-interactive
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "type": "Microsoft.Compute/virtualMachines",
  "properties": {
    "osProfile": {
      "adminUsername": "{your-username}",
      "secrets": [],
      "computerName": "myVM",
      "windowsConfiguration": {
        "provisionVMAgent": true,
        "enableAutomaticUpdates": true
      }
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/nsgExistingNic",
          "properties": {
            "primary": true
          }
        }
      ]
    },
    "storageProfile": {
      "imageReference": {
        "sku": "2016-Datacenter",
        "publisher": "MicrosoftWindowsServer",
        "version": "latest",
        "offer": "WindowsServer"
      },
      "osDisk": {
        "osType": "Windows",
        "caching": "ReadWrite",
        "createOption": "FromImage",
        "name": "myVMosdisk",
        "managedDisk": {
          "storageAccountType": "Standard_LRS"
        }
      },
      "dataDisks": [
        {
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Standard_LRS"
          },
          "createOption": "Empty",
          "lun": 0,
          "diskSizeGB": 1023,
        "name": "f94901ef-75ee-4477-9ad6-1c74da50e7ef",
          "toBeDetached": true
        },
        {
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Standard_LRS"
          },
          "createOption": "Empty",
          "lun": 1,
          "diskSizeGB": 1023,
          "toBeDetached": false
        }
      ]
    },
    "vmId": "3906fef9-a1e5-4b83-a8a8-540858b41df0",
    "hardwareProfile": {
      "vmSize": "Standard_D2_v2"
    },
    "provisioningState": "Updating"
  },
  "name": "myVM",
  "location": "westus"
}
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Probleme beim Herstellen einer Verbindung mit Ihrer VM auftreten, helfen Ihnen die Informationen unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](troubleshoot-rdp-connection.md) weiter.

Konsultieren Sie [Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird](troubleshoot-app-connection.md) bei Problemen mit dem Zugriff auf Anwendungen, die auf Ihrer VM ausgeführt werden.
