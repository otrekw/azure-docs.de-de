---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 11/06/2019
ms.author: larryfr
ms.openlocfilehash: 52689c585e148c18d16ad627570414a8de444fea
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74926977"
---
Die Einträge im `inferenceconfig.json`-Dokument werden den Parametern für die [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)-Klasse zugeordnet. In der folgenden Tabelle wird die Zuordnung zwischen den Entitäten im JSON-Dokument und den Parametern für die Methode beschrieben:

| JSON-Entität | Methodenparameter | BESCHREIBUNG |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | Pfad zu einer lokalen Datei, die den Code zum Ausführen für das Image enthält. |
| `runtime` | `runtime` | Optional. Die Runtime, die für das Image verwendet werden soll. Aktuelle unterstützte Runtimes sind `spark-py` und `python`. Wenn `environment` festgelegt ist, wird diese Angabe ignoriert. |
| `condaFile` | `conda_file` | Optional. Pfad zu einer lokalen Datei, die eine für das Image zu verwendende Conda-Umgebungsdefinition enthält.  Wenn `environment` festgelegt ist, wird diese Angabe ignoriert. |
| `extraDockerFileSteps` | `extra_docker_file_steps` | Optional. Pfad zu einer lokalen Datei mit zusätzlichen Docker-Schritten, die ausgeführt werden sollen, wenn das Image eingerichtet wird.  Wenn `environment` festgelegt ist, wird diese Angabe ignoriert.|
| `sourceDirectory` | `source_directory` | Optional. Pfad zu Ordnern, die alle Dateien zum Erstellen des Images enthalten. |
| `enableGpu` | `enable_gpu` | Optional. Gibt an, ob die GPU-Unterstützung im Image aktiviert werden soll. Das GPU-Image muss in einem Azure-Dienst wie Azure Container Instances, Azure Machine Learning Compute, Azure Virtual Machines und Azure Kubernetes Service verwendet werden. Die Standardeinstellung lautet „false“. Wenn `environment` festgelegt ist, wird diese Angabe ignoriert.|
| `baseImage` | `base_image` | Optional. Ein benutzerdefiniertes Image, das als Basisimage verwendet werden soll. Ist kein Basisimage angegeben, basiert das Image auf dem angegebenen runtime-Parameter. Wenn `environment` festgelegt ist, wird diese Angabe ignoriert. |
| `baseImageRegistry` | `base_image_registry` | Optional. Imageregistrierung, die das Basisimage enthält. Wenn `environment` festgelegt ist, wird diese Angabe ignoriert.|
| `cudaVersion` | `cuda_version` | Optional. CUDA-Version, die für Images installiert wird, die GPU-Unterstützung benötigen. Das GPU-Image muss in einem Azure-Dienst wie Azure Container Instances, Azure Machine Learning Compute, Azure Virtual Machines und Azure Kubernetes Service verwendet werden. Unterstützt werden die Versionen 9.0, 9.1 und 10.0. Ist `enable_gpu` festgelegt, lautet der Standardwert „9.1“. Wenn `environment` festgelegt ist, wird diese Angabe ignoriert. |
| `description` | `description` | Eine Beschreibung für das Image. Wenn `environment` festgelegt ist, wird diese Angabe ignoriert.  |
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

Der folgende JSON-Code ist eine exemplarische Rückschlusskonfiguration, in der eine vorhandene [Umgebung](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) für Azure Machine Learning mit einer spezifischen Version für die Verwendung mit der CLI verwendet wird:

```json
{
    "entryScript": "score.py",
    "environment":{
        "name": "myenv",
        "version": "1"
    },
    "condaFile": "myenv.yml",
    "sourceDirectory": null
}
```

Der folgende JSON-Code ist eine exemplarische Rückschlusskonfiguration, in der eine vorhandene [Umgebung](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) für Azure Machine Learning mit der neuesten Version für die Verwendung mit der CLI verwendet wird:

```json
{
    "entryScript": "score.py",
    "environment":{
        "name": "myenv",
        "version": null
    },
    "condaFile": "myenv.yml",
    "sourceDirectory": null
}
```
