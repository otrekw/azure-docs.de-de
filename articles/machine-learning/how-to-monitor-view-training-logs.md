---
title: Überwachen und Anzeigen von ML-Ausführungsprotokollen und -metriken
titleSuffix: Azure Machine Learning
description: Verwenden Sie Jupyter-Widgets und Azure Machine Learning Studio, um Ihre ML-Experimente zu überwachen und Ausführungsmetriken anzuzeigen.
services: machine-learning
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.service: machine-learning
ms.subservice: core
ms.date: 07/30/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 47531da9c1e508281a57074df7aa10ffffe78810
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102518737"
---
# <a name="monitor-and-view-ml-run-logs-and-metrics"></a>Überwachen und Anzeigen von ML-Ausführungsprotokollen und -metriken

Erfahren Sie, wie Sie Azure Machine Learning-Ausführungen überwachen und die zugehörigen Protokolle anzeigen. 

Wenn Sie ein Experiment ausführen, werden Protokolle und Metriken für Sie gestreamt.  Sie können auch eigene hinzufügen.  Informationen zur Vorgehensweise finden Sie unter [Aktivieren der Protokollierung in Azure ML-Trainingsausführungen](how-to-track-experiments.md).

Mithilfe der Protokolle können Sie Fehler und Warnungen für Ihre Ausführungen diagnostizieren. Leistungsmetriken wie Parameter und die Modellgenauigkeit helfen Ihnen beim Nachverfolgen und Überwachen Ihrer Ausführungen.

In diesem Artikel erfahren Sie, wie Sie Protokolle über die folgenden Methoden anzeigen:

> [!div class="checklist"]
> * Überwachen von Ausführungen in Studio
> * Überwachen von Ausführungen mit dem Jupyter Notebook-Widget
> * Überwachen von Ausführungen des automatisierten maschinellen Lernens
> * Anzeigen von Ausgabeprotokollen nach Abschluss
> * Anzeigen von Ausgabeprotokollen in Studio

Allgemeine Informationen zum Verwalten von Experimenten finden Sie unter [Starten, Überwachen und Abbrechen von Trainingsausführungen](how-to-manage-runs.md).

## <a name="monitor-runs-using-the-jupyter-notebook-widget"></a>Überwachen von Ausführungen mit dem Jupyter Notebook-Widget

Wenn Sie Ausführungen mithilfe der **ScriptRunConfig**-Methode übermitteln, können Sie den Fortschritt der Ausführung mit dem [Jupyter-Widget](/python/api/azureml-widgets/azureml.widgets) anzeigen. Ebenso wie die Übermittlung der Ausführung ist das Widget asynchron und stellt alle 10 bis 15 Sekunden Liveupdates bereit, bis der Auftrag abgeschlossen ist.

Zeigen Sie das Jupyter-Widget an, während Sie darauf warten, dass die Ausführung abgeschlossen wird.
    
```python
from azureml.widgets import RunDetails
RunDetails(run).show()
```

![Screenshot des Jupyter-Notebook-Widgets](./media/how-to-track-experiments/run-details-widget.png)

Sie können auch einen Link zur gleichen Anzeige in Ihrem Arbeitsbereich abrufen.

```python
print(run.get_portal_url())
```

## <a name="monitor-automated-machine-learning-runs"></a>Überwachen von Ausführungen des automatisierten maschinellen Lernens

Um bei Ausführungen des automatisierten maschinellen Lernens auf die Diagramme aus einer vorherigen Ausführung zuzugreifen, ersetzen Sie `<<experiment_name>>` durch den Namen des entsprechenden Experiments:

```python
from azureml.widgets import RunDetails
from azureml.core.run import Run

experiment = Experiment (workspace, <<experiment_name>>)
run_id = 'autoML_my_runID' #replace with run_ID
run = Run(experiment, run_id)
RunDetails(run).show()
```

![Jupyter Notebook-Widget für automatisiertes Machine Learning](./media/how-to-track-experiments/azure-machine-learning-auto-ml-widget.png)

## <a name="show-output-upon-completion"></a>Anzeigen der Ausgabe nach Abschluss

