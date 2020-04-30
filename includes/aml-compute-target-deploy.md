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
ms.date: 08/23/2019
ms.openlocfilehash: 9eedc0c3044717360494b222c88d73b3c8999e94
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82587846"
---
| Computeziel | Syntaxelemente | GPU-Unterstützung | FPGA-Unterstützung | BESCHREIBUNG |
| ----- | ----- | ----- | ----- | ----- |
| [Lokaler&nbsp;&nbsp;Webservice](../articles/machine-learning/how-to-deploy-and-where.md#local) | Testen/Debuggen | &nbsp; | &nbsp; | Für eingeschränkte Tests und Problembehandlung verwenden. Die Hardwarebeschleunigung hängt von der Verwendung von Bibliotheken im lokalen System ab.
| [Webdienst für Azure Machine Learning-Computeinstanzen&nbsp;&nbsp;](../articles/machine-learning/how-to-deploy-and-where.md#notebookvm) | Testen/Debuggen | &nbsp; | &nbsp; | Für eingeschränkte Tests und Problembehandlung verwenden.
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/how-to-deploy-and-where.md#aks) | Echtzeitrückschluss |  [Ja](../articles/machine-learning/how-to-deploy-inferencing-gpus.md) (Webdienstbereitstellung) | [Ja](../articles/machine-learning/how-to-deploy-fpga-web-service.md)   |Für hochgradig skalierbare Produktionsbereitstellungen verwenden. Bietet schnelle Antwortzeiten und die automatische Skalierung von bereitgestellten Diensten. Die automatische Skalierung von Clustern wird vom Azure Machine Learning SDK nicht unterstützt. Die Knoten in Ihrem AKS-Cluster können Sie über die entsprechende Benutzeroberfläche im Azure-Portal ändern. AKS ist die einzige für den Designer verfügbare Option. |
| [Azure Container Instances](../articles/machine-learning/how-to-deploy-and-where.md#aci) | Testen oder Entwicklung | &nbsp;  | &nbsp; | Für CPU-lastige Workloads im kleinen Maßstab verwenden, die weniger als 48 GB Arbeitsspeicher erfordern. |
| [Azure Machine Learning-Computecluster](../articles/machine-learning/how-to-use-parallel-run-step.md) | (Vorschau) &nbsp;Batchrückschluss | [Ja](../articles/machine-learning/how-to-use-parallel-run-step.md) (Machine Learning-Pipeline) | &nbsp;  | Ausführen von Batchbewertungen auf serverlosen Computezielen. Unterstützt virtuelle Computer mit normaler und niedriger Priorität. |
| [Azure-Funktionen](../articles/machine-learning/how-to-deploy-functions.md) | (Vorschau) Echtzeitrückschluss | &nbsp; | &nbsp; | &nbsp; |
| [Azure IoT Edge](../articles/machine-learning/how-to-deploy-and-where.md#iotedge) | (Vorschau) IoT-&nbsp;Modul |  &nbsp; | &nbsp; | Bereitstellen und Verarbeiten von ML-Modellen auf IoT-Geräten. |
| [Azure Data Box Edge](../articles/databox-online/azure-stack-edge-overview.md)   | Über IoT Edge |  &nbsp; | Ja | Bereitstellen und Verarbeiten von ML-Modellen auf IoT-Geräten. |

> [!NOTE]
> Zwar unterstützen Computeziele wie lokal, Azure Machine Learning-Computeinstanz und Azure Machine Learning-Computecluster die GPU für Training und Experimente, jedoch wird die Verwendung von GPUs für Rückschlüsse nur __bei Bereitstellung als Webdienst__ in Azure Kubernetes Service unterstützt.
>
> Das Verwenden einer GPU für Rückschlüsse __bei der Bewertung mit einer Machine Learning-Pipeline__ wird nur in Azure Machine Learning Compute unterstützt.