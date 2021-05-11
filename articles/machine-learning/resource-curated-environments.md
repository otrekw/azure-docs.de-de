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
ms.openlocfilehash: 16d5cbc1d48b4559ac34314582b9c01b7c6bb58c
ms.sourcegitcommit: 62e800ec1306c45e2d8310c40da5873f7945c657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108166647"
---
# <a name="azure-machine-learning-curated-environments"></a>Azure Machine Learning – zusammengestellte Umgebungen

In diesem Artikel werden die in Azure Machine Learning verfügbaren zusammengestellten Umgebungen aufgelistet. Zusammengestellte Umgebungen werden von Azure Machine Learning bereitgestellt und sind standardmäßig in Ihrem Arbeitsbereich verfügbar. Sie werden durch zwischengespeicherte Docker-Images unterstützt, in denen die neueste Version des Azure Machine Learning SDK verwendet wird. Damit werden die Kosten für die Vorbereitung der Ausführung reduziert und kürzere Bereitstellungszeiten ermöglicht. Verwenden Sie diese Umgebungen, um schnell mit verschiedenen Machine Learning-Frameworks zu beginnen.

> [!NOTE]
> Diese Liste wurde im April 2021 aktualisiert. Mit dem Python SDK oder der CLI können Sie die aktuellste Liste der Umgebungen mit ihren Abhängigkeiten abrufen. Weitere Informationen finden Sie im [Artikel zu Umgebungen](./how-to-use-environments.md#use-a-curated-environment). Nach dem Release dieses neuen Satzes werden vorherige zusammengestellte Umgebungen ausgeblendet, können aber weiterhin verwendet werden. 

## <a name="pytorch"></a>PyTorch
- AzureML-Pytorch1.7-Cuda11-OpenMpi4.1.0-py36
     - Eine Umgebung für Deep Learning mit PyTorch, die das Azure ML SDK und zusätzliche Python-Pakete enthält.
     - PyTorch-Version: 1.7
     - Python-Version: 3.6.9
     - Basisimage: mcr.microsoft.com/azureml/openmpi4.1.0-cuda11.0.3-cudnn8-ubuntu18.04
     - CUDA-Version: 11.0.3
     - OpenMPI: 4.1.0

## <a name="scikit"></a>Scikit
- AzureML-Scikit-learn0.20.4-Cuda11-OpenMpi4.1.0-py36
     - Eine Umgebung für Aufgaben wie Regression, Clustering und Klassifizierung mit Scikit-learn. Enthält das Azure ML SDK und zusätzliche Python-Pakete.
     - Scikit-learn-Version: 20.4
     - Python-Version: 3.6.9
     - Basisimage: mcr.microsoft.com/azureml/openmpi4.1.0-cuda11.0.3-cudnn8-ubuntu18.04
     - CUDA-Version: 11.0.3
     - OpenMPI: 4.1.0

## <a name="tensorflow"></a>TensorFlow
- AzureML-TensorFlow2.4-Cuda11-OpenMpi4.1.0-py36
     - Eine Umgebung für Deep Learning mit Tensorflow, die das Azure ML SDK und zusätzliche Python-Pakete enthält.
     - Tensorflow-Version: 2.4
     - Python-Version: 3.6.9
     - Basisimage: mcr.microsoft.com/azureml/openmpi4.1.0-cuda11.0.3-cudnn8-ubuntu18.04
     - CUDA-Version: 11.0.3
     - OpenMPI: 4.1.0
