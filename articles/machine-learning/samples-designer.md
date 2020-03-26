---
title: Beispiele für Designerpipelines
titleSuffix: Azure Machine Learning
description: Beschleunigen Sie den Start Ihrer Machine Learning-Pipelines mithilfe von Beispielen im Azure Machine Learning-Designer.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 03/10/2020
ms.openlocfilehash: 82476b9cc8d92c815df602496ed3dcb33014a4fd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "79037296"
---
# <a name="designer-sample-pipelines"></a>Beispielpipelines für den Designer

Verwenden Sie die integrierten Beispiele in Azure Machine Learning-Designer, um die Erstellung eigener Machine Learning-Pipelines zu beschleunigen. Das [GitHub-Repository](https://github.com/Azure/MachineLearningDesigner) des Azure Machine Learning-Designers enthält eine ausführliche Dokumentation mit grundlegenden Informationen zu einigen gängigen Machine Learning-Szenarien.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen.
* Ein Azure Machine Learning-Arbeitsbereich mit der Enterprise-SKU.


## <a name="how-to-use-sample-pipelines"></a>Verwenden von Beispielpipelines

Der Designer speichert eine Kopie der Beispielpipelines in Ihrem Studio-Arbeitsbereich. Sie können die Pipeline an Ihre Anforderungen anpassen und als eigene Pipeline speichern. Verwenden Sie sie als Ausgangspunkt, um Ihre Projekte zu beschleunigen.

1. Melden Sie sich bei <a href="https://ml.azure.com?tabs=jre" target="_blank">ml.azure.com</a> an, und wählen Sie den gewünschten Arbeitsbereich aus.

1. Wählen Sie **Designer** aus.

1. Wählen Sie im Abschnitt **Neue Pipeline** eine Beispielpipeline aus.

    Wählen Sie **Mehr Stichproben anzeigen** aus, um eine vollständige Liste mit Beispielen anzuzeigen.

## <a name="regression-samples"></a>Regressionsbeispiele

Hier finden Sie weitere Informationen zu den integrierten Regressionsbeispielen.

| Beispieltitel | BESCHREIBUNG | 
| --- | --- |
| [Sample 1: Regression - Automobile Price Prediction (Basic)](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-regression-automobile-price-basic.md) (Beispiel 1: Regression: Automobilpreisvorhersage (Standard)) | Dient zum Vorhersagen von Automobilpreisen mittels linearer Regression. |
| [Sample 2: Regression - Automobile Price Prediction (Advanced)](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-regression-automobile-price-compare-algorithms.md) (Beispiel 2: Regression: Automobilpreisvorhersage (Erweitert)) | Dient zum Vorhersagen von Automobilpreisen mittels Entscheidungsstruktur und Boosted Decision Tree-Regressoren. Vergleichen Sie die Modelle, um den besten Algorithmus zu ermitteln.

## <a name="classification-samples"></a>Klassifizierungsbeispiele

Hier finden Sie weitere Informationen zu den integrierten Klassifizierungsbeispielen. Anstelle der Dokumentationslinks können Sie auch die Beispiele öffnen und sich die Modulkommentare ansehen, um mehr über die Beispiele zu erfahren.

| Beispieltitel | BESCHREIBUNG | 
| --- | --- |
| [Sample 3: Binary Classification with Feature Selection - Income Prediction](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-predict-income.md) (Beispiel 3: Binäre Klassifizierung mit Featureauswahl: Vorhersage des Einkommens) | Dient zum Vorhersagen des Einkommens (hoch oder gering) mittels Two-Class Boosted Decision Tree. Für die Featureauswahl wird die Pearson-Korrelation verwendet.
| [Sample 4: Binary Classification with custom Python script - Credit Risk Prediction](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-credit-risk-cost-sensitive.md) (Beispiel 4: Binäre Klassifizierung mit benutzerdefiniertem Python-Skript: Vorhersage des Kreditrisikos) | Dient zum Klassifizieren des Risikos von Kreditanträgen (hoch oder gering). Zur Gewichtung Ihrer Daten wird das Modul „Python-Skript ausführen“ verwendet.
| [Sample 5: Binary Classification - Customer Relationship Prediction](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-churn.md) (Beispiel 5: Binäre Klassifizierung: Vorhersage von Kundenbeziehungen) | Dient zum Vorhersagen der Kundenabwanderung mittels Two-Class Boosted Decision Tree. Für das Sampling verzerrter Daten wird SMOTE verwendet.
| [Sample 7: Text Classification - Wikipedia SP 500 Dataset](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-text-classification.md) (Beispiel 7: Textklassifizierung: Wikipedia SP 500-Dataset) | Dient zum Klassifizieren von Unternehmenstypen aus Wikipedia-Artikeln mittels logistischer Regression mit mehreren Klassen. |
| Beispiel 12: Klassifizierung mit mehreren Klassen: Buchstabenerkennung | Dient zum Erstellen eines Ensembles von binären Klassifizierern für die Klassifizierung geschriebener Buchstaben. |

## <a name="recommender-samples"></a>Empfehlungsbeispiele

Hier finden Sie weitere Informationen zu den integrierten Empfehlungsbeispielen. Anstelle der Dokumentationslinks können Sie auch die Beispiele öffnen und sich die Modulkommentare ansehen, um mehr über die Beispiele zu erfahren.

| Beispieltitel | BESCHREIBUNG | 
| --- | --- |
| Beispiel 10: Empfehlung: Filmbewertungstweets | Dient zum Erstellen einer Filmempfehlungsengine auf der Grundlage von Filmtiteln und -bewertungen. |

## <a name="utility-samples"></a>Hilfsprogrammbeispiele

Hier finden Sie weitere Informationen zu den Beispielen, mit denen ML-Hilfsprogramme und -Features veranschaulicht werden. Anstelle der Dokumentationslinks können Sie auch die Beispiele öffnen und sich die Modulkommentare ansehen, um mehr über die Beispiele zu erfahren.

| Beispieltitel | BESCHREIBUNG | 
| --- | --- |
| [Sample 6: Use custom R script - Flight Delay Prediction](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-flight-delay.md) (Beispiel 6: Verwenden eines benutzerdefinierten R-Skripts: Vorhersage von Flugverspätungen) |
| Beispiel 8: Kreuzvalidierung für binäre Klassifizierung: Vorhersage des Einkommens von Erwachsenen | Dient zum Erstellen eines binären Klassifizierers für das Einkommen von Erwachsenen mittels Kreuzvalidierung.
| Beispiel 9: Permutation Feature Importance (PFI) | Dient zum Berechnen von Wichtigkeitsbewertungen für das Testdataset mittels Permutation Feature Importance. 
| Beispiel 11: Optimierungsparameter für binäre Klassifizierung: Vorhersage des Einkommens von Erwachsenen | Dient zum Ermitteln optimaler Hyperparameter für die Erstellung eines binären Klassifizierers unter Verwendung von „Tune Model Hyperparameters“. |

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Erstellen und Bereitstellen von Machine Learning-Modellen finden Sie unter [Tutorial: Prognostizieren von Automobilpreisen mit dem Designer](tutorial-designer-automobile-price-train-score.md).
