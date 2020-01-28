---
title: 'Tutorial: Trainieren und Bereitstellen eines TensorFlow-Modells für die Bildklassifizierung mit der Azure Machine Learning-Erweiterung für Visual Studio Code'
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie mit TensorFlow und der Azure Machine Learning-Erweiterung für Visual Studio Code ein Modell für die Bildklassifizierung trainieren und bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: luisquintanilla
ms.author: luquinta
ms.date: 01/16/2019
ms.openlocfilehash: 899681f2bb9c3ef2a0368015a58db30a843738f5
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76157381"
---
# <a name="train-and-deploy-an-image-classification-tensorflow-model-using-the-azure-machine-learning-visual-studio-code-extension"></a>Trainieren und Bereitstellen eines TensorFlow-Modells für die Bildklassifizierung mit der Azure Machine Learning-Erweiterung für Visual Studio Code

Hier erfahren Sie, wie Sie mit TensorFlow und der Azure Machine Learning-Erweiterung für Visual Studio Code ein Bildklassifizierungsmodell für die Erkennung handgeschriebener Ziffern trainieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Grundlegendes zum Code
> * Erstellen eines Arbeitsbereichs
> * Erstellen eines Experiments
> * Konfigurieren von Computezielen
> * Ausführen einer Konfigurationsdatei
> * Trainieren eines Modells
> * Registrieren eines Modells
> * Bereitstellen eines Modells

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement. Wenn Sie keines besitzen, können Sie sich für die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) registrieren.
- Installieren Sie [Visual Studio Code](https://code.visualstudio.com/docs/setup/setup-overview). Hierbei handelt es sich um einen einfachen plattformübergreifenden Code-Editor.
- Azure Machine Learning Studio-Erweiterung für Visual Studio Code. Eine Installationsanleitung finden Sie im Tutorial [Einrichten der Azure Machine Learning-Erweiterung für Visual Studio Code](./tutorial-setup-vscode-extension.md).

## <a name="understand-the-code"></a>Grundlegendes zum Code

Der Code für dieses Tutorial verwendet TensorFlow zum Trainieren eines Machine Learning-Modells für die Bildklassifizierung, das handgeschriebene Ziffern von 0 bis 9 kategorisiert. Dazu wird ein neuronales Netzwerk erstellt, das die Pixelwerte eines Bilds mit 28 px × 28 px als Eingabe verwendet und eine Liste mit zehn Wahrscheinlichkeiten ausgibt, eine für jede der zu klassifizierenden Ziffern. Im Folgenden finden Sie ein Beispiel dafür, wie diese Daten aussehen:  

![MNIST-Ziffern](./media/tutorial-train-deploy-image-classification-model-vscode/digits.png)

Rufen Sie den Code für dieses Tutorial ab, indem Sie das [Repository mit den VS Code Tools für KI](https://github.com/microsoft/vscode-tools-for-ai/archive/master.zip) herunterladen und an einem beliebigen Speicherort auf Ihrem Computer entpacken.

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Als Erstes müssen Sie einen Arbeitsbereich erstellen, damit Sie eine Anwendung in Azure Machine Learning erstellen können. Ein Arbeitsbereich enthält die Ressourcen zum Trainieren von Modellen sowie die trainierten Modelle selbst. Weitere Informationen finden Sie unter [Was ist ein Arbeitsbereich?](./concept-workspace.md). 

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das **Azure**-Symbol aus, um die Azure Machine Learning-Ansicht zu öffnen.
1. Klicken Sie mit der rechten Maustaste auf Ihr Azure-Abonnement, und wählen Sie **Arbeitsbereich erstellen** aus. 
    
    > [!div class="mx-imgBorder"]
    > ![Erstellen eines Arbeitsbereichs](./media/tutorial-train-deploy-image-classification-model-vscode/create-workspace.png)

1. Standardmäßig wird ein Name mit dem Datum und der Uhrzeit der Erstellung generiert. Ändern Sie in der Befehlspalette den Namen in „TeamWorkspace“, und drücken Sie die **EINGABETASTE**.
1. Wählen Sie in der Befehlspalette die Option **Neue Ressourcengruppe erstellen** aus. 
1. Geben Sie im Textfeld der Befehlspalette „TeamWorkspace-rg“ ein, und drücken Sie die **EINGABETASTE**. 
1. Wählen Sie in der Befehlspalette einen Standort für Ihren Arbeitsbereich aus. Wählen Sie am besten einen Standort aus, der dem Standort, an dem Sie Ihr Modell bereitstellen möchten, am nächsten gelegen ist. Wählen Sie in diesem Fall **USA, Westen 2** aus.
1. Wenn Sie aufgefordert werden, eine Arbeitsbereichs-SKU auszuwählen, wählen Sie **Basic** aus, um einen Basic-Arbeitsbereich zu erstellen. Weitere Informationen zu verschiedenen Arbeitsbereichsangeboten finden Sie in der Übersicht über [Azure Machine Learning](./overview-what-is-azure-ml.md#sku).

Jetzt wird eine Anforderung an Azure zum Erstellen eines neuen Arbeitsbereichs in Ihrem Konto gesendet. Nach einigen Minuten wird der neue Arbeitsbereich in Ihrem Abonnementknoten angezeigt. 

## <a name="create-an-experiment"></a>Erstellen eines Experiments

In Ihrem Arbeitsbereich können Experimente erstellt werden, um einzelne Modelltrainingsausführungen zu verfolgen und zu analysieren. Ausführungen können in der Azure-Cloud oder auf Ihrem lokalen Computer erfolgen.

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das **Azure**-Symbol aus. Die Azure Machine Learning-Ansicht wird angezeigt.
1. Erweitern Sie Ihren Abonnementknoten.
1. Erweitern Sie den Knoten **TeamWorkspace**. 
1. Klicken Sie mit der rechten Maustaste auf den Knoten **Experimente**.
1. Wählen Sie im Kontextmenü **Create Experiment** (Experiment erstellen) aus.

    > [!div class="mx-imgBorder"]
    > ![Erstellen eines Experiments](./media/tutorial-train-deploy-image-classification-model-vscode/create-experiment.png)

1. Geben Sie in der Eingabeaufforderung der Befehlspalette Ihrem Experiment den Namen „MNIST“, und drücken Sie die **EINGABETASTE**, um das neue Experiment zu erstellen. 

Wie bei Arbeitsbereichen wird eine Anforderung an Azure gesendet, um ein Experiment mit den bereitgestellten Konfigurationen zu erstellen. Nach einigen Minuten wird das neue Experiment im Knoten *Experimente* Ihres Arbeitsbereichs angezeigt. 

## <a name="configure-compute-targets"></a>Konfigurieren von Computezielen

Ein Computeziel ist die Computeressource oder Umgebung, in der Sie Skripts ausführen und trainierte Modelle bereitstellen. Weitere Informationen finden Sie in der Dokumentation zu [Azure Machine Learning-Computezielen](./concept-compute-target.md).

Erstellen eines Computeziels:

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das **Azure**-Symbol aus. Die Azure Machine Learning-Ansicht wird angezeigt. 
1. Erweitern Sie Ihren Abonnementknoten. 
1. Erweitern Sie den Knoten **TeamWorkspace**. 
1. Klicken Sie unter dem Arbeitsbereichsknoten mit der rechten Maustaste auf den Knoten **Compute**, und wählen Sie **Create Compute** (Compute erstellen) aus. 

    > [!div class="mx-imgBorder"]
    > ![Erstellen eines Computeziels](./media/tutorial-train-deploy-image-classification-model-vscode/create-compute.png)

1. Wählen Sie **Azure Machine Learning Compute (AmlCompute)** aus. Azure Machine Learning Compute ist eine verwaltete Computeinfrastruktur, die Benutzern das einfache Erstellen von Computezielen mit einem oder mehreren Knoten ermöglicht, die Sie gemeinsam mit anderen Benutzern in Ihrem Arbeitsbereich verwenden können.
1. Wählen Sie eine VM-Größe aus. Wählen Sie in der Eingabeaufforderung der Befehlspalette **Standard_F2s_v2** aus. Die Größe Ihrer VM wirkt sich auf die Zeitspanne aus, die zum Trainieren der Modelle erforderlich ist. Weitere Informationen zu VM-Größen finden Sie unter [Größen für virtuelle Linux-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes).
1. Geben Sie dem Computeziel in der Eingabeaufforderung der Befehlspalette den Namen „TeamWkspc-com“, und drücken Sie die **EINGABETASTE**, um das Computeziel zu erstellen.

Nach einigen Minuten wird das neue Computeziel im Knoten *Compute* Ihres Arbeitsbereichs angezeigt.

## <a name="create-a-run-configuration"></a>Erstellen einer Laufzeitkonfiguration

Wenn Sie eine Trainingsausführung an ein Computeziel übermitteln, übermitteln Sie auch die Konfiguration, die zum Ausführen des Trainingsauftrags erforderlich ist. Dazu zählt beispielsweise das Skript, das den Trainingscode und die Python-Abhängigkeiten enthält, die zum Ausführen erforderlich sind.

So erstellen Sie eine Laufzeitkonfiguration:

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das **Azure**-Symbol aus. Die Azure Machine Learning-Ansicht wird angezeigt. 
1. Erweitern Sie Ihren Abonnementknoten. 
1. Erweitern Sie den Knoten **TeamWorkspace**. 
1. Klicken Sie unter dem Arbeitsbereichsknoten mit der rechten Maustaste auf den Computeknoten **TeamWkspc-com**, und wählen Sie **Laufzeitkonfiguration erstellen** aus.

    > [!div class="mx-imgBorder"]
    > ![Erstellen einer Laufzeitkonfiguration](./media/tutorial-train-deploy-image-classification-model-vscode/create-run-configuration.png)

1. Geben Sie der Laufzeitkonfiguration in der Eingabeaufforderung der Befehlspalette den Namen „MNIST-rc“, und drücken Sie die **EINGABETASTE**, um das Computeziel zu erstellen.
1. Wählen Sie anschließend den Auftragstyp **TensorFlow Single-Node Training** (TensorFlow: Training auf einem einzelnen Knoten) aus.
1. Drücken Sie die **EINGABETASTE**, um die Skriptdatei für die Ausführung auf dem Computeziel zu suchen. In diesem Fall ist die Datei `train.py` im Verzeichnis `vscode-tools-for-ai/mnist-vscode-docs-sample` das Skript zum Trainieren des Modells.
1. Geben Sie in der Eingabeaufforderung der Befehlspalette Folgendes ein, um die erforderlichen Pakete anzugeben:
    
    ```text
    pip: azureml-defaults; conda: python=3.6.2, tensorflow=1.15.0
    ```
    
    Eine Datei mit dem Namen `MNIST-rc.runconfig` wird in VS Code angezeigt, deren Inhalt dem folgenden ähnelt:

    ```json
    {
        "script": "train.py",
        "framework": "Python",
        "communicator": "None",
        "target": "TeamWkspc-com",
        "environment": {
            "python": {
                "userManagedDependencies": false,
                "condaDependencies": {
                    "dependencies": [
                        "python=3.6.2",
                        "tensorflow=1.15.0",
                        {
                            "pip": [
                                "azureml-defaults"
                            ]
                        }
                    ]
                }
            },
            "docker": {
                "baseImage": "mcr.microsoft.com/azureml/base:0.2.4",
                "enabled": true,
                "baseImageRegistry": {
                    "address": null,
                    "username": null,
                    "password": null
                }
            }
        },
        "nodeCount": 1,
        "history": {
            "outputCollection": true,
            "snapshotProject": false,
            "directoriesToWatch": [
                "logs"
            ]
        }
    }
    ```

1. Wenn Sie mit der Konfiguration zufrieden sind, öffnen Sie die Befehlspalette, indem Sie **Ansicht > Befehlspalette** auswählen.
1. Geben Sie in der Befehlspalette den folgenden Befehl ein, um die Konfigurationsdatei der Laufzeit zu speichern:

    ```text
    Azure ML: Save and Continue
    ```

Die Laufzeitkonfiguration `MNIST-rc` wird unter dem Computeknoten *TeamWkspc-com* hinzugefügt.

## <a name="train-the-model"></a>Trainieren des Modells

Während des Trainingsprozesses wird ein TensorFlow-Modell erstellt, indem die darin eingebetteten Trainingsdaten und Lernmuster für die einzelnen zu klassifizierenden Ziffern verarbeitet werden. 

So führen Sie ein Azure Machine Learning-Experiment aus

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das **Azure**-Symbol aus. Die Azure Machine Learning-Ansicht wird angezeigt. 
1. Erweitern Sie Ihren Abonnementknoten. 
1. Erweitern Sie den Knoten **TeamWorkspace > Experimente**. 
1. Klicken Sie mit der rechten Maustaste auf das Experiment **MNIST**.
1. Wählen Sie **Run Experiment** (Experiment ausführen) aus.

    > [!div class="mx-imgBorder"]
    > ![Ausführen eines Experiments](./media/tutorial-train-deploy-image-classification-model-vscode/run-experiment.png)

1. Wählen Sie in der Befehlspalette das Computeziel **TeamWkspc-com** aus.
1. Wählen Sie anschließend die Laufzeitkonfiguration **MNIST-rc** aus.
1. Jetzt wird eine Anforderung zum Ausführen Ihres Experiments auf dem ausgewählten Computeziel in Ihrem Arbeitsbereich an Azure gesendet. Dieser Vorgang dauert einige Minuten. Die Zeitspanne für die Ausführung des Trainingsauftrags ist von mehreren Faktoren abhängig, wie z. B. dem Computetyp und der Größe der Trainingsdaten. Wenn Sie den Status des Experiments nachverfolgen möchten, klicken Sie mit der rechten Maustaste auf den aktuellen Ausführungsknoten, und wählen Sie **View Run in Azure portal** (Ausführung im Azure-Portal anzeigen) aus.
1. Wenn das Dialogfeld mit der Anforderung zum Öffnen einer externen Website angezeigt wird, wählen Sie **Öffnen** aus.

    > [!div class="mx-imgBorder"]
    > ![Nachverfolgen des Experimentfortschritts](./media/tutorial-train-deploy-image-classification-model-vscode/track-experiment-progress.png)

Wenn das Modell mit dem Training fertig ist, wird die Statusbezeichnung neben dem Ausführungsknoten in „Abgeschlossen“ geändert.

## <a name="register-the-model"></a>Registrieren des Modells

Nachdem Sie Ihr Modell nun trainiert haben, können Sie es in Ihrem Arbeitsbereich registrieren. 

Registrieren des Modells:

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das **Azure**-Symbol aus. Die Azure Machine Learning-Ansicht wird angezeigt.
1. Erweitern Sie Ihren Abonnementknoten. 
1. Erweitern Sie den Knoten **TeamWorkspace > Experimente > MNIST**.
1. Rufen Sie die Modellausgaben ab, die beim Trainieren des Modells generiert werden. Klicken Sie mit der rechten Maustaste auf den Ausführungsknoten **Run 1** (Ausführung 1), und wählen Sie **Download outputs** (Ausgaben herunterladen) aus. 

    > [!div class="mx-imgBorder"]
    > ![Herunterladen von Modellausgaben](./media/tutorial-train-deploy-image-classification-model-vscode/download-outputs.png)

1. Wählen Sie das Verzeichnis aus, in dem die heruntergeladenen Ausgaben gespeichert werden sollen. Standardmäßig werden die Ausgaben in das Verzeichnis eingefügt, das momentan in Visual Studio Code geöffnet ist.
1. Klicken Sie mit der rechten Maustaste auf den Knoten **Modelle**, und wählen Sie **Modell registrieren** aus.

    > [!div class="mx-imgBorder"]
    > ![Registrieren eines Modells](./media/tutorial-train-deploy-image-classification-model-vscode/register-model.png)

1. Geben Sie Ihrem Modell in der Befehlspalette den Namen „MNIST-TensorFlow-model“, und drücken Sie die **EINGABETASTE**.
1. Ein TensorFlow-Modell besteht aus mehreren Dateien. Wählen Sie in der Befehlspalette **Model folder** (Modellordner) als Modellpfadformat aus. 
1. Wählen Sie das Verzeichnis `azureml_outputs/Run_1/outputs/Run_1/outputs/outputs/model` aus.

    Eine Datei, die Ihre Modellkonfigurationen enthält, wird mit etwa folgendem Inhalt in Visual Studio Code angezeigt:

    ```json
    {
        "modelName": "MNIST-TensorFlow-model",
        "tags": {
            "": ""
        },
        "modelPath": "c:\\Dev\\vscode-tools-for-ai\\mnist-vscode-docs-sample\\azureml_outputs\\Run_1\\outputs\\Run_1\\outputs\\outputs\\model",
        "description": ""
    }
    ```

1. Wenn Sie mit Ihrer Konfiguration zufrieden sind, speichern Sie sie, indem Sie die Befehlspalette öffnen und den folgenden Befehl eingeben:

    ```text
    Azure ML: Save and Continue
    ```

Nach einigen Minuten wird das Modell unter dem Knoten *Modelle* angezeigt.

## <a name="deploy-the-model"></a>Bereitstellen des Modells

In Visual Studio Code können Sie Ihr Modell als Webdienst für Folgendes bereitstellen:

+ Azure Container Instances (ACI)
+ Azure Kubernetes Service (AKS)

Sie müssen zum Testen nicht vorab einen ACI-Container erstellen, da ACI-Container nach Bedarf erstellt werden. AKS-Cluster müssen allerdings im Voraus konfiguriert werden. Weitere Informationen zu Bereitstellungsoptionen finden Sie unter [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md).

So stellen Sie einen Webdienst als ACI bereit:

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das **Azure**-Symbol aus. Die Azure Machine Learning-Ansicht wird angezeigt.
1. Erweitern Sie Ihren Abonnementknoten. 
1. Erweitern Sie den Knoten **TeamWorkspace > Modelle**. 
1. Klicken Sie mit der rechten Maustaste auf **MNIST-TensorFlow-model**, und wählen Sie **Deploy Service from Registered Model** (Dienst aus registriertem Modell bereitstellen) aus.

    > [!div class="mx-imgBorder"]
    > ![Bereitstellen des Modells](./media/tutorial-train-deploy-image-classification-model-vscode/register-model.png)

1. Wählen Sie in der Befehlspalette **Azure Container Instances** aus.
1. Geben Sie Ihrem Dienst den Namen „mnist-tensorflow-svc“, und drücken Sie in der Befehlspalette die **EINGABETASTE**.
1. Wählen Sie das Skript aus, das im Container ausgeführt werden soll, indem Sie in der Befehlspalette die **EINGABETASTE** drücken und im Verzeichnis `mnist-vscode-docs-sample` nach der Datei `score.py` suchen.
1. Geben Sie die Abhängigkeiten an, die zum Ausführen des Skripts erforderlich sind, indem Sie in der Befehlspalette die **EINGABETASTE** drücken und im Verzeichnis `mnist-vscode-docs-sample` nach der Datei `env.yml` suchen.

    Eine Datei, die Ihre Modellkonfigurationen enthält, wird mit etwa folgendem Inhalt in Visual Studio Code angezeigt:

    ```json
    {
        "name": "mnist-tensorflow-svc",
        "imageConfig": {
            "runtime": "python",
            "executionScript": "score.py",
            "dockerFile": null,
            "condaFile": "env.yml",
            "dependencies": [],
            "schemaFile": null,
            "enableGpu": false,
            "description": ""
        },
        "deploymentConfig": {
            "cpu_cores": 1,
            "memory_gb": 10,
            "tags": {
                "": ""
            },
            "description": ""
        },
        "deploymentType": "ACI",
        "modelIds": [
            "MNIST-TensorFlow-model:1"
        ]
    }
    ```
1. Wenn Sie mit Ihrer Konfiguration zufrieden sind, speichern Sie sie, indem Sie die Befehlspalette öffnen und den folgenden Befehl eingeben:

    ```text
    Azure ML: Save and Continue
    ```

Jetzt wird eine Anforderung zum Bereitstellen des Webdiensts an Azure gesendet. Dieser Vorgang dauert einige Minuten. Nach der Bereitstellung wird der neue Dienst unter dem Knoten *Endpunkte* angezeigt.

## <a name="next-steps"></a>Nächste Schritte

* Eine exemplarische Vorgehensweise zum Trainieren mit Azure Machine Learning außerhalb von Visual Studio Code finden Sie in [Tutorial: Trainieren von Modellen mit Azure Machine Learning](tutorial-train-models-with-aml.md).
* Eine exemplarische Vorgehensweise zum lokalen Bearbeiten, Ausführen und Debuggen von Code finden Sie im [Python-Hello World-Tutorial](https://code.visualstudio.com/docs/Python/Python-tutorial).

