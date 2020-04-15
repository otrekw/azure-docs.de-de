---
title: 'Tutorial: Entität vom Typ „Regulärer Ausdruck“: LUIS'
description: Hier erfahren Sie, wie Sie mithilfe der Entität vom Typ „Regulärer Ausdruck“ einheitlich formatierte Daten aus einer Äußerung extrahieren.
ms.topic: tutorial
ms.date: 04/01/2020
ms.openlocfilehash: 5b585ee52880c474d3f2736b34a267012b390aad
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80545833"
---
# <a name="tutorial-get-well-formatted-data-from-the-utterance"></a>Tutorial: Abrufen von gut formatierten Daten aus der Äußerung
In diesem Tutorial erstellen Sie eine Entität vom Typ „Regulärer Ausdruck“, um einheitlich formatierte Daten aus einer Äußerung zu extrahieren.

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Importieren einer App
> * Hinzufügen einer Absicht
> * Hinzufügen einer Entität vom Typ „Regulärer Ausdruck“
> * Trainieren, Veröffentlichen und Abfragen der App, um extrahierte Daten zu erhalten

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="regular-expression-entities"></a>RegEx-Entitäten

Verwenden Sie die Entität vom Typ „Regulärer Ausdruck“, um ordnungsgemäß formatierten Text aus einer Äußerung zu extrahieren. Zwar wird die Absicht der Äußerung immer durch Machine Learning bestimmt, doch wird bei diesem spezifischen Entitätstyp kein Machine Learning verwendet. Die Entität vom Typ „Regulärer Ausdruck“ eignet sich für jeglichen Text, der konsistent durch einen [regulären Ausdruck](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) dargestellt werden kann.

`Send pizza delivery time to x123432`

In diesem Beispiel wird ein _Kurzcode_ für den SMS-Versand verwendet. Bei diesem Kurzcode handelt es sich um einen fünf- oder sechsstelligen numerischen Code mit dem Präfix „x“, der mit dem regulären Ausdruck `x\d{5,6}` beschrieben werden kann.

Wenn Sie einer LUIS-App eine Entität vom Typ „Regulärer Ausdruck“ hinzufügen, muss der Text nicht mit dieser Entität [bezeichnet](label-entity-example-utterance.md) werden. Sie wird auf alle Äußerungen in allen Absichten angewendet.

## <a name="import-example-json-to-begin-app"></a>Importieren von JSON-Beispielcode zum Starten einer App

1.  Laden Sie die [App-JSON-Datei](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-tutorial-with-entities.json) herunter, und speichern Sie sie.

[!INCLUDE [Import app steps](includes/import-app-steps.md)]

## <a name="create-intent-for-sending-confirmation-text-messages"></a>Erstellen einer Absicht zum Senden von Bestätigungen per SMS

1. Wählen Sie **+ Erstellen** aus, um eine neue Absicht für die Klassifizierung der Absicht einer Äußerung zum Senden einer Bestätigungs-SMS zu erstellen.

1. Geben Sie im Popupdialogfeld die Zeichenfolge `ConfirmationText` ein, und klicken Sie anschließend auf **Fertig**.

1. Fügen Sie der Absicht Beispieläußerungen hinzu.

    |Beispiele für Äußerungen|
    |--|
    |„Send pizza delivery time to x123432“ (Pizzalieferzeit an x123432 senden)|
    |„Txt x234567 for time“ (SMS an x234567 für Lieferzeitpunkt)|
    |„x23987 for the notice“ (x23987 für Benachrichtigung)|

    Bei der Extraktion von Entitäten, die durch maschinelles Lernen erworben werden, empfiehlt es sich, Beispiele bereitzustellen, die die Entität in verschiedenen Äußerungen enthalten. Bei dieser nicht durch maschinelles Lernen erworbenen Entität ist dies jedoch nicht erforderlich. Text muss lediglich dem regulären Ausdruck entsprechen, um extrahiert zu werden.

## <a name="use-the-regular-expression-entity-for-well-formatted-data"></a>Verwenden der Entität „Regulärer Ausdruck“ für gut formatierte Daten
Erstellen Sie eine Entität vom Typ „Regulärer Ausdruck“, um die Textnummer abzugleichen. Dieser reguläre Ausdruck gleicht Text ab und ignoriert sowohl Groß-/Kleinschreibung als auch kulturelle Varianten.

1. Klicken Sie im linken Bereich auf **Entitäten**.

1. Wählen Sie auf der Listenseite „Entitäten“ die Option **+ Erstellen** aus.

