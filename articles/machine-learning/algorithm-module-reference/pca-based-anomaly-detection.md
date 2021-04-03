---
title: 'PCA-basierte Anomalieerkennung: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul für die PCA-basierte Anomalieerkennung zur Erstellung eines Anomalieerkennungsmodells basierend auf der Hauptkomponentenanalyse (PCA) verwenden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: fa90fcb9ebc17be9a658b08873234eada98b0fba
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "90898462"
---
# <a name="pca-based-anomaly-detection-module"></a>Modul für die PCA-basierte Anomalieerkennung

In diesem Artikel wird beschrieben, wie das Modul für die PCA-basierte Anomalieerkennung in Azure Machine Learning-Designer verwendet wird, um ein Anomalieerkennungsmodell auf Basis der Hauptkomponentenanalyse (Principal Component Analysis, PCA) zu erstellen.

Dieses Modul hilft Ihnen bei der Erstellung eines Modells in Szenarios, in denen es einfach ist, Trainingsdaten aus einer Klasse abzurufen, z. B. gültige Transaktionen, in denen es aber gleichzeitig schwierig ist, ausreichende Beispiele für die angestrebten Anomalien zu erhalten. 

Wenn Sie z. B. betrügerische Transaktionen erkennen möchten, haben Sie oft nicht genügend Betrugsbeispiele, um mit ihnen zu trainieren. Möglicherweise gibt es jedoch viele Beispiele für nicht betrügerische Transaktionen. Das Modul für die PCA-basierte Anomalieerkennung löst dieses Problem durch die Analyse verfügbarer Features, um so zu ermitteln, was eine „normale“ Klasse ausmacht. Das Modul wendet dann Abstandsmetriken an, um Fälle zu identifizieren, die Anomalien darstellen. So können Sie ein Modell mit vorhandenen unausgeglichenen Daten trainieren.

## <a name="more-about-principal-component-analysis"></a>Weitere Informationen zur Hauptkomponentenanalyse

Die PCA ist ein bewährtes Verfahren beim maschinellen Lernen. Sie wird häufig bei der explorativen Datenanalyse verwendet, da diese Technik die innere Struktur der Daten aufzeigt und die Varianz in den Daten erklärt.

Die PCA analysiert Daten, die mehrere Variablen enthalten. Sie sucht nach Korrelationen zwischen den Variablen und bestimmt die Kombination von Werten, die die Unterschiede in den Ergebnissen am besten erfasst. Diese kombinierten Merkmalswerte werden verwendet, um einen kompakteren Merkmalsraum zu schaffen, der als *Hauptkomponenten* bezeichnet wird.

Für die Erkennung von Anomalien wird jede neue Eingabe analysiert. Der Algorithmus zur Anomalieerkennung berechnet seine Projektion auf die Eigenvektoren zusammen mit einem normalisierten Rekonstruktionsfehler. Dieser normalisierte Fehler wird als Anomalieergebnis verwendet. Je höher der Fehler, desto anomaler ist die Instanz.

Weitere Informationen zur Funktionsweise von PCA sowie zur Implementierung für die Anomalieerkennung finden Sie diesen Dokumenten:

