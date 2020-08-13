---
title: include file
description: include file
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 06/25/2020
ms.openlocfilehash: bd3ac8d512c1b9d151c0dc549ffeee6a05c7f94b
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87542782"
---
Das Computeziel, das Sie zum Hosten Ihres Modells verwenden, wirkt sich auf die Kosten und Verfügbarkeit des bereitgestellten Endpunkts aus. Verwenden Sie die folgende Tabelle, um ein geeignetes Computeziel auszuwählen:

| Computeziel | Syntaxelemente | GPU-Unterstützung | FPGA-Unterstützung | BESCHREIBUNG |
| ----- | ----- | ----- | ----- | ----- |
| [Lokaler&nbsp;&nbsp;Webservice](../articles/machine-learning/how-to-deploy-local-container-notebook-vm.md) | Testen/Debuggen | &nbsp; | &nbsp; | Für eingeschränkte Tests und Problembehandlung verwenden. Die Hardwarebeschleunigung hängt von der Verwendung von Bibliotheken im lokalen System ab.
| [Webdienst für Azure Machine Learning-Computeinstanzen&nbsp;&nbsp;](../articles/machine-learning/how-to-deploy-local-container-notebook-vm.md) | Testen/Debuggen | &nbsp; | &nbsp; | Für eingeschränkte Tests und Problembehandlung verwenden.
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/how-to-deploy-azure-kubernetes-service.md) | Echtzeitrückschluss |  [Ja](../articles/machine-learning/how-to-deploy-inferencing-gpus.md) (Webdienstbereitstellung) | [Ja](../articles/machine-learning/how-to-deploy-fpga-web-service.md)   |Für hochgradig skalierbare Produktionsbereitstellungen verwenden. Bietet schnelle Antwortzeiten und die automatische Skalierung von bereitgestellten Diensten. Die automatische Skalierung von Clustern wird vom Azure Machine Learning SDK nicht unterstützt. Die Knoten in Ihrem AKS-Cluster können Sie über die entsprechende Benutzeroberfläche im Azure-Portal ändern. AKS ist die einzige für den Designer verfügbare Option. |
| [Azure Container Instances](../articles/machine-learning/how-to-deploy-azure-container-instance.md) | Testen oder Entwicklung | &nbsp;  | &nbsp; | Für CPU-lastige Workloads im kleinen Maßstab verwenden, die weniger als 48 GB Arbeitsspeicher erfordern. |
| [Azure Machine Learning-Computecluster](../articles/machine-learning/how-to-use-parallel-run-step.md) | Batchrückschluss&nbsp; | [Ja](../articles/machine-learning/how-to-use-parallel-run-step.md) (Machine Learning-Pipeline) | &nbsp;  | Ausführen von Batchbewertungen auf serverlosen Computezielen. Unterstützt virtuelle Computer mit normaler und niedriger Priorität. |
| [Azure-Funktionen](../articles/machine-learning/how-to-deploy-functions.md) | (Vorschau) Echtzeitrückschluss | &nbsp; | &nbsp; | &nbsp; |
| [Azure IoT Edge](../articles/machine-learning/how-to-deploy-and-where.md#iotedge) | (Vorschau) IoT-&nbsp;Modul |  &nbsp; | &nbsp; | Bereitstellen und Verarbeiten von ML-Modellen auf IoT-Geräten. |
| [Azure Data Box Edge](../articles/databox-online/azure-stack-edge-overview.md)   | Über IoT Edge |  &nbsp; | Ja | Bereitstellen und Verarbeiten von ML-Modellen auf IoT-Geräten. |

> [!NOTE]
> Zwar unterstützen Computeziele wie lokal, Azure Machine Learning-Computeinstanz und Azure Machine Learning-Computecluster die GPU für Training und Experimente, jedoch wird die Verwendung von GPUs für Rückschlüsse nur __bei Bereitstellung als Webdienst__ in Azure Kubernetes Service unterstützt.
>
> Das Verwenden einer GPU für Rückschlüsse __bei der Bewertung mit einer Machine Learning-Pipeline__ wird nur in Azure Machine Learning Compute unterstützt.

> [!NOTE]
> * Azure Container Instances (ACI) eignet sich nur für kleine Modelle mit einer Größe von unter 1 GB. 
> * Es wird empfohlen, AKS-Cluster (Azure Kubernetes Service) mit einem Knoten für Dev-Tests größerer Modelle zu verwenden.