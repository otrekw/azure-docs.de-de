---
title: Erste Schritte für die erweiterte lokale Entwicklung mit Azure Percept
description: Erste Schritte für die lokale Machine Learning-Entwicklung mit VS Code und Jupyter Notebook-Instanzen in Azure Machine Learning
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: tutorial
ms.date: 02/10/2021
ms.custom: template-tutorial
ms.openlocfilehash: 88e71ac21177a13d30176212e97442c63272eca6
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101658807"
---
# <a name="getting-started-with-machine-learning-development-using-vs-code--jupyter-notebooks-on-azureml"></a>Erste Schritte für die Machine Learning-Entwicklung mit VS Code und Jupyter Notebook-Instanzen in Azure Machine Learning

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie einen Azure Machine Learning-Arbeitsbereich einrichten, eine Compute-Instanz erstellen, eine Visual Studio Code-Entwicklungsumgebung auf Ihrem lokalen Computer einrichten und die Zellen eines vorkonfigurierten Jupyter Notebook-Beispiels innerhalb von VS Code ausführen.

Das [Notebook](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) führt einen Lerntransfer unter Verwendung eines vorab trainierten TensorFlow-Modells (MobileNetSSDV2Lite) für Azure Machine Learning in Python mit einem benutzerdefinierten Dataset durch, um Schalen zu erkennen.

