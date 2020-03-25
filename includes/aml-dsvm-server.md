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
ms.openlocfilehash: 2504245a14eee5308e046148f8073cf34ffa8143
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "79486077"
---
1. [Erstellen Sie einen Azure Machine Learning-Arbeitsbereich](../articles/machine-learning/how-to-manage-workspace.md).

1. Klonen Sie das [GitHub-Repository](https://aka.ms/aml-notebooks).

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Fügen Sie mit einer der folgenden Methoden dem geklonten Verzeichnis eine Datei für die Arbeitsbereichskonfiguration hinzu:

    * Wählen Sie im [Azure-Portal](https://ms.portal.azure.com) im Abschnitt **Übersicht** Ihres Arbeitsbereichs **„config.json“ herunterladen** aus. 

    ![Herunterladen von „config.json“](./media/aml-dsvm-server/download-config.png)

    * Erstellen Sie einen neuen Arbeitsbereich, indem Sie den Code im Notebook [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) in Ihrem geklonten Verzeichnis verwenden.

1. Starten Sie den Notebook-Server aus Ihrem geklonten Verzeichnis.

    ```bash
    jupyter notebook
    ```