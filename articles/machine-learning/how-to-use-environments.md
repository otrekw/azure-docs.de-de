---
title: Erstellen von wiederverwendbaren ML-Umgebungen
titleSuffix: Azure Machine Learning
description: Erstellen und Verwalten von Umgebungen für Modelltraining und -bereitstellung. Verwalten von Python-Paketen und anderen Einstellungen für die Umgebung.
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: cb76c7d7804a7d39e8a18c7a4cf41e9b4e0a7593
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77623638"
---
# <a name="reuse-environments-for-training-and-deployment-by-using-azure-machine-learning"></a>Wiederverwenden von Umgebungen für Training und Bereitstellung mithilfe von Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Erfahren Sie in diesem Artikel, wie Azure Machine Learning-[Umgebungen](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) erstellt und verwaltet werden. Verwenden Sie die Umgebungen, um die Softwareabhängigkeiten Ihrer Projekte in ihrem zeitlichen Ablauf nachzuverfolgen und zu reproduzieren.

Die Verwaltung von Softwareabhängigkeiten ist eine gängige Aufgabe für Entwickler. Sinnvollerweise möchten Sie sicherstellen, dass Builds ohne extensive manuelle Softwarekonfiguration reproduzierbar sind. Die Klasse `Environment` von Azure Machine Learning ist für lokale Entwicklungslösungen bestimmt, wie etwa pip und Conda, und bietet eine Lösung sowohl für lokale als auch für verteilte Cloudentwicklung.

Die Beispiele in diesem Artikel veranschaulichen Folgendes:

* Erstellen einer Umgebung und Angeben von Paketabhängigkeiten
* Abrufen und Aktualisieren von Umgebungen
* Verwenden einer Umgebung für Training
* Verwenden einer Umgebung für die Webdienstbereitstellung

Eine allgemeine Übersicht zur Funktionsweise von Umgebungen in Azure Machine Learning finden Sie unter [Was sind ML-Umgebungen?](concept-environments.md).

## <a name="prerequisites"></a>Voraussetzungen

