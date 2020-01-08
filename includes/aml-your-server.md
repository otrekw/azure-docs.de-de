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
ms.date: 09/26/2019
ms.openlocfilehash: 1260ee32c267a2652ad02e74c3978654bf2f9669
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/28/2019
ms.locfileid: "75529784"
---
1. Installieren Sie das Azure Machine Learning SDK für Python gemäß den Anweisungen unter [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

1. Erstellen Sie einen [Azure Machine Learning-Arbeitsbereich](../articles/machine-learning/how-to-manage-workspace.md).

1. Schreiben Sie eine [Konfigurationsdatei](../articles/machine-learning/how-to-configure-environment.md#workspace) (**aml_config/config.json**).

1. Klonen Sie das [GitHub-Repository](https://aka.ms/aml-notebooks).

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Starten Sie den Notebook-Server aus Ihrem geklonten Verzeichnis.

    ```shell
    jupyter notebook
    ```
