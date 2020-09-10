---
title: Übermitteln einer Trainingsausführung an ein Computeziel
titleSuffix: Azure Machine Learning
description: Trainieren Sie Ihr Machine Learning-Modell in verschiedenen Trainingsumgebungen (Computeziele). Sie können problemlos zwischen Trainingsumgebungen wechseln. Beginnen Sie das Training lokal. Wenn ein Aufskalieren erforderlich ist, wechseln Sie zu einem cloudbasierten Computeziel.
services: machine-learning
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.date: 08/28/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, contperfq1
ms.openlocfilehash: ca76ec5bef1d908ca3cea6ce0f58b1205c1676ca
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2020
ms.locfileid: "89144089"
---
# <a name="submit-a-training-run-to-a-compute-target"></a>Übermitteln einer Trainingsausführung an ein Computeziel

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie verschiedene Trainingsumgebungen ([Computeziele](concept-compute-target.md)) einsetzen können, um Ihr Machine Learning-Modell zu trainieren.

Beim Training ist es üblich, auf dem lokalen Computer zu starten und dieses Trainingsskript später auf einem anderen Computeziel auszuführen. Mit Azure Machine Learning können Sie Ihr Skript auf unterschiedlichen Computezielen ausführen, ohne das Trainingsskript zu ändern.

