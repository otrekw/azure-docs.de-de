---
title: Wie und wo Modelle bereitgestellt werden
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie und wo Sie Ihre Azure Machine Learning-Modelle bereitstellen können, einschließlich Azure Container Instances, Azure Kubernetes Service, Azure IoT Edge und Field Programmable Gate Arrays.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: gopalv
author: gvashishtha
ms.reviewer: larryfr
ms.date: 07/08/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: 67a66ae94508a8d7f54d6112de95fa65a8fd5f09
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88185416"
---
# <a name="deploy-models-with-azure-machine-learning"></a>Bereitstellen von Modellen mit Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Erfahren Sie, wie Sie Ihr Machine Learning-Modell als Webdienst in der Azure-Cloud oder auf Azure IoT Edge-Geräten bereitstellen.

Der Workflow ist unabhängig vom Bereitstellungsort Ihres Modells sehr ähnlich:

1. Registrieren des Modells.
1. Vorbereiten einer Rückschlusskonfiguration
1. Vorbereiten eines Eingabeskripts (es sei denn, es wird eine [Bereitstellung ohne Code verwendet](how-to-deploy-no-code-deployment.md))
1. Bereitstellen des Modells auf dem Computeziel.
1. Testen des bereitgestellten Modells (das auch als „Webdienst“ bezeichnet wird).

Weitere Informationen zu den am Bereitstellungsworkflow beteiligten Konzepten finden Sie unter [Verwalten, Bereitstellen und Überwachen von Modellen mit Azure Machine Learning](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Voraussetzungen

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azcli)