1. Geben Sie im daraufhin angezeigten Popupdialogfeld `ConfirmationTextRegEx` als Name der neuen Entität ein, wählen Sie **RegEx** als Entitätstyp aus, und wählen Sie anschließend **Weiter** aus.

    > [!div class="mx-imgBorder"]
    > ![Erste Schritte für die Erstellung einer Entität vom Typ „Regulärer Ausdruck“](./media/luis-quickstart-intents-regex-entity/pizza-create-new-entity.png)

1. Geben Sie auf der Seite **Create a regex entity** (Entität vom Typ „Regulärer Ausdruck“ erstellen) für **RegEx** den Wert `x\d{5,6}` ein, und wählen Sie anschließend **Erstellen** aus.

    > [!div class="mx-imgBorder"]
    > ![Eingeben eines regulären Ausdrucks zum Extrahieren von Daten aus der Beispieläußerung](./media/luis-quickstart-intents-regex-entity/pizza-set-regular-expression-for-new-entity.png)

1. Wählen Sie im linken Menü die Option **Absichten** und dann die Absicht **ConfirmationText** aus, um den gekennzeichneten regulären Ausdruck in den Äußerungen zu sehen.

    > [!div class="mx-imgBorder"]
    > ![Anzeigen des gekennzeichneten regulären Ausdrucks in Beispielausdrücken](./media/luis-quickstart-intents-regex-entity/pizza-reg-ex-entity-shown-example-utterances-intent.png)

    Da die Entität keine durch maschinelles Lernen erworbene Entität ist, wird sie sofort nach der Erstellung auf die Äußerungen angewendet und im LUIS-Portal angezeigt.

## <a name="train-the-app-before-testing-or-publishing"></a>Trainieren der App vor dem Testen oder Veröffentlichen

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app-to-query-from-the-endpoint"></a>Veröffentlichen der App, um sie vom Endpunkt aus abzufragen

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Abrufen von Absicht und Entitätsvorhersage vom Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Ersetzen Sie in der Adressleiste am Ende der URL _IHRE_ABFRAGE_ durch:

    `Text my pizza delivery to x23456 x234567 x12345`

    ```json
    {
        "query": "Text my pizza delivery to x23456 x234567 x12345",
        "prediction": {
            "topIntent": "ConfirmationText",
            "intents": {
                "ConfirmationText": {
                    "score": 0.7061845
                },
                "ModifyOrder": {
                    "score": 0.196021989
                },
                "None": {
                    "score": 0.02342912
                },
                "Test-Pizza": {
                    "score": 0.01213586
                },
                "CancelOrder": {
                    "score": 0.0063042324
                },
                "Confirmation": {
                    "score": 0.0058615827
                },
                "Greetings": {
                    "score": 0.00398947531
                }
            },
            "entities": {
                "ConfirmationTextRegEx": [
                    "x23456",
                    "x234567",
                    "x12345"
                ],
                "$instance": {
                    "ConfirmationTextRegEx": [
                        {
                            "type": "ConfirmationTextRegEx",
                            "text": "x23456",
                            "startIndex": 26,
                            "length": 6,
                            "modelTypeId": 8,
                            "modelType": "Regex Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        },
                        {
                            "type": "ConfirmationTextRegEx",
                            "text": "x234567",
                            "startIndex": 33,
                            "length": 7,
                            "modelTypeId": 8,
                            "modelType": "Regex Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        },
                        {
                            "type": "ConfirmationTextRegEx",
                            "text": "x12345",
                            "startIndex": 41,
                            "length": 6,
                            "modelTypeId": 8,
                            "modelType": "Regex Entity Extractor",
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

    Durch die Verwendung einer Entität vom Typ „Regulärer Ausdruck“ extrahiert LUIS benannte Daten. Dies ist programmatisch hilfreicher für die Clientanwendung, die die JSON-Antwort erhält.


[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>Verwandte Informationen

* [Entitäten und ihr Zweck in LUIS](luis-concept-entity-types.md)
* [Entität vom Typ „RegEx“](reference-entity-regular-expression.md?tabs=V3)
* [Hinzufügen von Entitäten zum Extrahieren von Daten](luis-how-to-add-entities.md)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wurden eine neue Absicht erstellt, Beispieläußerungen hinzugefügt und dann eine Entität vom Typ „Regulärer Ausdruck“ erstellt, um korrekt formatierte Daten aus den Äußerungen zu extrahieren. Nach dem Trainieren und Veröffentlichen der App hat eine Abfrage des Endpunkts die Absicht identifiziert und die extrahierten Daten zurückgegeben.

> [!div class="nextstepaction"]
> [Erfahren Sie mehr über die Listenentität](tutorial-list-entity.md)

