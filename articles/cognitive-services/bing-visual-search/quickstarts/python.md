---
title: 'Schnellstart: Gewinnen von Erkenntnissen zu Bildern mit der REST-API und Python – Visuelle Bing-Suche'
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie ein Bild in die API für die visuelle Bing-Suche hochladen und Erkenntnisse dazu gewinnen.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 4/02/2019
ms.author: scottwhi
ms.openlocfilehash: 6fafc35d9d74927789fee3f3fea3014ff3be5717
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74383179"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-python"></a>Schnellstart: Gewinnen von Erkenntnissen zu Bildern mit der REST-API für die visuelle Bing-Suche und Python

Verwenden Sie diese Schnellstartanleitung, um die API für die visuelle Bing-Suche zum ersten Mal aufzurufen und die Ergebnisse anzuzeigen. Diese Python-Anwendung lädt ein Bild in die API hoch und zeigt die zurückgegebenen Informationen an. Die Anwendung ist zwar in Python geschrieben, an sich ist die API aber ein RESTful-Webdienst und mit den meisten Programmiersprachen kompatibel.

Wenn Sie ein lokales Bild hochladen, müssen die Formulardaten den Header `Content-Disposition` enthalten. Der Parameter `name` muss auf „image“ festgelegt werden. Den Parameter `filename` können Sie auf eine beliebige Zeichenfolge festlegen. Der Inhalt des Formulars umfasst die Binärdaten des Bilds. Das hochzuladende Bild darf maximal 1 MB groß sein.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Voraussetzungen

* [Python 3.x](https://www.python.org/)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Initialisieren der Anwendung

1. Erstellen Sie in Ihrer bevorzugten IDE bzw. in einem Editor eine neue Python-Datei, und fügen Sie die folgende `import`-Anweisung hinzu:

    ```python
    import requests, json
    ```

2. Erstellen Sie Variablen für Ihren Abonnementschlüssel, den Endpunkt und den Pfad zu dem Bild, das hochgeladen werden soll:

    ```python

    BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'
    SUBSCRIPTION_KEY = 'your-subscription-key'
    imagePath = 'your-image-path'
    ```

3. Erstellen Sie ein Wörterbuchobjekt für die Headerinformationen Ihrer Anforderung. Binden Sie Ihren Abonnementschlüssel wie hier gezeigt an die Zeichenfolge `Ocp-Apim-Subscription-Key`:

    ```python
    HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}
    ```

4. Erstellen Sie ein weiteres Wörterbuch für Ihr Bild, das beim Senden der Anforderung geöffnet und hochgeladen wird:

    ```python
    file = {'image' : ('myfile', open(imagePath, 'rb'))}
    ```

## <a name="parse-the-json-response"></a>Analysieren der JSON-Antwort

1. Erstellen Sie eine Methode mit dem Namen `print_json()` für die API-Antwort, und geben Sie den JSON-Code aus:

    ```python
    def print_json(obj):
        """Print the object as json"""
        print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))
    ```

## <a name="send-the-request"></a>Senden der Anforderung

1. Verwenden Sie `requests.post()`, um eine Anforderung an die API für die visuelle Bing-Suche zu senden. Fügen Sie die Zeichenfolge für Ihren Endpunkt, den Header und die Dateiinformationen ein. Geben Sie `response.json()` mit `print_json()` aus:

    ```python
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())
    
    except Exception as ex:
        raise ex
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Web-App für die visuelle Suche](../tutorial-bing-visual-search-single-page-app.md)