Bei der Verwendung von **ScriptRunConfig** können Sie ```run.wait_for_completion(show_output = True)``` einsetzen,um anzuzeigen, wenn das Modelltraining abgeschlossen ist. Mithilfe des ```show_output```-Flags erhalten Sie eine ausführlichen Ausgabe. Weitere Informationen finden Sie im Abschnitt „ScriptRunConfig“ unter [Aktivieren der Protokollierung](how-to-track-experiments.md#scriptrun-logs).

<a id="queryrunmetrics"></a>

## <a name="view-run-metrics"></a>Anzeigen von Ausführungsmetriken

## <a name="via-the-sdk"></a>Über das SDK
Mit ```run.get_metrics()``` können Sie die Metriken eines trainierten Modells anzeigen. Betrachten Sie das folgende Beispiel. 

```python
from azureml.core import Run
run = Run.get_context()
run.log('metric-name', metric_value)

metrics = run.get_metrics()
# metrics is of type Dict[str, List[float]] mapping mertic names
# to a list of the values for that metric in the given run.

metrics.get('metric-name')
# list of metrics in the order they were recorded
```

<a name="view-the-experiment-in-the-web-portal"></a>

## <a name="view-run-records-in-the-studio"></a>Anzeigen von Ausführungsaufzeichnungen in Studio

Sie können abgeschlossene Ausführungsaufzeichnungen, einschließlich protokollierter Metriken, in [Azure Machine Learning Studio](https://ml.azure.com) durchsuchen.

Navigieren Sie zur Registerkarte **Experimente**. Um alle Ihre Ausführungen in Ihrem Arbeitsbereich experimentübergreifend anzuzeigen, wählen Sie die Registerkarte **Alle Ausführungen** aus. Sie können einen Drilldown für die Ausführungen für bestimmte Experimente ausführen, indem Sie den Experimentfilter auf der oberen Menüleiste anwenden.

Wählen Sie für die Ansicht einzelner Experimente die Registerkarte **Alle Experimente** aus. Auf dem Dashboard für die Experimentausführung können Sie nachverfolgte Metriken und Protokolle für jede Ausführung sehen. 

Sie können auch die Tabelle der Ausführungsliste bearbeiten, um mehrere Ausführungen auszuwählen und den letzten, den minimalen oder den maximalen protokollierten Wert für Ihre Ausführungen anzuzeigen. Passen Sie Ihre Diagramme an, um die protokollierten Metrikwerte und Aggregate über mehrere Ausführungen zu vergleichen. 

![Ausführungsdetails im Azure Machine Learning-Studio](media/how-to-track-experiments/experimentation-tab.gif)

### <a name="view-log-files-for-a-run"></a>Anzeigen der Protokolldateien zu einer Ausführung 

Protokolldateien sind eine wichtige Ressource zum Debuggen von Azure Machine Learning-Workloads. Führen Sie einen Drilldown für einen bestimmten Testlauf aus, um die zugehörigen Protokolle und Ausgaben anzuzeigen:  

1. Navigieren Sie zur Registerkarte **Experimente**.
1. Wählen Sie die runID für eine bestimmte Ausführung aus.
1. Wählen Sie oben auf der Seite **Ausgaben und Protokolle** aus.

:::image type="content" source="media/how-to-monitor-view-training-logs/view-logs.png" alt-text="Screenshot des Abschnitts „Ausgaben und Protokolle“ einer Ausführung":::

In den folgenden Tabellen werden die Inhalte der Protokolldateien in den Ordnern aufgelistet, die in diesem Abschnitt angezeigt werden.

> [!NOTE]
> Es ist nicht notwendigerweise jede Datei bei jeder Ausführung vorhanden. Beispielsweise ist die Datei „20_image_build_log*.txt“ nur vorhanden, wenn ein neues Image erstellt wird (z. B. wenn Sie die Umgebung ändern).

#### <a name="azureml-logs-folder"></a>`azureml-logs` "

|Datei  |BESCHREIBUNG  |
|---------|---------|
|20_image_build_log.txt     | Das Buildprotokoll für das Docker-Image der Trainingsumgebung, optional, einmal pro Ausführung. Nur vorhanden, wenn Sie Ihre Umgebung aktualisieren. Andernfalls wird das zwischengespeicherte Image von AML wiederverwendet. Bei erfolgreicher Ausführung enthält sie Registrierungsdetails für das entsprechende Image.         |
|55_azureml-execution-<Knoten-ID>.txt     | stdout-/stderr-Protokoll des Hosttools, einmal pro Knoten. Pullt das Image auf das Computeziel. Beachten Sie, dass dieses Protokoll nur vorhanden ist, nachdem Sie Computeressourcen geschützt haben.         |
|65_job_prep-<Knoten-ID>.txt     |   stdout-/stderr-Protokoll des Skripts zur Auftragsvorbereitung, einmal pro Knoten. Laden Sie Ihren Code auf das Computeziel und in Ihre Datenspeicher herunter (falls angefordert).       |
|70_driver_log(_x).txt      |  stdout-/stderr-Protokoll des AML-Steuerskripts und des Kundentrainingsskripts, einmal pro Prozess. **Dies ist die Standardausgabe Ihres Skripts. Hier werden die Protokolle Ihres Codes (z. B. print-Anweisungen) angezeigt.** In den meisten Fällen überwachen Sie hier die Protokolle.       |
|70_mpi_log.txt     |   Protokoll des MPI-Frameworks, optional, einmal pro Ausführung. Nur bei MPI-Ausführungen.   |
|75_job_post-<Knoten-ID>.txt     |  stdout-/stderr-Protokoll des Skripts zur Auftragsfreigabe, einmal pro Knoten. Senden Sie Protokolle, und geben Sie die Computeressourcen wieder in Azure frei.        |
|process_info.json      |   Zeigt an, welcher Prozess auf welchem Knoten ausgeführt wird.  |
|process_status.json      | Zeigt den Prozessstatus an, d. h., ob ein Prozess nicht gestartet wurde, ausgeführt wird oder abgeschlossen wurde.         |

#### <a name="logs--azureml-folder"></a>`logs > azureml` "

|Datei  |BESCHREIBUNG  |
|---------|---------|
|110_azureml.log      |         |
|job_prep_azureml.log     |   Systemprotokoll für die Auftragsvorbereitung        |
|job_release_azureml.log     | Systemprotokoll für die Auftragsfreigabe        |

#### <a name="logs--azureml--sidecar--node_id-folder"></a>`logs > azureml > sidecar > node_id` "

Wenn Sidecar aktiviert ist, werden die Skripts für die Auftragsvorbereitung und -freigabe im Sidecar-Container ausgeführt.  Es gibt für jeden Knoten einen Ordner. 

|Datei  |BESCHREIBUNG  |
|---------|---------|
|start_cms.txt     |  Protokoll des Prozesses, der beim Start des Sidecar-Containers gestartet wird       |
|prep_cmd.txt      |   Protokoll für ContextManagers, wird beim Ausführen von `job_prep.py` gestartet (teilweise an `azureml-logs/65-job_prep` gestreamt)       |
|release_cmd.txt     |  Protokoll für ComtextManagers, wird beim Ausführen von `job_release.py` beendet        |

#### <a name="other-folders"></a>Andere Ordner

Für Aufträge zum Trainieren auf mehreren Computeclustern sind für jede Knoten-IP-Adresse Protokolle vorhanden. Die Struktur ist bei den einzelnen Knoten identisch mit der bei Aufträgen mit einzelnen Knoten. Es gibt einen zusätzlichen Protokollordner für die Protokolle zur allgemeinen Ausführung und für stderr und stdout.

Azure Machine Learning protokolliert während des Trainings auch Informationen aus zahlreichen Quellen, wie z. B. aus AutoML oder dem Docker-Container, in dem der Trainingsauftrag ausgeführt wird. Viele dieser Protokolle sind nicht dokumentiert. Wenn Sie Probleme haben und sich an den Microsoft-Support wenden, können diese Protokolle möglicherweise bei der Problembehandlung verwendet werden.

## <a name="monitor-a-compute-cluster"></a>Überwachen eines Computeclusters

Führen Sie die folgenden Schritte aus, um die Ausführungen für ein bestimmtes Computeziel in Ihrem Browser zu überwachen:

1. Wählen Sie in [Azure Machine Learning Studio](https://ml.azure.com/) Ihren Arbeitsbereich aus, und wählen Sie dann links auf der Seite __Compute__ aus.

1. Wählen Sie __Trainingscluster__ aus, um eine Liste der für das Training verwendeten Computeziele anzuzeigen. Wählen Sie dann den Cluster aus.

    ![Auswählen des Trainingsclusters](./media/how-to-track-experiments/select-training-compute.png)

1. Wählen Sie __Ausführungen__ aus. Die Liste der Ausführungen wird angezeigt, die diesen Cluster verwenden. Zum Anzeigen von Details für eine bestimmte Ausführung verwenden Sie den Link in der Spalte __Ausführung__. Zum Anzeigen von Details für ein bestimmtes Experiment verwenden Sie den Link in der Spalte __Experiment__.

    ![Auswählen von Ausführungen für Trainingscluster](./media/how-to-track-experiments/show-runs-for-compute.png)
    
    > [!TIP]
    > Da Trainingscomputeziele freigegebene Ressourcen sind, können zu jedem Zeitpunkt mehrere Ausführungen in die Warteschlange eingereiht oder aktiv sein.
    > 
    > Eine Ausführung kann untergeordnete Ausführungen enthalten, sodass ein Trainingsauftrag zu mehreren Einträgen führen kann.

Sobald eine Ausführung abgeschlossen ist, wird sie nicht mehr auf dieser Seite angezeigt. Informationen zu abgeschlossenen Ausführungen finden Sie in Studio im Abschnitt __Experimente__. Wählen Sie dazu das Experiment und die Ausführung aus. Weitere Informationen finden Sie im Abschnitt [Anzeigen von Metriken für abgeschlossene Ausführungen](#view-the-experiment-in-the-web-portal).


## <a name="next-steps"></a>Nächste Schritte

Probieren Sie diese nächsten Schritte aus, um zu erfahren, wie Sie Azure Machine Learning verwenden können:

* Erfahren Sie, wie Sie [Experimente nachverfolgen und Protokolle im Azure Machine Learning-Designer aktivieren](how-to-track-designer-experiments.md).

* Ein Beispiel für die Registrierung des besten Modells und dessen Bereitstellung finden Sie im Tutorial [Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning](tutorial-train-models-with-aml.md).
