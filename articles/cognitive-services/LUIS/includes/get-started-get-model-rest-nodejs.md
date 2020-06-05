---
title: Abrufen eines Modells mit einem REST-Aufruf in C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/03/2020
ms.author: diberry
ms.openlocfilehash: a1a72d9be339ed1ee0a1c525ee426047b1768f2f
ms.sourcegitcommit: 8e5b4e2207daee21a60e6581528401a96bfd3184
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2020
ms.locfileid: "84416381"
---
[Referenzdokumentation](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c45) | [Beispiel](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/LUIS/node-model-with-rest/model.js)

## <a name="prerequisites"></a>Voraussetzungen

* Programmiersprache [Node.js](https://nodejs.org/)
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>JSON-Datei mit Beispieläußerungen

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]

## <a name="create-the-nodejs-project"></a>Erstellen des Node.js-Projekts

1. Erstellen Sie einen neuen Ordner zur Aufnahme Ihres Node.js-Projekts, beispielsweise `node-model-with-rest`.

1. Öffnen Sie eine neue Eingabeaufforderung, navigieren Sie zu dem erstellten Ordner, und führen Sie den folgenden Befehl aus:

    ```console
    npm init
    ```

    Drücken Sie an jeder Eingabeaufforderung die EINGABETASTE, um die Standardeinstellungen zu übernehmen.

1. Installieren Sie das Request-Promise-Modul, indem Sie den folgenden Befehl eingeben:

    ```console
    npm install --save request
    npm install --save request-promise
    npm install --save querystring
    ```

## <a name="change-model-programmatically"></a>Programmgesteuertes Ändern des Modells

1. Erstellen Sie eine neue Datei mit dem Namen `model.js`. Fügen Sie den folgenden Code hinzu:

    [!code-javascript[Code snippet](~/cognitive-services-quickstart-code/javascript/LUIS/node-model-with-rest/model.js)]

1. Ersetzen Sie die Werte, die mit `YOUR-` beginnen, durch Ihre eigenen Werte.

    |Information|Zweck|
    |--|--|
    |`YOUR-APP-ID`| Ihre LUIS-App-ID. |
    |`YOUR-AUTHORING-KEY`|Ihr Erstellungsschlüssel mit 32 Zeichen.|
    |`YOUR-AUTHORING-ENDPOINT`| Ihr URL-Endpunkt für die Erstellung. Beispiel: `https://replace-with-your-resource-name.api.cognitive.microsoft.com/`. Sie haben Ihren Ressourcennamen festgelegt, als Sie die Ressource erstellt haben.|

    Zugewiesene Schlüssel und Ressourcen werden im LUIS-Portal auf der Seite **Azure-Ressourcen** im Abschnitt „Verwalten“ angezeigt. Die App-ID wird auf der Seite **Anwendungseinstellungen** ebenfalls im Abschnitt „Verwalten“ angezeigt.

1. Geben Sie an der Eingabeaufforderung den folgenden Befehl ein, um das Projekt auszuführen:

    ```console
    node model.js
    ```

1. Überprüfen Sie die Erstellungsantwort:

    ```json
    addUtterance:
    [
      {
        "value": {
          "ExampleId": 1137150691,
          "UtteranceText": "order a pizza"
        },
        "hasError": false
      },
      {
        "value": {
          "ExampleId": 1137150692,
          "UtteranceText": "order a large pepperoni pizza"
        },
        "hasError": false
      },
      {
        "value": {
          "ExampleId": 1137150693,
          "UtteranceText": "i want two large pepperoni pizzas on thin crust"
        },
        "hasError": false
      }
    ]
    train POST:
    {
      "statusId": 9,
      "status": "Queued"
    }
    train GET:
    [
      {
        "modelId": "edb46abf-0000-41ab-beb2-a41a0fe1630f",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "a5030be2-616c-4648-bf2f-380fa9417d37",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "3f2b1f31-a3c3-4fbd-8182-e9d9dbc120b9",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "e4b6704b-1636-474c-9459-fe9ccbeba51c",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "031d3777-2a00-4a7a-9323-9a3280a30000",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "9250e7a1-06eb-4413-9432-ae132ed32583",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      }
    ]
    done
    ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie den Projektordner aus dem Dateisystem, nachdem Sie diese Schnellstartanleitung durchgearbeitet haben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bewährte Methoden für eine App](../luis-concept-best-practices.md)