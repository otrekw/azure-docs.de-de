---
title: Zusammengestellte Umgebungen
titleSuffix: Azure Machine Learning
description: Erfahren Sie mehr über die für Azure Machine Learning zusammengestellten Umgebungen, eine Reihe von vorkonfigurierten Umgebungen, die dabei helfen, die Vorbereitungszeiten für Experimente und Bereitstellung zu verkürzen.
services: machine-learning
author: luisquintanilla
ms.author: luquinta
ms.reviewer: luquinta
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.date: 4/2/2021
ms.openlocfilehash: 1bea195d6f08832415a16531212d176ada7402e6
ms.sourcegitcommit: 6323442dbe8effb3cbfc76ffdd6db417eab0cef7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110612573"
---
# <a name="azure-machine-learning-curated-environments"></a>Azure Machine Learning – zusammengestellte Umgebungen

In diesem Artikel werden die in Azure Machine Learning verfügbaren zusammengestellten Umgebungen aufgelistet. Zusammengestellte Umgebungen werden von Azure Machine Learning bereitgestellt und sind standardmäßig in Ihrem Arbeitsbereich verfügbar. Sie werden durch zwischengespeicherte Docker-Images unterstützt, in denen die neueste Version des Azure Machine Learning SDK verwendet wird. Damit werden die Kosten für die Vorbereitung der Ausführung reduziert und kürzere Bereitstellungszeiten ermöglicht. Verwenden Sie diese Umgebungen, um schnell mit verschiedenen Machine Learning-Frameworks zu beginnen.

> [!NOTE]
> Diese Liste wurde im April 2021 aktualisiert. Mit dem Python SDK oder der CLI können Sie die aktuellste Liste der Umgebungen mit ihren Abhängigkeiten abrufen. Weitere Informationen finden Sie im [Artikel zu Umgebungen](./how-to-use-environments.md#use-a-curated-environment). Nach dem Release dieses neuen Satzes werden vorherige zusammengestellte Umgebungen ausgeblendet, können aber weiterhin verwendet werden. 

## <a name="pytorch"></a>PyTorch
- AzureML-pytorch-1.7-ubuntu18.04-py37-cuda11-gpu
     - Eine Umgebung für Deep Learning mit PyTorch, die das Azure ML SDK und zusätzliche Python-Pakete enthält.
     - PyTorch-Version: 1.7
     - Python-Version: 3.7
     - Basisimage: mcr.microsoft.com/azureml/openmpi4.1.0-cuda11.0.3-cudnn8-ubuntu18.04
     - CUDA-Version: 11.0.3
     - OpenMPI-Version: 4.1.0
     - Ubuntu-Version: 18.04

## <a name="sklearn"></a>Sklearn
- AzureML-sklearn-0.24-ubuntu18.04-py37-cuda11-gpu
     - Eine Umgebung für Aufgaben wie Regression, Clustering und Klassifizierung mit Scikit-learn. Enthält das Azure ML SDK und zusätzliche Python-Pakete.
     - Scikit-learn-Version: 24.1
     - Python-Version: 3.7
     - Basisimage: mcr.microsoft.com/azureml/openmpi4.1.0-cuda11.0.3-cudnn8-ubuntu18.04
     - CUDA-Version: 11.0.3
     - OpenMPI-Version: 4.1.0
     - Ubuntu-Version: 18.04

## <a name="tensorflow"></a>TensorFlow
- AzureML-tensorflow-2.4-ubuntu18.04-py37-cuda11-gpu
     - Eine Umgebung für Deep Learning mit Tensorflow, die das Azure ML SDK und zusätzliche Python-Pakete enthält.
     - Tensorflow-Version: 2.4
     - Python-Version: 3.7
     - Basisimage: mcr.microsoft.com/azureml/openmpi4.1.0-cuda11.0.3-cudnn8-ubuntu18.04
     - CUDA-Version: 11.0.3
     - OpenMPI-Version: 4.1.0
     - Ubuntu-Version: 18.04

## <a name="inference-only-curated-environments-and-prebuilt-docker-images"></a>Rückschluss nur für zusammengestellte Umgebungen und vordefinierte Docker-Images
- Weitere Informationen zum Rückschluss von ausschließlich zusammengestellten Umgebungen und zum MCR-Pfad für vordefinierte Docker-Images finden Sie unter [Vordefinierte Docker-Images für Rückschlüsse](concept-prebuilt-docker-images-inference.md#list-of-prebuilt-docker-images-for-inference).
