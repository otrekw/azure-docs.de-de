---
title: Auslösen von Azure Machine Learning-Pipelines
titleSuffix: Azure Machine Learning
description: Ausgelöste Pipelines ermöglichen die Automatisierung routinemäßiger, zeitaufwändiger Aufgaben wie Datenverarbeitung, Training und Überwachung.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: laobri
author: lobrien
ms.date: 12/16/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: a006dfd4f78f90ed323e5780b173cffb6daeac4a
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98881736"
---
# <a name="trigger-machine-learning-pipelines-with-azure-machine-learning-sdk-for-python"></a>Auslösen von Machine Learning-Pipelines mit dem Azure Machine Learning-SDK für Python

In diesem Artikel erfahren Sie, wie Sie programmgesteuert eine Pipeline für die Ausführung in Azure planen. Sie können wahlweise einen Plan erstellen, der auf verstrichener Zeit oder auf Änderungen im Dateisystem beruht. Zeitbasierte Pläne können zum Erledigen von Routineaufgaben (z.B. Datendriftüberwachung) verwendet werden. Pläne auf Basis von Änderungen können für die Reaktion auf unregelmäßige bzw. unvorhersehbare Änderungen (z.B. Hochladen neuer Daten oder Bearbeiten alter Daten) verwendet werden. Nachdem Sie gelernt haben, wie Pläne erstellt werden, erfahren Sie, wie Sie diese abrufen und deaktivieren können. Schließlich erfahren Sie, wie Sie mit einer Azure-Logik-App komplexere auslösende Logik oder komplexeres Verhalten ermöglichen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen.

* Eine Python-Umgebung, in der das Azure Machine Learning SDK für Python installiert ist. Weitere Informationen finden Sie unter [Erstellen und Verwalten von wiederverwendbaren Umgebungen für Training und Bereitstellung mit Azure Machine Learning](how-to-use-environments.md).

* Ein Machine Learning-Arbeitsbereich mit einer veröffentlichten Pipeline. Sie können die Pipeline verwenden, die in [Erstellen und Ausführen von Machine Learning-Pipelines mit dem Azure Machine Learning SDK](./how-to-create-machine-learning-pipelines.md) erstellt wurde.

## <a name="initialize-the-workspace--get-data"></a>Initialisieren des Arbeitsbereichs und Abrufen von Daten

Zum Planen einer Pipeline benötigen Sie einen Verweis auf Ihren Arbeitsbereich, den Bezeichner (ID) der veröffentlichten Pipeline und den Namen des Experiments, in dem Sie den Plan erstellen möchten. Diese Werte können Sie mithilfe des folgenden Codes abrufen:

```Python
import azureml.core
from azureml.core import Workspace
from azureml.pipeline.core import Pipeline, PublishedPipeline
from azureml.core.experiment import Experiment

ws = Workspace.from_config()

experiments = Experiment.list(ws)
for experiment in experiments:
    print(experiment.name)

published_pipelines = PublishedPipeline.list(ws)
for published_pipeline in  published_pipelines:
    print(f"{published_pipeline.name},'{published_pipeline.id}'")

experiment_name = "MyExperiment" 
pipeline_id = "aaaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" 
```

## <a name="create-a-schedule"></a>Erstellen eines Zeitplans

Wenn eine Pipeline regelmäßig ausgeführt werden soll, erstellen Sie einen Plan. Ein `Schedule` verknüpft eine Pipeline, ein Experiment und einen Auslöser. Der Auslöser kann entweder vom Typ `ScheduleRecurrence` sein, der die Wartezeit zwischen den Ausführungen beschreibt, oder ein Datenspeicherpfad, der ein Verzeichnis angibt, das auf Änderungen überwacht werden soll. In beiden Fällen benötigen Sie die ID der Pipeline und den Namen des Experiments, in dem der Plan erstellt werden soll.

Importieren Sie am Anfang der Python-Datei die Klassen `Schedule` und `ScheduleRecurrence`:

```python

from azureml.pipeline.core.schedule import ScheduleRecurrence, Schedule
```

### <a name="create-a-time-based-schedule"></a>Erstellen eines zeitbasierten Plans

