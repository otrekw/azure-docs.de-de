---
title: 'Vorgehensweise: Verwalten der Netzwerkschnittstellen von VMs auf Ihrer Azure Stack Edge Pro-Instanz über das Azure-Portal'
description: Erfahren Sie, wie Sie Netzwerkschnittstellen von VMs verwalten, die auf Ihrer Azure Stack Edge Pro GPU-Instanz über das Azure-Portal bereitgestellt werden.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/30/2021
ms.author: alkohli
ms.openlocfilehash: 3e709b04b4eac60e6cc0ba3e53eb77583162dfef
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106078892"
---
# <a name="use-the-azure-portal-to-manage-network-interfaces-on-the-vms-on-your-azure-stack-edge-pro-gpu"></a>Verwenden des Azure-Portals zum Verwalten von Netzwerkschnittstellen auf VMs auf Ihrer Azure Stack Edge Pro-GPU-Instanz

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Sie können virtuelle Computer (Virtual Machines, VMs) auf einem Azure Stack Edge-Gerät über das Azure-Portal, Vorlagen, Azure PowerShell-Cmdlets sowie über die Azure CLI bzw. Python-Skripts erstellen und verwalten. In diesem Artikel wird beschrieben, wie Sie die Netzwerkschnittstellen einer VM, die auf Ihrem Azure Stack Edge-Gerät ausgeführt wird, über das Azure-Portal verwalten können. 

Wenn Sie einen virtuellen Computer erstellen, geben Sie eine zu erstellende virtuelle Netzwerkschnittstelle an. Möglicherweise möchten Sie dem virtuellen Computer nach seiner Erstellung eine oder mehrere Netzwerkschnittstellen hinzufügen. Vielleicht möchten Sie auch die Standardeinstellungen einer vorhandenen Netzwerkschnittstelle ändern.

In diesem Artikel wird erläutert, wie Sie eine Netzwerkschnittstelle einem vorhandenen virtuellen Computer hinzufügen, vorhandene Einstellungen wie z. B. den IP-Typ (statisch oder dynamisch) ändern und schließlich eine vorhandene Schnittstelle entfernen oder trennen können. 

        
## <a name="about-network-interfaces-on-vms"></a>Informationen zu Netzwerkschnittstellen von VMs

Über eine Netzwerkschnittstelle kann ein virtueller Computer (VM), der auf Ihrem Azure Stack Edge Pro-Gerät ausgeführt wird, mit Azure und lokalen Ressourcen kommunizieren. Wenn Sie einen Port für das Computenetzwerk auf Ihrem Gerät aktivieren, wird an dieser Netzwerkschnittstelle ein virtueller Switch erstellt. Dieser virtuelle Switch wird dann verwendet, um Computeworkloads wie VMs oder containerisierte Anwendungen auf Ihrem Gerät bereitzustellen. 

Ihr Gerät unterstützt nur einen virtuellen Switch, aber mehrere virtuelle Netzwerkschnittstellen. Jeder Netzwerkschnittstelle an Ihrer VM ist eine statische oder dynamische IP-Adresse zugewiesen. Wenn IP-Adressen mehreren Netzwerkschnittstellen auf Ihrem virtuellen Computer zugewiesen sind, sind bestimmte Funktionen auf Ihrer VM aktiviert. Ihre VM kann z. B. mehrere Websites oder Dienste mit unterschiedlichen IP-Adressen und SSL-Zertifikaten auf einem einzelnen Server hosten. Eine VM auf Ihrem Gerät kann als virtuelles Netzwerkgerät fungieren, z. B. als Firewall oder Lastenausgleichsmodul. <!--Is it possible to do that on ASE?-->

<!--There is a limit to how many virtual network interfaces can be created on the virtual switch on your device. See the Azure Stack Edge Pro limits article for details.--> 


## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit dem Verwalten von VMs auf Ihrem Gerät über das Azure-Portal beginnen, stellen Sie Folgendes sicher:

