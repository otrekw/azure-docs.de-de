---
title: 'Tutorial: Erste Schritte mit Machine Learning: Python'
titleSuffix: Azure Machine Learning
description: In diesem Tutorial erhalten Sie Informationen zu den ersten Schritten mit dem Azure Machine Learning SDK für Python, das in Ihrer persönlichen Entwicklungsumgebung ausgeführt wird.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 09/15/2020
ms.custom: devx-track-python
ms.openlocfilehash: 2f33fe4fafbe194238fcfbd4942807ed2fc4d6ff
ms.sourcegitcommit: 0aec60c088f1dcb0f89eaad5faf5f2c815e53bf8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98183539"
---
# <a name="tutorial-get-started-with-azure-machine-learning-in-your-development-environment-part-1-of-4"></a>Tutorial: Erste Schritte mit Azure Machine Learning in Ihrer Entwicklungsumgebung (Teil 1 von 4)

In dieser *vierteiligen Tutorialreihe* lernen Sie die Grundlagen von Azure Machine Learning kennen und führen auftragsbasierte Python-Machine Learning-Aufgaben auf der Azure-Cloudplattform durch. 

In Teil 1 dieser Tutorialreihe führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Installieren des Azure Machine Learning SDK
> * Einrichten der Verzeichnisstruktur für Code
> * Erstellen eines Azure Machine Learning-Arbeitsbereichs
> * Konfigurieren Ihrer lokalen Entwicklungsumgebung
> * Einrichten eines Computeclusters

