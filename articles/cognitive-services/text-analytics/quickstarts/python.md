---
title: 'Schnellstart: Verwenden von Python zum Aufrufen der Textanalyse-API'
titleSuffix: Azure Cognitive Services
description: Diese Schnellstartanleitung zeigt Ihnen, wie Sie Informationen und Codebeispiele erhalten, damit Sie die ersten Schritte mit der Textanalyse-API in Azure Cognitive Services schnell durchführen können.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 07/20/2020
ms.author: aahi
ms.custom: tracking-python
ms.openlocfilehash: 99c22edac7acefe3ec34ff40e892a7071b932ada
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87013809"
---
# <a name="quickstart-using-the-python-rest-api-to-call-the-text-analytics-cognitive-service"></a>Schnellstart: Aufrufen der Textanalyse von Cognitive Services mit der Python-REST-API 
<a name="HOLTop"></a>

Verwenden Sie diesen Schnellstart, um mit der Textanalyse-REST-API und Python in die Sprachanalyse einzusteigen. Dieser Artikel veranschaulicht, wie Sie eine [Sprache erkennen](#Detect), [Stimmungen analysieren](#SentimentAnalysis), [Schlüsselbegriffe extrahieren](#KeyPhraseExtraction) und [verknüpfte Entitäten erkennen](#Entities).

[!INCLUDE [text-analytics-api-references](../includes/text-analytics-api-references.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Python 3.x](https://python.org)

* Die Anforderungsbibliothek für Python
    
    Sie können die Bibliothek mit dem folgenden Befehl installieren:

    ```console
    pip install --upgrade requests
    ```

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]


## <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

Erstellen Sie in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE eine neue Python-Anwendung. Fügen Sie Ihrer Datei die folgenden Importe hinzu.

```python
import requests
# pprint is used to format the JSON response
from pprint import pprint
```

Erstellen Sie Variablen für den Azure-Endpunkt und den Abonnementschlüssel Ihrer Ressource.
    
```python
import os

subscription_key = "<paste-your-text-analytics-key-here>"
endpoint = "<paste-your-text-analytics-endpoint-here>"
```

In den folgenden Abschnitten wird das Aufrufen der einzelnen API-Funktionen beschrieben.

<a name="Detect"></a>

## <a name="detect-languages"></a>Sprachenerkennung

Fügen Sie `/text/analytics/v3.0/languages` an den Basisendpunkt für die Textanalyse an, um die URL für die Sprachenerkennung zu bilden. Beispiel: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/languages`
    
```python
language_api_url = endpoint + "/text/analytics/v3.0/languages"
```

Die Nutzlast an die API besteht aus einer Liste von `documents`, die jeweils Tupel aus einem `id`- und einem `text`-Attribut sind. Das `text`-Attribut speichert den Text, der analysiert werden soll, und die `id` kann ein beliebiger Wert sein. 

```python
documents = {"documents": [
    {"id": "1", "text": "This is a document written in English."},
    {"id": "2", "text": "Este es un document escrito en Español."},
    {"id": "3", "text": "这是一个用中文写的文件"}
]}
```

Verwenden Sie die Anforderungsbibliothek, um die Dokumente an die API zu senden. Fügen Sie dem Header `Ocp-Apim-Subscription-Key` Ihren Abonnementschlüssel hinzu, und senden Sie die Anforderung mit `requests.post()` zurück. 

```python
headers = {"Ocp-Apim-Subscription-Key": subscription_key}
response = requests.post(language_api_url, headers=headers, json=documents)
languages = response.json()
pprint(languages)
```

### <a name="output"></a>Output

```json
{
    "documents": [
        {
            "id": "1",
            "detectedLanguage": {
                "name": "English",
                "iso6391Name": "en",
                "confidenceScore": 1.0
            },
            "warnings": []
        },
        {
            "id": "2",
            "detectedLanguage": {
                "name": "Spanish",
                "iso6391Name": "es",
                "confidenceScore": 1.0
            },
            "warnings": []
        },
        {
            "id": "3",
            "detectedLanguage": {
                "name": "Chinese_Simplified",
                "iso6391Name": "zh_chs",
                "confidenceScore": 1.0
            },
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2019-10-01"
}
```

<a name="SentimentAnalysis"></a>

## <a name="analyze-sentiment"></a>Analysieren von Stimmungen

Fügen Sie zum Erkennen der Stimmung (zwischen positiv und negativ) in einem Satz von Dokumenten `/text/analytics/v3.0/sentiment` an den Basisendpunkt der Textanalyse an, um die URL für die Sprachenerkennung zu bilden. Beispiel: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/sentiment`
    
```python
sentiment_url = endpoint + "/text/analytics/v3.0/sentiment"
```

Erstellen Sie wie im Sprachenerkennungsbeispiel ein Wörterbuch mit einem `documents`-Schlüssel, der aus einer Dokumentenliste besteht. Jedes Dokument ist ein Tupel aus der `id`, dem zu analysierenden `text` und der `language` des Texts. 

```python
documents = {"documents": [
    {"id": "1", "language": "en",
        "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."},
    {"id": "2", "language": "es",
        "text": "Este ha sido un dia terrible, llegué tarde al trabajo debido a un accidente automobilistico."}
]}
```

Verwenden Sie die Anforderungsbibliothek, um die Dokumente an die API zu senden. Fügen Sie dem Header `Ocp-Apim-Subscription-Key` Ihren Abonnementschlüssel hinzu, und senden Sie die Anforderung mit `requests.post()` zurück. 

```python
headers = {"Ocp-Apim-Subscription-Key": subscription_key}
response = requests.post(sentiment_url, headers=headers, json=documents)
sentiments = response.json()
pprint(sentiments)
```

### <a name="output"></a>Output

Die Stimmungspunktzahl eines Dokuments liegt zwischen 0,0 und 1,0, dabei gilt: Je höher der Wert, umso positiver die Stimmung.

```json
{
    "documents": [
        {
            "id": "1",
            "sentiment": "positive",
            "confidenceScores": {
                "positive": 1.0,
                "neutral": 0.0,
                "negative": 0.0
            },
            "sentences": [
                {
                    "sentiment": "positive",
                    "confidenceScores": {
                        "positive": 1.0,
                        "neutral": 0.0,
                        "negative": 0.0
                    },
                    "offset": 0,
                    "length": 102,
                    "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."
                }
            ],
            "warnings": []
        },
        {
            "id": "2",
            "sentiment": "negative",
            "confidenceScores": {
                "positive": 0.02,
                "neutral": 0.05,
                "negative": 0.93
            },
            "sentences": [
                {
                    "sentiment": "negative",
                    "confidenceScores": {
                        "positive": 0.02,
                        "neutral": 0.05,
                        "negative": 0.93
                    },
                    "offset": 0,
                    "length": 92,
                    "text": "Este ha sido un dia terrible, llegué tarde al trabajo debido a un accidente automobilistico."
                }
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2020-04-01"
}
```

<a name="KeyPhraseExtraction"></a>

## <a name="extract-key-phrases"></a>Extrahieren von Schlüsselbegriffen
 
Fügen Sie zum Extrahieren der Schlüsselausdrücke aus einem Satz von Dokumenten `/text/analytics/v3.0/keyPhrases` an den Basisendpunkt für die Textanalyse an, um die URL für die Sprachenerkennung zu bilden. Beispiel: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/keyPhrases`
    
```python
keyphrase_url = endpoint + "/text/analytics/v3.0/keyphrases"
```

Diese Sammlung der Dokumente entspricht der Sammlung, die für das Beispiel zur Standpunktanalyse verwendet wurde.

```python
documents = {"documents": [
    {"id": "1", "language": "en",
        "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."},
    {"id": "2", "language": "es",
        "text": "Si usted quiere comunicarse con Carlos, usted debe de llamarlo a su telefono movil. Carlos es muy responsable, pero necesita recibir una notificacion si hay algun problema."},
    {"id": "3", "language": "en",
        "text": "The Grand Hotel is a new hotel in the center of Seattle. It earned 5 stars in my review, and has the classiest decor I've ever seen."}
]}
```

Verwenden Sie die Anforderungsbibliothek, um die Dokumente an die API zu senden. Fügen Sie dem Header `Ocp-Apim-Subscription-Key` Ihren Abonnementschlüssel hinzu, und senden Sie die Anforderung mit `requests.post()` zurück. 

```python
headers = {"Ocp-Apim-Subscription-Key": subscription_key}
response = requests.post(keyphrase_url, headers=headers, json=documents)
key_phrases = response.json()
pprint(key_phrases)
```

### <a name="output"></a>Output

```json
{
    "documents": [
        {
            "id": "1",
            "keyPhrases": [
                "HDR resolution",
                "new XBox",
                "clean look"
            ],
            "warnings": []
        },
        {
            "id": "2",
            "keyPhrases": [
                "Carlos",
                "notificacion",
                "algun problema",
                "telefono movil"
            ],
            "warnings": []
        },
        {
            "id": "3",
            "keyPhrases": [
                "new hotel",
                "Grand Hotel",
                "review",
                "center of Seattle",
                "classiest decor",
                "stars"
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2019-10-01"
}
```

<a name="Entities"></a>

## <a name="identify-entities"></a>Identifizieren von Entitäten

Fügen Sie zum Identifizieren von bekannten Entitäten (Personen, Orten und Gegenständen) in Textdokumenten `/text/analytics/v3.0/entities/recognition/general` an den Basisendpunkt für die Textanalyse an, um die URL für die Sprachenerkennung zu bilden. Beispiel: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/recognition/general`
    
```python
entities_url = endpoint + "/text/analytics/v3.0/entities/recognition/general"
```

Erstellen Sie wie in den vorherigen Beispielen eine Sammlung von Dokumenten. 

```python
documents = {"documents": [
    {"id": "1", "text": "Microsoft is an It company."}
]}
```

Verwenden Sie die Anforderungsbibliothek, um die Dokumente an die API zu senden. Fügen Sie dem Header `Ocp-Apim-Subscription-Key` Ihren Abonnementschlüssel hinzu, und senden Sie die Anforderung mit `requests.post()` zurück.

```python
headers = {"Ocp-Apim-Subscription-Key": subscription_key}
response = requests.post(entities_url, headers=headers, json=documents)
entities = response.json()
pprint(entities)
```

### <a name="output"></a>Output

```json
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "text": "Microsoft",
                    "category": "Organization",
                    "offset": 0,
                    "length": 9,
                    "confidenceScore": 0.86
                },
                {
                    "text": "IT",
                    "category": "Skill",
                    "offset": 16,
                    "length": 2,
                    "confidenceScore": 0.8
                }
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2020-04-01"
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Textanalyse mit Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Weitere Informationen 

 [Übersicht über die Textanalyse](../overview.md)  
 [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md)
