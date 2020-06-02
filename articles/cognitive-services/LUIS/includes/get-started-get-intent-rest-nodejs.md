---
title: Abrufen der Absicht mit einem REST-Aufruf in Node.js
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/18/2020
ms.author: diberry
ms.openlocfilehash: f60b4391f5b68f163eb2e97153667d82454639d5
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83654337"
---
## <a name="prerequisites"></a>Voraussetzungen

* Programmiersprache [Node.js](https://nodejs.org/)
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="create-pizza-app"></a>Erstellen Sie die Pizza-App

[!INCLUDE [Create pizza app](get-started-get-intent-create-pizza-app.md)]

## <a name="create-the-nodejs-project"></a>Erstellen Sie das Node.js-Projekt

1. Erstellen Sie einen neuen Ordner zur Aufnahme Ihres Node.js-Projekts, beispielsweise `node-predict-with-rest`.

1. Öffnen Sie eine neue Eingabeaufforderung, navigieren Sie zu dem erstellten Ordner, und führen Sie den folgenden Befehl aus:

    ```console
    npm init
    ```

    Drücken Sie an jeder Eingabeaufforderung die EINGABETASTE, um die Standardeinstellungen zu übernehmen.

1. Installieren Sie die Abhängigkeiten, indem Sie die folgenden Befehle eingeben:

    ```console
    npm install --save request
    npm install --save request-promise
    npm install --save querystring
    ```

## <a name="get-intent-programmatically"></a>Programmgesteuertes Abrufen der Absicht

Verwenden Sie Node.js, um den [Vorhersageendpunkt](https://aka.ms/luis-apim-v3-prediction) abzurufen und ein Vorhersageergebnis zu erhalten.

1. Kopieren Sie den folgenden Codeausschnitt in eine Datei namens `predict.js`:

    ```javascript
    var requestPromise = require('request-promise');
    var queryString = require('querystring');

    // Analyze a string utterance.
    getPrediction = async () => {

        //////////
        // Values to modify.

        // YOUR-APP-ID: The App ID GUID found on the www.luis.ai Application Settings page.
        const LUIS_appId = "YOUR-APP-ID";

        // YOUR-PREDICTION-KEY: Your LUIS authoring key, 32 character value.
        const LUIS_predictionKey = "YOUR-PREDICTION-KEY";

        // YOUR-PREDICTION-ENDPOINT: Replace this with your authoring key endpoint.
        // For example, "https://westus.api.cognitive.microsoft.com/"
        const LUIS_endpoint = "YOUR-PREDICTION-ENDPOINT";

        // The utterance you want to use.
        const utterance = "I want a deep dish supreme pizza with extra cheese, hold the onions.";
        //////////

        // Create query string
        const queryParams = {
            "show-all-intents": true,
            "verbose":  true,
            "query": utterance,
            "subscription-key": LUIS_predictionKey
        }

        // Create the URI for the REST call.
        const URI = `${LUIS_endpoint}luis/prediction/v3.0/apps/${LUIS_appId}/slots/production/predict?${queryString.stringify(queryParams)}`

        // Send the REST call.
        const response = await requestPromise(URI);

        // Display the response from the REST call.
        console.log(response);
    }

    // Pass an utterance to the sample LUIS app
    getPrediction().then(()=>console.log("done")).catch((err)=>console.log(err));
    ```

1. Ersetzen Sie die Werte `YOUR-KEY` und `YOUR-ENDPOINT` durch Ihren eigenen Schlüssel- bzw. Endpunktwert für die **Vorhersageruntime**.

    |Information|Zweck|
    |--|--|
    |`YOUR-KEY`|Ihr 32-stelliger Schlüssel für die **Vorhersageruntime**.|
    |`YOUR-ENDPOINT`| Ihr URL-Endpunkt für die Vorhersage. Beispiel: `replace-with-your-resource-name.api.cognitive.microsoft.com`.|

 1. Überprüfen Sie die Vorhersageantwort, die im JSON-Format zurückgegeben wird:

    ```json
    {"query":"I want two large pepperoni pizzas on thin crust please","prediction":{"topIntent":"ModifyOrder","intents":{"ModifyOrder":{"score":1.0},"None":{"score":8.55E-09},"Greetings":{"score":1.82222226E-09},"CancelOrder":{"score":1.47272727E-09},"Confirmation":{"score":9.8125E-10}},"entities":{"Order":[{"FullPizzaWithModifiers":[{"PizzaType":["pepperoni pizzas"],"Size":[["Large"]],"Quantity":[2],"Crust":[["Thin"]],"$instance":{"PizzaType":[{"type":"PizzaType","text":"pepperoni pizzas","startIndex":17,"length":16,"score":0.9978157,"modelTypeId":1,"modelType":"Entity Extractor","recognitionSources":["model"]}],"Size":[{"type":"SizeList","text":"large","startIndex":11,"length":5,"score":0.9984481,"modelTypeId":1,"modelType":"Entity Extractor","recognitionSources":["model"]}],"Quantity":[{"type":"builtin.number","text":"two","startIndex":7,"length":3,"score":0.999770939,"modelTypeId":1,"modelType":"Entity Extractor","recognitionSources":["model"]}],"Crust":[{"type":"CrustList","text":"thin crust","startIndex":37,"length":10,"score":0.933985531,"modelTypeId":1,"modelType":"Entity Extractor","recognitionSources":["model"]}]}}],"$instance":{"FullPizzaWithModifiers":[{"type":"FullPizzaWithModifiers","text":"two large pepperoni pizzas on thin crust","startIndex":7,"length":40,"score":0.90681237,"modelTypeId":1,"modelType":"Entity Extractor","recognitionSources":["model"]}]}}],"ToppingList":[["Pepperoni"]],"$instance":{"Order":[{"type":"Order","text":"two large pepperoni pizzas on thin crust","startIndex":7,"length":40,"score":0.9047088,"modelTypeId":1,"modelType":"Entity Extractor","recognitionSources":["model"]}],"ToppingList":[{"type":"ToppingList","text":"pepperoni","startIndex":17,"length":9,"modelTypeId":5,"modelType":"List Entity Extractor","recognitionSources":["model"]}]}}}}
        ```

    The JSON response formatted for readability:

    ```JSON
    {
      "query": "I want two large pepperoni pizzas on thin crust please",
      "prediction": {
        "topIntent": "ModifyOrder",
        "intents": {
          "ModifyOrder": {
            "score": 1
          },
          "None": {
            "score": 8.55e-9
          },
          "Greetings": {
            "score": 1.82222226e-9
          },
          "CancelOrder": {
            "score": 1.47272727e-9
          },
          "Confirmation": {
            "score": 9.8125e-10
          }
        },
        "entities": {
          "Order": [
            {
              "FullPizzaWithModifiers": [
                {
                  "PizzaType": [
                    "pepperoni pizzas"
                  ],
                  "Size": [
                    [
                      "Large"
                    ]
                  ],
                  "Quantity": [
                    2
                  ],
                  "Crust": [
                    [
                      "Thin"
                    ]
                  ],
                  "$instance": {
                    "PizzaType": [
                      {
                        "type": "PizzaType",
                        "text": "pepperoni pizzas",
                        "startIndex": 17,
                        "length": 16,
                        "score": 0.9978157,
                        "modelTypeId": 1,
                        "modelType": "Entity Extractor",
                        "recognitionSources": [
                          "model"
                        ]
                      }
                    ],
                    "Size": [
                      {
                        "type": "SizeList",
                        "text": "large",
                        "startIndex": 11,
                        "length": 5,
                        "score": 0.9984481,
                        "modelTypeId": 1,
                        "modelType": "Entity Extractor",
                        "recognitionSources": [
                          "model"
                        ]
                      }
                    ],
                    "Quantity": [
                      {
                        "type": "builtin.number",
                        "text": "two",
                        "startIndex": 7,
                        "length": 3,
                        "score": 0.999770939,
                        "modelTypeId": 1,
                        "modelType": "Entity Extractor",
                        "recognitionSources": [
                          "model"
                        ]
                      }
                    ],
                    "Crust": [
                      {
                        "type": "CrustList",
                        "text": "thin crust",
                        "startIndex": 37,
                        "length": 10,
                        "score": 0.933985531,
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
                "FullPizzaWithModifiers": [
                  {
                    "type": "FullPizzaWithModifiers",
                    "text": "two large pepperoni pizzas on thin crust",
                    "startIndex": 7,
                    "length": 40,
                    "score": 0.90681237,
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
          "ToppingList": [
            [
              "Pepperoni"
            ]
          ],
          "$instance": {
            "Order": [
              {
                "type": "Order",
                "text": "two large pepperoni pizzas on thin crust",
                "startIndex": 7,
                "length": 40,
                "score": 0.9047088,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                  "model"
                ]
              }
            ],
            "ToppingList": [
              {
                "type": "ToppingList",
                "text": "pepperoni",
                "startIndex": 17,
                "length": 9,
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

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie den Projektordner aus dem Dateisystem, nachdem Sie diese Schnellstartanleitung durchgearbeitet haben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen und Trainieren](../get-started-get-model-rest-apis.md)