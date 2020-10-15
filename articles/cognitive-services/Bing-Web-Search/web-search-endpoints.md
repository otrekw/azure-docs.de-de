---
title: Websuche-Endpunkt
titleSuffix: Azure Cognitive Services
description: Um Ergebnisse der Websuche zu erhalten, senden Sie eine `GET`-Anforderung an den folgenden Endpunkt. Die Header und die URL-Parameter definieren weitere Spezifikationen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: aahi
ms.openlocfilehash: 2edc6a4cf4d4fb174c1b2067d8b3fb3b6f31859f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "74111338"
---
# <a name="web-search-endpoint"></a>Websuche-Endpunkt

Die **Websuche-API** gibt Webseiten, Nachrichten, Bilder, Videos und [Entitäten](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web) zurück. Entitäten verfügen über zusammenfassende Informationen zur einer Person, einem Ort oder einem Thema.

## <a name="endpoint"></a>Endpunkt

Um mithilfe der Bing-API Websuchergebnisse zu erhalten, senden Sie eine `GET`-Anforderung an den folgenden Endpunkt. Die Header und die URL-Parameter definieren weitere Spezifikationen.

**Endpunkt**: Gibt Webergebnisse zurück, die für die von `?q=""` definierte Suchabfrage des Benutzers relevant sind.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```

Endpunkt: Weitere Informationen zu Headern, Parametern, Marktcodes, Antwortobjekten, Fehlern und Ähnlichem finden Sie in der Referenz [API für die Bing-Websuche v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference).

## <a name="response-json"></a>JSON-Antwort

Die Antwort auf eine Websuchanforderung enthält alle Ergebnisse als JSON-Objekten. Für eine Analyse sind Prozeduren zum Verarbeiten der einzelnen Elementtypen erforderlich. Beispiele finden Sie unter [Tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app) und [Quellcode](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/Tutorials/Bing-Web-Search).

## <a name="next-steps"></a>Nächste Schritte

Die **Bing**-APIs unterstützen Suchaktionen, die Ergebnisse gemäß ihrem Typ zurückgeben. Alle Suchendpunkte geben Ergebnisse als JSON-Antwortobjekte zurück.  Alle Endpunkte unterstützen Abfragen, die eine bestimmte Sprache und einen bestimmten Ort anhand von Längengrad, Breitengrad und Suchradius zurückgeben.

Die umfassenden Informationen zu den Parametern, die von jedem Endpunkt unterstützt werden, finden Sie auf den Referenzseiten des jeweiligen Typs.
Beispiele für grundlegende Anforderung mithilfe der Websuche-API finden Sie in den [Web-Schnellstarts](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web).
