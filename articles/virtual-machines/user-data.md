---
title: Benutzerdaten für Azure Virtual Machines
description: Ermöglichen Sie Kunden, zur Bereitstellungszeit Skripts oder andere Metadaten in eine Azure-VM einzufügen.
services: virtual-machines-linux
author: ningk
manager: rogardle
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 04/30/2021
ms.author: ningk
ms.reviewer: azmetadata
ms.openlocfilehash: d149ead5cd45b55c846852f92342e6bbc56ba3cf
ms.sourcegitcommit: 19dfdfa85e92c6a34933bdd54a7c94e8b00eacfd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2021
ms.locfileid: "109665310"
---
# <a name="user-data-for-azure-virtual-machine"></a>Benutzerdaten für Azure Virtual Machines 

Benutzerdaten ermöglichen Ihnen, eigene Skripts oder Metadaten an Ihre VM zu übergeben.

## <a name="what-is-user-data"></a>Was sind „Benutzerdaten“?

Benutzerdaten sind Skripts oder andere Metadaten, die zur Bereitstellungszeit in eine Azure-VM eingefügt werden. Jede Anwendung auf der VM kann nach der Bereitstellung über Azure Instance Metadata Service (IMDS) auf die Benutzerdaten zugreifen. 

Benutzerdaten sind eine neue Version von [benutzerdefinierten Daten](./custom-data.md) mit zusätzlichen Vorteilen:

* Benutzerdaten können nach der Bereitstellung über Azure Instance Metadata Service (IMDS) abgerufen werden.

* Benutzerdaten sind persistent. Sie bleiben während der Lebensdauer der VM verfügbar.

* Benutzerdaten können von außerhalb der VM aktualisiert werden, ohne diese zu beenden oder neu zu starten.

* Benutzerdaten können über die GET-VM-/VMSS-API mit der Option $expand abgefragt werden.

 Wenn Benutzerdaten nicht während der Bereitstellung hinzugefügt werden, können sie auch danach noch hinzugefügt werden.

**Sicherheitswarnung**

> [!WARNING]
> Benutzerdaten werden nicht verschlüsselt, und jeder Prozess auf der VM kann diese Daten abfragen. Sie sollten keine vertraulichen Informationen in Benutzerdaten speichern.

Stellen Sie sicher, dass Sie die neueste Azure Resource Manager-API nutzen, um die neuen Benutzerdatenfeatures zu verwenden. Der Inhalt sollte Base64-codiert werden, bevor er an die API übergeben wird. Die Dateigröße darf 64 KB nicht überschreiten.

## <a name="create-user-data-for-azure-vmvmss"></a>Erstellen von Benutzerdaten für Azure-VMs/-VMSS

**Hinzufügen von Benutzerdaten beim Erstellen einer neuen VM**

