---
title: 'Tutorial: Überprüfung von Endpunktäußerungen: LUIS'
description: In diesem Tutorial verbessern Sie App-Vorhersagen, indem Sie über den LUIS-HTTP-Endpunkt erhaltene Äußerungen, bei denen LUIS unsicher ist, überprüfen bzw. korrigieren. Bei einigen Äußerungen kann eine Überprüfung hinsichtlich der Absicht und bei anderen eine Überprüfung hinsichtlich der Entität erforderlich sein.
services: cognitive-services
ms.topic: tutorial
ms.date: 06/22/2020
ms.openlocfilehash: c2df8cdba3422c522aa4ccf1fe4138a510355d12
ms.sourcegitcommit: 74ba70139781ed854d3ad898a9c65ef70c0ba99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85445922"
---
# <a name="tutorial-fix-unsure-predictions-by-reviewing-endpoint-utterances"></a>Tutorial: Beheben unsicherer Vorhersagen durch Überprüfung von Endpunktäußerungen
In diesem Tutorial verbessern Sie App-Vorhersagen, indem Sie über den LUIS-HTTPS-Endpunkt erhaltene Äußerungen, bei denen LUIS unsicher ist, überprüfen bzw. korrigieren. Endpunktäußerungen sollten regelmäßig im Rahmen der geplanten LUIS-Wartung überprüft werden.

Dieser Überprüfungsprozess ermöglicht es LUIS, sich mit Ihrer App-Domäne vertraut zu machen. Die in der Überprüfungsliste angezeigten Äußerungen werden von LUIS ausgewählt. Für diese Liste gilt Folgendes:

* Sie gilt spezifisch für die App.
* Sie soll die Vorhersagegenauigkeit der App verbessern.
* Sie sollte in regelmäßigen Abständen überprüft werden.

Indem Sie die Endpunktäußerungen überprüfen, verifizieren bzw. korrigieren Sie die vorhergesagte Absicht der Äußerung.

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Importieren der Beispiel-App
> * Überprüfen von Endpunktäußerungen
> * Trainieren und Veröffentlichen der App
> * Abfragen des App-Endpunkts zum Anzeigen der LUIS-JSON-Antwort

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="download-json-file-for-app"></a>Herunterladen der JSON-Datei für die App

Laden Sie die [App-JSON-Datei](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-sentiment-HumanResources.json?raw=true) herunter, und speichern Sie sie.

## <a name="import-json-file-for-app"></a>Importieren der JSON-Datei für die App

[!INCLUDE [Import app steps](includes/import-app-steps.md)]

## <a name="train-the-app-to-apply-the-entity-changes-to-the-app"></a>Trainieren der App zum Anwenden der Entitätsänderungen auf die App

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app-to-access-it-from-the-http-endpoint"></a>Veröffentlichen der App für den Zugriff über den HTTP-Endpunkt

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="add-utterances-at-the-endpoint"></a>Hinzufügen von Äußerungen am Endpunkt

In dieser App sind Absichten und Entitäten vorhanden, aber keine Endpunktverwendung. Diese Endpunktverwendung wird benötigt, um die App mithilfe der Überprüfung von Endpunktäußerungen zu verbessern.

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Ersetzen Sie in der Adressleiste am Ende der URL _IHRE_ABFRAGE_ durch die Äußerungen in der folgenden Tabelle. Übermitteln Sie jede Äußerung, und rufen Sie das Ergebnis ab. Ersetzen Sie dann die Äußerung am Ende durch die nächste Äußerung.

    |Endpunktäußerung|Zugeordnete Absicht|
    |--|--|
    |`I'm looking for a job with Natural Language Processing`|`GetJobInformation`|
    |`I want to cancel on March 3`|`Utilities.Cancel`|
    |`When were HRF-123456 and hrf-234567 published in the last year?`|`FindForm`|
    |`shift 123-45-6789 from Z-1242 to T-54672`|`MoveEmployee`|
    |`Please relocation jill-jones@mycompany.com from x-2345 to g-23456`|`MoveEmployee`|
    |`Here is my c.v. for the programmer job`|`ApplyForJob`|
    |`This is the lead welder paperwork.`|`ApplyForJob`|
    |`does form hrf-123456 cover the new dental benefits and medical plan`|`FindForm`|
    |`Jill Jones work with the media team on the public portal was amazing`|`EmployeeFeedback`|

    Es muss nur ein Pool mit Äußerungen überprüft werden. Dies gilt unabhängig davon, welche Version Sie aktiv bearbeiten oder welche Version der App auf dem Endpunkt veröffentlicht wurde.

## <a name="review-endpoint-utterances"></a>Überprüfen von Endpunktäußerungen

Überprüfen Sie, ob den Endpunktäußerungen die passende Absicht zugeordnet ist. Es ist zwar ein einzelner Pool von Äußerungen vorhanden, der für alle Versionen überprüft wird, durch die korrekte Zuordnung der Absicht wird die Beispielanwendung jedoch nur dem _aktiven Modell_ hinzugefügt.

1. Wählen Sie im Portalabschnitt **Erstellen** im linken Navigationsbereich die Option **Endpunktäußerungen überprüfen** aus. Die Liste wird für die Absicht **ApplyForJob** gefiltert.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Schaltfläche „Endpunktäußerungen überprüfen“ im linken Navigationsbereich](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png)

    Diese Äußerung (`I'm looking for a job with Natural Language Processing`) weist nicht die richtige Absicht auf.

