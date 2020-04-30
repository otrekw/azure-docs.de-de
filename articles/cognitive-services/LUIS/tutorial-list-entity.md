---
title: 'Tutorial: Listenentität – LUIS'
description: Abrufen von Daten, die mit einer vordefinierten Liste von Elementen übereinstimmen. Jedes Element auf der Liste kann Synonyme aufweisen, die ebenfalls exakt übereinstimmen
ms.topic: tutorial
ms.date: 03/12/2020
ms.openlocfilehash: 9530719c43260751d64d7ccf446bc7941078d6e9
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82101126"
---
# <a name="tutorial-get-exact-text-matched-data-from-an-utterance-with-list-entity"></a>Tutorial: Abrufen von Daten mit genauer Textübereinstimmung aus einer Äußerung mit Listenentität

In diesem Tutorial erfahren Sie, wie Sie Daten abrufen, die exakt mit einer vordefinierten Liste von Elementen übereinstimmen.

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Importieren einer App und Verwenden einer vorhandenen Absicht
> * Hinzufügen einer Entität vom Typ „Liste“
> * Trainieren, Veröffentlichen und Abfragen der App, um extrahierte Daten zu erhalten

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="what-is-a-list-entity"></a>Was ist eine Listenentität?

Eine Listenentität ist eine genaue Textübereinstimmung mit den Wörtern in der Äußerung. Jedes Element auf der Liste kann eine Liste mit Synonymen enthalten. Verwenden Sie eine Listenentität, wenn Sie eine exakte Übereinstimmung benötigen.

Erstellen Sie für die importierte Pizza-App eine Listenentität für die verschiedenen Arten von Pizzaböden.

Eine Listenentität ist eine gute Wahl für diese Art von Daten, wenn Folgendes gilt:

* Bei den Datenwerten handelt es sich um einen bekannten Satz.
* Für den Satz werden die maximalen LUIS-[Grenzen](luis-limits.md) dieses Entitätstyps nicht überschritten.
* Der Text in der Äußerung stimmt mit einem Synonym oder dem kanonischen Namen überein (ohne Berücksichtigung der Groß-/Kleinschreibung). Die Liste wird von LUIS nicht über die Übereinstimmung hinaus verwendet. Wortstammerkennung, Pluralformen und andere Varianten lassen sich nur mithilfe einer Listenentität nicht auflösen. Um Varianten zu behandeln, sollten Sie die Verwendung eines [Musters](reference-pattern-syntax.md#syntax-to-mark-optional-text-in-a-template-utterance) mit der optionalen Textsyntax in Erwägung ziehen.

> [!CAUTION]
> Falls Sie unsicher sind, ob Sie eine Listenentität oder eine durch maschinelles Lernen erworbene Entität mit einer Ausdrucksliste als Deskriptor verwenden sollen, empfiehlt sich die Verwendung einer durch maschinelles Lernen erworbenen Entität mit einer Ausdrucksliste, da diese Option flexibler ist. Bei Verwendung dieser Methode kann LUIS die Werte der zu extrahierenden Daten lernen und erweitern.

## <a name="import-example-json-and-add-utterances"></a>Importieren der JSON-Beispieldatei und Hinzufügen von Äußerungen

1.  Laden Sie die [App-JSON-Datei](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-tutorial-with-entities.json) herunter, und speichern Sie sie.

    [!INCLUDE [Import app steps](includes/import-app-steps.md)]

1. Die importierte App verfügt über eine Absicht vom Typ `OrderPizza`. Wählen Sie diese Absicht aus, und fügen Sie einige Äußerungen mit neuen Pizzabodentypen hinzu:

    |Neue Äußerungen|
    |--|--|
    |„please order a pan crust small pepperoni pizza“ (Eine kleine Peperoni-Pan-Pizza bestellen)|
    |„3 thin crust hawaiian pizzas“ (Drei Pizza Hawaii mit dünnem Boden)|
    |„deliver 2 stuffed crust pizzas with bread sticks“ (Zwei Pizzen mit gefülltem Boden und Knabbergebäck liefern)|
    |„one thick crust pizza for pickup“ (Eine Pizza mit dickem Boden zum Abholen)|
    |„one deep dish pepperoni pizza“ (Eine Deep-Dish-Peperoni-Pizza)|

## <a name="crust-list-entity"></a>Listenentität für den Boden

Nachdem die Absicht **OrderPizza** nun über Beispieläußerungen mit Bodentypen verfügt, muss LUIS verstehen, mit welchen Wörtern der Bodentyp angegeben wird.

Im Anschluss folgen einige Beispiele für den primären Namen und für Synonyme:

|Kanonischer Name|Synonyme|
|--|--|
|Deep dish|deep<br>deep dish crust<br>thick<br>thick crust|
|Pan|regular<br>original<br>normal<br>regular crust<br>original crust<br>normal crust|
|Stuffed|stuffed crust|
|Thin|thin crust<br>skinny<br>skinny crust|

1. Klicken Sie im linken Bereich auf **Entitäten**.

1. Wählen Sie **+ Erstellen** aus.

1. Geben Sie im Popupdialogfeld `CrustList` als Entitätsname und **List** (Liste) als Entitätstyp ein. Wählen Sie **Weiter** aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Popupdialogfeld zum Erstellen einer neuen Entität](media/luis-quickstart-intent-and-list-entity/create-pizza-crust-list-entity.png)