Der `ScheduleRecurrence`-Konstruktor verfügt über das erforderliche `frequency`-Argument, das einer der folgenden Zeichenfolgen entsprechen muss: „Minute“, „Stunde“, „Tag“, „Woche“ oder „Monat“. Außerdem wird ein ganzzahliges `interval`-Argument benötigt, das angibt, wie viele `frequency`-Einheiten zwischen den einzelnen Starts des Zeitplans verstreichen sollen. Optionale Argumente ermöglichen Ihnen eine genauere Angabe der Startzeiten, wie in der [Dokumentation zum ScheduleRecurrence SDK](/python/api/azureml-pipeline-core/azureml.pipeline.core.schedule.schedulerecurrence?preserve-view=true&view=azure-ml-py) ausführlich erläutert.

Erstellen Sie einen `Schedule`, der alle 15 Minuten eine Ausführung startet:

```python
recurrence = ScheduleRecurrence(frequency="Minute", interval=15)
recurring_schedule = Schedule.create(ws, name="MyRecurringSchedule", 
                            description="Based on time",
                            pipeline_id=pipeline_id, 
                            experiment_name=experiment_name, 
                            recurrence=recurrence)
```

### <a name="create-a-change-based-schedule"></a>Erstellen eines änderungsbasierten Plans

Pipelines, die durch Dateiänderungen ausgelöst werden, sind möglicherweise effizienter als zeitbasierte Pläne. Vielleicht möchten Sie zum Beispiel einen Vorverarbeitungsschritt ausführen, wenn eine Datei geändert oder einem Datenverzeichnis eine neue Datei hinzugefügt wird. Sie können alle Änderungen an einem Datenspeicher oder Änderungen in einem bestimmten Verzeichnis des Datenspeichers überwachen. Wenn Sie ein bestimmtes Verzeichnis überwachen, lösen Änderungen in den Unterverzeichnissen dieses Verzeichnisses _keine_ Ausführung aus.