* über das [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* Ein [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md).

## <a name="create-an-environment"></a>Erstellen einer Umgebung

In den folgenden Abschnitten werden die verschiedenen Möglichkeiten erläutert, wie Sie eine Umgebung für Ihre Experimente erstellen können.

### <a name="use-a-curated-environment"></a>Verwenden einer zusammengestellten Umgebung

Sie können zunächst eine der zusammengestellten Umgebungen auswählen: 

* Die _AzureML-Minimal_-Umgebung enthält einen minimalen Satz von Paketen, um die Nachverfolgung von Durchläufen und das Hochladen von Ressourcen zu ermöglichen. Sie können sie als Ausgangspunkt für Ihre eigene Umgebung verwenden.

* Die _AzureML-Tutorial_-Umgebung enthält allgemeine Data Science-Pakete. Zu diesen Paketen gehören Scikit-Learn, Pandas, Matplotlib und eine größere Menge an azureml-sdk-Paketen.

Zusammengestellte Umgebungen werden durch zwischengespeicherte Docker-Images unterstützt. Durch diese Unterstützung verringern sich die Kosten für die Ausführungsvorbereitung.

Verwenden Sie die `Environment.get`-Methode, um eine der zusammengestellten Umgebungen auszuwählen:

```python
from azureml.core import Workspace, Environment

ws = Workspace.from_config()
env = Environment.get(workspace=ws, name="AzureML-Minimal")
```

Sie können die zusammengestellten Umgebungen und deren Pakete mit folgendem Code auflisten:

```python
envs = Environment.list(workspace=ws)

for env in envs:
    if env.startswith("AzureML"):
        print("Name",env)
        print("packages", envs[env].python.conda_dependencies.serialize_to_string())
```

> [!WARNING]
>  Beginnen Sie den Namen Ihrer eigenen Umgebung nicht mit dem Präfix _AzureML_. Dieses Präfix ist für zusammengestellte Umgebungen reserviert.

### <a name="instantiate-an-environment-object"></a>Instanziieren eines Umgebungsobjekts

Zum manuellen Erstellen einer Umgebung importieren Sie die `Environment`-Klasse aus dem SDK. Verwenden Sie anschließend den folgenden Code, um ein Umgebungsobjekt zu instanziieren.

```python
from azureml.core.environment import Environment
Environment(name="myenv")
```

### <a name="use-conda-and-pip-specification-files"></a>Verwenden von Conda- und pip-Spezifikationsdateien

Sie können eine Umgebung auch aus einer Conda-Spezifikations- oder einer pip-Anforderungsdatei erstellen. Verwenden Sie die [`from_conda_specification()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-conda-specification-name--file-path-)-Methode oder die [`from_pip_requirements()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-pip-requirements-name--file-path-)-Methode. Schließen Sie den Namen Ihrer Umgebung und den Dateipfad der gewünschten Datei in das Methodenargument ein.

```python
# From a Conda specification file
myenv = Environment.from_conda_specification(name = "myenv",
                                             file_path = "path-to-conda-specification-file")

# From a pip requirements file
myenv = Environment.from_pip_requirements(name = "myenv"
                                          file_path = "path-to-pip-requirements-file")
```

### <a name="use-existing-conda-environments"></a>Verwenden vorhandener Conda-Umgebungen

Wenn Sie über eine vorhandene Conda-Umgebung auf Ihrem lokalen Computer verfügen, können Sie den Dienst verwenden, um ein Umgebungsobjekt zu erstellen. Mit dieser Strategie können Sie Ihre lokale interaktive Umgebung bei Remoteausführungen wiederverwenden.

Der folgende Code erstellt ein Umgebungsobjekt aus der vorhandenen Conda-Umgebung `mycondaenv`. Dabei wird die [`from_existing_conda_environment()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-existing-conda-environment-name--conda-environment-name-)-Methode verwendet.

``` python
myenv = Environment.from_existing_conda_environment(name = "myenv",
                                                    conda_environment_name = "mycondaenv")
```

### <a name="create-environments-automatically"></a>Automatisches Erstellen von Umgebungen

Erstellen Sie automatisch eine Umgebung, indem Sie einen Trainingslauf übermitteln. Übermitteln Sie den Lauf mithilfe der `submit()`-Methode. Wenn Sie einen Trainingslauf übermitteln, kann das Erstellen der neuen Umgebung einige Minuten dauern. Die Dauer der Erstellung hängt von der Größe der erforderlichen Abhängigkeiten ab. 

Wenn Sie in Ihrer Ausführungskonfiguration keine Umgebung angeben, bevor Sie den Lauf übermitteln, wird eine Standardumgebung für Sie erstellt.

```python
from azureml.core import ScriptRunConfig, Experiment, Environment
# Create experiment 
myexp = Experiment(workspace=ws, name = "environment-example")

# Attach training script and compute target to run config
runconfig = ScriptRunConfig(source_directory=".", script="example.py")
runconfig.run_config.target = "local"

# Submit the run
run = myexp.submit(config=runconfig)

# Show each step of run 
run.wait_for_completion(show_output=True)
```

In ähnlicher Weise können Sie, wenn Sie ein [`Estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py)-Objekt für das Training verwenden, die Estimator-Instanz direkt als Lauf übermitteln, ohne eine Umgebung anzugeben. Das `Estimator`-Objekt kapselt die Umgebung und das Computeziel bereits ein.

## <a name="add-packages-to-an-environment"></a>Hinzufügen von Paketen zu einer Umgebung

Sie fügen einer Umgebung Pakete mithilfe von Conda-, pip- oder Private Wheels-Dateien hinzu. Geben Sie jede Paketabhängigkeit mithilfe der [`CondaDependency`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py)-Klasse an. Fügen Sie es der `PythonSection` der Umgebung hinzu.

### <a name="conda-and-pip-packages"></a>Conda- und pip-Pakete

Wenn ein Paket in einem Conda-Paketrepository verfügbar ist, empfehlen wir, die Conda-Installation anstelle der pip-Installation zu verwenden. Conda-Pakete verfügen in der Regel über vorgefertigte Binärdateien, die die Installation zuverlässiger machen.

Das folgende Beispiel zeigt eine Hinzufügung zur Umgebung. Sie fügt Version 0.21.3 von `scikit-learn` hinzu. Außerdem wird das `pillow`-Paket `myenv` hinzugefügt. Im Beispiel wird die [`add_conda_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-conda-package-conda-package-)-Methode bzw. die [`add_pip_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-pip-package-pip-package-)-Methode verwendet.

```python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies

myenv = Environment(name="myenv")
conda_dep = CondaDependencies()

# Installs scikit-learn version 0.21.3 conda package
conda_dep.add_conda_package("scikit-learn==0.21.3")

# Adds dependencies to PythonSection of myenv
myenv.python.conda_dependencies=conda_dep
```

### <a name="private-wheel-files"></a>Private Wheel-Dateien

Sie können private pip-Wheel-Dateien verwenden, indem Sie sie zuerst in den Speicher Ihres Arbeitsbereichs hochladen. Sie können sie mithilfe einer statischen [`add_private_pip_wheel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#add-private-pip-wheel-workspace--file-path--exist-ok-false-)-Methode hochladen. Anschließend erfassen Sie die Speicher-URL und übergeben sie der `add_pip_package()`-Methode.

```python
# During environment creation the service replaces the URL by secure SAS URL, so your wheel file is kept private and secure
whl_url = Environment.add_private_pip_wheel(workspace=ws,file_path = "my-custom.whl")
myenv = Environment(name="myenv")
conda_dep = CondaDependencies()
conda_dep.add_pip_package(whl_url)
myenv.python.conda_dependencies=conda_dep
```

## <a name="manage-environments"></a>Verwalten von Umgebungen

Verwalten Sie Umgebungen, damit Sie sie über Computeziele hinweg und mit anderen Benutzern des Arbeitsbereichs zusammen aktualisieren, nachverfolgen und wiederverwenden können.

### <a name="register-environments"></a>Registrieren von Umgebungen

Die Umgebung wird automatisch bei Ihrem Arbeitsbereich registriert, wenn Sie einen Lauf übermitteln oder einen Webdienst bereitstellen. Sie können die Umgebung auch manuell mithilfe der [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#register-workspace-)-Methode registrieren. Durch diesen Vorgang wird die Umgebung zu einer Entität, die in der Cloud nachverfolgt und versionsverwaltet wird. Die Entität kann von Arbeitsbereichsbenutzern gemeinsam genutzt werden.

Mit folgendem Code wird die `myenv`-Umgebung beim `ws`-Arbeitsbereich registriert.

```python
myenv.register(workspace=ws)
```

Wenn Sie die Umgebung zum ersten Mal in Training oder Bereitstellung verwenden, wird sie beim Arbeitsbereich registriert. Anschließend wird sie auf dem Computeziel erstellt und bereitgestellt. Der Dienst speichert die Umgebungen zwischen. Die Wiederverwendung einer zwischengespeicherten Umgebung braucht viel weniger Zeit als die Verwendung eines neuen oder aktualisierten Diensts.

### <a name="get-existing-environments"></a>Abrufen vorhandener Umgebungen

Die `Environment`-Klasse bietet Methoden, mit denen Sie vorhandene Umgebungen in Ihrem Arbeitsbereich abrufen können. Sie können Umgebungen anhand des Namens, als Liste oder nach einem bestimmten Trainingslauf abrufen. Diese Informationen sind für Problembehandlung, Überwachung und Reproduzierbarkeit nützlich.

#### <a name="view-a-list-of-environments"></a>Anzeigen einer Liste der Umgebungen

Zeigen Sie die Umgebungen in Ihrem Arbeitsbereich mithilfe der [`Environment.list(workspace="workspace_name")`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#list-workspace-)-Klasse an. Wählen Sie dann eine wiederzuverwendende Umgebung aus.

#### <a name="get-an-environment-by-name"></a>Abrufen einer Umgebung anhand des Namens

Sie können auch eine bestimmte Umgebung mittels Name und Version abrufen. Der folgende Code verwendet die [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#get-workspace--name--version-none-)-Methode, um Version `1` der `myenv`-Umgebung im Arbeitsbereich `ws` abzurufen.

```python
restored_environment = Environment.get(workspace=ws,name="myenv",version="1")
```

#### <a name="train-a-run-specific-environment"></a>Trainieren einer laufspezifischen Umgebung

Um nach dem Abschluss des Trainings die Umgebung abzurufen, die für einen bestimmten Lauf verwendet wurde, verwenden Sie die [`get_environment()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-environment--)-Methode der `Run`-Klasse.

```python
from azureml.core import Run
Run.get_environment()
```

### <a name="update-an-existing-environment"></a>Aktualisieren einer vorhandenen Umgebung

Angenommen, Sie ändern eine vorhandene Umgebung, beispielsweise durch Hinzufügen eines Python-Pakets. Dann wird eine neue Version der Umgebung erstellt, wenn Sie einen Lauf übermitteln, ein Modell bereitstellen oder die Umgebung manuell registrieren. Mithilfe der Versionsverwaltung können Sie die Änderungen an der Umgebung im zeitlichen Verlauf anzeigen.

Um die Version eines Python-Pakets einer vorhandenen Umgebung zu aktualisieren, geben Sie die Versionsnummer für dieses Paket an. Wenn Sie nicht die genaue Versionsnummer angeben, führt Azure Machine Learning eine Wiederverwendung der vorhandenen Umgebung mit ihren ursprünglichen Paketversionen aus.

### <a name="debug-the-image-build"></a>Debuggen der Imageerstellung

Das folgende Beispiel verwendet die [`build()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#build-workspace-)-Methode, um eine Umgebung manuell als Docker-Image zu erstellen. Die Ausgabeprotokolle der Imageerstellung werden mithilfe von [`wait_for_completion()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-) überwacht. Das erstellte Image wird dann in der Azure Container Registry-Instanz des Arbeitsbereichs angezeigt. Diese Informationen sind beim Debuggen nützlich.

```python
from azureml.core import Image
build = env.build(workspace=ws)
build.wait_for_completion(show_output=True)
```

## <a name="enable-docker"></a>Aktivieren von Docker

 Die [`DockerSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.dockersection?view=azure-ml-py) der Azure Machine Learning-Klasse `Environment` ermöglicht es Ihnen, das Gastbetriebssystem, in dem Ihr Trainingslauf ausgeführt wird, im Detail anzupassen und zu steuern.

Wenn Sie Docker aktivieren, erstellt der Dienst ein Docker-Image. Außerdem wird eine Python-Umgebung erstellt, die Ihre Angaben innerhalb des betreffenden Docker-Containers verwendet. Diese Funktionalität bietet zusätzliche Isolation und Reproduzierbarkeit für Ihre Trainingsläufe.

```python
# Creates the environment inside a Docker container.
myenv.docker.enabled = True
```

Standardmäßig wird das neu erstellte Docker-Image in der Containerregistrierung angezeigt, die dem Arbeitsbereich zugeordnet ist.  Der Repositoryname hat das Format *azureml/azureml_\<uuid\>* . Der *UUID*-Anteil (universally unique identifier, global eindeutiger Bezeichner) des Namens entspricht einem Hash, der aus der Umgebungskonfiguration errechnet wird. Diese Entsprechung ermöglicht es dem Dienst, zu ermitteln, ob bereits ein Image der betreffenden Umgebung zur Wiederverwendung vorhanden ist.

Darüber hinaus verwendet der Dienst automatisch eines der Ubuntu Linux-basierten [Basisimages](https://github.com/Azure/AzureML-Containers). Er installiert die angegebenen Python-Pakete. Das Basisimage verfügt über CPU- und GPU-Versionen. Azure Machine Learning erkennt automatisch, welche Version verwendet werden soll.

```python
# Specify custom Docker base image and registry, if you don't want to use the defaults
myenv.docker.base_image="your_base-image"
myenv.docker.base_image_registry="your_registry_location"
# Alternatively, you can specify the contents of dockerfile of your base image
with open("docker_file_of_your_base_image", "r") as f:
    dockerfile_contents_of_your_base_image=f.read()
myenv.docker.base_dockerfile=dockerfile_contents_of_your_base_image 
```

> [!NOTE]
> Wenn Sie `environment.python.user_managed_dependencies=False` angeben, während Sie ein benutzerdefiniertes Docker-Image verwenden, erstellt der Dienst eine Conda-Umgebung innerhalb des Images. Er führt den Lauf in der betreffenden Umgebung aus, anstatt die im Basisimage ggf. installierten Python-Bibliotheken zu verwenden. Legen Sie den Parameter auf `True` fest, um Ihre eigenen installierten Pakete zu verwenden.

## <a name="use-environments-for-training"></a>Verwenden von Umgebungen für das Training

Zum Übermitteln eines Trainingslaufs müssen Sie Ihre Umgebung, das [Computeziel](concept-compute-target.md) und das Python-Trainingsskript zu einer Ausführungskonfiguration kombinieren. Diese Konfiguration ist ein Wrapper-Objekt, das zum Übermitteln von Läufen verwendet wird.

Wenn Sie einen Trainingslauf übermitteln, kann das Erstellen einer neuen Umgebung einige Minuten dauern. Die Dauer hängt von der Größe der erforderlichen Abhängigkeiten ab. Die Umgebungen werden vom Dienst zwischengespeichert. Sofern die Umgebungsdefinition unverändert bleibt, wird die gesamte Setupzeit nur einmal in Anspruch genommen.

Das folgende Beispiel einer lokalen Skriptausführung zeigt, wo Sie [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) als Wrapper-Objekt verwenden würden.

```python
from azureml.core import ScriptRunConfig, Experiment
from azureml.core.environment import Environment

exp = Experiment(name="myexp", workspace = ws)
# Instantiate environment
myenv = Environment(name="myenv")

# Add training script to run config
runconfig = ScriptRunConfig(source_directory=".", script="train.py")

# Attach compute target to run config
runconfig.run_config.target = "local"

# Attach environment to run config
runconfig.run_config.environment = myenv

# Submit run 
run = exp.submit(runconfig)
```

> [!NOTE]
> Um den Ausführungsverlauf oder Momentaufnahmen der Ausführung zu deaktivieren, verwenden Sie die Einstellung unter `ScriptRunConfig.run_config.history`.

Wenn Sie in Ihrer Ausführungskonfiguration keine Umgebung angeben, erstellt der Dienst eine Standardumgebung für Sie, wenn Sie Ihren Lauf übermitteln.

### <a name="use-an-estimator-for-training"></a>Verwenden einer Estimator-Instanz für das Training

Wenn Sie für das Training einen [Estimator](how-to-train-ml-models.md) verwenden, können Sie die Estimator-Instanz direkt übermitteln. Sie kapselt bereits die Umgebung und das Computeziel.

Der folgende Code verwendet einen Estimator für einen Trainingslauf auf einem Einzelknoten. Er wird bei einem Remotecompute für ein `scikit-learn`-Modell ausgeführt. Dabei wird davon ausgegangen, dass Sie zuvor ein Computezielobjekt `compute_target` und ein Datenspeicherobjekt `ds` erstellt haben.

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])

# Submit the run 
run = experiment.submit(sk_est)
```

## <a name="use-environments-for-web-service-deployment"></a>Verwenden von Umgebungen für die Webdienstbereitstellung

Sie können Umgebungen verwenden, wenn Sie Ihr Modell als Webdienst bereitstellen. Diese Funktion ermöglicht einen reproduzierbaren, verbundenen Workflow. In diesem Workflow können Sie das Modell trainieren, testen und bereitstellen, indem Sie die gleichen Bibliotheken sowohl für das Trainingscompute als auch für das Rückschlusscompute verwenden.

Zum Bereitstellen eines Webdiensts kombinieren Sie die Umgebung, das Rückschlusscompute, das Bewertungsskript und das registrierte Modell in Ihrem Bereitstellungsobjekt [`deploy()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-). Weitere Informationen finden Sie unter [Wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md).

Nehmen wir in diesem Beispiel an, dass Sie einen Trainingslauf abgeschlossen haben. Nun möchten Sie dieses Modell in Azure Container Instances bereitstellen. Beim Erstellen des Webdiensts werden die Modell- und Bewertungsdateien in das Image eingebunden, und der Azure Machine Learning-Rückschlussstapel wird dem Image hinzugefügt.

```python
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import AciWebservice, Webservice

# Register the model to deploy
model = run.register_model(model_name = "mymodel", model_path = "outputs/model.pkl")

# Combine scoring script & environment in Inference configuration
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)

# Set deployment configuration
deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)

# Define the model, inference, & deployment configuration and web service name and location to deploy
service = Model.deploy(
    workspace = ws,
    name = "my_web_service",
    models = [model],
    inference_config = inference_config,
    deployment_config = deployment_config)
```

## <a name="example-notebooks"></a>Beispielnotebooks

Dieses [Beispiel-Notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/using-environments) geht tiefer auf Konzepte und Methoden ein, die in diesem Artikel demonstriert wurden.

[Bereitstellen eines Modells mithilfe eines benutzerdefinierten Docker-Basisimages](how-to-deploy-custom-docker-image.md) veranschaulicht, wie ein Modell mit einem benutzerdefinierten Docker-Basisimage bereitgestellt wird.

Dieses [Beispielnotebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment/spark) zeigt, wie ein Spark-Modell als Webdienst bereitgestellt werden kann.

## <a name="create-and-manage-environments-with-the-cli"></a>Erstellen und Verwalten von Umgebungen mit der Befehlszeilenschnittstelle

Die [Azure Machine Learning-CLI](reference-azure-machine-learning-cli.md) spiegelt den größten Teil der Funktionalität des Python SDK. Sie können sie zum Erstellen und Verwalten von Umgebungen verwenden. Die in diesem Abschnitt erörterten Befehle veranschaulichen die grundlegende Funktionalität.

Das folgende Befehlsgerüst stellt die Dateien für eine standardmäßige Umgebungsdefinition im angegebenen Verzeichnis dar. Diese Dateien sind JSON-Dateien. Sie funktionieren wie die entsprechende Klasse im SDK. Sie können die Dateien verwenden, um neue Umgebungen zu erstellen, die benutzerdefinierte Einstellungen aufweisen. 

```azurecli-interactive
az ml environment scaffold -n myenv -d myenvdir
```

Führen Sie den folgenden Befehl aus, um eine Umgebung aus einem bestimmten Verzeichnis zu registrieren.

```azurecli-interactive
az ml environment register -d myenvdir
```

Führen Sie den folgenden Befehl aus, um alle registrierten Umgebungen aufzulisten.

```azurecli-interactive
az ml environment list
```

Laden Sie mit dem folgenden Befehl eine registrierte Umgebung herunter.

```azurecli-interactive
az ml environment download -n myenv -d downloaddir
```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Verwenden eines verwalteten Computeziels zum Trainieren eines Modells finden Sie unter [Tutorial: Trainieren eines Modells](tutorial-train-models-with-aml.md).
* Erfahren Sie nach dem Erstellen eines trainierten Modells, [wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md).
* Sehen Sie sich die [SDK-Referenz der `Environment`-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py) an.
* Weitere Informationen zu den in diesem Artikel beschriebenen Konzepten und Methoden finden Sie im [Beispielnotebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/using-environments).
