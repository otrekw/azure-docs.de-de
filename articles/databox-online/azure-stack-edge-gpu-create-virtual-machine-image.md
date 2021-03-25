---
title: Erstellen von VM-Images für Ihr Azure Stack Edge Pro-GPU-Gerät
description: Hier wird beschrieben, wie Sie Linux- oder Windows-VM-Images für die Verwendung mit Ihrem Azure Stack Edge Pro-GPU-Gerät erstellen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: ee0587063c0fac67068869c58471ada58354fab7
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102437671"
---
# <a name="create-custom-vm-images-for-your-azure-stack-edge-pro-device"></a>Erstellen benutzerdefinierter VM-Images für Ihr Azure Stack Edge Pro-Gerät

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Wenn Sie virtuelle Computer auf Ihrem Azure Stack Edge Pro-Gerät bereitstellen möchten, müssen Sie in der Lage sein, benutzerdefinierte VM-Images zu erstellen, mit denen Sie wiederum virtuelle Computer erstellen können. In diesem Artikel werden die Schritte beschrieben, die Sie ausführen müssen, um benutzerdefinierte Linux- oder Windows-VM-Images zu erstellen, mit denen Sie virtuelle Computer auf Ihrem Azure Stack Edge Pro-Gerät bereitstellen können.

## <a name="vm-image-workflow"></a>Workflow für VM-Images

Der Workflow erfordert, dass Sie einen virtuellen Computer in Azure erstellen, den virtuellen Computer anpassen und generalisieren und dann die virtuelle Festplatte (VHD) für diesen virtuellen Computer herunterladen. Diese generalisierte VHD wird in Azure Stack Edge Pro hochgeladen. Es wird ein verwalteter Datenträger auf Grundlage dieser VHD erstellt. Auf Grundlage des verwalteten Datenträgers wird ein Image erstellt. Zum Schluss werden virtuelle Computer auf Grundlage dieses Images erstellt.

Weitere Informationen finden Sie unter [Bereitstellen von VMs auf Ihrem Azure Stack Edge Pro-Gerät mithilfe von Azure PowerShell](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md).


## <a name="create-a-windows-custom-vm-image"></a>Erstellen eines benutzerdefinierten Windows-VM-Images

Führen Sie die folgenden Schritte aus, um ein Windows-VM-Image zu erstellen:

1. Erstellen Sie einen virtuellen Windows-Computer. Weitere Informationen finden Sie unter [Tutorial: Erstellen und Verwalten von virtuellen Windows-Computern mit Azure PowerShell](../virtual-machines/windows/tutorial-manage-vm.md).

2. Laden Sie einen vorhandenen Betriebssystemdatenträger herunter.

    - Führen Sie die Schritte unter [Herunterladen einer Windows-VHD von Azure](../virtual-machines/windows/download-vhd.md) aus.

    - Verwenden Sie den folgenden `sysprep`-Befehl anstelle des vorherigen Verfahrens.
    
        `c:\windows\system32\sysprep\sysprep.exe /oobe /generalize /shutdown /mode:vm`
   
       Sie können auch die Informationen unter [Übersicht über Sysprep (Systemvorbereitung)](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview) lesen.

Verwenden Sie diese virtuelle Festplatte, um jetzt einen virtuellen Computer zu erstellen und auf Ihrem Azure Stack Edge Pro-Gerät bereitzustellen.

## <a name="create-a-linux-custom-vm-image"></a>Erstellen eines benutzerdefinierten Linux-VM-Images

Führen Sie die folgenden Schritte aus, um ein Linux-VM-Image zu erstellen:

1. Erstellen eines virtuellen Linux-Computers. Weitere Informationen finden Sie unter [Tutorial: Erstellen und Verwalten virtueller Linux-Computer mit der Azure-Befehlszeilenschnittstelle](../virtual-machines/linux/tutorial-manage-vm.md).

1. Heben Sie die Bereitstellung der VM auf. Verwenden Sie den Azure VM-Agent zum Löschen computerspezifischer Dateien und Daten. Verwenden Sie auf dem virtuellen Linux-Quellcomputer den Befehl `waagent` mit dem Parameter `-deprovision+user`. Weitere Informationen finden Sie unter [Verstehen und Verwenden des Azure Linux-Agents](../virtual-machines/extensions/agent-linux.md).

    1. Stellen Sie mit einem SSH-Client eine Verbindung mit Ihrem virtuellen Linux-Computer her.
    2. Geben Sie im SSH-Fenster den folgenden Befehl ein:
       
        ```bash
        sudo waagent -deprovision+user
        ```
       > [!NOTE]
       > Führen Sie diesen Befehl nur auf einem virtuellen Computer aus, den Sie als Image erfassen möchten. Die Ausführung dieses Befehls garantiert nicht, dass alle vertraulichen Informationen aus dem Image gelöscht werden oder dass es für eine erneute Verteilung geeignet ist. Der Parameter `+user` entfernt auch das zuletzt bereitgestellte Benutzerkonto. Wenn die Anmeldeinformationen des Benutzerkontos auf dem virtuellen Computer verbleiben sollen, verwenden Sie nur `-deprovision`.
     
    3. Geben Sie **y** ein, um fortzufahren. Sie können den Parameter `-force` hinzufügen, um diesen Bestätigungsschritt zu vermeiden.
    4. Geben Sie nach der Ausführung dieses Befehls den Befehl **exit** ein, um den SSH-Client zu schließen.  Der virtuelle Computer wird zu diesem Zeitpunkt immer noch ausgeführt.


1. [Laden Sie einen vorhandenen Betriebssystemdatenträger herunter.](../virtual-machines/linux/download-vhd.md)

Verwenden Sie diese virtuelle Festplatte, um jetzt einen virtuellen Computer zu erstellen und auf Ihrem Azure Stack Edge Pro-Gerät bereitzustellen. Sie können die folgenden zwei Azure Marketplace-Images verwenden, um benutzerdefinierte Linux-Images zu erstellen:

|Name des Elements  |BESCHREIBUNG  |Herausgeber  |
|---------|---------|---------|
|[Ubuntu Server](https://azuremarketplace.microsoft.com/marketplace/apps/canonical.ubuntuserver) |Ubuntu Server ist das weltweit beliebteste Linux für Cloudumgebungen.|Canonical|
|[Debian 8 „Jessie“](https://azuremarketplace.microsoft.com/marketplace/apps/credativ.debian) |Debian GNU/Linux ist eine der am häufigsten verwendeten Linux-Distributionen.     |credativ|

Eine vollständige Liste der Azure Marketplace-Images, die funktionieren könnten (derzeit noch nicht getestet), finden Sie unter [Für Azure Stack Hub verfügbare Azure Marketplace-Elemente](/azure-stack/operator/azure-stack-marketplace-azure-items?view=azs-1910&preserve-view=true).


## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen virtueller Computer auf Ihrem Azure Stack Edge Pro-Gerät](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md)