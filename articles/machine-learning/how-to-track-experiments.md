---
title: Protokollieren von ML-Experimenten und -Metriken
titleSuffix: Azure Machine Learning
description: Überwachen Sie Ihre ML-Experimente, und überwachen Sie Ausführungsmetriken, um den Modellerstellungsprozess zu verbessern. Fügen Sie Ihrem Trainingsskript Protokollierung hinzu, und zeigen Sie die protokollierten Ergebnisse einer Ausführung an.  Verwenden Sie „run.log“, „Run.start_logging“ oder „ScriptRunConfig“.
services: machine-learning
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: how-to
ms.date: 03/12/2020
ms.custom: seodec18
ms.openlocfilehash: 426c79c19b599127e2235f61e8c917062ede3b79
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84675201"
---
# <a name="monitor-azure-ml-experiment-runs-and-metrics"></a>Überwachen von Azure ML-Experimentausführungen und -metriken
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Verbessern Sie den Modellerstellungsvorgang, indem Sie Ihre Experimente nachverfolgen und Ausführungsmetriken überwachen. In diesem Artikel erfahren Sie, wie Sie in Azure Machine Learning Protokollierungscode zu Ihrem Trainingsskript hinzufügen, eine Experimentausführung übermitteln, diese Ausführung überwachen und die Ergebnisse untersuchen.

> [!NOTE]
> Azure Machine Learning kann während des Trainings auch Informationen aus anderen Quellen protokollieren, etwa aus automatisierten Machine Learning-Ausführungen oder aus dem Docker-Container, in dem der Trainingsauftrag ausgeführt wird. Diese Protokolle sind nicht dokumentiert. Wenn Sie Probleme haben und sich an den Microsoft-Support wenden, können diese Protokolle möglicherweise bei der Problembehandlung verwendet werden.

> [!TIP]
> Die Informationen in diesem Dokument sind hauptsächlich für Datenanalysten und Entwickler gedacht, die den Modelltrainingsprozess überwachen möchten. Wenn Sie Administrator sind und sich für die Überwachung der Nutzung und Ereignisse von Azure Machine Learning (z. B. Kontingente, abgeschlossene Trainingsausführungen oder abgeschlossene Modellimplementierungen) interessieren, helfen Ihnen die Informationen im Artikel [Überwachen von Azure Machine Learning](monitor-azure-machine-learning.md) weiter.

## <a name="available-metrics-to-track"></a>Zur Nachverfolgung verfügbare Metriken

Die folgenden Metriken können während des Trainings eines Experiments zu einem Durchlauf hinzugefügt werden. Ausführliche Informationen dazu, was Sie bei einer Ausführung nachverfolgen können, finden Sie in der [Referenzdokumentation zur Run-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py).

|type| Python-Funktion | Notizen|
|----|:----|:----|
|Skalarwerte |Funktion:<br>`run.log(name, value, description='')`<br><br>Beispiel:<br>run.log("accuracy", 0.95) |Protokollieren Sie einen Zahlen- oder Zeichenfolgenwert für die Ausführung unter dem jeweiligen Namen. Wenn Sie eine Metrik für eine Ausführung protokollieren, wird diese Metrik in der Ausführungsaufzeichnung des Experiments gespeichert.  Sie können dieselbe Metrik innerhalb einer Ausführung mehrmals protokollieren, wobei das Ergebnis als Vektor dieser Metrik betrachtet wird.|
|Listen|Funktion:<br>`run.log_list(name, value, description='')`<br><br>Beispiel:<br>run.log_list("accuracies", [0.6, 0.7, 0.87]) | Protokollieren Sie eine Liste mit Werten für die Ausführung unter dem jeweiligen Namen.|
|Zeile|Funktion:<br>`run.log_row(name, description=None, **kwargs)`<br>Beispiel:<br>run.log_row("Y over X", x=1, y=0.4) | Mit *log_row* wird eine Metrik mit mehreren Spalten erstellt, wie in kwargs beschrieben. Jeder benannte Parameter erzeugt eine Spalte mit dem angegebenen Wert.  *log_row* kann einmal aufgerufen werden, um ein beliebiges Tupel zu protokollieren, oder mehrmals in einer Schleife, um eine vollständige Tabelle zu erzeugen.|
|Tabelle|Funktion:<br>`run.log_table(name, value, description='')`<br><br>Beispiel:<br>run.log_table("Y over X", {"x":[1, 2, 3], "y":[0.6, 0.7, 0.89]}) | Protokollieren Sie ein Wörterbuchobjekt für die Ausführung unter dem jeweiligen Namen. |
|Bilder|Funktion:<br>`run.log_image(name, path=None, plot=None)`<br><br>Beispiel:<br>`run.log_image("ROC", plot=plt)` | Protokollieren Sie ein Image für die Ausführungsaufzeichnung. Verwenden Sie log_image, um eine PNG-Bilddatei oder einen Matplotlib-Plot für die Ausführung zu protokollieren.  Diese Images werden angezeigt und können mit der Ausführungsaufzeichnung verglichen werden.|
|Ausführung kennzeichnen|Funktion:<br>`run.tag(key, value=None)`<br><br>Beispiel:<br>run.tag("selected", "yes") | Kennzeichnen Sie die Ausführung mit einem Zeichenfolgenschlüssel und einem optionalen Zeichenfolgenwert.|
|Datei oder Verzeichnis hochladen|Funktion:<br>`run.upload_file(name, path_or_stream)`<br> <br> Beispiel:<br>run.upload_file("best_model.pkl", "./model.pkl") | Laden Sie eine Datei in die Ausführungsaufzeichnung hoch. Ausführungen erfassen die Datei im angegebenen Ausgabeverzeichnis. Die ist für die meisten Ausführungstypen standardmäßig „./outputs“.  Verwenden Sie upload_file nur, wenn zusätzliche Dateien hochgeladen werden müssen, oder kein Ausgabeverzeichnis angegeben ist. Wir empfehlen, `outputs` zum Namen hinzuzufügen, damit das Hochladen in das Ausgabeverzeichnis erfolgt. Sie können alle Dateien, die dieser Ausführungsaufzeichnung zugeordnet sind, mit `run.get_file_names()` auflisten.|

