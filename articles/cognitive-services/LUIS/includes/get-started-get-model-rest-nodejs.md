---
title: Abrufen eines Modells mit einem REST-Aufruf in C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/31/2020
ms.author: diberry
ms.openlocfilehash: bbb2ae0b10af795d71f0a78c045bec0c216ee378
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2020
ms.locfileid: "77368417"
---
## <a name="prerequisites"></a>Voraussetzungen

* Azure Language Understanding: Ressourcenschlüssel mit 32 Zeichen und Endpunkt-URL für die Erstellung. Führen Sie die Erstellung mit dem [Azure-Portal](../luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) oder der [Azure CLI](../luis-how-to-azure-subscription.md#create-resources-in-azure-cli) durch.
* Importieren Sie die App [TravelAgent](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/change-model/TravelAgent.json) aus dem GitHub-Repository „cognitive-services-language-understanding“.
* Die LUIS-Anwendungs-ID für die importierte TravelAgent-App. Die Anwendungs-ID wird auf dem Anwendungsdashboard angezeigt.
* Die Versions-ID der Anwendung, die die Äußerungen empfängt. Die Standard-ID lautet „0.1“.
* Programmiersprache [Node.js](https://nodejs.org/)
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>JSON-Datei mit Beispieläußerungen

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]


## <a name="change-model-programmatically"></a>Programmgesteuertes Ändern des Modells

1. Erstellen Sie eine neue Datei mit dem Namen `model.js`. Fügen Sie den folgenden Code hinzu:

    ```javascript
    var request = require('request');
    var requestpromise = require('request-promise');

    // 32 character key value
    const LUIS_authoringKey = "YOUR-KEY";

    // endpoint example: your-resource-name.api.cognitive.microsoft.com
    const LUIS_endpoint = "YOUR-ENDPOINT";
    const LUIS_appId = "YOUR-APP-ID";
    const LUIS_versionId = "0.1";
    const addUtterancesURI = `https://${LUIS_endpoint}/luis/authoring/v3.0-preview/apps/${LUIS_appId}/versions/${LUIS_versionId}/examples`;
    const addTrainURI = `https://${LUIS_endpoint}/luis/authoring/v3.0-preview/apps/${LUIS_appId}/versions/${LUIS_versionId}/train`;

    const utterances = [
            {
              'text': 'go to Seattle today',
              'intentName': 'BookFlight',
              'entityLabels': [
                {
                  'entityName': 'Location::LocationTo',
                  'startCharIndex': 6,
                  'endCharIndex': 12
                }
              ]
            },
            {
                'text': 'a barking dog is annoying',
                'intentName': 'None',
                'entityLabels': []
            }
          ];

    const main = async() =>{


        await addUtterance();
        await train("POST");
        await trainStatus("GET");

    }
    const addUtterance = async () => {

        const options = {
            uri: addUtterancesURI,
            method: 'POST',
            headers: {
                'Ocp-Apim-Subscription-Key': LUIS_authoringKey
            },
            json: true,
            body: utterances
        };

        const response = await requestpromise(options)
        console.log(response.body);
    }
    const train = async (verb) => {

        const options = {
            uri: addTrainURI,
            method: verb,
            headers: {
                'Ocp-Apim-Subscription-Key': LUIS_authoringKey
            },
            json: true,
            body: null // The body can be empty for a training request
        };

        const response = await requestpromise(options)
        console.log(response.body);
    }

    // MAIN
    main().then(() => console.log("done")).catch((err)=> console.log(err returned));
    ```

1. Ersetzen Sie die Werte, die mit `YOUR-` beginnen, durch Ihre eigenen Werte.

    |Information|Zweck|
    |--|--|
    |`YOUR-KEY`|Ihr Erstellungsschlüssel mit 32 Zeichen.|
    |`YOUR-ENDPOINT`| Ihr URL-Endpunkt für die Erstellung. Beispiel: `replace-with-your-resource-name.api.cognitive.microsoft.com`. Sie haben Ihren Ressourcennamen festgelegt, als Sie die Ressource erstellt haben.|
    |`YOUR-APP-ID`| Ihre LUIS-App-ID. |

    Zugewiesene Schlüssel und Ressourcen werden im LUIS-Portal auf der Seite **Azure-Ressourcen** im Abschnitt „Verwalten“ angezeigt. Die App-ID wird auf der Seite **Anwendungseinstellungen** ebenfalls im Abschnitt „Verwalten“ angezeigt.

1. Geben Sie in dem Verzeichnis, in dem Sie die Datei erstellt haben, an einer Eingabeaufforderung den folgenden Befehl ein, um die Datei auszuführen:

    ```console
    node model.js
    ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Datei aus dem Dateisystem, nachdem Sie diese Schnellstartanleitung durchgearbeitet haben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bewährte Methoden für eine App](../luis-concept-best-practices.md)