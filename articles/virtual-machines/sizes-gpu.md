---
title: 'Azure-VM-Größen: GPU | Microsoft-Dokumentation'
description: Liste der verschiedenen verfügbaren GPU-optimierten Größen für virtuelle Computer in Azure Dieser Artikel listet Informationen zur Anzahl von vCPUs, Datenträgern und Netzwerkschnittstellenkarten sowie zum Speicherdurchsatz und zur Netzwerkbandbreite für Größen dieser Serie auf.
services: virtual-machines
documentationcenter: ''
author: vikancha
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jonbeck
ms.openlocfilehash: 5d36ba05d2138a06ebb2ef4e49aadb6032b62b92
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82627040"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>Für GPU optimierte VM-Größen

GPU-optimierte VM-Größen sind für spezialisierte virtuelle Computer mit einzelnen, mehreren oder Teil-GPUs verfügbar. Diese Größen sind für rechenintensive, grafikintensive und visualisierungsorientierte Workloads vorgesehen. Dieser Artikel enthält Informationen über die Anzahlen und Typen von GPUs, vCPUs, Datenträgern und NICs. Der Speicherdurchsatz und die Netzwerkbandbreite sind für die jeweiligen Größen in dieser Gruppe ebenfalls enthalten.

- Die Größen der [NC-Serie](nc-series.md), der [NCv2-Serie](ncv2-series.md) und der [NCv3-Serie](ncv3-series.md) sind für rechen- und netzwerkintensive Anwendungen und Algorithmen optimiert. Einige Beispiele sind CUDA- und OpenCL-basierte Anwendungen und Simulationen, KI und Deep Learning. Die NCv3-Serie ist für High Performance Computing-Workloads, die mit der Tesla V100-GPU von NVIDIA ausgestattet sind, konzipiert. Virtuelle Computer der NC-Serie verwenden den Prozessor Intel Xeon E5-2690 v3 mit 2,60 GHz (Haswell) und virtuelle Computer der NCv2- und der NCv3-Serie den Prozessor Intel Xeon E5-2690 v4 (Broadwell).

- Die Größen der [ND-Serie](nd-series.md) und der [NDv2-Serie](ndv2-series.md) sind speziell auf Trainings- und Rückschlussszenarien für Deep Learning ausgelegt. Sie nutzen die GPU NVIDIA Tesla P40 und den Prozessor Intel Xeon E5-2690 v4 (Broadwell). Für die NDv2-Serie wird der Intel Xeon Platinum 8168-Prozessor (Skylake) verwendet.

- Die Größen der [NV-Serie](nv-series.md) und der [NVv3-Serie](nvv3-series.md) sind für Remotevisualisierung, Streaming, Spiele, Codierung und VDI-Szenarien mit Frameworks wie OpenGL und DirectX konzipiert und optimiert. Diese VMs werden von der NVIDIA Tesla M60-GPU unterstützt.

- VM-Größen der [NVv4-Serie](nvv4-series.md) sind für VDI und Remotevisualisierung konzipiert und optimiert. Dank partitionierter GPUs bietet NVv4 die passende Größe für Workloads, die geringere GPU-Ressourcen erfordern. Diese virtuellen Computer basieren auf der AMD Radeon Instinct MI25-GPU. VMs der NVv4-Serie unterstützen derzeit nur das Windows-Gastbetriebssystem.

## <a name="supported-operating-systems-and-drivers"></a>Unterstützte Betriebssysteme und Treiber

Um die GPU-Funktionen von Azure-VMs der N-Serie nutzen zu können, müssen NVIDIA- oder AMD-GPU-Treiber installiert sein.

- Bei VMs mit NVIDIA-GPUs werden mit der [NVIDIA-GPU-Treibererweiterung](/azure/virtual-machines/extensions/hpccompute-gpu-windows) die entsprechenden NVIDIA-CUDA- oder -GRID-Treiber installiert. Installieren oder verwalten Sie die Erweiterung mithilfe des Azure-Portals oder mit Tools wie Azure PowerShell oder Azure Resource Manager-Vorlagen. Informationen zu unterstützten Betriebssystemen und Bereitstellungsschritten finden Sie in der [Dokumentation zur NVIDIA-GPU-Treibererweiterung](/azure/virtual-machines/extensions/hpccompute-gpu-windows). Allgemeine Informationen zu VM-Erweiterungen finden Sie unter [Erweiterungen und Features für virtuelle Azure-Computer](/azure/virtual-machines/extensions/overview).

   Alternativ dazu können Sie NVIDIA-GPU-Treiber manuell installieren. Informationen zu unterstützten Betriebssystemen, Treibern sowie Installations- und Überprüfungsschritten finden Sie unter [Installieren von NVIDIA GPU-Treibern für virtuelle Computer der Serie N mit Windows](/azure/virtual-machines/windows/n-series-driver-setup) bzw. [Installieren von NVIDIA GPU-Treibern für virtuelle Computer der Serie N mit Linux](/azure/virtual-machines/linux/n-series-driver-setup).

- Informationen zu unterstützten Betriebssystemen, Treibern sowie Installations- und Überprüfungsschritten zu VMs mit AMD-GPUs finden Sie unter [Installieren von AMD-GPU-Treibern für virtuelle Computer der N-Serie unter Windows](/azure/virtual-machines/windows/n-series-amd-driver-setup).

## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

- Informationen zur Verfügbarkeit von virtuellen Computern der N-Serie finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).

- Virtuelle Computer der N-Serie können nur im Resourcen Manager-Bereitstellungsmodell bereitgestellt werden.

- Virtuelle Computer der Serie N unterscheiden hinsichtlich des Typs von Azure Storage, den sie für ihre Datenträger unterstützen. Virtuelle NC- oder NV-Computer unterstützen nur VM-Datenträger, die durch Standard Disk Storage (HDD) gesichert sind. Virtuelle NCv2-, NCv3-, ND-, NDv2- und NVv2-Computer unterstützen nur VM-Datenträger, die durch Premium Disk Storage (SSD) gesichert sind.

- Wenn Sie eine größere Anzahl von virtuellen Computern der N-Serie bereitstellen möchten, sollten Sie ein nutzungsbasiertes Abonnement oder andere Kaufoptionen in Erwägung ziehen. Bei Verwendung eines [kostenlosen Azure-Kontos](https://azure.microsoft.com/free/)können Sie nur eine begrenzte Anzahl von Azure-Compute-Kernen nutzen.

- Möglicherweise müssen Sie das Kernnutzungskontingent (pro Region) in Ihrem Azure-Abonnement sowie das separate Kontingent für NC-, NCv2-, NCv3-, ND-, NDv2-, NV- oder NVv2-Kerne erhöhen. In diesem Fall können Sie kostenlos [eine Anfrage an den Onlinekundensupport richten](../azure-portal/supportability/how-to-create-azure-support-request.md) und eine Erhöhung des Kontingents anfordern. Standardgrenzwerte variieren unter Umständen je nach Abonnementkategorie.

## <a name="other-sizes"></a>Andere Größen

- [Allgemeiner Zweck](sizes-general.md)
- [Computeoptimiert](sizes-compute.md)
- [High Performance Computing](sizes-hpc.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.