- [A randomized algorithm for principal component analysis](https://arxiv.org/abs/0809.2274), von Rokhlin, Szlan und Tygert

- [Finding Structure with Randomness: Probabilistic Algorithms for Constructing Approximate Matrix Decompositions](http://users.cms.caltech.edu/~jtropp/papers/HMT11-Finding-Structure-SIREV.pdf) (PDF-Download), von Halko, Martinsson und Tropp

## <a name="how-to-configure-pca-based-anomaly-detection"></a>Konfigurieren der PCA-basierten Anomalieerkennung

1. Fügen Sie das Modul für die **PCA-basierte Anomalieerkennung** zu Ihrer Pipeline im Designer hinzu. Sie finden dieses Modul in der Kategorie **Anomalieerkennung**.

2. Wählen Sie im rechten Bereich des Moduls die Option **Trainingsmodus** aus. Geben Sie an, ob Sie das Modell mit bestimmten Parametern trainieren möchten, oder verwenden Sie eine Parameterbereinigung, um die besten Parameter zu finden.

    Wenn Sie wissen, wie Sie das Modell konfigurieren möchten, wählen Sie die Option **Einzelner Parameter** aus, und geben Sie bestimmte Werte als Argumente an.

3. Geben Sie für **Number of components to use in PCA** (Anzahl der Komponenten zur Verwendung in der PCA) die Anzahl der gewünschten Ausgabefeatures bzw. Komponenten an.

    Die Entscheidung, wie viele Komponenten einzubeziehen sind, ist ein wichtiger Teil des Experimententwurfs, der die PCA verwendet. Als allgemeine Richtlinie gilt, dass Sie nicht die gleiche Anzahl von PCA-Komponenten einbeziehen sollten, wie es Variablen gibt. Stattdessen sollten Sie mit einer kleineren Zahl von Komponenten beginnen und deren Zahl erhöhen, bis einige Kriterien erfüllt sind.

    Die besten Ergebnisse werden erzielt, wenn die Anzahl der Ausgabekomponenten *weniger* als die Anzahl der im Dataset verfügbaren Merkmalsspalten beträgt.

4. Geben Sie den Betrag der Überquotierung an, die während des PCA-Trainings in Zufallsreihenfolge durchgeführt werden soll. Bei Problemen mit der Erkennung von Anomalien erschweren unausgeglichene Daten die Anwendung von standardmäßigen PCA-Techniken. Durch die Angabe eines gewissen Betrags an Überquotierung können Sie die Anzahl der Zielinstanzen erhöhen.

    Wenn Sie **1** angeben, wird keine Überquotierung durchgeführt. Wenn Sie einen höheren Wert als **1** angeben, werden zusätzliche Beispiele generiert, die beim Training des Modells verwendet werden.

    Es gibt zwei Optionen, je nachdem, ob Sie eine Parameterbereinigung verwenden oder nicht:

    - **Oversampling parameter for randomized PCA (Parameter für Überquotierung für PCA in Zufallsreihenfolge)** : Geben Sie eine einzelne ganze Zahl ein, die das Verhältnis der Überquotierung der Minderheitsklasse zur normalen Klasse darstellt. (Diese Option ist verfügbar, wenn Sie die Trainingsmethode **Einzelner Parameter** verwenden.)

    > [!NOTE]
    > Sie können das überquotierte Dataset nicht anzeigen. Weitere Informationen zur Verwendung der Überquotierung bei der PCA finden Sie unter [Technische Hinweise](#technical-notes).

5. Wählen Sie die Option **Enable input feature mean normalization** (Eingabefunktion „mittlere Normalisierung“ aktivieren) aus, um alle Eingabefunktionen auf einen Mittelwert von 0 (null) zu normalisieren. Eine Normalisierung oder Skalierung auf Null wird im Allgemeinen für die PCA empfohlen, da das Ziel der PCA darin besteht, die Varianz zwischen den Variablen zu maximieren.

    Diese Option ist standardmäßig aktiviert. Deaktivieren Sie sie, wenn die Werte bereits mit einer anderen Methode oder Skalierung normalisiert wurden.

6. Stellen Sie eine Verbindung eines markierten Trainingsdatasets mit einem der Trainingsmodule her.

   Wenn Sie die Option **Create trainer mode** (Trainermodus erstellen) auf **Single Parameter** (Einzelner Parameter) festlegen, müssen Sie das Modul [Train Anomaly Detection Model](train-anomaly-detection-model.md) (Modell für die Anomalieerkennung trainieren) verwenden.

7. Übermitteln Sie die Pipeline.

## <a name="results"></a>Ergebnisse

Wenn das Training abgeschlossen ist, können Sie das trainierte Modell speichern. Alternativ können Sie eine Verbindung mit dem Modul [Score Model](score-model.md) herstellen, um Anomalieergebnisse vorherzusagen.

So werten Sie die Ergebnisse eines Anomalieerkennungsmodells aus:

1. Stellen Sie sicher, dass in beiden Datasets eine Ergebnisspalte verfügbar ist.

    Wenn Sie versuchen, ein Anomalieerkennungsmodell auszuwerten und eine Fehlermeldung erhalten, die besagt, dass keine Ergebnisspalte in einem bewerteten Dataset für einen Vergleich verfügbar ist, bedeutet dies, dass Sie ein typisches Auswertungsdataset verwenden, das eine Bezeichnungsspalte, aber keine Wahrscheinlichkeitsergebnisse enthält. Wählen Sie ein Dataset aus, das mit der Schemaausgabe für Anomalieerkennungsmodelle übereinstimmt, die eine Spalte **Scored Labels** (Bewertete Bezeichnungen) und eine Spalte **Scored Probabilities** (Bewertete Wahrscheinlichkeiten) enthält.

2. Stellen Sie sicher, dass die Bezeichnungsspalten markiert sind.

    Manchmal werden die mit der Bezeichnungsspalte verbundenen Metadaten im Pipelinegraph entfernt. Wenn Sie in diesem Fall das Modul [Evaluate Model](evaluate-model.md) (Modell auswerten) verwenden, um die Ergebnisse zweier Anomalieerkennungsmodelle zu vergleichen, erhalten Sie möglicherweise eine Fehlermeldung, dass keine Bezeichnungsspalte im bewerteten Dataset vorhanden ist. Oder Sie erhalten evtl. eine Fehlermeldung, die besagt, dass keine Bezeichnungsspalte im bewerteten Dataset zum Vergleichen vorhanden ist.

    Sie können diese Fehler vermeiden, indem Sie das Modul [Edit Metadata](edit-metadata.md) (Metadaten bearbeiten) vor dem Modul [Evaluate Model](evaluate-model.md) (Modell auswerten) hinzufügen. Verwenden Sie den Spaltenselektor, um die Klassenspalte auszuwählen, und wählen Sie in der Liste **Felder** die Option **Bezeichnung** aus.

3. Verwenden Sie das Modul [Python-Skript ausführen](execute-python-script.md), um die Bezeichnungen der Spaltenkategorien in **1 (positiv, normal)** und **0 (negativ, anormal)** zu ändern.

    ````
    label_column_name = 'XXX'
    anomaly_label_category = YY
    dataframe1[label_column_name] = dataframe1[label_column_name].apply(lambda x: 0 if x == anomaly_label_category else 1)
    ````

    
## <a name="technical-notes"></a>Technische Hinweise

Dieser Algorithmus verwendet die PCA zur Annäherung an den Teilbereich, der die normale Klasse enthält. Der Teilbereich wird von Eigenvektoren umfasst, die mit den oberen Eigenwerten der Datenkovarianzmatrix verknüpft sind. 

Für jede neue Eingabe berechnet die Anomalieerkennung zuerst die Projektion auf die Eigenvektoren und dann den normalisierten Rekonstruktionsfehler. Dieser Fehler ist das Anomalieergebnis. Je höher der Fehler, desto anomaler ist die Instanz. Details zur Berechnung des normalen Bereichs finden Sie in Wikipedia: [Principal component analysis](https://wikipedia.org/wiki/Principal_component_analysis) bzw. unter „Hauptkomponentenanalyse“ (deutschsprachig). 


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 

Eine Liste der Fehler, die bei den Designer-Modulen auftreten können, finden Sie unter [Ausnahmen und Fehlercodes für den Designer](designer-error-codes.md).