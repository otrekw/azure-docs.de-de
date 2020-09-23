---
title: Veröffentlichen von ML-Pipelines
titleSuffix: Azure Machine Learning
description: Führen Sie Workflows für maschinelles Lernen mit Pipelines des maschinellen Lernens und dem Azure Machine Learning SDK für Python aus.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: sgilley
ms.author: laobri
author: lobrien
ms.date: 8/25/2020
ms.topic: conceptual
ms.custom: how-to, contperfq1
ms.openlocfilehash: ddc8186e85001a2a3ed2ed9f57b8f025133ef16a
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90897777"
---
# <a name="publish-and-track-machine-learning-pipelines"></a>Veröffentlichen und Nachverfolgen von Pipelines des maschinellen Lernens



In diesem Artikel erfahren Sie, wie Sie eine Pipeline des maschinellen Lernens für Ihre Kollegen oder Kunden freigeben.

Pipelines des maschinellen Lernens sind wiederverwendbare Workflows für Aufgaben beim maschinellem Lernen. Ein Vorteil von Pipelines ist die verstärkte Kollaboration. Sie können auch Versionspipelines verwenden, mit denen Kunden das aktuelle Modell verwenden können, während Sie an einer neuen Version arbeiten. 

## <a name="prerequisites"></a>Voraussetzungen

* Erstellen Sie einen [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md), der Ihre gesamten Pipelineressourcen aufnehmen soll.

* [Konfigurieren Sie Ihre Entwicklungsumgebung](how-to-configure-environment.md) für die Installation des Azure Machine Learning SDK, oder verwenden Sie eine [Azure Machine Learning-Computeinstanz](concept-compute-instance.md) mit bereits installiertem SDK.

* Erstellen Sie eine Pipeline des maschinellen Lernens, und führen Sie sie aus, z. B. durch Befolgen der Schritte im [Tutorial: Erstellen einer Azure Machine Learning-Pipeline für die Batchbewertung](tutorial-pipeline-batch-scoring-classification.md). Weitere Optionen finden Sie unter [Erstellen und Ausführen von Pipelines des maschinellen Lernens mit dem Azure Machine Learning SDK](how-to-create-your-first-pipeline.md).

## <a name="publish-a-pipeline"></a>Veröffentlichen einer Pipeline

Sobald Sie eine Pipeline eingerichtet haben, können Sie eine Pipeline so veröffentlichen, dass sie mit unterschiedlichen Eingaben ausgeführt wird. Damit der REST-Endpunkt einer bereits veröffentlichten Pipeline Parameter akzeptiert, müssen Sie die Pipeline so konfigurieren, dass sie verschiedene `PipelineParameter`-Objekte als Argumente verwendet.

1. Um einen Pipelineparameter zu erstellen, verwenden Sie ein [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py&preserve-view=true)-Objekt mit einem Standardwert.

   ```python
   from azureml.pipeline.core.graph import PipelineParameter
   
   pipeline_param = PipelineParameter(
     name="pipeline_arg",
     default_value=10)
   ```

2. Fügen Sie dieses `PipelineParameter`-Objekt als Parameter zu einem der Schritte in der Pipeline wie folgt hinzu:

   ```python
   compareStep = PythonScriptStep(
     script_name="compare.py",
     arguments=["--comp_data1", comp_data1, "--comp_data2", comp_data2, "--output_data", out_data3, "--param1", pipeline_param],
     inputs=[ comp_data1, comp_data2],
     outputs=[out_data3],
     compute_target=compute_target,
     source_directory=project_folder)
   ```

3. Veröffentlichen Sie diese Pipeline, die beim Aufruf einen Parameter akzeptiert.

   ```python
   published_pipeline1 = pipeline_run1.publish_pipeline(
        name="My_Published_Pipeline",
        description="My Published Pipeline Description",
        version="1.0")
   ```

## <a name="run-a-published-pipeline"></a>Ausführen einer veröffentlichten Pipeline

Alle veröffentlichten Pipelines weisen einen REST-Endpunkt auf. Mit dem Pipelineendpunkt können Sie eine Ausführung der Pipeline von beliebigen externen Systemen aus auslösen, einschließlich Clients, die nicht Python verwenden. Dieser Endpunkt ermöglicht „verwaltete Wiederholbarkeit“ in Szenarien für die Batchbewertung und für erneutes Training.

