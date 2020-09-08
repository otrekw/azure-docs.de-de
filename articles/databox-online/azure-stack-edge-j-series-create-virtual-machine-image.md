---
title: Erstellen von VM-Images für Ihr Azure Stack Edge-GPU-Gerät
description: Hier erfahren Sie, wie Sie Linux- oder Windows-VM-Images für die Verwendung mit Ihrem Azure Stack Edge-GPU-Gerät erstellen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: 83332c3bfa0b2b99d7333fa679fb8d398aecf8bd
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89268909"
---
# <a name="create-custom-vm-images-for-your-azure-stack-edge-device"></a>Erstellen benutzerdefinierter VM-Images für Ihr Azure Stack Edge-Gerät

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

Wenn Sie virtuelle Computer auf Ihrem Azure Stack Edge-Gerät bereitstellen möchten, müssen Sie in der Lage sein, benutzerdefinierte VM-Images zu erstellen, mit denen Sie wiederum virtuelle Computer erstellen können. In diesem Artikel werden die Schritte beschrieben, die zum Erstellen von benutzerdefinierten Linux- oder Windows-VM-Images erforderlich sind, mit denen Sie virtuelle Computer auf Ihrem Azure Stack Edge-Gerät bereitstellen können.

## <a name="vm-image-workflow"></a>Workflow für VM-Images

Der Workflow erfordert, dass Sie einen virtuellen Computer in Azure erstellen, den virtuellen Computer anpassen und generalisieren und dann die virtuelle Festplatte (VHD) für diesen virtuellen Computer herunterladen. Diese generalisierte virtuelle Festplatte wird in Azure Stack Edge hochgeladen, und ein verwalteter Datenträger wird auf der Grundlage dieser virtuellen Festplatte und das Image auf der Grundlage des verwalteten Datenträgers erstellt. Schließlich werden virtuelle Computer auf der Grundlage dieses Images erstellt.   

Weitere Informationen finden Sie unter [Bereitstellen virtueller Computer auf Ihrem Azure Stack Edge-GPU-Gerät über Azure PowerShell](azure-stack-edge-j-series-deploy-virtual-machine-powershell.md).


## <a name="create-a-windows-custom-vm-image"></a>Erstellen eines benutzerdefinierten Windows-VM-Images

Führen Sie die folgenden Schritte aus, um ein Windows-VM-Image zu erstellen:

1. Erstellen Sie einen virtuellen Windows-Computer. Weitere Informationen finden Sie unter [Tutorial: Erstellen und Verwalten von virtuellen Windows-Computern mit Azure PowerShell](../virtual-machines/windows/tutorial-manage-vm.md).

2. Laden Sie einen vorhandenen Betriebssystemdatenträger herunter.

    - Führen Sie die Schritte unter [Herunterladen einer Windows-VHD von Azure](../virtual-machines/windows/download-vhd.md) aus.

    - Verwenden Sie den folgenden `sysprep`-Befehl anstelle des vorherigen Verfahrens.
    
        `c:\windows\system32\sysprep\sysprep.exe /oobe /generalize /shutdown /mode:vm`
   
       Sie können auch die Informationen unter [Übersicht über Sysprep (Systemvorbereitung)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview) lesen.

Verwenden Sie diese virtuelle Festplatte, um jetzt einen virtuellen Computer zu erstellen und auf dem Azure Stack Edge-Gerät bereitzustellen.

## <a name="create-a-linux-custom-vm-image"></a>Erstellen eines benutzerdefinierten Linux-VM-Images

Führen Sie die folgenden Schritte aus, um ein Linux-VM-Image zu erstellen:

1. Erstellen eines virtuellen Linux-Computers. Weitere Informationen finden Sie unter [Tutorial: Erstellen und Verwalten virtueller Linux-Computer mit der Azure-Befehlszeilenschnittstelle](../virtual-machines/linux/tutorial-manage-vm.md).

2. [Laden Sie einen vorhandenen Betriebssystemdatenträger herunter.](../virtual-machines/linux/download-vhd.md)

Verwenden Sie diese virtuelle Festplatte, um jetzt einen virtuellen Computer zu erstellen und auf dem Azure Stack Edge-Gerät bereitzustellen. Sie können die folgenden zwei Azure Marketplace-Images verwenden, um benutzerdefinierte Linux-Images zu erstellen:

|Name des Elements  |BESCHREIBUNG  |Herausgeber  |
|---------|---------|---------|
|[Ubuntu Server](https://azuremarketplace.microsoft.com/marketplace/apps/canonical.ubuntuserver) |Ubuntu Server ist das weltweit beliebteste Linux für Cloudumgebungen.|Canonical|
|[Debian 8 „Jessie“](https://azuremarketplace.microsoft.com/marketplace/apps/credativ.debian) |Debian GNU/Linux ist eine der am häufigsten verwendeten Linux-Distributionen.     |credativ|

Eine vollständige Liste der Azure Marketplace-Images, die funktionieren könnten (derzeit noch nicht getestet), finden Sie unter [Für Azure Stack Hub verfügbare Azure Marketplace-Elemente](https://docs.microsoft.com/azure-stack/operator/azure-stack-marketplace-azure-items?view=azs-1910).


## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen virtueller Computer auf Ihrem Azure Stack Edge-Gerät](azure-stack-edge-j-series-deploy-virtual-machine-powershell.md)
