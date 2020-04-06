---
title: Herstellen einer Verbindung mit einer Azure Lab Services-VM über einen Mac | Microsoft-Dokumentation
description: Dieser Artikel
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2020
ms.author: spelluru
ms.openlocfilehash: 0d3484c1008e00bcfde3adb399e925c0e054f49e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79504110"
---
# <a name="connect-to-a-vm-using-rdp-on-a-mac"></a>Herstellen einer RDP-Verbindung mit einem virtuellen Computer auf einem Mac
In diesem Abschnitt wird gezeigt, wie ein Kursteilnehmer über einen Mac eine RDP-Verbindung mit einer Classroom-Lab-VM herstellen kann.

## <a name="install-microsoft-remote-desktop-on-a-mac"></a>Installieren von Microsoft-Remotedesktop auf einem Mac
1. Öffnen Sie den App Store auf Ihrem Mac, und suchen Sie nach **Microsoft-Remotedesktop**.

    ![Microsoft-Remotedesktop](../media/how-to-use-classroom-lab/install-ms-remote-desktop.png)
1. Installieren Sie die neueste Version von Microsoft-Remotedesktop. 

## <a name="access-the-vm-from-your-mac-using-rdp"></a>Zugreifen auf den virtuellen Computer über Ihren Mac per RDP
1. Öffnen Sie die auf Ihren Computer heruntergeladene **RDP**-Datei. (Auf dem Computer muss **Microsoft-Remotedesktop** installiert sein.) Daraufhin sollte eine Verbindung mit dem virtuellen Computer hergestellt werden. 

    ![Herstellen einer Verbindung mit dem virtuellen Computer](../media/how-to-use-classroom-lab/connect-linux-vm.png)
1. Wählen Sie **Weiter** aus, wenn Sie die folgende Warnung erhalten. 

    ![Zertifikatwarnung](../media/how-to-use-classroom-lab/certificate-error.png)
1. Der virtuelle Computer sollte angezeigt werden. 

    > [!NOTE]
    > Das folgende Beispiel ist ein Beispiel für einen virtuellen Computer mit CentOS Linux. 

    ![VM](../media/how-to-use-classroom-lab/vm-ui.png)


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Herstellen einer Verbindung mit Linux-VMs mithilfe von RDP finden Sie unter [Verwenden von Remote Desktop für virtuelle Linux-Computer](how-to-use-remote-desktop-linux-student.md)