> [!NOTE]
> Metriken für Skalare, Listen, Zeilen und Tabellen können den Typ „float“, „integer“ oder „string“ haben.

## <a name="choose-a-logging-option"></a>Auswählen einer Protokollierungsoption

Wenn Sie Ihr Experiment nachverfolgen oder überwachen möchten, müssen Sie Code hinzufügen, um die Protokollierung zu starten, wenn Sie die Ausführung übermitteln. Im Folgenden werden Möglichkeiten beschrieben, wie Sie die Übermittlung auslösen können:
* __Run.start_logging__ – Fügen Sie Ihrem Trainingsskript Protokollierungsfunktionen hinzu und starten Sie eine interaktive Protokollierungssitzung im angegebenen Experiment. **Start_logging** erstellt eine interaktive Ausführung für die Verwendung in Szenarien wie z.B. Notebooks. Alle Metriken, die während der Sitzung protokolliert werden, werden der Ausführungsaufzeichnung im Experiment hinzugefügt.
* __ScriptRunConfig__ – Fügen Sie Ihrem Trainingsskript Protokollierungsfunktionen hinzu und laden Sie den gesamten Skriptordner mit der Ausführung.  **ScriptRunConfig** ist eine Klasse für das Einrichten der Konfigurationen für Skriptausführungen. Mit dieser Option können Sie Überwachungscode hinzufügen, um über den Abschluss informiert zu werden oder um ein visuelles Widget zur Überwachung zu erhalten.
* __Designerprotokollierung:__ Mithilfe des Moduls zum __Ausführen des Python-Skripts__ können Sie Protokollierungsfunktionen in einer Drag & Drop-Designerpipeline hinzufügen. Fügen Sie Python-Code hinzu, um Designerexperimente zu protokollieren. 

## <a name="set-up-the-workspace"></a>Arbeitsbereich einrichten
Bevor Sie die Protokollierung hinzufügen und ein Experiment übermitteln, müssen Sie den Arbeitsbereich einrichten.

1. Laden Sie den Arbeitsbereich. Weitere Informationen zum Einrichten der Arbeitsbereichskonfiguration finden Sie in der [Datei mit der Arbeitsbereichskonfiguration](how-to-configure-environment.md#workspace).

[!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb?name=load_ws)]


## <a name="option-1-use-start_logging"></a>Option 1: Verwenden von start_logging

**Start_logging** erstellt eine interaktive Ausführung für die Verwendung in Szenarien wie z.B. Notebooks. Alle Metriken, die während der Sitzung protokolliert werden, werden der Ausführungsaufzeichnung im Experiment hinzugefügt.

