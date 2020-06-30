---
title: 'Schnellstart: Analysieren eines Remotebilds mit der REST-API und Node.js'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung analysieren Sie ein Bild mit der Maschinelles Sehen-API und Node.js.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.custom: seodec18, seo-javascript-september2018, seo-javascript-october2019
ms.openlocfilehash: 2ed27f92617992e50160d96f25132b1ff9625acc
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84986758"
---
# <a name="quickstart-analyze-a-remote-image-using-the-computer-vision-rest-api-with-nodejs"></a>Schnellstart: Analysieren eines Remotebilds mit der Maschinelles Sehen-REST-API und Node.js

In dieser Schnellstartanleitung verwenden Sie die Maschinelles Sehen-REST-API mit Node.js, um ein remote gespeichertes Bild zu analysieren und visuelle Merkmale zu extrahieren. Mit der Methode [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) können Sie basierend auf dem Inhalt des Bilds visuelle Merkmale extrahieren.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: [Kostenloses Azure-Konto](https://azure.microsoft.com/free/cognitive-services/)
* [Node.js](https://nodejs.org) 4.x oder höher 
* [npm](https://www.npmjs.com/) 
* Sobald Sie über Ihr Azure-Abonnement verfügen, sollten Sie über <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title="Erstellen einer Ressource für maschinelles Sehen"  target="_blank"> im Azure-Portal eine Ressource für maschinelles Sehen <span class="docon docon-navigate-external x-hidden-focus"></span></a> erstellen, um Ihren Schlüssel und Endpunkt abzurufen. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um eine Verbindung Ihrer Anwendung mit dem Dienst für maschinelles Sehen herzustellen. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.
* [Erstellen Sie Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für den Schlüssel und die Endpunkt-URL, die Sie `COMPUTER_VISION_SUBSCRIPTION_KEY` bzw. `COMPUTER_VISION_ENDPOINT` nennen.

## <a name="create-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Führen Sie zum Erstellen und Ausführen des Beispiels die folgenden Schritte aus:

1. Installieren Sie das npm-Paket [`request`](https://www.npmjs.com/package/request).
   1. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator.
   1. Führen Sie den folgenden Befehl aus:

      ```console
      npm install request
      ```

   1. Wenn das Paket erfolgreich installiert wurde, schließen Sie das Eingabeaufforderungsfenster.

1. Kopieren Sie den folgenden Code in einen Text-Editor.
1. Ersetzen Sie optional den Wert von `imageUrl` durch die URL eines anderen Bilds, das analysiert werden soll.
1. Ersetzen Sie optional den Wert des Anforderungsparameters `language` durch eine andere Sprache.
1. Speichern Sie den Code als Datei mit der Erweiterung `.js`. Beispiel: `analyze-image.js`.
1. Öffnen Sie ein Eingabeaufforderungsfenster.
1. Verwenden Sie an der Eingabeaufforderung den Befehl `node`, um die Datei auszuführen. Beispiel: `node analyze-image.js`.

```javascript
'use strict';

const request = require('request');

let subscriptionKey = process.env['COMPUTER_VISION_SUBSCRIPTION_KEY'];
let endpoint = process.env['COMPUTER_VISION_ENDPOINT']
if (!subscriptionKey) { throw new Error('Set your environment variables for your subscription key and endpoint.'); }

var uriBase = endpoint + 'vision/v3.0/analyze';

const imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg';

// Request parameters.
const params = {
    'visualFeatures': 'Categories,Description,Color',
    'details': '',
    'language': 'en'
};

const options = {
    uri: uriBase,
    qs: params,
    body: '{"url": ' + '"' + imageUrl + '"}',
    headers: {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key' : subscriptionKey
    }
};

request.post(options, (error, response, body) => {
  if (error) {
    console.log('Error: ', error);
    return;
  }
  let jsonResponse = JSON.stringify(JSON.parse(body), null, '  ');
  console.log('JSON Response\n');
  console.log(jsonResponse);
});
```

## <a name="examine-the-response"></a>Untersuchen der Antwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Das Beispiel analysiert eine Antwort und zeigt diese bei erfolgreicher Ausführung im Eingabeaufforderungsfenster an. Im Folgenden finden Sie ein Beispiel dafür:

```json
{
  "categories": [
    {
      "name": "outdoor_water",
      "score": 0.9921875,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "nature",
      "water",
      "waterfall",
      "outdoor",
      "rock",
      "mountain",
      "rocky",
      "grass",
      "hill",
      "covered",
      "hillside",
      "standing",
      "side",
      "group",
      "walking",
      "white",
      "man",
      "large",
      "snow",
      "grazing",
      "forest",
      "slope",
      "herd",
      "river",
      "giraffe",
      "field"
    ],
    "captions": [
      {
        "text": "a large waterfall over a rocky cliff",
        "confidence": 0.916458423253597
      }
    ]
  },
  "color": {
    "dominantColorForeground": "Grey",
    "dominantColorBackground": "Green",
    "dominantColors": [
      "Grey",
      "Green"
    ],
    "accentColor": "4D5E2F",
    "isBwImg": false
  },
  "requestId": "81b4e400-e3c1-41f1-9020-e6871ad9f0ed",
  "metadata": {
    "height": 959,
    "width": 1280,
    "format": "Jpeg"
  }
}
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Datei nicht mehr benötigen, löschen Sie sie, und deinstallieren Sie dann das npm-Paket `request`. Führen Sie die folgenden Schritte durch, um das Paket zu deinstallieren:

1. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator.
2. Führen Sie den folgenden Befehl aus:

   ```console
   npm uninstall request
   ```

3. Wenn das Paket erfolgreich deinstalliert wurde, schließen Sie das Eingabeaufforderungsfenster.

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie als Nächstes die Maschinelles Sehen-APIs, die zum Analysieren von Bildern, Erkennen von Prominenten und Sehenswürdigkeiten, Erstellen von Miniaturansichten und Extrahieren von gedrucktem sowie handschriftlichem Text verwendet werden.

> [!div class="nextstepaction"]
> [Erkunden der Maschinelles Sehen-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