Um einen `Schedule` zu erstellen, der auf Dateiänderungen reagiert, müssen Sie den `datastore`-Parameter im Aufruf auf [Schedule.create](/python/api/azureml-pipeline-core/azureml.pipeline.core.schedule.schedule?preserve-view=true&view=azure-ml-py#&preserve-view=truecreate-workspace--name--pipeline-id--experiment-name--recurrence-none--description-none--pipeline-parameters-none--wait-for-provisioning-false--wait-timeout-3600--datastore-none--polling-interval-5--data-path-parameter-name-none--continue-on-step-failure-none--path-on-datastore-none---workflow-provider-none---service-endpoint-none-) festlegen. Zum Überwachen eines Ordners legen Sie das `path_on_datastore`-Argument fest.

Mit dem `polling_interval`-Argument können Sie das Intervall, in dem der Datenspeicher auf Änderungen geprüft wird, in Minuten angeben.

Wenn die Pipeline mit [DataPath](/python/api/azureml-core/azureml.data.datapath.datapath?preserve-view=true&view=azure-ml-py) [PipelineParameter](/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelineparameter?preserve-view=true&view=azure-ml-py) erstellt wurde, können Sie durch Festlegen des `data_path_parameter_name`-Arguments diese Variable auf den Namen der geänderten Datei einstellen.

```python
datastore = Datastore(workspace=ws, name="workspaceblobstore")

reactive_schedule = Schedule.create(ws, name="MyReactiveSchedule", description="Based on input file change.",
                            pipeline_id=pipeline_id, experiment_name=experiment_name, datastore=datastore, data_path_parameter_name="input_data")
```

### <a name="optional-arguments-when-creating-a-schedule"></a>Optionale Argumente beim Erstellen eines Plans

Neben den zuvor beschriebenen Argumenten können Sie das `status`-Argument auf `"Disabled"` einstellen, um einen inaktiven Plan zu erstellen. Schließlich können Sie noch mit `continue_on_step_failure` einen booleschen Wert übergeben, der das standardmäßige Fehlerverhalten der Pipeline überschreibt.

## <a name="view-your-scheduled-pipelines"></a>Anzeigen der geplanten Pipelines

Navigieren Sie in Ihrem Webbrowser zu Azure Machine Learning. Wählen Sie im Navigationsbereich im Bereich **Endpunkte** den Eintrag **Pipelineendpunkte** aus. Dadurch gelangen Sie zu einer Liste der im Arbeitsbereich veröffentlichten Pipelines.

![Seite „Pipelines“ in AML](./media/how-to-trigger-published-pipeline/scheduled-pipelines.png)

Auf dieser Seite können Sie Übersichtsinformationen wie Name, Beschreibung, Status usw. zu allen Pipelines im Arbeitsbereich anzeigen. Durch Klicken auf eine Pipeline erhalten Sie ausführlichere Informationen. Auf der Seite, die dann geöffnet wird, finden Sie weitere Details zu Ihrer Pipeline und können einzelne Ausführungen genauer betrachten.

## <a name="deactivate-the-pipeline"></a>Deaktivieren der Pipeline

Wenn Sie über eine `Pipeline` verfügen, die zwar veröffentlicht, aber noch nicht geplant wurde, können Sie diese wie folgt deaktivieren:

```python
pipeline = PublishedPipeline.get(ws, id=pipeline_id)
pipeline.disable()
```

Wenn die Pipeline bereits geplant ist, müssen Sie zuerst den Plan abbrechen. Rufen Sie die ID des Plans aus dem Portal ab, oder führen Sie folgenden Befehl aus:

```python
ss = Schedule.list(ws)
for s in ss:
    print(s)
```

Sobald Sie über die `schedule_id` des zu deaktivierenden Plans verfügen, führen Sie Folgendes aus:

```python
def stop_by_schedule_id(ws, schedule_id):
    s = next(s for s in Schedule.list(ws) if s.id == schedule_id)
    s.disable()
    return s

stop_by_schedule_id(ws, schedule_id)
```

Wenn Sie dann `Schedule.list(ws)` erneut ausführen, sollte eine leere Liste angezeigt werden.

## <a name="use-azure-logic-apps-for-complex-triggers"></a>Verwenden von Azure Logic Apps für komplexere Workflows 

Komplexere Triggerregeln oder komplexeres Verhalten können mithilfe einer [Azure-Logik-App](../logic-apps/logic-apps-overview.md) erstellt werden.

Um eine Azure-Logik-App zum Auslösen einer Machine Learning-Pipeline zu verwenden, benötigen Sie den REST-Endpunkt für eine veröffentlichte Machine Learning-Pipeline. [Erstellen und veröffentlichen Sie Ihre Pipeline](./how-to-create-machine-learning-pipelines.md). Suchen Sie anschließend mithilfe der Pipeline-ID den REST-Endpunkt Ihrer `PublishedPipeline`:

```python
# You can find the pipeline ID in Azure Machine Learning studio

published_pipeline = PublishedPipeline.get(ws, id="<pipeline-id-here>")
published_pipeline.endpoint 
```

## <a name="create-a-logic-app"></a>Erstellen einer Logik-App

Erstellen Sie nun eine [Azure Logic Apps](../logic-apps/logic-apps-overview.md)-Instanz. Auf Wunsch können Sie [eine Integrationsdienstumgebung (Integration Service Environment, ISE) verwenden](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) und [einen vom Kunden verwalteten Schlüssel](../logic-apps/customer-managed-keys-integration-service-environment.md) für die Verwendung durch Ihre Logik-App einrichten.

Führen Sie nach der Bereitstellung Ihrer Logik-App die folgenden Schritte aus, um einen Trigger für Ihre Pipeline zu konfigurieren:

1. [Erstellen Sie eine systemseitig zugewiesene verwaltete Identität](../logic-apps/create-managed-service-identity.md), um der App Zugriff auf Ihren Azure Machine Learning-Arbeitsbereich zu gewähren.

1. Navigieren Sie zur Logik-App-Designer-Ansicht, und wählen Sie die Vorlage „Leere Logik-App“ aus. 
    > [!div class="mx-imgBorder"]
    > ![Leere Vorlage](media/how-to-trigger-published-pipeline/blank-template.png)

1. Suchen Sie im Designer nach **Blob**. Wählen Sie den Trigger **Beim Hinzufügen oder Ändern eines Blobs (nur Eigenschaften)** aus, und fügen Sie diesen Trigger Ihrer Logik-App hinzu.
    > [!div class="mx-imgBorder"]
    > ![Hinzufügen eines Triggers](media/how-to-trigger-published-pipeline/add-trigger.png)

1. Geben Sie die Verbindungsinformationen für das Blob Storage-Konto ein, das Sie auf Ergänzungen oder Änderungen an Blobs überwachen möchten. Wählen Sie den zu überwachenden Container aus. 
 
    Wählen Sie ein für Sie geeignetes **Intervall** und eine **Häufigkeit** für den Abruf von Updates aus.  

    > [!NOTE]
    > Mit diesem Auslöser wird der ausgewählte Container überwacht, jedoch keine Unterordner.

1. Fügen Sie eine HTTP-Aktion hinzu, die ausgeführt wird, wenn ein neues oder geändertes Blob erkannt wird. Wählen Sie **+ Neuer Schritt** aus, suchen Sie nach der HTTP-Aktion, und wählen Sie sie aus.

  > [!div class="mx-imgBorder"]
  > ![Suchen einer HTTP-Aktion](media/how-to-trigger-published-pipeline/search-http.png)

  Konfigurieren Sie Ihre Aktion mit den folgenden Einstellungen:

  | Einstellung | Wert | 
  |---|---|
  | HTTP-Aktion | POST |
  | URI |Der Endpunkt für die veröffentlichte Pipeline, den Sie als [Voraussetzung](#prerequisites) festgelegt haben |
  | Authentifizierungsmodus | Verwaltete Identität |

1. Richten Sie Ihren Zeitplan so ein, dass ggf. der Wert aller [DataPath-Pipelineparameter](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-showcasing-datapath-and-pipelineparameter.ipynb) festgelegt wird:

    ```json
    {
      "DataPathAssignments": {
        "input_datapath": {
          "DataStoreName": "<datastore-name>",
          "RelativePath": "@{triggerBody()?['Name']}" 
        }
      },
      "ExperimentName": "MyRestPipeline",
      "ParameterAssignments": {
        "input_string": "sample_string3"
      },
      "RunSource": "SDK"
    }
    ```

    Verwenden Sie den `DataStoreName`, den Sie Ihrem Arbeitsbereich als [Voraussetzung](#prerequisites) hinzugefügt haben.
     
    > [!div class="mx-imgBorder"]
    > ![HTTP-Einstellungen](media/how-to-trigger-published-pipeline/http-settings.png)

1. Wählen Sie **Speichern** aus. Der Zeitplan ist nun bereit.

> [!IMPORTANT]
> Wenn Sie mit der rollenbasierten Zugriffssteuerung in Azure (Azure RBAC) den Zugriff auf Ihre Pipeline verwalten, [legen Sie die Berechtigungen für Ihr Pipelineszenario fest (Training oder Bewertung)](how-to-assign-roles.md#common-scenarios).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie das Azure Machine Learning SDK für Python verwendet, um eine Pipeline auf zwei verschiedene Arten zu planen. Ein Plan wiederholt sich auf Basis der verstrichenen Uhrzeit. Der andere Plan wird ausgeführt, wenn eine Datei in einem angegebenen `Datastore` oder in einem Verzeichnis in diesem Speicher geändert wird. Sie haben erfahren, wie Sie das Portal zum Untersuchen von Pipelines und einzelnen Ausführungen verwenden können. Sie haben gelernt, wie Sie einen Plan deaktivieren, damit die Pipeline nicht mehr ausgeführt wird. Schließlich haben Sie eine Azure-Logik-App erstellt, um eine Pipeline auszulösen. 

Weitere Informationen finden Sie unter

> [!div class="nextstepaction"]
> [Verwenden von Azure Machine Learning-Pipelines für die Batchbewertung](tutorial-pipeline-batch-scoring-classification.md)

* Weitere Informationen zu [Pipelines](concept-ml-pipelines.md)
* Erfahren Sie mehr über das [Erkunden von Azure Machine Learning mit Jupyter](samples-notebooks.md)