---
title: 'Schnellstart: Senden einer Abfrage an die API mit Node.js – Bing-Suche für ortsansässige Unternehmen'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diesen Schnellstart, um Anforderungen an die API für die Bing-Suche nach ortsansässigen Unternehmen zu senden, die zum Leistungsumfang von Cognitive Services gehört.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: quickstart
ms.date: 05/12/2020
ms.author: aahi
ms.openlocfilehash: 2b5de1c5bf99ff2516a0eb836d540fc2833d99cb
ms.sourcegitcommit: 32592ba24c93aa9249f9bd1193ff157235f66d7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2020
ms.locfileid: "85611253"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-using-nodejs"></a>Schnellstart: Senden einer Abfrage an die API für die Bing-Suche nach ortsansässigen Unternehmen mit Node.js

Verwenden Sie diesen Schnellstart, um zu lernen. wie Sie Anforderungen an die API für die Bing-Suche für ortsansässige Unternehmen senden, die zum Leistungsumfang von Azure Cognitive Services gehört. Diese einfache Anwendung ist zwar in Node.js geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit jeder Programmiersprache kompatibel ist, die HTTP-Anforderungen stellen und JSON analysieren kann.

Diese Beispielanwendung ruft lokale Antwortdaten aus der API für eine Suchabfrage ab.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: [Kostenloses Azure-Konto](https://azure.microsoft.com/free/cognitive-services/)
* Die aktuelle Version von [Node.js](https://nodejs.org/en/download/)
* Die [JavaScript-Bibliothek für Anforderungen](https://github.com/request/request).
* Sobald Sie über Ihr Azure-Abonnement verfügen, sollten Sie über <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7"  title="Erstellen einer Ressource für die Bing-Suche"  target="_blank"> im Azure-Portal eine Ressource für die Bing-Suche <span class="docon docon-navigate-external x-hidden-focus"></span></a> erstellen, um Ihren Schlüssel und Endpunkt abzurufen. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.


## <a name="code-scenario"></a>Codeszenario

Der folgende Code definiert und sendet die Anforderung, die in den folgenden Schritten implementiert wird:

1. Deklarieren Sie Variablen zum Angeben des Endpunkts nach Host und Pfad.
2. Geben Sie die Abfrage an, und fügen Sie den Abfrageparameter hinzu.
3. Erstellen Sie eine Handlerfunktion für die Antwort.
4. Definieren Sie die Suchfunktion, die die Anforderung erstellt und den Header `Ocp-Apim-Subscription-Key` hinzufügt.
5. Führen Sie die Suchfunktion aus.


```javascript
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'your-access-key';

let host = 'api.cognitive.microsoft.com/bing';
let path = '/v7.0/localbusinesses/search';

let mkt = 'en-US';
let q = 'hotel in Bellevue';

let params = '?q=' + encodeURI(q) + "&mkt=" + mkt;

let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        let body_ = JSON.parse(body);
        let body__ = JSON.stringify(body_, null, '  ');
        console.log(body__);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};

let Search = function () {
    let request_params = {
        method: 'GET',
        hostname: host,
        path: path + params,
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey,
        }
    };

    let req = https.request(request_params, response_handler);
    req.end();
}

Search();

```

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Suche nach ortsansässigen Unternehmen mit C#](local-quickstart.md)
* [Schnellstart: Suche nach ortsansässigen Unternehmen mit Java](local-search-java-quickstart.md)
* [Schnellstart: Suche nach ortsansässigen Unternehmen mit Python](local-search-python-quickstart.md)
