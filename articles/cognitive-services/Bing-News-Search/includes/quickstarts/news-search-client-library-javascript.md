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
ms.openlocfilehash: 858e6b9e0e40ab988a4cdf04b31580c1ca28d40a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "79503525"
---
Verwenden Sie diese Schnellstartanleitung, um unter Verwendung der Bing-News-Suche-Clientbibliothek für JavaScript mit der Suche nach Nachrichten zu beginnen. Die Bing-News-Suche verfügt zwar über eine REST-API, die mit den meisten Programmiersprachen kompatibel ist, die Clientbibliothek ist jedoch eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js).

## <a name="prerequisites"></a>Voraussetzungen

* [Node.js](https://nodejs.org/en/)

So richten Sie eine Konsolenanwendung mit der Clientbibliothek für die Bing-News-Suche ein:
1. Führen Sie `npm install ms-rest-azure` in Ihrer Entwicklungsumgebung aus.
2. Führen Sie `npm install azure-cognitiveservices-newssearch` in Ihrer Entwicklungsumgebung aus.


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Erstellen und Initialisieren der Anwendung

1. Erstellen Sie eine Instanz der `CognitiveServicesCredentials`. Erstellen Sie Variablen für Ihren Abonnementschlüssel und einen Suchbegriff.

    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let search_term = 'Winter Olympics'
    ```

2. Instanziieren Sie den Client:
    
    ```javascript
    const NewsSearchAPIClient = require('azure-cognitiveservices-newssearch');
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