Zum Aufrufen der Ausführung der vorhergehenden Pipeline benötigen Sie ein Headertoken für die Azure Active Directory-Authentifizierung. Das Abrufen eines solchen Tokens wird in der Referenz zur [AzureCliAuthentication-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py&preserve-view=true) und im Notebook [Authentifizierung in Azure Machine Learning](https://aka.ms/pl-restep-auth) beschrieben.

```python
from azureml.pipeline.core import PublishedPipeline
import requests

response = requests.post(published_pipeline1.endpoint,
                         headers=aad_token,
                         json={"ExperimentName": "My_Pipeline",
                               "ParameterAssignments": {"pipeline_arg": 20}})
```

## <a name="create-a-versioned-pipeline-endpoint"></a>Erstellen eines Pipelineendpunkts mit Versionsangabe

Sie können einen Pipelineendpunkt erstellen, hinter dem sich mehrere veröffentlichte Pipelines befinden. Dieser stellt einen festen REST-Endpunkt bereit, wenn Sie Ihre ML-Pipelines durchlaufen und aktualisieren.

```python
from azureml.pipeline.core import PipelineEndpoint

published_pipeline = PipelineEndpoint.get(workspace=ws, name="My_Published_Pipeline")
pipeline_endpoint = PipelineEndpoint.publish(workspace=ws, name="PipelineEndpointTest",
                                            pipeline=published_pipeline, description="Test description Notebook")
```

## <a name="submit-a-job-to-a-pipeline-endpoint"></a>Übermitteln eines Auftrags an einen Pipelineendpunkt

Sie können einen Auftrag an die Standardversion eines Pipelineendpunkts übermitteln:

```python
pipeline_endpoint_by_name = PipelineEndpoint.get(workspace=ws, name="PipelineEndpointTest")
run_id = pipeline_endpoint_by_name.submit("PipelineEndpointExperiment")
print(run_id)
```

Sie können einen Auftrag auch an eine bestimmte Version übermitteln:

```python
run_id = pipeline_endpoint_by_name.submit("PipelineEndpointExperiment", pipeline_version="0")
print(run_id)
```

Das gleiche Ergebnis kann mit der REST-API erreicht werden:

```python
rest_endpoint = pipeline_endpoint_by_name.endpoint
response = requests.post(rest_endpoint, 
                         headers=aad_token, 
                         json={"ExperimentName": "PipelineEndpointExperiment",
                               "RunSource": "API",
                               "ParameterAssignments": {"1": "united", "2":"city"}})
```

## <a name="use-published-pipelines-in-the-studio"></a>Verwenden von veröffentlichten Pipelines in Studio

Sie können über Studio auch eine veröffentlichte Pipeline ausführen:

1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com) an.

1. [Zeigen Sie den Arbeitsbereich an](how-to-manage-workspace.md#view).

1. Klicken Sie links auf **Endpunkte**.

1. Wählen Sie oben **Pipelineendpunkte** aus.
 ![Liste der veröffentlichen Machine Learning-Pipelines](./media/how-to-create-your-first-pipeline/pipeline-endpoints.png)

1. Wählen Sie eine bestimmte Pipeline aus, um einen Pipelineendpunkt auszuführen, zu nutzen oder die Ergebnisse früherer Ausführungen des Pipelineendpunkts zu überprüfen.

## <a name="disable-a-published-pipeline"></a>Deaktivieren einer veröffentlichten Pipeline

Um eine Pipeline aus der Liste der veröffentlichten Pipelines auszublenden, deaktivieren Sie sie in Studio oder über das SDK:

```python
# Get the pipeline by using its ID from Azure Machine Learning studio
p = PublishedPipeline.get(ws, id="068f4885-7088-424b-8ce2-eeb9ba5381a6")
p.disable()
```

Sie können sie mit `p.enable()` wieder aktivieren. Weitere Informationen finden Sie in der Referenz zur [PublishedPipeline-Klasse](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.publishedpipeline?view=azure-ml-py&preserve-view=true).

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [diese Jupyter-Notebooks auf GitHub](https://aka.ms/aml-pipeline-readme), um die Pipelines für maschinelles Lernen eingehender zu erkunden.
- Hinweise zu den Paketen [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py&preserve-view=true) und [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py&preserve-view=true) finden Sie in der SDK-Referenzhilfe.
- Tipps zum Debuggen und zur Problembehandlung von Pipelines finden Sie in der [Schrittanleitung](how-to-debug-pipelines.md).
