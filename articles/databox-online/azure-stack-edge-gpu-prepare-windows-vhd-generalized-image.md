---
title: Erstellen von VM-Images aus einem generalisierten Image der Windows-VHD für Ihr Azure Stack Edge Pro-GPU-Gerät
description: Erfahren Sie, wie VM-Images aus generalisierten Images über eine Windows-VHD oder -VHDX erstellt werden. Mithilfe dieser generalisierten Images können Sie VM-Images zur Verwendung auf VMs erstellen, die auf Ihrem Azure Stack Edge Pro-GPU-Gerät bereitgestellt werden.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/18/2021
ms.author: alkohli
ms.openlocfilehash: 978099accd57d15c750a27f77b2e220f569a2dd0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104962313"
---
# <a name="use-generalized-image-from-windows-vhd-to-create-a-vm-image-for-your-azure-stack-edge-pro-device"></a>Verwenden eines generalisierten Images der Windows-VHD zum Erstellen eines VM-Images für Ihr Azure Stack Edge Pro-Gerät

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Wenn Sie virtuelle Computer auf Ihrem Azure Stack Edge Pro-Gerät bereitstellen möchten, müssen Sie in der Lage sein, benutzerdefinierte VM-Images zu erstellen, mit denen Sie wiederum virtuelle Computer erstellen können. In diesem Artikel werden die Schritte beschrieben, die erforderlich sind, um eine Windows-VHD oder -VHDX für die Erstellung eines generalisierten Images vorzubereiten. Das generalisierte Image wird dann zum Erstellen eines VM-Images für Ihr Azure Stack Edge Pro-Gerät verwendet. 

## <a name="about-preparing-windows-vhd"></a>Vorbereitung einer Windows-VHD

Sie können eine Windows-VHD oder -VHDX verwenden, um ein *generalisiertes* Image oder ein *spezialisiertes* Image zu erstellen. In der folgenden Tabelle sind die wichtigsten Unterschiede zwischen *generalisierten* und *spezialisierten* Images zusammengefasst.


|Imagetyp  |Generalisiert  |Spezialisiert  |
|---------|---------|---------|
|Ziel     |Bereitstellung auf beliebigen Systemen         | Auf ein bestimmtes System ausgerichtet        |
|Setup nach dem Start     | Setup beim ersten Start der VM erforderlich          | Kein Setup erforderlich <br> VM-Aktivierung über die Plattform        |
|Konfiguration     |Hostname, Administratorbenutzer und andere VM-spezifische Einstellungen erforderlich         |Vollständig vorkonfiguriert         |
|Verwendung beim     |Erstellen mehrerer neuer VMs aus demselben Image         |Migrieren eines bestimmten Computers oder Wiederherstellen einer VM aus einer früheren Sicherung         |


In diesem Artikel werden die erforderlichen Schritte für die Bereitstellung über ein generalisiertes Image beschrieben. Informationen zur Bereitstellung über ein spezialisiertes Image finden Sie unter [Verwenden einer spezialisierten Windows-VHD](azure-stack-edge-placeholder.md) für Ihr Gerät.

> [!IMPORTANT]
> Die beschriebenen Schritte beziehen sich nicht auf Fälle, bei denen die Quell-VHD mit benutzerdefinierten Konfigurationen und Einstellungen konfiguriert ist. Beispielsweise können zusätzliche Aktionen erforderlich sein, um eine VHD zu generalisieren, die benutzerdefinierte Firewallregeln oder Proxyeinstellungen beinhaltet. Weitere Informationen zu diesen zusätzlichen Aktionen finden Sie unter [Vorbereiten einer Windows-VHD für das Hochladen in Azure – Azure Virtual Machines](../virtual-machines/windows/prepare-for-upload-vhd-image.md).


## <a name="vm-image-workflow"></a>Workflow für VM-Images

Der allgemeine Workflow zum Vorbereiten einer Windows-VHD für die Verwendung als generalisiertes Image umfasst die folgenden Schritte:

