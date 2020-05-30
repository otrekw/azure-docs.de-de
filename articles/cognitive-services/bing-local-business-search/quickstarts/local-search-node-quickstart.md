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
ms.openlocfilehash: 3bb31c36e8c614a72b86f95cb7e7d1c588692f97
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873083"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-using-nodejs"></a>Schnellstart: Senden einer Abfrage an die API für die Bing-Suche nach ortsansässigen Unternehmen mit Node.js

Verwenden Sie diesen Schnellstart, um zu lernen. wie Sie Anforderungen an die API für die Bing-Suche für ortsansässige Unternehmen senden, die zum Leistungsumfang von Azure Cognitive Services gehört. Diese einfache Anwendung ist zwar in Node.js geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit jeder Programmiersprache kompatibel ist, die HTTP-Anforderungen stellen und JSON analysieren kann.

Diese Beispielanwendung ruft lokale Antwortdaten aus der API für eine Suchabfrage ab.

## <a name="prerequisites"></a>Voraussetzungen

* Die aktuelle Version von [Node.js](https://nodejs.org/en/download/)
* Die [JavaScript-Bibliothek für Anforderungen](https://github.com/request/request).
* Ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit Bing-Suche-APIs. Für diesen Schnellstart ist die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ausreichend. Speichern Sie den API-Schlüssel, den Sie bei der Aktivierung Ihrer kostenlosen Testversion erhalten. Weitere Informationen finden Sie unter [Cognitive Services-Preise: Bing-Suche-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).


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
