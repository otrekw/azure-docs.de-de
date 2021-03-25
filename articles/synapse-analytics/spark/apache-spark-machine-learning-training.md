---
title: Trainieren von Machine Learning-Modellen mit Apache Spark
description: Verwenden von Apache Spark in Azure Synapse Analytics zum Trainieren von Machine Learning-Modellen
author: midesa
ms.author: midesa
ms.reviewer: euang
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: machine-learning
ms.date: 09/13/2020
ms.openlocfilehash: 56b9a98eb72b375aacfeb7cb147997028d3d9ba7
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98116805"
---
# <a name="train-machine-learning-models"></a>Trainieren von Machine Learning-Modellen
Apache Spark in Azure Synapse Analytics ermöglicht maschinelles Lernen mit Big Data und bietet so die Chance, wertvolle Erkenntnisse aus großen Mengen strukturierter, unstrukturierter und sich schnell verändernder Daten zu gewinnen. Beim Trainieren von Machine Learning-Modellen mithilfe von Azure Spark in Azure Synapse Analytics gibt es mehrere Optionen: Apache Spark MLlib, Azure Machine Learning und verschiedene andere Open-Source-Bibliotheken. 

## <a name="apache-sparkml-and-mllib"></a>Apache SparkML und MLlib
Apache Spark in Azure Synapse Analytics ist eine der cloudbasierten Apache Spark-Implementierungen von Microsoft. Sie bietet ein einheitliches Open-Source-Framework für die Parallelverarbeitung von Daten, das die arbeitsspeicherinterne Verarbeitung unterstützt, um die Leistung von Big Data-Analysen zu steigern. Die Spark-Verarbeitungs-Engine ist auf Geschwindigkeit, einfache Nutzung und anspruchsvolle Analysen ausgelegt. Dank seiner verteilten In-Memory-Datenverarbeitungsfunktionen eignet sich Spark besonders für iterative Algorithmen beim maschinellen Lernen und für Graphberechnungen. 

Es gibt zwei skalierbare Bibliotheken für maschinelles Lernen, die algorithmische Modellierungsfunktionen in dieser verteilten Umgebung bereitstellen: MLlib und SparkML. MLlib enthält die Original-API, die auf Grundlage von RDDs erstellt wurde. SparkML ist ein neueres Paket, das eine API auf höherer Ebene bereitstellt, die auf Dataframes basiert, um ML-Pipelines zu erstellen. SparkML unterstützt noch nicht alle Features von MLlib, ersetzt jedoch MLlib als Machine Learning-Standardbibliothek in Spark.

> [!NOTE]
> 
> In diesem [Tutorial](../spark/apache-spark-azure-machine-learning-tutorial.md) erfahren Sie, wie Sie ein SparkML-Modell erstellen.

