---
title: Nachverfolgen der Nutzung eines Labs in Azure Lab Services | Microsoft-Dokumentation
description: In diesem Tutorial verfolgen Sie als Lab-Ersteller/-Besitzer die Nutzung Ihres Labs nach.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 02/10/2020
ms.author: spelluru
ms.openlocfilehash: 55c7e2f30324fe7878e38f1129c63c6e0c51a75f
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77134830"
---
# <a name="tutorial-track-usage-of-a-lab-in-azure-lab-service"></a>Tutorial: Nachverfolgen der Nutzung eines Labs in Azure Lab Services
In diesem Tutorial wird gezeigt, wie ein Lab-Ersteller/-Besitzer die Nutzung eines Labs nachverfolgen kann.

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Anzeigen der beim Lab registrierten Benutzer
> * Anzeigen der Nutzung der virtuellen Computer im Lab
> * Verwalten von Studenten-VMs 


## <a name="view-registered-users"></a>Anzeigen registrierter Benutzer

1. Navigieren Sie zur Website [Azure Lab Services](https://labs.azure.com). 
2. Wählen Sie **Anmelden**, und geben Sie Ihre Anmeldeinformationen ein. Azure Lab Services unterstützt Geschäfts-, Schul- oder Unikonten und Microsoft-Konten.
3. Wählen Sie auf der Seite **My labs** (Meine Labs) das Lab aus, dessen Nutzung Sie nachverfolgen möchten. 
4. Wählen Sie **Benutzer** im linken Menü bzw. die Kachel **Benutzer**. Daraufhin werden bei Ihrem Lab registrierte Studenten angezeigt.  

    ![Registrierte Benutzer](../media/tutorial-track-usage/registered-users.png)

    Weitere Informationen zum Hinzufügen und Verwalten von Benutzern für das Lab finden Sie unter [Hinzufügen und Verwalten von Labbenutzern](how-to-configure-student-usage.md).

## <a name="view-the-usage-of-vms"></a>Anzeigen der Nutzung virtueller Computer

1. Wählen Sie im Menü auf der linken Seite die Option **Virtuelle Computer** aus. 
2. Überprüfen Sie, ob Sie den Status der virtuellen Computer und die Ausführungszeit der virtuellen Computer anzeigen können. Die von einem Labbesitzer für die VM eines Teilnehmers aufgewendete Zeit zählt nicht zur Nutzungszeit in der letzten Spalte. 

    ![VM-Nutzung](../media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Verwalten von Studenten-VMs 
Auf dieser Seite können Sie mithilfe der Steuerelemente in der Spalte **Status** oder auf der Symbolleiste die virtuellen Computer der Kursteilnehmer starten, beenden oder zurücksetzen.

![Steuerelemente für virtuelle Computer](../media/tutorial-track-usage/vm-controls.png)

Weitere Informationen zum Verwalten des VM-Pools für das Lab finden Sie unter [Einrichten und Verwalten eines VM-Pools](how-to-set-virtual-machine-passwords.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Classroom-Labs finden Sie in den Artikeln unter [Anleitungen](how-to-manage-lab-accounts.md).
