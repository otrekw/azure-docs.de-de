---
title: Bereitstellen von VMs auf einem Azure Stack Edge Pro GPU-Gerät über das Azure-Portal
description: Erfahren Sie, wie Sie VMs auf Azure Stack Edge Pro GPU über das Azure-Portal erstellen und verwalten.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 05/14/2021
ms.author: alkohli
ms.openlocfilehash: 79cc482d5bbd32cd4ba4efbce692d32ecc0cad8e
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112081315"
---
# <a name="deploy-vms-on-your-azure-stack-edge-pro-gpu-device-via-the-azure-portal"></a>Bereitstellen von VMs auf einem Azure Stack Edge Pro-GPU-Gerät über das Azure-Portal

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Sie können virtuelle Computer (Virtual Machines, VMs) auf einem Azure Stack Edge Pro GPU-Gerät über das Azure-Portal, Vorlagen und Azure PowerShell-Cmdlets sowie über die Azure CLI bzw. Python-Skripts erstellen und verwalten. In diesem Artikel wird beschrieben, wie Sie eine VM auf Ihrem Azure Stack Edge Pro GPU-Gerät über das Azure-Portal erstellen und verwalten. 

> [!IMPORTANT] 
> Es wird empfohlen, für Benutzer, die auf Ihrem Gerät bereitgestellte VMs über die Cloud verwalten, die mehrstufige Authentifizierung zu aktivieren.
        
## <a name="vm-deployment-workflow"></a>VM-Bereitstellungsworkflow

Die allgemeine Zusammenfassung des Bereitstellungsworkflows lautet wie folgt:

1. Aktivieren einer Netzwerkschnittstelle für Compute auf dem Azure Stack Edge-Gerät In diesem Schritt wird ein virtueller Switch an der angegebenen Netzwerkschnittstelle erstellt.
1. Aktivieren Sie die Cloudverwaltung von virtuellen Computern über das Azure-Portal.
1. Laden Sie mit dem Azure Storage-Explorer eine VHD in ein Azure Storage-Konto hoch. 
1. Verwenden Sie die hochgeladene VHD, um diese auf das Gerät herunterzuladen und ein VM-Image von der VHD zu erstellen. 
1. Verwenden Sie die Ressourcen, die Sie in den vorherigen Schritten erstellt haben:
    1. VM-Image, das Sie erstellt haben
    1. Virtueller Switch, der der Netzwerkschnittstelle zugeordnet ist, an der Sie Compute aktiviert haben
    1. Subnetz, das dem virtuellen Switch zugeordnet ist

    Und erstellen Sie die folgenden Ressourcen inline, oder geben Sie diese inline an:
    1. VM-Name – wählen Sie eine unterstützte VM-Größe und Anmeldeinformationen für die VM aus. 
    1. Erstellen Sie neue Datenträger, oder fügen Sie vorhandene Datenträger an.
    1. Konfigurieren Sie eine statische oder dynamische IP-Adresse für die VM. Wenn Sie eine statische IP-Adresse bereitstellen, wählen Sie eine freie IP-Adresse im Subnetzbereich der für Compute aktivierten Netzwerkschnittstelle aus.

    Verwenden Sie die oben genannten Ressourcen, um einen virtuellen Computer zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit dem Erstellen und Verwalten von VMs auf Ihrem Gerät über das Azure-Portal beginnen, stellen Sie Folgendes sicher:

1. Sie haben die Netzwerkeinstellungen auf Ihrem Azure Stack Edge Pro GPU-Gerät wie unter [Schritt 1: Konfigurieren eines Azure Stack Edge Pro GPU-Geräts](./azure-stack-edge-gpu-connect-resource-manager.md#step-1-configure-azure-stack-edge-pro-device) beschrieben festgelegt.

    1. Sie haben eine Netzwerkschnittstelle für Compute aktiviert. Diese IP-Adresse der Netzwerkschnittstelle wird verwendet, um einen virtuellen Switch für die VM-Bereitstellung zu erstellen. Navigieren Sie auf der lokalen Benutzeroberfläche Ihres Geräts zu **Compute**. Wählen Sie die Netzwerkschnittstelle aus, die Sie verwenden möchten, um einen virtuellen Switch zu erstellen.

        > [!IMPORTANT] 
        > Sie können nur einen Port für Compute konfigurieren.

    1. Aktivieren Sie Compute für die Netzwerkschnittstelle. Azure Stack Edge Pro GPU erstellt und verwaltet einen virtuellen Switch, der dieser Netzwerkschnittstelle entspricht.

1. Sie haben Zugriff auf eine Windows- oder Linux-VHD, die Sie verwenden, um das VM-Image für den virtuellen Computer zu erstellen, den Sie erstellen möchten.

## <a name="deploy-a-vm"></a>Bereitstellen einer VM

Führen Sie die folgenden Schritte aus, um auf Ihrem Azure Stack Edge Pro GPU-Gerät eine VM zu erstellen.

### <a name="add-a-vm-image"></a>Hinzufügen eines VM-Images

1. Laden Sie eine VHD in ein Azure Storage-Konto hoch. Folgen Sie den Schritten unter [Verwenden des Storage-Explorer für den Upload](azure-stack-edge-gpu-deploy-virtual-machine-templates.md#use-storage-explorer-for-upload).

   Informationen zum Vorbereiten der VHD finden Sie unter [Vorbereiten eines generalisierten Images von einer Windows-VHD](azure-stack-edge-gpu-prepare-windows-vhd-generalized-image.md).

1. Wechseln Sie im Azure-Portal zur Azure Stack Edge-Ressource für Ihr Gerät. Wechseln Sie zu **Edgedienste** > **VMs**.

    ![Screenshot: Edgedienste und VMs](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-1.png)

1. Wählen Sie **Virtuelle Computer** aus, um zur Seite **Übersicht** zu wechseln. Wählen Sie **Aktivieren** aus, um die Cloudverwaltung für virtuelle Computer zu aktivieren.

    ![Screenshot der Seite „Übersicht“ mit Schaltfläche „Aktivieren“](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-2.png)

1. Der erste Schritt besteht darin, ein VM-Image hinzuzufügen. Sie haben im vorherigen Schritt bereits eine VHD in das Speicherkonto hochgeladen. Sie verwenden diese VHD, um ein VM-Image zu erstellen.

    Wählen Sie **Hinzufügen** aus, um die VHD aus dem Speicherkonto herunterzuladen und zum Gerät hinzuzufügen. Der Downloadvorgang dauert einige Minuten und ist von der Größe der VHD und der für den Download verfügbaren Internetbandbreite abhängig. 

    ![Screenshot der Seite „Übersicht“ mit Schaltfläche „Hinzufügen“](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-3.png)

1. Geben Sie im Bereich **Image hinzufügen** die folgenden Parameter ein. Wählen Sie **Hinzufügen**.


    |Parameter  |BESCHREIBUNG  |
    |---------|---------|
    |Download from storage blob (Aus Speicherblob herunterladen)    |Navigieren Sie zum Speicherort des Speicherblobs im Speicherkonto, in das Sie die VHD hochgeladen haben.         |
    |Herunterladen nach    | Wird automatisch auf das aktuelle Gerät festgelegt, auf dem Sie den virtuellen Computer bereitstellen.        |
    |Image speichern unter      | Der Name des VM-Images, das Sie von der VHD erstellen, die Sie in das Speicherkonto hochgeladen haben.        |
    |Betriebssystemtyp     |Wählen Sie Windows oder Linux als Betriebssystem der VHD aus, die Sie zum Erstellen des VM-Images verwenden.         |
   

    ![Screenshot der Seite „Image hinzufügen“ mit Schaltfläche „Hinzufügen“](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-6.png)

1. Die VHD wird heruntergeladen, und das VM-Image wird erstellt. Die Imageerstellung dauert einige Minuten. Sie werden benachrichtigt, sobald das VM-Image erfolgreich erstellt wurde.

    ![Screenshot mit der Benachrichtigung über den erfolgreichen Abschluss](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-8.png)


1. Nachdem das VM-Image erfolgreich erstellt wurde, wird es der Liste der Images im Bereich **Images** hinzugefügt.

    ![Screenshot des Bereichs „Images“](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-9.png)

    Der Bereich **Bereitstellungen** wird aktualisiert, um den Status der Bereitstellung widerzuspiegeln.

    ![Screenshot des Bereichs „Bereitstellungen“](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-10.png)

    Das neu hinzugefügte Image wird auch auf der Seite **Übersicht** angezeigt.

    ![Screenshot der Seite „Übersicht“ mit dem Image](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-11.png)


### <a name="add-a-vm"></a>Hinzufügen eines virtuellen Computers

Führen Sie diese Schritte aus, um eine VM zu erstellen, nachdem Sie ein VM-Image erstellt haben.

1. Wählen Sie auf der Seite **Übersicht** die Option **Virtuellen Computer hinzufügen** aus.

    ![Screenshot der Seite „Übersicht“ mit der Schaltfläche „Virtuellen Computer hinzufügen“](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-1.png)

1. Geben Sie auf der Registerkarte **Grundlagen** die folgenden Parameter ein.


    |Parameter |BESCHREIBUNG  |
    |---------|---------|
    |Name des virtuellen Computers     |         |
    |Edgeressourcengruppe     | Erstellen Sie eine neue Ressourcengruppe für alle Ressourcen, die der VM zugeordnet sind.        |
    |Image     | Wählen Sie aus den VM-Images aus, die auf dem Gerät verfügbar sind.        |
    |Size     | Wählen Sie aus den [Unterstützten VM-Größen](azure-stack-edge-gpu-virtual-machine-sizes.md) aus.        |
    |Username     | Verwenden Sie den Standardbenutzernamen **azureuser** für den Administrator, um sich bei der VM anzumelden.        |
    |Authentifizierungsart    | Wählen Sie einen öffentlichen SSH-Schlüssel oder ein benutzerdefiniertes Kennwort aus.       |
    |Kennwort     | Geben Sie ein Kennwort zur Anmeldung bei der VM ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die definierten [Komplexitätsanforderungen](../virtual-machines/windows/faq.yml#what-are-the-password-requirements-when-creating-a-vm-) erfüllen.        |
    |Kennwort bestätigen    | Geben Sie das Kennwort erneut ein.        |


    ![Screenshot der Registerkarte „Grundlagen“](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-basics-1.png)

    Klicken Sie auf **Weiter: Datenträger**.

1. Auf der Registerkarte **Datenträger** fügen Sie Datenträger an Ihre VM an. 
    
    1. Sie können **Neuen Datenträger erstellen und anfügen** oder **Vorhandenen Datenträger anfügen** auswählen.

        ![Screenshot mit der Registerkarte „Datenträger“](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-disks-1.png)

    1. Wählen Sie **Neuen Datenträger erstellen und anfügen** aus. Geben Sie im Bereich **Neuen Datenträger erstellen** einen Namen für den Datenträger und die Größe in GiB an.

        ![Screenshot der Registerkarte „Neuen Datenträger erstellen“](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-disks-2.png)

    1. Wiederholen Sie den vorherigen Vorgang, um weitere Datenträger hinzuzufügen. Nachdem die Datenträger erstellt wurden, werden sie auf der Registerkarte **Datenträger** angezeigt. Wählen Sie **Weiter: Netzwerk** aus.

1. Auf der Registerkarte **Netzwerk** konfigurieren Sie die Netzwerkkonnektivität für die VM.

    
    |Parameter  |BESCHREIBUNG |
    |---------|---------|
    |Virtuelles Netzwerk    | Wählen Sie in der Dropdownliste den virtuellen Switch aus, der auf Ihrem Azure Stack Edge-Gerät erstellt wurde, als Sie Compute an der Netzwerkschnittstelle aktiviert haben.    |
    |Subnet     | Dieses Feld wird automatisch mit dem Subnetz ausgefüllt, das der Netzwerkschnittstelle zugeordnet ist, an der Sie Compute aktiviert haben.         |
    |IP-Adresse     | Geben Sie eine statische oder dynamische IP-Adresse für die VM an. Die statische IP-Adresse sollte eine verfügbare, freie IP-Adresse aus dem angegebenen Subnetzbereich sein.        |

    ![Screenshot: Registerkarte „Netzwerk“](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-networking-1.png)

    Klicken Sie auf **Weiter: Erweitert**.

1. Auf der Registerkarte **Erweitert** können Sie die benutzerdefinierten Daten oder cloud-init angeben, um Ihre VM anzupassen. 

    Mit cloud-init können Sie eine VM beim ersten Start anpassen. Verwenden Sie cloud-init zum Installieren von Paketen und Schreiben von Dateien oder zum Konfigurieren von Benutzern und Sicherheit. Da cloud-init während des ersten Startvorgangs ausgeführt wird, sind keine weiteren Schritte zum Anwenden Ihrer Konfiguration erforderlich. Weitere Informationen zu cloud-init finden Sie unter [Übersicht zu cloud-init](../virtual-machines/linux/tutorial-automate-vm-deployment.md#cloud-init-overview).

    ![Screenshot der Registerkarte „Erweitert“](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-advanced-1.png)

    Klicken Sie auf **Weiter: Überprüfen + erstellen**.

1. Überprüfen Sie auf der Registerkarte **Überprüfen + erstellen** die Angaben für die VM. Klicken Sie anschließend auf **Erstellen**.

    ![Screenshot der Registerkarte „Überprüfen + erstellen“](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-review-create-1.png)

1. Die Erstellung der VM wird gestartet und kann bis zu 20 Minuten dauern. Sie können zu **Bereitstellungen** wechseln, um die VM-Erstellung zu überwachen.

    ![Screenshot der Seite „Bereitstellungen“](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-deployments-page-1.png)

    
1. Nachdem die VM erfolgreich erstellt wurde, wird die Seite **Übersicht** aktualisiert, damit die neue VM angezeigt wird.

    ![Screenshot der Seite „Übersicht“ mit aufgeführter neuer VM](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-overview-page-1.png)

1. Wählen Sie die neu erstellte VM aus, um zu **Virtuelle Computer** zu wechseln.

    ![Screenshot zur Auswahl der neuen VM](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-page-1.png)

    Wählen Sie die VM aus, um die Details anzuzeigen.

    ![Screenshot zu den VM-Details](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-details-1.png)

    Sie verwenden die IP-Adresse für die Netzwerkschnittstelle, um eine Verbindung mit der VM herzustellen.

## <a name="connect-to-a-vm"></a>Herstellen einer Verbindung mit einem virtuellen Computer

Je nachdem, ob Sie eine Linux- oder Windows-VM erstellt haben, können die Schritte abweichen. Sie können keine Verbindung mit den auf Ihrem Gerät bereitgestellten VMs über das Azure-Portal herstellen. Befolgen Sie die Schritte, um eine Verbindung mit Ihrer Linux- oder Windows-VM herzustellen.

### <a name="connect-to-a-linux-vm"></a>Herstellen einer Verbindung mit einem virtuellen Linux-Computer

Führen Sie diese Schritte aus, um eine Verbindung mit einer Linux-VM herzustellen.

[!INCLUDE [azure-stack-edge-gateway-connect-vm](../../includes/azure-stack-edge-gateway-connect-virtual-machine-linux.md)]

### <a name="connect-to-a-windows-vm"></a>Herstellen einer Verbindung mit einem virtuellen Windows-Computer

Führen Sie diese Schritte aus, um eine Verbindung mit einer Windows-VM herzustellen.

[!INCLUDE [azure-stack-edge-gateway-connect-vm](../../includes/azure-stack-edge-gateway-connect-virtual-machine-windows.md)]

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Verwalten Ihres Azure Stack Edge Pro GPU-Geräts finden Sie unter [Verwenden der lokalen Webbenutzeroberfläche zur Verwaltung eines Azure Stack Edge Pro GPU-Geräts](azure-stack-edge-manage-access-power-connectivity-mode.md).