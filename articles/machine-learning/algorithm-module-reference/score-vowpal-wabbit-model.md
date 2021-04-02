---
title: Score Vowpal Wabbit-Modell
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mit dem Modul Score Vowpal Wabbit-Modell Ergebnisse für einen Satz von Eingabedaten mithilfe eines vorhandenen trainierten Vowpal Wabbit-Modells generieren können.
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 07/02/2020
ms.openlocfilehash: 23dfee7b78f2606c54525391e1260af69a9b0779
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "90898393"
---
# <a name="score-vowpal-wabbit-model"></a>Score Vowpal Wabbit-Modell
In diesem Artikel wird beschrieben, wie Sie mit dem Modul **Score Vowpal Wabbit-Modell** im Azure Machine Learning-Designer Ergebnisse für eine Gruppe von Eingabedaten mithilfe eines vorhandenen trainierten Vowpal Wabbit-Modells generieren können.  

Dieses Modul stellt die neueste Version 8.8.1 des Vowpal Wabbit-Frameworks bereit. Bewerten Sie mit diesem Modul Daten mithilfe eines trainierten Modells, das im VW-Format Version 8 gespeichert ist.  

## <a name="how-to-configure-score-vowpal-wabbit-model"></a>Konfigurieren eines Score Vowpal Wabbit-Modells

1.  Fügen Sie Ihrem Experiment das Modul **Score Vowpal Wabbit-Modell** hinzu.  
  
2.  Fügen Sie ein trainiertes Vowpal Wabbit-Modell hinzu, und verbinden Sie es mit dem linken Eingabeport. Sie können ein trainiertes Modell verwenden, das im gleichen Experiment erstellt wurde, oder ein gespeichertes Modell in der **Datasets**-Kategorie des linken Navigationsbereichs des Designers suchen. Das Modell muss jedenfalls im Azure Machine Learning-Designer verfügbar sein.  
  
    > [!NOTE]
    > Nur Vowpal Wabbit 8.8.1-Modelle werden unterstützt. Sie können keine gespeicherten Modelle verbinden, die mithilfe anderer Algorithmen trainiert wurden.
  
3.  Fügen Sie das Testdataset hinzu, und verbinden Sie es mit dem rechten Eingabeport. Wenn es sich bei dem Testdataset um ein Verzeichnis handelt, das die Testdatendatei enthält, geben Sie den Namen der Testdatendatei unter **Name of the test data file** (Name der Testdatendatei) an. Wenn das Testdataset eine einzelne Datei ist, lassen Sie das Feld **Name of the test data file** (Name der Testdatendatei) leer.

4. Geben Sie im Textfeld **VW arguments** (VW-Argumente) eine Gruppe gültiger Befehlszeilenargumente für die ausführbare Datei für Vowpal Wabbit an.  

    Informationen darüber, welche Vowpal Wabbit-Argumente in Azure Machine Learning unterstützt werden und welche nicht, finden Sie im Abschnitt [Technische Hinweise](#technical-notes).  

5.  **Name of the test data file** (Name der Testdatendatei): Geben Sie den Namen der Datei an, die die Eingabedaten enthält. Dieses Argument wird nur verwendet, wenn es sich bei dem Testdataset um ein Verzeichnis handelt.

6. **Specify file type** (Dateityp angeben): Geben Sie an, welches Format für Ihre Trainingsdaten verwendet wird. Vowpal Wabbit unterstützt die folgenden beiden Eingabedateiformate:  

   - **VW** ist das von Vowpal Wabbit verwendete interne Format. Ausführliche Informationen finden Sie auf der [Vowpal Wabbit-Wikiseite](https://github.com/JohnLangford/vowpal_wabbit/wiki/Input-format). 
   - **SVMLight** ist ein von einigen anderen Machine-Learning-Tools verwendetes Format. 

7. Wählen Sie die Option **Include an extra column containing labels** (Eine zusätzliche Spalte mit Bezeichnungen einschließen) aus, wenn Sie Bezeichnungen mit den Bewertungen ausgeben möchten.  

   Bei der Verarbeitung von Textdaten setzt Vowpal Wabbit in der Regel keine Bezeichnungen voraus, und es werden nur die Ergebnisse für jede Datenzeile zurückgegeben.  

8. Wählen Sie die Option **Include an extra column containing raw scores** (Eine zusätzliche Spalte mit Rohergebnissen einschließen) aus, wenn Sie Rohergebnisse mit den Bewertungen ausgeben möchten.  

9. Übermitteln Sie die Pipeline.

## <a name="results"></a>Ergebnisse

Nach Abschluss des Trainings:

+ Um die Ergebnisse zu visualisieren, klicken Sie mit der rechten Maustaste auf die Ausgabe des Moduls [Score Vowpal Wabbit-Modell](score-vowpal-wabbit-model.md). Die Ausgabe gibt an, dass ein Vorhersageergebnis von 0 bis 1 normalisiert wurde. 

+ Um die Ergebnisse auszuwerten, sollte das Ausgabedataset bestimmte Bewertungsspaltennamen enthalten, die die Anforderungen des Evaluate Model-Moduls erfüllen.

  + Für die Regressionsaufgabe muss das auszuwertende Dataset über eine Spalte mit dem Namen `Regression Scored Labels` verfügen, die bewertete Bezeichnungen darstellt.
  + Für die binäre Klassifizierungsaufgabe muss das auszuwertende Dataset über zwei Spalten mit den Namen `Binary Class Scored Labels` und `Binary Class Scored Probabilities` verfügen, die bewertete Bezeichnungen bzw. Wahrscheinlichkeiten darstellen.
  + Für die Mehrfachklassifizierungsaufgabe muss das auszuwertende Dataset über eine Spalte mit dem Namen `Multi Class Scored Labels` verfügen, die bewertete Bezeichnungen darstellt.

  Beachten Sie, dass die Ergebnisse des Moduls „Score Vowpal Wabbit-Modell“ nicht direkt ausgewertet werden können. Vor der Auswertung sollte das Dataset gemäß den oben aufgeführten Anforderungen geändert werden.

##  <a name="technical-notes"></a>Technische Hinweise

Dieser Abschnitt enthält Implementierungsdetails, Tipps und Antworten auf häufig gestellte Fragen.

### <a name="parameters"></a>Parameter

Vowpal Wabbit verfügt über viele Befehlszeilenoptionen zum Auswählen und Optimieren von Algorithmen. Eine vollständige Erläuterung dieser Optionen ist hier nicht möglich. Sie sollten die [Wiki-Seite von Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki/Command-line-arguments) besuchen.  

Die folgenden Parameter werden in Azure Machine Learning Studio (klassisch) nicht unterstützt.  

-   Die unter [https://github.com/JohnLangford/vowpal_wabbit/wiki/Command-line-arguments](https://github.com/JohnLangford/vowpal_wabbit/wiki/Command-line-arguments) aufgeführten Eingabe-/Ausgabeoptionen.  
  
     Diese Eigenschaften werden bereits automatisch vom Modul konfiguriert.  
  
-   Außerdem sind alle Optionen, die mehrere Ausgaben generieren oder mehrere Eingaben erfordern, nicht zulässig. Dazu zählen *`--cbt`* , *`--lda`* und *`--wap`* .  
  
-   Nur überwachte Lernalgorithmen werden unterstützt. Dies lässt diese Optionen nicht zu: *`–active`* , `--rank`, *`--search`* usw.  

Alle anderen Argumente als die oben beschriebenen sind zulässig.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 