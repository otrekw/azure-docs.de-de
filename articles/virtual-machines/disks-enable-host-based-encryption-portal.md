---
title: Aktivieren der End-to-End-Verschlüsselung mit Verschlüsselung auf dem Host – Azure-Portal – verwaltete Datenträger
description: Verwenden Sie Verschlüsselung auf dem Host, um die End-to-End-Verschlüsselung auf Ihren von Azure verwalteten Datenträgern zu aktivieren – Azure-Portal.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 08/24/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: cdb22805e2e68893d3883272b66c2cfac13c807e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104721867"
---
# <a name="use-the-azure-portal-to-enable-end-to-end-encryption-using-encryption-at-host"></a>Verwenden des Azure-Portals zum Aktivieren der End-to-End-Verschlüsselung mit Verschlüsselung auf dem Host

Wenn Sie die Verschlüsselung auf dem Host aktivieren, werden die auf dem VM-Host gespeicherten Daten ruhend verschlüsselt und verschlüsselt an den Speicherdienst übermittelt. Informationen zum Konzept der Verschlüsselung auf dem Host sowie zu anderen Verschlüsselungstypen für verwaltete Datenträger finden Sie in folgenden Artikeln:

* Linux: [Verschlüsselung auf dem Host: End-to-End-Verschlüsselung für Ihre VM-Daten](./disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data)

* Windows: [Verschlüsselung auf dem Host: End-to-End-Verschlüsselung für Ihre VM-Daten](./disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data)

## <a name="restrictions"></a>Beschränkungen

[!INCLUDE [virtual-machines-disks-encryption-at-host-restrictions](../../includes/virtual-machines-disks-encryption-at-host-restrictions.md)]


### <a name="supported-vm-sizes"></a>Unterstützte VM-Größen

[!INCLUDE [virtual-machines-disks-encryption-at-host-suported-sizes](../../includes/virtual-machines-disks-encryption-at-host-suported-sizes.md)]

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen das Feature für Ihr Abonnement aktivieren, bevor Sie die EncryptionAtHost-Eigenschaft für Ihre VM/VMSS verwenden. Führen Sie die folgenden Schritte aus, um das Feature für Ihr Abonnement zu aktivieren:

1. **Azure-Portal**: Klicken Sie im [Azure-Portal](https://portal.azure.com) auf das Symbol „Cloud Shell“:

    ![Symbol zum Starten von Cloud Shell über das Azure-Portal](../Cloud-Shell/media/overview/portal-launch-icon.png)
    
2.  Führen Sie den folgenden Befehl aus, um das Feature für Ihr Abonnement zu registrieren.

    ```powershell
     Register-AzProviderFeature -FeatureName "EncryptionAtHost" -ProviderNamespace "Microsoft.Compute" 
    ```

3.  Überprüfen Sie mithilfe des folgenden Befehls, ob der Registrierungsstatus „Registriert“ lautet (dies dauert einige Minuten), bevor Sie das Feature ausprobieren.

    ```powershell
     Get-AzProviderFeature -FeatureName "EncryptionAtHost" -ProviderNamespace "Microsoft.Compute"  
    ```


Melden Sie sich mit dem [bereitgestellten Link](https://aka.ms/diskencryptionupdates) beim Azure-Portal an.

> [!IMPORTANT]
> Sie müssen den [bereitgestellten Link](https://aka.ms/diskencryptionupdates) verwenden, um auf das Azure-Portal zuzugreifen. Die Verschlüsselung auf dem Host ist derzeit im öffentlichen Azure-Portal ohne Verwendung des Links nicht sichtbar.

### <a name="create-an-azure-key-vault-and-disk-encryption-set"></a>Erstellen einer Azure Key Vault-Instanz und eines Datenträgerverschlüsselungssatzes

Nach dem Aktivieren des Features müssen Sie eine Azure Key Vault-Instanz und einen Datenträgerverschlüsselungssatz einrichten, sofern noch nicht geschehen.

[!INCLUDE [virtual-machines-disks-encryption-create-key-vault-portal](../../includes/virtual-machines-disks-encryption-create-key-vault-portal.md)]

## <a name="deploy-a-vm"></a>Bereitstellen einer VM

Sie müssen eine neue VM bereitstellen, um die Verschlüsselung auf dem Host zu aktivieren. Diese kann auf vorhandenen VMs nicht aktiviert werden.

1. Suchen Sie nach **Virtuelle Computer**, und wählen Sie **+ Hinzufügen** aus, um eine VM zu erstellen.
1. Erstellen Sie einen neuen virtuellen Computer, und wählen Sie eine geeignete Region und eine unterstützte VM-Größe aus.
1. Geben Sie die anderen Werte auf dem Blatt **Grundlagen** nach Wunsch ein, und fahren Sie dann mit dem Blatt **Datenträger** fort.

    :::image type="content" source="media/virtual-machines-disks-encryption-at-host-portal/disks-encryption-at-host-basic-blade.png" alt-text="Screenshot des Blatts „Grundlagen“ zum Erstellen virtueller Computer mit hervorgehobenen Optionen für Region und VM-Größe":::

1. Wählen Sie auf dem Blatt **Datenträger** die Option **Ja** für **Verschlüsselung auf dem Host** aus.
1. Treffen Sie die verbleibende Auswahl nach Wunsch.

    :::image type="content" source="media/virtual-machines-disks-encryption-at-host-portal/disks-encryption-at-host-disk-blade.png" alt-text="Screenshot des Blatts „Datenträger“ zum Erstellen virtueller Computer mit hervorgehobener Option für Verschlüsselung auf dem Host":::

1. Beenden Sie den VM-Bereitstellungsprozess, und wählen Sie Optionen passend für Ihre Umgebung aus.

Sie haben nun eine VM mit aktivierter Verschlüsselung auf dem Host bereitgestellt. Alle zugehörigen Datenträger werden mithilfe der Verschlüsselung auf dem Host verschlüsselt.

## <a name="next-steps"></a>Nächste Schritte

[Beispiele für Azure Resource Manager-Vorlagen](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/EncryptionAtHost)