> [!NOTE]
> Bei dieser Tutorialreihe liegt der Schwerpunkt auf den Azure Machine Learning-Konzepten, die zum Übermitteln von **Batchaufträgen** erforderlich sind. Dabei wird der Code zur Ausführung im Hintergrund ohne jegliche Benutzerinteraktion in die Cloud übermittelt. Dies ist nützlich für fertige Skripts oder fertigen Code, die bzw. den Sie wiederholt ausführen möchten, oder für rechenintensive Machine Learning-Aufgaben. Wenn Sie mehr an einem explorativen Workflow interessiert sind, könnten Sie stattdessen [Jupyter oder RStudio auf einer Azure Machine Learning-Compute-Instanz](tutorial-1st-experiment-sdk-setup.md) verwenden.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie [Azure Machine Learning](https://aka.ms/AMLFree) aus.
- [Anaconda](https://www.anaconda.com/download/) oder [Miniconda](https://www.anaconda.com/download/) zum Verwalten virtueller Python-Umgebungen und zum Installieren von Paketen

## <a name="install-the-azure-machine-learning-sdk"></a>Installieren des Azure Machine Learning SDK

In diesem Tutorial verwenden Sie durchgängig das Azure Machine Learning SDK für Python. Sie erstellen eine isolierte Umgebung, um Python-Abhängigkeitsprobleme zu vermeiden. In dieser Tutorialreihe wird Conda zum Erstellen dieser Umgebung verwendet. Wenn Sie andere Lösungen verwenden möchten (etwa `venv`, `virtualenv` oder Docker), stellen Sie sicher, dass Sie mindestens Python-Version 3.5, aber eine niedrigere Version als 3.9 verwenden.

Überprüfen Sie, ob Conda auf Ihrem System installiert ist:
    
```bash
conda --version
```
    
Wird von diesem Befehl der Fehler `conda not found` zurückgegeben, [müssen Sie Miniconda herunterladen und installieren](https://docs.conda.io/en/latest/miniconda.html). 

Verwenden Sie nach der Installation von Conda ein Terminal oder ein Anaconda-Eingabeaufforderungsfenster, um eine neue Umgebung zu erstellen:

```bash
conda create -n tutorial python=3.8
```

Installieren Sie als Nächstes das Azure Machine Learning SDK in der von Ihnen erstellten Conda-Umgebung:

```bash
conda activate tutorial
pip install azureml-core
```
    
> [!NOTE]
> Es dauert etwa zwei Minuten, bis die Installation des Azure Machine Learning SDK abgeschlossen ist.
>
> Wird ein Timeoutfehler angezeigt, versuchen Sie es stattdessen mit `pip install --default-timeout=100 azureml-core`.


> [!div class="nextstepaction"]
> [Ich habe das SDK installiert.](?success=install-sdk#dir) [Es ist ein Problem aufgetreten.](https://www.research.net/r/7C8Z3DN?issue=install-sdk)

## <a name="create-a-directory-structure-for-code"></a><a name="dir"></a>Erstellen einer Verzeichnisstruktur für Code

Wir empfehlen Ihnen, für dieses Tutorial die folgende einfache Verzeichnisstruktur einzurichten:

```markdown
tutorial
└──.azureml
```

- `tutorial`: Oberstes Verzeichnis des Projekts.
- `.azureml`: Ausgeblendetes Unterverzeichnis zum Speichern von Azure Machine Learning-Konfigurationsdateien.

> [!TIP]
> Sie können das ausgeblendete Unterverzeichnis „.azureml“ in einem Terminalfenster erstellen.  Oder verwenden Sie Folgendes:
>
> * Verwenden Sie auf einem Macintosh-Computer in einem Suchfenster die Tastenkombination **BEFEHL+UMSCHALT+.** , um das Anzeigen und Erstellen von Verzeichnissen, die mit einem Punkt beginnen, zu aktivieren.  
> * Informationen zum Windows 10-Datei-Explorer finden Sie unter [Anzeigen ausgeblendeter Dateien und Ordner](https://support.microsoft.com/en-us/windows/view-hidden-files-and-folders-in-windows-10-97fbc472-c603-9d90-91d0-1166d1d9f4b5). 
> * Verwenden Sie auf der grafischen Linux-Benutzeroberfläche **STRG+h** oder das Menü **Ansicht**, und aktivieren Sie das Kontrollkästchen **Show hidden files** (Ausgeblendete Dateien anzeigen).

> [!div class="nextstepaction"]
> [Ich habe ein Verzeichnis erstellt.](?success=create-dir#workspace) [Es ist ein Problem aufgetreten.](https://www.research.net/r/7C8Z3DN?issue=create-dir)

## <a name="create-an-azure-machine-learning-workspace"></a><a name="workspace"></a>Erstellen eines Azure Machine Learning-Arbeitsbereichs

Ein Arbeitsbereich stellt eine Ressource der obersten Ebene für Azure Machine Learning dar und bildet den zentralen Ort für diese Aufgaben:

- Verwalten von Ressourcen, z. B. Computeressourcen
- Speichern von Ressourcen wie Notebooks, Umgebungen, Datasets, Pipelines, Modellen und Endpunkten
- Zusammenarbeiten mit anderen Teammitgliedern

Fügen Sie im Verzeichnis der obersten Ebene (`tutorial`) eine neue Python-Datei mit dem Namen `01-create-workspace.py` hinzu, indem Sie den unten angegebenen Code verwenden. Passen Sie die Parameter (Name, Abonnement-ID, Ressourcengruppe und [Speicherort](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-service)) nach Ihren Wünschen an.

Sie können den Code in einer interaktiven Sitzung oder als Python-Datei ausführen.

>[!NOTE]
> Bei Verwendung einer lokalen Entwicklungsumgebung (z. B. Ihr Computer) werden Sie zum Durchführen der Authentifizierung für Ihren Arbeitsbereich mit einem *Gerätecode* aufgefordert, wenn Sie den unten angegebenen Code zum ersten Mal ausführen. Befolgen Sie die Anweisungen auf dem Bildschirm.

```python
# tutorial/01-create-workspace.py
from azureml.core import Workspace

ws = Workspace.create(name='<my_workspace_name>', # provide a name for your workspace
                      subscription_id='<azure-subscription-id>', # provide your subscription ID
                      resource_group='<myresourcegroup>', # provide a resource group name
                      create_resource_group=True,
                      location='<NAME_OF_REGION>') # For example: 'westeurope' or 'eastus2' or 'westus2' or 'southeastasia'.

# write out the workspace details to a configuration file: .azureml/config.json
ws.write_config(path='.azureml')
```

Führen Sie in dem Fenster mit der aktivierten Conda-Umgebung *tutorial1* den folgenden Code aus dem Verzeichnis `tutorial` aus:

```bash
cd <path/to/tutorial>
python ./01-create-workspace.py
```

> [!TIP]
> Wenn Sie beim Ausführen dieses Codes den Fehler erhalten, dass Sie keinen Zugriff auf das Abonnement haben, finden Sie Informationen zu Authentifizierungsoptionen unter [Erstellen eines Arbeitsbereichs](how-to-manage-workspace.md?tab=python#create-multi-tenant).


Nachdem Sie *01-create-workspace.py* erfolgreich ausgeführt haben, sieht Ihre Ordnerstruktur wie folgt aus:

```markdown
tutorial
└──.azureml
|  └──config.json
└──01-create-workspace.py
```

Die Datei `.azureml/config.json` enthält die Metadaten, die für die Verbindungsherstellung mit Ihrem Azure Machine Learning-Arbeitsbereich benötigt werden. Sie enthält die Abonnement-ID, die Ressourcengruppe und den Namen des Arbeitsbereichs. 

> [!NOTE]
> Beim Inhalt von `config.json` handelt es sich nicht um Geheimnisse. Diese Details können also weitergegeben werden.
>
> Authentifizierung ist jedoch trotzdem erforderlich, um mit Ihrem Azure Machine Learning-Arbeitsbereich zu interagieren.

> [!div class="nextstepaction"]
> [Ich habe einen Arbeitsbereich erstellt.](?success=create-workspace#cluster) [Es ist ein Problem aufgetreten.](https://www.research.net/r/7C8Z3DN?issue=create-workspace)

## <a name="create-an-azure-machine-learning-compute-cluster"></a><a name="cluster"></a> Erstellen eines Computeclusters für Azure Machine Learning

Erstellen Sie im Verzeichnis `tutorial` der obersten Ebene ein Python-Skript mit dem Namen `02-create-compute.py`. Fügen Sie den folgenden Code ein, um einen Azure Machine Learning-Computecluster zu erstellen, mit dem automatisch eine Skalierung auf null bis vier Knoten durchgeführt wird:

```python
# tutorial/02-create-compute.py
from azureml.core import Workspace
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

ws = Workspace.from_config() # This automatically looks for a directory .azureml

# Choose a name for your CPU cluster
cpu_cluster_name = "cpu-cluster"

# Verify that the cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           idle_seconds_before_scaledown=2400,
                                                           min_nodes=0,
                                                           max_nodes=4)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)
```

Führen Sie in dem Fenster mit der aktivierten Conda-Umgebung *tutorial1* die Python-Datei aus:

```bash
python ./02-create-compute.py
```


> [!NOTE]
> Nachdem der Cluster erstellt wurde, weist er keine bereitgestellten Knoten auf. Der Cluster verursacht *keine* Kosten, bis Sie dafür einen Auftrag übermitteln. Der Cluster wird herunterskaliert, nachdem er sich 2.400 Sekunden (40 Minuten) im Leerlauf befunden hat.

Ihre Ordnerstruktur sieht nun wie folgt aus:

```bash
tutorial
└──.azureml
|  └──config.json
└──01-create-workspace.py
└──02-create-compute.py
```

> [!div class="nextstepaction"]
> [Ich habe einen Computecluster erstellt.](?success=create-compute-cluster#next-steps) [Es ist ein Problem aufgetreten.](https://www.research.net/r/7C8Z3DN?issue=create-compute-cluster)

## <a name="view-in-the-studio"></a>Anzeigen in Studio

Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com) an, um den Arbeitsbereich und die Compute-Instanz anzuzeigen, den bzw. die Sie erstellt haben.

1. Wählen Sie das **Abonnement** aus, das Sie zum Erstellen des Arbeitsbereichs verwendet haben.
1. Wählen Sie den **Machine Learning-Arbeitsbereich** aus, den Sie erstellt haben: *tutorial-ws*.
1. Wählen Sie nach dem Laden des Arbeitsbereichs auf der linken Seite **Compute** aus.
1. Wählen Sie oben die Registerkarte **Computecluster** aus.

:::image type="content" source="media/tutorial-1st-experiment-sdk-local/compute-instance-in-studio.png" alt-text="Screenshot: Anzeigen der Compute-Instanz in Ihrem Arbeitsbereich":::

In dieser Ansicht wird der bereitgestellte Computecluster zusammen mit der Anzahl der Knoten im Leerlauf, der ausgelasteten Knoten und der nicht bereitgestellten Knoten angezeigt.  Da Sie den Cluster noch nicht verwendet haben, sind alle Knoten derzeit nicht bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Setup-Tutorial haben Sie folgende Aufgaben durchgeführt:

- Erstellen eines Azure Machine Learning-Arbeitsbereichs
- Einrichten Ihrer lokalen Entwicklungsumgebung
- Erstellen eines Computeclusters für Azure Machine Learning

In den anderen Teilen dieses Tutoriums werden Sie Folgendes erfahren:

* Teil 2. Ausführen von Code in der Cloud mit dem Azure Machine Learning SDK für Python
* Teil 3. Verwalten der Python-Umgebung, die Sie zum Trainieren von Modellen verwenden
* Teil 4. Hochladen von Daten in Azure und verbrauchen dieser Daten im Training.

Im nächsten Tutorial erfahren Sie, wie Sie ein Skript an den Azure Machine Learning-Computecluster senden.

> [!div class="nextstepaction"]
> [Tutorial: Ausführen eines Python-Skripts „Hello World!“ (Teil 2 von 4)](tutorial-1st-experiment-hello-world.md)
