---
title: Herstellen einer Verbindung mit Ihren Windows-VMs in Azure DevTest Labs
description: Hier erfahren Sie, wie Sie in einem Lab (Azure DevTest Labs) eine Verbindung mit Ihrer Windows-VM herstellen.
ms.topic: how-to
ms.date: 07/17/2020
ms.openlocfilehash: e1e786daa396548030976159d1b150caa4b24396
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86540600"
---
# <a name="connect-to-a-windows-vm-in-your-lab-azure-devtest-labs"></a>Herstellen einer Verbindung mit einer Windows-VM im Lab (Azure DevTest Labs)
In diesem Artikel erfahren Sie, wie Sie in Ihrem Lab eine Verbindung mit einer Windows-VM herstellen. 

## <a name="connect-to-a-windows-vm"></a>Herstellen einer Verbindung mit einem virtuellen Windows-Computer
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie in der Suchleiste nach **DevTest Labs**, und wählen Sie die Option aus. 

    :::image type="content" source="./media/connect-windows-virtual-machine/search-select.png" alt-text="Suchen nach und Auswählen von DevTest Labs":::    
1. Wählen Sie in der Liste mit den Labs Ihr **Lab** aus.

    :::image type="content" source="./media/connect-windows-virtual-machine/select-lab.png" alt-text="Suchen nach und Auswählen von DevTest Labs":::            
1. Wählen Sie auf der Startseite für Ihr Lab in der Liste **Meine virtuellen Computer** Ihre Windows-VM aus. 

    :::image type="content" source="./media/connect-windows-virtual-machine/select-windows-vm.png" alt-text="Suchen nach und Auswählen von DevTest Labs":::                
1. Wählen Sie auf der Seite **Virtueller Computer** für Ihre VM in der Symbolleiste die Option **Verbinden** aus. Wenn die VM angehalten wurde, wählen Sie zuerst **Starten** aus, um die VM zu starten.

    :::image type="content" source="./media/connect-windows-virtual-machine/select-connect.png" alt-text="Suchen nach und Auswählen von DevTest Labs":::                    
1. Wenn Sie den Edge-Browser verwenden, sehen Sie am unteren Rand des Browsers den Link zur **heruntergeladenen RDP-Datei**. 

    :::image type="content" source="./media/connect-windows-virtual-machine/rdp-download.png" alt-text="Suchen nach und Auswählen von DevTest Labs":::                        
1. Öffnen Sie die RDP-Datei, und geben Sie die VM-Anmeldeinformationen ein, die Sie beim Erstellen der VM verwendet haben. Die Verbindung mit der Windows-VM sollte jetzt hergestellt sein. 

## <a name="next-steps"></a>Nächste Schritte
[Herstellen einer Verbindung mit einem virtuellen Linux-Computer](connect-linux-virtual-machine.md)
