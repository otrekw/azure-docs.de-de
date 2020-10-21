---
title: Includedatei
description: Includedatei
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 10/12/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b079ede0845de3794507691bc3c252930e2a6604
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91977179"
---
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Navigieren Sie zur VM, mit der Sie eine Verbindung herstellen möchten, und klicken Sie dann auf **Verbinden**. Wählen Sie in der Dropdownliste die Option **Bastion** aus.

   :::image type="content" source="./media/bastion-vm-rdp/connect-vm.png" alt-text="Auswählen von „Bastion“" border="false":::

1. Nachdem Sie „Bastion“ aus dem Dropdown ausgewählt haben, wird eine Seitenleiste mit drei Registerkarten angezeigt: RDP, SSH und Bastion. Wenn Bastion für das virtuelle Netzwerk bereitgestellt wurde, ist die Registerkarte „Bastion“ standardmäßig aktiv. Klicken Sie auf **Bastion verwenden**.

   :::image type="content" source="./media/bastion-vm-rdp/use-bastion.png" alt-text="Auswählen von „Bastion“" border="false":::

1. Geben Sie auf der Seite **Verbindung über Azure Bastion herstellen** den Benutzernamen und das Kennwort für Ihre VM ein, und klicken Sie dann auf **Verbinden**.

   :::image type="content" source="./media/bastion-vm-rdp/host.png" alt-text="Auswählen von „Bastion“" border="false":::

1. Die RDP-Verbindung zu diesem virtuellen Computer über Bastion wird direkt im Azure-Portal (über HTML5) über Port 443 und den Bastion-Dienst geöffnet.

   :::image type="content" source="./media/bastion-vm-rdp/connection.png" alt-text="Auswählen von „Bastion“" border="false":::
