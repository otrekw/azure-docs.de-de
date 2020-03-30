---
title: Abrufen der Absicht mit einem REST-Aufruf in Python
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/31/2020
ms.author: diberry
ms.openlocfilehash: 23ac98f91c989c9bedb6b91e6a7ce26dc164ac5a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76987770"
---
## <a name="prerequisites"></a>Voraussetzungen

* [Python 3.6](https://www.python.org/downloads/) oder höher
* [Visual Studio Code](https://code.visualstudio.com/)
* ID der öffentlichen App: `df67dcdb-c37d-46af-88e1-8b97951ca1c2`

## <a name="create-luis-runtime-key-for-predictions"></a>Erstellen eines LUIS-Runtimeschlüssels für Vorhersagen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie auf [**Language Understanding** erstellen](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne).
1. Geben Sie alle erforderlichen Einstellungen für den **Runtimeschlüssel** ein:

    |Einstellung|value|
    |--|--|
    |Name|Gewünschter Name (2 bis 64 Zeichen)|
    |Subscription|Wählen Sie ein passendes Abonnement aus|
    |Location|Wählen Sie einen nahe gelegenen und verfügbaren Speicherort aus|
    |Preisstufe|`F0` – der Mindesttarif|
    |Ressourcengruppe|Wählen Sie eine verfügbare Ressourcengruppe aus|

1. Klicken Sie auf **Erstellen**, und warten Sie, bis die Ressource erstellt wird. Nachdem sie erstellt wurde, navigieren Sie zur Ressourcenseite.
1. Erfassen Sie den konfigurierten Endpunkt (`endpoint`) und einen Schlüssel (`key`).

## <a name="get-intent-from-the-prediction-endpoint"></a>Abrufen der Absicht vom Vorhersageendpunkt

Verwenden Sie Python, um den [Vorhersageendpunkt](https://aka.ms/luis-apim-v3-prediction) abzurufen und ein Vorhersageergebnis zu erhalten.

1. Kopieren Sie diesen Codeausschnitt in eine Datei namens `predict.py`:

    ```python
    ########### Python 3.6 #############
    import requests

    try:

        key = 'YOUR-KEY' # your Runtime key
        endpoint = 'YOUR-ENDPOINT' # such as 'your-resource-name.api.cognitive.microsoft.com'
        appId = 'df67dcdb-c37d-46af-88e1-8b97951ca1c2'
        utterance = 'turn on all lights'

        headers = {
        }

        params ={
            'query': utterance,
            'timezoneOffset': '0',
            'verbose': 'true',
            'show-all-intents': 'true',
            'spellCheck': 'false',
            'staging': 'false',
            'subscription-key': key
        }

        r = requests.get(f'https://{endpoint}/luis/prediction/v3.0/apps/{appId}/slots/production/predict',headers=headers, params=params)
        print(r.json())

    except Exception as e:
        print(f'{e}')
    ```

1. Ersetzen Sie die Werte `YOUR-KEY` und `YOUR-ENDPOINT` durch Ihren eigenen Schlüssel- bzw. Endpunktwert für die **Vorhersageruntime**.

    |Information|Zweck|
    |--|--|
    |`YOUR-KEY`|Ihr 32-stelliger Schlüssel für die **Vorhersageruntime**.|
    |`YOUR-ENDPOINT`| Ihr URL-Endpunkt für die Vorhersage. Beispiel: `replace-with-your-resource-name.api.cognitive.microsoft.com`.|

1. Installieren Sie die Abhängigkeit `requests`. Diese wird für die Ausführung von HTTP-Anforderungen verwendet:

    ```console
    pip install requests
    ```

1. Führen Sie das Skript mit diesem Konsolenbefehl aus:

    ```console
    python predict.py
    ```

1. Überprüfen Sie die Vorhersageantwort, die im JSON-Format zurückgegeben wird:

    ```console
    {'query': 'turn on all lights', 'prediction': {'topIntent': 'HomeAutomation.TurnOn', 'intents': {'HomeAutomation.TurnOn': {'score': 0.5375382}, 'None': {'score': 0.08687421}, 'HomeAutomation.TurnOff': {'score': 0.0207554}}, 'entities': {'HomeAutomation.Operation': ['on'], '$instance': {'HomeAutomation.Operation': [{'type': 'HomeAutomation.Operation', 'text': 'on', 'startIndex': 5, 'length': 2, 'score': 0.724984169, 'modelTypeId': -1, 'modelType': 'Unknown', 'recognitionSources': ['model']}]}}}}
    ```

    Dies ist die zur besseren Lesbarkeit formatierte JSON-Antwort:

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "None": {
                    "score": 0.08687421
                },
                "HomeAutomation.TurnOff": {
                    "score": 0.0207554
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ],
                "$instance": {
                    "HomeAutomation.Operation": [
                        {
                            "type": "HomeAutomation.Operation",
                            "text": "on",
                            "startIndex": 5,
                            "length": 2,
                            "score": 0.724984169,
                            "modelTypeId": -1,
                            "modelType": "Unknown",
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

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Datei aus dem Dateisystem, nachdem Sie diese Schnellstartanleitung durchgearbeitet haben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen und Trainieren](../get-started-get-model-rest-apis.md)