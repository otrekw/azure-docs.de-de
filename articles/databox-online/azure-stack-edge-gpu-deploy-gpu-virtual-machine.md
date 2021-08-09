---
title: Übersicht über GPU-VMs auf Ihrem Azure Stack Edge Pro-GPU-Gerät und deren Bereitstellung
description: Erfahren Sie, wie GPU-VMs auf einem Azure Stack Edge Pro-GPU-Gerät anhand von Vorlagen erstellt und verwaltet werden.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 05/28/2021
ms.author: alkohli
ms.openlocfilehash: 754cb296d6edebe4a8026df612fc52113e171a1c
ms.sourcegitcommit: df574710c692ba21b0467e3efeff9415d336a7e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110663874"
---
# <a name="deploy-gpu-vms-on-your-azure-stack-edge-pro-gpu-device"></a>Bereitstellen von GPU-VMs auf Ihrem Azure Stack Edge Pro-GPU-Gerät

[!INCLUDE [applies-to-GPU-and-pro-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-sku.md)]

Dieser Artikel enthält eine Übersicht über GPU-VMs (virtuelle Computer) auf Ihrem Azure Stack Edge Pro-GPU-Gerät. In diesem Artikel wird auch beschrieben, wie Sie eine GPU-VM mithilfe der Azure Resource Manager-Vorlagen erstellen. 


## <a name="about-gpu-vms"></a>Informationen zu GPU-VMs

Ihre Azure Stack Edge-Geräte können mit einer oder zwei Tesla T4-GPUs von Nvidia ausgestattet sein. Verwenden Sie für die Bereitstellung von VMs mit GPU-Beschleunigung auf diesen Geräten die GPU-optimierten VM-Größen. Beispielsweise sollte für das Bereitstellen von Rückschlussworkloads mit T4-GPUs die Serie NC T4 v3 verwendet werden. 

Weitere Informationen finden Sie unter [VMs der NC T4 v3-Serie](../virtual-machines/nct4-v3-series.md).

## <a name="supported-os-and-gpu-drivers"></a>Unterstützte Betriebssysteme und GPU-Treiber 

Um die GPU-Funktionen von Azure-VMs der N-Serie nutzen zu können, müssen Nvidia-GPU-Treiber installiert werden. 

Mit der Nvidia-GPU-Treibererweiterung werden die entsprechende Nvidia-CUDA- oder -GRID-Treiber installiert. Sie können die Erweiterung mithilfe von Azure Resource Manager-Vorlagen installieren und verwalten.

### <a name="supported-os-for-gpu-extension-for-windows"></a>Von der GPU-Erweiterung für Windows unterstützte Betriebssysteme

Diese Erweiterung unterstützt die folgenden Betriebssysteme. Andere Versionen funktionieren zwar möglicherweise ebenfalls, wurden jedoch nicht intern auf GPU-VMs getestet, die auf Azure Stack Edge-Geräten ausgeführt werden.

| Distribution | Version |
|---|---|
| Windows Server 2019 | Core |
| Windows Server 2016 | Core |

### <a name="supported-os-for-gpu-extension-for-linux"></a>Von der GPU-Erweiterung für Linux unterstützte Betriebssysteme

Diese Erweiterung unterstützt die folgenden Betriebssystem-Distributionen, abhängig von der Treiberunterstützung für bestimmte Betriebssystemversionen. Andere Versionen funktionieren zwar möglicherweise ebenfalls, wurden jedoch nicht intern auf GPU-VMs getestet, die auf Azure Stack Edge-Geräten ausgeführt werden.


| Distribution | Version |
|---|---|
| Ubuntu | 18.04 LTS |
| Red Hat Enterprise Linux | 7.4 |


## <a name="gpu-vms-and-kubernetes"></a>GPU-VMs und Kubernetes

Beachten Sie vor der Bereitstellung von GPU-VMs auf Ihrem Gerät die folgenden Überlegungen, wenn darauf Kubernetes konfiguriert ist.

#### <a name="for-1-gpu-device"></a>Für Geräte mit einer GPU: 

- **Sie erstellen zuerst eine GPU-VM auf Ihrem Gerät und konfigurieren danach Kubernetes:** In diesem Szenario werden sowohl die GPU-VM-Erstellung als auch die Kubernetes-Konfiguration erfolgreich abgeschlossen. Kubernetes hat in diesem Fall keinen Zugriff auf die GPU.

- **Sie konfigurieren zuerst Kubernetes auf Ihrem Gerät und erstellen danach eine GPU-VM:** In diesem Szenario beansprucht Kubernetes die GPU auf Ihrem Gerät, sodass die VM-Erstellung zu einem Fehler führt, da keine GPU-Ressourcen verfügbar sind.

#### <a name="for-2-gpu-device"></a>Für Geräte mit zwei GPUs:

- **Sie erstellen zuerst eine GPU-VM auf Ihrem Gerät und konfigurieren danach Kubernetes:** In diesem Szenario beansprucht die erstellte GPU-VM eine GPU auf Ihrem Gerät, und die Kubernetes-Konfiguration ist ebenfalls erfolgreich und beansprucht die verbleibende GPU. 