Das folgende Beispiel trainiert ein einfaches Sklearn Ridge-Modell lokal in einem lokalen Jupyter-Notebook. Weitere Informationen zur Übermittlung von Experimenten in verschiedenen Umgebungen finden Sie unter [Einrichten und Verwenden von Computezielen für das Modelltraining](https://docs.microsoft.com/azure/machine-learning/how-to-set-up-training-targets).

### <a name="load-the-data"></a>Laden der Daten

In diesem Beispiel wird das Diabetes-Dataset verwendet – ein bekanntes kleines Dataset, das in Scikit-learn enthalten ist. Durch diese Zelle wird das Dataset geladen und nach dem Zufallsprinzip in Trainings- und Testsätze aufgeteilt.

[!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb?name=load_data)]

### <a name="add-tracking"></a>Hinzufügen der Nachverfolgung
Fügen Sie die Experimentnachverfolgung mit dem Azure Machine Learning SDK hinzu und laden Sie ein persistentes Modell in die Ausführungsaufzeichnung des Experiments hoch. Der folgende Code fügt Tags und Protokolle hinzu und lädt eine Modelldatei in die Ausführung des Experiments hoch.

[!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb?name=create_experiment)]

Das Skript endet mit ```run.complete()```, wodurch angegeben wird, dass die Ausführung abgeschlossen ist.  Diese Funktion wird in der Regel in Szenarien mit einem interaktiven Notebook verwendet.

## <a name="option-2-use-scriptrunconfig"></a>Option 2: Verwenden von ScriptRunConfig

[**ScriptRunConfig**](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) ist eine Klasse für das Einrichten der Konfigurationen für Skriptausführungen. Mit dieser Option können Sie Überwachungscode hinzufügen, um über den Abschluss informiert zu werden oder um ein visuelles Widget zur Überwachung zu erhalten.

Dieses Beispiel erweitert das grundlegende sklearn Ridge-Modell von oben. Es führt eine einfache Parametersuche durch, um Alphawerte des Modells zu überfliegen, um Metriken und trainierte Modelle in Ausführungen im Rahmen des Experiments zu erfassen. Das Beispiel wird lokal in einer von Benutzer verwalteten Umgebung ausgeführt. 

1. Erstellen Sie ein Trainingsskript`train.py`.

   [!code-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train.py)]

2. Über diese `train.py`-Skriptreferenz`mylib.py` erhalten Sie eine Liste der Alphawerte, die im Ridge-Modell verwendet werden sollen.

   [!code-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/mylib.py)] 

3. Konfigurieren Sie eine vom Benutzer verwaltete lokale Umgebung.

   [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=user_managed_env)]


4. Übermitteln Sie das ```train.py```-Skript, um in der vom Benutzer verwalteten Umgebung ausgeführt zu werden. Der gesamte Skriptordner wird für das Training übermittelt, einschließlich der ```mylib.py```-Datei.

   [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=src)] [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=run)]

## <a name="option-3-log-designer-experiments"></a>Option 3: Protokollieren von Designerexperimenten

Verwenden Sie das Modul zum __Ausführen des Python-Skripts__, um Ihren Designerexperimenten eine Protokollierungslogik hinzuzufügen. Sie können beliebige Werte mithilfe dieses Workflows protokollieren. Er ist aber besonders dann hilfreich, wenn Sie Metriken aus dem Modell zum __Evaluieren des Modells__ protokollieren möchten, um die Modellleistung für mehrere Ausführungen überwachen zu können.

1. Stellen Sie eine Verbindung vom Modul zum __Ausführen des Python-Skripts__ zur Ausgabe des Moduls zum __Evaluieren des Modells__ her.

    ![Herstellen einer Verbindung vom Modul zum Ausführen des Python-Skripts zum Modul zum Evaluieren des Modells](./media/how-to-track-experiments/designer-logging-pipeline.png)

1. Fügen Sie den folgenden Code in den Code-Editor zum __Ausführen des Python-Skripts__ ein, um den mittleren absoluten Fehler für Ihr trainiertes Modell zu protokollieren:

    ```python
    # dataframe1 contains the values from Evaluate Model
    def azureml_main(dataframe1 = None, dataframe2 = None):
        print(f'Input pandas.DataFrame #1: {dataframe1}')

        from azureml.core import Run

        run = Run.get_context()

        # Log the mean absolute error to the current run to see the metric in the module detail pane.
        run.log(name='Mean_Absolute_Error', value=dataframe1['Mean_Absolute_Error'])

        # Log the mean absolute error to the parent run to see the metric in the run details page.
        # Note: 'run.parent.log()' should not be called multiple times because of performance issues.
        # If repeated calls are necessary, cache 'run.parent' as a local variable and call 'log()' on that variable.
        run.parent.log(name='Mean_Absolute_Error', value=dataframe1['Mean_Absolute_Error'])
    
        return dataframe1,
    ```

## <a name="manage-a-run"></a>Verwalten einer Ausführung