1. Konvertieren der Quell-VHD oder -VHDX in eine VHD mit fester Größe
1. Erstellen einer VM in Hyper-V mithilfe der VHD mit fester Größe
1. Herstellen einer Verbindung mit der Hyper-V-VM
1. Generalisieren der VHD mit dem Hilfsprogramm *sysprep* 
1. Kopieren des generalisierten Images in Blob Storage
1. Verwenden des generalisierten Images zum Bereitstellen von VMs auf Ihrem Gerät Weitere Informationen finden Sie unter [Bereitstellen von VMs über das Azure-Portal](azure-stack-edge-gpu-deploy-virtual-machine-portal.md) oder [Bereitstellen von VMs über Azure PowerShell](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md).


## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor der Vorbereitung einer Windows-VHD zur Verwendung als generalisiertes Image in Azure Stack Edge Folgendes sicher:

- Die verwendete VHD oder VHDX enthält eine unterstützte Version von Windows. Entsprechende Informationen finden Sie unter [Unterstützte Gastbetriebssysteme]() für Ihr Azure Stack Edge Pro-Gerät. 
- Sie haben Zugriff auf einen Windows-Client mit installiertem Hyper-V-Manager. 
- Sie haben Zugriff auf ein Azure Blob Storage-Konto, um die VHD nach der Vorbereitung zu speichern.

## <a name="prepare-a-generalized-windows-image-from-vhd"></a>Vorbereiten eines generalisierten Windows-Images über eine VHD

## <a name="convert-to-a-fixed-vhd"></a>Konvertieren in eine VHD mit fester Größe 

Für Ihr Gerät benötigen Sie zum Erstellen von VM-Images VHDs mit fester Größe. Sie müssen die Windows-Quell-VHD oder -VHDX in eine VHD mit fester Größe konvertieren. Führen Sie die folgenden Schritte aus:

1. Öffnen Sie Hyper-V-Manager auf dem Clientsystem. Navigieren Sie zu **Datenträger bearbeiten**.

    ![Öffnen von Hyper-V-Manager](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/convert-fixed-vhd-1.png)

1. Wählen Sie auf der Seite **Vorbereitung** die Option **Weiter >** aus.

1. Wechseln Sie auf der Seite **Virtuelle Festplatte suchen** zum Speicherort der zu konvertierenden Windows-Quell-VHD oder -VHDX. Wählen Sie **Weiter >** aus.

    ![Seite „Virtuelle Festplatte suchen“](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/convert-fixed-vhd-2.png)

1. Wählen Sie auf der Seite **Aktion auswählen** die Option **Konvertieren** und dann **Weiter >** aus.

    ![Seite „Aktion auswählen“](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/convert-fixed-vhd-3.png)

1. Wählen Sie auf der Seite **Datenträgerformat auswählen** das Format **VHD** und dann **Weiter >** aus.

   ![Seite „Datenträgerformat auswählen“](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/convert-fixed-vhd-4.png)


1. Wählen Sie auf der Seite **Datenträgertyp auswählen** die Option **Feste Größe** und dann **Weiter >** aus.

   ![Seite „Datenträgertyp auswählen“](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/convert-fixed-vhd-5.png)


1. Navigieren Sie auf der Seite **Datenträger konfigurieren** zum Speicherort, und geben Sie einen Namen für den VHD-Datenträger mit fester Größe an. Wählen Sie **Weiter >** aus.

   ![Seite „Datenträger konfigurieren“](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/convert-fixed-vhd-6.png)


1. Überprüfen Sie die Zusammenfassung, und wählen Sie **Fertig stellen** aus. Die Konvertierung der VHD oder VHDX dauert einige Minuten. Die Konvertierungszeit hängt von der Größe des Quelldatenträgers ab. 

<!--
1. Run PowerShell on your Windows client.
1. Run the following command:

    ```powershell
    Convert-VHD -Path <source VHD path> -DestinationPath <destination-path.vhd> -VHDType Fixed 
    ```
-->
Diese VHD mit fester Größe wird für alle nachfolgenden Schritte in diesem Artikel verwendet.


## <a name="create-a-hyper-v-vm-from-fixed-vhd"></a>Erstellen einer Hyper-V-VM über die VHD mit fester Größe

1. Klicken Sie im **Hyper-V-Manager** im Bereichsfenster mit der rechten Maustaste auf den Systemknoten, um das Kontextmenü zu öffnen. Wählen Sie dann **Neu** > **Virtueller Computer** aus.

    ![Auswählen einer neuen VM im Bereichsfenster](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/create-virtual-machine-1.png)

