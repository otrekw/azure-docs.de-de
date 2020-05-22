---
title: 'Tutorial: Extrahieren strukturierter Daten mit durch maschinelles Lernen erworbener Entität – LUIS'
description: Extrahieren Sie strukturierte Daten aus einer Äußerung mithilfe der durch maschinelles Lernen erworbenen Entität. Fügen Sie untergeordnete Entitäten mit Features hinzu, um die Genauigkeit der Extraktion zu erhöhen.
ms.topic: tutorial
ms.date: 05/08/2020
ms.openlocfilehash: d1bc8fc6aac52e264cb4352ca05f9df45ccfc50e
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588869"
---
# <a name="tutorial-extract-structured-data-from-user-utterance-with-machine-learned-entities-in-language-understanding-luis"></a>Tutorial: Extrahieren strukturierter Daten aus Benutzeräußerungen mithilfe der durch maschinelles Lernen erworbenen Entitäten in Language Understanding (LUIS)

In diesem Tutorial extrahieren Sie strukturierte Daten aus einer Äußerung mithilfe der durch maschinelles Lernen erworbenen Entität.

Die durch maschinelles Lernen erworbene Entität unterstützt das [Modellaufschlüsselungskonzept](luis-concept-model.md#v3-authoring-model-decomposition), indem sie untergeordnete Entitäten von Entitäten mit [Features](luis-concept-feature.md) bereitstellt.

**In diesem Tutorial lernen Sie Folgendes:**

> [!div class="checklist"]
> * Importieren der Beispiel-App
> * Hinzufügen einer durch maschinelles Lernen erworbenen Entität
> * Hinzufügen von untergeordneten Entitäten und Features
> * Trainieren, Testen und Veröffentlichen einer App
> * Abrufen der Entitätsvorhersage vom Endpunkt

[!INCLUDE [LUIS Free account](includes/quickstart-tutorial-use-free-starter-key.md)]


## <a name="why-use-a-machine-learned-entity"></a>Gründe für die Verwendung einer durch maschinelles Lernen erworbenen Entität

In diesem Tutorial wird eine durch maschinelles Lernen erworbene Entität hinzugefügt, um Daten aus der Äußerung eines Benutzers zu extrahieren.

Die Entität definiert die Daten, die aus der Äußerung extrahiert werden sollen. Dies umfasst die Angabe eines Namens, eines Typs (falls möglich), einer beliebigen Auflösung der Daten bei Mehrdeutigkeit und des genauen Textes, aus dem die Daten bestehen.

Gehen Sie wie folgt vor, um die Daten zu definieren:
* Erstellen der Entität
* Bezeichnen Sie den Text in den Beispieläußerungen, die die Entität darstellen. Diese beschrifteten Beispiele vermitteln LUIS, worum es sich bei der Entität handelt und wo sie sich in einer Äußerung befindet.

## <a name="entity-decomposability-is-important"></a>Die Möglichkeit zur Aufschlüsselung von Entitäten ist wichtig.

Die Möglichkeit zur Aufschlüsselung von Entitäten ist sowohl für die Absichtsvorhersage als auch für die Datenextraktion mit der Entität wichtig.

Beginnen Sie mit einer durch maschinelles Lernen erworbenen Entität, die den Anfang und die Entität der höchsten Ebene für die Datenextraktion darstellt. Unterteilen Sie die Entität anschließend in untergeordnete Entitäten.

Zu Beginn der Erstellung Ihrer App wissen Sie unter Umständen noch nicht, wie detailliert Ihre Entität sein soll. Es ist aber eine bewährte Methode, mit einer durch maschinelles Lernen erworbenen Entität zu beginnen und dann mit der Unterteilung in untergeordnete Entitäten fortzufahren, während die Entwicklung Ihrer App voranschreitet.

In diesem Tutorial wird eine durch maschinelles Lernen erworbene Entität erstellt, um eine Bestellung für eine Pizza-App darzustellen. Die Entität extrahiert zunächst bestellungsbezogenen Text, z. B. zur Größe und Menge.

Bei einer Äußerung wie `Please deliver one large cheese pizza to me` sollte `one large cheese pizza` als Bestellung und dann `1` als Menge und `large` als Größe extrahiert werden.

## <a name="download-json-file-for-app"></a>Herunterladen der JSON-Datei für die App

Laden Sie die [App-JSON-Datei](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-intents-only.json) herunter, und speichern Sie sie.

## <a name="import-json-file-for-app"></a>Importieren der JSON-Datei für die App

[!INCLUDE [Import app steps](includes/import-app-steps.md)]

## <a name="label-text-as-entities-in-example-utterances"></a>Bezeichnungstext als Entitäten in Beispieläußerungen

Um Details einer Pizzabestellung zu extrahieren, erstellen Sie eine durch maschinelles Lernen erworbene `Order`-Entität auf höchster Ebene.

1. Wählen Sie auf der Seite **Absichten** die Absicht **OrderPizza** (Pizza bestellen) aus.

1. Wählen Sie in der Liste der Beispieläußerungen die folgende Äußerung aus.

    |Beispieläußerung zu einer Bestellung|
    |--|
    |`pickup a cheddar cheese pizza large with extra anchovies`|

    Beginnen Sie mit der Auswahl kurz vor dem linken Text von `pickup` (#1), dann gehen Sie kurz über den rechten Text hinaus, `anchovies` (#2 – damit endet der Bezeichnungsprozess). Ein Popupmenü wird angezeigt. Geben Sie im Popupfeld den Namen der Entität als `Order` (#3) ein. Wählen Sie dann `Order - Create new entity` aus der Liste aus (#4).

    ![Bezeichnen von Anfang und Ende des Textes für die vollständige Bestellung](media/tutorial-machine-learned-entity/mark-complete-order.png)

    > [!NOTE]
    > Eine Entität ist nicht immer die gesamte Äußerung. In diesem speziellen Fall gibt `pickup` an, wie die Bestellung empfangen werden soll. Aus konzeptioneller Sicht sollte `pickup` Teil der bezeichneten Entität für die Bestellung sein.

1. Wählen Sie im Feld **Entitätstyp auswählen** die Option **Struktur hinzufügen** und dann **Weiter** aus. Die Struktur ist erforderlich, um untergeordnete Entitäten wie die Größe und Menge hinzuzufügen.

    ![Hinzufügen einer Struktur zur Entität](media/tutorial-machine-learned-entity/add-structure-to-entity.png)

1. Fügen Sie im Feld zum **Erstellen einer durch maschinelles Lernen erworbenen Entität** im Feld **Struktur** die Option `Size` hinzu, und drücken Sie dann die EINGABETASTE.
1. Wählen Sie zum Hinzufügen eines **Features** im Bereich **Features** das Pluszeichen (`+`) und dann die Option **Neue Liste mit Ausdrücken erstellen** aus.

1. Geben Sie im Feld **Neue Liste mit Ausdrücken erstellen** den Namen `SizeFeature` und dann die Werte für `small`, `medium` und `large` ein. Wenn das Feld **Vorschläge** ausgefüllt ist, wählen Sie `extra large` und `xl` aus. Wählen Sie **Fertig** aus, um die neue Ausdrucksliste zu erstellen.

    Dieses Ausdruckslistenfeature hilft der untergeordneten Entität `Size` dabei, Wörter mit Bezug zur Größe zu finden, indem Beispielwörter bereitgestellt werden. Diese Liste muss nicht jedes auf die Größe bezogene Wort enthalten, sondern sollte Wörter einbeziehen, die voraussichtlich die Größe angeben.

    ![Erstellen eines Features für die untergeordnete Entität für Größe](media/tutorial-machine-learned-entity/size-entity-size-descriptor-phrase-list.png)

1. Wählen Sie im Fenster **Durch maschinelles Lernen erworbene Entität** die Option **Erstellen** aus, um die Erstellung der untergeordneten Entität `Size` abzuschließen.

    Die Entität `Order` mit der Entität `Size` wird erstellt, aber nur die Entität `Order` wurde auf die Äußerung angewendet. Sie müssen den `Size`-Entitätstext in der Beispieläußerung bezeichnen.

1. Bezeichnen Sie in derselben Beispieläußerung die untergeordnete **Size**-Entität `large`, indem Sie das Wort und dann die Entität **Size** aus der Dropdownliste auswählen.

    ![Bezeichnen Sie die Entität „Größe“ für Text in der Äußerung.](media/tutorial-machine-learned-entity/mark-and-create-size-entity.png)

    Die Linie unter dem Text ist durchgezogen, da die Bezeichnung und die Vorhersage übereinstimmen, nachdem Sie den Text _explizit_ beschriftet haben.

1. Bezeichnen Sie die `Order`-Entität in den restlichen Äußerungen zusammen mit der Entität „Größe“. Die eckigen Klammern im Text zeigen die bezeichnete `Order`-Entität und die darin enthaltene `Size`-Entität.

    |Beispieläußerungen für die Bestellung|
    |--|
    |`can i get [a pepperoni pizza and a can of coke] please`|
    |`can i get [a [small] pizza with onions peppers and olives]`|
    |`[delivery for a [small] pepperoni pizza]`|
    |`i need [2 [large] cheese pizzas 6 [large] pepperoni pizzas and 1 [large] supreme pizza]`|

    ![Erstellen Sie Entitäten und untergeordnete Entitäten in allen verbleibenden Beispieläußerungen.](media/tutorial-machine-learned-entity/entity-subentity-labeled-not-trained.png)

    > [!CAUTION]
    > Wie gehen Sie mit impliziten Daten wie dem Buchstaben `a` um, der eine einzelne Pizza auszeichnet? Oder das Fehlen von `pickup` und `delivery`, um anzugeben, wo die Pizza erwartet wird? Oder das Fehlen einer Größe, um Ihre Standardgröße bzw. „Klein“ oder „Groß“ anzugeben? Betrachten Sie den Umgang mit impliziten Daten als Teil Ihrer Geschäftsregeln in der Clientanwendung anstelle von oder ergänzend zu LUIS.

1. Wählen Sie zum Trainieren der App **Trainieren** aus. Das Training wendet die Änderungen, z. B. die neuen Entitäten und die bezeichneten Äußerungen, auf das aktive Modell an.

1. Nach dem Training fügen Sie der Absicht eine neue Beispieläußerung hinzu, um zu erfahren, wie gut LUIS die durch maschinelles Lernen erworbene Entität versteht.

    |Beispieläußerung zu einer Bestellung|
    |--|
    |`pickup XL meat lovers pizza`|

    Die oberste Entität `Order` ist bezeichnet, und die untergeordnete Entität `Size` ist ebenfalls mit gepunkteten Linien gekennzeichnet.

    ![Neues Beispieläußerung mit Vorhersage durch Entität](media/tutorial-machine-learned-entity/new-example-utterance-predicted-with-entity.png)

    Die gepunktete Linie zeigt die Vorhersage an.

1. Wählen Sie beim Ändern der Vorhersage in eine bezeichnete Entität die Zeile aus, und wählen Sie dann **Entitätsvorhersagen bestätigen** aus.

    ![Akzeptieren Sie die Vorhersage, indem Sie „Entitätsvorhersage bestätigen“ auswählen.](media/tutorial-machine-learned-entity/confirm-entity-prediction-for-new-example-utterance.png)

    An diesem Punkt funktioniert die durch maschinelles Lernen erworbene Entität, da sie die Entität innerhalb einer neuen Beispieläußerung finden kann. Wenn die Entität beim Hinzufügen von Beispieläußerungen nicht korrekt vorhergesagt wird, sollten Sie die Entität und die untergeordneten Entitäten bezeichnen. Wenn die Entität richtig vorhergesagt wird, stellen Sie sicher, dass Sie die Vorhersagen bestätigen.


<a name="create-subcomponent-entity-with-constraint-to-help-extract-data"></a>

## <a name="add-subentity-with-feature-of-prebuilt-entity"></a>Hinzufügen einer untergeordneten Entität mit einem Feature für eine vordefinierte Entität

Die Bestellinformationen sollten auch eine Angabe dazu enthalten, welche Menge eines Artikels die Bestellung umfasst, z. B. die Anzahl von Pizzas. Zum Extrahieren dieser Daten muss `Order` eine untergeordnete Entität hinzugefügt werden, die durch maschinelles Lernen erworben wurde. Diese untergeordnete Entität benötigt dann ein erforderliches Feature mit einer vordefinierten Anzahl. Indem ein Feature mit einer vordefinierten Anzahl verwendet wird, findet und extrahiert die Entität Zahlen – unabhängig davon, ob es sich bei dem Text um eine Ziffer (`2`) oder Text (`two`) handelt.

## <a name="add-prebuilt-number-entity-to-app"></a>Hinzufügen einer vordefinierten Zahlenentität zu einer App
Die Bestellinformationen sollten auch enthalten, wie viele Artikel die Bestellung umfasst, z. B. die Anzahl von Pizzas. Um diese Daten zu extrahieren, muss `Order` eine neue durch maschinelles Lernen erworbene Unterkomponente hinzugefügt werden, und für diese Komponente wird ein erforderliches Feature mit einer vordefinierten Anzahl benötigt. Durch die Einschränkung der Entität auf eine vordefinierte Nummer findet und extrahiert die Entität Zahlen, unabhängig davon, ob es sich bei dem Text um eine Ziffer (`2`) oder Text (`two`) handelt.

Beginnen Sie mit dem Hinzufügen der vordefinierten Nummernentität zur App.

1. Wählen Sie **Entitäten** aus dem linken Menü und dann **+ Vordefinierte Entität hinzufügen** aus.

1. Suchen Sie im Feld **Vordefinierte Entitäten hinzufügen** nach **Nummer** und wählen Sie dies aus. Anschließend wählen Sie **Fertig** aus.

    ![Hinzufügen einer vordefinierten Entität](media/tutorial-machine-learned-entity/add-prebuilt-entity-as-constraint-to-quantity-subcomponent.png)

    Die vordefinierte Entität wird der App hinzugefügt, aber es ist noch kein Feature.

## <a name="create-subentity-entity-with-required-feature-to-help-extract-data"></a>Erstellen einer untergeordneten Entität mit dem erforderlichen Feature für die Unterstützung der Datenextraktion

Die Entität `Order` sollte die untergeordnete Entität `Quantity` aufweisen, um für die Bestellung die Anzahl von Artikeln zu ermitteln. Für die Menge sollte ein erforderliches Feature mit einer vordefinierten Anzahl verwendet werden, damit die extrahierten Daten umgehend auf der Grundlage des Namens für die Clientanwendung zur Verfügung stehen.

Ein erforderliches Feature wird als Textübereinstimmung angewendet, entweder mit genauer Übereinstimmung (z. B. eine Listenentität) oder durch reguläre Ausdrücke (z. B. eine Entität mit regulärem Ausdruck oder eine vordefinierte Entität).

Indem eine nicht durch maschinelles Lernen erworbene Entität als Feature verwendet wird, wird nur übereinstimmender Text extrahiert.

1. Wählen Sie **Entitäten** und dann die `Order`-Einheit aus.
1. Wählen Sie **+ Entität hinzufügen** aus, geben Sie den Namen `Quantity` ein, und drücken Sie dann die EINGABETASTE, um der Entität `Order` die neue untergeordnete Entität hinzuzufügen.
1. Wählen Sie nach Erhalt der Erfolgsmeldung unter **Erweiterte Optionen** den Einschränkungsstift aus.
1. Wählen Sie in der Dropdownliste die vordefinierte Nummer aus.

    ![Erstellen Sie eine Mengenentität mit vordefinierter Nummer als Einschränkung.](media/tutorial-machine-learned-entity/create-constraint-from-prebuilt-number.png)

    Die Entität `Quantity` wird angewendet, wenn Text der vordefinierten Zahlenentität entspricht.

    Die Entität mit dem erforderlichen Feature wird erstellt, aber noch nicht auf die Beispieläußerungen angewendet.

    > [!NOTE]
    > Eine untergeordnete Entität kann mit bis zu fünf Ebenen in einer untergeordneten Entität geschachtelt werden. Obwohl dies in diesem Artikel nicht dargestellt wird, ist es über das Portal und die API verfügbar.

## <a name="label-example-utterance-to-teach-luis-about-the-entity"></a>Bezeichnen einer Beispieläußerung, damit LUIS über die Entität lernen kann

1. Wählen Sie im linken Navigationsbereich **Absichten** und dann die Absicht **OrderPizza** aus. Die drei Nummern in den folgenden Äußerungen sind bezeichnet, liegen aber optisch unter der `Order`-Entitätslinie. Diese niedrigere Ebene bedeutet, dass die Entitäten gefunden, aber nicht als Teil der Entität `Order` betrachtet werden.

    ![Die vordefinierte Nummer wird gefunden, aber noch nicht als Teil der Entität „Bestellung“ betrachtet.](media/tutorial-machine-learned-entity/prebuilt-number-not-part-of-order-entity.png)

1. Bezeichnen Sie die Nummern mit der `Quantity`-Einheit, indem Sie die `2` in der Beispieläußerung und dann `Quantity` aus der Liste auswählen. Bezeichnen Sie die `6` und die `1` in derselben Beispieläußerung.

    ![Bezeichnen Sie den Text mit der Entität „Menge“.](media/tutorial-machine-learned-entity/mark-example-utterance-with-quantity-entity.png)

## <a name="train-the-app-to-apply-the-entity-changes-to-the-app"></a>Trainieren der App zum Anwenden der Entitätsänderungen auf die App

Wählen Sie **Trainieren** aus, um die App mit diesen neuen Äußerungen zu trainieren. Nach dem Training wird die untergeordnete Entität `Quantity` in der Entität `Order` richtig vorhergesagt. Diese korrekte Vorhersage wird durch eine durchgezogene Linie angegeben.

![Trainieren Sie die App, und überprüfen Sie dann die Beispieläußerungen.](media/tutorial-machine-learned-entity/trained-example-utterances.png)

An dieser Stelle weist die Bestellung einige Details auf, die extrahiert werden können (Größe, Menge und gesamter Text der Bestellung). Es gibt eine weitere Verfeinerung der `Order`-Entität, z. B. Pizzabeläge, Krustentyp und Nebenbestellungen. Jede dieser Komponenten sollte als untergeordnete Entität der Entität `Order` erstellt werden.

## <a name="test-the-app-to-validate-the-changes"></a>Testen der App zum Überprüfen der Änderungen

Testen Sie die App über den interaktiven **Testbereich**. Mit diesem Prozess können Sie eine neue Äußerung eingeben und dann die Vorhersageergebnisse anzeigen, um zu überprüfen, wie gut die aktive und trainierte App funktioniert. Die Absichtsvorhersage sollte ziemlich zuverlässig sein (über 70 %) und die Entitätsextraktion sollte mindestens die `Order`-Entität aufnehmen. Die Details der Entität „Bestellung“ fehlen möglicherweise, da fünf Äußerungen nicht ausreichen, um jeden Fall zu bearbeiten.

1. Klicken Sie auf der oberen Navigationsleiste auf **Test** (Testen).
1. Geben Sie die Äußerung `deliver a medium veggie pizza` ein, und wählen Sie die EINGABETASTE aus. Das aktive Modell hat die richtige Absicht mit einer Zuverlässigkeit von über 70 % vorhergesagt.

    ![Geben Sie eine neue Äußerung ein, um die Absicht zu testen.](media/tutorial-machine-learned-entity/interactive-test-panel-with-first-utterance.png)

1. Wählen Sie **Überprüfen** aus, um die Entitätsvorhersagen anzuzeigen.

    ![Zeigen Sie die Entitätsvorhersagen im interaktiven Testbereich an.](media/tutorial-machine-learned-entity/interactive-test-panel-with-first-utterance-and-entity-predictions.png)

    Die Größe wurde richtig identifiziert. Denken Sie daran, dass die Beispieläußerungen in der Absicht `OrderPizza` kein Beispiel für `medium` als Größe aufweisen, sondern dass ein Feature mit einer `SizeFeature`-Ausdrucksliste verwendet wird, die „medium“ enthält.

    Die Menge ist nicht richtig vorhergesagt. Legen Sie in Ihrer Clientanwendung die Größe standardmäßig auf „1“ fest, wenn in der LUIS-Vorhersage keine Größe zurückgegeben wird, um dieses Problem zu korrigieren.

## <a name="publish-the-app-to-access-it-from-the-http-endpoint"></a>Veröffentlichen der App für den Zugriff über den HTTP-Endpunkt

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-and-entity-prediction-from-http-endpoint"></a>Abrufen von Absicht und Entitätsvorhersage vom HTTP-Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Ersetzen Sie in der Adressleiste am Ende der URL die Angabe _YOUR_QUERY_HERE_ durch die Abfrage, die Sie im interaktiven Testbereich eingegeben haben.

    `deliver a medium veggie pizza`

    Der letzte Parameter der Abfragezeichenfolge lautet `query` (für die Abfrage (**query**) der Äußerung).

    ```json
    {
        "query": "deliver a medium veggie pizza",
        "prediction": {
            "topIntent": "OrderPizza",
            "intents": {
                "OrderPizza": {
                    "score": 0.7812769
                },
                "None": {
                    "score": 0.0314020254
                },
                "Confirm": {
                    "score": 0.009299271
                },
                "Greeting": {
                    "score": 0.007551549
                }
            },
            "entities": {
                "Order": [
                    {
                        "Size": [
                            "medium"
                        ],
                        "$instance": {
                            "Size": [
                                {
                                    "type": "Size",
                                    "text": "medium",
                                    "startIndex": 10,
                                    "length": 6,
                                    "score": 0.9955588,
                                    "modelTypeId": 1,
                                    "modelType": "Entity Extractor",
                                    "recognitionSources": [
                                        "model"
                                    ]
                                }
                            ]
                        }
                    }
                ],
                "$instance": {
                    "Order": [
                        {
                            "type": "Order",
                            "text": "a medium veggie pizza",
                            "startIndex": 8,
                            "length": 21,
                            "score": 0.7983857,
                            "modelTypeId": 1,
                            "modelType": "Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```


[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>Verwandte Informationen

* [Tutorial: Absichten](luis-quickstart-intents-only.md)
* [Konzept: Entitäten](luis-concept-entity-types.md) – Informationen zum Konzept
* [Konzept: Features](luis-concept-feature.md) – Informationen zum Konzept
* [Informationen zum Trainieren](luis-how-to-train.md)
* [Informationen zum Veröffentlichen](luis-how-to-publish-app.md)
* [Informationen zum Testen im LUIS-Portal](luis-interactive-test.md)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial verwendet die App eine durch maschinelles Lernen erworbene Entität, um die Absicht der Äußerung eines Benutzers zu finden und Details aus dieser Äußerung zu extrahieren. Die Verwendung der durch maschinelles Lernen erworbenen Entität ermöglicht es Ihnen, die Details der Entität aufzuschlüsseln.

> [!div class="nextstepaction"]
> [Hinzufügen einer vorgefertigten keyPhrase-Entität](luis-quickstart-intent-and-key-phrase.md)
