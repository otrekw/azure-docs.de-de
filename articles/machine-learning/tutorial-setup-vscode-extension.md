---
title: 'Tutorial: Einrichten der Azure Machine Learning-Erweiterung für Visual Studio Code'
titleSuffix: Azure Machine Learning
description: Es wird beschrieben, wie Sie die Azure Machine Learning-Erweiterung für Visual Studio Code einrichten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: luisquintanilla
ms.author: luquinta
ms.date: 01/16/2019
ms.openlocfilehash: 4000fcc80d507d3b1e871d7f3288fc3b77693c76
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76157461"
---
# <a name="set-up-azure-machine-learning-visual-studio-code-extension"></a>Einrichten der Azure Machine Learning-Erweiterung für Visual Studio Code

In diesem Artikel wird beschrieben, wie Sie die Azure Machine Learning-Erweiterung für Visual Studio Code verwenden, um Skripts zu installieren und auszuführen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Installieren der Azure Machine Learning-Erweiterung für Visual Studio Code
> * Anmelden beim Azure-Konto über Visual Studio Code
> * Verwenden der Azure Machine Learning-Erweiterung zum Ausführen eines Beispielskripts

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement. Wenn Sie keins besitzen, können Sie sich für die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) registrieren.
- Installieren Sie [Visual Studio Code](https://code.visualstudio.com/docs/setup/setup-overview). Hierbei handelt es sich um einen einfachen plattformübergreifenden Code-Editor. 

## <a name="install-the-extension"></a>Installieren der Erweiterung

1. Öffnen Sie Visual Studio Code.
1. Wählen Sie in der **Aktivitätsleiste** das Symbol **Erweiterungen** aus, um die Ansicht „Erweiterungen“ zu öffnen.
1. Suchen Sie in der Ansicht „Erweiterungen“ nach „Azure Machine Learning“.
1. Wählen Sie **Installieren** aus.

> [!NOTE]
> Alternativ können Sie die Azure Machine Learning-Erweiterung auch über den Visual Studio Marketplace installieren, indem Sie das [Installationsprogramm direkt herunterladen](https://aka.ms/vscodetoolsforai). 

Die restlichen Schritte dieses Tutorials wurden mit **Version 0.6.8** der Erweiterung getestet.

## <a name="sign-in-to-your-azure-account"></a>Anmelden bei Ihrem Azure-Konto

Zum Bereitstellen von Ressourcen und Ausführen von Workloads in Azure müssen Sie sich mit Ihren Anmeldeinformationen für Ihr Azure-Konto anmelden. Zur Unterstützung der Kontoverwaltung wird die Azure-Kontoerweiterung von Azure Machine Learning automatisch installiert. Besuchen Sie die folgende Website, um [mehr zur Azure-Kontoerweiterung zu erfahren](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

1. Öffnen Sie hierzu die Befehlspalette, indem Sie in der Menüleiste **Ansicht > Befehlspalette** auswählen. 
1. Geben Sie in das Textfeld den Befehl „Azure: Anmelden“ ein, um den Anmeldeprozess zu starten.

## <a name="run-a-script-in-azure"></a>Ausführen eines Skripts in Azure

Nachdem Sie sich nun mit Ihren Anmeldeinformationen für das Konto bei Azure angemeldet haben, können Sie mit den Schritten in diesem Abschnitt darüber informieren, wie Sie die Erweiterung zum Trainieren eines Machine Learning-Modells verwenden.

1. Laden Sie das [Repository mit den VS Code Tools für KI](https://github.com/microsoft/vscode-tools-for-ai/archive/master.zip) an einen beliebigen Speicherort auf Ihrem Computer herunter, und entzippen Sie es.
1. Öffnen Sie das Verzeichnis `mnist-vscode-docs-sample` in Visual Studio Code.
1. Wählen Sie in der Aktivitätsleiste das **Azure**-Symbol aus.
1. Wählen Sie oben in der Azure Machine Learning-Ansicht das Symbol **Experiment ausführen** aus.

    > [!div class="mx-imgBorder"]
    > ![Experiment ausführen](./media/tutorial-setup-vscode-extension/run-experiment.PNG)

1. Befolgen Sie die Aufforderungen, wenn die Befehlspalette erweitert wird.

    1. Wählen Sie Ihr Azure-Abonnement.
    1. Wählen Sie die Option **Create a new Azure ML workspace** (Neuen Azure ML-Arbeitsbereich erstellen) aus.
    1. Wählen Sie den Auftragstyp **TensorFlow Single-Node Training** (TensorFlow: Training auf einem einzelnen Knoten) aus.
    1. Geben Sie `train.py` als Skript ein, das trainiert werden soll. Dies ist die Datei, die Code für ein Machine Learning-Modell enthält, mit dem die Bilder von handschriftlichen Ziffern kategorisiert werden.
    1. Geben Sie die folgenden Pakete als Anforderungen für die Ausführung an.

        ```text
        pip: azureml-defaults; conda: python=3.6.2, tensorflow=1.15.0
        ```

1. An diesem Punkt wird im Text-Editor eine Konfigurationsdatei angezeigt, die der unten angegebenen Datei ähnelt. Die Konfiguration enthält die Informationen, die zum Trainieren des Auftrags benötigt werden. Dies umfasst beispielsweise die Datei mit dem Code zum Trainieren des Modells sowie alle Python-Abhängigkeiten, die im vorherigen Schritt angegeben wurden.

    ```json
    {
        "workspace": "WS12191742",
        "resourceGroup": "WS12191742-rg2",
        "location": "South Central US",
        "experiment": "WS12191742-exp2",
        "compute": {
            "name": "WS12191742-com2",
            "vmSize": "Standard_D1_v2, Cores: 1; RAM: 3.5GB;"
        },
        "runConfiguration": {
            "filename": "WS12191742-com2-rc1",
            "condaDependencies": [
                "python=3.6.2",
                "tensorflow=1.15.0"
            ],
            "pipDependencies": [
                "azureml-defaults"
            ]
        }
    }
    ```

1. Wählen Sie **Submit experiment** (Experiment übermitteln) aus, um Ihr Experiment in Azure auszuführen. Hierbei werden die Datei `train.py` und die Konfigurationsdatei an Ihren Azure Machine Learning-Arbeitsbereich gesendet. Anschließend wird der Trainingsauftrag auf einer Computeressource in Azure gestartet.
1. Nach einigen Minuten wird lokal ein Verzeichnis mit dem Namen `output` erstellt, das ein trainiertes TensorFlow-Modell enthält.

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Trainieren und Bereitstellen eines TensorFlow-Modells für die Bildklassifizierung mit der Azure Machine Learning-Erweiterung für Visual Studio Code](tutorial-train-deploy-image-classification-model-vscode.md).