- **Sie erstellen zuerst zwei GPU-VMs auf Ihrem Gerät und konfigurieren danach Kubernetes:** In diesem Szenario beanspruchen die beiden GPU-VMs die beiden GPUs auf dem Gerät. Kubernetes wird zwar erfolgreich konfiguriert, kann aber keine GPUs nutzen. 

- **Sie konfigurieren zuerst Kubernetes auf Ihrem Gerät und erstellen danach eine GPU-VM:** In diesem Szenario beansprucht Kubernetes beide GPUs auf Ihrem Gerät, sodass die VM-Erstellung zu einem Fehler führt, da keine GPU-Ressourcen mehr verfügbar sind.

<!--Li indicated that this is fixed. If you have GPU VMs running on your device and Kubernetes is also configured, then anytime the VM is deallocated (when you stop or remove a VM using Stop-AzureRmVM or Remove-AzureRmVM), there is a risk that the Kubernetes cluster will claim all the GPUs available on the device. In such an instance, you will not be able to restart the GPU VMs deployed on your device or create GPU VMs. -->


## <a name="create-gpu-vms"></a>Erstellen von GPU-VMs

Führen Sie die folgenden Schritte aus, um auf Ihrem Gerät GPU-VMs bereitzustellen:

1. Ermitteln Sie, ob auf Ihrem Gerät auch Kubernetes ausgeführt wird. Wenn auf dem Gerät Kubernetes ausgeführt wird, müssen Sie zuerst die GPU-VM erstellen und erst danach Kubernetes konfigurieren. Wenn Kubernetes zuerst konfiguriert wird, werden alle verfügbaren GPU-Ressourcen beansprucht, und die Erstellung der GPU-VM führt zu einem Fehler.

1. [Laden Sie die VM-Vorlagen und Parameterdateien](https://aka.ms/ase-vm-templates) auf Ihrem Clientcomputer herunter. Entpacken Sie die Vorlagen und Dateien in einem Verzeichnis, das Sie als Arbeitsverzeichnis verwenden.

1. Bevor Sie virtuelle Computer auf Ihrem Azure Stack Edge-Gerät bereitstellen können, müssen Sie den Client so konfigurieren, dass er über Azure Resource Manager über Azure PowerShell eine Verbindung mit dem Gerät herstellt. Eine ausführliche Anleitung finden Sie unter [Herstellen einer Verbindung mit Azure Resource Manager auf Ihrem Azure Stack Edge-Gerät](azure-stack-edge-gpu-connect-resource-manager.md).

1. Führen Sie zum Erstellen von GPU-VMs alle Schritte unter [Bereitstellen von VMs auf Ihrem Azure Stack Edge-Gerät über Vorlagen](azure-stack-edge-gpu-deploy-virtual-machine-templates.md) oder [Bereitstellen von VMs auf Ihrem Azure Stack Edge-Gerät über das Azure-Portal](azure-stack-edge-gpu-deploy-virtual-machine-portal.md) mit Ausnahme der folgenden Unterschiede aus: 

            
    1. Wenn Sie eine VM mithilfe der Vorlagen erstellen, vergewissern Sie sich bei der Angabe der GPU-VM-Größen, dass Sie die NCasT4-v3-Serie in der Datei `CreateVM.parameters.json` verwenden, da diese für GPU-VMs unterstützt wird. Weitere Informationen finden Sie unter [Unterstützte VM-Größen für GPU-VMs](azure-stack-edge-gpu-virtual-machine-sizes.md#ncast4_v3-series-preview).

        ```json
            "vmSize": {
          "value": "Standard_NC4as_T4_v3"
        },
        ```
        Auch wenn Sie das Azure-Portal zum Erstellen Ihrer VM verwenden, können Sie eine VM-Größe aus der NCasT4-v3-Serie auswählen.

    1. Nachdem die GPU-VM erstellt wurde, können Sie sie in der Liste der VMs in Ihrer Azure Stack Edge-Ressource im Azure-Portal anzeigen.

        ![GPU-VM in der Liste der VMs im Azure-Portal](media/azure-stack-edge-gpu-deploy-gpu-virtual-machine/list-virtual-machine-1.png)

1. Wählen Sie die VM aus, und zeigen Sie die Detailinformationen an. Kopieren Sie die der VM zugewiesene IP-Adresse.

    ![Die der GPU-VM zugewiesene IP-Adresse im Azure-Portal](media/azure-stack-edge-gpu-deploy-gpu-virtual-machine/get-ip-gpu-virtual-machine-1.png)

1. Bei Bedarf können Sie das Computenetzwerk wieder auf die gewünschte Konfiguration umstellen. 


Nachdem die VM erstellt wurde, können Sie die GPU-Erweiterung mithilfe der Erweiterungsvorlage bereitstellen.


> [!NOTE]
> Wenn Sie die Softwareversion Ihres Geräts von 2012 auf eine höhere Version aktualisieren, müssen Sie die GPU-VMs manuell beenden.



## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie die [GPU-Erweiterung auf den GPU-VMs installieren](azure-stack-edge-gpu-deploy-virtual-machine-install-gpu-extension.md), die auf Ihrem Gerät ausgeführt werden.
