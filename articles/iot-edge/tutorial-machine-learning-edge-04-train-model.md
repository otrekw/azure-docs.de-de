---
title: 'Tutorial: Trainieren und Bereitstellen eines Modells: Machine Learning in Azure IoT Edge'
description: In diesem Tutorial trainieren Sie ein Machine Learning-Modell mit Azure Machine Learning und verpacken das Modell dann als Containerimage, das als Azure IoT Edge-Modul bereitgestellt werden kann.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 2/10/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a5c754373ba9437c631e62acbb5d6d246db4c862
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77650757"
---
# <a name="tutorial-train-and-deploy-an-azure-machine-learning-model"></a>Tutorial: Trainieren und Bereitstellen eines Azure Machine Learning-Modells

> [!NOTE]
> Dieser Artikel ist Teil einer Tutorialreihe zur Verwendung von Azure Machine Learning für IoT Edge. Falls Sie direkt zu diesem Artikel navigiert sind, ist es ratsam, mit dem [ersten Artikel](tutorial-machine-learning-edge-01-intro.md) der Reihe zu beginnen.

In diesem Artikel führen Sie die folgenden Aufgaben aus:

* Verwenden von Azure Notebooks zum Trainieren eines Machine Learning-Modells
* Packen des trainierten Modells als Containerimage
* Bereitstellen des Containerimages als Azure IoT Edge-Modul

Von Azure Notebooks wird ein Azure Machine Learning-Arbeitsbereich genutzt. Dabei handelt es sich um eine grundlegende Komponente für das Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen.

Normalerweise werden die Schritte in diesem Artikel von Data Scientists ausgeführt.

## <a name="set-up-azure-notebooks"></a>Einrichten von Azure Notebooks

Wir verwenden Azure Notebooks, um die beiden Jupyter Notebooks und die unterstützenden Dateien zu hosten. Hier erstellen und konfigurieren wir ein Azure Notebooks-Projekt. Falls Sie Jupyter bzw. Azure Notebooks noch nicht verwendet haben, helfen Ihnen diese Dokumente mit Informationen zum Einstieg weiter:

* **Schnellstart:** [Erstellen und Freigeben eines Notebooks](../notebooks/quickstart-create-share-jupyter-notebook.md)
* **Tutorial:** [Erstellen und Ausführen einer Jupyter Notebook-Datei mit Python](../notebooks/tutorial-create-run-jupyter-notebook.md)

Mit Azure Notebooks wird die Einheitlichkeit der Umgebung für die Übung sichergestellt.

> [!NOTE]
> Nach der Einrichtung kann von jedem Computer aus auf den Azure Notebooks-Dienst zugegriffen werden. Während der Einrichtung sollten Sie den virtuellen Entwicklungscomputer nutzen, auf dem alle benötigten Dateien vorhanden sind.

### <a name="create-an-azure-notebooks-account"></a>Erstellen eines Azure Notebooks-Kontos

Sie müssen ein Konto erstellen, um Azure Notebooks verwenden zu können. Azure Notebooks-Konten sind unabhängig von Azure-Abonnements.