1. Sie haben Zugriff auf ein aktiviertes Azure Stack Edge Pro GPU-Gerät. Sie haben auch eine Netzwerkschnittstelle für Compute auf Ihrem Gerät aktiviert. Durch diese Aktion wird ein virtueller Switch für diese Netzwerkschnittstelle auf der VM erstellt. 
    1. Navigieren Sie auf der lokalen Benutzeroberfläche Ihres Geräts zu **Compute**. Wählen Sie die Netzwerkschnittstelle aus, die Sie verwenden möchten, um einen virtuellen Switch zu erstellen.

        > [!IMPORTANT] 
        > Sie können nur einen Port für Compute konfigurieren.

    1. Aktivieren Sie Compute für die Netzwerkschnittstelle. Azure Stack Edge Pro GPU erstellt und verwaltet einen virtuellen Switch, der dieser Netzwerkschnittstelle entspricht.

1. Auf Ihrem Gerät ist mindestens eine VM bereitgestellt. Informationen zum Erstellen dieser VM finden Sie in den Anweisungen unter [Bereitstellen von VMs auf einem Azure Stack Edge Pro-GPU-Gerät über das Azure-Portal](azure-stack-edge-gpu-deploy-virtual-machine-portal.md).

1. Die VM sollte den Status **Beendet** aufweisen. Zum Beenden der VM navigieren Sie zu **Virtuelle Computer > Übersicht**, und wählen Sie die zu beendende VM aus. Wählen Sie auf der VM-Eigenschaftenseite die Option **Beenden** aus. Wenn Sie zur Bestätigung aufgefordert werden, wählen Sie **Ja** aus. Bevor Sie Netzwerkschnittstellen hinzufügen, bearbeiten oder löschen, müssen Sie den virtuellen Computer beenden.

    ![Beenden der VM auf der VM-Eigenschaftenseite](./media/azure-stack-edge-gpu-manage-virtual-machine-network-interfaces-portal/stop-vm-2.png)


## <a name="add-a-network-interface"></a>Hinzufügen einer Netzwerkschnittstelle

Führen Sie die folgenden Schritte aus, um eine Netzwerkschnittstelle einer VM hinzuzufügen, die auf Ihrem Gerät bereitgestellt wird. 

1. Wechseln Sie zu der VM, die Sie beendet haben, und navigieren Sie dann zur Seite **VM-Eigenschaften**. Wählen Sie **Netzwerk** aus.
    
    ![Auswählen von „Netzwerk“ auf der VM-Eigenschaftenseite](./media/azure-stack-edge-gpu-manage-virtual-machine-network-interfaces-portal/add-nic-1.png)

2. Wählen Sie auf dem Blatt **Netzwerk** in der Befehlsleiste **+ Netzwerkschnittstelle hinzufügen** aus.

    ![Auswählen von „Netzwerkschnittstelle hinzufügen“](./media/azure-stack-edge-gpu-manage-virtual-machine-network-interfaces-portal/add-nic-2.png)

