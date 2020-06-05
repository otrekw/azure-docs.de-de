---
title: Herstellen einer Verbindung mit einer Azure Lab Services-VM über einen Mac | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in Azure Lab Services eine Verbindung von einem Mac zu einem virtuellen Computer herstellen.
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
ms.openlocfilehash: 9087e4fb7989f89c19a70afd9ee8f061f9061166
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83704365"
---
# <a name="connect-to-a-vm-using-remote-desktop-protocol-on-a-mac"></a>Herstellen einer Verbindung mit einer VM mithilfe des Remotedesktopprotokolls auf einem Mac
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


