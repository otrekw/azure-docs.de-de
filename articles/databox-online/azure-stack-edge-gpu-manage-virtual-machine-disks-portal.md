---
title: Verwalten von VM-Datenträgern auf Azure Stack Edge Pro-GPU-, Pro R- und Mini R-Geräten über das Azure-Portal
description: Erfahren Sie, wie Sie mithilfe des Azure-Portals Datenträger auf VMs verwalten, die auf Geräten vom Typ Azure Stack Edge Pro-GPU, Azure Stack Edge Pro R, and Azure Stack Edge Mini R bereitgestellt werden. Dies umfasst auch das Hinzufügen und Trennen von Datenträgern.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/30/2021
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to manage disks on a VM running on an Azure Stack Edge Pro device so that I can use it to run applications using Edge compute before sending it to Azure.
ms.openlocfilehash: dff3f4bafdb35d89e8f1792c1aca68824a9bc685
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106079896"
---
# <a name="use-the-azure-portal-to-manage-disks-on-the-vms-on-your-azure-stack-edge-pro-gpu"></a>Verwenden des Azure-Portals zum Verwalten von Datenträgern auf VMs auf Azure Stack Edge Pro-GPU-Geräten

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Über das Azure-Portal können Sie Datenträger auf den virtuellen Computern (VMs) bereitstellen, die auf Ihrem Azure Stack Edge Pro-Gerät verfügbar gemacht werden. Die Datenträger werden über die lokale Azure Resource Manager-Instanz auf dem Gerät bereitgestellt und belegen Gerätekapazität. Vorgänge wie das Hinzufügen und das Trennen eines Datenträgers können über das Azure-Portal durchgeführt werden, das wiederum Aufrufe an die lokale Azure Resource Manager-Instanz sendet, um den Speicher bereitzustellen. 

In diesem Artikel wird erläutert, wie Sie über das Azure-Portal einer vorhandenen VM einen Datenträger hinzufügen, einen Datenträger trennen und schließlich die Größe der VM selbst ändern. 

        
## <a name="about-disks-on-vms"></a>Informationen zu Datenträgern auf VMs

Ihre VM kann einen Datenträger für das Betriebssystem und einen Datenträger für Daten aufweisen. Jeder VM, die auf Ihrem Gerät bereitgestellt wird, ist ein Betriebssystemdatenträger angefügt. Dieser Betriebssystemdatenträger weist ein vorinstalliertes Betriebssystem auf, das beim Erstellen der VM ausgewählt wurde. Dieser Datenträger enthält das Startvolume.

> [!NOTE]
> Sie können die Größe des Betriebssystemdatenträgers für die VM auf Ihrem Gerät nicht ändern. Die Größe des Betriebssystemdatenträgers wird durch die von Ihnen ausgewählte VM-Größe bestimmt. 


Ein Datenträger für Daten hingegen ist ein verwalteter Datenträger, der an die VM angefügt ist, die auf Ihrem Gerät ausgeführt wird. Ein solcher Datenträger wird verwendet, um Anwendungsdaten zu speichern. Datenträger für Daten sind in der Regel SCSI-Laufwerke. Die Größe der VM bestimmt, wie viele Datenträger für Daten Sie anfügen können. Standardmäßig wird Storage Premium zum Hosten der Datenträger verwendet.

Eine auf Ihrem Gerät bereitgestellte VM kann mitunter einen temporären Datenträger enthalten. Der temporäre Datenträger bietet kurzfristigen Speicher für Anwendungen und Prozesse und ist ausschließlich dafür ausgelegt, Daten wie z. B. Seiten- oder Auslagerungsdateien zu speichern. Daten auf dem temporären Datenträger können während eines Wartungsereignisses verloren gehen, oder wenn Sie eine VM erneut bereitstellen. Während eines erfolgreichen standardmäßigen Neustarts der VM bleiben die Daten auf dem temporären Datenträger erhalten. 


## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie damit beginnen, Datenträger auf den VMs auf Ihrem Gerät über das Azure-Portal zu verwalten, stellen Sie Folgendes sicher:


