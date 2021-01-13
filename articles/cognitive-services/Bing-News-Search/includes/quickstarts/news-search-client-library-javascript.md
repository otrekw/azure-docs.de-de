---
title: 'Schnellstart: JavaScript-Clientbibliothek für die Bing-News-Suche'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/12/2020
ms.author: aahi
ms.custom: devx-track-js
ms.openlocfilehash: 3760213c7f469dfe89599c0f01afe98168efde2d
ms.sourcegitcommit: 5dbea4631b46d9dde345f14a9b601d980df84897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376944"
---
Verwenden Sie diese Schnellstartanleitung, um unter Verwendung der Bing-News-Suche-Clientbibliothek für JavaScript mit der Suche nach Nachrichten zu beginnen. Die Bing-News-Suche verfügt zwar über eine REST-API, die mit den meisten Programmiersprachen kompatibel ist, die Clientbibliothek ist jedoch eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js).

## <a name="prerequisites"></a>Voraussetzungen

* Die aktuelle Version von [Node.js](https://nodejs.org/en/download/)
* Das [Bing News-Suche-SDK für JavaScript](https://www.npmjs.com/package/@azure/cognitiveservices-newssearch)
     *  Führen Sie zum Installieren `npm install @azure/cognitiveservices-newssearch` aus.
* Die `CognitiveServicesCredentials`-Klasse aus dem Paket `@azure/ms-rest-azure-js` zum Authentifizieren des Clients.
     * Führen Sie zum Installieren `npm install @azure/ms-rest-azure-js` aus.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Erstellen und Initialisieren der Anwendung

1. Erstellen Sie eine Instanz der `CognitiveServicesCredentials`. Erstellen Sie Variablen für Ihren Abonnementschlüssel und einen Suchbegriff.

    ```javascript
    const CognitiveServicesCredentials = require('@azure/ms-rest-azure-js').CognitiveServicesCredentials;
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let search_term = 'Winter Olympics'
    ```

2. Instanziieren Sie den Client:
    
    ```javascript
    const NewsSearchAPIClient = require('@azure/cognitiveservices-newssearch');
    let client = new NewsSearchAPIClient(credentials);
    ```

## <a name="send-a-search-query"></a>Senden einer Suchabfrage

1. Verwenden Sie den Client zur Suche mit einem Abfrageausdruck, in diesem Fall „Winter Olympics“:
    
    ```javascript
    client.newsOperations.search(search_term).then((result) => {
        console.log(result.value);
    }).catch((err) => {
        throw err;
    });
    ```

Der Code gibt `result.value` Elemente an der Konsole aus, ohne Text zu analysieren. Die Ergebnisse pro Kategorie (sofern vorhanden) schließen Folgendes ein:

- `_type: 'NewsArticle'`
- `_type: 'WebPage'`
- `_type: 'VideoObject'`
- `_type: 'ImageObject'`

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Web-App](../../tutorial-bing-news-search-single-page-app.md)
