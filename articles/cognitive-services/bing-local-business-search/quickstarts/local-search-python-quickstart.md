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
ms.custom: devx-track-python
ms.openlocfilehash: 30e66260f0603139bf45f9a0f0b5f19539ae468e
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87843554"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-in-python"></a>Schnellstart: Senden einer Abfrage an die API für die Bing-Suche nach ortsansässigen Unternehmen mit Python

Verwenden Sie diesen Schnellstart, um zu lernen. wie Sie Anforderungen an die API für die Bing-Suche für ortsansässige Unternehmen senden, die zum Leistungsumfang von Azure Cognitive Services gehört. Diese einfache Anwendung ist zwar in Python geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit jeder Programmiersprache kompatibel ist, die HTTP-Anforderungen stellen und JSON analysieren kann.

Diese Beispielanwendung ruft lokale Antwortdaten aus der API für eine Suchabfrage ab.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: [Kostenloses Azure-Konto](https://azure.microsoft.com/free/cognitive-services/)
* [Python](https://www.python.org/) 2.x oder 3.x.
* Sobald Sie über Ihr Azure-Abonnement verfügen, sollten Sie über <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7"  title="Erstellen einer Ressource für die Bing-Suche"  target="_blank"> im Azure-Portal eine Ressource für die Bing-Suche <span class="docon docon-navigate-external x-hidden-focus"></span></a> erstellen, um Ihren Schlüssel und Endpunkt abzurufen. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.

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
