---
title: Trainieren eines Modells mit einem benutzerdefinierten Docker-Image
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Modelle mit benutzerdefinierten Docker-Images in Azure Machine Learning trainieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: sagopal
author: saachigopal
ms.date: 08/11/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: d90b56366cb22e80162983c982e861de608e4e9e
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90893117"
---
# <a name="train-a-model-using-a-custom-docker-image"></a>Trainieren eines Modells mit einem benutzerdefinierten Docker-Image

In diesem Artikel erfahren Sie, wie Sie ein benutzerdefiniertes Docker-Image zum Trainieren von Modellen mit Azure Machine Learning verwenden. 

Mit den Beispielskripts in diesem Artikel werden Bilder von Haustieren durch Erstellen eines Convolutional Neural Network klassifiziert. 

Azure Machine Learning bietet ein standardmäßiges Docker-Basisimage, Sie können jedoch auch Azure Machine Learning-Umgebungen verwenden, um ein bestimmtes Basisimage anzugeben, z. B. eines der verwalteten [Azure ML-Basisimages](https://github.com/Azure/AzureML-Containers) oder Ihr eigenes [benutzerdefiniertes Image](how-to-deploy-custom-docker-image.md#create-a-custom-base-image). Mithilfe von benutzerdefinierten Basisimages können Sie Abhängigkeiten umfassend verwalten und die Komponentenversionen beim Ausführen von Trainingsaufträgen genauer steuern. 

## <a name="prerequisites"></a>Voraussetzungen 
Führen Sie diesen Code in einer dieser Umgebungen aus:
* Azure Machine Learning-Compute-Instanz: keine Downloads oder Installationen erforderlich
    * Absolvieren Sie [Tutorial: Einrichten von Umgebung und Arbeitsbereich](tutorial-1st-experiment-sdk-setup.md), um einen dedizierten Notebookserver zu erstellen, auf dem das SDK und das Beispielrepository vorinstalliert sind.
    * Im Azure Machine Learning-[Beispielrepository](https://github.com/Azure/azureml-examples) finden Sie ein abgeschlossenes Notebook im Verzeichnis **notebooks > fastai > train-pets-resnet34.ipynb**. 

* Ihr eigener Jupyter Notebook-Server
    * Erstellen Sie eine [Konfigurationsdatei für den Arbeitsbereich](how-to-configure-environment.md#workspace).
    * Das [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true). 
    * Ein [Azure Container Registry](/azure/container-registry) oder eine andere Docker-Registrierung, die über das Internet zugänglich ist.

## <a name="set-up-the-experiment"></a>Einrichten des Experiments 
In diesem Abschnitt wird das Trainingsexperiment eingerichtet, indem ein Arbeitsbereich initialisiert, ein Experiment erstellt und die Trainingsdaten und -skripts hochgeladen werden.

### <a name="initialize-a-workspace"></a>Initialisieren eines Arbeitsbereichs
Der [Azure Machine Learning-Arbeitsbereich](concept-workspace.md) ist die Ressource der obersten Ebene für den Dienst. Er stellt den zentralen Ort für die Arbeit mit allen erstellten Artefakten dar. Im Python SDK können Sie auf die Arbeitsbereichsartefakte zugreifen, indem Sie ein [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py&preserve-view=true)-Objekt erstellen.

Erstellen Sie ein Arbeitsbereichsobjekt aus der Datei `config.json`, die im [Abschnitt „Voraussetzungen“](#prerequisites) erstellt wurde.

```Python
from azureml.core import Workspace

ws = Workspace.from_config()
```

### <a name="prepare-scripts"></a>Vorbereiten von Skripts
Das Trainingsskript **train.py** für dieses Tutorial wird [hier](https://github.com/Azure/azureml-examples/blob/main/code/models/fastai/pets-resnet34/train.py) bereitgestellt. In der Praxis können Sie jedes benutzerdefinierte Trainingsskript, wie es ist, verwenden und mit Azure Machine Learning ausführen.

### <a name="define-your-environment"></a>Definieren der Umgebung
Erstellen Sie ein Umgebungsobjekt, und aktivieren Sie Docker. 

```python
from azureml.core import Environment

fastai_env = Environment("fastai2")
fastai_env.docker.enabled = True
```

Dieses angegebene Basisimage unterstützt die Bibliothek „fast.ai“, die Funktionen für verteiltes Deep Learning ermöglicht. Weitere Informationen finden Sie im [DockerHub zu „fast.ai“](https://hub.docker.com/u/fastdotai). 

Wenn Sie Ihr benutzerdefiniertes Docker-Image verwenden, ist Ihre Python-Umgebung möglicherweise bereits ordnungsgemäß eingerichtet. Legen Sie in diesem Fall das Flag `user_managed_dependencies` auf TRUE fest, um die integrierte Python-Umgebung Ihres benutzerdefinierten Images zu nutzen. Standardmäßig erstellt Azure ML eine Conda-Umgebung mit den von Ihnen angegebenen Abhängigkeiten und führt die Ausführung in dieser Umgebung aus, anstatt die Python-Bibliotheken zu verwenden, die Sie auf dem Basisimage installiert haben.

```python
fastai_env.docker.base_image = "fastdotai/fastai2:latest"
fastai_env.python.user_managed_dependencies = True
```

Um ein Image aus einer privaten Containerregistrierung zu nutzen, die sich nicht in Ihrem Arbeitsbereich befindet, müssen Sie `docker.base_image_registry` verwenden, um die Adresse des Repositorys sowie einen Benutzernamen und ein Kennwort anzugeben:

```python
# Set the container registry information
fastai_env.docker.base_image_registry.address = "myregistry.azurecr.io"
fastai_env.docker.base_image_registry.username = "username"
fastai_env.docker.base_image_registry.password = "password"
```

Sie können auch ein benutzerdefiniertes Dockerfile verwenden. Verwenden Sie diesen Ansatz, wenn Sie Nicht-Python-Pakete als Abhängigkeiten installieren müssen, und denken Sie dabei daran, das Basisimage auf „Keines“ festzulegen.

```python 
# Specify docker steps as a string. 
dockerfile = r"""
FROM mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04
RUN echo "Hello from custom container!"
"""

# Set base image to None, because the image is defined by dockerfile.
fastai_env.docker.base_image = None
fastai_env.docker.base_dockerfile = dockerfile

# Alternatively, load the string from a file.
fastai_env.docker.base_image = None
fastai_env.docker.base_dockerfile = "./Dockerfile"
```

### <a name="create-or-attach-existing-amlcompute"></a>Erstellen oder Anfügen einer vorhandenen AmlCompute-Instanz
Sie müssen für das Training des Modells ein [Computeziel](concept-azure-machine-learning-architecture.md#compute-targets) erstellen. In diesem Tutorial erstellen Sie eine AmlCompute-Instanz als Trainingscomputeressource.

Die Erstellung von AmlCompute dauert etwa fünf Minuten. Wenn sich in Ihrem Arbeitsbereich bereits eine AmlCompute-Instanz mit diesem Namen befindet, überspringt dieser Code den Erstellungsvorgang.

Ebenso wie bei anderen Azure-Diensten gelten bei bestimmten Ressourcen (z. B. AmlCompute) in Verbindung mit Azure Machine Learning Service Grenzwerte. Lesen Sie [diesen Artikel](how-to-manage-quotas.md) zu den Standardgrenzwerten und zum Anfordern zusätzlicher Kontingente. 

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
cluster_name = "gpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target.')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6',
                                                           max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True)

# use get_status() to get a detailed status for the current AmlCompute
print(compute_target.get_status().serialize())
```

### <a name="create-a-scriptrunconfig"></a>Erstellen eines ScriptRunConfig-Elements
Mit diesem ScriptRunConfig-Element wird Ihr Auftrag zur Ausführung auf dem gewünschten [Computeziel](how-to-set-up-training-targets.md) konfiguriert.

```python
from azureml.core import ScriptRunConfig

fastai_config = ScriptRunConfig(source_directory='fastai-example', script='train.py')
fastai_config.run_config.environment = fastai_env
fastai_config.run_config.target = compute_target
```

### <a name="submit-your-run"></a>Übermitteln Ihrer Ausführung
Wenn eine Trainingsausführung mithilfe eines ScriptRunConfig-Objekts übermittelt wird, gibt die Übermittlungsmethode ein Objekt vom Typ ScriptRun zurück. Das zurückgegebene ScriptRun-Objekt ermöglicht den programmgesteuerten Zugriff auf Informationen zur Trainingsausführung. 

```python
from azureml.core import Experiment

run = Experiment(ws,'fastai-custom-image').submit(fastai_config)
run.wait_for_completion(show_output=True)
```

> [!WARNING]
> Zum Ausführen von Trainingsskripts wird von Azure Machine Learning das gesamte Quellverzeichnis kopiert. Sind vertrauliche Daten vorhanden, die nicht hochgeladen werden sollen, verwenden Sie die [IGNORE-Datei](how-to-save-write-experiment-files.md#storage-limits-of-experiment-snapshots), oder platzieren Sie sie nicht im Quellverzeichnis. Greifen Sie stattdessen mit einem [Datenspeicher](https://docs.microsoft.com/python/api/azureml-core/azureml.data?view=azure-ml-py&preserve-view=true) auf Ihre Daten zu.

Weitere Informationen zum Anpassen Ihrer Python-Umgebung finden Sie unter [Erstellen und Verwenden von Softwareumgebungen](how-to-use-environments.md). 

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie ein Modell mit einem benutzerdefinierten Docker-Image trainiert. Weitere Informationen zu Azure Machine Learning finden Sie in den folgenden Artikeln.
* [Erfassen von Ausführungsmetriken](how-to-track-experiments.md) während des Trainings
* [Bereitstellen eines Modells](how-to-deploy-custom-docker-image.md) mit einem benutzerdefinierten Docker-Image
