---
title: Testen von Apps im LUIS-Portal
description: Verwenden Sie LUIS (Language Understanding Intelligent Service), um Ihre Anwendung fortlaufend zu optimieren und ihr Sprachverständnis zu verbessern.
ms.topic: how-to
ms.date: 05/20/2020
ms.openlocfilehash: 86ee90e2d3bb322a4f55439d105941cf43462d3e
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344151"
---
# <a name="test-your-luis-app-in-the-luis-portal"></a>Testen Ihrer LUIS-App im LUIS-Portal

Das [Testen](luis-concept-test.md) einer App ist ein iterativer Vorgang. Testen Sie Ihre LUIS-App nach dem Trainieren mit Beispieläußerungen, um festzustellen, ob die Absichten und Entitäten ordnungsgemäß erkannt werden. Ist das nicht der Fall, aktualisieren Sie die LUIS-App, und trainieren und testen Sie sie erneut.

<!-- anchors for H2 name changes -->
<a name="train-your-app"></a>
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="luis-interactive-testing"></a>

## <a name="train-before-testing"></a>Vor dem Testen trainieren

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.
1. Um die aktuellste Version der aktiven App zu testen, wählen Sie vor dem Testen im oberen Menü **Train** (Trainieren) aus.

## <a name="test-an-utterance"></a>Testen einer Äußerung

Die Testäußerung sollte nicht exakt mit einer der Beispieläußerungen in der App übereinstimmen. Die Testäußerung sollte die Wortwahl, Satzlänge und Entitätsverwendung aufweisen, die Sie von einem Benutzer erwarten.

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.

1. Um auf den Einblendbereich **Test** zuzugreifen, wählen Sie im oberen Bereich Ihrer Anwendung **Test** aus.

    > [!div class="mx-imgBorder"]
    > ![Seite „Train & Test App“](./media/luis-how-to-interactive-test/test.png) (App trainieren und testen)

1. Geben Sie eine Äußerung in das Textfeld ein, und drücken Sie die EINGABETASTE. Sie können im **Test** beliebig viele Äußerungen eingeben, aber immer nur einzeln.

1. Die Äußerung, die am höchsten bewertete Absicht und ihre Bewertung werden der Liste der Äußerungen unter dem Textfeld hinzugefügt.

    > [!div class="mx-imgBorder"]
    > ![Erkennen einer falschen Absicht in interaktiven Tests](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="inspect-the-prediction"></a>Untersuchen der Vorhersage

Sie überprüfen die Details des Testergebnisses im Bereich **Überprüfen**.

1. Wählen Sie bei geöffnetem Einblendbereich **Test** die Option **Überprüfen** für eine Äußerung aus, die verglichen werden soll.

    > [!div class="mx-imgBorder"]
    > ![Auswählen der Schaltfläche „Überprüfen“, um weitere Details zu den Testergebnissen anzuzeigen](./media/luis-how-to-interactive-test/inspect.png)

1. Der Bereich **Überprüfung** wird angezeigt. Dieser Bereich enthält die Absicht mit der höchsten Bewertung sowie alle identifizierten Entitäten. Die Vorhersage der ausgewählten Äußerung wird im Bereich angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Ausschnitt aus einem Screenshot des Testüberprüfungsbereichs](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="add-to-example-utterances"></a>Hinzufügen zu Beispieläußerungen

Im Überprüfungsbereich können Sie die Testäußerung einer Absicht hinzufügen, indem Sie **Add to example utterances** (Zu Beispieläußerungen hinzufügen) auswählen.

## <a name="disable-required-features"></a>Deaktivieren erforderlicher Features

Mit diesem Umschalter können Sie anzeigen, wie die Vorhersage lauten würde, wenn das Feature der Entität nicht erforderlich wäre.

So können Sie feststellen, ob die trainierte App Ihre Entitäten basierend auf den erforderlichen Features richtig vorhersagt. Die trainierte App kann eine maschinell erlernte Entität falsch vorhersagen, wenn die Bezeichnung von Beispieläußerungen fehlerhaft ist oder das erforderliche Feature nicht mit dem Text übereinstimmt.

## <a name="view-sentiment-results"></a>Anzeigen von Standpunktergebnissen

Wenn die **Standpunktanalyse** auf der Seite **[Publish](luis-how-to-publish-app.md#enable-sentiment-analysis)** (Veröffentlichen) konfiguriert wird, schließen die Testergebnisse den in der Äußerung gefundenen Standpunkt ein.

## <a name="correct-matched-patterns-intent"></a>Korrigieren der Absicht des übereinstimmenden Musters

Wenn Sie [Muster](luis-concept-patterns.md) verwenden und die Äußerung mit einem Muster übereinstimmte, jedoch die falsche Absicht vorhergesagt wurde, wählen Sie den Link **Bearbeiten** für das Muster und dann die richtige Absicht aus.

## <a name="compare-with-published-version"></a>Vergleichen mit der veröffentlichten Version

Sie können die aktive Version Ihrer App mit der veröffentlichten [Endpunktversion](luis-glossary.md#endpoint) testen. Wählen Sie im Bereich **Überprüfen** die Option **Compare with published** (Mit veröffentlichtem Element vergleichen) aus. Tests mit dem veröffentlichten Modell werden mit dem Kontingent Ihres Azure-Abonnements verrechnet.

> [!div class="mx-imgBorder"]
> ![Vergleichen mit veröffentlichtem Element](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Anzeigen des Endpunkt-JSON-Codes im Testbereich
Sie können den für den Vergleich zurückgegebenen JSON-Code des Endpunkts anzeigen, indem Sie die Ansicht **JSON anzeigen** auswählen.

> [!div class="mx-imgBorder"]
> ![Veröffentlichte JSON-Antwort](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

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
