---
title: 'Tutorial:  Erste Schritte mit maschinellem Lernen'
titleSuffix: Azure Machine Learning
description: In diesem Tutorial erhalten Sie Informationen zu den ersten Schritten mit dem Azure Machine Learning Python SDK, das in Ihrer persönlichen Entwicklungsumgebung ausgeführt wird.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 09/15/2020
ms.custom: devx-track-python
ms.openlocfilehash: 48edc138e7ab16e712339d6291db52a4a2b36b32
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90929488"
---
# <a name="tutorial-get-started-with-azure-machine-learning-on-your-development-environment-part-1-of-4"></a>Tutorial: Machen Sie die ersten Schritte mit Azure Machine Learning in Ihrer Entwicklungsumgebung (Teil 1 von 4)

In dieser **vierteiligen Tutorialreihe** lernen Sie die Grundlagen von Azure Machine Learning kennen und führen auftragsbasierte Python ML-Aufgaben in der Azure-Cloud aus, darunter:

1. Einrichten eines Arbeitsbereichs und Ihrer lokalen Entwicklerumgebung für maschinelles Lernen.
2. Ausführen von Code in der Cloud mit dem Python-SDK von Azure Machine Learning.
3. Verwalten der Python-Umgebung, die Sie zum Trainieren von Modellen verwenden.
4. Hochladen von Daten in Azure und verbrauchen dieser Daten im Training.

In **Teil 1 dieser Tutorialreihe** führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Installieren des Azure Machine Learning SDK
> * Einrichten einer Verzeichnisstruktur für Code
> * Erstellen eines Azure Machine Learning-Arbeitsbereichs
> * Konfigurieren Ihrer lokalen Entwicklungsumgebung
> * Einrichten eines Computeclusters

