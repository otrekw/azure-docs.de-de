---
title: Aktivieren von Remotedesktop für Linux in Azure Lab Services | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Remotedesktop für virtuelle Linux-Computer in einem Lab in Azure Lab Services aktivieren.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2020
ms.author: spelluru
ms.openlocfilehash: a40575340fc5b1c202be6b001807085954439f03
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588155"
---
# <a name="enable-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Aktivieren von Remotedesktop für virtuelle Linux-Computer in einem Lab in Azure Lab Services
In diesem Artikel wird gezeigt, wie Sie Folgendes durchführen:

- Aktivieren von Remotedesktop für einen virtuellen Linux-Computer
- Herstellen einer Verbindung mit der Vorlage für virtuelle Computer per Remotedesktopverbindung (RDP) für Lehrer/Dozenten.

## <a name="enable-remote-desktop-for-linux-vm"></a>Aktivieren von Remotedesktop für einen virtuellen Linux-Computer
Während der Lab-Erstellung können Lehrer/Dozenten die **Remotedesktopverbindung** für **Linux**-Images aktivieren. Die Option **Remotedesktop aktivieren** wird angezeigt, wenn für die Vorlage ein Linux-Image ausgewählt wurde. Wenn diese Option aktiviert ist, können Lehrer/Dozenten eine Verbindung mit der Vorlage für virtuelle Computer und mit virtuellen Computern für Kursteilnehmer per RDP (Remotedesktopverbindung) herstellen. 

![Aktivieren der Remotedesktopverbindung für ein Linux-Image](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

Wählen Sie im Meldungsfeld **Enabling Remote Desktop Connection** (Aktivieren der Remotedesktopverbindung) die Option **Continue with Remote Desktop** (Weiter mit Remotedesktop) aus. 

![Aktivieren der Remotedesktopverbindung für ein Linux-Image](../media/how-to-enable-remote-desktop-linux/enabling-remote-desktop-connection-dialog.png)

> [!IMPORTANT] 
> Beim Aktivieren der **Remotedesktopverbindung** wird auf Linux-Computern nur der **RDP**-Port geöffnet. Wenn RDP bereits für das VM-Image installiert und konfiguriert ist, können Sie/Kursteilnehmer über RDP eine Verbindung mit virtuellen Computern herstellen, ohne weitere Schritte ausführen zu müssen.
> 
> Wenn in dem VM-Image kein RDP installiert und konfiguriert ist, müssen Sie beim ersten Mal eine Verbindung mit dem Linux-Computer per SSH herstellen und dann RDP- und GUI-Pakete installieren, damit Sie/Studenten später eine Verbindung mit dem Linux-Computer per RDP herstellen können. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Remotedesktop zum Herstellen einer Verbindung mit einem virtuellen Linux-Computer in Azure](../../virtual-machines/linux/use-remote-desktop.md). Anschließend veröffentlichen Sie das Image, damit die Kursteilnehmer per RDP eine Verbindung mit den virtuellen Linux-Computern herstellen können, die für sie bestimmt sind. 

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
Derzeit wird die Remotedesktopverbindung für die folgenden Betriebssysteme unterstützt:

- openSUSE Leap 42.3
- 7\.5 (CentOS-basiert)
- Debian 9 „Stretch“
- Ubuntu Server 16.04 LTS

## <a name="connect-to-the-template-vm"></a>Herstellen einer Verbindung mit der Vorlage für virtuelle Computer 
Lehrer/Dozenten müssen zunächst per SSH eine Verbindung mit der Vorlage für virtuelle Computer herstellen und darauf RDP- und GUI-Pakete installieren. Anschließend können die Lehrer/Dozenten RDP verwenden, um eine Verbindung mit der Vorlage für virtuelle Computer herzustellen: 

1. Wird **Customize template** (Vorlage anpassen) auf der Symbolleiste angezeigt, wählen Sie die Option aus. Wählen Sie anschließend im Dialogfeld **Customize template** (Vorlage anpassen) die Option **Weiter** aus. Durch diese Aktion wird die Vorlage für virtuelle Computer gestartet.  

    ![Anpassen der Vorlage](../media/how-to-enable-remote-desktop-linux/customize-template.png)
2. Nach dem Starten der Vorlage für virtuelle Computer können Sie **Connect template** (Vorlage verbinden) und dann auf der Symbolleiste **Connect via SSH** (Über SSH verbinden) auswählen. 

    ![Herstellen einer Verbindung mit der Vorlage per RDP nach der Lab-Erstellung](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 
3. Das Dialogfeld **Eine Verbindung zu Ihrem virtuellen Computer herstellen** wird angezeigt. Wählen Sie neben dem Textfeld die Schaltfläche **Kopieren** aus, um den Wert in die Zwischenablage zu kopieren. Speichern Sie die SSH-Verbindungszeichenfolge. Verwenden Sie diese Verbindungszeichenfolge über ein SSH-Terminal (z. B. [Putty](https://www.putty.org/)), um eine Verbindung mit dem virtuellen Computer herzustellen.
 
    ![SSH-Verbindungszeichenfolge](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)
4. Installieren Sie RDP- und GUI-Pakete, damit Sie bzw. Kursteilnehmer später über RDP eine Verbindung mit dem Linux-Computer herstellen können. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Remotedesktop zum Herstellen einer Verbindung mit einem virtuellen Linux-Computer in Azure](../../virtual-machines/linux/use-remote-desktop.md). Anschließend veröffentlichen Sie das Image, damit die Kursteilnehmer per RDP eine Verbindung mit den virtuellen Linux-Computern herstellen können, die für sie bestimmt sind.
5. Nach der Installation dieser Pakete können Sie auf der Symbolleiste die Schaltfläche **Connect to template** (Mit Vorlage verbinden) und anschließend **Connect via RDP** (Über RDP verbinden) auswählen, um eine RDP-Verbindung mit der Vorlage für virtuelle Computer herzustellen. Speichern Sie die RDP-Datei, und verwenden Sie sie, um über RDP eine Verbindung mit der Vorlage für virtuelle Computer herzustellen. 

## <a name="next-steps"></a>Nächste Schritte
Nachdem ein Dozent das Feature „Remotedesktopverbindung“ aktiviert hat, können Kursteilnehmer über RDP/SSH eine Verbindung mit ihren VMs herstellen. Weitere Informationen finden Sie unter [Verwenden von Remotedesktop für virtuelle Linux-Computer in einem Classroom-Lab in Azure Lab Services](how-to-use-remote-desktop-linux-student.md). 