1.  Wählen Sie zum Zuordnen dieser Äußerung in der Zeile der Äußerung die korrekte Option für **Zugeordnete Absicht** aus (in diesem Fall: `GetJobInformation`). Wählen Sie das Häkchen aus, um die geänderte Äußerung der App hinzuzufügen.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Schaltfläche „Endpunktäußerungen überprüfen“ im linken Navigationsbereich](./media/luis-tutorial-review-endpoint-utterances/select-correct-aligned-intent-for-endpoint-utterance.png)

    Überprüfen Sie die restlichen Äußerungen in dieser Absicht, und korrigieren Sie bei Bedarf die zugeordnete Absicht. Verwenden Sie die ursprüngliche Äußerungstabelle in diesem Tutorial, um sich die zugeordnete Absicht anzusehen.

    Die korrigierten Äußerungen sollten in der Liste **Überprüfen von Endpunktäußerungen** nicht mehr enthalten sein. Sollten weitere Äußerungen angezeigt werden, können Sie die Liste weiter durchgehen und zugeordnete Absichten korrigieren, bis die Liste leer ist.

    Die Korrektur von Entitätsbezeichnungen erfolgt nach Abschluss der Absichtszuordnung auf der Seite mit den Absichtsdetails.

1. Trainieren Sie die App und veröffentlichen Sie sie erneut.

## <a name="get-intent-prediction-from-endpoint"></a>Abrufen der Absichtsvorhersage vom Endpunkt

Vergewissern Sie sich mithilfe einer Äußerung, die der korrigierten Äußerung ähnelt, dass sich die Vorhersage der App durch die korrekt zugeordneten Beispieläußerungen verbessert hat.

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Ersetzen Sie in der Adressleiste am Ende der URL _IHRE_ABFRAGE_ durch `Are there any natural language processing jobs in my department right now?`.

   ```json
    {
        "query": "Are there any natural language processing jobs in my department right now?",
        "prediction": {
            "topIntent": "GetJobInformation",
            "intents": {
                "GetJobInformation": {
                    "score": 0.903607249
                },
                "EmployeeFeedback": {
                    "score": 0.0312187821
                },
                "ApplyForJob": {
                    "score": 0.0230276529
                },
                "MoveEmployee": {
                    "score": 0.008322801
                },
                "Utilities.Stop": {
                    "score": 0.004480808
                },
                "FindForm": {
                    "score": 0.00425248267
                },
                "Utilities.StartOver": {
                    "score": 0.004224336
                },
                "Utilities.Help": {
                    "score": 0.00373591436
                },
                "None": {
                    "score": 0.0034621188
                },
                "Utilities.Cancel": {
                    "score": 0.00230977475
                },
                "Utilities.Confirm": {
                    "score": 0.00112078607
                }
            },
            "entities": {
                "keyPhrase": [
                    "natural language processing jobs",
                    "department"
                ],
                "datetimeV2": [
                    {
                        "type": "datetime",
                        "values": [
                            {
                                "timex": "PRESENT_REF",
                                "resolution": [
                                    {
                                        "value": "2019-12-05 23:23:53"
                                    }
                                ]
                            }
                        ]
                    }
                ],
                "$instance": {
                    "keyPhrase": [
                        {
                            "type": "builtin.keyPhrase",
                            "text": "natural language processing jobs",
                            "startIndex": 14,
                            "length": 32,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        },
                        {
                            "type": "builtin.keyPhrase",
                            "text": "department",
                            "startIndex": 53,
                            "length": 10,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "datetimeV2": [
                        {
                            "type": "builtin.datetimeV2.datetime",
                            "text": "right now",
                            "startIndex": 64,
                            "length": 9,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
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

   Nach ordnungsgemäßer Zuordnung der unsicheren Äußerungen wird die korrekte Absicht mit einer **hohen Bewertung** vorhergesagt.

## <a name="can-reviewing-be-replaced-by-adding-more-utterances"></a>Kann die Überprüfung durch das Hinzufügen von weiteren Äußerungen ersetzt werden?
Sie fragen sich vielleicht, warum nicht einfach weitere Beispieläußerungen hinzugefügt werden. Was ist der Zweck der Überprüfung von Endpunktäußerungen? Bei einer LUIS-App in der Praxis stammen die Endpunktäußerungen von Benutzern mit einer Wortwahl und einer Zusammensetzung, die Sie noch nicht verwendet haben. Wenn Sie die gleiche Wortwahl und Zusammensetzung verwendet hätten, würde die ursprüngliche Vorhersage einen höheren Prozentsatz aufweisen.

## <a name="why-is-the-top-intent-on-the-utterance-list"></a>Warum ist die oberste Absicht in der Liste mit den Äußerungen enthalten?
Einige Endpunktäußerungen weisen in der Überprüfungsliste einen hohen Vorhersagewert auf. Sie müssen diese Äußerungen trotzdem überprüfen und verifizieren. Sie sind in der Liste enthalten, weil die nächsthöhere Absicht über eine Bewertung verfügt hat, die zu nah an der Bewertung der obersten Absicht gelegen hat. Es sollte ein Unterschied von ungefähr 15 % zwischen den beiden am höchsten bewerteten Absichten bestehen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie am Endpunkt ausgegebene Äußerungen überprüft, bei denen LUIS unsicher war. Sobald diese Äußerungen überprüft und als Beispieläußerungen unter die richtigen Absichten verschoben wurden, weist LUIS eine verbesserte Vorhersagegenauigkeit auf.

> [!div class="nextstepaction"]
> [Verwenden von Mustern](luis-tutorial-pattern.md)
