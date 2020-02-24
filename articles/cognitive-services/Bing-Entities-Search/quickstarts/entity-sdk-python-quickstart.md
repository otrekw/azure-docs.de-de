---
title: 'Schnellstart: Suchen nach Entitäten mit dem SDK für Python – Bing-Entitätssuche'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung, um unter Verwendung des Bing Entity Search SDK für Python nach Entitäten zu suchen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/11/2020
ms.author: aahi
ms.openlocfilehash: a6b62f7ab95f7b2720434c0cf59cce33b0adb1b4
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77201236"
---
# <a name="quickstart-bing-entity-search-sdk-with-python"></a>Schnellstart: Bing-Entitätssuche-SDK mit Python

Verwenden Sie diese Schnellstartanleitung, um unter Verwendung des Bing-Entitätssuche-SDK für Python mit der Suche nach Entitäten zu beginnen. Die Bing-Entitätssuche verfügt zwar über eine REST-API, die mit den meisten Programmiersprachen kompatibel ist, aber das SDK ist eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/entity_search_samples.py).

## <a name="prerequisites"></a>Voraussetzungen

* Python [2.x oder 3.x](https://www.python.org/)

* Das [Bing-Entitätssuche-SDK für Python](https://pypi.org/project/azure-cognitiveservices-search-entitysearch/)

Es wird empfohlen, eine virtuelle Python-Umgebung zu verwenden. Sie können eine virtuelle Umgebung mit dem venv-Modul installieren und initialisieren. virtualenv kann wie folgt installiert werden:

```Console
python -m venv mytestenv
```

Installieren Sie das Bing-Entitätssuche-SDK wie folgt:

```Console
cd mytestenv
python -m pip install azure-cognitiveservices-search-entitysearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Erstellen und Initialisieren der Anwendung

1. Erstellen Sie in Ihrer bevorzugten IDE bzw. in einem Editor eine neue Python-Datei, und fügen Sie die folgenden Importanweisungen hinzu. 

    ```python
    from azure.cognitiveservices.search.entitysearch import EntitySearchClient
    from azure.cognitiveservices.search.entitysearch.models import Place, ErrorResponseException
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Erstellen Sie eine Variable für Ihren Abonnementschlüssel und den Endpunkt. Instanziieren Sie den Client, indem Sie ein neues `CognitiveServicesCredentials`-Objekt mit Ihrem Schlüssel erstellen.
    
    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    client = EntitySearchclient(endpoint=endpoint, credentials=CognitiveServicesCredentials(subscription_key))
    ```

## <a name="send-a-search-request-and-receive-a-response"></a>Senden einer Suchanforderung und Erhalten einer Antwort

1. Senden Sie an die Bing-Entitätssuche eine Suchanforderung mit `client.entities.search()` und einer Suchabfrage. 
    
    ```python
    entity_data = client.entities.search(query="Gibralter")
    ```

2. Falls Entitäten zurückgegeben wurden, konvertieren Sie `entity_data.entities.value` in eine Liste, und geben Sie das erste Ergebnis aus.
    ```python
    if entity_data.entities.value:
    
        main_entities = [entity for entity in entity_data.entities.value
                         if entity.entity_presentation_info.entity_scenario == "DominantEntity"]
    
        if main_entities:
            print(main_entities[0].description)
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Web-App](../tutorial-bing-entities-search-single-page-app.md)

* [Was ist die Bing-Entitätssuche-API?](../overview.md )