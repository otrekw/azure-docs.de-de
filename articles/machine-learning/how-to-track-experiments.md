---
title: Protokollieren von ML-Experimenten und -Metriken
titleSuffix: Azure Machine Learning
description: Aktivieren Sie die Protokollierung für Ihre ML-Trainingsausführungen, um Ausführungsmetriken in Echtzeit zu überwachen und Fehler und Warnungen zu diagnostizieren.
services: machine-learning
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.service: machine-learning
ms.subservice: core
ms.date: 07/30/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 3d970193bd8d73baeac89fb45da4c8a3d81cbde4
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102518398"
---
# <a name="enable-logging-in-ml-training-runs"></a>Aktivieren der Protokollierung in ML-Trainingsausführungen


Mit dem Python-SDK für Azure Machine Learning können Sie Echtzeitinformationen protokollieren, indem Sie sowohl das standardmäßige Python-Protokollierungspaket als auch SDK-spezifische Funktionen verwenden. Sie können lokal protokollieren und Protokolle an Ihren Arbeitsbereich im Portal senden.

Protokolle helfen bei der Diagnose von Fehlern und Warnungen und beim Nachverfolgen von Leistungsmetriken wie Parametern und Modellleistung. In diesem Artikel erfahren Sie, wie Sie die Protokollierung in den folgenden Szenarien aktivieren:

> [!div class="checklist"]
> * Interaktive Trainingssitzungen
> * Übermitteln von Trainingsaufträgen mithilfe von ScriptRunConfig
> * Native `logging`-Einstellungen für Python
> * Protokollierung von zusätzlichen Quellen


> [!TIP]
> In diesem Artikel erfahren Sie, wie Sie den Modelltrainingsprozess überwachen. Wenn Sie sich für die Überwachung der Nutzung und Ereignisse von Azure Machine Learning (z. B. Kontingente, abgeschlossene Trainingsausführungen oder abgeschlossene Modellimplementierungen) interessieren, helfen Ihnen die Informationen im Artikel [Überwachen von Azure Machine Learning](monitor-azure-machine-learning.md) weiter.

## <a name="data-types"></a>Datentypen

Sie können mehrere Datentypen protokollieren, einschließlich skalarer Werte, Listen, Tabellen, Images, Verzeichnisse und mehr. Weitere Informationen und Python-Codebeispiele für verschiedene Datentypen finden Sie auf der [Referenzseite für die Run-Klasse](/python/api/azureml-core/azureml.core.run%28class%29).

### <a name="logging-run-metrics"></a>Protokollieren von Ausführungsmetriken 

