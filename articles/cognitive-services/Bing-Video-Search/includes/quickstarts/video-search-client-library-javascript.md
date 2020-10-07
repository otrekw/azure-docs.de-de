---
title: 'Schnellstart: JavaScript-Clientbibliothek für die Bing-Videosuche'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/19/2020
ms.author: aahi
ms.custom: devx-track-js
ms.openlocfilehash: fa1ecb24649d9355e6104bff8add5430fdd64c3c
ms.sourcegitcommit: 5dbea4631b46d9dde345f14a9b601d980df84897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376291"
---
Verwenden Sie diese Schnellstartanleitung, um unter Verwendung der Bing-Videosuche-Clientbibliothek für JavaScript mit der Suche nach Nachrichten zu beginnen. Die Bing-Videosuche verfügt zwar über eine REST-API, die mit den meisten Programmiersprachen kompatibel ist, die Clientbibliothek ist jedoch eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js). Er enthält weitere Anmerkungen und Features.

## <a name="prerequisites"></a>Voraussetzungen

* Die aktuelle Version von [Node.js](https://nodejs.org/en/download/)
* Das [Bing Videosuche-SDK für JavaScript](https://www.npmjs.com/package/@azure/cognitiveservices-videosearch)
     *  Führen Sie zum Installieren `npm install @azure/cognitiveservices-videosearch` aus.
* Die `CognitiveServicesCredentials`-Klasse aus dem Paket `@azure/ms-rest-azure-js` zum Authentifizieren des Clients.
     * Führen Sie zum Installieren `npm install @azure/ms-rest-azure-js` aus.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](~/includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Erstellen und Initialisieren der Anwendung

1. Erstellen Sie eine neue JavaScript-Datei in Ihrer bevorzugten IDE oder einem Editor, und fügen Sie eine `require()`-Anweisung für die Bing-Videosuche-Clientbibliothek sowie das Modul `CognitiveServicesCredentials` hinzu. Erstellen Sie eine Variable für Ihren Abonnementschlüssel. 
    
    ```javascript
    const CognitiveServicesCredentials = require('@azure/ms-rest-azure-js').CognitiveServicesCredentials;
    const VideoSearchAPIClient = require('@azure/cognitiveservices-videosearch');
    ```

2. Erstellen Sie eine Instanz der `CognitiveServicesCredentials`-Klasse mit Ihrem Schlüssel. Verwenden Sie sie anschließend zum Erstellen einer Instanz des Clients für die Videosuche.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let client = new VideoSearchAPIClient(credentials);
    ```

## <a name="send-the-search-request"></a>Senden der Suchanforderung

1. Senden Sie mithilfe von `client.videosOperations.search()` eine Suchanforderung an die Bing-Videosuche-API. Wenn die Suchergebnisse zurückgegeben werden, verwenden Sie `.then()`, um das Ergebnis zu protokollieren.
    
    ```javascript
    client.videosOperations.search('Interstellar Trailer').then((result) => {
        console.log(result.value);
    }).catch((err) => {
        throw err;
    });
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Webanwendung](../../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Weitere Informationen 

* [Worum handelt es sich bei der Bing-Videosuche-API?](../../overview.md)
* [Cognitive Services SDK-Beispiele für .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)