---
title: Azure Lab Services – Hochladen eines benutzerdefinierten Windows-Images in Shared Image Gallery
description: Beschreibt, wie ein benutzerdefiniertes Windows-Image in Shared Image Gallery hochgeladen wird.
ms.date: 09/30/2020
ms.topic: how-to
ms.openlocfilehash: 87ae45819a19390b1776cc81e45b85b4ba1e2aee
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111967784"
---
# <a name="bring-a-windows-custom-image-to-shared-image-gallery"></a>Hochladen eines benutzerdefinierten Windows-Images in Shared Image Gallery

Sie können Shared Image Gallery verwenden, um Ihre eigenen benutzerdefinierten Windows-Images hochzuladen und diese Images zum Erstellen von Labs in Azure Lab Services zu verwenden.  In diesem Artikel wird gezeigt, wie Sie ein benutzerdefiniertes Image hochladen:

* Aus Ihrer [physischen Laborumgebung](upload-custom-image-shared-image-gallery.md#bring-a-windows-custom-image-from-a-physical-lab-environment).
* Von eine [virtuellen Azure-Computer](upload-custom-image-shared-image-gallery.md#bring-a-windows-custom-image-from-an-azure-virtual-machine).

Diese Aufgabe wird in der Regel von der IT-Abteilung einer Bildungseinrichtung ausgeführt.

## <a name="bring-a-windows-custom-image-from-a-physical-lab-environment"></a>Hochladen eines benutzerdefinierten Images aus einer physischen Laborumgebung

Die Schritte in diesem Absatz zeigen, wie Sie ein benutzerdefiniertes Image importieren, das aus Ihrer physischen Laborumgebung gestartet wird.  Bei diesem Ansatz erstellen Sie eine VHD aus Ihrer physischen Umgebung und importieren die VHD in Shared Image Gallery, damit sie in Lab Services verwendet werden kann.  Dieser Ansatz hat unter anderem folgende Vorteile:

* Sie können flexibel [generalisierte oder spezialisierte](../virtual-machines/shared-image-galleries.md#generalized-and-specialized-images) Images erstellen, die Sie in Ihren Labs verwenden können.  Wenn Sie die [Vorlagen-VM eines Labs verwenden, um ein Image zu exportieren](how-to-use-shared-image-gallery.md), ist das Image jedoch immer spezialisiert.
* Sie können Images hochladen, die mit anderen Tools wie [Microsoft Endpoint Configuration Manager](/mem/configmgr/core/understand/introduction) erstellt wurden, damit Sie ein Image nicht manuell mithilfe der Vorlagen-VM eines Labs einrichten müssen.

Die Schritte in diesem Abschnitt erfordern, dass Sie über die Berechtigung zum Erstellen eines [verwalteten Datenträgers](../virtual-machines/managed-disks-overview.md) im Azure-Abonnement Ihrer Bildungseinrichtung verfügen.

> [!IMPORTANT]
> Wenn Sie Images aus einer physischen Laborumgebung in Lab Services verschieben, sollten Sie jedes Image so umstrukturieren, dass es nur Software enthält, die für den Unterricht eines Labs erforderlich ist.  Weitere Informationen finden Sie im Blogbeitrag [Moving from a Physical Lab to Azure Lab Services](https://techcommunity.microsoft.com/t5/azure-lab-services/moving-from-a-physical-lab-to-azure-lab-services/ba-p/1654931) (Der Weg von einem physischen Lab zu Azure Lab Services).

Viele Optionen sind für das Erstellen einer virtuellen Festplatte aus einer physischen Laborumgebung verfügbar. Die folgenden Schritte zeigen, wie Sie eine VHD aus einem virtuellen Windows Hyper-V-Computer (VM) mithilfe des Hyper-V-Managers erstellen.

1. Beginnen Sie mit einer Hyper-V-VM in ihrer physischen Laborumgebung, die aus dem Image erstellt wurde.  Weitere Informationen finden Sie im Artikel zum [Erstellen eines virtuellen Computers in Hyper-V](/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v).
    1. Die VM muss als VM der **Generation 1** erstellt werden.
    1. Der virtuelle Datenträger der VM muss eine VHD fester Größe sein.  Die Datenträgergröße darf *nicht* über 128 GB liegen. Geben Sie beim Erstellen der VM die Größe des Datenträgers ein, wie in der folgenden Abbildung dargestellt.
       
        :::image type="content" source="./media/upload-custom-image-shared-image-gallery/connect-virtual-hard-disk.png" alt-text="Virtuelle Festplatte verbinden":::

    > [!IMPORTANT]
    > Images mit einer Datenträgergröße über 128 GB werden von Azure Lab Services *nicht* unterstützt. 
   
1. Führen Sie die folgenden Schritte aus, um eine Verbindung mit der Hyper-V-VM herzustellen und sie [für Azure vorzubereiten](../virtual-machines/windows/prepare-for-upload-vhd-image.md):
    1. [Legen Sie die Windows-Konfigurationen für Azure fest](../virtual-machines/windows/prepare-for-upload-vhd-image.md#set-windows-configurations-for-azure).
    1. [Überprüfen Sie die Windows-Dienste, die zum Sicherstellen der VM-Konnektivität erforderlich sind](../virtual-machines/windows/prepare-for-upload-vhd-image.md#check-the-windows-services).
    1. [Aktualisieren Sie die Remotedesktop-Registrierungseinstellungen](../virtual-machines/windows/prepare-for-upload-vhd-image.md#update-remote-desktop-registry-settings).
    1. [Konfigurieren Sie die Windows-Firewallregeln](../virtual-machines/windows/prepare-for-upload-vhd-image.md#configure-windows-firewall-rules).
    1. [Installieren Sie Windows-Updates](../virtual-machines/windows/prepare-for-upload-vhd-image.md).
    1. [Abschließen empfohlener Konfigurationen](../virtual-machines/windows/prepare-for-upload-vhd-image.md#complete-the-recommended-configurations) 
        
    Sie können spezialisierte oder generalisierte Images in Shared Image Gallery hochladen und diese zum Erstellen von Labs verwenden.  Mit den oben beschriebenen Schritten wird ein spezialisiertes Image erstellt. Wenn Sie stattdessen ein generalisiertes Image erstellen müssen, müssen Sie auch [SysPrep ausführen](../virtual-machines/windows/prepare-for-upload-vhd-image.md#determine-when-to-use-sysprep).  

    > [!IMPORTANT]
    > Sie sollten ein spezialisiertes Image erstellen, wenn Sie computerspezifische Informationen und Benutzerprofile verwalten möchten.  Weitere Informationen zu den Unterschieden zwischen generalisierten und spezialisierten Images finden Sie unter [Generalisierte und spezialisierte Images](../virtual-machines/shared-image-galleries.md#generalized-and-specialized-images).

1. Da **Hyper-V** standardmäßig eine **VHDX**-Datei erstellt, müssen Sie diese in eine VHD-Datei konvertieren.
    1. Navigieren Sie zu **Hyper-V-Manager** -> **Aktion** -> **Datenträger bearbeiten**.
    1. **Konvertieren** Sie nun den Datenträger von einer VHDX in eine VHD.  
     - Wenn Sie die Datenträgergröße erweitern, stellen Sie sicher, dass Sie 128 GB *nicht* überschreiten.        
        :::image type="content" source="./media/upload-custom-image-shared-image-gallery/choose-action.png" alt-text="Aktion auswählen":::   

    Weitere Informationen finden Sie im Artikel zum [Konvertieren des virtuellen Datenträgers in eine VHD mit fester Größe](../virtual-machines/windows/prepare-for-upload-vhd-image.md#convert-the-virtual-disk-to-a-fixed-size-vhd).

1. Laden Sie die VHD-Datei in Azure hoch, um einen verwalteten Datenträger zu erstellen.
    1. Sie können entweder Storage-Explorer oder AzCopy in der Befehlszeile verwenden, wie unter [Hochladen einer VHD in Azure oder Kopieren eines verwalteten Datenträgers in eine andere Region](../virtual-machines/windows/disks-upload-vhd-to-managed-disk-powershell.md) beschrieben.        

    1. Nachdem Sie die VHD hochgeladen haben, sollten Sie nun über einen verwalteten Datenträger verfügen, den Sie im Azure-Portal anzeigen können. 
    
    > [!WARNING]
    > Wenn Ihr Computer in den Standbymodus wechselt oder gesperrt wird, wird der Uploadvorgang möglicherweise unterbrochen und ein Fehler tritt auf. 
    
    > [!IMPORTANT] 
    > Auf Registerkarte **Größe und Leistung** des verwalteten Datenträgers im Azure-Portal können Sie die Datenträgergröße ändern. Wie bereits erwähnt, darf die Größe *nicht* größer als 128 GB sein.

1. Erstellen Sie eine Momentaufnahme des verwalteten Datenträgers.
    Dies kann entweder über PowerShell, mithilfe des Azure-Portals oder vom Storage-Explorer aus erfolgen, wie unter [Erstellen einer Momentaufnahme mithilfe des Portals oder PowerShell](../virtual-machines/windows/snapshot-copy-managed-disk.md) beschrieben.

1. Erstellen Sie in Shared Image Gallery eine Imagedefinition und -version:
    1. [Erstellen Sie eine Imagedefinition](../virtual-machines/windows/shared-images-portal.md#create-an-image-definition).  
     - Wählen Sie **Gen 1** als **VM-Generation** aus.
     - Wählen Sie aus, ob Sie ein **spezialisiertes** oder **generalisiertes** Image für den **Betriebssystemstatus** erstellen.
     
    Weitere Informationen zu den Werten, die Sie für eine Imagedefinition angeben können, finden Sie unter [Imagedefinitionen](../virtual-machines/shared-image-galleries.md#image-definitions). 
    
    > [!NOTE] 
    > Sie können auch eine vorhandene Imagedefinition verwenden und eine neue Version für Ihr benutzerdefiniertes Image erstellen.
    
1. [Erstellen Sie eine Imageversion](../virtual-machines/windows/shared-images-portal.md#create-an-image-version).
    - Die Eigenschaft **Versionsnummer** verwendet das folgende Format: *MajorVersion.MinorVersion.Patch*.   Wenn Sie Lab Services verwenden, um ein Lab zu erstellen und ein benutzerdefiniertes Image auswählen, wird automatisch die neueste Version des Images verwendet.  Die neueste Version wird basierend auf dem höchsten Wert von MajorVersion, MinorVersion und Patch ausgewählt.
    - Wählen Sie als **Quelle** die Option **Datenträger und/oder Momentaufnahmen** aus der Dropdownliste aus.
    - Wählen Sie für die Eigenschaft **Betriebssystemdatenträger** die Momentaufnahme aus, die Sie in den vorherigen Schritten erstellt haben.
    
    Weitere Informationen zu den Werten, die Sie für eine Imagedefinition angeben können, finden Sie unter [Imageversionen](../virtual-machines/shared-image-galleries.md#image-versions). 
   
1. [Erstellen Sie das Lab](tutorial-setup-classroom-lab.md) in Azure Lab Services, und wählen Sie das benutzerdefinierte Image in Shared Image Gallery aus.

    Wenn Sie den Datenträger *nach* der Installation des Betriebssystems auf der ursprünglichen Hyper-V-VM erweitert haben, müssen Sie auch das Laufwerk „C:“ in Windows so erweitern, dass der nicht zugeordnete Datenträgerspeicherplatz verwendet wird:      
    - Melden Sie sich bei der Vorlagen-VM des Labs an, und führen Sie ähnliche Schritte wie unter [Erweitern eines Basisvolumes](/windows-server/storage/disk-management/extend-a-basic-volume) beschrieben aus. 

## <a name="bring-a-windows-custom-image-from-an-azure-virtual-machine"></a>Verwenden eines benutzerdefinierten Windows-Images von einem virtuellen Azure-Computer
Ein weiterer Ansatz besteht in der Einrichtung Ihres Windows-Images mithilfe eines [virtuellen Azure-Computers](../virtual-machines/windows/overview.md).  Die Verwendung eines virtuellen Azure-Computers (VM) bietet Ihnen die Flexibilität, ein spezialisiertes oder generalisiertes Image zur Verwendung mit Ihren Labs zu erstellen.  Der Prozess zum Vorbereiten eines Images von einer Azure-VM ist einfacher als das [Erstellen eines Images aus einer VHD](upload-custom-image-shared-image-gallery.md#bring-a-windows-custom-image-from-a-physical-lab-environment), da das Image bereits für die Ausführung in Azure vorbereitet ist.

Sie benötigen Berechtigungen zum Erstellen einer Azure-VM im Azure-Abonnement Ihrer Bildungseinrichtung, um die folgenden Schritte ausführen zu können:

1. Erstellen Sie eine Azure-VM mithilfe des [Azure-Portals](../virtual-machines/windows/quick-create-portal.md), mit [PowerShell](../virtual-machines/windows/quick-create-powershell.md), mit der [Azure CLI](../virtual-machines/windows/quick-create-cli.md) oder aus einer [ARM-Vorlage](../virtual-machines/windows/quick-create-template.md).
    
    - Wenn Sie die Datenträgereinstellungen angeben, stellen Sie sicher, dass die Größe des Datenträgers *nicht* größer als 128 GB ist.
    
1. Installieren Sie Software, und nehmen Sie alle erforderlichen Konfigurationsänderungen am Image der Azure-VM vor.

1. Führen Sie [SysPrep](../virtual-machines/windows/capture-image-resource.md#generalize-the-windows-vm-using-sysprep) aus, wenn Sie ein generalisiertes Image erstellen möchten.  Weitere Informationen finden Sie unter [Generalisierte und spezialisierte Images](../virtual-machines/shared-image-galleries.md#generalized-and-specialized-images).

1. Erstellen Sie in Shared Image Gallery eine [Imagedefinition](../virtual-machines/windows/shared-images-portal.md#create-an-image-definition), oder wählen Sie eine vorhandene Imagedefinition aus.
     - Wählen Sie **Gen 1** als **VM-Generation** aus.
     - Wählen Sie aus, ob Sie ein **spezialisiertes** oder **generalisiertes** Image für den **Betriebssystemstatus** erstellen.
    
1. [Erstellen Sie eine Imageversion](../virtual-machines/windows/shared-images-portal.md#create-an-image-version).
    - Die Eigenschaft **Versionsnummer** verwendet das folgende Format: *MajorVersion.MinorVersion.Patch*.   
    - Wählen Sie als **Quelle** die Option **Datenträger und/oder Momentaufnahmen** aus der Dropdownliste aus.
    - Wählen Sie für die Eigenschaft **Betriebssystemdatenträger** den Datenträger Ihrer Azure-VM aus, die Sie in den vorherigen Schritten erstellt haben.

1. [Erstellen Sie das Lab](tutorial-setup-classroom-lab.md) in Azure Lab Services, und wählen Sie das benutzerdefinierte Image in Shared Image Gallery aus.

Sie können Ihr benutzerdefiniertes Image auch mithilfe von PowerShell von einer Azure-VM in Shared Image Gallery importieren.  Weitere Informationen finden Sie im folgenden Skript und in der zugehörigen Infodatei:
    
- [Hochladen eines Images in Shared Image Gallery mit einem Skript](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/BringImageToSharedImageGallery/)

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über Shared Image Gallery](../virtual-machines/shared-image-galleries.md)
* [Anfügen oder Trennen einer Shared Image Gallery](how-to-attach-detach-shared-image-gallery.md)
* [Verwenden eines Katalogs mit freigegebenen Images in Azure Lab Services](how-to-use-shared-image-gallery.md)