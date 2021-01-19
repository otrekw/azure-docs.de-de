---
title: 'Tutorial: Trainieren und Bereitstellen eines Modells: Machine Learning in Azure IoT Edge'
description: In diesem Tutorial trainieren Sie ein Machine Learning-Modell mit Azure Machine Learning und verpacken das Modell dann als Containerimage, das als Azure IoT Edge-Modul bereitgestellt werden kann.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 3/24/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 2cc96db88d9a2aec02de5e2fc4ed18b445972e7b
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2021
ms.locfileid: "98121135"
---
# <a name="tutorial-train-and-deploy-an-azure-machine-learning-model"></a>Tutorial: Trainieren und Bereitstellen eines Azure Machine Learning-Modells

In diesem Artikel führen Sie die folgenden Aufgaben aus:

* Trainieren eines Machine Learning-Modells mithilfe von Azure Machine Learning Studio
* Packen des trainierten Modells als Containerimage
* Bereitstellen des Containerimages als Azure IoT Edge-Modul

Azure Machine Learning Studio ist eine grundlegende Komponente für das Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen.

Normalerweise werden die Schritte in diesem Artikel von Data Scientists ausgeführt.

In diesem Abschnitt des Tutorials lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen von Jupyter Notebook-Instanzen in einem Azure Machine Learning-Arbeitsbereich zum Trainieren eines Machine Learning-Modells
> * Containerisieren des trainiertes Machine Learning-Modells
> * Erstellen eines Azure IoT Edge-Moduls auf der Grundlage des containerisierten Machine Learning-Modells

## <a name="prerequisites"></a>Voraussetzungen

Dieser Artikel ist Teil einer Tutorialreihe zur Verwendung von Azure Machine Learning für IoT Edge. Jeder Artikel in der Reihe baut auf der Arbeit im vorherigen Artikel auf. Wenn Sie diesen Artikel direkt aufgerufen haben, wechseln Sie zum [ersten Artikel](tutorial-machine-learning-edge-01-intro.md) in der Reihe.

## <a name="set-up-azure-machine-learning"></a>Einrichten von Azure Machine Learning 

Sie verwenden Azure Machine Learning Studio, um die beiden Jupyter Notebook-Instanzen und die unterstützenden Dateien zu hosten. Hier erstellen und konfigurieren Sie ein Azure Machine Learning-Projekt. Falls Sie Jupyter und/oder Azure Machine Learning Studio noch nicht verwendet haben, helfen Ihnen diese Dokumente mit Informationen zum Einstieg weiter:

