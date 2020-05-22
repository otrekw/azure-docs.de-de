---
title: Zugriff auf VMs in Azure Lab Services durch einen Lehrer/Dozenten
description: In diesem Artikel wird erläutert, wie Lehrer/Dozenten über die Lehrer-/Dozentenansicht auf ihre Kursteilnehmer-VMs zugreifen können. Ein Tutor kann z. B. ein Lehrer/Dozent für eine Klasse, aber ein Kursteilnehmer in anderen Klassen sein.
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
ms.date: 05/15/2020
ms.author: spelluru
ms.openlocfilehash: 449ff8aafd6dec1e9c0ff16dc407155949fd1313
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83586166"
---
# <a name="access-virtual-machines-as-a-student-from-the-educator-view"></a>Zugreifen auf virtuelle Computer als Kursteilnehmer in der Lehrer-/Dozentenansicht
In diesem Artikel wird gezeigt, wie Lehrer/Dozenten auf ihre VMs für Klassen zugreifen können, die sie als Kursteilnehmer besuchen. 

Im Folgenden finden Sie ein Szenario, in dem dieses Feature hilfreich ist. Ein Tutor ist ein Lehrer/Dozent für eine Klasse, aber ein Kursteilnehmer in anderen Klassen. Und der Tutor möchte die Kursteilnehmer-VMs in der Lehrer-/Dozentenansicht, in der die Labs in seinem Besitz aufgeführt werden, anzeigen und auf sie zugreifen. 

## <a name="access-vms-from-educator-view"></a>Zugreifen auf virtuelle Computer über die Lehrer-/Dozentenansicht

1. Melden Sie sich bei der [Azure Lab Services-Website](https://labs.azure.com) an. Sie sehen die Labs in Ihrem Besitz. Dies können Labs sein, die Sie selbst erstellt haben, oder Labs, die der Administrator Ihnen als Besitzer zugewiesen hat. Weitere Informationen finden Sie unter [Hinzufügen zusätzlicher Besitzer zu einem vorhandenen Lab](how-to-add-user-lab-owner.md).
2. Wählen Sie das Computersymbol in der rechten oberen Ecke aus, um auf VMs für Klassen zuzugreifen, die Sie als Kursteilnehmer besuchen. Vergewissern Sie sich, dass VMs angezeigt werden, auf die Sie als Kursteilnehmer zugreifen können. Im folgenden Beispiel ist der Benutzer ein Tutor für das Python-Lab, aber ein Kursteilnehmer im Java-Lab. Der Benutzer sieht daher die VM aus dem Java-Lab in der Dropdownliste. Der Benutzer kann die VM starten und eine Verbindung mit ihr herstellen. 
    
    ![Zugreifen auf Kursteilnehmer-VMs](../media/instructors-access-virtual-machines/access-student-virtual-machines.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln:

- [Herstellen einer Verbindung mit einem virtuellen Computer](how-to-use-classroom-lab.md#connect-to-the-vm)
- [Herstellen einer RDP-Verbindung mit einem virtuellen Computer auf einem Mac](connect-virtual-machine-mac-rdp.md)
- [Verwenden von Remotedesktop für virtuelle Linux-Computer](how-to-use-remote-desktop-linux-student.md)
