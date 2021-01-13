---
title: Herunterladen einer Linux-VHD von Azure
description: Laden Sie eine Linux-VHD mithilfe der Azure-Befehlszeilenschnittstelle und dem Azure-Portal herunter.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: how-to
ms.date: 08/03/2020
ms.author: cynthn
ms.openlocfilehash: 897cae53e589f4058e5499c0e6e941d4f1d9bb2f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87761053"
---
# <a name="download-a-linux-vhd-from-azure"></a>Herunterladen einer Linux-VHD von Azure

In diesem Artikel erfahren Sie, wie Sie mithilfe des Azure-Portals eine Linux-VHD-Datei (Virtual Hard Disk, virtuelle Festplatte) von Azure herunterladen. 

## <a name="stop-the-vm"></a>Beenden des virtuellen Computers

Eine VHD kann nicht von Azure heruntergeladen werden, wenn sie an eine ausgeführte VM angefügt ist. Sie müssen die VM beenden, um die VHD herunterzuladen. 

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2.  Wählen Sie im linken Menü **Virtual Machines** aus.
3.  Wählen Sie die VM aus der Liste aus.
4.  Wählen Sie auf der Seite für den virtuellen Computer die Option **Beenden** aus.

    :::image type="content" source="./media/download-vhd/export-stop.PNG" alt-text="Menüschaltfläche zum Beenden der VM":::

## <a name="generate-sas-url"></a>Generieren der SAS-URL

Um die VHD-Datei herunterzuladen, müssen Sie eine [SAS-URL (Shared Access Signature)](../../storage/common/storage-sas-overview.md?toc=/azure/virtual-machines/windows/toc.json) generieren. Wenn die URL generiert wird, wird der URL eine Ablaufzeit zugewiesen.

1. Wählen Sie im Menü auf der Seite des virtuellen Computers die Option **Datenträger** aus.
2. Wählen Sie den Betriebssystem-Datenträger für den virtuellen Computer und anschließend **Datenträgerexport** aus.
1. Aktualisieren Sie bei Bedarf den Wert **URL läuft ab in (Sekunden)** , damit Sie genügend Zeit zum Abschließen des Downloads haben. Der Standardwert ist 3.600 Sekunden (eine Stunde).
3. Wählen Sie **URL generieren** aus.
 
      
## <a name="download-vhd"></a>Herunterladen der VHD

1.  Wählen Sie unter der generierten URL die Option **VHD-Datei herunterladen** aus.

    :::image type="content" source="./media/download-vhd/export-download.PNG" alt-text="Menüschaltfläche zum Beenden der VM":::

2.  Gegebenenfalls muss im Browser die Option **Speichern** ausgewählt werden, um den Download zu starten. Der Standardname für die VHD-Datei lautet *abcd*.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum [Hochladen und Erstellen eines virtuellen Linux-Computers aus einem benutzerdefinierten Datenträger mithilfe von Azure CLI](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- [Verwalten von Azure-Datenträgern mit der Azure-CLI](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
