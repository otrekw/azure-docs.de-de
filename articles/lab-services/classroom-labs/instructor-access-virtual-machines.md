---
title: Zugriff auf VMs in Azure Lab Services durch einen Kursleiter
description: In diesem Artikel wird erläutert, wie Kursleiter über die Kursleiteransicht auf ihre Kursteilnehmer-VMs zugreifen können. Ein Tutor kann z. B. ein Kursleiter für eine Klasse, aber ein Kursteilnehmer in anderen Klassen sein.
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
ms.date: 04/17/2020
ms.author: spelluru
ms.openlocfilehash: c048a2f159784a5bd38f185af29cec314b31824f
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2020
ms.locfileid: "81642603"
---
# <a name="access-virtual-machines-as-a-student-from-the-instructor-view"></a>Zugreifen auf virtuelle Computer als Kursteilnehmer in der Kursleiteransicht
In diesem Artikel wird gezeigt, wie Kursleiter auf ihre VMs für Klassen zugreifen können, die sie als Kursteilnehmer besuchen. 

Im Folgenden finden Sie ein Szenario, in dem dieses Feature hilfreich ist. Ein Tutor ist ein Kursleiter für eine Klasse, aber ein Kursteilnehmer in anderen Klassen. Er möchte die Kursteilnehmer-VMs in der Kursleiteransicht, in der die Labs in seinem Besitz aufgeführt werden, anzeigen und auf sie zugreifen. 

## <a name="access-vms-from-instructor-view"></a>Zugreifen auf VMs über die Kursleiteransicht

1. Melden Sie sich bei der [Azure Lab Services-Website](https://labs.azure.com) an. Sie sehen die Labs in Ihrem Besitz. Dies können Labs sein, die Sie selbst erstellt haben, oder Labs, die der Administrator Ihnen als Besitzer zugewiesen hat. Weitere Informationen finden Sie unter [Hinzufügen zusätzlicher Besitzer zu einem vorhandenen Lab](how-to-add-user-lab-owner.md).
2. Wählen Sie das Computersymbol in der rechten oberen Ecke aus, um auf VMs für Klassen zuzugreifen, die Sie als Kursteilnehmer besuchen. Vergewissern Sie sich, dass VMs angezeigt werden, auf die Sie als Kursteilnehmer zugreifen können. Im folgenden Beispiel ist der Benutzer ein Tutor für das Python-Lab, aber ein Kursteilnehmer im Java-Lab. Der Benutzer sieht daher die VM aus dem Java-Lab in der Dropdownliste. Der Benutzer kann die VM starten und eine Verbindung mit ihr herstellen. 
    
    ![Zugreifen auf Kursteilnehmer-VMs](../media/instructors-access-virtual-machines/access-student-virtual-machines.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln:

- [Herstellen einer Verbindung mit einem virtuellen Computer](how-to-use-classroom-lab.md#connect-to-the-vm)
- [Herstellen einer RDP-Verbindung mit einem virtuellen Computer auf einem Mac](connect-virtual-machine-mac-rdp.md)
- [Verwenden von Remotedesktop für virtuelle Linux-Computer](how-to-use-remote-desktop-linux-student.md)
