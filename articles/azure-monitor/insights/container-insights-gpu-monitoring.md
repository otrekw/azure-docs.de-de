---
title: Konfigurieren der GPU-Überwachung mit Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Überwachung von Kubernetes-Clustern mit NVIDIA- und AMD-GPU-fähigen Knoten mit Azure Monitor für Container konfigurieren können.
ms.topic: conceptual
ms.date: 03/27/2020
ms.openlocfilehash: 958f5ab33edcd280f5673391eba907728f1153c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80373273"
---
# <a name="configure-gpu-monitoring-with-azure-monitor-for-containers"></a>Konfigurieren der GPU-Überwachung mit Azure Monitor für Container

Ab der Agent-Version *ciprod03022019* unterstützt der integrierte Agent von Azure Monitor für Container das Überwachen der GPU-Nutzung (Graphical Processing Unit) auf GPU-fähigen Kubernetes-Clusterknoten sowie das Überwachen von Pods/Containern, die GPU-Ressourcen anfordern und verwenden.

## <a name="supported-gpu-vendors"></a>Unterstützte GPU-Anbieter

Azure Monitor für Container unterstützt die Überwachung von GPU-Clustern von folgenden GPU-Anbietern:

- [NVIDIA](https://developer.nvidia.com/kubernetes-gpu)

- [AMD](https://github.com/RadeonOpenCompute/k8s-device-plugin)

Azure Monitor für Container beginnt automatisch mit der Überwachung der GPU-Nutzung auf Knoten sowie GPUs anfordernden Pods und Workloads, indem die folgenden Metriken in 60-Sekunden-Intervallen gesammelt und in der **insightMetrics**-Tabelle gespeichert werden:

|Metrikname |Metrikdimension (Tags) |BESCHREIBUNG |
|------------|------------------------|------------|
|containerGpuDutyCycle |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor|Der Prozentsatz der Zeit im Verlauf des letzten Beispielzeitraums (60 Sekunden), während dessen die GPU ausgelastet war/aktiv die Verarbeitung für einen Container ausgeführt hat. Der Arbeitszyklus ist eine Zahl zwischen 1 und 100. |
|containerGpuLimits |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName |In jedem Container können Grenzwerte als eine oder mehrere GPUs angegeben werden. Es ist nicht möglich, einen Bruchteil einer GPU anzufordern oder einzuschränken. |
|containerGpuRequests |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName |Jeder Container kann einen oder mehrere GPUs anfordern. Es ist nicht möglich, einen Bruchteil einer GPU anzufordern oder einzuschränken.|
|containerGpumemoryTotalBytes |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor |Menge an GPU-Arbeitsspeicher in Bytes, die für einen bestimmten Container verwendet werden kann. |
|containerGpumemoryUsedBytes |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor |Menge an GPU-Arbeitsspeicher in Bytes, die für einen bestimmten Container verwendet wird. |
|nodeGpuAllocatable |container.azm.ms/clusterId, container.azm.ms/clusterName, gpuVendor |Anzahl von GPUs in einem Knoten, die von Kubernetes verwendet werden können. |
|nodeGpuCapacity |container.azm.ms/clusterId, container.azm.ms/clusterName, gpuVendor |Gesamtanzahl der GPUs in einem Knoten. |

## <a name="gpu-performance-charts"></a>GPU-Leistungsdiagramme 

Azure Monitor für Container enthält vorkonfigurierte Diagramme für die Metriken, die weiter oben in der Tabelle als GPU-Arbeitsmappe für jeden Cluster aufgeführt sind. Sie finden die GPU-Arbeitsmappe **Node GPU** direkt von einem AKS-Cluster aus, indem Sie **Arbeitsmappen** im linken Bereich und in der Dropdownliste in Insight **Arbeitsmappen anzeigen** auswählen.

## <a name="next-steps"></a>Nächste Schritte

- In [Verwenden von GPUs für computeintensive Workloads in Azure Kubernetes Service (AKS)](../../aks/gpu-cluster.md) erfahren Sie, wie Sie einen AKS-Cluster bereitstellen, der GPU-fähige Knoten enthält.

- Erfahren Sie mehr über [Für GPU optimierte VM-Größen](../../virtual-machines/sizes-gpu.md).

- In [Schedule GPUs](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/) (Planen von GPUs)erfahren Sie mehr darüber, wie Kubernetes Experimente zur Verwaltung von GPUs auf einem oder mehreren Knoten in einem Cluster unterstützt.