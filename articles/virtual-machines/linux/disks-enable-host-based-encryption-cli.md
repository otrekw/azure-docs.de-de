---
title: Aktivieren der End-to-End-Verschlüsselung mit Verschlüsselung auf dem Host – Azure CLI – verwaltete Datenträger
description: Verwenden Sie Verschlüsselung auf dem Host, um die End-to-End-Verschlüsselung auf Ihren von Azure verwalteten Datenträgern zu aktivieren.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 07/10/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 705f9f3055d40d23c9ec5e24cfccfc0c96e114a5
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86235688"
---
# <a name="enable-end-to-end-encryption-using-encryption-at-host---azure-cli"></a>Aktivieren der End-to-End-Verschlüsselung mit Verschlüsselung auf dem Host – Azure CLI

Wenn Sie die Verschlüsselung auf dem Host aktivieren, werden die auf dem VM-Host gespeicherten Daten ruhend verschlüsselt und verschlüsselt an den Speicherdienst übermittelt. Informationen zum Konzept der Verschlüsselung auf dem Host sowie zu anderen Verschlüsselungstypen für verwaltete Datenträger finden Sie unter [Verschlüsselung auf dem Host: End-to-End-Verschlüsselung für Ihre VM-Daten](disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data).

## <a name="restrictions"></a>Beschränkungen

[!INCLUDE [virtual-machines-disks-encryption-at-host-restrictions](../../../includes/virtual-machines-disks-encryption-at-host-restrictions.md)]

### <a name="supported-regions"></a>Unterstützte Regionen

[!INCLUDE [virtual-machines-disks-encryption-at-host-regions](../../../includes/virtual-machines-disks-encryption-at-host-regions.md)]

### <a name="supported-vm-sizes"></a>Unterstützte VM-Größen

[!INCLUDE [virtual-machines-disks-encryption-at-host-suported-sizes](../../../includes/virtual-machines-disks-encryption-at-host-suported-sizes.md)]

