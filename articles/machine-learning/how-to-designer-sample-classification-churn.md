---
title: 'Designer: Beispiel zur Vorhersage von Kundenabwanderung'
titleSuffix: Azure Machine Learning
description: Befolgen Sie dieses Klassifizierungsbeispiel, um Kundenabwanderung mit Azure Machine Learning-Designer und Boosted Decision Trees vorherzusagen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.reviewer: sgilley
ms.date: 12/25/2019
ms.openlocfilehash: 9e0bae722430662ddbe252e82c5108eb820ce352
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2020
ms.locfileid: "75660109"
---
# <a name="use-boosted-decision-tree-to-predict-churn-with-azure-machine-learning-designer"></a>Verwenden von Boosted Decision Tree zum Vorhersagen von Kundenabwanderung mit Azure Machine Learning-Designer

**Designer (Vorschauversion) – Beispiel 5**

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Hier erfahren Sie, wie Sie im Designer (Vorschauversion) eine komplexe Machine Learning-Pipeline erstellen, ohne eine einzige Codezeile zu schreiben.

In dieser Pipeline werden zwei Klassifizierer des Typs **Verstärkte Entscheidungsstruktur mit zwei Klassen** trainiert, um allgemeine Aufgaben für CRM-Systeme (Customer Relationship Management) vorherzusagen: Kundenabwanderung. Die Datenwerte und Bezeichnungen sind über mehrere Datenquellen verteilt und verschlüsselt, um Kundeninformationen zu anonymisieren. Trotzdem kann der Designer weiterhin verwendet werden, um Datasets zu kombinieren und ein Modell mit den unkenntlich gemachten Werten zu trainieren.

Weil Sie versuchen, die Frage „Welche?“ zu beantworten, wird dies als Klassifikationsproblem bezeichnet. Sie können jedoch dieselbe Logik in diesem Beispiel anwenden, um jede Art von Problem für maschinelles Lernen zu bewältigen, sei es Regression, Klassifizierung, Clustering usw.

Der fertige Graph für diese Pipeline sieht wie folgt aus:

![Graph der Pipeline](./media/how-to-designer-sample-classification-churn/pipeline-graph.png)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [aml-ui-prereq](../../includes/aml-ui-prereq.md)]

4. Klicken Sie auf Beispiel 5, um es zu öffnen. 

## <a name="data"></a>Data

Die Daten für diese Pipeline stammen vom KDD Cup 2009. Die Daten sind in 50.000 Zeilen und 230 Featurespalten (Merkmalspalten) enthalten. Die Aufgabe ist die Vorhersage der Kundenabwanderung, Kauflust und des Up-Sellings für Kunden, die diese Features nutzen. Weitere Informationen zu den Daten und zur Aufgabe finden Sie auf der [KDD-Website](https://www.kdd.org/kdd-cup/view/kdd-cup-2009).

## <a name="pipeline-summary"></a>Pipelineübersicht

Die Beispiel-Pipeline im Designer zeigt eine auf einem binären Klassifizierer basierende Vorhersage von Kundenabwanderung, Kauflust und Up-Selling, eine übliche Aufgabe beim Customer Relationship Management (CRM).

Zunächst eine einfache Datenverarbeitung.

- Das Dataset mit den Rohdaten enthält viele fehlende Werte. Verwenden Sie das Modul **Clean Missing Data** (Bereinigen fehlender Daten), um die fehlenden Werte durch 0 zu ersetzen.

    ![Bereinigen des Datasets](media/how-to-designer-sample-classification-churn/sample5-dataset-1225.png)

- Die Features und die zugehörigen Bezeichnungen befinden sich in verschiedenen Datasets. Verwenden Sie das Modul **Add Columns** (Spalten hinzufügen), um die Bezeichnungsspalten an die Featurespalten anzufügen. Die erste Spalte, **Col1**, ist die Bezeichnungsspalte. Das Visualisierungsergebnis zeigt, dass das Dataset unausgewogen ist. Es gibt wesentlich mehr negative (-1) als positive Beispiele (+ 1). Wir verwenden das Modul **SMOTE**, um unterrepräsentierte Fälle später zu erhöhen.

    ![Hinzufügen des Spaltendatasets](./media/how-to-designer-sample-classification-churn/sample5-addcol-1225.png)



- Wir verwenden das Modul **Split Data** (Daten aufteilen) zum Aufteilen des Datasets in Trainings- und Testdatasets.

- Verwenden Sie dann die binäre Klassifizierung „Boosted Decision Tree“ (Verstärkter Entscheidungsbaum) mit den Standardparametern, um die Vorhersagemodelle zu erstellen. Erstellen Sie ein Modell pro Aufgabe, d. h. jeweils ein Modell zur Vorhersage von Up-Selling, Kauflust und Kundenabwanderung.

- Im rechten Teil der Pipeline verwenden wir das Modul **SMOTE**, um den Prozentsatz positiver Beispiele zu erhöhen. Der SMOTE-Prozentsatz wird auf 100 festgelegt, um die positiven Beispiele zu verdoppeln. In der [Referenz zum SMOTE-Modul](algorithm-module-reference/smote.md) erfahren Sie mehr zu dessen Funktionsweise.

## <a name="results"></a>Ergebnisse

Visualisieren Sie die Ausgabe des Moduls **Evaluate Model** (Auswerten des Modells), um die Leistung des Modells für das Testdataset zu ermitteln. 

![Auswertung der Ergebnisse](./media/how-to-designer-sample-classification-churn/sample5-evaluate-1225.png)

 Sie können den Schieberegler **Threshold** (Schwellenwert) verschieben und prüfen, wie sich die Metriken für die binäre Klassifizierungsaufgabe ändern. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die anderen Beispiele, die für den Designer zur Verfügung stehen:

- [Beispiel 1 – Regression: Vorhersagen des Preises eines Autos](how-to-designer-sample-regression-automobile-price-basic.md)
- [Beispiel 2 – Regression: Vergleichen von Algorithmen für die Vorhersage von Autopreisen](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Beispiel 3 – Klassifizierung mit Featureauswahl: Vorhersage des Einkommens](how-to-designer-sample-classification-predict-income.md)
- [Beispiel 4 – Klassifizierung: Vorhersagen des Kreditrisikos (kostensensibel)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Beispiel 6 – Klassifizierung: Vorhersage von Flugverspätungen](how-to-designer-sample-classification-flight-delay.md)
- [Beispiel 7 – Textklassifizierung: Wikipedia SP 500-Dataset](how-to-designer-sample-text-classification.md)