- Ein Azure Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).
- Ein Modell. Wenn Sie über kein trainiertes Modell verfügen, können Sie die Modell- und Abhängigkeitsdateien verwenden, die in [diesem Tutorial](https://aka.ms/azml-deploy-cloud) bereitgestellt werden.
- Die [Azure CLI-Erweiterung für den Machine Learning Service](reference-azure-machine-learning-cli.md).

# <a name="python"></a>[Python](#tab/python)

- Ein Azure Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).
- Ein Modell. Wenn Sie über kein trainiertes Modell verfügen, können Sie die Modell- und Abhängigkeitsdateien verwenden, die in [diesem Tutorial](https://aka.ms/azml-deploy-cloud) bereitgestellt werden.
- Das [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

---

## <a name="connect-to-your-workspace"></a>Herstellen einer Verbindung mit Ihrem Arbeitsbereich

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azcli)

Befolgen Sie die Anweisungen in der Azure CLI-Dokumentation zum [Festlegen Ihres Abonnementkontexts](/cli/azure/manage-azure-subscriptions-azure-cli#change-the-active-subscription).

Führen Sie dann Folgendes aus:

```azurecli-interactive
az ml workspace list --resource-group=<my resource group>
```

um die Arbeitsbereiche anzuzeigen, auf die Sie Zugriff haben.

# <a name="python"></a>[Python](#tab/python)

```python
from azureml.core import Workspace
ws = Workspace.from_config(path=".file-path/ws_config.json")
```

Weitere Informationen zur Verwendung des SDKs, um eine Verbindung mit einem Arbeitsbereich herzustellen, finden Sie in der Dokumentation zum [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py#workspace).


---


## <a name="register-your-model"></a><a id="registermodel"></a> Registrieren Ihres Modells

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azcli)

Ein registriertes Modell ist ein logischer Container für eine oder mehrere Dateien, aus denen Ihr Modell besteht. Wenn Sie beispielsweise ein Modell verwenden, das in mehreren Dateien gespeichert ist, können Sie diese als einzelnes Modell im Arbeitsbereich registrieren. Nachdem Sie die Dateien registriert haben, können Sie das registrierte Modell herunterladen oder bereitstellen und alle Dateien empfangen, die Sie registriert haben.

> [!TIP]
> Wenn Sie ein Modell registrieren, geben Sie den Pfad eines Cloudspeicherorts (aus einem Trainingslauf) oder eines lokalen Verzeichnisses an. Dieser Pfad wird lediglich dazu verwendet, die Dateien während des Registrierungsprozesses für das Hochladen zu finden. Er muss nicht mit dem Pfad identisch sein, der im Eingabeskript verwendet wird. Weitere Informationen finden Sie unter [Suchen nach Modelldateien im Eingabeskript](how-to-deploy-advanced-entry-script.md#load-registered-models).

Machine Learning-Modelle werden in Ihrem Azure Machine Learning-Arbeitsbereich registriert. Das Modell kann aus Azure Machine Learning oder aus einer anderen Quelle stammen. Wenn Sie ein Modell registrieren, können Sie optional Metadaten zum Modell bereitstellen. Die Wörterbücher `tags` und `properties`, die Sie auf eine Modellregistrierung anwenden, können dann zum Filtern von Modellen verwendet werden.

Die folgenden Beispiele veranschaulichen das Registrieren eines Modells.

### <a name="register-a-model-from-an-azure-ml-training-run"></a>Registrieren eines Modells aus einer Azure ML-Trainingsausführung

```azurecli-interactive
az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --run-id myrunid --tag area=mnist
```

[!INCLUDE [install extension](../../includes/machine-learning-service-install-extension.md)]

Der `--asset-path`-Parameter verweist auf den Cloudspeicherort des Modells. In diesem Beispiel wird der Pfad einer einzelnen Datei verwendet. Um mehrere Dateien in die Modellregistrierung aufzunehmen, legen Sie `--asset-path` auf den Pfad eines Ordners fest, der die Dateien enthält.

### <a name="register-a-model-from-a-local-file"></a>Registrieren eines Modells aus einer lokalen Datei

```azurecli-interactive
az ml model register -n onnx_mnist -p mnist/model.onnx
```

Um mehrere Dateien in die Modellregistrierung aufzunehmen, legen Sie `-p` auf den Pfad eines Ordners fest, der die Dateien enthält.

Weitere Informationen zu `az ml model register` finden Sie in der [Referenzdokumentation](/cli/azure/ext/azure-cli-ml/ml/model).

# <a name="python"></a>[Python](#tab/python)


Ein registriertes Modell ist ein logischer Container für eine oder mehrere Dateien, aus denen Ihr Modell besteht. Wenn Sie beispielsweise ein Modell verwenden, das in mehreren Dateien gespeichert ist, können Sie diese als einzelnes Modell im Arbeitsbereich registrieren. Nachdem Sie die Dateien registriert haben, können Sie das registrierte Modell herunterladen oder bereitstellen und alle Dateien empfangen, die Sie registriert haben.

> [!TIP]
> Wenn Sie ein Modell registrieren, geben Sie den Pfad eines Cloudspeicherorts (aus einem Trainingslauf) oder eines lokalen Verzeichnisses an. Dieser Pfad wird lediglich dazu verwendet, die Dateien während des Registrierungsprozesses für das Hochladen zu finden. Er muss nicht mit dem Pfad identisch sein, der im Eingabeskript verwendet wird. Weitere Informationen finden Sie unter [Suchen nach Modelldateien im Eingabeskript](how-to-deploy-advanced-entry-script.md#load-registered-models).

Machine Learning-Modelle werden in Ihrem Azure Machine Learning-Arbeitsbereich registriert. Das Modell kann aus Azure Machine Learning oder aus einer anderen Quelle stammen. Wenn Sie ein Modell registrieren, können Sie optional Metadaten zum Modell bereitstellen. Die Wörterbücher `tags` und `properties`, die Sie auf eine Modellregistrierung anwenden, können dann zum Filtern von Modellen verwendet werden.

Die folgenden Beispiele veranschaulichen das Registrieren eines Modells.

### <a name="register-a-model-from-an-azure-ml-training-run"></a>Registrieren eines Modells aus einer Azure ML-Trainingsausführung

  Wenn Sie das SDK verwenden, um ein Modell zu trainieren, können Sie entweder ein [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py)-Objekt oder ein [AutoMLRun](/python/api/azureml-train-automl-client/azureml.train.automl.run.automlrun)-Objekt empfangen. Dies hängt davon ab, wie Sie das Modell trainiert haben. Jedes Objekt kann verwendet werden, um ein Modell zu registrieren, das von einer Experimentausführung erstellt wurde.

  + Registrieren Sie ein Modell über ein `azureml.core.Run`-Objekt:
 
    ```python
    model = run.register_model(model_name='sklearn_mnist',
                               tags={'area': 'mnist'},
                               model_path='outputs/sklearn_mnist_model.pkl')
    print(model.name, model.id, model.version, sep='\t')
    ```

    Der `model_path`-Parameter verweist auf den Cloudspeicherort des Modells. In diesem Beispiel wird der Pfad einer einzelnen Datei verwendet. Um mehrere Dateien in die Modellregistrierung aufzunehmen, legen Sie `model_path` auf den Pfad eines Ordners fest, der die Dateien enthält. Weitere Informationen finden Sie in der Dokumentation zum [Run.register-Modell](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#register-model-model-name--model-path-none--tags-none--properties-none--model-framework-none--model-framework-version-none--description-none--datasets-none--sample-input-dataset-none--sample-output-dataset-none--resource-configuration-none----kwargs-).

  + Registrieren Sie ein Modell über ein `azureml.train.automl.run.AutoMLRun`-Objekt:

    ```python
        description = 'My AutoML Model'
        model = run.register_model(description = description,
                                   tags={'area': 'mnist'})

        print(run.model_id)
    ```

    In diesem Beispiel sind die Parameter `metric` und `iteration` nicht angegeben. Daher wird die Iteration mit der besten primären Metrik registriert. Der von der Ausführung zurückgegebene `model_id`-Wert wird anstelle eines Modellnamens verwendet.

    Weitere Informationen finden Sie in der Dokumentation zu [AutoMLRun.register-Modell](/python/api/azureml-train-automl-client/azureml.train.automl.run.automlrun#register-model-model-name-none--description-none--tags-none--iteration-none--metric-none-).


### <a name="register-a-model-from-a-local-file"></a>Registrieren eines Modells aus einer lokalen Datei

Sie können ein Modell registrieren, indem Sie den lokalen Pfad des Modells bereitstellen. Sie können den Pfad eines Ordners oder einer einzelnen Datei angeben. Mit dieser Methode können Sie Modelle registrieren, die mit dem Azure Machine Learning trainiert und dann heruntergeladen wurden. Mit dieser Methode können Sie auch Modelle registrieren, die außerhalb von Azure Machine Learning trainiert wurden.

[!INCLUDE [trusted models](../../includes/machine-learning-service-trusted-model.md)]

+ **Verwenden des SDK und von ONNX**

    ```python
    import os
    import urllib.request
    from azureml.core.model import Model
    # Download model
    onnx_model_url = "https://www.cntk.ai/OnnxModels/mnist/opset_7/mnist.tar.gz"
    urllib.request.urlretrieve(onnx_model_url, filename="mnist.tar.gz")
    os.system('tar xvzf mnist.tar.gz')
    # Register model
    model = Model.register(workspace = ws,
                            model_path ="mnist/model.onnx",
                            model_name = "onnx_mnist",
                            tags = {"onnx": "demo"},
                            description = "MNIST image classification CNN from ONNX Model Zoo",)
    ```

  Um mehrere Dateien in die Modellregistrierung aufzunehmen, legen Sie `model_path` auf den Pfad eines Ordners fest, der die Dateien enthält.

Weitere Informationen finden Sie in der Dokumentation zur [Model-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

Weitere Informationen zur Arbeit mit Modellen, die außerhalb des Azure Machine Learning trainiert wurden, finden Sie unter [Verwenden eines vorhandenen Modells mit Azure Machine Learning](how-to-deploy-existing-model.md).

---

## <a name="define-an-entry-script"></a>Definieren eines Einstiegsskripts

[!INCLUDE [write entry script](../../includes/machine-learning-entry-script.md)]


## <a name="define-an-inference-configuration"></a>Definieren einer Rückschlusskonfiguration

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azcli)

[!INCLUDE [inference config](../../includes/machine-learning-service-inference-config.md)]

Der folgende Befehl veranschaulicht, wie Sie ein Modell mithilfe über die Befehlszeilenschnittstelle (Call-Level-Interface, CLI) bereitstellen:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

In diesem Beispiel gibt die Konfiguration die folgenden Einstellungen an:

* Die Angabe, dass für das Modell Python erforderlich ist
* Das [Eingangsskript](#define-an-entry-script), das zum Verarbeiten von an den bereitgestellten Dienst gesendeten Webanforderungen verwendet wird.
* Die Conda-Datei, in der die Python-Pakete beschrieben sind, die für Rückschlüsse erforderlich sind

Informationen zur Verwendung eines benutzerdefinierten Docker-Images mit einer Rückschlusskonfiguration finden Sie unter [Bereitstellen eines Modells mithilfe eines benutzerdefinierten Docker-Basisimages](how-to-deploy-custom-docker-image.md).

# <a name="python"></a>[Python](#tab/python)

Eine Rückschlusskonfiguration beschreibt, wie der Webdienst, der Ihr Modell enthält, eingerichtet werden kann. Sie wird später verwendet, wenn Sie das Modell bereitstellen.

Bei der Rückschlusskonfiguration werden Azure Machine Learning-Umgebungen verwendet, um die Softwareabhängigkeiten zu definieren, die für Ihre Bereitstellung benötigt werden. Mit Umgebungen können Sie die Softwareabhängigkeiten, die für das Training und die Bereitstellung erforderlich sind, erstellen, verwalten und wiederverwenden. Sie können eine Umgebung aus benutzerdefinierten Abhängigkeitsdateien erstellen oder eine der zusammengestellten Azure Machine Learning-Umgebungen verwenden. Das folgende YAML-Beispiel zeigt eine Conda-Abhängigkeitsdatei für den Rückschluss. Beachten Sie, dass Sie „azureml-defaults“ mit einer Version größer oder gleich 1.0.45 als pip-Abhängigkeit angeben müssen, da sie die Funktionalität enthält, die zum Hosten des Modells als Webdienst erforderlich ist. Wenn Sie automatische Schemagenerierung verwenden möchten, muss Ihr Eingangsskript auch die `inference-schema`-Pakete importieren.

```YAML

name: project_environment
dependencies:
    - python=3.6.2
    - scikit-learn=0.20.0
    - pip:
        # You must list azureml-defaults as a pip dependency
    - azureml-defaults>=1.0.45
    - inference-schema[numpy-support]
```

> [!IMPORTANT]
> Wenn die Abhängigkeit sowohl über Conda als auch über pip (von PyPi) verfügbar ist, empfiehlt Microsoft die Verwendung der Conda-Version, da Conda-Pakete in der Regel mit vorgefertigten Binärdateien geliefert werden, welche die Zuverlässigkeit der Installation erhöhen.
>
> Weitere Informationen finden Sie unter [Grundlegendes zu Conda und pip](https://www.anaconda.com/understanding-conda-and-pip/).
>
> Überprüfen Sie mithilfe des `conda search <package-name>`-Befehls, ob ihre Abhängigkeit über Conda verfügbar ist, oder verwenden Sie die Paketindizes unter [https://anaconda.org/anaconda/repo](https://anaconda.org/anaconda/repo) und [https://anaconda.org/conda-forge/repo](https://anaconda.org/conda-forge/repo).

Sie können die Abhängigkeitsdatei verwenden, um ein Umgebungsobjekt zu erstellen und es zur späteren Verwendung in Ihrem Arbeitsbereich zu speichern:

```python
from azureml.core.environment import Environment
myenv = Environment.from_conda_specification(name = 'myenv',
                                                file_path = 'path-to-conda-specification-file'
myenv.register(workspace=ws)
```

Ausführliche Informationen zum Verwenden und Anpassen von Python-Umgebungen mit Azure Machine Learning finden Sie unter [Erstellen und Verwenden von Softwareumgebungen in Azure Machine Learning](how-to-use-environments.md).

Informationen zur Verwendung eines benutzerdefinierten Docker-Images mit einer Rückschlusskonfiguration finden Sie unter [Bereitstellen eines Modells mithilfe eines benutzerdefinierten Docker-Basisimages](how-to-deploy-custom-docker-image.md).


Im folgenden Beispiel wird das Laden einer Umgebung aus Ihrem Arbeitsbereich und die anschließende Verwendung mit der Rückschlusskonfiguration veranschaulicht:

```python
from azureml.core.environment import Environment
from azureml.core.model import InferenceConfig


myenv = Environment.get(workspace=ws, name='myenv', version='1')
inference_config = InferenceConfig(entry_script='path-to-score.py',
                                    environment=myenv)
```

Weitere Informationen zu Umgebungen finden Sie unter [Erstellen und Verwalten von Umgebungen für Training und Bereitstellung](how-to-use-environments.md).

Weitere Informationen zur Rückschlusskonfiguration finden Sie in der Dokumentation der [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)-Klasse.

---

## <a name="choose-a-compute-target"></a>Auswählen eines Computeziels

[!INCLUDE [aml-compute-target-deploy](../../includes/aml-compute-target-deploy.md)]

## <a name="define-a-deployment-configuration"></a>Definieren einer Bereitstellungskonfiguration

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azcli)

Die für eine Bereitstellungskonfiguration verfügbaren Optionen unterscheiden sich je nach dem von Ihnen ausgewählten Computeziel.

[!INCLUDE [aml-local-deploy-config](../../includes/machine-learning-service-local-deploy-config.md)]

Weitere Informationen finden Sie in der Dokumentation zu [az ml model deploy](/cli/azure/ext/azure-cli-ml/ml/model#ext-azure-cli-ml-az-ml-model-deploy).

# <a name="python"></a>[Python](#tab/python)

Bevor Sie Ihr Modell bereitstellen, müssen Sie die Bereitstellungskonfiguration definieren. *Die Bereitstellungskonfiguration ist spezifisch für das Computeziel, das den Webdienst hosten wird.* Beispielsweise müssen Sie, wenn Sie ein Modell lokal bereitstellen, den Port angeben, an dem der Dienst Anforderungen akzeptiert. Die Bereitstellungskonfiguration ist kein Bestandteil Ihres Eingabeskripts. Sie wird verwendet, um die Merkmale des Computeziels zu definieren, von dem das Modell und das Eingabeskript gehostet werden.

Möglicherweise müssen Sie auch die Computeressource erstellen. Dies ist beispielsweise der Fall, wenn Sie noch keine AKS-Instanz (Azure Kubernetes Service) haben, die mit Ihrem Arbeitsbereich verknüpft ist.

Die folgende Tabelle enthält ein Beispiel für das Erstellen einer Bereitstellungskonfiguration für jedes Computeziel:

| Computeziel | Beispiel für eine Bereitstellungskonfiguration |
| ----- | ----- |
| Lokal | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Instances | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Azure Kubernetes Service | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

Die Klassen für lokale, Azure Container Instances- und AKS-Webdienste können aus `azureml.core.webservice` importiert werden:

```python
from azureml.core.webservice import AciWebservice, AksWebservice, LocalWebservice
```

---



## <a name="deploy-your-model"></a>Bereitstellen Ihres Modells

Sie können Ihr Modell jetzt bereitstellen. 

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azcli)

### <a name="using-a-registered-model"></a>Verwenden eines registrierten Modells

Wenn Sie Ihr Modell im Azure Machine Learning-Arbeitsbereich registriert haben, ersetzen Sie „mymodel:1“ durch den Namen Ihres Modells und seine Versionsnummer.

```azurecli-interactive
az ml model deploy -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json
```

### <a name="using-a-local-model"></a>Verwenden eines lokalen Modells

Wenn Sie Ihr Modell lieber nicht registrieren möchten, können Sie den Parameter „sourceDirectory“ in Ihrer Datei „inferenceconfig.json“ übergeben, um ein lokales Verzeichnis anzugeben, aus dem Ihr Modell bereitgestellt werden soll.

```azurecli-interactive
az ml model deploy --ic inferenceconfig.json --dc deploymentconfig.json
```

# <a name="python"></a>[Python](#tab/python)

Im folgenden Beispiel wird eine lokale Bereitstellung veranschaulicht. Die Syntax variiert in Abhängigkeit von dem Computeziel, das Sie im vorherigen Schritt ausgewählt haben.

```python
from azureml.core.webservice import LocalWebservice, Webservice

deployment_config = LocalWebservice.deploy_configuration(port=8890)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Weitere Informationen finden Sie in der Dokumentation zu [LocalWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py), [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) und [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py).

---

### <a name="understanding-service-state"></a>Grundlegendes zum Dienstzustand

Während der Modellimplementierung kann es vorkommen, dass sich der Dienstzustand während der vollständigen Bereitstellung ändert.

In der folgenden Tabelle werden die verschiedenen Dienstzustände beschrieben:

| Webservice-Zustand | BESCHREIBUNG | Endgültiger Zustand?
| ----- | ----- | ----- |
| Im Übergang | Der Dienst wird gerade bereitgestellt. | Nein |
| Fehlerhaft | Der Dienst wurde bereitgestellt, ist aber zurzeit nicht erreichbar.  | Nein |
| Nicht planbar | Der Dienst kann derzeit aufgrund fehlender Ressourcen nicht bereitgestellt werden. | Nein |
| Fehler | Der Dienst konnte aufgrund eines Fehlers oder Absturzes nicht bereitgestellt werden. | Ja |
| Healthy | Der Dienst ist fehlerfrei und der Endpunkt ist verfügbar. | Ja |


### <a name="batch-inference"></a><a id="azuremlcompute"></a> Batchrückschluss
Azure Machine Learning Compute-Ziele werden von Azure Machine Learning erstellt und verwaltet. Sie können für Batchvorhersagen über Azure Machine Learning-Pipelines verwendet werden.

Eine exemplarische Vorgehensweise zu Batchrückschlüssen mit Azure Machine Learning Compute finden Sie unter [Verwenden von Azure Machine Learning-Pipelines für Batchbewertung](tutorial-pipeline-batch-scoring-classification.md).

### <a name="iot-edge-inference"></a><a id="iotedge"></a> IoT Edge-Rückschluss
Unterstützung für die Bereitstellung auf Edge-Geräten befindet sich in der Vorschauphase. Weitere Informationen finden Sie unter [Bereitstellen von Azure Machine Learning als IoT Edge-Modul](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning).

## <a name="delete-resources"></a>Löschen von Ressourcen

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azcli)

Um einen bereitgestellten Webdienst zu löschen, verwenden Sie `az ml service <name of webservice>`.

Um ein registriertes Modell aus Ihrem Arbeitsbereich zu löschen, verwenden Sie `az ml model delete <model id>`.

Erfahren Sie mehr über das [Löschen eines Webdiensts](/cli/azure/ext/azure-cli-ml/ml/service#ext-azure-cli-ml-az-ml-service-delete) und das [Löschen eines Modells](/cli/azure/ext/azure-cli-ml/ml/model#ext-azure-cli-ml-az-ml-model-delete).

# <a name="python"></a>[Python](#tab/python)

Verwenden Sie zum Löschen eines bereitgestellten Webdiensts `service.delete()`.
Verwenden Sie zum Löschen eines registrierten Modells `model.delete()`.

Weitere Informationen finden Sie in der Dokumentation zu [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--) und [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

---


## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung bei einer fehlerhaften Bereitstellung](how-to-troubleshoot-deployment.md)
* [Bereitstellen für Azure Kubernetes Service](how-to-deploy-azure-kubernetes-service.md)
* [Erstellen von Clientanwendungen zum Nutzen von Webdiensten](how-to-consume-web-service.md)
* [Aktualisieren des Webdiensts](how-to-deploy-update-web-service.md)
* [Wie man ein Modell mit einem benutzerdefinierten Docker-Image bereitstellt](how-to-deploy-custom-docker-image.md)
* [Verwenden von TLS zum Absichern eines Webdiensts mit Azure Machine Learning](how-to-secure-web-service.md)
* [Überwachen Ihrer Azure Machine Learning-Modelle mit Application Insights](how-to-enable-app-insights.md)
* [Sammeln von Daten für Modelle in der Produktion](how-to-enable-data-collection.md)
* [Erstellen von Ereigniswarnungen und Triggern für Modellbereitstellungen](how-to-use-event-grid.md)

