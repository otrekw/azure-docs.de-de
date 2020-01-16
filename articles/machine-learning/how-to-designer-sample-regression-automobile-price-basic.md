---
title: 'Designer: Beispiel zur Vorhersage von Autopreisen (einfach)'
titleSuffix: Azure Machine Learning
description: Erstellen Sie mit dem Azure Machine Learning-Designer ein Machine Learning-Modell zum Vorhersagen eines Fahrzeugpreises, ohne eine einzige Codezeile schreiben zu müssen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.date: 12/25/2019
ms.openlocfilehash: 156b963fc1644d1f863d8ddd1d86c15b311e18a8
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75763384"
---
# <a name="use-regression-to-predict-car-prices-with-azure-machine-learning-designer"></a>Sagen Sie Autopreise mit dem Azure Machine Learning-Designer mithilfe von Regression vorher.

**Designer (Vorschauversion) – Beispiel 1**

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Erfahren Sie, wie Sie über den Designer (Vorschauversion) ein Machine Learning-Regressionsmodell erstellen, ohne eine einzige Codezeile zu schreiben.

Mit dieser Pipeline wird ein **linearer Regressor** trainiert, um den Preis eines Fahrzeugs anhand technischer Merkmale wie Marke, Modell, Leistung und Größe vorherzusagen. Weil Sie versuchen, die Frage „Wie viel?“ zu beantworten, wird dies als Regressionsproblem bezeichnet. Sie können jedoch dieselben elementaren Schritte aus diesem Beispiel anwenden, um jede Art von Problem für maschinelles Lernen zu bewältigen, sei es Regression, Klassifizierung, Clustering usw.

Die elementaren Schritte des Trainings eines Machine Learning-Modells sind:

1. Abrufen von Daten
1. Vorverarbeiten der Daten
1. Trainieren des Modells
1. Auswerten des Modells

Hier ist der endgültige vollständige Graph der Pipeline. Dieser Artikel stellt Begründungen für alle Module bereit, sodass Sie ähnliche Entscheidungen selbst treffen können.

![Graph der Pipeline](./media/how-to-designer-sample-regression-automobile-price-basic/overall-graph.png)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [aml-ui-prereq](../../includes/aml-ui-prereq.md)]

4. Klicken Sie auf Beispiel 1, um es zu öffnen.


## <a name="get-the-data"></a>Abrufen von Daten

In diesem Beispiel wird das Dataset **Automobile Price Data (Raw)** (Automobilpreisdaten (Rohdaten)) aus dem UCI Machine Learning-Repository verwendet. Dieses Dataset enthält 26 Spalten mit Informationen zu Automobilen, darunter Marke, Modell, Preis, Fahrzeugmerkmale (etwa die Anzahl der Zylinder), Treibstoffverbrauch und eine Versicherungsrisikobewertung. Das Ziel dieses Beispiels besteht darin, den Preis eines Autos vorherzusagen.

## <a name="pre-process-the-data"></a>Vorverarbeiten der Daten

Zu den Hauptaufgaben der Datenaufbereitung gehören die Datenbereinigung, Integration, Transformation, Reduktion und Diskretisierung oder Quantisierung. Im Designer finden Sie Module für diese Vorgänge und andere Aufgaben der Datenvorverarbeitung in der Gruppe **Datentransformation** im linken Bereich.

Verwenden Sie das Modul **Select Columns in Dataset** (Spalten in Dataset auswählen), um normalisierte Verluste auszuschließen, die viele fehlende Werte aufweisen. Verwenden Sie dann **Clean Missing Data** (Fehlende Daten bereinigen), um die Zeilen zu entfernen, die fehlende Werte aufweisen. Dies hilft, einen bereinigten Satz von Trainingsdaten zu erstellen.

![Datenvorverarbeitung](./media/how-to-designer-sample-regression-automobile-price-basic/data-processing.png)

## <a name="train-the-model"></a>Trainieren des Modells

Machine Learning-Probleme sind vielfältig. Machine Learning-Aufgaben sind z.B. Klassifizierung, Clustering, Regression und Empfehlungssysteme, die möglicherweise jeweils einen anderen Algorithmus erfordern. Die Auswahl des Algorithmus hängt häufig von den Anforderungen des Anwendungsfalls ab. Nachdem Sie einen Algorithmus ausgewählt haben, müssen Sie seine Parameter optimieren, um ein genaueres Modell zu trainieren. Sie müssen alle Modelle basierend auf Metriken wie Genauigkeit, Verständlichkeit und Effizienz auswerten.

Da das Ziel dieses Beispiels darin besteht, Automobilpreise vorherzusagen, und die Bezeichnungsspalte (Preis) echte Zahlen enthält, ist ein Regressionsmodell eine gute Wahl. In Anbetracht der Tatsache, dass die Anzahl der Features relativ klein ist (weniger als 100) und diese Features nicht spärlich sind, ist es wahrscheinlich, dass die Entscheidungsgrenze nichtlinear ist. Daher verwenden wir die **Entscheidungswaldregression** für diese Pipeline.

Verwenden Sie das Modul **Split Data** (Daten teilen), um nach dem Zufallsprinzip die eingegebenen Daten so aufzuteilen, dass das Trainingsdataset 70 % der ursprünglichen Daten enthält und das Testdataset 30 % der ursprünglichen Daten.

## <a name="test-evaluate-and-compare"></a>Testen, Evaluieren und Vergleichen

Teilen Sie das Dataset auf, und verwenden Sie unterschiedliche Datasets zum Trainieren und Testen des Modells, um die Auswertung des Modells objektiver zu gestalten.

Nachdem das Modell trainiert wurde, verwenden Sie die Module **Score Model** (Modell bewerten) und **Evaluate Model** (Modell auswerten), um die vorhergesagten Ergebnisse zu generieren und die Modelle auszuwerten.

**Score Model** generiert Vorhersagen für das Testdataset mithilfe des trainierten Modells. Um das Ergebnis zu überprüfen, wählen Sie den Ausgabeport des **Score Model**-Moduls und dann **Visualize** (Visualisieren) aus.

![Bewerten des Ergebnisses](./media/how-to-designer-sample-regression-automobile-price-basic/sample1-score-1225.png)

Übergeben Sie die Bewertungen dann an das **Evaluate Model**-Modul, um Auswertungsmetriken zu generieren. Um das Ergebnis zu überprüfen, wählen Sie den Ausgabeport des **Evaluate Model**-Moduls und dann **Visualize** (Visualisieren) aus.

![Auswerten des Ergebnisses](./media/how-to-designer-sample-regression-automobile-price-basic/sample1-evaluate-1225.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die anderen Beispiele, die für den Designer zur Verfügung stehen:

- [Beispiel 2 – Regression: Vergleichen von Algorithmen für die Vorhersage von Autopreisen](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Beispiel 3 – Klassifizierung mit Featureauswahl: Vorhersage des Einkommens](how-to-designer-sample-classification-predict-income.md)
- [Beispiel 4 – Klassifizierung: Vorhersagen des Kreditrisikos (kostensensibel)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Beispiel 5 – Klassifizierung: Vorhersage der Kundenabwanderung](how-to-designer-sample-classification-churn.md)
- [Beispiel 6 – Klassifizierung: Vorhersage von Flugverspätungen](how-to-designer-sample-classification-flight-delay.md)
- [Beispiel 7 – Textklassifizierung: Wikipedia SP 500-Dataset](how-to-designer-sample-text-classification.md)