* **Jupyter Notebook:** [Arbeiten mit Jupyter Notebook in Visual Studio Code](https://code.visualstudio.com/docs/python/jupyter-support)
* **Azure Machine Learning:** [Tutorial: Machen Sie sich mit den ersten Schritten mit Azure Machine Learning in Jupyter Notebooks vertraut.](../machine-learning/tutorial-1st-experiment-sdk-setup.md)


> [!NOTE]
> Nach der Einrichtung kann von jedem Computer aus auf Azure Machine Learning Service zugegriffen werden. Während der Einrichtung sollten Sie den virtuellen Entwicklungscomputer nutzen, auf dem alle benötigten Dateien vorhanden sind.

### <a name="install-azure-machine-learning-visual-studio-code-extension"></a>Installieren der Azure Machine Learning-Erweiterung für Visual Studio Code
Für VS Code auf dem virtuellen Entwicklungscomputer muss diese Erweiterung installiert sein. Wenn die Ausführung in einer anderen Instanz erfolgt, installieren Sie die Erweiterung wie [hier](../machine-learning/tutorial-setup-vscode-extension.md) beschrieben neu.

### <a name="create-an-azure-machine-learning-account"></a>Erstellen eines Azure Machine Learning-Kontos  
Zum Bereitstellen von Ressourcen und Ausführen von Workloads in Azure müssen Sie sich mit Ihren Anmeldeinformationen für Ihr Azure-Konto anmelden.

1. Öffnen Sie in Visual Studio Code die Befehlspalette, indem Sie auf der Menüleiste **Ansicht** > **Befehlspalette** auswählen. 

1. Geben Sie in der Befehlspalette den Befehl `Azure: Sign In` ein, um den Anmeldeprozess zu starten. Folgen Sie den Anweisungen, um die Anmeldung abzuschließen. 

1. Erstellen Sie eine Azure ML Compute-Instanz, um die Workload auszuführen. Geben Sie unter Verwendung der Befehlspalette den Befehl `Azure ML: Create Compute` ein. 
1. Auswählen Ihres Azure-Abonnements
1. Wählen Sie die Option **+ Create a new Azure ML workspace** (Neuen Azure ML-Arbeitsbereich erstellen) aus, und geben Sie den Namen `turbofandemo` ein.
1. Wählen Sie die Ressourcengruppe aus, die Sie für diese Demo verwendet haben.
1. Der Status der Arbeitsbereichserstellung sollte in der unteren rechten Ecke des VS Code-Fensters angezeigt werden: **Arbeitsbereich wird erstellt: turobofandemo**. (Dieser Vorgang kann ein bis zwei Minuten dauern.) 
1. Warten Sie, bis der Arbeitsbereich erstellt wurde. Die Meldung **Azure ML workspace turbofandemo created** (Der Azure ML-Arbeitsbereich 'turbofandemo' wurde erstellt.) sollte angezeigt werden.


### <a name="upload-jupyter-notebook-files"></a>Hochladen von Jupyter Notebook-Dateien

Sie laden Notebook-Beispieldateien in einen neuen Azure ML-Arbeitsbereich hoch.

1. Navigieren Sie zu ml.azure.com, und melden Sie sich an.
1. Wählen Sie Ihr Microsoft-Verzeichnis, das Azure-Abonnement und den neu erstellten Azure ML-Arbeitsbereich aus.

    :::image type="content" source="media/tutorial-machine-learning-edge-04-train-model/select-studio-workspace.png" alt-text="Auswählen des Azure ML-Arbeitsbereichs" :::

1. Nachdem Sie sich bei Ihrem Azure ML-Arbeitsbereich angemeldet haben, navigieren Sie im Menü auf der linken Seite zum Abschnitt **Notebooks**.
1. Wählen Sie die Registerkarte **Eigene Dateien** aus.

1. Wählen Sie **Hochladen** (Pfeil nach oben) aus. 


1. Navigieren Sie zu **C:\source\IoTEdgeAndMlSample\AzureNotebooks**. Wählen Sie alle Dateien in der Liste aus, und klicken Sie auf **Öffnen**.

1. Aktivieren Sie das Kontrollkästchen **I trust the content of these files** (Ich vertraue dem Inhalt dieser Dateien.).

1. Wählen Sie **Hochladen**, um mit dem Upload zu beginnen, und wählen Sie anschließend **Fertig**, wenn der Prozess abgeschlossen ist.

### <a name="jupyter-notebook-files"></a>Jupyter Notebook-Dateien

Sehen Sie sich die Dateien an, die Sie in Ihren Azure ML-Arbeitsbereich hochgeladen haben. Die Aktivitäten in diesem Teil des Tutorials umfassen zwei Notebook-Dateien, die einige unterstützende Dateien verwenden.

* **01-turbofan\_regression.ipynb:** Dieses Notebook verwendet den Machine Learning-Dienstarbeitsbereich zum Erstellen und Ausführen eines Machine Learning-Experiments. Mit dem Notebook werden im Allgemeinen die folgenden Schritte ausgeführt:

  1. Herunterladen von Daten aus dem Azure Storage-Konto, das von der Geräteumgebung erstellt wurde
  1. Untersucht und bereitet die Daten vor und verwendet diese dann zum Trainieren des Klassifizierungsmodells.
  1. Bewerten des Modells aus dem Experiment mithilfe eines Testdatasets (Test\_FD003.txt)
  1. Veröffentlichen des besten Klassifizierungsmodells im Machine Learning-Dienstarbeitsbereich

* **02-turbofan\_deploy\_model.ipynb:** Bei diesem Notebook wird das im vorherigen Notebook erstellte Modell für die Erstellung eines Containerimages verwendet, das für die Bereitstellung auf einem Azure IoT Edge-Gerät bereit ist. Mit diesem Notebook werden die folgenden Schritte ausgeführt:

  1. Erstellen eines Bewertungsskripts für das Modell
  1. Erstellen eines Containerimages mithilfe des Klassifizierungsmodells, das im Machine Learning-Dienstarbeitsbereich gespeichert wurde
  1. Bereitstellen des Images als Webdienst in Azure Container Instances
  1. Verwenden des Webdiensts zum Überprüfen, ob das Modell und das Image wie erwartet funktionieren Das überprüfte Image wird im Teil [Erstellen und Bereitstellen von benutzerdefinierten IoT Edge-Modulen](tutorial-machine-learning-edge-06-custom-modules.md) dieses Tutorials auf unserem IoT Edge-Gerät bereitgestellt.

* **Test\_FD003.txt:** Diese Datei enthält die Daten, die wir beim Überprüfen unseres trainierten Klassifizierers als unseren Testsatz verwenden. Der Einfachheit halber haben wir die Testdaten so genutzt, wie sie ursprünglich bereitgestellt wurden.

* **RUL\_FD003.txt:** Diese Datei enthält die verbleibende Nutzungsdauer (Remaining Useful Life, RUL) für den letzten Zyklus jedes Geräts in der Datei „Test\_FD003.txt“. Die Dateien „readme.txt“ und „Damage Propagation Modeling.pdf“ unter „C:\\source\\IoTEdgeAndMlSample\\data\\Turbofan“ enthalten eine ausführliche Beschreibung der Daten.

* **Utils.py:** Enthält einen Satz mit Python-Hilfsfunktionen für die Arbeit mit Daten. Das erste Notebook enthält eine ausführliche Beschreibung der Funktionen.

* **README.md:** Infodatei, in der die Verwendung der Notebooks beschrieben ist.  

## <a name="run-jupyter-notebooks"></a>Ausführen von Jupyter Notebook-Instanzen

Der Arbeitsbereich wurde erstellt, und Sie können nun die Notebooks ausführen. 

1. Wählen Sie **01-turbofan\_regression.ipynb** auf der Seite **Eigene Dateien** aus.

    :::image type="content" source="media/tutorial-machine-learning-edge-04-train-model/select-turbofan-notebook.png" alt-text="Auswählen des ersten auszuführenden Notebooks":::

1. Klicken Sie oben rechts im Notebook auf das Widget **Not Trusted** (Nicht vertrauenswürdig), falls das Notebook als **Not Trusted** (Nicht vertrauenswürdig) aufgeführt ist. Wählen Sie **Trust** (Vertrauen), wenn das Dialogfeld angezeigt wird.

1. Lesen Sie die Dokumentation für jede Zelle, und führen Sie sie einzeln aus, um optimale Ergebnisse zu erzielen. Wählen Sie auf der Symbolleiste **Ausführen** aus. Später empfiehlt es sich, mehrere Zellen auszuführen. Sie können Upgrade- und Ablaufwarnungen ignorieren.

    Wenn eine Zelle ausgeführt wird, wird dafür ein Sternchen zwischen eckigen Klammern angezeigt ([\*]). Wenn der Vorgang für die Zelle abgeschlossen ist, wird das Sternchen durch eine Zahl ersetzt und ggf. die relevante Ausgabe angezeigt. Die Zellen in einem Notebook werden nacheinander erstellt, und es kann jeweils nur eine Zelle ausgeführt werden.

    Sie können auch Ausführungsoptionen im **Zellenmenü** bzw. `Ctrl` + `Enter` zum Ausführen einer Zelle und `Shift` + `Enter` zum Ausführen einer Zelle und Fortfahren mit der nächsten Zelle verwenden.

    > [!TIP]
    > Führen Sie das gleiche Notebook nicht auf mehreren Tabs im Browser aus, um konsistente Zellvorgänge zu gewährleisten.

1. Schreiben Sie die Werte für Ihr Azure-Abonnement, die Einstellungen und die Ressourcen in die Zelle nach den Anweisungen **Set global properties** (Festlegen globaler Eigenschaften). Führen Sie anschließend die Zelle aus.

    ![Festlegen globaler Eigenschaften im Notebook](media/tutorial-machine-learning-edge-04-train-model/set-global-properties.png)

1. Suchen Sie in der Zelle vor den **Details zum Arbeitsbereich** nach ihrer Ausführung nach dem Link, der Sie anweist, sich für die Authentifizierung anzumelden.

    ![Anmeldeaufforderung für die Geräteauthentifizierung](media/tutorial-machine-learning-edge-04-train-model/sign-in-prompt.png)

    Öffnen Sie den Link, und geben Sie den angegebenen Code ein. Mit diesem Anmeldeverfahren wird die Jupyter Notebook-Instanz für den Zugriff auf Azure-Ressourcen unter Verwendung der plattformübergreifenden Microsoft Azure-Befehlszeilenschnittstelle authentifiziert.  

    ![Bestätigung der Anwendungsauthentifizierung auf dem Gerät](media/tutorial-machine-learning-edge-04-train-model/cross-platform-cli.png)

1. Kopieren Sie in der Zelle vor **Untersuchen der Ergebnisse** den Wert der Ausführungs-ID, und fügen Sie ihn für die Ausführungs-ID in die Zelle nach **Wiederherstellen einer Ausführung** ein.

   ![Kopieren der Ausführungs-ID zwischen Zellen](media/tutorial-machine-learning-edge-04-train-model/automl-id.png)

1. Führen Sie die übrigen Zellen im Notebook aus.

1. Speichern Sie das Notebook, und kehren Sie zur Projektseite zurück.

1. Öffnen Sie **02-turbofan\_deploy\_model.ipynb**, und führen Sie die einzelnen Zellen aus. Zur Authentifizierung müssen Sie sich in der Zelle nach **Konfigurieren des Arbeitsbereichs** anmelden.

1. Speichern Sie das Notebook, und kehren Sie zur Projektseite zurück.

### <a name="verify-success"></a>Überprüfen des erfolgreichen Abschlusses

Überprüfen Sie, ob Elemente erstellt wurden, um sich zu vergewissern, dass die Notebooks erfolgreich abgeschlossen wurden.

1. Wählen Sie auf der Registerkarte **Eigene Dateien** der Azure ML-Notebooks die Option **Aktualisieren** aus.

1. Überprüfen Sie, ob die folgenden Dateien erstellt wurden:

    | Datei | BESCHREIBUNG |
    | --- | --- |
    | ./aml_config/.azureml/config.json | Die zum Erstellen des Azure Machine Learning-Arbeitsbereichs verwendete Konfigurationsdatei |
    | ./aml_config/model_config.json | Die Konfigurationsdatei, die Sie zum Bereitstellen des Modells im Machine Learning-Arbeitsbereich **turbofanDemo** in Azure benötigen |
    | myenv.yml| Diese Datei enthält Informationen zu den Abhängigkeiten für das bereitgestellte Machine Learning-Modell.|

1. Überprüfen Sie, ob die folgenden Azure-Ressourcen erstellt wurden. An einige Ressourcennamen werden zufällig gewählte Zeichen angefügt.

    | Azure-Ressource | Name |
    | --- | --- |
    | Machine Learning-Arbeitsbereich | turborfanDemo |
    | Containerregistrierung | turbofandemoxxxxxxxx |
    | Application Insights | turbofaninsightxxxxxxxx |
    | Key Vault | turbofankeyvaultbxxxxxxxx |
    | Storage | turbofanstoragexxxxxxxxx |

### <a name="debugging"></a>Debuggen

Sie können Python-Anweisungen zum Debuggen in das Notebook einfügen, etwa den Befehl `print()` zum Anzeigen von Werten. Wenn nicht definierte Variablen oder Objekte angezeigt werden, führen Sie die Zellen aus, in denen sie zum ersten Mal deklariert oder instanziiert werden.

Unter Umständen müssen zuvor erstellte Dateien und Azure-Ressourcen gelöscht werden, falls Sie die Notebooks wiederholen möchten.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Dieses Tutorial ist Teil einer Reihe, in der jeder Artikel auf den Schritten aufbaut, die jeweils im vorherigen Artikel ausgeführt wurden. Warten Sie mit dem Bereinigen von Ressourcen, bis Sie das letzte Tutorial abgeschlossen haben.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie zwei Jupyter Notebook-Instanzen verwendet, die in Azure ML Studio ausgeführt werden. Hierbei wurden die Daten von den turbofan-Geräten genutzt, um einen Klassifizierer für die Restlebensdauer (RUL) zu trainieren, den Klassifizierer als Modell zu speichern, ein Containerimage zu erstellen und das Image als Webdienst bereitzustellen und zu testen.

Fahren Sie mit dem nächsten Artikel fort, um ein IoT Edge-Gerät zu erstellen.

> [!div class="nextstepaction"]
> [Konfigurieren eines IoT Edge-Geräts](tutorial-machine-learning-edge-05-configure-edge-device.md)