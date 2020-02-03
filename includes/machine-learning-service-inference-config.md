---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 01/28/2020
ms.author: larryfr
ms.openlocfilehash: 6970732f53ffa99849e20f279c8bdf4160c30a0a
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76845150"
---
Die Einträge im `inferenceconfig.json`-Dokument werden den Parametern für die [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)-Klasse zugeordnet. In der folgenden Tabelle wird die Zuordnung zwischen den Entitäten im JSON-Dokument und den Parametern für die Methode beschrieben:

| JSON-Entität | Methodenparameter | Beschreibung |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | Pfad zu einer lokalen Datei, die den Code zum Ausführen für das Image enthält. |
| `runtime` | `runtime` | Optional. Die Runtime, die für das Image verwendet werden soll. Unterstützte Runtimes sind `spark-py` und `python`. Wenn `environment` festgelegt ist, wird dieser Eintrag ignoriert. |
| `condaFile` | `conda_file` | Optional. Pfad zu einer lokalen Datei, die eine für das Image zu verwendende Conda-Umgebungsdefinition enthält.  Wenn `environment` festgelegt ist, wird dieser Eintrag ignoriert. |
| `extraDockerFileSteps` | `extra_docker_file_steps` | Optional. Pfad zu einer lokalen Datei mit zusätzlichen Docker-Schritten, die ausgeführt werden sollen, wenn das Image eingerichtet wird.  Wenn `environment` festgelegt ist, wird dieser Eintrag ignoriert.|
| `sourceDirectory` | `source_directory` | Optional. Pfad zu Ordnern, die alle Dateien zum Erstellen des Bilds enthalten, wodurch der Zugriff auf Dateien in diesem Ordner oder Unterordner vereinfacht wird. Sie können einen vollständigen Ordner von Ihrem lokalen Computer als Abhängigkeiten für den Webdienst hochladen. Hinweis: Ihre „entry_script“-, „conda_file“- und „extra_docker_file_steps“-Pfad sind relative Pfade zum „source_directory“-Pfad. |
| `enableGpu` | `enable_gpu` | Optional. Gibt an, ob die GPU-Unterstützung im Image aktiviert werden soll. Das GPU-Image muss in einem Azure-Dienst wie Azure Container Instances verwendet werden. Beispielsweise Azure Machine Learning Compute, Azure Virtual Machines und Azure Kubernetes Service. Die Standardeinstellung lautet „false“. Wenn `environment` festgelegt ist, wird dieser Eintrag ignoriert.|
| `baseImage` | `base_image` | Optional. Ein benutzerdefiniertes Image, das als Basisimage verwendet werden soll. Ist kein Basisimage angegeben, basiert das Image auf dem angegebenen runtime-Parameter. Wenn `environment` festgelegt ist, wird dieser Eintrag ignoriert. |
| `baseImageRegistry` | `base_image_registry` | Optional. Imageregistrierung, die das Basisimage enthält. Wenn `environment` festgelegt ist, wird dieser Eintrag ignoriert.|
| `cudaVersion` | `cuda_version` | Optional. CUDA-Version, die für Images installiert wird, die GPU-Unterstützung benötigen. Das GPU-Image muss in einem Azure-Dienst verwendet werden. Beispielsweise Azure Container Instances, Azure Machine Learning Compute, Azure Virtual Machines und Azure Kubernetes Service. Unterstützt werden die Versionen 9.0, 9.1 und 10.0. Ist `enable_gpu` festgelegt, lautet der Standardwert „9.1“. Wenn `environment` festgelegt ist, wird dieser Eintrag ignoriert. |
| `description` | `description` | Eine Beschreibung für das Image. Wenn `environment` festgelegt ist, wird dieser Eintrag ignoriert.  |
| `environment` | `environment` | Optional.  Die [Umgebung](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) für Azure Machine Learning.|

Der folgende JSON-Code ist ein Beispiel für eine Rückschlusskonfiguration für die Verwendung mit der CLI:

```json
{
    "entryScript": "score.py",
    "runtime": "python",
    "condaFile": "myenv.yml",
    "extraDockerfileSteps": null,
    "sourceDirectory": null,
    "enableGpu": false,
    "baseImage": null,
    "baseImageRegistry": null
}
```

Sie können die vollständigen Spezifikationen einer Azure Machine Learning-[Umgebung](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) in die Rückschlusskonfigurationsdatei aufnehmen. Wenn diese Umgebung in Ihrem Arbeitsbereich nicht vorhanden ist, erstellt Azure Machine Learning sie. Andernfalls aktualisiert Azure Machine Learning die Umgebung bei Bedarf. Nachfolgend finden Sie ein JSON-Beispiel:

```json
{
    "entryScript": "score.py",
    "environment": {
        "docker": {
            "arguments": [],
            "baseDockerfile": null,
            "baseImage": "mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04",
            "enabled": false,
            "sharedVolumes": true,
            "shmSize": null
        },
        "environmentVariables": {
            "EXAMPLE_ENV_VAR": "EXAMPLE_VALUE"
        },
        "name": "my-deploy-env",
        "python": {
            "baseCondaEnvironment": null,
            "condaDependencies": {
                "channels": [
                    "conda-forge"
                ],
                "dependencies": [
                    "python=3.6.2",
                    {
                        "pip": [
                            "azureml-defaults",
                            "azureml-telemetry",
                            "scikit-learn",
                            "inference-schema[numpy-support]"
                        ]
                    }
                ],
                "name": "project_environment"
            },
            "condaDependenciesFile": null,
            "interpreterPath": "python",
            "userManagedDependencies": false
        },
        "version": "1"
    }
}
```

Sie können auch eine vorhandene Azure Machine Learning-[Umgebung](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) in getrennten CLI-Parametern verwenden und den Schlüssel „Environment“ aus der Rückschlusskonfigurationsdatei entfernen. Verwenden Sie „-e“ für den Umgebungsnamen und „--ev“ für die Umgebungsversion. Wenn Sie „--ev“ nicht angeben, wird die neueste Version verwendet. Hier sehen Sie ein Beispiel für eine Rückschlusskonfigurationsdatei:

```json
{
    "entryScript": "score.py",
    "sourceDirectory": null
}
```

Der folgende Befehl veranschaulicht, wie Sie ein Modell mithilfe der vorherigen Rückschlusskonfigurationsdatei (namens „myInferenceConfig.json“) bereitstellen. 

Außerdem wird die neueste Version einer vorhandenen Azure Machine Learning-[Umgebung](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) (namens „AzureML-Minimal“) verwendet.

```azurecli-interactive
az ml model deploy -m mymodel:1 --ic myInferenceConfig.json -e AzureML-Minimal --dc deploymentconfig.json
```