Im Artikel [Starten, Überwachen und Abbrechen von Trainingsausführungen in Python](how-to-manage-runs.md) sind die speziellen Azure Machine Learning-Workflows beschrieben, in denen Sie Experimente verwalten.

## <a name="view-run-details"></a>Anzeigen von Ausführungsdetails

### <a name="view-activequeued-runs-from-the-browser"></a>Anzeigen von aktiven/in der Warteschlange befindlichen Ausführungen über den Browser

Computeziele, die zum Trainieren von Modellen verwendet werden, sind eine freigegebene Ressource. Aus diesem Grund können jederzeit mehrere Ausführungen aktiv oder in die Warteschlange eingereiht sein. Führen Sie die folgenden Schritte aus, um die Ausführungen für ein bestimmtes Computeziel in Ihrem Browser anzuzeigen:

1. Wählen Sie in [Azure Machine Learning Studio](https://ml.azure.com/) Ihren Arbeitsbereich aus, und wählen Sie dann links __Compute__ aus.

1. Wählen Sie __Trainingscluster__ aus, um eine Liste der für das Training verwendeten Computeziele anzuzeigen. Wählen Sie dann den Cluster aus.

    ![Auswählen des Trainingsclusters](./media/how-to-track-experiments/select-training-compute.png)

1. Wählen Sie __Ausführungen__ aus. Die Liste der Ausführungen wird angezeigt, die diesen Cluster verwenden. Zum Anzeigen von Details für eine bestimmte Ausführung verwenden Sie den Link in der Spalte __Ausführung__. Zum Anzeigen von Details für ein bestimmtes Experiment verwenden Sie den Link in der Spalte __Experiment__.

    ![Auswählen von Ausführungen für Trainingscluster](./media/how-to-track-experiments/show-runs-for-compute.png)
    
    > [!TIP]
    > Eine Ausführung kann untergeordnete Ausführungen enthalten, sodass ein Trainingsauftrag zu mehreren Einträgen führen kann.

Sobald eine Ausführung abgeschlossen ist, wird sie nicht mehr auf dieser Seite angezeigt. Informationen zu abgeschlossenen Ausführungen finden Sie in Studio im Abschnitt __Experimente__. Wählen Sie dazu das Experiment und die Ausführung aus. Weitere Informationen finden Sie im Abschnitt [Metriken der Abfrageausführung](#queryrunmetrics).

### <a name="monitor-run-with-jupyter-notebook-widget"></a>Überwachen der Ausführung mit einem Jupyter-Notebook-Widget
Wenn Sie Ausführungen mithilfe der Methode **ScriptRunConfig** übermitteln, können Sie den Fortschritt der Ausführung mit einem [Jupyter-Widget](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) überwachen. Ebenso wie die Übermittlung der Ausführung ist das Widget asynchron und stellt alle 10 bis 15 Sekunden Liveupdates bereit, bis der Auftrag abgeschlossen ist.

1. Zeigen Sie das Jupyter-Widget an, während Sie darauf warten, dass die Ausführung abgeschlossen wird.

   ```python
   from azureml.widgets import RunDetails
   RunDetails(run).show()
   ```

   ![Screenshot des Jupyter-Notebook-Widgets](./media/how-to-track-experiments/run-details-widget.png)

   Sie können auch einen Link zur gleichen Anzeige in Ihrem Arbeitsbereich abrufen.

   ```python
   print(run.get_portal_url())
   ```

2. **[Für automatisierte Machine Learning-Ausführungen]** Für den Zugriff auf die Diagramme aus einer vorherigen Ausführung. Ersetzen Sie `<<experiment_name>>` durch den entsprechenden Experimentnamen:

   ``` 
   from azureml.widgets import RunDetails
   from azureml.core.run import Run

   experiment = Experiment (workspace, <<experiment_name>>)
   run_id = 'autoML_my_runID' #replace with run_ID
   run = Run(experiment, run_id)
   RunDetails(run).show()
   ```

   ![Jupyter Notebook-Widget für automatisiertes Machine Learning](./media/how-to-track-experiments/azure-machine-learning-auto-ml-widget.png)


Zum Anzeigen weiterer Details zu einer Pipeline klicken Sie in der Tabelle auf die Pipeline, die Sie untersuchen möchten. Die Diagramme werden dann in einem Popupfenster aus Azure Machine Learning-Studio gerendert.

### <a name="get-log-results-upon-completion"></a>Abrufen von Protokollergebnissen nach Abschluss

Modelltraining und -überwachung erfolgen im Hintergrund, sodass Sie währenddessen andere Aufgaben ausführen können. Sie können auch warten, bis das Training des Modells abgeschlossen ist, bevor Sie weiteren Code ausführen. Bei der Verwendung von **ScriptRunConfig** können Sie ```run.wait_for_completion(show_output = True)``` einsetzen,um anzuzeigen, wenn das Modelltraining abgeschlossen ist. Mithilfe des ```show_output```-Flags erhalten Sie eine ausführlichen Ausgabe. 

<a id="queryrunmetrics"></a>

### <a name="query-run-metrics"></a>Metriken der Abfrageausführung

Mit ```run.get_metrics()``` können Sie die Metriken eines trainierten Modells anzeigen. Sie können nun alle Metriken abrufen, die im obigen Beispiel protokolliert wurden, um das beste Modell zu ermitteln.

<a name="view-the-experiment-in-the-web-portal"></a>
## <a name="view-the-experiment-in-your-workspace-in-azure-machine-learning-studio"></a>Anzeigen des Experiments in Ihrem Arbeitsbereich in [Azure Machine Learning-Studio](https://ml.azure.com)

Wenn ein Experiment abgeschlossen ist, können Sie zu der aufgezeichneten Ausführungsaufzeichnung des Experiments navigieren. Sie können aus [Azure Machine Learning-Studio](https://ml.azure.com) auf den Verlauf zugreifen.

Navigieren Sie zur Registerkarte „Experimente“, und wählen Sie Ihr Experiment aus. Sie gelangen zum Dashboard „Experimentausführung“, auf dem Sie nachverfolgte Metriken und Diagramme sehen können, die für jede Ausführung protokolliert wurden. 

Sie können die Tabelle der Ausführungsliste bearbeiten, um entweder den letzten, minimalen oder maximalen protokollierten Wert für Ihre Ausführungen anzuzeigen. Sie können in der Ausführungsliste mehrere Ausführungen auswählen oder die Auswahl aufheben, und die ausgewählten Ausführungen werden die Diagramme mit Ihren Daten füllen. Sie können auch neue Diagramme hinzufügen oder Diagramme bearbeiten, um die protokollierten Metriken (Minimum, Maximum, letzte oder alle Werte) über mehrere Ausführungen hinweg zu vergleichen. Sie können Ihre Daten effektiver untersuchen, indem Sie Ihre Diagramme maximieren.

:::image type="content" source="media/how-to-track-experiments/experimentation-tab.gif" alt-text="Ausführungsdetails in Azure Machine Learning Studio":::

Sie können einen Drilldown für eine bestimmte Ausführung ausführen, um deren Ausgaben oder Protokolle anzuzeigen, oder Sie können die Momentaufnahme des von Ihnen übermittelten Experiments herunterladen, damit Sie den Experimentordner mit anderen teilen können.

### <a name="viewing-charts-in-run-details"></a>Anzeigen von Diagrammen in Ausführungsdetails

Es gibt verschiedene Möglichkeiten, wie Sie die Protokollierungs-APIs während einer Ausführung zum Aufzeichnen verschiedener Arten von Metriken und für deren Anzeige als Diagramme in Azure Machine Learning-Studio nutzen können.

|Protokollierter Wert|Beispielcode| Anzeigen im Portal|
|----|----|----|
|Protokollieren eines Arrays mit numerischen Werten| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|Liniendiagramm mit nur einer Variablen|
|Protokollieren eines einzelnen numerischen Werts mit mehrfacher Verwendung desselben Metriknamens (z.B. in einer for-Schleife)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Liniendiagramm mit nur einer Variablen|
|Wiederholtes Protokollieren einer Zeile mit zwei numerischen Spalten|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|Liniendiagramm mit zwei Variablen|
|Protokollieren einer Tabelle mit zwei numerischen Spalten|`run.log_table(name='Sine Wave', value=sines)`|Liniendiagramm mit zwei Variablen|


## <a name="example-notebooks"></a>Beispielnotebooks
Die folgenden Notebooks verdeutlichen die Konzepte in diesem Artikel:
* [how-to-use-azureml/training/train-within-notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook)
* [how-to-use-azureml/training/train-on-local](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [how-to-use-azureml/track-and-monitor-experiments/logging-api](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie diese nächsten Schritte aus, um zu erfahren, wie Sie das Azure Machine Learning SDK für Python verwenden können:

* Ein Beispiel für die Registrierung des besten Modells und dessen Bereitstellung finden Sie im Tutorial [Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning](tutorial-train-models-with-aml.md).

* Erfahren Sie mehr zum [Trainieren von PyTorch-Modellen mit Azure Machine Learning](how-to-train-pytorch.md).