1. Sie haben Zugriff auf ein aktiviertes Azure Stack Edge Pro-GPU-Gerät. Sie haben auch eine Netzwerkschnittstelle für Compute auf Ihrem Gerät aktiviert. Durch diese Aktion wird ein virtueller Switch für diese Netzwerkschnittstelle auf der VM erstellt. 
    1. Navigieren Sie auf der lokalen Benutzeroberfläche Ihres Geräts zu **Compute**. Wählen Sie die Netzwerkschnittstelle aus, die Sie verwenden möchten, um einen virtuellen Switch zu erstellen.

        > [!IMPORTANT] 
        > Sie können nur einen Port für Compute konfigurieren.

    1. Aktivieren Sie Compute für die Netzwerkschnittstelle. Azure Stack Edge Pro-GPU erstellt und verwaltet einen virtuellen Switch, der dieser Netzwerkschnittstelle entspricht.

1. Auf Ihrem Gerät ist mindestens eine VM bereitgestellt. Informationen zum Erstellen dieser VM finden Sie in den Anweisungen unter [Bereitstellen von VMs auf einem Azure Stack Edge Pro-GPU-Gerät über das Azure-Portal](azure-stack-edge-gpu-deploy-virtual-machine-portal.md).



## <a name="add-a-data-disk"></a>Hinzufügen eines Datenträgers

Führen Sie die folgenden Schritte aus, um einen Datenträger zu einer VM hinzuzufügen, die auf Ihrem Gerät bereitgestellt wird. 

1. Wechseln Sie zu der VM, der Sie einen Datenträger hinzufügen möchten, und navigieren Sie dann zur Seite **Übersicht**. Wählen Sie **Datenträger** aus.
    
    ![Auswählen von „Datenträger“ auf der Seite „Übersicht“](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/add-data-disk-1.png)

1. Wählen Sie auf dem Blatt **Datenträger** unter **Datenträger** die Option **Neuen Datenträger erstellen und anfügen** aus.

    ![Erstellen und Anfügen eines neuen Datenträgers](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/add-data-disk-2.png)

1. Geben Sie auf dem Blatt **Neuen Datenträger erstellen** die folgenden Parameter ein:

    
    |Feld  |BESCHREIBUNG  |
    |---------|---------|
    |Name     | Ein eindeutiger Name innerhalb der Ressourcengruppe. Nach Erstellung des Datenträgers kann der Name nicht mehr geändert werden.     |
    |Size| Die Größe des Datenträgers in GiB. Die maximale Größe eines Datenträgers wird durch die von Ihnen ausgewählte VM-Größe bestimmt. Beim Bereitstellen eines Datenträgers sollten Sie auch den tatsächlichen Speicherplatz auf Ihrem Gerät und andere aktive VM-Workloads berücksichtigen, die Kapazität belegen.  |         

    ![Blatt zum Erstellen eines neuen Datenträgers](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/add-data-disk-3.png)

    Wählen Sie **OK** aus, und setzen Sie den Vorgang fort.

