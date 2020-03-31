---
title: 'Tutorial: Batchtests zur Fehlersuche: LUIS'
description: In diesem Tutorial wird veranschaulicht, wie Sie Batchtests nutzen, um die Qualität Ihrer LUIS-App (Language Understanding) zu überprüfen.
ms.topic: tutorial
ms.date: 03/02/2020
ms.openlocfilehash: c276f0b52f83937fbe3b6fd9e0b7c1a66f665095
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "78250557"
---
# <a name="tutorial-batch-test-data-sets"></a>Tutorial: Testen von Datasets in Batches

In diesem Tutorial wird veranschaulicht, wie Sie Batchtests nutzen, um die Qualität Ihrer LUIS-App (Language Understanding) zu überprüfen.

Anhand von Batchtests können Sie den Zustand des aktiven, trainierten Modells mit einer bekannten Gruppe von bezeichneten Äußerungen und Entitäten überprüfen. Fügen Sie in die JSON-formatierte Batchdatei die Äußerungen hinzu, und legen Sie die Entitätsbezeichnungen fest, die innerhalb der Äußerung vorhergesagt werden sollen.

Anforderungen für Batchtests:

* Maximal 1000 Äußerungen pro Test
* Keine Duplikate
* Zulässige Entitätstypen: nur per maschinellem Lernen trainierte Entitäten.

Wenn Sie eine andere App als die App aus diesem Tutorial verwenden, sollten Sie *nicht* die Beispieläußerungen nutzen, die Ihrer App bereits hinzugefügt wurden.

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Importieren der Beispiel-App
> * Erstellen einer Batchtestdatei
> * Ausführen eines Batchtests
> * Überprüfen der Testergebnisse

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Importieren der Beispiel-App

Importieren Sie eine App, mit der eine Pizzabestellung angenommen wird, z. B. `1 pepperoni pizza on thin crust`.

1.  Laden Sie die [App-JSON-Datei](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-with-machine-learned-entity.json?raw=true) herunter, und speichern Sie sie.

1. Verwenden Sie das [LUIS-Vorschauportal](https://preview.luis.ai/), importieren Sie den JSON-Code in eine neue App, und geben Sie der App den Namen `Pizza app`.

1. Wählen Sie oben rechts im Navigationsbereich die Option **Trainieren** aus, um die App zu trainieren.

## <a name="what-should-the-batch-file-utterances-include"></a>Inhalt der Batchdatei mit den Äußerungen

Die Batchdatei sollte Äußerungen mit bezeichneten Entitäten der obersten Ebene enthalten, die per maschinellem Lernen trainiert wurden, einschließlich Start- und Endposition. Die Äußerungen sollten nicht Teil der Beispiele sein, die bereits in der App enthalten sind. Es sollte sich um Äußerungen handeln, für die Sie in Bezug auf Absichten und Entitäten positive Vorhersagen treffen möchten.

Sie können Tests nach Absicht bzw. Entität unterteilen oder alle Tests (maximal 1.000 Äußerungen) in derselben Datei vorhalten.

## <a name="batch-file"></a>Batchdatei

Der JSON-Beispielcode enthält eine Äußerung mit einer bezeichneten Entität, um darzustellen, wie eine Testdatei aussieht. Für Ihre eigenen Tests sollten Sie viele Äußerungen mit der richtigen Absicht und einer bezeichneten Entität verwenden, die per maschinellem Lernen trainiert wurde.

1. Erstellen Sie `pizza-with-machine-learned-entity-test.json` in einem Text-Editor, oder [laden Sie](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/batch-tests/pizza-with-machine-learned-entity-test.json?raw=true) es herunter.

2. Fügen Sie in der Batchdatei im JSON-Format eine Äußerung mit der **Absicht** hinzu, für die Sie im Test eine Vorhersage erhalten möchten.

   [!code-json[Add the intents to the batch test file](~/samples-cognitive-services-data-files/luis/batch-tests/pizza-with-machine-learned-entity-test.json "Add the intent to the batch test file")]

## <a name="run-the-batch"></a>Ausführen des Batchs

1. Wählen Sie auf der oberen Navigationsleiste **Test** aus.

2. Wählen Sie im rechten Bereich **Batch testing panel** (Batchtestbereich) aus.

3. Wählen Sie **Dataset importieren** aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot der LUIS-App mit hervorgehobener Option „Dataset importieren“](./media/luis-tutorial-batch-testing/import-dataset-button.png)

4. Wählen Sie den Speicherort für die Datei `pizza-with-machine-learned-entity-test.json` aus.

5. Versehen Sie das Dataset mit dem Namen `pizza test`, und klicken Sie auf **Fertig**.

    > [!div class="mx-imgBorder"]
    > ![Auswählen einer Datei](./media/luis-tutorial-batch-testing/import-dataset-modal.png)

6. Wählen Sie die Schaltfläche **Ausführen**.

7. Wählen Sie **See results** (Ergebnisse anzeigen) aus.

8. Überprüfen Sie die Ergebnisse im Diagramm und in der Legende.

## <a name="review-batch-results-for-intents"></a>Überprüfen der Batchergebnisse für Absichten

