---
title: 'Schnellstart: Generieren einer Miniaturansicht – REST, Node.js'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart generieren Sie eine Miniaturansicht von einem Bild, indem Sie die Maschinelles Sehen-API mit Node.js verwenden.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 5c3a6ba009771a879694fe869862f2c3e5836114
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83685081"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-nodejs"></a>Schnellstart: Generieren einer Miniaturansicht mit der Maschinelles Sehen-REST-API und Node.js

In dieser Schnellstartanleitung verwenden Sie die Maschinelles Sehen-REST-API, um eine Miniaturansicht von einem Bild zu generieren. Mit der [Get Thumbnail](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb)-Methode können Sie eine Miniaturansicht von einem Bild generieren. Sie geben die Höhe und Breite an. Diese kann sich vom Seitenverhältnis des Eingabebilds unterscheiden. Für das maschinelle Sehen wird die intelligente Zuschneidefunktion verwendet, um den gewünschten Bereich zu identifizieren und basierend auf dieser Region Zuschneidekoordinaten zu generieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- [Node.js](https://nodejs.org) 4.x oder höher muss installiert sein.
- [npm](https://www.npmjs.com/) muss installiert sein.
- Sie benötigen einen Abonnementschlüssel für maschinelles Sehen. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) können Sie einen Schlüssel für eine kostenlose Testversion abrufen. Alternativ gehen Sie wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um „Maschinelles Sehen“ zu abonnieren und Ihren Schlüssel zu erhalten. [Erstellen Sie dann Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für die Schlüssel- und Dienstendpunkt-Zeichenfolge, und nennen Sie sie `COMPUTER_VISION_SUBSCRIPTION_KEY` bzw. `COMPUTER_VISION_ENDPOINT`.

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
1. Speichern Sie den Code als Datei mit der Erweiterung `.js`. Beispiel: `get-thumbnail.js`.
1. Öffnen Sie ein Eingabeaufforderungsfenster.
1. Verwenden Sie an der Eingabeaufforderung den Befehl `node`, um die Datei auszuführen. Beispiel: `node get-thumbnail.js`.

```javascript
'use strict';

const fs = require('fs');
const request = require('request').defaults({ encoding: null });

let subscriptionKey = process.env['COMPUTER_VISION_SUBSCRIPTION_KEY'];
let endpoint = process.env['COMPUTER_VISION_ENDPOINT']

var uriBase = endpoint + 'vision/v3.0/generateThumbnail';

const imageUrl = 'https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg';

// Request parameters.
const params = {
    'width': '100',
    'height': '100',
    'smartCropping': 'true'
};

// Construct the request
const options = {
    uri: uriBase,
    qs: params,
    body: '{"url": ' + '"' + imageUrl + '"}',
    headers: {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key' : subscriptionKey
    }
}

// Post the request and get the response (an image stream)
request.post(options, (error, response, body) => {
    // Write the stream to file
    var buf = Buffer.from(body, 'base64');
    fs.writeFile('thumbnail.png', buf, function (err) {
        if (err) throw err;
    });

    console.log('Image saved')
});
```

## <a name="examine-the-response"></a>Untersuchen der Antwort

Das Miniaturbild wird in einem Popupfenster angezeigt.
Eine erfolgreiche Antwort wird als Binärdaten zurückgegeben, die die Bilddaten für die Miniaturansicht darstellen. Wenn bei der Anforderung ein Fehler auftritt, wird die Antwort im Konsolenfenster angezeigt. Die Antwort für die fehlerhafte Anforderung enthält einen Fehlercode und eine Meldung, damit festgestellt werden kann, was nicht funktioniert hat.

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie als Nächstes die Maschinelles Sehen-API, die zum Analysieren von Bildern, Erkennen von Prominenten und Sehenswürdigkeiten, Erstellen von Miniaturansichten und Extrahieren von gedrucktem sowie handschriftlichem Text verwendet wird.

> [!div class="nextstepaction"]
> [Erkunden der Maschinelles Sehen-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