Sie können die VM-Größen auch programmgesteuert ermitteln. Informationen dazu, wie Sie sie programmgesteuert abrufen können, finden Sie im Abschnitt [Ermitteln der unterstützten VM-Größen](#finding-supported-vm-sizes).

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen das Feature für Ihr Abonnement aktivieren lassen, um die Verschlüsselung auf dem Host für Ihre virtuellen Computer oder VM-Skalierungsgruppen verwenden zu können. Senden Sie eine E-Mail mit Ihren Abonnement-IDs an encryptionAtHost@microsoft.com, um das Feature für Ihre Abonnements aktivieren zu lassen.

### <a name="create-an-azure-key-vault-and-diskencryptionset"></a>Erstellen einer Azure Key Vault-Instanz und einer DiskEncryptionSet-Klasse

Nach dem Aktivieren des Features müssen Sie eine Azure Key Vault-Instanz und eine DiskEncryptionSet-Klasse einrichten, sofern noch nicht geschehen.

[!INCLUDE [virtual-machines-disks-encryption-create-key-vault-cli](../../../includes/virtual-machines-disks-encryption-create-key-vault-cli.md)]

## <a name="enable-encryption-at-host-for-disks-attached-to-vm-and-virtual-machine-scale-sets"></a>Aktivieren der Verschlüsselung auf dem Host für an VMs und VM-Skalierungsgruppen angefügte Datenträger

Sie können die Verschlüsselung auf dem Host aktivieren, indem Sie mit der API-Version **2020-06-01** und höher eine neue Eigenschaft „EncryptionAtHost“ in den Sicherheitsprofilen (securityProfile) von VMs oder VM-Skalierungsgruppen festlegen.

`"securityProfile": { "encryptionAtHost": "true" }`

## <a name="example-scripts"></a>Beispielskripts

### <a name="enable-encryption-at-host-for-disks-attached-to-a-vm-with-customer-managed-keys"></a>Aktivieren der Verschlüsselung auf dem Host für Datenträger, die mit kundenseitig verwalteten Schlüsseln an eine VM angefügt sind

Erstellen Sie eine VM mit verwalteten Datenträgern mithilfe des Ressourcen-URIs der zuvor erstellten DiskEncryptionSet-Klasse.

Ersetzen Sie `<yourPassword>`, `<yourVMName>`, `<yourVMSize>`, `<yourDESName>`, `<yoursubscriptionID>`, `<yourResourceGroupName>` und `<yourRegion>`, und führen Sie dann das Skript aus.

```azurecli
az group deployment create -g <yourResourceGroupName> \
--template-uri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/EncryptionAtHost/CreateVMWithDisksEncryptedAtHostWithCMK.json" \
--parameters "virtualMachineName=<yourVMName>" "adminPassword=<yourPassword>" "vmSize=<yourVMSize>" "diskEncryptionSetId=/subscriptions/<yoursubscriptionID>/resourceGroups/<yourResourceGroupName>/providers/Microsoft.Compute/diskEncryptionSets/<yourDESName>" "region=<yourRegion>"
```

### <a name="enable-encryption-at-host-for-disks-attached-to-a-vm-with-platform-managed-keys"></a>Aktivieren der Verschlüsselung auf dem Host für Datenträger, die mit plattformseitig verwalteten Schlüsseln an eine VM angefügt sind

Ersetzen Sie `<yourPassword>`, `<yourVMName>`, `<yourVMSize>`, `<yourResourceGroupName>` und `<yourRegion>`, und führen Sie dann das Skript aus.

```azurecli
az group deployment create -g <yourResourceGroupName> \
--template-uri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/EncryptionAtHost/CreateVMWithDisksEncryptedAtHostWithPMK.json" \
--parameters "virtualMachineName=<yourVMName>" "adminPassword=<yourPassword>" "vmSize=<yourVMSize>" "region=<yourRegion>"
```

## <a name="finding-supported-vm-sizes"></a>Ermitteln der unterstützten VM-Größen

Ältere VM-Größen werden nicht unterstützt. Die Liste der unterstützten VM-Größen finden Sie folgendermaßen:

Rufen Sie die [Resource Skus-API](https://docs.microsoft.com/rest/api/compute/resourceskus/list) auf, und stellen Sie sicher, dass die `EncryptionAtHostSupported`-Funktion auf **True** festgelegt ist.

```json
    {
        "resourceType": "virtualMachines",
        "name": "Standard_DS1_v2",
        "tier": "Standard",
        "size": "DS1_v2",
        "family": "standardDSv2Family",
        "locations": [
        "CentralUSEUAP"
        ],
        "capabilities": [
        {
            "name": "EncryptionAtHostSupported",
            "value": "True"
        }
        ]
    }
```

Alternativ können Sie auch das PowerShell-Cmdlet [Get-AzComputeResourceSku](https://docs.microsoft.com/powershell/module/az.compute/get-azcomputeresourcesku?view=azps-3.8.0) aufrufen.

```powershell
$vmSizes=Get-AzComputeResourceSku | where{$_.ResourceType -eq 'virtualMachines' -and $_.Locations.Contains('CentralUSEUAP')} 

foreach($vmSize in $vmSizes)
{
    foreach($capability in $vmSize.capabilities)
    {
        if($capability.Name -eq 'EncryptionAtHostSupported' -and $capability.Value -eq 'true')
        {
            $vmSize

        }

    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie diese Ressourcen erstellt und konfiguriert haben, können Sie diese zum Sichern Ihrer verwalteten Datenträger verwenden. Der folgende Link enthält Beispielskripts mit jeweils einem entsprechenden Szenario, die Sie zum Sichern Ihrer verwalteten Datenträger verwenden können.

[Beispiele für Azure Resource Manager-Vorlagen](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/EncryptionAtHost)
