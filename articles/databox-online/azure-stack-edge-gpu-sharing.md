---
title: GPU-Freigabe auf Azure Stack Edge Pro-Geräten mit GPU
description: In diesem Artikel werden die Ansätze für die Freigabe von GPUs auf Azure Stack Edge Pro-Geräten mit GPU beschrieben.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/05/2021
ms.author: alkohli
ms.openlocfilehash: 6683e39cfa3601b1ae1fbbe02e69e4dc0a54e8e7
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103564632"
---
# <a name="gpu-sharing-on-your-azure-stack-edge-pro-gpu-device"></a>GPU-Freigabe auf Ihrem Azure Stack Edge Pro-Gerät mit GPU

Der Grafikprozessor (Graphics Processing Unit, GPU) ist ein spezieller Prozessor zum Beschleunigen der Grafikdarstellung. Da GPUs eine Vielzahl von Datenelementen gleichzeitig verarbeiten können, sind sie für maschinelles Lernen (ML), Videobearbeitung und Gaminganwendungen nützlich. Neben der CPU für allgemeine Computezwecke können Ihre Azure Stack Edge Pro-Geräte mit GPU ein oder zwei Nvidia Tesla T4-GPUs für rechenintensive Workloads wie das hardwarebeschleunigte Rückschließen umfassen. Weitere Informationen finden Sie auf der Seite zur [Nvidia Tesla T4-GPU](https://www.nvidia.com/data-center/tesla-t4/).


## <a name="about-gpu-sharing"></a>Informationen zur GPU-Freigabe

Für viele ML- und andere Computeworkloads wird keine dedizierte GPU benötigt. GPUs können freigegeben werden. Durch die gemeinsame Nutzung von GPUs durch Container- oder VM-Workloads lässt sich die GPU-Auslastung verbessern, ohne die Leistungsvorteile der GPU erheblich zu mindern.  

## <a name="using-gpu-with-vms"></a>Verwenden von GPUs mit VMs

Auf Ihrem Azure Stack Edge Pro-Gerät kann eine GPU nicht freigegeben werden, wenn VM-Workloads bereitgestellt werden. Eine GPU kann lediglich einer VM zugeordnet werden. Das bedeutet, dass auf einem Gerät mit einer GPU nur eine GPU-VM und auf einem Gerät mit zwei GPUs zwei GPU-VMs vorhanden sein können. Bei Verwendung von GPU-VMs auf einem Gerät, auf dem Kubernetes für Containerworkloads konfiguriert ist, müssen weitere Faktoren berücksichtigt werden. Weitere Informationen finden Sie unter [GPU-VMs und Kubernetes](azure-stack-edge-gpu-deploy-gpu-virtual-machine.md#gpu-vms-and-kubernetes).


## <a name="using-gpu-with-containers"></a>Verwenden von GPUs mit Containern

Wenn Sie containerisierte Workloads bereitstellen, gibt es mehrere Möglichkeiten, um eine GPU auf Hardware- und Softwareebene freizugeben. Mit der Tesla T4-GPU auf Ihrem Azure Stack Edge Pro-Gerät ist die Freigabe auf die Softwareebene beschränkt. Auf Ihrem Gerät werden die beiden folgenden Ansätze für die GPU-Softwarefreigabe verwendet: 

- Beim ersten Ansatz werden Umgebungsvariablen verwendet, um die Anzahl von GPUs anzugeben, die freigegeben werden können. Bei diesem Ansatz müssen folgende Aspekte berücksichtigt werden:

    - Bei dieser Methode können Sie eine oder beide GPUs bzw. keine GPU angeben. Eine Teilnutzung kann nicht festgelegt werden.
    - Es ist möglich, einer GPU mehrere Module zuzuordnen, ein Modul lässt sich jedoch nicht mehreren GPUs zuordnen.
    - Über die Nvidia SMI-Ausgabe lässt sich die Gesamtnutzung der GPU einschließlich Speicherauslastung ermitteln.
    
    Weitere Informationen finden Sie in den Schritten zum [Bereitstellen eines IoT Edge-Moduls unter Verwendung einer GPU](azure-stack-edge-gpu-configure-gpu-modules.md) auf Ihrem Gerät.

- Für den zweiten Ansatz müssen Sie den Multiprozessdienst (Multi-Process Service, MPS) für Ihre Nvidia-GPUs aktivieren. MPS ist ein Laufzeitdienst, der die gleichzeitige Ausführung mehrere Prozesse unter Verwendung von CUDA auf einer einzelnen geteilten GPU ermöglicht. Für eine maximale Auslastung ermöglicht MPS eine Überlappung von Kernel- und memcopy-Vorgängen unterschiedlicher Prozesse auf der GPU. Weitere Informationen finden Sie unter [Multi-Process Service](https://docs.nvidia.com/deploy/pdf/CUDA_Multi_Process_Service_Overview.pdf).

    Bei diesem Ansatz müssen folgende Aspekte berücksichtigt werden:
    
    - Bei MPS können Sie bei der GPU-Bereitstellung mehr Flags angeben.
    - Sie können eine Teilnutzung über MPS angeben und dadurch die Nutzung der einzelnen Anwendungen einschränken, die auf dem Gerät bereitgestellt sind. Sie können den GPU-Prozentsatz für die Verwendung durch die einzelnen Apps im Abschnitt `env` von `deployment.yaml` angeben, indem Sie den folgenden Parameter hinzufügen: 

    ```yml
    // Example: application wants to limit gpu percentage to 20%
    
        env:
              - name: CUDA_MPS_ACTIVE_THREAD_PERCENTAGE 
                value: "20"    
    ```

## <a name="gpu-utilization"></a>GPU-Auslastung
 
Wenn Sie GPU für Containerworkloads auf Ihrem Gerät freigeben, können Sie Nvidia System Management Interface (nvidia-smi) verwenden. Nvidia-smi ist ein Befehlszeilen-Hilfsprogramm zum Verwalten und Überwachen von Nvidia-GPU-Geräten. Weitere Informationen finden Sie unter [Nvidia System Management Interface](https://developer.nvidia.com/nvidia-system-management-interface).

Zur Anzeige der GPU-Nutzung stellen Sie zunächst eine Verbindung mit der PowerShell-Schnittstelle des Geräts her. Führen Sie den Befehl `Get-HcsNvidiaSmi` aus, und sehen Sie sich die Nvidia SMI-Ausgabe an. Sie können auch überprüfen, wie sich die GPU-Auslastung ändert. Aktivieren Sie dazu MPS, und stellen Sie dann mehrere Workloads auf dem Gerät bereit. Weitere Informationen finden Sie unter [Aktivieren des Multiprozessdiensts (MPS)](azure-stack-edge-gpu-connect-powershell-interface.md#enable-multi-process-service-mps).


## <a name="next-steps"></a>Nächste Schritte

- [GPU-Freigabe für Kubernetes-Bereitstellungen auf Ihrem Azure Stack Edge Pro-Gerät](azure-stack-edge-gpu-deploy-kubernetes-gpu-sharing.md).
- [GPU-Freigabe für IoT-Bereitstellungen auf Ihrem Azure Stack Edge Pro-Gerät](azure-stack-edge-gpu-deploy-iot-edge-gpu-sharing.md).