1. Wählen Sie auf der Seite **Vorbereitung** des Assistenten für neue VMs die Option **Weiter** aus.

1. Geben Sie auf der Seite **Namen und Speicherort angeben** einen **Namen** und einen **Speicherort** für die VM an. Wählen Sie **Weiter** aus.

    ![Angeben des Namens und Speicherorts für die VM](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/create-virtual-machine-2.png)

1. Wählen Sie auf der Seite **Generation angeben** die Option **Generation 1** für den VHD-Imagetyp des Geräts und dann **Weiter** aus.    

    ![Angeben der Generation](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/create-virtual-machine-3.png)

1. Weisen Sie die gewünschten Speicher- und Netzwerkkonfigurationen zu.

1. Wählen Sie auf der Seite **Virtuelle Festplatte verbinden** die Option **Vorhandene virtuelle Festplatte verwenden** aus. Geben Sie den Speicherort der zuvor erstellten Windows-VHD mit fester Größe an, und wählen Sie dann **Weiter** aus.

    ![Seite „Virtuelle Festplatte verbinden“](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/create-virtual-machine-4.png)

1. Überprüfen Sie die **Zusammenfassung**, und wählen Sie dann **Fertig stellen** aus, um die VM zu erstellen.

Die Erstellung der VM dauert einige Minuten.
     

## <a name="connect-to-the-hyper-v-vm"></a>Herstellen einer Verbindung mit der Hyper-V-VM

Die VM wird in der Liste der VMs in Ihrem Clientsystem angezeigt. 


1. Wählen Sie die VM aus, klicken Sie mit der rechten Maustaste, und wählen Sie **Starten** aus. 

    ![Auswählen und Starten der VM](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/connect-virtual-machine-2.png)

2. Die VM wird als **Wird ausgeführt** angezeigt. Wählen Sie die VM aus, klicken Sie mit der rechten Maustaste, und wählen Sie **Verbinden** aus.

    ![Herstellen einer Verbindung mit dem virtuellen Computer](./media/azure-stack-edge-gpu-prepare-windows-vhd-generalized-image/connect-virtual-machine-4.png)

Nachdem die Verbindung mit der VM hergestellt wurde, schließen Sie den Assistenten zum Einrichten des Computers ab, und melden Sie sich bei der VM an.


## <a name="generalize-the-vhd"></a>Generalisieren der VHD  

Verwenden Sie das Hilfsprogramm *sysprep*, um die VHD zu generalisieren. 

1. Öffnen Sie in der VM eine Eingabeaufforderung.
1. Führen Sie den folgenden Befehl aus, um die VM zu generalisieren. 

    ```
    c:\windows\system32\sysprep\sysprep.exe /oobe /generalize /shutdown /mode:vm
    ```
    Weitere Informationen finden Sie unter [Übersicht über Sysprep (Systemvorbereitung)](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).
1.  Nachdem der Befehl ausgeführt wurde, wird die VM heruntergefahren. **Starten Sie den virtuellen Computer nicht neu**.

## <a name="upload-the-vhd-to-azure-blob-storage"></a>Hochladen der VHD in Azure Blob Storage

Die VHD kann nun dazu verwendet werden, ein generalisiertes Image in Azure Stack Edge zu erstellen. 

1. Laden Sie die VHD in Azure Blob Storage hoch. Detaillierte Anweisungen finden Sie unter [Hochladen einer VHD-Datei mithilfe des Microsoft Azure Storage-Explorers](../devtest-labs/devtest-lab-upload-vhd-using-storage-explorer.md).
1. Nach Abschluss des Uploads können Sie das hochgeladene Image verwenden, um VM-Images und VMs zu erstellen. 

<!-- this should be added to deploy VM articles - If you experience any issues creating VMs from your new image, you can use VM console access to help troubleshoot. For information on console access, see [link].-->



## <a name="next-steps"></a>Nächste Schritte

Abhängig von der Art der Bereitstellung können Sie eine der folgenden Vorgehensweisen wählen.

- [Bereitstellen einer VM über ein generalisiertes Image über das Azure-Portal](azure-stack-edge-gpu-deploy-virtual-machine-portal.md)
- [Bereitstellen einer VM über ein generalisiertes Image mit Azure PowerShell](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md)  
