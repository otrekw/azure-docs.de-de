---
title: Trainieren von Scikit-learn-Machine Learning-Modellen
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie Scikit-learn-Trainingsskripts in Azure Machine Learning ausführen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: jordane
author: jpe316
ms.date: 07/24/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: 7b63ef36d7df43168ed132a740bab026e6e00f3f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90897236"
---
# <a name="build-scikit-learn-models-at-scale-with-azure-machine-learning"></a>Erstellen von Scikit-learn-Modellen nach Maß mit Azure Machine Learning


In diesem Artikel erfahren Sie, wie Sie Scikit-learn-Trainingsskripts mit Azure Machine Learning ausführen.

Mit den Beispielskripts in diesem Artikel werden Irisbilder zum Erstellen eines Machine Learning-Modells auf der Grundlage des [Iris-Datasets](https://archive.ics.uci.edu/ml/datasets/iris) von Scikit-learn klassifiziert.

Unabhängig davon, ob Sie ein Scikit-learn-Machine Learning-Modell von Grund auf trainieren oder ob Sie ein vorhandenes Modell in die Cloud verschieben, können Sie Azure Machine Learning zum Aufskalieren von Open-Source-Trainingsaufträgen mithilfe elastischer Cloudcomputeressourcen verwenden. Sie können produktionsgeeignete Modelle mit Azure Machine Learning erstellen, bereitstellen, überwachen sowie die Versionen verwalten.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie diesen Code in einer dieser Umgebungen aus:
 - Azure Machine Learning-Compute-Instanz: keine Downloads oder Installationen erforderlich

    - Absolvieren Sie [Tutorial: Einrichten von Umgebung und Arbeitsbereich](tutorial-1st-experiment-sdk-setup.md), um einen dedizierten Notebook-Server zu erstellen, auf dem das SDK und Beispielrepository vorinstalliert sind.
    - Im Beispieltrainingsordner auf dem Notebookserver finden Sie im folgenden Verzeichnis ein fertiges und erweitertes Notebook: **how-to-use-azureml > ml-frameworks > scikit-learn > training > train-hyperparameter-tune-deploy-with-sklearn**.

 - Ihr eigener Jupyter Notebook-Server

    - [Installieren Sie das Azure Machine Learning SDK.](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true)
    - [Erstellen Sie eine Konfigurationsdatei für den Arbeitsbereich.](how-to-configure-environment.md#workspace)

## <a name="set-up-the-experiment"></a>Einrichten des Experiments

In diesem Abschnitt wird das Trainingsexperiment eingerichtet, indem die erforderlichen Python-Pakete geladen, ein Arbeitsbereich initialisiert, ein Experiment erstellt und die Trainingsdaten und -skripts hochgeladen werden.

### <a name="initialize-a-workspace"></a>Initialisieren eines Arbeitsbereichs

Der [Azure Machine Learning-Arbeitsbereich](concept-workspace.md) ist die Ressource der obersten Ebene für den Dienst. Er stellt den zentralen Ort für die Arbeit mit allen erstellten Artefakten dar. Im Python SDK können Sie auf die Arbeitsbereichsartefakte zugreifen, indem Sie ein [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py&preserve-view=true)-Objekt erstellen.

Erstellen Sie ein Arbeitsbereichsobjekt aus der Datei `config.json`, die im [Abschnitt „Voraussetzungen“](#prerequisites) erstellt wurde.

```Python
from azureml.core import Workspace

ws = Workspace.from_config()
```


### <a name="prepare-scripts"></a>Vorbereiten von Skripts

In diesem Tutorial ist das Trainingsskript **train_iris.py** bereits [hier](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/scikit-learn/training/train-hyperparameter-tune-deploy-with-sklearn/train_iris.py) bereitgestellt. In der Praxis sollten Sie benutzerdefinierte Trainingsskripts unverändert übernehmen und mit Azure ML ausführen können, ohne Ihren Code ändern zu müssen.

Hinweise:
- Das bereitgestellte Trainingsskript zeigt, wie Sie mit dem `Run`-Objekt innerhalb des Skripts einige Metriken zu Ihrer Azure ML-Ausführung protokollieren können.
- Das bereitgestellte Trainingsskript verwendet Beispieldaten aus der `iris = datasets.load_iris()`-Funktion.  Für Ihre eigenen Daten müssen Sie unter Umständen einige Schritte (etwa [Hochladen von Dataset und Skripts](how-to-train-keras.md#data-upload)) ausführen, um Daten während des Trainings verfügbar zu machen.

### <a name="define-your-environment"></a>Definieren der Umgebung

#### <a name="create-a-custom-environment"></a>Erstellen einer benutzerdefinierten Umgebung

Erstellen Sie Ihre Conda-Umgebung (sklearn-env.yml).
Wenn Sie die Conda-Umgebung über ein Notebook schreiben möchten, können Sie die Zeile ```%%writefile sklearn-env.yml``` oben in der Zelle hinzufügen.

```yaml
name: sklearn-training-env
dependencies:
  - python=3.6.2
  - scikit-learn
  - numpy
  - pip:
    - azureml-defaults
```

Erstellen Sie anhand dieser Spezifikation der Conda-Umgebung eine Azure ML-Umgebung. Die Umgebung wird zur Laufzeit in einen Docker-Container gepackt.
```python
from azureml.core import Environment

myenv = Environment.from_conda_specification(name = "myenv", file_path = "sklearn-env.yml")
myenv.docker.enabled = True
```

#### <a name="use-a-curated-environment"></a>Verwenden einer zusammengestellten Umgebung
Azure ML bietet vorgefertigte, zusammengestellte Containerumgebungen, falls Sie kein eigenes Image erstellen möchten. Weitere Informationen finden Sie [hier](resource-curated-environments.md).
Wenn Sie eine zusammengestellte Umgebung verwenden möchten, können Sie stattdessen den folgenden Befehl ausführen:

```python
env = Environment.get(workspace=ws, name="AzureML-Tutorial")
```

### <a name="create-a-scriptrunconfig"></a>Erstellen eines ScriptRunConfig-Elements

Mit diesem ScriptRunConfig-Element wird Ihr Auftrag zur Ausführung auf dem lokalen Computeziel übermittelt.

```python
from azureml.core import ScriptRunConfig

sklearnconfig = ScriptRunConfig(source_directory='.', script='train_iris.py')
sklearnconfig.run_config.environment = myenv
```

Bei der Übermittlung für einen Remotecluster können Sie „run_config.target“ in das gewünschte Computeziel ändern.

### <a name="submit-your-run"></a>Übermitteln Ihrer Ausführung
```python
from azureml.core import Experiment

run = Experiment(ws,'train-sklearn').submit(config=sklearnconfig)
run.wait_for_completion(show_output=True)

```

> [!WARNING]
> Zum Ausführen von Trainingsskripts wird von Azure Machine Learning das gesamte Quellverzeichnis kopiert. Sind vertrauliche Daten vorhanden, die nicht hochgeladen werden sollen, verwenden Sie die [IGNORE-Datei](how-to-save-write-experiment-files.md#storage-limits-of-experiment-snapshots), oder platzieren Sie sie nicht im Quellverzeichnis. Greifen Sie stattdessen mit einem [Datenspeicher](https://docs.microsoft.com/python/api/azureml-core/azureml.data?view=azure-ml-py&preserve-view=true) auf Ihre Daten zu.

Weitere Informationen zur Anpassung Ihrer Python-Umgebung finden Sie unter [Erstellen und Verwalten von Umgebungen für Training und Bereitstellung](how-to-use-environments.md). 

## <a name="what-happens-during-run-execution"></a>Was passiert während der Ausführung
Die Ausführung durchläuft die folgenden Phasen:

- **Vorbereitung**: Gemäß dem TensorFlow-Estimator wird ein Docker-Image erstellt. Das Image wird in die Containerregistrierung des Arbeitsbereichs hochgeladen und für spätere Ausführungen zwischengespeichert. Darüber hinaus werden Protokolle in den Ausführungsverlauf gestreamt, mit deren Hilfe der Status überwacht werden kann.

- **Skalierung**: Der Cluster versucht ein Hochskalieren, wenn der Batch KI-Cluster mehr Knoten zur Ausführung benötigt, als derzeit verfügbar sind.

- **Wird ausgeführt:** Alle Skripts im Skriptordner werden auf das Computeziel hochgeladen, Datenspeicher werden bereitgestellt oder kopiert, und das „entry_script“ wird ausgeführt. Ausgaben aus „stdout“ und dem Ordner „./logs“ werden in den Ausführungsverlauf gestreamt und können zur Überwachung der Ausführung verwendet werden.

- **Nachbearbeitung**: Der Ordner „./outputs“ der Ausführung wird in den Ausführungsverlauf kopiert.

## <a name="save-and-register-the-model"></a>Speichern und Registrieren des Modells

Sobald Sie das Modell trainiert haben, können Sie es in Ihrem Arbeitsbereich speichern und registrieren. Die Modellregistrierung bietet die Möglichkeit, Ihre Modelle in Ihrem Arbeitsbereich zu speichern und zu versionieren, um die [Modellverwaltung und -bereitstellung](concept-model-management-and-deployment.md) zu vereinfachen.

Fügen Sie Ihrem Trainingsskript train_iris.py den folgenden Code hinzu, um das Modell zu speichern. 

``` Python
import joblib

joblib.dump(svm_model_linear, 'model.joblib')
```

Registrieren Sie das Modell mit dem folgenden Code in Ihrem Arbeitsbereich. Durch die Angabe der Parameter `model_framework`, `model_framework_version` und `resource_configuration` wird die Implementierung von Modellen ohne Code verfügbar. Mit der Modellimplementierung ohne Code können Sie Ihr Modell direkt als Webdienst aus dem registrierten Modell bereitstellen, und das Objekt [`ResourceConfiguration`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.resource_configuration.resourceconfiguration?view=azure-ml-py&preserve-view=true) definiert die Computeressource für den Webdienst.

```Python
from azureml.core import Model
from azureml.core.resource_configuration import ResourceConfiguration

model = run.register_model(model_name='sklearn-iris', 
                           model_path='outputs/model.joblib',
                           model_framework=Model.Framework.SCIKITLEARN,
                           model_framework_version='0.19.1',
                           resource_configuration=ResourceConfiguration(cpu=1, memory_in_gb=0.5))
```

## <a name="deployment"></a>Bereitstellung

Das soeben registrierte Modell kann genauso eingesetzt werden wie jedes andere registrierte Modell in Azure Machine Learning, unabhängig davon, welchen Estimator Sie für das Training verwendet haben. Die Schrittanleitung zur Bereitstellung enthält einen Abschnitt zur Registrierung von Modellen, aber Sie können direkt zu [Erstellen eines Computeziels](how-to-deploy-and-where.md#choose-a-compute-target) für die Bereitstellung springen, da Sie bereits über ein registriertes Modell verfügen.

### <a name="preview-no-code-model-deployment"></a>(Vorschau) Modellimplementierung ohne Code

Anstelle der traditionellen Bereitstellungsroute können Sie auch die Bereitstellung ohne Code (Vorschau) für SciKit-learn verwenden. Die Modellimplementierung ohne Code wird für alle integrierten Scikit-learn-Modelltypen unterstützt. Indem Sie Ihr Modell wie oben gezeigt mit den Parametern `model_framework`, `model_framework_version` und `resource_configuration` registrieren, können Sie einfach die statische Funktion [`deploy()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py#&preserve-view=truedeploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) verwenden, um Ihr Modell bereitzustellen.

```python
web_service = Model.deploy(ws, "scikit-learn-service", [model])
```

HINWEIS:  Diese Abhängigkeiten sind im vordefinieren Scikit-learn-Container für Rückschlüsse enthalten.

```yaml
    - azureml-defaults
    - inference-schema[numpy-support]
    - scikit-learn
    - numpy
```

Die vollständige [Schrittanleitung](how-to-deploy-and-where.md) behandelt die Bereitstellung in Azure Machine Learning eingehender.


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie ein sciKit-learn-Modell trainiert und registriert und sich über Bereitstellungsoptionen informiert. Weitere Informationen zu Azure Machine Learning finden Sie in den folgenden Artikeln.

* [Erfassen einer Ausführungsmetrik während des Trainings](how-to-track-experiments.md)
* [Optimieren von Hyperparametern](how-to-tune-hyperparameters.md)
* [Verteiltes Trainieren von Deep Learning-Modellen in Azure](/azure/architecture/reference-architectures/ai/training-deep-learning)
