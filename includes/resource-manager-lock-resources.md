---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 05/03/2021
ms.author: tomfitz
ms.openlocfilehash: 8864e8025c77fb0b4b6efc694d9f4938e9ed8ae7
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108737483"
---
1. Wählen Sie im Blatt „Einstellungen“ für die Ressourcen, Ressourcengruppen oder Abonnements, die Sie sperren möchten, die Option **Sperren**.

    :::image type="content" source="media/resource-manager-lock-resources/select-lock.png" alt-text="Sperre auswählen.":::

1. Wählen Sie zum Hinzufügen einer Sperre **Hinzufügen**. Wenn Sie eine Sperre auf einer übergeordneten Ebene erstellen möchten, wählen Sie das übergeordnete Element. Die derzeit ausgewählte Ressource erbt die Sperre vom übergeordneten Element. Sie könnten z.B. die Ressourcengruppe sperren, um eine Sperre auf alle ihre Ressourcen anzuwenden.

    :::image type="content" source="media/resource-manager-lock-resources/add-lock.png" alt-text="Sperre hinzufügen":::.

1. Weisen Sie der Sperre einen Namen und eine Ebene zu. Optional können Sie Notizen hinzufügen, die die Sperre beschreiben.

    :::image type="content" source="media/resource-manager-lock-resources/set-lock.png" alt-text="Sperre festlegen.":::

1. Wählen Sie die Schaltfläche **Löschen** aus, um die ausgewählte Sperre zu löschen.

    :::image type="content" source="media/resource-manager-lock-resources/delete-lock.png" alt-text="Sperre löschen.":::
