---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/23/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 950fd82e14902c3a7a94fddb44d50ac372ebe119
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87177024"
---
[!INCLUDE [virtual-machines-disks-encryption-create-key-vault-portal](virtual-machines-disks-encryption-create-key-vault-portal.md)]

## <a name="deploy-a-vm"></a>Bereitstellen einer VM

Nachdem Sie nun Ihren Schlüsseltresor und den Datenträgerverschlüsselungssatz erstellt und eingerichtet haben, können Sie eine VM mit der Verschlüsselung bereitstellen.
Das Verfahren zur VM-Bereitstellung ähnelt dem Standardbereitstellungsverfahren. Die einzigen Unterschiede bestehen darin, dass Sie den virtuellen Computer in derselben Region wie Ihre anderen Ressourcen bereitstellen müssen und einen vom Kunden verwalteten Schlüssel verwenden.

1. Suchen Sie nach **Virtuelle Computer**, und wählen Sie **+ Hinzufügen** aus, um eine VM zu erstellen.
1. Wählen Sie auf dem Blatt **Standard** dieselbe Region aus, in der sich Ihr Datenträgerverschlüsselungssatz und Ihre Azure Key Vault-Instanz befinden.
1. Geben Sie die anderen Werte auf dem Blatt **Standard** nach Wunsch ein.

    ![Screenshot des VM-Erstellungsvorgangs mit hervorgehobenem Wert für die Region.](media/virtual-machines-disk-encryption-portal/server-side-encryption-create-a-vm-region.png)

1. Wählen Sie auf dem Blatt **Datenträger** die Option **Verschlüsselung ruhender Daten mit einem kundenseitig verwalteten Schlüssel** aus.
1. Wählen Sie den Datenträgerverschlüsselungssatz in der Dropdownliste **Datenträgerverschlüsselungssatz** aus.
1. Treffen Sie die verbleibende Auswahl nach Wunsch.

    ![Screenshot des VM-Erstellungsvorgangs, Datenträgerblatt. Das Dropdownfeld für den Datenträgerverschlüsselungssatz ist hervorgehoben.](media/virtual-machines-disk-encryption-portal/server-side-encryption-create-vm-select-customer-managed-key-disk-encryption-set.png)

## <a name="enable-on-an-existing-disk"></a>Aktivieren auf einem vorhandenen Datenträger

> [!CAUTION]
> Zum Aktivieren der Datenträgerverschlüsselung auf Datenträgern, die an eine VM angefügt sind, müssen Sie die VM beenden.
    
1. Navigieren Sie zu einer VM, die sich in derselben Region befindet wie einer Ihrer Datenträgerverschlüsselungssätze.
1. Öffnen Sie die VM, und wählen Sie **Beenden** aus.

    ![Screenshot der Hauptüberlagerung für Ihre Beispiel-VM, wobei die Schaltfläche „Beenden“ hervorgehoben ist.](media/virtual-machines-disk-encryption-portal/server-side-encryption-stop-vm-to-encrypt-disk-fix.png)

1. Nachdem die VM beendet wurde, wählen Sie **Datenträger** und dann den Datenträger aus, den Sie verschlüsseln möchten.

    ![Screenshot Ihrer Beispiel-VM mit geöffnetem Datenträgerblatt. Der Betriebssystemdatenträger ist als Beispieldatenträger hervorgehoben, den Sie auswählen können.](media/virtual-machines-disk-encryption-portal/server-side-encryption-existing-disk-select.png)

1. Wählen Sie **Verschlüsselung**, anschließend **Verschlüsselung ruhender Daten mit einem kundenseitig verwalteten Schlüssel** und dann in der Dropdownliste den Datenträgerverschlüsselungssatz aus.
1. Wählen Sie **Speichern** aus.

    ![Screenshot Ihres Beispiel-Betriebssystemdatenträgers. Das Verschlüsselungsblatt ist geöffnet, die Verschlüsselung ruhender Daten mit einem kundenseitig verwalteten Schlüssel und Ihr Beispiel-Azure Key Vault sind ausgewählt. Nach dem Treffen dieser Auswahl wird die Speichern-Schaltfläche ausgewählt.](media/virtual-machines-disk-encryption-portal/server-side-encryption-encrypt-existing-disk-customer-managed-key.png)

1. Wiederholen Sie diesen Vorgang für alle anderen an die VM angefügten Datenträger, die Sie verschlüsseln möchten.
1. Wenn die Umstellung der Datenträger auf kundenseitig verwaltete Schlüssel abgeschlossen ist und Sie keine weiteren angefügten Datenträger mehr verschlüsseln möchten, können Sie Ihren virtuellen Computer starten.
