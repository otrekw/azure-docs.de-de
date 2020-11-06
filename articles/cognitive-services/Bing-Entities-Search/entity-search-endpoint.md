---
title: API-Endpunkt für die Bing-Entitätssuche
titleSuffix: Azure Cognitive Services
description: Die API für die Bing-Entitätssuche verfügt über einen Endpunkt, der basierend auf einer Abfrage Entitäten aus dem Internet zurückgibt. Diese Suchergebnisse werden im JSON-Format zurückgegeben.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 5d6b0a6be0d99815c4d14bb04e56241d011812b5
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93084951"
---
# <a name="bing-entity-search-api-endpoint"></a>API-Endpunkt für die Bing-Entitätssuche

> [!WARNING]
> Die APIs der Bing-Suche werden von Cognitive Services auf Bing-Suchdienste umgestellt. Ab dem **30. Oktober 2020** müssen alle neuen Instanzen der Bing-Suche mit dem [hier](https://aka.ms/cogsvcs/bingmove) dokumentierten Prozess bereitgestellt werden.
> APIs der Bing-Suche, die mit Cognitive Services bereitgestellt wurden, werden noch drei Jahre lang bzw. bis zum Ablauf Ihres Enterprise Agreement unterstützt (je nachdem, was zuerst geschieht).
> Eine Anleitung zur Migration finden Sie unter [Bing-Suchdienste](https://aka.ms/cogsvcs/bingmigration).


Die API für die Bing-Entitätssuche verfügt über einen Endpunkt, der basierend auf einer Abfrage Entitäten aus dem Internet zurückgibt. Diese Suchergebnisse werden im JSON-Format zurückgegeben.

## <a name="get-entity-results-from-the-endpoint"></a>Abrufen von Entitätsergebnissen vom Endpunkt

Um mithilfe der **Bing-API** Entitätsergebnisse zu erhalten, senden Sie eine `GET`-Anforderung an den folgenden Endpunkt. Verwenden Sie [Header](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#headers) und [Abfrageparameter](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#query-parameters), um Ihre Suchanforderungen anzupassen. Suchanforderungen können mithilfe des Parameters `?q=` gesendet werden.

```cURL
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Was ist die Bing-Entitätssuche-API?](overview.md)

## <a name="see-also"></a>Weitere Informationen 

Weitere Informationen zu Headern, Parametern, Marktcodes, Antwortobjekten, Fehlern usw. finden Sie im Referenzartikel zur [Bing-Entitätssuche-API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference).