## <a name="popular-libraries"></a>Beliebte Bibliotheken
Jeder Apache Spark-Pool in Azure Synapse Analytics wird mit einer Reihe beliebter, vorab geladener Machine Learning-Bibliotheken bereitgestellt. Diese Bibliotheken enthalten wiederverwendbaren Code, den Sie in Ihren Programmen oder Projekten nutzen können. Zu den relevanten Machine Learning-Bibliotheken, die standardmäßig enthalten sind, gehören die folgenden:
- [Scikit-learn](https://scikit-learn.org/stable/index.html) ist eine der beliebtesten Machine Learning-Bibliotheken für einen Knoten für klassische ML-Algorithmen. Scikit-learn unterstützt die meisten beaufsichtigten und unbeaufsichtigten Lernalgorithmen und kann ebenfalls für Data Mining und Datenanalyse verwendet werden.
  
- [XGBoost](https://xgboost.readthedocs.io/en/latest/) ist eine beliebte Machine Learning-Bibliothek, die optimierte Algorithmen zum Trainieren von Entscheidungsstrukturen und zufälligen Gesamtstrukturen enthält. 
  
- [PyTorch](https://pytorch.org/) & [Tensorflow](https://www.tensorflow.org/) sind leistungsfähige Deep Learning-Bibliotheken von Python. Innerhalb eines Apache Spark-Pools in Azure Synapse Analytics können Sie diese Bibliotheken verwenden, um Modelle mit einzelnen Computern zu erstellen, indem Sie die Anzahl von Executors in Ihrem Pool auf Null festlegen. Auch wenn Apache Spark in dieser Konfiguration nicht funktionsfähig ist, stellt es eine einfache und kostengünstige Möglichkeit dar, Modelle mit einzelnen Computern zu erstellen.

Weitere Informationen zu den verfügbaren Bibliotheken und zugehörigen Versionen finden Sie in der veröffentlichten [Azure Synapse Analytics-Runtime](../spark/apache-spark-version-support.md).

## <a name="mmlspark"></a>MMLSpark
Die Microsoft Machine Learning-Bibliothek für Apache Spark ist [MMLSpark](https://github.com/Azure/mmlspark). Diese Bibliothek ist darauf ausgelegt, die Produktivität von Data Scientists mit Spark zu steigern, die Experimentierrate zu erhöhen und innovative Verfahren für das maschinelle Lernen – einschließlich Deep Learning – in großen Datasets zu nutzen. 

MMLSpark bietet zusätzlich zu den SparkML-APIs auf niedriger Ebene eine weitere Ebene beim Erstellen skalierbarer ML-Modelle, z.B. zum Indizieren von Zeichenfolgen, Umwandeln von Daten in ein von Algorithmen für maschinelles Lernen erwartetes Layout und Zusammenstellen von Featurevektoren. Die MMLSpark-Bibliothek vereinfacht diese und andere gängige Aufgaben zum Erstellen von Modellen in PySpark.

## <a name="automated-ml-in-azure-machine-learning"></a>Automatisiertes maschinelles Lernen in Azure Machine Learning 
Azure Machine Learning ist eine cloudbasierte Umgebung, die Ihnen das Trainieren, Bereitstellen, Automatisieren, Verwalten und Nachverfolgen von Machine Learning-Modellen ermöglicht. Automatisiertes maschinelles Lernen in Azure Machine Learning akzeptiert Trainingsdaten und Konfigurationseinstellungen und durchläuft automatisch Kombinationen aus verschiedenen Methoden, Modellen und Hyperparametereinstellungen zur Normalisierung und Standardisierung von Features, um das beste Modell zu erzielen. 

Durch die Nutzung von automatisiertem ML in Azure Synapse Analytics profitieren Sie von der tief greifenden Integration der verschiedenen Dienste, um die Authentifizierung und das Modelltraining zu vereinfachen. 

> [!NOTE]
> 
> In [diesem Tutorial](./spark/../apache-spark-azure-machine-learning-tutorial.md) erfahren Sie, wie Sie in Azure Machine Learning ein Experiment mit automatisiertem maschinellem Lernen erstellen.

## <a name="azure-cognitive-services"></a>Azure Cognitive Services
[Azure Cognitive Services](../../cognitive-services/what-are-cognitive-services.md) bietet ML-Funktionen, mit denen allgemeine Probleme gelöst werden können, beispielsweise durch Analyse von Texten im Hinblick auf emotionale Stimmungslagen oder durch Analyse von Bildern zum Erkennen von Objekten oder Gesichtern. Sie benötigen keine speziellen Kenntnisse zu maschinellem Lernen oder Data Science, um diese Dienste zu verwenden. Ein Cognitive Service stellt einige oder alle Komponenten einer Machine Learning-Lösung bereit: Daten, Algorithmus und trainiertes Modell. Zur Nutzung dieser Dienste sind allgemeine Kenntnisse über Ihre Daten erforderlich, ohne dass Sie Erfahrung mit maschinellem Lernen oder Data Science benötigen. Sie können dieses vorab trainierten Cognitive Services in Azure Synapse Analytics automatisch verwenden.

## <a name="next-steps"></a>Nächste Schritte
Dieser Artikel bietet eine Übersicht über die verschiedenen Optionen zum Trainieren von ML-Modellen innerhalb von Apache Spark-Pools in Azure Synapse Analytics. In den folgenden Tutorials erfahren Sie mehr über das Modelltraining:

- Ausführen von AutoML-Experimenten mit Azure Machine Learning und Azure Synapse Analytics: [Tutorial zum automatisierten maschinellen Lernen](../spark/apache-spark-azure-machine-learning-tutorial.md) 
- Ausführen von SparkML-Experimenten: [Tutorial zu Apache SparkML](../spark/apache-spark-machine-learning-mllib-notebook.md)
- Anzeigen der Standardbibliotheken: [Azure Synapse Analytics-Runtime](../spark/apache-spark-version-support.md)