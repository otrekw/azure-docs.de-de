---
title: Verstehen von automatisierten ML-Ergebnissen
titleSuffix: Azure Machine Learning
description: Es wird beschrieben, wie Sie Diagramme und Metriken für Ihre verschiedenen automatisierten Machine Learning-Ausführungen anzeigen und sich damit vertraut machen.
services: machine-learning
author: RachelKellam
ms.author: rakellam
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 12/05/2019
ms.openlocfilehash: 69cf79f8258f85f2fb5e787f91aa843837d0a3a1
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/28/2019
ms.locfileid: "75534694"
---
# <a name="understand-automated-machine-learning-results"></a>Grundlagen von Ergebnissen des automatisierten maschinellen Lernens
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel wird beschrieben, wie Sie Diagramme und Metriken für Ihre verschiedenen automatisierten Machine Learning-Ausführungen anzeigen und sich damit vertraut machen. 

Weitere Informationen:
+ [Metriken, Diagramme und Kurven für Klassifizierungsmodelle](#classification)
+ [Metriken, Diagramme und Graphen für Regressionsmodelle](#regression)
+ [Interpretierbarkeit von Modellen und Featurepriorität](#explain-model)

## <a name="prerequisites"></a>Voraussetzungen

* ein Azure-Abonnement Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

* Erstellen Sie ein Experiment für Ihre Ausführung des automatisierten maschinellen Lernens, entweder mit dem SDK oder in Azure Machine Learning-Studio.

    * Verwenden Sie das SDK zum Erstellen eines [Klassifizierungsmodells](how-to-auto-train-remote.md) oder [Regressionsmodells](tutorial-auto-train-models.md)
    * Verwenden Sie [Azure Machine Learning-Studio](how-to-create-portal-experiments.md), um ein Klassifizierungs- oder Regressionsmodell zu erstellen, indem Sie die entsprechenden Daten hochladen.

## <a name="view-the-run"></a>Anzeigen der Ausführung

Nach dem Ausführen eines Experiments für automatisiertes maschinelles Lernen finden Sie einen Verlauf der Ausführungen in Ihrem Machine Learning-Arbeitsbereich. 

1. Wechseln Sie zu Ihrem Arbeitsbereich.

1. Wählen Sie links im Arbeitsbereich die Option **Experimente**.

   ![Screenshot des Menüs „Experiment“](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-menu.png)

1. Wählen Sie in der Liste mit den Experimenten den gewünschten Eintrag aus.

   [![Experimentliste](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list-expanded.png)

1. Wählen Sie in der unteren Tabelle die Option **Ausführen** aus.

   [![Experimentausführung](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run-expanded.png)

1. Wählen Sie in den Modellen den **Algorithmusnamen** für das Modell aus, das Sie näher untersuchen möchten.

   [![Experimentmodell](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model-expanded.png)

Diese Ergebnisse werden auch während einer Ausführung angezeigt, wenn Sie das `RunDetails`[Jupyter-Widget](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) verwenden.

## <a name="classification"></a> Klassifizierungsergebnisse

Die folgenden Metriken und Diagramme sind für jedes Klassifizierungsmodell verfügbar, das Sie mit den automatisierten Machine Learning-Funktionen von Azure Machine Learning erstellen:

+ [Metriken](#classification-metrics)
+ [Konfusionsmatrix](#confusion-matrix)
+ [Genauigkeit-Trefferquote-Diagramm](#precision-recall-chart)
+ [ROC-Kurve (Receiver Operating Characteristics)](#roc)
+ [Prognosegütekurve](#lift-curve)
+ [Gewinnkurve](#gains-curve)
+ [Kalibrierungsdiagramm](#calibration-plot)

### <a name="classification-metrics"></a>Klassifizierungsmetrik

Bei jeder Ausführungsiteration werden die unten angegebenen Metriken für eine Klassifizierungsaufgabe gespeichert.

Metrik|BESCHREIBUNG|Berechnung|Zusätzliche Parameter
--|--|--|--
AUC_Macro| AUC ist die Fläche unter der ROC-Kurve (Receiver Operating Characteristic Curve). Macro ist das arithmetische Mittel der AUC für jede Klasse.  | [Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average="macro"|
AUC_Micro| AUC ist die Fläche unter der ROC-Kurve (Receiver Operating Characteristic Curve). „Micro“ wird global durch die Kombination der echt positiven und der falsch positiven Ergebnisse aus jeder Klasse berechnet.| [Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average="micro"|
AUC_Weighted  | AUC ist die Fläche unter der ROC-Kurve (Receiver Operating Characteristic Curve). Der gewichtete Wert ist das arithmetische Mittel des Ergebnisses für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.| [Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|average="weighted"
accuracy|Die Genauigkeit ist der Prozentsatz der prognostizierten Bezeichnungen, die den TRUE-Bezeichnungen genau entsprechen. |[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |Keine|
average_precision_score_macro|Die durchschnittliche Genauigkeit fasst eine Precision-Recall-Kurve als gewichteten Mittelwert der bei jedem Schwellenwert erzielten Genauigkeiten zusammen, wobei die Zunahme beim Recall aus dem vorherigen Schwellenwert als Gewichtung verwendet wird. „Macro“ ist das arithmetische Mittel des durchschnittlichen Genauigkeitswerts jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="macro"|
average_precision_score_micro|Die durchschnittliche Genauigkeit fasst eine Precision-Recall-Kurve als gewichteten Mittelwert der bei jedem Schwellenwert erzielten Genauigkeiten zusammen, wobei die Zunahme beim Recall aus dem vorherigen Schwellenwert als Gewichtung verwendet wird. „Micro“ wird global durch Kombinieren der echt positiven und der falsch positiven Ergebnisse bei jedem Cutoff berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="micro"|
average_precision_score_weighted|Die durchschnittliche Genauigkeit fasst eine Precision-Recall-Kurve als gewichteten Mittelwert der bei jedem Schwellenwert erzielten Genauigkeiten zusammen, wobei die Zunahme beim Recall aus dem vorherigen Schwellenwert als Gewichtung verwendet wird. Der gewichtete Wert ist das arithmetische Mittel der durchschnittlichen Genauigkeit für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="weighted"|
balanced_accuracy|„Balanced accuracy“ ist das arithmetische Mittel des Recalls für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
f1_score_macro|„F1 score“ ist das harmonische Mittel aus Genauigkeit und Recall. „Macro“ ist das arithmetische Mittel des F1-Ergebnisses für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="macro"|
f1_score_micro|„F1 score“ ist das harmonische Mittel aus Genauigkeit und Recall. „Micro“ wird global durch Zählen der insgesamt echt positiven, falsch negativen und falsch positiven Ergebnisse berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="micro"|
f1_score_weighted|„F1 score“ ist das harmonische Mittel aus Genauigkeit und Recall. Gewichteter Mittelwert nach Klassenhäufigkeit des F1-Ergebnisses für jede Klasse|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="weighted"|
log_loss|Dies ist die Verlustfunktion, die bei der (multinomialen) logistischen Regression und deren Erweiterungen wie z. B. neuronalen Netzen verwendet wird. Sie ist definiert als die Negativ-Log-Wahrscheinlichkeit der True-Bezeichnungen bei den Vorhersagen eines probabilistischen Klassifizierers. Für eine einzelne Stichprobe mit der TRUE-Bezeichnung „yt“ in {0,1} und der geschätzten Wahrscheinlichkeit yp, dass yt = 1 lautet der logarithmische Verlust -log P(yt&#124;yp) = -(yt log(yp) + (1 - yt) log(1 - yp)).|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|Keine|
norm_macro_recall|Der normalisierte Makro-Recall wird normalisiert, damit die zufällige Leistung ein Ergebnis von 0 und die ideale Leistung einen Wert von 1 liefert. Dies kann erzielt werden durch norm_macro_recall := (recall_score_macro - R)/(1 - R), wobei R der erwartete Wert von recall_score_macro für zufällige Vorhersagen ist (d. h. R=0,5 für die binäre Klassifizierung und R=(1/C) für C-Klassen-Klassifizierungsprobleme).|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average = "macro" |
precision_score_macro|Die Genauigkeit ist der Prozentsatz der positiv vorhergesagten Elemente, die richtig bezeichnet sind. „Macro“ ist das arithmetische Mittel der Genauigkeit für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="macro"|
precision_score_micro|Die Genauigkeit ist der Prozentsatz der positiv vorhergesagten Elemente, die richtig bezeichnet sind. „Micro“ wird global durch Zählen der insgesamt echt positiven und falsch positiven Ergebnisse berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="micro"|
precision_score_weighted|Die Genauigkeit ist der Prozentsatz der positiv vorhergesagten Elemente, die richtig bezeichnet sind. Der gewichtete Wert ist das arithmetische Mittel der Genauigkeit für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="weighted"|
recall_score_macro|Der Abruf ist der prozentuale Anteil der richtig bezeichneten Elemente in einer bestimmten Klasse. „Macro“ ist das arithmetische Mittel des Recalls für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
recall_score_micro|Der Abruf ist der prozentuale Anteil der richtig bezeichneten Elemente in einer bestimmten Klasse. „Micro“ wird global durch Zählen der insgesamt echt positiven, falsch negativen und falsch positiven Ergebnisse berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="micro"|
recall_score_weighted|Der Abruf ist der prozentuale Anteil der richtig bezeichneten Elemente in einer bestimmten Klasse. Der gewichtete Wert ist das arithmetische Mittel des Recalls für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="weighted"|
weighted_accuracy|Die gewichtete Genauigkeit ist die Genauigkeit, bei der die Gewichtung, die jedem Beispiel zugewiesen wird, gleich dem Anteil der TRUE-Instanzen in der TRUE-Klasse dieses Beispiels ist.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight ist ein Vektor gleich dem Anteil der betreffenden Klasse für jedes Element im Ziel.|

<a name="confusion-matrix"></a>

### <a name="confusion-matrix"></a>Konfusionsmatrix

#### <a name="what-is-a-confusion-matrix"></a>Was ist eine Konfusionsmatrix?
Eine Konfusionsmatrix wird verwendet, um die Leistung eines Klassifizierungsmodells zu beschreiben. Jede Zeile zeigt die Instanzen der wahren bzw. tatsächlichen Klasse in Ihrem Dataset an, und jede Spalte stellt die Instanzen der Klasse dar, die vom Modell vorhergesagt wurde. 

#### <a name="what-does-automated-ml-do-with-the-confusion-matrix"></a>Was macht automatisiertes ML mit der Konfusionsmatrix?
Für Klassifizierungsprobleme stellt Azure Machine Learning automatisch eine Konfusionsmatrix für jedes Modell zur Verfügung, das erstellt wird. Von automatisiertem Machine Learning wird für jede Konfusionsmatrix die Häufigkeit der einzelnen vorhergesagten Bezeichnungen (Spalte) im Vergleich mit der tatsächlichen Bezeichnung (Zeile) angezeigt. Je dunkler die Farbe, desto höher die Anzahl in diesem bestimmten Teil der Matrix. 

#### <a name="what-does-a-good-model-look-like"></a>Wie sieht ein gutes Modell aus?
Wir vergleichen den tatsächlichen Wert des Datasets mit den vorhergesagten Werten, die das Modell geliefert hat. Aus diesem Grund haben Machine Learning-Modelle eine höhere Genauigkeit, wenn die meisten Werte des Modells entlang der Diagonalen liegen, was bedeutet, dass das Modell den richtigen Wert vorhergesagt hat. Wenn ein Modell ein Klassenungleichgewicht aufweist, hilft die Konfusionsmatrix dabei, ein verzerrtes Modell zu erkennen.

##### <a name="example-1-a-classification-model-with-poor-accuracy"></a>Beispiel 1: Ein Klassifizierungsmodell mit schlechter Genauigkeit.
![Ein Klassifizierungsmodell mit schlechter Genauigkeit.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix1.png)

##### <a name="example-2-a-classification-model-with-high-accuracy"></a>Beispiel 2: Ein Klassifizierungsmodell mit hoher Genauigkeit. 
![Ein Klassifizierungsmodell mit hoher Genauigkeit.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix2.png)

##### <a name="example-3-a-classification-model-with-high-accuracy-and-high-bias-in-model-predictions"></a>Beispiel 3: Ein Klassifizierungsmodell mit hoher Genauigkeit und hoher Verzerrung bei Modellvorhersagen.
![Ein Klassifizierungsmodell mit hoher Genauigkeit und hoher Verzerrung bei Modellvorhersagen.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-biased-model.png)

<a name="precision-recall-chart"></a>
### <a name="precision-recall-chart"></a>Genauigkeit-Trefferquote-Diagramm
#### <a name="what-is-a-precision-recall-chart"></a>Was ist ein Genauigkeit-Trefferquote-Diagramm?
Die Genauigkeit-Trefferquote-Kurve zeigt die Beziehung zwischen Genauigkeit und Trefferquote eines Modells. Der Begriff „Genauigkeit“ steht für die Fähigkeit eines Modells, alle Instanzen richtig zu bezeichnen. „Trefferquote“ stellt die Möglichkeit für einen Klassifizierer dar, alle Instanzen einer bestimmten Bezeichnung zu finden.

#### <a name="what-does-automated-ml-do-with-the-precision-recall-chart"></a>Was macht automatisiertes ML mit dem Genauigkeit-Trefferquote-Diagramm?

Mit diesem Diagramm können Sie die Genauigkeit-Trefferquote-Kurven für jedes Modell vergleichen, um festzustellen, welches Modell eine akzeptable Beziehung zwischen Genauigkeit und Trefferquote für Ihr spezielles Geschäftsproblem aufweist. Dieses Diagramm zeigt die durchschnittliche Makro-Genauigkeit-Trefferquote, die durchschnittliche Mikro-Genauigkeit-Trefferquote und die Genauigkeit-Trefferquote, die allen Klassen für ein Modell zugeordnet ist. 

Der Makrodurchschnitt berechnet die Metrik unabhängig für jede Klasse und nimmt dann den Durchschnitt, sodass alle Klassen gleich behandelt werden. Der Mikrodurchschnitt aggregiert jedoch die Beiträge aller Klassen, um den Durchschnitt zu berechnen. Der Mikrodurchschnitt ist vorzuziehen, wenn im Dataset ein Klassenungleichgewicht vorhanden ist.

#### <a name="what-does-a-good-model-look-like"></a>Wie sieht ein gutes Modell aus?
Abhängig vom Ziel des Geschäftsproblems kann sich die ideale Genauigkeit-Trefferquote-Kurve unterscheiden. Im Folgenden finden Sie einige Beispiele

##### <a name="example-1-a-classification-model-with-low-precision-and-low-recall"></a>Beispiel 1: Ein Klassifizierungsmodell mit niedriger Genauigkeit und niedriger Trefferquote.
![Ein Klassifizierungsmodell mit niedriger Genauigkeit und niedriger Trefferquote.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall1.png)

##### <a name="example-2-a-classification-model-with-100-precision-and-100-recall"></a>Beispiel 2: Ein Klassifizierungsmodell mit ~100 % Genauigkeit und ~100 % Trefferquote. 
![Ein Klassifizierungsmodell mit hoher Genauigkeit und hoher Trefferquote.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall2.png)
<a name="roc"></a>
### <a name="roc-chart"></a>ROC-Diagramm

#### <a name="what-is-a-roc-chart"></a>Was ist ein ROC-Diagramm?
Receiver Operating Characteristic (oder ROC) ist ein Diagramm der richtig klassifizierten Bezeichnungen im Vergleich zu den falsch klassifizierten Bezeichnungen für ein bestimmtes Modell. Die ROC-Kurve kann weniger aussagekräftig sein, wenn Modelle mit Datasets mit einem großen Bias trainiert werden, da sie die falsch-positiven Bezeichnungen nicht anzeigt.

#### <a name="what-does-automated-ml-do-with-the-roc-chart"></a>Was macht automatisiertes ML mit dem ROC-Diagramm?
Automatisiertes ML generiert die durchschnittliche Makro-Genauigkeit-Trefferquote, die durchschnittliche Mikro-Genauigkeit-Trefferquote und die Genauigkeit-Trefferquote, die allen Klassen für ein Modell zugeordnet ist. 

Der Makrodurchschnitt berechnet die Metrik unabhängig für jede Klasse und nimmt dann den Durchschnitt, sodass alle Klassen gleich behandelt werden. Der Mikrodurchschnitt aggregiert jedoch die Beiträge aller Klassen, um den Durchschnitt zu berechnen. Der Mikrodurchschnitt ist vorzuziehen, wenn im Dataset ein Klassenungleichgewicht vorhanden ist.

#### <a name="what-does-a-good-model-look-like"></a>Wie sieht ein gutes Modell aus?
Im Idealfall weist das Modell eine True Positive-Rate in der Nähe von 100 % und eine False Positive-Rate in der Nähe von 0 % auf. 

##### <a name="example-1-a-classification-model-with-low-true-labels-and-high-false-labels"></a>Beispiel 1: Ein Klassifizierungsmodell mit niedrigen wahren Bezeichnungen und hohen falschen Bezeichnungen.
![Ein Klassifizierungsmodell mit niedrigen wahren Bezeichnungen und hohen falschen Bezeichnungen.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-1.png)

##### <a name="example-2-a-classification-model-with-high-true-labels-and-low-false-labels"></a>Beispiel 2: Ein Klassifizierungsmodell mit hohen wahren Bezeichnungen und niedrigen falschen Bezeichnungen.
![Ein Klassifizierungsmodell mit hohen wahren Bezeichnungen und niedrigen falschen Bezeichnungen.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-2.png)
<a name="lift-curve"></a>
### <a name="lift-chart"></a>Prognosegütediagramm
#### <a name="what-is-a-lift-chart"></a>Was ist ein Prognosegütediagramm?
Prognosegütediagramme werden verwendet, um die Leistung eines Klassifizierungsmodells auszuwerten. Sie zeigen, wie viel bessere Ergebnisse hinsichtlich der Genauigkeit Sie bei der Verwendung des generierten Modells im Vergleich dazu erwarten können, wenn kein Modell verwendet wird.
#### <a name="what-does-automated-ml-do-with-the-lift-chart"></a>Was macht automatisiertes ML mit dem Prognosegütediagramm?
Sie können die Prognosegüte des automatisch mit Azure Machine Learning erstellten Modells mit der Baseline vergleichen, um den Wertzuwachs dieses bestimmten Modells anzuzeigen.
#### <a name="what-does-a-good-model-look-like"></a>Wie sieht ein gutes Modell aus?

##### <a name="example-1-a-classification-model-that-does-worse-than-a-random-selection-model"></a>Beispiel 1: Ein Klassifizierungsmodell, das schlechter abschneidet als ein Zufallsauswahlmodell.
![Ein Klassifizierungsmodell, das schlechter abschneidet als ein Zufallsauswahlmodell.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve1.png)
##### <a name="example-2-a-classification-model-that-performs-better-than-a-random-selection-model"></a>Beispiel 2: Ein Klassifizierungsmodell, das besser abschneidet als ein Zufallsauswahlmodell.
![Ein Klassifizierungsmodell, das besser abschneidet.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve2.png)
<a name="gains-curve"></a>
### <a name="gains-chart"></a>Gewinndiagramm
#### <a name="what-is-a-gains-chart"></a>Was ist ein Gewinndiagramm?

Ein Gewinndiagramm wertet die Leistung eines Klassifizierungsmodells anhand jedes Teils der Daten aus. Es zeigt für jedes Perzentil des Datensatzes an, wie viel besser Sie im Vergleich zu einem Zufallsauswahlmodell abschneiden können.

#### <a name="what-does-automated-ml-do-with-the-gains-chart"></a>Was macht automatisiertes ML mit dem Gewinndiagramm?
Verwenden Sie das kumulierte Gewinndiagramm, um die Auswahl der Klassifizierungsgrenze anhand eines Prozentsatzes zu erleichtern, der einem gewünschten Gewinn aus dem Modell entspricht. Diese Informationen bieten eine weitere Möglichkeit, die Ergebnisse im zugehörigen Prognosegütediagramm zu untersuchen.

#### <a name="what-does-a-good-model-look-like"></a>Wie sieht ein gutes Modell aus?
##### <a name="example-1-a-classification-model-with-minimal-gain"></a>Beispiel 1: Ein Klassifizierungsmodell mit minimalem Gewinn.
![Ein Klassifizierungsmodell mit minimalem Gewinn.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve1.png)

##### <a name="example-2-a-classification-model-with-significant-gain"></a>Beispiel 2: Ein Klassifizierungsmodell mit signifikantem Gewinn.
![Ein Klassifizierungsmodell mit signifikantem Gewinn.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve2.png)
<a name="calibration-plot"></a>
### <a name="calibration-chart"></a>Kalibrierungsdiagramm

#### <a name="what-is-a-calibration-chart"></a>Was ist ein Kalibrierungsdiagramm?
Ein Kalibrierungsdiagramm wird verwendet, um die Sicherheit eines Vorhersagemodells anzuzeigen. Dies geschieht, indem es die Beziehung zwischen der vorhergesagten Wahrscheinlichkeit und der tatsächlichen Wahrscheinlichkeit zeigt, wobei die „Wahrscheinlichkeit“ die Wahrscheinlichkeit darstellt, dass eine bestimmte Instanz zu einer bestimmten Bezeichnung gehört.
#### <a name="what-does-automated-ml-do-with-the-calibration-chart"></a>Was macht automatisiertes ML mit dem Kalibrierungsdiagramm?
Für alle Klassifizierungsprobleme können Sie die Kalibrierungskurve auf Mikrodurchschnitt, Makrodurchschnitt und jede Klasse in einem bestimmten Vorhersagemodell überprüfen.

Der Makrodurchschnitt berechnet die Metrik unabhängig für jede Klasse und nimmt dann den Durchschnitt, sodass alle Klassen gleich behandelt werden. Der Mikrodurchschnitt aggregiert jedoch die Beiträge aller Klassen, um den Durchschnitt zu berechnen. 
#### <a name="what-does-a-good-model-look-like"></a>Wie sieht ein gutes Modell aus?
 Ein gut kalibriertes Modell richtet sich nach der y=x-Linie aus, wo es in Bezug auf seine Vorhersagen recht zuverlässig ist. Ein übermäßig zuverlässiges Modell richtet sich nach der y=0-Linie aus, wobei die vorhergesagte Wahrscheinlichkeit vorhanden ist, aber keine tatsächliche Wahrscheinlichkeit vorliegt. 


##### <a name="example-1-a-well-calibrated-model"></a>Beispiel 1: Ein gut kalibriertes Modell.
![ Ein besser kalibriertes Modell.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve1.png)

##### <a name="example-2-an-over-confident-model"></a>Beispiel 2: Ein übermäßig zuverlässiges Modell.
![Ein übermäßig zuverlässiges Modell.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve2.png)

## <a name="regression"></a> Regressionsergebnisse

Die folgenden Metriken und Diagramme sind für jedes Regressionsmodell verfügbar, das Sie mit den Funktionen für automatisiertes maschinelles Lernen von Azure Machine Learning erstellen:

+ [Metriken](#reg-metrics)
+ [Vorhergesagt im Vergleich zu TRUE](#pvt)
+ [Residualhistogramm](#histo)


### <a name="reg-metrics"></a> Regressionsmetriken

Bei jeder Ausführungsiteration werden die unten angegebenen Metriken für eine Regressions- oder Vorhersageaufgabe gespeichert.

|Metrik|BESCHREIBUNG|Berechnung|Zusätzliche Parameter
--|--|--|--|
explained_variance|Die erläuterte Varianz ist der Anteil, mit dem ein mathematisches Modell für die Variation eines bestimmten Datasets verantwortlich ist. Hierbei handelt es sich um den prozentualen Rückgang der Varianz der ursprünglichen Daten im Vergleich zur Varianz der Fehler. Wenn der Mittelwert der Fehler 0 beträgt, entspricht er der erläuterten Varianz.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|Keine|
r2_score|R2 ist der Ermittlungskoeffizient bzw. die prozentuale Reduzierung der quadratischen Fehler im Vergleich zu einem Baseline-Modell, das den Mittelwert ausgibt. |[Berechnung](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|Keine|
spearman_correlation|Die Spearman-Korrelation ist ein nicht parametrisches Maß für die Monotonie der Beziehung zwischen zwei Datasets. Im Gegensatz zur Pearson-Korrelation geht die Spearman-Korrelation nicht davon aus, dass beide Datasets normal verteilt sind. Wie bei anderen Korrelationskoeffizienten variiert dieser Wert zwischen -1 und +1, wobei 0 für keine Korrelation steht. Korrelationen von -1 oder +1 implizieren eine exakt monotone Beziehung. Positive Korrelationen implizieren, dass sich x ebenso wie y erhöht. Negative Korrelationen implizieren, dass sich x erhöht und y niedriger wird.|[Berechnung](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|Keine|
mean_absolute_error|Der mittlere absolute Fehler ist der erwartete Wert des absoluten Differenzwerts zwischen dem Ziel und der Vorhersage.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Keine|
normalized_mean_absolute_error|Der normalisierte mittlere absolute Fehler ist der mittlere absolute Fehler, dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Division durch den Datenbereich|
median_absolute_error|Der mittlere absolute Fehler ist der Median aller absoluten Differenzen zwischen dem Ziel und der Vorhersage. Dieser Verlust ist stabil in Bezug auf Ausreißer.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Keine|
normalized_median_absolute_error|Der normalisierte mittlere absolute Fehler ist der mittlere absolute Fehler, dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Division durch den Datenbereich|
root_mean_squared_error|Die Wurzel aus dem mittleren quadratischen Fehler ist die Quadratwurzel der erwarteten quadratischen Differenz zwischen dem Ziel und der Vorhersage.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Keine|
normalized_root_mean_squared_error|Die normalisierte Wurzel aus dem mittleren quadratischen Fehler ist die Wurzel aus dem mittleren quadratischen Fehler, dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Division durch den Datenbereich|
root_mean_squared_log_error|Die Wurzel aus dem mittleren quadratischen logarithmischen Fehler ist die Quadratwurzel des erwarteten quadratischen logarithmischen Fehlers.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Keine|
normalized_root_mean_squared_log_error|Die normalisierte Wurzel aus dem mittleren quadratischen logarithmischen Fehler ist die Wurzel aus dem mittleren quadratischen logarithmischen Fehler dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Division durch den Datenbereich|

### <a name="pvt"></a> Vorhergesagt im Vergleich zu Wahr-Diagramm
#### <a name="what-is-a-predicted-vs-true-chart"></a>Was ist ein Diagramm mit vorhergesagten Werten gegenüber einem Diagramm mit wahren Werten?
Vorhergesagt im Vergleich zu TRUE zeigt die Beziehung zwischen einem vorhergesagten Wert und seinem korrelierenden wahren Wert für ein Regressionsproblem. Dieses Diagramm kann verwendet werden, um die Leistung eines Modells zu messen, da Folgendes gilt: Je näher die vorhergesagten Werte an der y=x-Linie liegen, desto besser ist die Genauigkeit eines Vorhersagemodells.

#### <a name="what-does-automated-ml-do-with-the-predicted-vs-true-chart"></a>Was macht automatisiertes ML mit dem Diagramm mit vorhergesagten Werten gegenüber einem Diagramm mit wahren Werten?
Nach jeder Ausführung wird für jedes Regressionsmodell ein Diagramm mit den vorhergesagten im Vergleich zu den wahren Werten angezeigt. Zum Schutz der Privatsphäre werden die Werte in Behältern zusammengeführt, und die Größe jedes Behälters wird als Balkendiagramm im unteren Teil des Diagrammbereichs angezeigt. Sie können das Vorhersagemodell (mit dem helleren Farbbereich, der die Fehlergrenzen anzeigt) mit dem Idealwert des Modells vergleichen.

#### <a name="what-does-a-good-model-look-like"></a>Wie sieht ein gutes Modell aus?
##### <a name="example-1-a-classification-model-with-low-accuracy"></a>Beispiel 1: Ein Klassifizierungsmodell mit niedriger Genauigkeit.
![Ein Regressionsmodell mit niedriger Genauigkeit bei Vorhersagen.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression1.png)

##### <a name="example-2-a-regression-model-with-high-accuracy"></a>Beispiel 2: Ein Regressionsmodell mit hoher Genauigkeit. 
[![Ein Regressionsmodell mit hoher Genauigkeit bei seinen Vorhersagen.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2-expanded.png)



### <a name="histo"></a> Histogramm der Residualwerte
#### <a name="what-is-a-residuals-chart"></a>Was ist ein Residualwertediagramm?
Ein Residual stellt ein beobachtetes y dar: das vorhergesagte y. Um eine Fehlerspanne mit geringem Bias darzustellen, sollte das Histogramm der Residualwerte als Glockenkurve geformt sein, die um 0 zentriert ist. 
#### <a name="what-does-automated-ml-do-with-the-residuals-chart"></a>Was macht automatisiertes ML mit dem Residualwertediagramm?
Automatisiertes ML stellt automatisch ein Residualwertediagramm bereit, um die Fehlerverteilung in den Vorhersagen zu zeigen.
#### <a name="what-does-a-good-model-look-like"></a>Wie sieht ein gutes Modell aus?
Ein gutes Modell weist in der Regel eine Glockenkurvenform oder Fehler um Null herum auf.

##### <a name="example-1-a-regression-model-with-bias-in-its-errors"></a>Beispiel 1: Ein Regressionsmodell mit Verzerrung bei seinen Fehlern.
![Ein Regressionsmodell mit Verzerrung bei seinen Fehlern.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression3.png)

##### <a name="example-2-a-regression-model-with-more-even-distribution-of-errors"></a>Beispiel 2: Ein Regressionsmodell mit gleichmäßigerer Verteilung von Fehlern.
![Ein Regressionsmodell mit gleichmäßigerer Verteilung von Fehlern.](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression4.png)

## <a name="explain-model"></a> Interpretierbarkeit von Modellen und Featurepriorität
Automatisiertes ML bietet ein Dashboard für die Machine Learning-Interpretierbarkeit für Ihre Ausführungen.
Weitere Informationen zum Aktivieren von Features der Interpretierbarkeit finden Sie unter [Interpretierbarkeit von Modellen für automatisiertes ML](how-to-machine-learning-interpretability-automl.md).

## <a name="next-steps"></a>Nächste Schritte

+ Erfahren Sie mehr über [automatisiertes Machine Learning](concept-automated-ml.md) in Azure Machine Learning.
+ Probieren Sie die Beispielnotebooks für die [Modellerklärung zum automatisierten Machine Learning](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model) aus.
