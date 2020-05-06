---
title: 'Schnellstart: Ausführen einer Suche mit Python – Bing-Websuche-API'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung zum Senden von Anforderungen an die REST-API der Bing-Websuche mit Python, und Sie erhalten eine JSON-Antwort.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: c1af1142faca76cc58b6b3ca9a7106bc0433ea18
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "74976365"
---
# <a name="quickstart-use-python-to-call-the-bing-web-search-api"></a>Schnellstart: Verwenden von Python zum Aufrufen der Bing-Websuche-API  

In dieser Schnellstartanleitung erfahren Sie, wie Sie die Bing-Websuche-API erstmals aufrufen und die JSON-Antwort empfangen. Diese Python-Anwendung sendet eine Suchanforderung an die API und zeigt die Antwort an. Diese Anwendung ist zwar in Python geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist.

Dieses Beispiel wird als Jupyter-Notebook unter [MyBinder](https://mybinder.org) ausgeführt. Wählen Sie den Badge zum Starten von Binder aus:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)

## <a name="prerequisites"></a>Voraussetzungen

* [Python 2.x oder 3.x](https://www.python.org/)

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="define-variables"></a>Definieren von Variablen

Ersetzen Sie den Wert von `subscription_key` durch einen gültigen Abonnementschlüssel aus Ihrem Azure-Konto.

```python
subscription_key = "YOUR_ACCESS_KEY"
assert subscription_key
```

Deklarieren Sie den Endpunkt für die Bing-Websuche-API. Sie können den unten angegebenen globalen Endpunkt oder den Endpunkt der [benutzerdefinierten Unterdomäne](../../../cognitive-services/cognitive-services-custom-subdomains.md) verwenden, der im Azure-Portal für Ihre Ressource angezeigt wird.

```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

Sie können die Suchabfrage auch anpassen, indem Sie den Wert für `search_term` ersetzen.

```python
search_term = "Azure Cognitive Services"
```

## <a name="make-a-request"></a>Erstellen einer Anforderung

In diesem Block wird die `requests`-Bibliothek verwendet, um die Bing-Websuche-API aufzurufen und die Ergebnisse als JSON-Objekt zurückzugeben. Der API-Schlüssel wird im `headers`-Wörterbuch übergeben, und der Suchbegriff und die Abfrageparameter werden im `params`-Wörterbuch übergeben. Eine vollständige Liste mit Optionen und Parametern finden Sie in der Dokumentation zur [Bing-Websuche-API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference).

```python
import requests

headers = {"Ocp-Apim-Subscription-Key": subscription_key}
params = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

## <a name="format-and-display-the-response"></a>Formatieren und Anzeigen der Antwort

Das `search_results`-Objekt enthält die Suchergebnisse und Metadaten, z. B. verwandte Abfragen und Seiten. In diesem Code wird die `IPython.display`-Bibliothek verwendet, um die Antwort in Ihrem Browser zu formatieren und anzuzeigen.

```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"], v["name"], v["snippet"])
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="sample-code-on-github"></a>Beispielcode auf GitHub

Wenn Sie diesen Code lokal ausführen möchten, können Sie das vollständige [Beispiel auf GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingWebSearchv7.py) verwenden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Einseitige Web-App für die Bing-Websuche](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