Sie müssen lediglich die Umgebung für jedes einzelne Computeziel in einer **Skriptlaufzeitkonfiguration** definieren.  Wenn Sie Ihr Trainingsexperiment auf einem anderen Computeziel ausführen möchten, geben Sie die Laufzeitkonfiguration für diese Computeressource an.

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.
* über das [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* Ein [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md), `ws`
* Ein Computeziel, `my_compute_target`  Erstellen Sie ein Computeziel mit Folgendem:
  * [Python SDK](how-to-create-attach-compute-sdk.md) 
  * [Azure Machine Learning Studio](how-to-create-attach-compute-studio.md)

## <a name="whats-a-script-run-configuration"></a><a name="whats-a-run-configuration"></a>Was ist eine Skriptlaufzeitkonfiguration?

Sie übermitteln Ihr Trainingsexperiment mit einem [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py)-Objekt.  Dieses Objekt enthält Folgendes:

* **source_directory**: Das Quellverzeichnis mit Ihrem Trainingsskript
* **script**: Bestimmen des Trainingsskripts
* **run_config**: Die [Laufzeitkonfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py), die wiederum definiert, wo das Training erfolgt. In `run_config` geben Sie das Computeziel und die Umgebung an, die beim Ausführen des Trainingsskripts verwendet werden soll.  

## <a name="whats-an-environment"></a>Was ist eine Umgebung?

Azure Machine Learning-[Umgebungen](concept-environments.md) sind eine Kapselung der Umgebung, in der Ihr Training für das maschinelle Lernen stattfindet. Sie geben die Python-Pakete, Umgebungsvariablen und Softwareeinstellungen bezüglich Ihrer Trainings- und Bewertungsskripts an. Sie geben auch Laufzeiten an (Python, Spark oder Docker).  

Umgebungen werden im `run_config`-Objekt innerhalb einer `ScriptRunConfig` angegeben.

## <a name="train-your-model"></a><a id="submit"></a>Trainieren Ihres Modells

Das Codemuster für die Übermittlung einer Trainingsausführung ist für alle Arten von Computezielen gleich:

1. Erstellen eines Experiments zum Ausführen
1. Erstellen einer Umgebung zum Ausführen des Skripts
1. Erstellen einer Skriptlaufzeitkonfiguration, die auf das Computeziel und die Umgebung verweist
1. Übermitteln der Ausführung
1. Warten, bis die Ausführung abgeschlossen ist

Alternative:

* Übermitteln Sie das Experiments mit einem `Estimator`-Objekt, wie in [Trainieren von ML-Modellen mit Kalkulatoren](how-to-train-ml-models.md) erläutert.
* Übermitteln Sie eine HyperDrive-Ausführung für die [Hyperparameteroptimierung](how-to-tune-hyperparameters.md).
* Übermitteln Sie ein Experiment über die [VS Code-Erweiterung](tutorial-train-deploy-image-classification-model-vscode.md#train-the-model).

## <a name="create-an-experiment"></a>Erstellen eines Experiments

Erstellen Sie ein Experiment in Ihrem Arbeitsbereich.

```python
from azureml.core import Experiment

experiment_name = 'my_experiment'

experiment = Experiment(workspace=ws, name=experiment_name)
```

## <a name="create-an-environment"></a>Erstellen einer Umgebung

Zusammengestellte Umgebungen enthalten Sammlungen mit Python-Paketen und sind standardmäßig in Ihrem Arbeitsbereich verfügbar. Diese Umgebungen werden durch zwischengespeicherte Docker-Images unterstützt, wodurch die Kosten für die Vorbereitung der Ausführung reduziert werden. Für ein Remotecomputeziel können Sie zunächst eine dieser beliebten zusammengestellten Umgebungen verwenden:

```python
from azureml.core import Workspace, Environment

ws = Workspace.from_config()
my_environment = Environment.get(workspace=ws, name="AzureML-Minimal")
```

Weitere Informationen und Details zu Umgebungen finden Sie unter [Erstellen und Verwenden von Softwareumgebungen in Azure Machine Learning](how-to-use-environments.md).
  
### <a name="local-compute-target"></a>Lokales Computeziel

Wenn Ihr Computeziel Ihr **lokaler Computer** ist, sind Sie dafür verantwortlich, dass alle notwendigen Pakete in der Python-Umgebung, in der das Skript ausgeführt wird, verfügbar sind.  Verwenden Sie `python.user_managed_dependencies`, um Ihre aktuelle Python-Umgebung (oder das Python in dem von Ihnen angegebenen Pfad) zu verwenden.

```python
from azureml.core import Environment

# Editing a run configuration property on-fly.
my_environment = Environment("user-managed-env")

my_environment.python.user_managed_dependencies = True

# You can choose a specific Python environment by pointing to a Python path 
#my_environment.python.interpreter_path = '/home/johndoe/miniconda3/envs/myenv/bin/python'
```

## <a name="create-script-run-configuration"></a>Erstellen einer Skriptlaufzeitkonfiguration

Da Sie jetzt über ein Computeziel (`compute_target`) und eine Umgebung (`my_environment`) verfügen, erstellen Sie eine Skriptlaufzeitkonfiguration, die Ihr Trainingsskript (`train.py`) ausführt, das sich in Ihrem `project_folder`-Verzeichnis befindet:

```python
from azureml.core import ScriptRunConfig

script_run_config = ScriptRunConfig(source_directory=project_folder, script='train.py')

# Set compute target
script_run_config.run_config.target = my_compute_target

# Set environment.   If you don't do this, a default environment will be created.
script_run_config.run_config.environment = my_environment
```

Vielleicht möchten Sie auch das Framework für Ihre Ausführung festlegen.

* Für einen HDI-Cluster:
    ```python
    src.run_config.framework = "pyspark"
    ```

* Für einen virtuellen Remotecomputer:
    ```python
    src.run_config.framework = "python"
    ```

## <a name="submit-the-experiment"></a>Übermitteln des Experiments

```python
run = experiment.submit(config=script_run_config)
```

> [!IMPORTANT]
> Wenn Sie eine Trainingsausführung übermitteln, wird eine Momentaufnahme des Verzeichnisses, das Ihre Trainingsskripts enthält, erstellt und an das Computeziel gesendet. Sie wird auch als Teil des Experiments in Ihrem Arbeitsbereich gespeichert. Wenn Sie Dateien ändern und die Ausführung erneut übermitteln, werden nur die geänderten Dateien hochgeladen.
>
> [!INCLUDE [amlinclude-info](../../includes/machine-learning-amlignore-gitignore.md)]
> 
> Weitere Informationen zu Momentaufnahmen finden Sie unter [Momentaufnahmen](concept-azure-machine-learning-architecture.md#snapshots).


<a id="gitintegration"></a>

## <a name="git-tracking-and-integration"></a>Git-Nachverfolgung und -Integration

Wenn Sie eine Trainingsausführung starten, bei der das Quellverzeichnis ein lokales Git-Repository ist, werden Informationen über das Repository im Ausführungsverlauf gespeichert. Weitere Informationen finden Sie unter [Git-Integration für Azure Machine Learning](concept-train-model-git-integration.md).

## <a name="notebook-examples"></a>Notebook-Beispiele

Beispiele für das Training mit verschiedenen Computeziele finden Sie in diesen Notebooks:
* [how-to-use-azureml/training](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [tutorials/img-classification-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/image-classification-mnist-data/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Trainieren eines Modells](tutorial-train-models-with-aml.md) verwendet ein verwaltetes Computeziel zum Trainieren eines Modells.
* Erfahren Sie, wie [Hyperparameter optimiert werden](how-to-tune-hyperparameters.md), um bessere Modelle zu erstellen.
* Erfahren Sie nach der Erstellung eines trainierten Modells, [wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md).
* Zeigen Sie die SDK-Referenz der [RunConfiguration-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) an.
* [Verwenden von Azure Machine Learning mit virtuellen Azure-Netzwerken](how-to-enable-virtual-network.md)
