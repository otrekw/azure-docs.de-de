---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/07/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: f0a706cc51027bc503da9ee3bc637216e9e52853
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87171343"
---
## <a name="restrictions"></a>Beschränkungen

[!INCLUDE [virtual-machines-disks-encryption-at-host-restrictions](virtual-machines-disks-encryption-at-host-restrictions.md)]

### <a name="supported-regions"></a>Unterstützte Regionen

[!INCLUDE [virtual-machines-disks-encryption-at-host-regions](virtual-machines-disks-encryption-at-host-regions.md)]

### <a name="supported-vm-sizes"></a>Unterstützte VM-Größen

[!INCLUDE [virtual-machines-disks-encryption-at-host-suported-sizes](virtual-machines-disks-encryption-at-host-suported-sizes.md)]

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen das Feature für Ihr Abonnement aktivieren lassen, um die Verschlüsselung auf dem Host für Ihre virtuellen Computer oder VM-Skalierungsgruppen verwenden zu können. Senden Sie eine E-Mail mit Ihren Abonnement-IDs an „encryptionAtHost@microsoft. com“, um das Feature für Ihre Abonnements aktivieren zu lassen.

Melden Sie sich mit dem [bereitgestellten Link](https://aka.ms/diskencryptionupdates) beim Azure-Portal an.

> [!IMPORTANT]
> Sie müssen den [bereitgestellten Link](https://aka.ms/diskencryptionupdates) verwenden, um auf das Azure-Portal zuzugreifen. Die Verschlüsselung auf dem Host ist derzeit im öffentlichen Azure-Portal ohne Verwendung des Links nicht sichtbar.

### <a name="create-an-azure-key-vault-and-disk-encryption-set"></a>Erstellen einer Azure Key Vault-Instanz und eines Datenträgerverschlüsselungssatzes

Nach dem Aktivieren des Features müssen Sie eine Azure Key Vault-Instanz und einen Datenträgerverschlüsselungssatz einrichten, sofern noch nicht geschehen.

[!INCLUDE [virtual-machines-disks-encryption-create-key-vault-portal](virtual-machines-disks-encryption-create-key-vault-portal.md)]

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