>[!NOTE]
> Diese Tutorialreihe konzentriert sich auf die Konzepte in Azure Machine Learning, die für __auftragsbasierte__ Machine Learning-Aufgaben mit Python geeignet sind, die rechenintensiv sind und/oder Reproduzierbarkeit voraussetzen. Wenn Ihre Machine Learning-Aufgaben nicht in dieses Profil passen, verwenden Sie die [Jupyter-oder RStudio-Funktionalität auf einer Compute-Instanz für Azure Machine Learning](tutorial-1st-experiment-sdk-setup.md) für Ihren Einstieg in Azure Machine Learning.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. [Azure Machine Learning](https://aka.ms/AMLFree) noch heute testen.
- Vertrautheit mit Python- und [Machine Learning-Konzepten](concept-azure-machine-learning-architecture.md). Beispielsweise Umgebungen, Training, Bewertung usw.
- Eine lokale Entwicklungsumgebung: ein Laptop, auf dem Python und Ihre bevorzugte IDE (beispielsweise VSCode, Pycharm, Jupyter usw.) installiert sind.

## <a name="install-the-azure-machine-learning-sdk"></a>Installieren des Azure Machine Learning SDK

In diesen Tutorial verwenden wir durchgängig das Azure Machine Learning Python SDK.

Sie können die Tools verwenden, mit denen Sie besonders vertraut sind – beispielsweise conda, pip usw. –, um eine Umgebung einzurichten, die Sie im Rahmen dieses Tutorials verwenden. Installieren Sie das Azure Machine Learning Python SDK mithilfe von pip in der Umgebung:

```bash
pip install azureml-sdk
```

## <a name="create-directory-structure-for-code"></a>Erstellen einer Verzeichnisstruktur für Code
Es empfiehlt sich, für dieses Tutorial das folgende einfache Verzeichnis einzurichten:

```markdown
tutorial
└──.azureml
```

- **tutorial** (Verzeichnis der obersten Projektebene)
- **.azureml** (verborgenes Unterverzeichnis von „tutorial“):  Das Verzeichnis `.azureml` wird verwendet, um Konfigurationsdateien von Azure Machine Learning zu speichern.

## <a name="create-an-azure-machine-learning-workspace"></a>Erstellen eines Azure Machine Learning-Arbeitsbereichs

Ein Arbeitsbereich stellt eine Ressource der obersten Ebene für Azure Machine Learning dar und bildet den zentralen Ort für diese Aufgaben:

- Verwalten von Ressourcen wie Compute
- Speichern von Ressourcen wie Notebooks, Umgebungen, Datasets, Pipelines, Modellen, Endpunkten usw.
- Zusammenarbeit mit anderen Teammitgliedern

Fügen Sie im obersten übergeordneten Verzeichnis – `tutorial` – eine neue Python-Datei mit dem Namen `01-create-workspace.py` hinzu, die den unten abgebildeten Code enthält. Passen Sie die Parameter (Name, Abonnement-ID, Ressourcengruppe und [Speicherort](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-service) nach Ihren Wünschen an.

Sie können den Code in einer interaktiven Sitzung oder als Python-Datei ausführen.

>[!NOTE]
> Wenn Sie eine lokale Entwicklungsumgebung (z. B. einen Laptop) verwenden, werden Sie aufgefordert, sich bei der ersten Ausführung des Codes unten mithilfe eines *Gerätecodes* bei Ihrem Arbeitsbereich zu authentifizieren. Befolgen Sie die Anweisungen auf dem Bildschirm.

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

Führen Sie diesen Code aus dem `tutorial`-Verzeichnis aus:

```bash
cd <path/to/tutorial>
python ./01-create-workspace.py
```

Nachdem der Codeausschnitt oben ausgeführt wurde, sieht Ihre Ordnerstruktur wie folgt aus:

```markdown
tutorial
└──.azureml
|  └──config.json
└──01-create-workspace.py
```

Die Datei `.azureml/config.json` enthält die Metadaten, die erforderlich sind, um eine Verbindung mit Ihrem Azure Machine Learning-Arbeitsbereich herzustellen – also Ihre Abonnement-ID, die Ressourcengruppe und den Namen des Arbeitsbereichs. 

> [!NOTE]
> Die Inhalte von `config.json` sind kein Geheimnis: Sie dürfen sie ohne weiteres teilen.
> Authentifizierung ist jedoch trotzdem erforderlich, um mit Ihrem Azure Machine Learning-Arbeitsbereich zu interagieren.

## <a name="create-an-azure-machine-learning-compute-cluster"></a>Erstellen eines Computeclusters für Azure Machine Learning

Erstellen Sie ein Python-Skript mit dem Namen `02-create-compute.py` im `tutorial`-Verzeichnis der obersten Ebene, und füllen Sie es mit dem folgenden Code auf, um einen Azure Machine Learning-Computecluster zu erstellen, der automatisch zwischen null und vier Knoten skaliert:

```python
# tutorial/02-create-compute.py
from azureml.core import Workspace
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

ws = Workspace.from_config() # this automatically looks for a directory .azureml

# Choose a name for your CPU cluster
cpu_cluster_name = "cpu-cluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                            max_nodes=4, 
                                                            idle_seconds_before_scaledown=2400)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)
```

Führen Sie die Python-Datei aus:

```bash
python ./02-create-compute.py
```


> [!NOTE]
> Wenn der Cluster erstellt wurde, weist er 0 bereitgestellte Knoten auf. Daher versursacht der Cluster **keine** Kosten, bis Sie ihm einen Auftrag übermitteln. Der Cluster skaliert nach 2400 Sekunden (40 Minuten) Leerlauf herunter.

Ihre Ordnerstruktur sieht nun wie folgt aus:

```bash
tutorial
└──.azureml
|  └──config.json
└──01-create-workspace.py
└──02-create-compute.py
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Setup-Tutorial haben Sie folgende Aufgaben ausgeführt:

- Erstellen eines Azure Machine Learning-Arbeitsbereichs
- Einrichten Ihrer lokalen Entwicklungsumgebung
- Erstellen eines Computeclusters für Azure Machine Learning

Im nächsten Tutorial erfahren Sie, wie Sie ein Skript an den Azure Machine Learning-Computecluster senden.

> [!div class="nextstepaction"]
> [Tutorial: Ausführen des Python-Skripts „Hallo Welt“ in Azure](tutorial-1st-experiment-hello-world.md)
