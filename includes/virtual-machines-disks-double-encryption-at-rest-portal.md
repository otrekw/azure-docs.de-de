---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 8313259d5375bb19e7caa08a01a658707453d133
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87177023"
---
## <a name="supported-regions"></a>Unterstützte Regionen

[!INCLUDE [virtual-machines-disks-double-encryption-at-rest-regions](virtual-machines-disks-double-encryption-at-rest-regions.md)]

## <a name="getting-started"></a>Erste Schritte

1. Melden Sie sich beim [Azure-Portal](https://aka.ms/diskencryptionupdates) an.

    > [!IMPORTANT]
    > Sie müssen den [bereitgestellten Link](https://aka.ms/diskencryptionupdates) verwenden, um auf das Azure-Portal zuzugreifen. Die doppelte Verschlüsselung im Ruhezustand ist derzeit im öffentlichen Azure-Portal ohne Verwendung des Links nicht sichtbar.

1. Suchen und wählen Sie **Datenträgerverschlüsselungssätze**.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-disk-encryption-sets-search.png" alt-text="Screenshot des Azure-Hauptportals, Datenträgerverschlüsselungssätze werden auf der Suchleiste hervorgehoben.":::

1. Wählen Sie **+ Hinzufügen**.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-add-disk-encryption-set.png" alt-text="Screenshot des Blatts für Datenträgerverschlüsselungssätze, „+ Hinzufügen“ ist hervorgehoben.":::

1. Wählen Sie eine der unterstützten Regionen aus.
1. Wählen Sie für **Verschlüsselungstyp** die Option **Doppelte Verschlüsselung mit plattformseitig und kundenseitig verwalteten Schlüsseln** aus.

    > [!NOTE]
    > Nachdem Sie einen Datenträgerverschlüsselungssatz mit einem bestimmten Verschlüsselungstyp erstellt haben, kann er nicht mehr geändert werden. Wenn Sie einen anderen Verschlüsselungstyp verwenden möchten, müssen Sie einen neuen Datenträgerverschlüsselungssatz erstellen.

1. Geben Sie die restlichen Informationen ein.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-create-disk-encryption-set-blade.png" alt-text="Screenshot des Blatts für die Erstellung von Datenträgerverschlüsselungssätzen, Regionen und doppelte Verschlüsselung mit plattformseitig und kundenseitig verwalteten Schlüsseln sind hervorgehoben.":::

1. Wählen Sie einen Azure Key Vault und einen Schlüssel aus, oder erstellen Sie bei Bedarf einen neuen.

    > [!NOTE]
    > Wenn Sie eine Key Vault-Instanz erstellen, müssen Sie vorläufiges Löschen und den Schutz vor endgültigem Löschen aktivieren. Diese Einstellungen sind obligatorisch, wenn ein Key Vault zum Verschlüsseln verwalteter Datenträger verwendet wird, und schützen Sie vor Datenverlust durch versehentliches Löschen.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-select-key-vault.png" alt-text="Screenshot des Blatts zur Key Vault-Erstellung.":::

1. Klicken Sie auf **Erstellen**.
1. Navigieren Sie zu dem von Ihnen erstellten Datenträgerverschlüsselungssatz, und wählen Sie den angezeigten Fehler aus. Dadurch wird Ihr Datenträgerverschlüsselungssatz so konfiguriert, dass er funktioniert.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-disk-set-error.png" alt-text="Screenshot des angezeigten Fehlers des Datenträgerverschlüsselungssatzes. Der Fehlertext lautet: „Um diesem Datenträgerverschlüsselungssatz einen Datenträger, ein Image oder eine Momentaufnahme zuzuordnen, müssen Sie dem Schlüsseltresor Berechtigungen erteilen“.":::

    Es sollte eine Benachrichtigung angezeigt werden. Auf diese Weise können Sie den Datenträgerverschlüsselungssatz mit Ihrem Schlüsseltresor verwenden.
    
    ![Screenshot der erfolgreichen Berechtigungs- und Rollenzuweisung für Ihren Schlüsseltresor.](media/virtual-machines-disk-encryption-portal/disk-encryption-notification-success.png)

1. Navigieren Sie zu Ihrem Datenträger.
1. Wählen Sie **Verschlüsselung** aus.
1. Wählen Sie für **Verschlüsselungstyp** die Option **Doppelte Verschlüsselung mit plattformseitig und kundenseitig verwalteten Schlüsseln** aus.
1. Wählen Sie Ihren Datenträgerverschlüsselungssatz aus.
1. Wählen Sie **Speichern** aus.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-enable-disk-blade.png" alt-text="Screenshot des Verschlüsselungsblatts für Ihren verwalteten Datenträger, wobei der oben genannte Verschlüsselungstyp hervorgehoben ist.":::

Sie haben jetzt die doppelte Verschlüsselung im Ruhezustand auf Ihrem verwalteten Datenträger aktiviert.