Die Testergebnisse zeigen grafisch, wie die Testäußerungen für die aktive Version vorhergesagt wurden.

Das Batchdiagramm zeigt die Ergebnisse in vier Quadranten an. Rechts neben dem Diagramm befindet sich ein Filter. Der Filter enthält Absichten und Entitäten. Wenn Sie einen [Diagrammabschnitt](luis-concept-batch-test.md#batch-test-results) oder einen Punkt innerhalb des Diagramms auswählen, werden die zugehörigen Äußerungen unterhalb des Diagramms angezeigt.

Wenn Sie auf das Diagramm zeigen, können Sie mit dem Mausrad die Ansicht im Diagramm vergrößern oder verkleinern. Dies ist hilfreich, wenn es viele Punkte im Diagramm gibt, die nah beieinander gruppiert sind.

Das Diagramm ist in vier Quadranten unterteilt, von denen zwei in Rot angezeigt werden.

1. Wählen Sie in der Filterliste die Absicht **ModifyOrder** aus.

    > [!div class="mx-imgBorder"]
    > ![Auswählen der Absicht „ModifyOrder“ in der Filterliste](./media/luis-tutorial-batch-testing/select-intent-from-filter-list.png)

    Die Äußerung wird als **Richtig positiv** vorhergesagt. Dies bedeutet, dass die Äußerung erfolgreich mit der zugehörigen positiven Vorhersage in der Batchdatei abgeglichen werden konnte.

    > [!div class="mx-imgBorder"]
    > ![Erfolgreicher Abgleich der Äußerung mit der positiven Vorhersage](./media/luis-tutorial-batch-testing/intent-predicted-true-positive.png)

    Die grünen Häkchen in der Filterliste zeigen auch den Erfolg des Tests für jede Absicht an. Alle anderen Absichten sind mit einer positiven Bewertung von „1/1“ angegeben, da die Äußerung für jede Absicht getestet wurde – als negativer Test für alle Absichten, die im Batchtest nicht aufgeführt sind.

1. Wählen Sie die Absicht **Bestätigung** aus. Diese Absicht ist im Batchtest nicht aufgelistet. Es handelt sich hierbei also um einen negativen Test der Äußerung aus dem Batchtest.

    > [!div class="mx-imgBorder"]
    > ![Erfolgreiche negative Vorhersage für die Äußerung für eine nicht in der Batchdatei enthaltene Absicht](./media/luis-tutorial-batch-testing/true-negative-intent.png)

    Der negative Test war erfolgreich. Dies ist am grünen Text im Filter und am Raster zu erkennen.

## <a name="review-batch-test-results-for-entities"></a>Überprüfen der Ergebnisse von Batchtests für Entitäten

Die Entität „ModifyOrder“ als Computerentität mit Unterentitäten zeigt an, ob sich für die Entität der obersten Ebene eine Übereinstimmung ergibt und wie die Vorhersage für die Unterentitäten lautet.

1. Wählen Sie in der Filterliste die Entität **ModifyOrder** und anschließend im Raster den Kreis aus.

1. Die Entitätsvorhersage wird unterhalb des Diagramms angezeigt. Die Anzeige enthält durchgehende Linien für Vorhersagen, die der Erwartung entsprechen, und gepunktete Linien für Vorhersagen, die die Erwartung nicht erfüllen.

    > [!div class="mx-imgBorder"]
    > ![Erfolgreiche Vorhersage des übergeordneten Entitätselements in der Bachdatei](./media/luis-tutorial-batch-testing/labeled-entity-prediction.png)

## <a name="finding-errors-with-a-batch-test"></a>Ermitteln von Fehlern mit einem Batchtest

In diesem Tutorial wurde veranschaulicht, wie Sie einen Test durchführen und die Ergebnisse interpretieren. Es wurde nicht beschrieben, welche Testphilosophie gewählt werden sollte oder wie Sie auf Fehler bei Tests reagieren.

* Stellen Sie sicher, dass Sie in Ihrem Test sowohl positive als auch negative Äußerungen abdecken, die ggf. für andere verwandte Absichten vorhergesagt werden.
* Führen Sie bei Fehlern für Äußerungen die folgenden Schritte aus, und führen Sie die Tests anschließend erneut durch:
    * Sehen Sie sich die Absichten und Entitäten in den aktuellen Beispielen an, und stellen Sie sicher, dass die Beispieläußerungen der aktiven Version sowohl in Bezug auf die Absicht als auch auf die Entitätsbezeichnung korrekt sind.
    * Hinzufügen von Features zum Vorhersagen von Absichten und Entitäten für Ihre App
    * Hinzufügen von weiteren positiven Beispieläußerungen
    * Überprüfen der gleichmäßigen Verteilung von Beispieläußerungen für alle Absichten

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](./includes/cleanup-resources-preview-portal.md)]

## <a name="next-step"></a>Nächster Schritt

Im Tutorial wurde ein Batchtest genutzt, um das aktuelle Modell zu überprüfen.

> [!div class="nextstepaction"]
> [Mehr über Muster erfahren](luis-tutorial-pattern.md)