1. Navigieren Sie zu [Azure Notebooks](https://notebooks.azure.com).

1. Klicken Sie oben rechts auf der Seite auf **Anmelden**.

1. Melden Sie sich entweder mit Ihrem Geschäfts-, Schul- oder Unikonto (Azure Active Directory) oder Ihrem persönlichen Konto (Microsoft-Konto) an.

1. Falls Sie Azure Notebooks noch nicht verwendet haben, werden Sie aufgefordert, den Zugriff für die Azure Notebooks-App zu gewähren.

1. Erstellen Sie eine Benutzer-ID für Azure Notebooks.

### <a name="upload-jupyter-notebook-files"></a>Hochladen von Jupyter Notebook-Dateien

Sie laden Notebook-Beispieldateien in ein neues Azure Notebooks-Projekt hoch.

1. Wählen Sie auf der Benutzerseite Ihres neuen Kontos auf der oberen Menüleiste **Meine Projekte** aus.

1. Geben Sie im Dialogfeld **Neues Projekt erstellen** unter **Projektname** einen Namen ein, der automatisch als **Projekt-ID** fungiert.

1. Lassen Sie **Öffentlich** and **INFODATEI** deaktiviert, da es nicht erforderlich ist, dass das Projekt öffentlich ist oder über eine Infodatei verfügt.

1. Klicken Sie auf **Erstellen**.

1. Wählen Sie **Hochladen** (Pfeil nach oben) und dann **Von Computer** aus.

1. Wählen Sie **Dateien auswählen**.

1. Navigieren Sie zu **C:\source\IoTEdgeAndMlSample\AzureNotebooks**. Wählen Sie alle Dateien in der Liste aus, und klicken Sie auf **Öffnen**.

1. Wählen Sie **Hochladen**, um mit dem Upload zu beginnen, und wählen Sie anschließend **Fertig**, wenn der Prozess abgeschlossen ist.

### <a name="azure-notebook-files"></a>Azure Notebook-Dateien

Sehen wir uns die Dateien an, die Sie in Ihr Azure Notebooks-Projekt hochgeladen haben. Die Aktivitäten in diesem Teil des Tutorials umfassen zwei Notebook-Dateien, die einige unterstützende Dateien verwenden.

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

* **RUL\_FD003.txt:** Diese Datei enthält die Restlebensdauer (RUL) für den letzten Zyklus jedes Geräts in der Datei „Test\_FD003.txt“. Die Dateien „readme.txt“ und „Damage Propagation Modeling.pdf“ unter „C:\\source\\IoTEdgeAndMlSample\\data\\Turbofan“ enthalten eine ausführliche Beschreibung der Daten.

* **Utils.py:** Enthält einen Satz mit Python-Hilfsfunktionen für die Arbeit mit Daten. Das erste Notebook enthält eine ausführliche Beschreibung der Funktionen.

* **README.md:** Infodatei, in der die Verwendung der Notebooks beschrieben ist.  

## <a name="run-azure-notebooks"></a>Ausführen von Azure Notebooks

Das Projekt wurde erstellt, und Sie können nun die Notebooks ausführen. 

1. Wählen Sie auf der Projektseite die Option **01-turbofan\_regression.ipynb** aus.

    ![Auswählen des ersten auszuführenden Notebooks](media/tutorial-machine-learning-edge-04-train-model/select-turbofan-regression-notebook.png)

1. Wählen Sie bei entsprechender Aufforderung im Dialogfeld den Python 3.6 Kernel und dann die Option **Set Kernel** (Kernel festlegen).

1. Klicken Sie oben rechts im Notebook auf das Widget **Not Trusted** (Nicht vertrauenswürdig), falls das Notebook als **Not Trusted** (Nicht vertrauenswürdig) aufgeführt ist. Wählen Sie **Trust** (Vertrauen), wenn das Dialogfeld angezeigt wird.

1. Scrollen Sie im Notebook nach unten zu der Zelle nach den Anweisungen **Set global properties** (Festlegen globaler Eigenschaften), die mit dem Code `AZURE_SUBSCRIPTION_ID =` beginnt, und geben Sie die Werte für Ihr Azure-Abonnement, die Einstellungen und die Ressourcen ein.

    ![Festlegen globaler Eigenschaften im Notebook](media/tutorial-machine-learning-edge-04-train-model/set-global-properties.png)

1. Führen Sie diese Zelle aus, indem Sie auf der Symbolleiste **Ausführen** auswählen.

    Wenn eine Zelle ausgeführt wird, wird dafür ein Sternchen zwischen eckigen Klammern angezeigt ([\*]). Wenn der Vorgang für die Zelle abgeschlossen ist, wird das Sternchen durch eine Zahl ersetzt und ggf. die relevante Ausgabe angezeigt. Die Zellen in einem Notebook werden nacheinander erstellt, und es kann jeweils nur eine Zelle ausgeführt werden.

    Befolgen Sie die Anleitung im Notebook. Sie können auch Ausführungsoptionen im **Zellenmenü** bzw. `Ctrl` + `Enter` zum Ausführen einer Zelle und `Shift` + `Enter` zum Ausführen einer Zelle und Fortfahren mit der nächsten Zelle verwenden.

    > [!TIP]
    > Führen Sie das gleiche Notebook nicht auf mehreren Tabs im Browser aus, um konsistente Zellvorgänge zu gewährleisten.

1. Scrollen Sie nach unten zur Zelle, die direkt auf den Übersichtstext **Erstellen eines Arbeitsbereichs** folgt, und führen Sie diese Zelle aus. Suchen Sie in der Ausgabe der Zelle nach dem Link, der Sie anweist, sich für die Authentifizierung anzumelden. 

    ![Anmeldeaufforderung für die Geräteauthentifizierung](media/tutorial-machine-learning-edge-04-train-model/sign-in-prompt.png)

    Öffnen Sie den Link, und geben Sie den angegebenen Code ein. Mit diesem Anmeldeverfahren wird die Jupyter Notebook-Instanz für den Zugriff auf Azure-Ressourcen unter Verwendung der plattformübergreifenden Microsoft Azure-Befehlszeilenschnittstelle authentifiziert.  

    ![Bestätigung der Anwendungsauthentifizierung auf dem Gerät](media/tutorial-machine-learning-edge-04-train-model/cross-platform-cli.png)

1. Zu diesem Zeitpunk können Sie die übrigen Zellen ausführen. Es ist optimal, alle Zellen auszuführen, sodass der Code in den Zellen nacheinander ausgeführt wird. Wählen Sie im **Zellenmenü** die Option **Alle ausführen** aus. Scrollen Sie nach oben durch das Notebook, und überprüfen Sie, wie die Zellvorgänge abgeschlossen werden.

    Im Abschnitt **Explore the data** (Untersuchen der Daten) können Sie Zellen im Unterabschnitt **Sensor readings and RUL** (Sensormesswerte und verbleibende Nutzungsdauer) überprüfen, die Streudiagramme der Sensormessungen rendern.

    ![Streudiagramme mit Sensormesswerten](media/tutorial-machine-learning-edge-04-train-model/sensor-readings.png)

1. Speichern Sie das Notebook, und kehren Sie zur Projektseite zurück, indem Sie in der rechten oberen Ecke des Notebooks auf den Projektnamen klicken oder im Browser zurücknavigieren.

1. Öffnen Sie **02-turbofan\_deploy\_model.ipynb**, und wiederholen Sie die in diesem Verfahren angegebenen Schritte, um das zweite Notebook auszuführen.

1. Speichern Sie das Notebook, und kehren Sie zur Projektseite zurück, indem Sie in der rechten oberen Ecke des Notebooks auf den Projektnamen klicken oder im Browser zurücknavigieren.

### <a name="verify-success"></a>Überprüfen des erfolgreichen Abschlusses

Überprüfen Sie, ob Elemente erstellt wurden, um sich zu vergewissern, dass die Notebooks erfolgreich abgeschlossen wurden.

1. Wählen Sie auf der Azure Notebooks-Projektseite die Option **Ausgeblendete Elemente anzeigen** aus, damit Elementnamen angezeigt werden, die mit einem Punkt beginnen.

1. Überprüfen Sie, ob die folgenden Dateien erstellt wurden:

    | Datei | BESCHREIBUNG |
    | --- | --- |
    | ./aml_config/.azureml/config.json | Die zum Erstellen des Azure Machine Learning-Arbeitsbereichs verwendete Konfigurationsdatei |
    | ./aml_config/model_config.json | Die Konfigurationsdatei, die Sie zum Bereitstellen des Modells im Machine Learning-Arbeitsbereich **turbofanDemo** in Azure benötigen |
    | myenv.yml| Diese Datei enthält Informationen zu den Abhängigkeiten für das bereitgestellte Machine Learning-Modell.|

1. Überprüfen Sie im Azure-Portal, ob der Machine Learning-Arbeitsbereich **turboFanDemo** in der Ressourcengruppe vorhanden ist.

### <a name="debugging"></a>Debuggen

Sie können Python-Anweisungen zum Debuggen in das Notebook einfügen. Dazu wird hauptsächlich der Befehl `print()` verwendet. Wenn nicht definierte Variablen oder Objekte angezeigt werden, führen Sie die Zellen aus, in denen sie zum ersten Mal deklariert oder instanziiert werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben wir zwei Jupyter Notebooks verwendet, die in Azure Notebooks ausgeführt werden. Hierbei wurden die Daten von den turbofan-Geräten genutzt, um einen Klassifizierer für die Restlebensdauer (RUL) zu trainieren, den Klassifizierer als Modell zu speichern, ein Containerimage zu erstellen und das Image als Webdienst bereitzustellen und zu testen.

Fahren Sie mit dem nächsten Artikel fort, um ein IoT Edge-Gerät zu erstellen.

> [!div class="nextstepaction"]
> [Konfigurieren eines IoT Edge-Geräts](tutorial-machine-learning-edge-05-configure-edge-device.md)
