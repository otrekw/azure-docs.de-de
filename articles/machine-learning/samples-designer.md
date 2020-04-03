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
ms.date: 03/29/2020
ms.openlocfilehash: f9a8b0a4c51024d91e517db2f6ae10a4dba62384
ms.sourcegitcommit: 0553a8b2f255184d544ab231b231f45caf7bbbb0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2020
ms.locfileid: "80389340"
---
# <a name="designer-sample-pipelines"></a>Beispielpipelines für den Designer

Verwenden Sie die integrierten Beispiele in Azure Machine Learning-Designer, um die Erstellung eigener Machine Learning-Pipelines zu beschleunigen. Das [GitHub-Repository](https://github.com/Azure/MachineLearningDesigner) des Azure Machine Learning-Designers enthält eine ausführliche Dokumentation mit grundlegenden Informationen zu einigen gängigen Machine Learning-Szenarien.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen.
* Ein Azure Machine Learning-Arbeitsbereich mit der Enterprise-SKU.


## <a name="how-to-use-sample-pipelines"></a>Verwenden von Beispielpipelines

Der Designer speichert eine Kopie der Beispielpipelines in Ihrem Studio-Arbeitsbereich. Sie können die Pipeline an Ihre Anforderungen anpassen und als eigene Pipeline speichern. Verwenden Sie sie als Ausgangspunkt, um Ihre Projekte zu beschleunigen.

### <a name="open-a-sample-pipeline"></a>Öffnen einer Beispielpipeline

1. Melden Sie sich bei <a href="https://ml.azure.com?tabs=jre" target="_blank">ml.azure.com</a> an, und wählen Sie den gewünschten Arbeitsbereich aus.

1. Wählen Sie **Designer** aus.

1. Wählen Sie im Abschnitt **Neue Pipeline** eine Beispielpipeline aus.

    Wählen Sie **Mehr Stichproben anzeigen** aus, um eine vollständige Liste mit Beispielen anzuzeigen.

### <a name="submit-a-pipeline-run"></a>Übermitteln einer Pipelineausführung

Zum Ausführen einer Pipeline müssen Sie zuerst das Standardcomputeziel für die Pipelineausführung festlegen.

1. Wählen Sie rechts neben der Canvas im Bereich **Einstellungen** die Option **Computeziel auswählen** aus.

1. Wählen Sie im angezeigten Dialogfeld ein vorhandenes Computeziel aus, oder erstellen Sie ein neues. Wählen Sie **Speichern** aus.

1. Wählen Sie oben auf der Canvas **Übermitteln** aus, um eine Pipelineausführung zu übermitteln.

Abhängig von der Beispielpipeline und den Computeeinstellungen kann die Ausführung einige Zeit in Anspruch nehmen. In den Standardcomputeeinstellungen ist eine minimale Knotengröße von 0 festgelegt. Das bedeutet, dass der Designer Ressourcen nach dem Leerlauf zuordnen muss. Wiederholte Pipelineausführungen werden schneller abgeschlossen, da die Computeressourcen bereits zugeordnet sind. Außerdem verwendet der Designer für jedes Modul zwischengespeicherte Ergebnisse, um die Effizienz weiter zu steigern.


### <a name="review-the-results"></a>Überprüfen der Ergebnisse

Nach Abschluss der Pipelineausführung können Sie die Pipeline überprüfen und die Ausgabe der einzelnen Module anzeigen, um weitere Informationen zu erhalten.

Führen Sie die folgenden Schritte aus, um die Modulausgaben anzuzeigen:

1. Wählen Sie ein Modul auf der Canvas aus.

1. Wählen Sie rechts neben der Canvas im Bereich mit den Moduldetails die Option **Ausgaben und Protokolle** aus. Wählen Sie das Diagrammsymbol ![Visualisierungssymbol](./media/tutorial-designer-automobile-price-train-score/visualize-icon.png) aus, um die Ergebnisse der einzelnen Module anzuzeigen. 

Verwenden Sie die Beispiele als Ausgangspunkt für einige der gängigsten Machine Learning-Szenarien.

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