Verwenden Sie [diese Azure Resource Manager-Vorlage](https://aka.ms/ImdsUserDataArmTemplate), um eine neue VM mit Benutzerdaten zu erstellen.
Wenn Sie die REST-API verwenden, fügen Sie bei einzelnen VMs im Abschnitt „properties“ den Eintrag „UserData“ mithilfe einer PUT-Anforderung zum Erstellen der VM hinzu.

```json
{
  "name": "testVM",
  "location": "West US",
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_A1"
    },
    "storageProfile": {
      "osDisk": {
        "osType": "Windows",
        "name": "osDisk",
        "createOption": "Attach",
        "vhd": {
          "uri": "http://myaccount.blob.core.windows.net/container/directory/blob.vhd"
        }
      }
    },
    "userData": "c2FtcGxlIHVzZXJEYXRh",
    "networkProfile": { "networkInterfaces" : [ { "name" : "nic1" } ] },
  }
}
```

**Hinzufügen von Benutzerdaten beim Erstellen einer neuen VM-Skalierungsgruppe**

Wenn Sie die REST-API verwenden, fügen Sie beim Erstellen der VM-Skalierungsgruppe im Abschnitt „virtualMachineProfile“ den Eintrag „UserData“ mit einer PUT-Anforderung hinzu.
```json
{
  "location": "West US",
  "sku": {
    "name": "Standard_A1",
    "capacity": 1
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
      "userData": "VXNlckRhdGE=",
      "osProfile": {
        "computerNamePrefix": "TestVM",
        "adminUsername": "TestUserName",
        "windowsConfiguration": {
          "provisionVMAgent": true,
          "timeZone": "Dateline Standard Time"
        }
      },
      "storageProfile": {
        "osDisk": {
          "createOption": "FromImage",
          "caching": "ReadOnly"
        },
        "imageReference": {
          "publisher": "publisher",
          "offer": "offer",
          "sku": "sku",
          "version": "1.2.3"
        }
      },
      "networkProfile": {"networkInterfaceConfigurations":[{"name":"nicconfig1","properties":{"ipConfigurations":[{"name":"ip1","properties":{"subnet":{"id":"vmssSubnet0"}}}]}}]},
      "diagnosticsProfile": {
        "bootDiagnostics": {
          "enabled": true,
          "storageUri": "https://crputest.blob.core.windows.net"
        }
      }
    },
    "provisioningState": 0,
    "overprovision": false,
    "uniqueId": "00000000-0000-0000-0000-000000000000"
  }
}
```


## <a name="retrieving-user-data"></a>Abrufen von Benutzerdaten

Anwendungen, die auf der VM ausgeführt werden, können Benutzerdaten über den IMDS-Endpunkt abrufen. Weitere Informationen finden Sie [hier im IMDS-Beispielcode](./linux/instance-metadata-service.md?tabs=linux#get-user-data).

Kunden können den vorhandenen Wert von Benutzerdaten über die REST-API mithilfe des Endpunkts „\$expand=userData“ abrufen (der Anforderungstext kann leer gelassen werden).

Einzelne VMs:

`GET "/subscriptions/{guid}/resourceGroups/{RGName}/providers/Microsoft.Compute/virtualMachines/{VMName}?$expand=userData"`

VM-Skalierungsgruppe:

`GET "/subscriptions/{guid}/resourceGroups/{RGName}/providers/Microsoft.Compute/virtualMachineScaleSets/{VMSSName}?$expand=userData"`

VM in VM-Skalierungsgruppe:

` GET "/subscriptions/{guid}/resourceGroups/{RGName}/providers/Microsoft.Compute/virtualMachineScaleSets/{VMSSName}/virtualmachines/{vmss instance id}?$expand=userData" `

## <a name="updating-user-data"></a>Aktualisieren von Benutzerdaten

Sie können mit der REST-API eine normale PUT- oder PATCH-Anforderung verwenden, um die Benutzerdaten zu aktualisieren. Die Benutzerdaten werden aktualisiert, ohne dass die VM beendet oder neu gestartet werden muss.

`PUT
"/subscriptions/{guid}/resourceGroups/{RGName}/providers/Microsoft.Compute/ virtualMachines/{VMName}
`

`PATCH
"/subscriptions/{guid}/resourceGroups/{RGName}/providers/Microsoft.Compute/ virtualMachines/{VMName}
`

In dieser Anforderung sollte „VM.Properties“ das gewünschte Feld „UserData“ wie folgt enthalten:

```json
"properties": {
        "hardwareProfile": {
          "vmSize": "Standard_D1_v2"
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
            "name": "vmOSdisk",
            "createOption": "FromImage"
          }
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
        },
        "osProfile": {
          "adminUsername": "{your-username}",
          "computerName": "{vm-name}",
          "adminPassword": "{your-password}"
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
            "storageUri": "http://{existing-storage-account-name}.blob.core.windows.net",
            "enabled": true
          }
        },
        "userData": "U29tZSBDdXN0b20gRGF0YQ=="
      } 
```
> [!NOTE]
> Wenn Sie in diesem Fall eine leere Zeichenfolge für „UserData“ übergeben, werden die Benutzerdaten gelöscht.

## <a name="user-data-and-custom-data"></a>Benutzerdaten und benutzerdefinierte Daten

Benutzerdefinierte Daten funktionieren weiterhin wie bisher. Beachten Sie jedoch, dass Sie benutzerdefinierte Daten nicht über IMDS abrufen können. 

## <a name="adding-user-data-to-an-existing-vm"></a>Hinzufügen von Benutzerdaten zu einer vorhandenen VM 

Wenn Sie über eine VM/VMSS ohne Benutzerdaten verfügen, können Sie dieser VM immer noch Benutzerdaten hinzufügen, indem Sie die Aktualisierungsbefehle wie im Abschnitt [Aktualisieren der Benutzerdaten](#updating-user-data) beschrieben verwenden. Stellen Sie sicher, dass Sie ein Upgrade auf die neueste Version der Azure Resource Manager-API durchführen.

## <a name="next-steps"></a>Nächste Schritte 
 
Probieren Sie den [Azure Instance Metadata Service](./linux/instance-metadata-service.md) aus, und erfahren Sie, wie Sie die Metadaten und Benutzerdaten der VM-Instanz über deren Endpunkt abrufen. 
