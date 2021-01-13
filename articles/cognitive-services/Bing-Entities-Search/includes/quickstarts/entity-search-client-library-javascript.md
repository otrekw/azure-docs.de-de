---
title: 'Schnellstart: JavaScript-Clientbibliothek für die Bing-Entitätssuche'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: aahi
ms.custom: devx-track-js
ms.openlocfilehash: b145cc1689ad2c1a39591df0e39bb8d0445333c7
ms.sourcegitcommit: 5dbea4631b46d9dde345f14a9b601d980df84897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376708"
---
Verwenden Sie diese Schnellstartanleitung, um unter Verwendung der Bing-Entitätssuche-Clientbibliothek für JavaScript mit der Suche nach Entitäten zu beginnen. Die Bing-Entitätssuche verfügt zwar über eine REST-API, die mit den meisten Programmiersprachen kompatibel ist, die Clientbibliothek ist jedoch eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js).

## <a name="prerequisites"></a>Voraussetzungen

* Die aktuelle Version von [Node.js](https://nodejs.org/en/download/)
* Das [Bing Entitätssuche-SDK für JavaScript](https://www.npmjs.com/package/@azure/cognitiveservices-entitysearch)
     *  Führen Sie zum Installieren `npm install @azure/cognitiveservices-entitysearch` aus.
* Die `CognitiveServicesCredentials`-Klasse aus dem Paket `@azure/ms-rest-azure-js` zum Authentifizieren des Clients.
     * Führen Sie zum Installieren `npm install @azure/ms-rest-azure-js` aus.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-entity-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Erstellen und Initialisieren der Anwendung

1. Erstellen Sie eine neue JavaScript-Datei in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor, und fügen Sie die folgenden Anforderungen hinzu.

    ```javascript
    const CognitiveServicesCredentials = require('@azure/ms-rest-azure-js').CognitiveServicesCredentials;
    const EntitySearchAPIClient = require('@azure/cognitiveservices-entitysearch');
    ```

2. Erstellen Sie mithilfe Ihres Abonnementschlüssels eine Instanz von `CognitiveServicesCredentials`. Dann erstellen Sie damit eine Instanz des Suchclients.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let entitySearchApiClient = new EntitySearchAPIClient(credentials);
    ```

## <a name="send-a-request-and-receive-a-response"></a>Senden einer Anforderung und Erhalten einer Antwort

1. Senden Sie mit `entitiesOperations.search()` eine Suchanforderung für Entitäten. Nach dem Empfang einer Antwort geben Sie das `queryContext`-Element, die Anzahl der zurückgegebenen Ergebnisse, und die Beschreibung des ersten Ergebnisses aus.

    ```javascript
    entitySearchApiClient.entitiesOperations.search('seahawks').then((result) => {
        console.log(result.queryContext);
        console.log(result.entities.value);
        console.log(result.entities.value[0].description);
    }).catch((err) => {
        throw err;
    });
    ```

<!-- Removing until we can replace with a sanitized version.
![Entity results](media/entity-search-sdk-node-quickstart-results.png)
-->

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Web-App](../../tutorial-bing-entities-search-single-page-app.md)

* [Was ist die Bing-Entitätssuche-API?](../../overview.md)