Verwenden Sie die folgenden Methoden in den Protokollierungs-APIs, um die Visualisierung Ihrer Metriken anzupassen. Beachten Sie die [Diensteinschränkungen](https://docs.microsoft.com/azure/machine-learning/resource-limits-quotas-capacity#metrics) für diese protokollierten Metriken. 

|Protokollierter Wert|Beispielcode| Formatieren im Portal|
|----|----|----|
|Protokollieren eines Arrays mit numerischen Werten| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|Liniendiagramm mit nur einer Variablen|
|Protokollieren eines einzelnen numerischen Werts mit mehrfacher Verwendung desselben Metriknamens (z.B. in einer for-Schleife)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Liniendiagramm mit nur einer Variablen|
|Wiederholtes Protokollieren einer Zeile mit zwei numerischen Spalten|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|Liniendiagramm mit zwei Variablen|
|Protokollieren einer Tabelle mit zwei numerischen Spalten|`run.log_table(name='Sine Wave', value=sines)`|Liniendiagramm mit zwei Variablen|
|Protokollieren eines Bilds|`run.log_image(name='food', path='./breadpudding.jpg', plot=None, description='desert')`|Verwenden Sie diese Methode, um eine Bilddatei oder einen Matplotlib-Plot für die Ausführung zu protokollieren. Diese Bilder sind in der Ausführungsaufzeichnung sicht- und vergleichbar.|

### <a name="logging-with-mlflow"></a>Protokollieren mit MLflow
Verwenden Sie MLFlowLogger zum Protokollieren von Metriken.

```python
from azureml.core import Run
# connect to the workspace from within your running code
run = Run.get_context()
ws = run.experiment.workspace

# workspace has associated ml-flow-tracking-uri
mlflow_url = ws.get_mlflow_tracking_uri()

#Example: PyTorch Lightning
from pytorch_lightning.loggers import MLFlowLogger

mlf_logger = MLFlowLogger(experiment_name=run.experiment.name, tracking_uri=mlflow_url)
mlf_logger._run_id = run.id
```

## <a name="interactive-logging-session"></a>Interaktive Protokollierungssitzung

Interaktive Protokollierungssitzungen werden in der Regel in Notebook-Umgebungen verwendet. Die Methode [Experiment.start_logging()](/python/api/azureml-core/azureml.core.experiment%28class%29#start-logging--args----kwargs-) startet eine interaktive Protokollierungssitzung. Alle Metriken, die während der Sitzung protokolliert werden, werden der Ausführungsaufzeichnung im Experiment hinzugefügt. Die Methode [run.complete()](/python/api/azureml-core/azureml.core.run%28class%29#complete--set-status-true-) beendet die Sitzungen und markiert die Ausführung als abgeschlossen.

## <a name="scriptrun-logs"></a>ScriptRun-Protokolle

In diesem Abschnitt erfahren Sie, wie Sie Protokollierungscode innerhalb von Ausführungen hinzufügen, die beim Konfigurieren mit ScriptRunConfig erstellt werden. Sie können die Klasse [**ScriptRunConfig**](/python/api/azureml-core/azureml.core.scriptrunconfig) verwenden, um Skripts und Umgebungen für wiederholbare Ausführungen zu kapseln. Sie können diese Option auch verwenden, um ein visuelles Jupyter Notebooks-Widget zur Überwachung anzuzeigen.

Dieses Beispiel führt einen Parameter-Sweep über Alphawerte durch und erfasst die Ergebnisse mit der Methode [run.log()](/python/api/azureml-core/azureml.core.run%28class%29#log-name--value--description----).

1. Erstellen Sie ein Trainingsskript, das die Protokollierungslogik (`train.py`) enthält.

   [!code-python[](~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train.py)]


1. Übermitteln Sie das ```train.py```-Skript, um in einer vom Benutzer verwalteten Umgebung ausgeführt zu werden. Der gesamte Skriptordner wird zum Training übermittelt.

   [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=src)]


   [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=run)]

    Der Parameter `show_output` aktiviert die ausführliche Protokollierung, mit der Sie Details aus dem Trainingsprozess sowie Informationen zu allen entfernten Ressourcen oder Computezielen anzeigen können. Verwenden Sie den folgenden Code, um die ausführliche Protokollierung zu aktivieren, wenn Sie das Experiment übermitteln.

```python
run = exp.submit(src, show_output=True)
```

Sie können denselben Parameter auch in der `wait_for_completion`-Funktion der resultierende Ausführung verwenden.

```python
run.wait_for_completion(show_output=True)
```

## <a name="native-python-logging"></a>Native Python-Protokollierung

Einige Protokolle im SDK können einen Fehler enthalten, der Sie anweist, den Protokolliergrad auf DEBUG festzulegen. Um den Protokolliergrad festzulegen, fügen Sie Ihrem Skript den folgenden Code hinzu.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## <a name="additional-logging-sources"></a>Zusätzliche Protokollierungsquellen

Azure Machine Learning kann während des Trainings auch Informationen aus anderen Quellen protokollieren, etwa aus automatisierten Machine Learning-Ausführungen oder aus Docker-Containern, in denen der Auftrag ausgeführt wird. Diese Protokolle sind nicht dokumentiert, aber wenn Probleme auftreten und Sie sich an den Microsoft-Support wenden, können diese Protokolle möglicherweise bei der Problembehandlung verwendet werden.

Informationen zur Protokollierung von Metriken im Azure Machine Learning-Designer finden Sie unter [Protokollieren von Metriken im Designer](how-to-track-designer-experiments.md)

## <a name="example-notebooks"></a>Beispielnotebooks

Die folgenden Notebooks verdeutlichen die Konzepte in diesem Artikel:
* [how-to-use-azureml/training/train-on-local](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [how-to-use-azureml/track-and-monitor-experiments/logging-api](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie diese Artikel, um mehr über die Verwendung von Azure Machine Learning zu erfahren:

* Erfahren Sie, wie Sie [Metriken in Azure Machine Learning-Designer protokollieren](how-to-track-designer-experiments.md).

* Ein Beispiel für die Registrierung des besten Modells und dessen Bereitstellung finden Sie im Tutorial [Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning](tutorial-train-models-with-aml.md).
