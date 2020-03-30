---
title: Testen von Apps im LUIS-Portal
description: Verwenden Sie LUIS (Language Understanding Intelligent Service), um Ihre Anwendung fortlaufend zu optimieren und ihr Sprachverständnis zu verbessern.
ms.topic: conceptual
ms.date: 11/19/2019
ms.openlocfilehash: ba7e23a72cd308dd4393bf9a581571e2bc9f5fa0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218740"
---
# <a name="test-your-luis-app-in-the-luis-portal"></a>Testen Ihrer LUIS-App im LUIS-Portal

Das [Testen](luis-concept-test.md) einer App ist ein iterativer Vorgang. Testen Sie Ihre LUIS-App nach dem Trainieren mit Beispieläußerungen, um festzustellen, ob die Absichten und Entitäten ordnungsgemäß erkannt werden. Ist das nicht der Fall, aktualisieren Sie die LUIS-App, und trainieren und testen Sie sie erneut.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

<!-- anchors for H2 name changes -->
<a name="train-your-app"></a>
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="luis-interactive-testing"></a>

## <a name="train-before-testing"></a>Vor dem Testen trainieren

Um die aktuellste Version der aktiven App zu testen, wählen Sie vor dem Testen im oberen Menü **Train** (Trainieren) aus.

## <a name="test-an-utterance"></a>Testen einer Äußerung

Die Testäußerung sollte nicht exakt mit einer der Beispieläußerungen in der App übereinstimmen. Die Testäußerung sollte die Wortwahl, Satzlänge und Entitätsverwendung aufweisen, die Sie von einem Benutzer erwarten.

1. Greifen Sie auf Ihre App zu, indem Sie ihren Namen auf der Seite **Meine Apps** auswählen.

1. Um auf den Einblendbereich **Test** zuzugreifen, wählen Sie im oberen Bereich Ihrer Anwendung **Test** aus.

    > [!div class="mx-imgBorder"]
    > ![Seite „Train & Test App“](./media/luis-how-to-interactive-test/test.png) (App trainieren und testen)

1. Geben Sie eine Äußerung in das Textfeld ein, und drücken Sie die EINGABETASTE. Sie können im **Test** beliebig viele Äußerungen eingeben, aber immer nur einzeln.

1. Die Äußerung, die am höchsten bewertete Absicht und ihre Bewertung werden der Liste der Äußerungen unter dem Textfeld hinzugefügt.

    ![Erkennen einer falschen Absicht in interaktiven Tests](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="inspect-score"></a>Überprüfen der Bewertung

Sie überprüfen die Details des Testergebnisses im Bereich **Überprüfen**.

1. Wählen Sie bei geöffnetem Einblendbereich **Test** die Option **Überprüfen** für eine Äußerung aus, die verglichen werden soll.

    ![Auswählen der Schaltfläche „Überprüfen“, um weitere Details zu den Testergebnissen anzuzeigen](./media/luis-how-to-interactive-test/inspect.png)

1. Der Bereich **Überprüfung** wird angezeigt. Dieser Bereich enthält die Absicht mit der höchsten Bewertung sowie alle identifizierten Entitäten. Das Ergebnis der ausgewählten Äußerung wird im Bereich angezeigt.

    ![Dieser Bereich enthält die Absicht mit der höchsten Bewertung sowie alle identifizierten Entitäten. Das Ergebnis der ausgewählten Äußerung wird im Bereich angezeigt.](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="correct-top-scoring-intent"></a>Korrigieren der Absicht mit der höchsten Bewertung

1. Wenn die Absicht mit der höchsten Bewertung falsch ist, wählen Sie die Schaltfläche **Bearbeiten** aus.

1.  Wählen Sie in der Dropdownliste die richtige Absicht für die Äußerung aus.

    ![Auswählen der richtigen Absicht](./media/luis-how-to-interactive-test/intent-select.png)

## <a name="view-sentiment-results"></a>Anzeigen von Standpunktergebnissen

Wenn die **Standpunktanalyse** auf der Seite **[Publish](luis-how-to-publish-app.md#enable-sentiment-analysis)** (Veröffentlichen) konfiguriert wird, schließen die Testergebnisse den in der Äußerung gefundenen Standpunkt ein.

![Abbildung des Testbereichs mit Standpunktanalyse](./media/luis-how-to-interactive-test/sentiment.png)

## <a name="correct-matched-patterns-intent"></a>Korrigieren der Absicht des übereinstimmenden Musters

Wenn Sie [Muster](luis-concept-patterns.md) verwenden und die Äußerung mit einem Muster übereinstimmte, jedoch die falsche Absicht vorhergesagt wurde, wählen Sie den Link **Bearbeiten** für das Muster und dann die richtige Absicht aus.

## <a name="compare-with-published-version"></a>Vergleichen mit der veröffentlichten Version

Sie können die aktive Version Ihrer App mit der veröffentlichten [Endpunktversion](luis-glossary.md#endpoint) testen. Wählen Sie im Bereich **Überprüfen** die Option **Compare with published** (Mit veröffentlichtem Element vergleichen) aus. Tests mit dem veröffentlichten Modell werden mit dem Kontingent Ihres Azure-Abonnements verrechnet.

![Compare with published (Mit veröffentlichtem Element vergleichen)](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Anzeigen des Endpunkt-JSON-Codes im Testbereich
Sie können den für den Vergleich zurückgegebenen JSON-Code des Endpunkts anzeigen, indem Sie die Ansicht **JSON anzeigen** auswählen.

![Veröffentlichte JSON-Antwort](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

## <a name="additional-settings-in-test-panel"></a>Zusätzliche Einstellungen im Testbereich

### <a name="luis-endpoint"></a>LUIS-Endpunkt

Wenn Sie mehrere LUIS-Endpunkte haben, können Sie den für Tests verwendeten Endpunkt über den Link **Zusätzliche Einstellungen** im Bereich „Veröffentlicht“ des Tests ändern. Wenn Sie nicht sicher sind, welchen Endpunkt Sie verwenden sollten, wählen Sie den Standard **Starter_Key** aus.

> [!div class="mx-imgBorder"]
> ![Testbereich mit hervorgehobenem Link „Zusätzliche Einstellungen“](media/luis-how-to-interactive-test/additional-settings-v3-settings.png)


## <a name="batch-testing"></a>Testen in Batches
Weitere Informationen finden Sie in den [Konzepten](luis-concept-batch-test.md) zu Batchtests und im Artikel zum [Testen eines Batches von Äußerungen](luis-how-to-batch-test.md).

## <a name="next-steps"></a>Nächste Schritte

Wenn sich in Tests herausstellt, dass Ihre LUIS-App nicht die richtigen Absichten und Entitäten erkennt, können Sie die Genauigkeit Ihrer LUIS-App verbessern, indem Sie aktiv weitere Äußerungen bezeichnen oder Features hinzufügen.

* [Bezeichnen von vorgeschlagenen Äußerungen mit LUIS](luis-how-to-review-endpoint-utterances.md)
* [Verwenden von Features zum Verbessern der Leistung Ihrer LUIS-App](luis-how-to-add-features.md)