1. Geben Sie auf der Seite **Create a list entity** (Listenentität erstellen) die kanonischen Namen und die Synonyme für die kanonischen Namen ein, und wählen Sie anschließend **Erstellen** aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Hinzufügen von Elementen zur Listenentität](media/luis-quickstart-intent-and-list-entity/add-pizza-crust-items-list-entity.png)

    Wenn Sie einer LUIS-App eine Listenentität hinzufügen, muss der Text nicht mit der Listenentität [bezeichnet](label-entity-example-utterance.md) werden. Sie wird auf alle Äußerungen in allen Absichten angewendet.

## <a name="train-the-app-before-testing-or-publishing"></a>Trainieren der App vor dem Testen oder Veröffentlichen

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app-to-query-from-the-endpoint"></a>Veröffentlichen der App, um sie vom Endpunkt aus abzufragen

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Abrufen von Absicht und Entitätsvorhersage vom Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

2. Geben Sie in der Adressleiste am Ende der URL die folgende Äußerung ein:

    `Deliver 2 deep dish hawaiian pizzas and a thin pepperoni`

    Der letzte Parameter der Abfragezeichenfolge lautet `query` (für die Abfrage (**query**) der Äußerung).


    ```json
    {
        "query": "Deliver 2 deep dish hawaiian pizzas and a thin pepperoni",
        "prediction": {
            "topIntent": "OrderPizza",
            "intents": {
                "OrderPizza": {
                    "score": 0.9957229
                },
                "None": {
                    "score": 0.016832687
                },
                "Confirm": {
                    "score": 0.0015708931
                },
                "Greeting": {
                    "score": 0.00057060417
                }
            },
            "entities": {
                "number": [
                    2
                ],
                "CrustList": [
                    [
                        "Deep dish"
                    ],
                    [
                        "Thin"
                    ]
                ],
                "$instance": {
                    "number": [
                        {
                            "type": "builtin.number",
                            "text": "2",
                            "startIndex": 8,
                            "length": 1,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "CrustList": [
                        {
                            "type": "CrustList",
                            "text": "deep dish",
                            "startIndex": 10,
                            "length": 9,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        },
                        {
                            "type": "CrustList",
                            "text": "thin",
                            "startIndex": 42,
                            "length": 4,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
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

    Die Bodentypen wurden als exakte Textübereinstimmungen gefunden und in der JSON-Antwort zurückgegeben. Diese Information wird von der Clientanwendung verwendet, um die Bestellung zu verarbeiten.

[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>Verwandte Informationen

* Informationen zum Konzept der [Listenentität](luis-concept-entity-types.md#list-entity)
* [Informationen zum Trainieren](luis-how-to-train.md)
* [Informationen zum Veröffentlichen](luis-how-to-publish-app.md)
* [Informationen zum Testen im LUIS-Portal](luis-interactive-test.md)
* [Entitäten und ihr Zweck in LUIS](luis-concept-entity-types.md)
* [Entität vom Typ „RegEx“](reference-entity-regular-expression.md?tabs=V3)
* [Hinzufügen von Entitäten zum Extrahieren von Daten](luis-how-to-add-entities.md)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Beispieläußerungen hinzugefügt und eine Listenentität erstellt, um exakte Textübereinstimmungen aus den Äußerungen zu extrahieren. Nach dem Trainieren und Veröffentlichen der App hat eine Abfrage des Endpunkts die Absicht identifiziert und die extrahierten Daten zurückgegeben.

> [!div class="nextstepaction"]
> [Tutorial: Extrahieren von Daten mit ähnlichem Kontext aus einer Äußerung](tutorial-entity-roles.md)

