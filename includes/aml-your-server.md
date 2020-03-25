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
ms.date: 03/05/2020
ms.openlocfilehash: ff449626ce528cfe0218a95330a567303c547e5f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "79485917"
---
1. Installieren Sie das Azure Machine Learning SDK für Python gemäß den Anweisungen unter [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

1. Erstellen Sie einen [Azure Machine Learning-Arbeitsbereich](../articles/machine-learning/how-to-manage-workspace.md).

1. Schreiben Sie eine [Konfigurationsdatei](../articles/machine-learning/how-to-configure-environment.md#workspace) (**aml_config/config.json**).

1. Klonen Sie das [GitHub-Repository](https://aka.ms/aml-notebooks).

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Starten Sie den Notebook-Server aus Ihrem geklonten Verzeichnis.

    ```bash
    jupyter notebook
    ```