Das Notebook veranschaulicht, wie Sie das [COCO-Dataset](https://cocodataset.org/#home) nach der relevanten Klasse (Schalen) filtern und anschließend in das passende Format konvertieren. Alternativ können Sie auch das Open-Source-Beschriftungstool [VoTT 2](https://github.com/microsoft/VoTT) verwenden, um Begrenzungsrahmen im PASCAL VOC-Format zu erstellen und zu beschriften.

Nachdem Sie das Modell mit dem benutzerdefinierten Dataset erneut trainiert haben, kann das Modell mithilfe der Modulzwilling-Aktualisierungsmethode für Ihr Azure Percept DK-Gerät bereitgestellt werden. Anschließend können Sie den Modellrückschluss überprüfen, indem Sie sich den RTSP-Livestream aus dem Azure Percept-Vision-SOM ansehen. Sowohl das erneute Trainieren als auch die Bereitstellung wird innerhalb des Notebooks über Ihre Remote-Compute-Instanz durchgeführt.

## <a name="prerequisites"></a>Voraussetzungen

- [Azure Machine Learning-Abonnement](https://azure.microsoft.com/free/services/machine-learning/)
- [Azure Percept DK mit verbundenem Azure Percept-Vision-SOM](./overview-azure-percept-dk.md)
- Abgeschlossenes [Azure Percept DK-Onboarding](./quickstart-percept-dk-set-up.md)

## <a name="download-azure-percept-github-repository"></a>Herunterladen des GitHub-Repositorys für Azure Percept

1. Navigieren Sie zum [GitHub-Repository für Azure Percept DK](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview).
1. Klonen Sie das Repository, oder laden Sie die ZIP-Datei herunter. Klicken Sie im oberen Bereich des Bildschirms auf **Code** -> **ZIP herunterladen**.

    :::image type="content" source="./media/advanced-development-local/download-zip.png" alt-text="GitHub-Downloadbildschirm":::

## <a name="create-an-azure-machine-learning-workspace-and-remote-compute-instance"></a>Erstellen eines Azure Machine Learning-Arbeitsbereichs und einer Remote-Compute-Instanz

1. Navigieren Sie zum [Azure Machine Learning-Portal](https://ml.azure.com).
1. Wählen Sie in den Dropdownmenüs Ihr Verzeichnis, Ihr Azure-Abonnement und Ihren Machine Learning-Arbeitsbereich aus, und klicken Sie auf **Erste Schritte**.

    :::image type="content" source="./media/advanced-development-local/machine-learning-portal-get-started.png" alt-text="Azure Machine Learning-Bildschirm für die ersten Schritte":::

    Sollten Sie noch nicht über einen Azure Machine Learning-Arbeitsbereich verfügen, klicken Sie auf **Neuen Arbeitsbereich erstellen**. Gehen Sie auf dem neuen Browsertab wie folgt vor:

    1. Wählen Sie Ihr Azure-Abonnement aus.
    1. Wählen Sie Ihre Ressourcengruppe aus.
    1. Geben Sie einen Namen für den Arbeitsbereich ein.
    1. Wählen Sie Ihre Region aus.
    1. Wählen Sie Ihre Arbeitsbereichs-Edition aus.
    1. Klicken Sie auf **Überprüfen und erstellen**.
    1. Überprüfen Sie auf der nächsten Seite Ihre Auswahl, und klicken Sie auf **Erstellen**.

        :::image type="content" source="./media/advanced-development-local/workspace-review-and-create.png" alt-text="Bildschirm für die Arbeitsbereichserstellung in Azure Machine Learning":::

    Die Erstellung des Arbeitsbereichs kann einige Minuten dauern. Nach Abschluss der Arbeitsbereichserstellung werden Ihre Ressourcen mit grünen Häkchen versehen, und im oberen Bereich der Übersichtsseite für Machine Learning Services wird **Ihre Bereitstellung wurde abgeschlossen.** angezeigt.

    :::image type="content" source="./media/advanced-development-local/workspace-creation-complete-inline.png" alt-text="Bestätigung der Arbeitsbereichserstellung" lightbox= "./media/advanced-development-local/workspace-creation-complete.png":::

    Kehren Sie nach Abschluss der Arbeitsbereichserstellung zum Tab mit dem Machine Learning-Portal zurück, und klicken Sie auf **Erste Schritte**.

1. Klicken Sie auf der Homepage des Machine Learning-Arbeitsbereichs im linken Bereich auf **Compute**.

1. Sollte noch keine Compute-Instanz vorhanden sein, erstellen Sie eine neue Compute-Instanz vom Typ „CPU“ oder „GPU“. Klicken Sie hierzu auf **Neu**. Geben Sie im Fenster **New compute instance** (Neue Compute-Instanz) unter **Computename** einen Computenamen ein, und wählen Sie einen **VM-Typ** und eine **VM-Größe** aus. Klicken Sie auf **Erstellen**.

    :::image type="content" source="./media/advanced-development-local/new-compute-instance.png" alt-text="Bildschirm für die Erstellung einer neuen Compute-Instanz":::

    Nach dem Klicken auf **Erstellen** dauert es einige Minuten, bis die Compute-Instanz erstellt wurde. Nachdem die Compute-Instanz erstellt wurde, werden als Status für **Compute** ein grüner Kreis und der Text **\<your compute name> – Wird ausgeführt** angezeigt. Diese Compute-Instanz dient zum Ausführen des Jupyter-Servers und wird für dieses Tutorial genutzt.

## <a name="visual-studio-code-development-environment-setup"></a>Einrichten der Visual Studio Code-Entwicklungsumgebung

1. Installieren Sie die erforderlichen Tools.

    1. Option 1:

        Verwenden Sie das [Dev Tools-Paketinstallationsprogramm](./dev-tools-installer.md), um die folgenden Pakete zu installieren:

        - Visual Studio Code
        - Python 3.5, 3.6 oder 3.7
        - Miniconda3
        - Intel OpenVino Toolkit 2020.2

        Hinweis: Das Intel OpenVino Toolkit ist im Dev Tools-Paketinstallationsprogramm als optionales Paket aufgeführt. Für die Arbeit mit dem Azure Percept-Vision-SOM aus dem Azure Percept Development Kit (DK) ist es jedoch erforderlich.

    1. Option 2:

        Wenn Sie das Dev Tools-Paketinstallationsprogramm nicht verwenden möchten, können Sie die folgenden Pakete manuell installieren, indem Sie auf die im Anschluss bereitgestellten Links klicken und die Download- und Installationsanweisungen befolgen:

        - [Visual Studio Code](https://code.visualstudio.com/)
        - [Python 3.5, 3.6 oder 3.7](https://www.python.org/)
        - [Miniconda3](https://docs.conda.io/en/latest/miniconda.html)
        - [Intel OpenVino Toolkit 2020.2](https://docs.openvinotoolkit.org/)

    Hinweis: Falls bei Ihnen bereits die vollständige Anaconda-Distribution installiert ist, muss Miniconda nicht installiert werden. Wenn Sie Anaconda oder Miniconda nicht verwenden möchten, können Sie alternativ eine virtuelle Python-Umgebung erstellen und die für Ihre KI-Modellentwicklung erforderlichen Pakete mithilfe von pip installieren.

1. Starten Sie Visual Studio Code.
1. Richten Sie eine Data Science-Umgebung ein:

    - Option 1: Stellen Sie eine Verbindung mit einer vorhandenen oder neuen Remote-Compute-Instanz für maschinelles Lernen her, die bereits über die zusammengestellten ML-Pakete verfügt. **Diese Option wird in diesem Tutorial verwendet.**

    - Option 2: Richten Sie eine Conda-Umgebung auf einem lokalen Computer ein.
        1. Öffnen Sie eine Anaconda- oder Miniconda-Eingabeaufforderung, und führen Sie den folgenden Befehl aus, um eine Umgebung namens **myenv** mit den angegebenen Paketen zu erstellen:

            `conda create -n myenv python=3.7 pandas jupyter seaborn scikit-learn keras tensorflow=1.15`

            Weitere Informationen zum Erstellen und Verwalten von Anaconda-Umgebungen finden Sie in der [Anaconda-Dokumentation](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).

1. Erstellen Sie einen VS Code-Arbeitsbereich:

    1. Erstellen Sie an einem geeigneten Speicherort einen Ordner, der als Ihr Visual Studio Code-Arbeitsbereich fungiert. Nennen Sie ihn **myworkspace**.
    1. Öffnen Sie **myworkspace** in Visual Studio Code, indem Sie auf **Datei** > **Ordner öffnen** > **Ordner auswählen** klicken.
    1. Navigieren Sie im Datei-Explorer zur Datei [Transferlearningusing_SSDLiteV2 Model.ipynb](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) aus Ihrer lokalen Kopie des GitHub-Repositorys für Azure Percept DK. Kopieren Sie diese Notebook-Datei in den Ordner „myworkspace“.

## <a name="azure-integration-with-visual-studio-code"></a>Azure-Integration mit Visual Studio Code

1. Registrieren Sie sich für die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree), sofern noch nicht geschehen.

1. Installieren Sie die folgenden Azure-Erweiterungen für VS Code:
    - [Azure Machine Learning-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai)
    - Python Insiders-Erweiterung. Navigieren Sie in VS Code zu **Ansicht** -> **Befehlspalette**. Geben Sie in der Befehlspalette **Python: Switch to Insiders Daily Channel** (Python: Zu Insiders Daily Channel wechseln) ein, und wählen Sie die entsprechende Option aus.
    - [Azure-Kontoerweiterung.](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) Laden Sie Ihr Fenster in VS Code neu, wenn Sie dazu aufgefordert werden.
    - [Azure IoT Hub-Toolkit-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)
    - [Azure IoT Edge-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)

1. Melden Sie sich innerhalb von Visual Studio Code bei Ihrem Azure-Konto an, um Ressourcen bereitzustellen und Workloads in Azure auszuführen.
    1. Öffnen Sie die Befehlspalette in Visual Studio Code, indem Sie auf der Menüleiste **Ansicht** > **Befehlspalette** auswählen.
    1. Geben Sie **Azure: Anmelden** in die Befehlspalette ein, um den Anmeldeprozess zu starten.

        :::image type="content" source="./media/advanced-development-local/transfer-learning-azure-sign-in-inline.png" alt-text="Azure-Anmeldebildschirm" lightbox= "./media/advanced-development-local/transfer-learning-azure-sign-in.png":::

    1. Aktivieren Sie die Python-Erweiterung und das Azure-Konto, indem Sie auf der linken Seite von VS Code auf das Azure-Symbol klicken.

        :::image type="content" source="./media/advanced-development-local/azure-icon.png" alt-text="Azure-Symbol in VS Code":::

    1. Öffnen Sie das Notebook „Transferlearningusing_SSDLiteV2 Model_VSCode.ipynb“ aus dem Ordner **myworkspace**.
    1. Öffnen Sie die Befehlspalette. Geben Sie **Python: specify local or remote Jupyter server for connections** (Python: Jupyter-Server (lokal oder remote) für Verbindungen angeben) ein, und wählen Sie die entsprechende Option aus.
    1. Daraufhin sollte eine Liste mit verfügbaren Verbindungsoptionen angezeigt werden. Wählen Sie **Azure ML Compute Instances** (Azure ML-Compute-Instanzen) aus.

        :::image type="content" source="./media/advanced-development-local/azure-machine-learning-compute-instances.png" alt-text="Optionen für Azure ML Compute-Instanzen in VS Code":::

    1. Wählen Sie ein Abonnement, einen Arbeitsbereich und eine Remote-Compute-Instanz aus, wenn Sie dazu aufgefordert werden. Wählen Sie den Arbeitsbereich und die Remote-Compute-Instanz aus, die Sie zuvor in diesem Tutorial erstellt haben. Sie können auch eine neue Compute-Instanz erstellen.
    1. Nach dem Auswählen einer Compute-Instanz werden Sie aufgefordert, Ihr VS Code-Fenster neu zu laden. Wählen Sie nach dem Neuladen den Kernel **Python 3.6 – AzureML** aus. Nun können Sie beliebige Zellen aus Ihrem Notebook ausführen. Durch Ausführen einer Notebookzelle wird die Verbindung zwischen Ihrem Notebook und Ihrer Compute-Instanz instanziiert. Die Notebooksymbolleiste wird mit der Compute-Instanz aktualisiert, an der Sie gerade arbeiten.

        :::image type="content" source="./media/advanced-development-local/kernel-inline.png" alt-text="Kernel-Auswahl in VS Code" lightbox= "./media/advanced-development-local/kernel.png":::

## <a name="working-with-the-notebook"></a>Verwenden des Notebooks

Nun können Sie das Notebook ausführen, um Ihre benutzerdefinierte Schüsselerkennung in VS Code zu trainieren und für Ihr Development Kit bereitzustellen. Wichtig: Führen Sie die Zellen des Notebooks jeweils einzeln aus, da für einige der Zellen Eingabeparameter benötigt werden, bevor das Skript ausgeführt wird. Zellen, die Eingabeparameter erfordern, können direkt im Notebook bearbeitet werden. Wenn Sie eine Zelle ausführen möchten, klicken Sie auf der linken Seite der Zelle auf das Wiedergabesymbol:

:::image type="content" source="./media/advanced-development-local/run-cell-1.png" alt-text="Notebook mit hervorgehobenem Zellensymbol":::

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispielnotebooks für Azure Machine Learning Service finden Sie in [diesem Repository](https://github.com/Azure/MachineLearningNotebooks/tree/2aa7c53b0ce84e67565d77e484987714fdaed36e/how-to-use-azureml).
