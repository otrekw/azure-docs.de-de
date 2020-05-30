---
title: 'Schnellstart: Senden einer Abfrage an die API mit Python – Bing-Suche für ortsansässige Unternehmen'
titleSuffix: Azure Cognitive Services
description: Nutzen Sie diesen Schnellstart zur Einführung in die Verwendung der API für die Bing-Suche nach ortsansässigen Unternehmen in Python.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: quickstart
ms.date: 05/12/2020
ms.author: aahi
ms.openlocfilehash: 3a90d5455c0664ceabf80647fc94a37ad0c716b5
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873026"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-in-python"></a>Schnellstart: Senden einer Abfrage an die API für die Bing-Suche nach ortsansässigen Unternehmen mit Python

Verwenden Sie diesen Schnellstart, um zu lernen. wie Sie Anforderungen an die API für die Bing-Suche für ortsansässige Unternehmen senden, die zum Leistungsumfang von Azure Cognitive Services gehört. Diese einfache Anwendung ist zwar in Python geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit jeder Programmiersprache kompatibel ist, die HTTP-Anforderungen stellen und JSON analysieren kann.

Diese Beispielanwendung ruft lokale Antwortdaten aus der API für eine Suchabfrage ab.

## <a name="prerequisites"></a>Voraussetzungen

* [Python](https://www.python.org/) 2.x oder 3.x.
* Ein [Cognitive Services-API-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit Bing-Suche-APIs. Für diesen Schnellstart ist die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ausreichend. Speichern Sie den API-Schlüssel, den Sie bei der Aktivierung Ihrer kostenlosen Testversion erhalten. Weitere Informationen finden Sie unter [Cognitive Services-Preise: Bing-Suche-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="run-the-complete-application"></a>Ausführen der vollständigen Anwendung

Im folgenden Beispiel werden lokalisierte Ergebnisse abgerufen, die in den folgenden Schritten implementiert werden:
1. Deklarieren Sie Variablen zum Angeben des Endpunkts nach Host und Pfad.
2. Geben Sie den Abfrageparameter an. 
3. Definieren Sie die Suchfunktion, die die Anforderung erstellt und den Header `Ocp-Apim-Subscription-Key` hinzufügt.
4. Legen Sie den Header `Ocp-Apim-Subscription-Key` fest. 
5. Stellen Sie die Verbindung her, und senden Sie die Anforderung.
6. Drucken Sie die JSON-Ergebnisse.

Der vollständige Code für diese Demo sieht wie folgt aus:

```python
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'

host = 'api.cognitive.microsoft.com'
path = '/bing/v7.0/localbusinesses/search'

query = 'restaurant in Bellevue'

params = '?q=' + urllib.parse.quote (query) + '&mkt=en-us'

def get_local():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_local()
print (json.dumps(json.loads(result), indent=4))

```

## <a name="next-steps"></a>Nächste Schritte
- [Schnellstart: Suche nach ortsansässigen Unternehmen mit Java](local-search-java-quickstart.md)
- [Schnellstart: Suche nach ortsansässigen Unternehmen mit C#](local-quickstart.md)
- [Schnellstart: Suche nach ortsansässigen Unternehmen mit Node.js](local-search-node-quickstart.md)
