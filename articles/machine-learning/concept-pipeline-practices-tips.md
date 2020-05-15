---
title: Iterieren und Weiterentwickeln von Machine Learning-Pipelines
titleSuffix: Azure Machine Learning
description: Muster, Vorgehensweisen und Tipps für schnelles Entwickeln
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: laobri
author: lobrien
ms.date: 05/01/2020
ms.openlocfilehash: 2ea353469ed111eebb591aa6ba86c652683cc2f0
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858183"
---
# <a name="iterating-and-evolving-machine-learning-pipelines"></a>Iterieren und Weiterentwickeln von Machine Learning-Pipelines

Azure Machine Learning-Pipelines bieten eine effiziente Möglichkeit, Ihren Code zu modularisieren, Ergebnisse wiederzuverwenden und Ihre Computeressourcen zu optimieren. Im Folgenden sind einige praktische Tipps und Vorgehensweisen für das Arbeiten mit Pipelines aufgeführt.

## <a name="how-do-you-get-started-with-pipelines"></a>Wie beginnt man, mit Pipelines zu arbeiten?

Es gibt mehrere Möglichkeiten für den Einstieg, wenn Sie bisher noch nicht mit Pipelines gearbeitet haben:

* Wenn Sie am besten lernen, indem Sie über den Erstellungsprozess lesen und das Gelesene nachbilden, ist der Artikel [Erstellen und Ausführen von Machine Learning-Pipelines mit dem Azure Machine Learning-SDK](how-to-create-your-first-pipeline.md) gut für Sie geeignet. 
* Wenn Sie gerne interaktiv mit einem Jupyter-Notebook lernen, ist die im Notebook [Azure Machine Learning-Pipelines: erste Schritte](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-getting-started.ipynb) entwickelte Pipeline möglicherweise die richtige Variante für Sie.
* Wenn Sie den Code-First-Ansatz bevorzugen, ist das Klonen des [Azure Machine Learning-Pipelinedemorepositorys](https://github.com/microsoft/aml-pipelines-demo) ein guter Ausgangspunkt.

## <a name="how-do-you-modularize-pipeline-code"></a>Wie modularisiert man Pipelinecode? 

Module und die Klasse `ModuleStep` bieten Ihnen eine hervorragende Möglichkeit, Ihren ML-Code zu modularisieren. Beachten Sie jedoch, dass ein Wechseln zwischen Pipelineschritten wesentlich teurer ist als ein Funktionsaufruf. Die Frage, die Sie stellen müssen, ist nicht so sehr: „Sind diese Funktionen und Daten konzeptionell anders als die in diesem anderen Abschnitt?“, sondern eher: „Möchte ich, dass diese Funktionen und Daten sich getrennt weiterentwickeln?“ oder: „Ist diese Berechnung teuer, und kann ich die Ausgabe wiederverwenden?“ Weitere Informationen finden Sie im Notebook [Erstellen von Modulen und Modulversionen und deren Verwendung in einer Pipeline mit ModuleStep](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-how-to-use-modulestep.ipynb).

Wie bereits erläutert, ist das Trennen der Datenvorbereitung vom Training eine häufige Verwendungsmöglichkeit. Manchmal ist die Datenvorbereitung so komplex und zeitaufwendig, dass Sie den Prozess möglicherweise in separate Pipelineschritte aufteilen. Weitere Möglichkeiten sind Tests und Analysen nach dem Training. 

## <a name="how-do-you-speed-pipeline-iteration"></a>Wie beschleunigt man das Durchlaufen von Pipelines? 

Zu den gängigen Verfahren für das schnelle Durchlaufen von Pipelines gehören: 

- Klonen der Pipeline (Erstellen einer Kopie) und schnelles erneutes Ausführen der Pipeline
- Durchgehendes Ausführen der Computeinstanz, um die Startzeit einzusparen
- Wenn die Daten und Schritte so konfiguriert sind, dass eine Wiederverwendung möglich ist, kann die Pipeline die Neuberechnung von Daten überspringen, die sich nicht geändert haben.

Wenn Sie ein schnelles Durchlaufen der Pipeline erreichen möchten, können Sie sie klonen, eine Pipeline erstellen und die Pipeline erneut ausführen. Ein weiteres hilfreiches Verfahren ist, Ihre Computeinstanz betriebsbereit zu halten, um so die Kosten für das Starten einer neuen Computeinstanz einzusparen. Wenn Sie einen Schritt so einrichten, dass das Ergebnis einer Ausführung wiederverwendet werden kann, werden die Ergebnisse bei der wiederholten Ausführung nach Möglichkeit wiederverwendet (wenn an den Schritten nichts geändert wurde).

## <a name="how-do-you-collaborate-using-ml-pipelines"></a>Wie funktioniert die Zusammenarbeit mit ML-Pipelines? 

Separate Pipelines sind eine natürliche Einteilung, mit der der Aufwand aufgeteilt werden kann. Mehrere Entwickler oder sogar mehrere Teams können an verschiedenen Schritten arbeiten, solange Einigkeit über die Daten und Argumente besteht, die zwischen den Schritten weitergegeben werden. 

Während der aktiven Entwicklung können Sie Ergebnisse der Ausführung von `PipelineRun` und `StepRun` aus dem Arbeitsbereich abrufen, diese Objekte verwenden, um die endgültige Ausgabe und Zwischenausgabe herunterzuladen, und diese Artefakte für Ihre eigene modularisierte Arbeit verwenden.

## <a name="use-pipelines-to-test-techniques-in-isolation"></a>Verwenden von Pipelines für das isolierte Testen von Verfahren

In der Praxis ist bei ML-Lösungen in der Regel eine beträchtliche Anpassung der einzelnen Schritte erforderlich. Rohdaten müssen häufig in irgendeiner Form vorbereitet werden: gefiltert, transformiert oder erweitert. Die Trainingsprozesse verfügen möglicherweise über mehrere mögliche Architekturen und für Deep Learning über viele mögliche Variationen für Ebenengrößen und Aktivierungsfunktionen. Auch bei einer konsistenten Architektur kann die Hyperparametersuche entscheidende Vorteile mit sich bringen.

Neben Tools wie [AutoML](concept-automated-ml.md) und der [automatischen Hyperparametersuche](how-to-tune-hyperparameters.md) können Pipelines ein wichtiges Tool für A/B-Tests für Lösungen sein. Wenn Sie über mehrere Varianten für Ihre Pipelineschritte verfügen, ist es einfach, separate Ausführungen zu generieren, um die verschiedenen Variationen testen: 

```python
data_preparation_variants = [data1, data2, data3]
data_augmentation_variants = [aug1, aug2]
architecture_variants = [train1, train2, train3]

# Cartesian product
all_variants = np.array(np.meshgrid(data_preparation_variants,data_augmentation_variants,architecture_variants)).T.reshape(-1,3)

pipelines = [Pipeline(workspace=ws, steps=variant.tolist(), description=str(variant)) for variant in all_variants]

```

