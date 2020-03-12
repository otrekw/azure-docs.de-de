---
title: 'Algorithmen und Module: Referenz'
description: Erfahren Sie mehr über die Module, die in Azure Machine Learning-Designer (Vorschauversion) verfügbar sind.
titleSuffix: Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: peterclu
ms.author: peterlu
ms.date: 02/22/2020
ms.openlocfilehash: 334cf69db70e8a1e0c2994c492f15ff44505af0d
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78673374"
---
# <a name="algorithm--module-reference-for-azure-machine-learning-designer-preview"></a>Algorithmen und Module – Referenz für Azure Machine Learning-Designer (Vorschau)

Dieser Referenzinhalt bietet den technischen Hintergrund zu den einzelnen Machine Learning-Algorithmen und -Modulen, die in Azure Machine Learning-Designer (Vorschauversion) verfügbar sind.

Jedes Modul stellt eine Codegruppe dar, die unabhängig ausgeführt werden und eine Machine Learning-Aufgabe ausführen kann, wenn die erforderlichen Eingaben erfolgen. Die Module enthalten möglicherweise bestimmte Algorithmen oder führen Aufgaben aus, die beim Machine Learning wichtig sind, z.B. Ersatz eines fehlenden Werts oder statistische Analyse.

Hilfe beim Auswählen von Algorithmen finden Sie unter: 
* [Auswählen von Algorithmen](../how-to-select-algorithms.md)
* [Azure Machine Learning – Cheat Sheet für Algorithmen](../algorithm-cheat-sheet.md)

> [!TIP]
> In allen Pipelines im Designer können Sie Informationen zu einem bestimmten Modul abrufen. Wählen Sie das Modul und dann den **Weitere Hilfe**-Link im Bereich **Direkthilfe** aus.

## <a name="data-preparation-modules"></a>Datenaufbereitungsmodule


| Funktionalität | BESCHREIBUNG | Modul |
| --- |--- | --- |
| Dateneingabe und -ausgabe | Verschieben Sie Daten aus Cloudquellen in Ihre Pipeline. Schreiben Sie Ihre Ergebnisse oder Zwischendaten, während Sie eine Pipeline ausführen, in Azure Storage, in eine SQL-Datenbank oder in Hive, oder verwenden Sie Cloudspeicher, um Daten zwischen Pipelines austauschen.  | [Manuelles Eingeben von Daten](enter-data-manually.md) <br/> [Daten exportieren](export-data.md) <br/> [Daten importieren](import-data.md) |
| Datentransformation | Vorgänge für Daten, die für maschinelles Lernen typisch sind, z. B. Normalisieren oder Quantisierung von Daten, Verringerung der Dimensionalität und Konvertierung von Daten zwischen verschiedenen Dateiformaten.| [Hinzufügen von Spalten](add-columns.md) <br/> [Hinzufügen von Zeilen](add-rows.md) <br/> [Anwenden einer mathematischen Operation](apply-math-operation.md) <br/> [Anwenden der SQL-Transformation](apply-sql-transformation.md) <br/> [Bereinigen fehlender Daten](clean-missing-data.md) <br/> [Beschneiden von Werten](clip-values.md) <br/> [Konvertieren in CSV](convert-to-csv.md) <br/> [Konvertieren in ein Dataset](convert-to-dataset.md) <br/> [Konvertieren in Indikatorwerte](convert-to-indicator-values.md) <br/> [Bearbeiten von Metadaten](edit-metadata.md) <br/> [Verknüpfen von Daten](join-data.md) <br/> [Normalisieren von Daten](normalize-data.md) <br/> [Partition und Beispiel](partition-and-sample.md)  <br/> [Entfernen doppelter Zeilen](remove-duplicate-rows.md) <br/> [SMOTE](smote.md) <br/> [Auswählen der Spaltentransformation](select-columns-transform.md) <br/> [Auswählen von Spalten im Dataset](select-columns-in-dataset.md) <br/> [Aufteilen von Daten](split-data.md) |
| Featureauswahl | Wählen Sie eine Teilmenge relevanter, nützlicher Features aus, die beim Erstellen eines analytischen Modells verwendet werden sollen. | [Filterbasierte Featureauswahl](filter-based-feature-selection.md) <br/> [Permutation Feature Importance](permutation-feature-importance.md) |
| Statistische Funktionen | Stellen eine Vielzahl von statistischen Methoden für Data Science bereit. | [Zusammenfassen von Daten](summarize-data.md)|