3. Geben Sie auf dem Blatt **Netzwerkschnittstelle hinzufügen** die folgenden Parameter ein:

    
    |Column1  |Column2  |
    |---------|---------|
    |Name     | Ein eindeutiger Name innerhalb der Ressourcengruppe. Nach dem Erstellen kann der Name der Netzwerkschnittstelle nicht mehr geändert werden. Wenn Sie mehrere Netzwerkschnittstellen problemlos verwalten möchten, verwenden Sie die Vorschläge in den [Benennungskonventionen](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#resource-naming).     |
    |Virtuelles Netzwerk| Das virtuelle Netzwerk, das dem virtuellen Switch zugeordnet ist, der auf Ihrem Gerät erstellt wurde, als Sie Compute an der Netzwerkschnittstelle aktiviert haben. Ihrem Gerät ist nur ein virtuelles Netzwerk zugeordnet.         |         
    |Subnet     | Ein Subnetz innerhalb des ausgewählten virtuellen Netzwerks. Dieses Feld wird automatisch mit dem Subnetz ausgefüllt, das der Netzwerkschnittstelle zugeordnet ist, an der Sie Compute aktiviert haben.         |       
    |IP-Adresszuweisung   | Eine statische oder dynamische IP-Adresse für Ihre Netzwerkschnittstelle. Die statische IP-Adresse sollte eine verfügbare, freie IP-Adresse aus dem angegebenen Subnetzbereich sein. Wählen Sie eine dynamische IP-Adresse aus, wenn ein DHCP-Server in der Umgebung vorhanden ist.        | 

    ![Blatt „Netzwerkschnittstelle hinzufügen“](./media/azure-stack-edge-gpu-manage-virtual-machine-network-interfaces-portal/add-nic-3.png)

4. Es wird eine Benachrichtigung angezeigt, dass die Netzwerkschnittstelle erstellt wird.

    ![Benachrichtigung bei Erstellung der Netzwerkschnittstelle](./media/azure-stack-edge-gpu-manage-virtual-machine-network-interfaces-portal/add-nic-4.png)

5.  Sobald die Netzwerkschnittstelle erstellt ist, wird die Liste der Netzwerkschnittstellen aktualisiert, um die neu erstellte Schnittstelle anzuzeigen.

    ![Aktualisierte Liste der Netzwerkschnittstellen](./media/azure-stack-edge-gpu-manage-virtual-machine-network-interfaces-portal/add-nic-5.png)


## <a name="edit-a-network-interface"></a>Bearbeiten einer Netzwerkschnittstelle

Führen Sie die folgenden Schritte aus, um eine Netzwerkschnittstelle zu bearbeiten, die einer auf Ihrem Gerät bereitgestellten VM zugeordnet ist.

1. Wechseln Sie zu der VM, die Sie beendet haben, und navigieren Sie zur Seite **VM-Eigenschaften**. Wählen Sie **Netzwerk** aus.

1. Wählen Sie in der Liste der Netzwerkschnittstellen die Schnittstelle aus, die Sie bearbeiten möchten. Wählen Sie ganz rechts neben der ausgewählten Netzwerkschnittstelle das Bearbeitungssymbol (Stift) aus.  

    ![Auswählen einer zu bearbeitenden Netzwerkschnittstelle](./media/azure-stack-edge-gpu-manage-virtual-machine-network-interfaces-portal/edit-nic-1.png)

1. Auf dem Blatt **Netzwerkschnittstelle bearbeiten** können Sie nur die IP-Zuweisung der Netzwerkschnittstelle ändern. Der Name, das virtuelle Netzwerk und das Subnetz, die der Netzwerkschnittstelle zugeordnet sind, können nach der Erstellung nicht mehr geändert werden. Ändern Sie die **IP-Zuweisung** in statisch, und speichern Sie die Änderungen.

    ![Ändern der IP-Zuweisung für die Netzwerkschnittstelle](./media/azure-stack-edge-gpu-manage-virtual-machine-network-interfaces-portal/edit-nic-2.png)

1. Die Liste der Netzwerkschnittstellen wird aktualisiert, um die aktualisierte Netzwerkschnittstelle anzuzeigen.


## <a name="detach-a-network-interface"></a>Trennen einer Netzwerkschnittstelle

Führen Sie die folgenden Schritte aus, um eine Netzwerkschnittstelle zu trennen oder zu entfernen, die einer auf Ihrem Gerät bereitgestellten VM zugeordnet ist.

1. Wechseln Sie zu der VM, die Sie beendet haben, und navigieren Sie zur Seite **VM-Eigenschaften**. Wählen Sie **Netzwerk** aus.

1. Wählen Sie in der Liste der Netzwerkschnittstellen die Schnittstelle aus, die Sie bearbeiten möchten. Wählen Sie ganz rechts neben der ausgewählten Netzwerkschnittstelle das Trennsymbol (Stecker abziehen) aus.  

    ![Auswählen einer zu trennenden Netzwerkschnittstelle](./media/azure-stack-edge-gpu-manage-virtual-machine-network-interfaces-portal/detach-nic-1.png)

1. Nach vollständigem Trennen der Schnittstelle wird die Liste der Netzwerkschnittstellen aktualisiert, um die verbleibenden Schnittstellen anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Bereitstellen von VMs auf Ihrem Azure Stack Edge Pro-Gerät finden Sie unter [Bereitstellen von VMs über das Azure-Portal](azure-stack-edge-gpu-deploy-virtual-machine-portal.md).