1. Auf der Seite **Übersicht** wird unter **Datenträger** ein Eintrag angezeigt, der dem neuen Datenträger entspricht. Übernehmen Sie die Standardeinstellung, oder weisen Sie dem Datenträger eine gültige logische Gerätenummer (LUN) zu, und wählen Sie **Speichern** aus. Eine LUN ist ein eindeutiger Bezeichner für einen SCSI-Datenträger. Weitere Informationen finden Sie unter [Was ist eine logische Gerätenummer?](../virtual-machines/linux/azure-to-guest-disk-mapping.md#what-is-a-lun).

    ![Neuer Datenträger auf der Seite „Übersicht“](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/add-data-disk-4.png)

1. Es wird eine Benachrichtigung angezeigt, dass der Datenträger erstellt wird. Nachdem der Datenträger erfolgreich erstellt wurde, wird die VM aktualisiert. 

    ![Benachrichtigung über Datenträgererstellung](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/add-data-disk-5.png)

1. Navigieren Sie zurück zur Seite **Übersicht**. Die Liste der Datenträger wird aktualisiert, um den neu erstellten Datenträger anzuzeigen.

    ![Aktualisierte Liste der Datenträger](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/add-data-disk-6.png)


## <a name="change-a-data-disk"></a>Ändern eines Datenträgers

Führen Sie die folgenden Schritte aus, um einen Datenträger zu ändern, der einer auf Ihrem Gerät bereitgestellten VM zugeordnet ist.

1. Wechseln Sie zu der VM mit dem zu ändernden Datenträger, und navigieren Sie zur Seite **Übersicht**. Wählen Sie **Datenträger** aus.

1. Wählen Sie in der Liste der Datenträger den Datenträger aus, den Sie ändern möchten. Wählen Sie ganz rechts neben dem ausgewählten Datenträger das Bearbeitungssymbol (Stift) aus.  

    ![Auswählen eines zu ändernden Datenträgers](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/edit-data-disk-1.png)

1. Auf dem Blatt **Datenträger ändern** können Sie nur die Größe des Datenträgers ändern. Der Name, der dem Datenträger zugeordnet ist, kann nach der Erstellung nicht mehr geändert werden. Ändern Sie die **Größe**, und speichern Sie die Änderungen.

    ![Ändern der Größe des Datenträgers](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/edit-data-disk-2.png)

    > [!NOTE]
    > Sie können einen Datenträger nur erweitern und nicht verkleinert.

1. Die Liste der Datenträger auf der Seite **Übersicht** wird aktualisiert, um den aktualisierten Datenträger anzuzeigen.


## <a name="attach-an-existing-disk"></a>Anfügen eines vorhandenen Datenträgers

Führen Sie die folgenden Schritte aus, um einen vorhandenen Datenträger an die auf Ihrem Gerät bereitgestellte VM anzufügen.

1. Wechseln Sie zu der VM, der Sie den vorhandenen Datenträger anfügen möchten, und navigieren Sie dann zur Seite **Übersicht**. Wählen Sie **Datenträger** aus.
    
    ![Auswählen von „Datenträger“ ](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/list-data-disks-1.png)

1. Wählen Sie auf dem Blatt **Datenträger** unter **Datenträger** die Option **Vorhandenen Datenträger anfügen** aus.

    ![Auswählen von „Vorhandenen Datenträger anfügen“](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/attach-existing-data-disk-1.png)

1. Übernehmen Sie die Standard-LUN, oder weisen Sie eine gültige LUN zu. Wählen Sie einen vorhandenen Datenträger aus der Dropdownliste aus. Wählen Sie „Speichern“ aus.

    ![Auswählen eines vorhandenen Datenträgers](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/attach-existing-data-disk-2.png)

    Wählen Sie **Speichern** aus, und setzen Sie den Vorgang fort.

1. Es wird eine Benachrichtigung angezeigt, dass die VM aktualisiert wird. Nachdem die VM aktualisiert wurde, navigieren Sie zurück zur Seite **Übersicht**. Aktualisieren Sie die Seite, um den neu angefügten Datenträger in der Liste der Datenträger anzuzeigen.

    ![Anzeigen der aktualisierten Liste der Datenträger auf der Seite „Übersicht“](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/list-data-disks-2.png)


## <a name="detach-a-data-disk"></a>Trennen eines Datenträgers

Führen Sie die folgenden Schritte aus, um einen Datenträger zu trennen oder zu entfernen, der einer auf Ihrem Gerät bereitgestellten VM zugeordnet ist.

> [!NOTE]
> - Sie können einen Datenträger entfernen, während die VM ausgeführt wird. Stellen Sie sicher, dass der Datenträger nicht aktiv verwendet wird, bevor Sie ihn von der VM trennen.
> - Beim Trennen eines Datenträgers wird dieser nicht automatisch gelöscht.

1. Wechseln Sie zu der VM, von der Sie einen Datenträger trennen möchten, und navigieren Sie zur Seite **Übersicht**. Wählen Sie **Datenträger** aus.

    ![Auswählen von „Datenträger“](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/list-data-disks-1.png)

1. Wählen Sie in der Liste der Datenträger den Datenträger aus, den Sie trennen möchten. Wählen Sie ganz rechts neben dem ausgewählten Datenträger das Trennsymbol (Kreuz) aus. Der ausgewählte Eintrag wird getrennt. Wählen Sie **Speichern** aus. 

    ![Auswählen eines zu trennenden Datenträgers](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/detach-data-disk-1.png)

1. Nachdem der Datenträger getrennt wurde, wird die VM aktualisiert. Aktualisieren Sie die Seite **Übersicht**, um die aktualisierte Liste der Datenträger anzuzeigen.

    ![Auswählen von „Speichern“](./media/azure-stack-edge-gpu-manage-virtual-machine-disks-portal/list-data-disks-2.png)


## <a name="next-steps"></a>Nächste Schritte

Informationen zum Bereitstellen von VMs auf Ihrem Azure Stack Edge Pro-Gerät finden Sie unter [Bereitstellen von VMs über das Azure-Portal](azure-stack-edge-gpu-deploy-virtual-machine-portal.md).