## <a name="machine-learning-algorithms"></a>Machine Learning-Algorithmen

| Funktionalität | BESCHREIBUNG | Modul |
| --- |--- | --- |
| Regression | Sagen Sie einen Wert vorher. | [Regression bei verstärktem Entscheidungsbaum](boosted-decision-tree-regression.md) <br/> [Entscheidungswaldregression](decision-forest-regression.md) <br/> [Lineare Regression](linear-regression.md)  <br/> [Regression mit neuronalen Netzwerken](neural-network-regression.md)  <br/> |
| Clustering | Gruppieren Sie Daten.| [K-Means-Clustering](k-means-clustering.md)
| Klassifizierung | Sagen Sie eine Klasse vorher.  Wählen Sie aus Binäralgorithmen (zwei Klassen) oder Multiklassenalgorithmen.| [Verstärkte Entscheidungsstruktur mit mehreren Klassen](multiclass-boosted-decision-tree.md) <br/> [Entscheidungswald mit mehreren Klassen](multiclass-decision-forest.md) <br/> [Logistische Regression mit mehreren Klassen](multiclass-logistic-regression.md)  <br/> [Mehrklassiges neuronales Netzwerk](multiclass-neural-network.md) <br/> [One-vs- All-Multiklasse](one-vs-all-multiclass.md) <br/> [Gemitteltes Perzeptron mit zwei Klassen](two-class-averaged-perceptron.md) <br/>  [Verstärkter Entscheidungsbaum mit zwei Klassen](two-class-boosted-decision-tree.md)  <br/> [Entscheidungswald mit zwei Klassen](two-class-decision-forest.md) <br/>  [Logistische Regression mit zwei Klassen](two-class-logistic-regression.md) <br/> [Zweiklassiges neuronales Netzwerk](two-class-neural-network.md) <br/> [Zweiklassige Support Vector Machine](two-class-support-vector-machine.md) | 

## <a name="modules-for-building-and-evaluating-models"></a>Module zum Entwickeln und Auswerten von Modellen

| Funktionalität | BESCHREIBUNG | Modul |
| --- |--- | --- |
| Modelltraining | Führen Sie Daten über den Algorithmus aus. |  [Trainieren des Clusteringmodells](train-clustering-model.md) <br/> [Train Model](train-model.md) (Modell trainieren)  <br/> [Tune Model Hyperparameters](tune-model-hyperparameters.md) |
| Modellbewertung und -auswertung | Bewerten Sie die Genauigkeit des trainierten Modells | [Anwenden der Transformation](apply-transformation.md) <br/> [Assign Data to Clusters](assign-data-to-clusters.md) (Zuweisen von Daten zu Clustern) <br/> [Cross Validate Model](cross-validate-model.md) <br/> [Auswertungsmodell](evaluate-model.md) <br/> [Score Model](score-model.md) (Modell bewerten) |
| Python (Programmiersprache) | Schreiben Sie Code, und betten Sie ihn in ein Modul ein, um Python in Ihre Pipeline zu integrieren. | [Erstellen eines Python-Modells](create-python-model.md) <br/> [Ausführen von Python-Skripts](execute-python-script.md) |
| R-Sprache | Schreiben Sie Code, und betten Sie ihn in ein Modul ein, um R in Ihre Pipeline zu integrieren. | [Ausführen von R-Skripts](execute-r-script.md) |
| Textanalyse | Stellen Sie spezielle Berechnungstools zum Arbeiten mit strukturiertem und unstrukturiertem Text bereit. | [Extrahieren von N-Gramm-Funktionen aus Text](extract-n-gram-features-from-text.md) <br/> [Feature Hashing](feature-hashing.md) <br/> [Vorverarbeiten von Text](preprocess-text.md) |
| Empfehlung | Erstellen Sie Empfehlungsmodelle. | [Evaluate Recommender](evaluate-recommender.md) <br/> [Score SVD Recommender](score-svd-recommender.md) <br/> [Train SVD Recommender](train-SVD-recommender.md) |


## <a name="error-messages"></a>Fehlermeldungen

Erfahren Sie mehr über [Fehlermeldungen und Ausnahmecodes](designer-error-codes.md), die bei der Verwendung von Modulen in Azure Machine Learning-Designer auftreten können.

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Erstellen eines Modells im Designer zum automatischen Vorhersagen von Preisen](../tutorial-designer-automobile-price-train-score.md)
