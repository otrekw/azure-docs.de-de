---
title: 'Schnellstart: Aufrufen der Textanalyse-REST-API mithilfe von cURL'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstartanleitung erhalten Sie einen Einstieg in die Verwendung der Textanalyse-API in Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 06/08/2021
ms.author: aahi
ms.openlocfilehash: f932630fe4f31026ffd7efce04b32b876ad8ab94
ms.sourcegitcommit: cc099517b76bf4b5421944bd1bfdaa54153458a0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2021
ms.locfileid: "113550638"
---
# <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

[Referenzdokumentation zu Version 3.1](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1/)

# <a name="version-30"></a>[Version 3.0](#tab/version-3)

[Referenzdokumentation zu Version 3](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0) 

---

## <a name="prerequisites"></a>Voraussetzungen

* Aktuelle Version von [cURL](https://curl.haxx.se/).
* Sobald Sie über Ihr Azure-Abonnement verfügen, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="Erstellen einer Textanalyseressource"  target="_blank"> erstellen Sie eine Textanalyseressource </a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und den Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Textanalyse-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.

> [!NOTE]
> * In den folgenden Bash-Beispielen wird das Zeilenfortsetzungszeichen `\` verwendet. Verwenden Sie dieses Zeichen, falls in Ihrer Konsole oder Ihrem Terminal ein anderes Zeilenfortsetzungszeichen genutzt wird.
> * Sprachspezifische Beispiele finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code).
> * Navigieren Sie zum Azure-Portal, und suchen Sie den Schlüssel und den Endpunkt für die unter „Voraussetzungen“ erstellte Textanalyseressource. Sie befinden sich auf der Seite mit dem **Schlüssel und Endpunkt** der Ressource unter **Ressourcenverwaltung**. Ersetzen Sie dann die Zeichenfolgen im unten angegebenen Code durch Ihren Schlüssel und Endpunkt.
Sie benötigen die folgenden Informationen, um die Textanalyse-API aufzurufen:


|parameter  |BESCHREIBUNG  |
|---------|---------|
|`-X POST <endpoint>`     | Gibt den Endpunkt für den Zugriff auf die API an        |
|`-H Content-Type: application/json`     | Der Inhaltstyp zum Senden von JSON-Daten          |
|`-H "Ocp-Apim-Subscription-Key:<key>`    | Gibt den Schlüssel für den Zugriff auf die API an        |
|`-d <documents>`     | Die JSON-Datei, die die zu sendenden Dokumente enthält         |

Die folgenden cURL-Befehle werden über eine Bash-Shell ausgeführt. Fügen Sie in diese Befehle Ihren Ressourcennamen und Ressourcenschlüssel sowie Ihre JSON-Werte ein.

## <a name="sentiment-analysis"></a>Standpunktanalyse

[!INCLUDE [REST API quickstart instructions](rest-api-instructions.md)]

#### <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

> [!NOTE]
> Das folgende Beispiel enthält eine Anfrage für die Opinion-Mining-Funktion der Standpunktanalysemit dem `opinionMining=true` Parameter, der granulare Informationen über Bewertungen (Adjektive) in Bezug auf Ziele (Substantive) im Text liefert.

```bash
curl -X POST https://<your-text-analytics-endpoint-here>/text/analytics/v3.1/sentiment?opinionMining=true \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <your-text-analytics-key-here>" \
-d '{ documents: [{ id: "1", text: "The customer service here is really good."}]}'
```

### <a name="json-response"></a>JSON-Antwort

```json
{
   "documents":[
      {
         "id":"1",
         "sentiment":"positive",
         "confidenceScores":{
            "positive":1.0,
            "neutral":0.0,
            "negative":0.0
         },
         "sentences":[
            {
               "sentiment":"positive",
               "confidenceScores":{
                  "positive":1.0,
                  "neutral":0.0,
                  "negative":0.0
               },
               "offset":0,
               "length":41,
               "text":"The customer service here is really good.",
               "targets":[
                  {
                     "sentiment":"positive",
                     "confidenceScores":{
                        "positive":1.0,
                        "negative":0.0
                     },
                     "offset":4,
                     "length":16,
                     "text":"customer service",
                     "relations":[
                        {
                           "relationType":"assessment",
                           "ref":"#/documents/0/sentences/0/assessments/0"
                        }
                     ]
                  }
               ],
               "assessments":[
                  {
                     "sentiment":"positive",
                     "confidenceScores":{
                        "positive":1.0,
                        "negative":0.0
                     },
                     "offset":36,
                     "length":4,
                     "text":"good",
                     "isNegated":false
                  }
               ]
            }
         ],
         "warnings":[
            
         ]
      }
   ],
   "errors":[
      
   ],
   "modelVersion":"2020-04-01"
}
``` 

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

```bash
curl -X POST https://<your-text-analytics-endpoint-here>/text/analytics/v3.0/sentiment/ \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <your-text-analytics-key-here>" \
-d '{ documents: [{ id: "1", text: "I had the best day of my life. I wish you were there with me."}]}'
```

### <a name="json-response"></a>JSON-Antwort

```json
{
  "documents":[
    {
      "id":"1",
      "sentiment":"positive",
      "documentScores":{
        "positive":1.0,
        "neutral":0.0,
        "negative":0.0
      },
      "sentences":[
        {
          "sentiment":"positive",
          "sentenceScores":{
            "positive":1.0,
            "neutral":0.0,
            "negative":0.0
          },
          "offset":0,
          "length":30
        }
      ]
    }
  ],
  "errors":[
  ],
  "modelVersion":"2019-10-01"
}
```

---


## <a name="language-detection"></a>Spracherkennung

[!INCLUDE [REST API quickstart instructions](rest-api-instructions.md)]

#### <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

```bash
curl -X POST https://<your-text-analytics-endpoint-here>/text/analytics/v3.1/languages/ \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <your-text-analytics-key-here>" \
-d '{ documents: [{ id: "1", text: "This is a document written in English."}]}'
```

### <a name="json-response"></a>JSON-Antwort

```json
{
   "documents":[
      {
         "id":"1",
         "detectedLanguage":{
            "name":"English",
            "iso6391Name":"en",
            "confidenceScore":1.0
         },
         "warnings":[
            
         ]
      }
   ],
   "errors":[
      
   ],
   "modelVersion":"2021-01-05"
}
```

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

```bash
curl -X POST https://<your-text-analytics-endpoint-here>/text/analytics/v3.0/languages/ \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <your-text-analytics-key-here>" \
-d '{ documents: [{ id: "1", text: "This is a document written in English."}]}'
```

### <a name="json-response"></a>JSON-Antwort

```json
{
   "documents":[
      {
         "id":"1",
         "detectedLanguage":{
            "name":"English",
            "iso6391Name":"en",
            "confidenceScore":0.99
         },
         "warnings":[
            
         ]
      }
   ],
   "errors":[
      
   ],
   "modelVersion":"2020-09-01"
}
```

---


## <a name="named-entity-recognition-ner"></a>Erkennung benannter Entitäten (NER)

[!INCLUDE [REST API quickstart instructions](rest-api-instructions.md)]

#### <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

```bash
curl -X POST https://<your-text-analytics-endpoint-here>/text/analytics/v3.1/entities/recognition/general \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <your-text-analytics-key-here>" \
-d '{ documents: [{ id: "1", language:"en", text: "I had a wonderful trip to Seattle last week."}]}'
```


### <a name="json-response"></a>JSON-Antwort

```json
{
   "documents":[
      {
         "id":"1",
         "entities":[
            {
               "text":"Seattle",
               "category":"Location",
               "subcategory":"GPE",
               "offset":26,
               "length":7,
               "confidenceScore":0.99
            },
            {
               "text":"last week",
               "category":"DateTime",
               "subcategory":"DateRange",
               "offset":34,
               "length":9,
               "confidenceScore":0.8
            }
         ],
         "warnings":[
            
         ]
      }
   ],
   "errors":[
      
   ],
   "modelVersion":"2021-01-15"
}

```

### <a name="detecting-personally-identifying-information"></a>Erkennen personenbezogener Informationen

[!INCLUDE [REST API quickstart instructions](rest-api-instructions.md)]

```bash
curl -X POST https://your-text-analytics-endpoint-here>/text/analytics/v3.1/entities/recognition/pii \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <your-text-analytics-key-here>" \
-d '{ documents: [{ id: "1", language:"en", text: "Call our office at 312-555-1234, or send an email to support@contoso.com"}]}'
```

### <a name="json-response"></a>JSON-Antwort

```json
{
   "documents":[
      {
         "redactedText":"Call our office at ************, or send an email to *******************",
         "id":"1",
         "entities":[
            {
               "text":"312-555-1234",
               "category":"PhoneNumber",
               "offset":19,
               "length":12,
               "confidenceScore":0.8
            },
            {
               "text":"support@contoso.com",
               "category":"Email",
               "offset":53,
               "length":19,
               "confidenceScore":0.8
            }
         ],
         "warnings":[
            
         ]
      }
   ],
   "errors":[
      
   ],
   "modelVersion":"2021-01-15"
}
```

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

```bash
curl -X POST https://<your-text-analytics-endpoint-here>/text/analytics/v3.0/entities/recognition/general \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <your-text-analytics-key-here>" \
-d '{ documents: [{ id: "1", language:"en", text: "I had a wonderful trip to Seattle last week."}]}'
```


### <a name="json-response"></a>JSON-Antwort

```json
{
   "documents":[
      {
         "id":"1",
         "entities":[
            {
               "text":"trip",
               "category":"Event",
               "offset":18,
               "length":4,
               "confidenceScore":0.61
            },
            {
               "text":"Seattle",
               "category":"Location",
               "subcategory":"GPE",
               "offset":26,
               "length":7,
               "confidenceScore":0.82
            },
            {
               "text":"last week",
               "category":"DateTime",
               "subcategory":"DateRange",
               "offset":34,
               "length":9,
               "confidenceScore":0.8
            }
         ],
         "warnings":[
            
         ]
      }
   ],
   "errors":[
      
   ],
   "modelVersion":"2020-04-01"
}
```

---

## <a name="entity-linking"></a>Entitätsverknüpfung

[!INCLUDE [REST API quickstart instructions](rest-api-instructions.md)]

#### <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

```bash
curl -X POST https://<your-text-analytics-endpoint-here>/text/analytics/v3.1/entities/linking \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <your-text-analytics-key-here>" \
-d '{ documents: [{ id: "1", language:"en", text: "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975."}]}'
```

### <a name="json-response"></a>JSON-Antwort

```json
{
   "documents":[
      {
         "id":"1",
         "entities":[
            {
               "bingId":"a093e9b9-90f5-a3d5-c4b8-5855e1b01f85",
               "name":"Microsoft",
               "matches":[
                  {
                     "text":"Microsoft",
                     "offset":0,
                     "length":9,
                     "confidenceScore":0.48
                  }
               ],
               "language":"en",
               "id":"Microsoft",
               "url":"https://en.wikipedia.org/wiki/Microsoft",
               "dataSource":"Wikipedia"
            },
            {
               "bingId":"0d47c987-0042-5576-15e8-97af601614fa",
               "name":"Bill Gates",
               "matches":[
                  {
                     "text":"Bill Gates",
                     "offset":25,
                     "length":10,
                     "confidenceScore":0.52
                  }
               ],
               "language":"en",
               "id":"Bill Gates",
               "url":"https://en.wikipedia.org/wiki/Bill_Gates",
               "dataSource":"Wikipedia"
            },
            {
               "bingId":"df2c4376-9923-6a54-893f-2ee5a5badbc7",
               "name":"Paul Allen",
               "matches":[
                  {
                     "text":"Paul Allen",
                     "offset":40,
                     "length":10,
                     "confidenceScore":0.54
                  }
               ],
               "language":"en",
               "id":"Paul Allen",
               "url":"https://en.wikipedia.org/wiki/Paul_Allen",
               "dataSource":"Wikipedia"
            },
            {
               "bingId":"52535f87-235e-b513-54fe-c03e4233ac6e",
               "name":"April 4",
               "matches":[
                  {
                     "text":"April 4",
                     "offset":54,
                     "length":7,
                     "confidenceScore":0.38
                  }
               ],
               "language":"en",
               "id":"April 4",
               "url":"https://en.wikipedia.org/wiki/April_4",
               "dataSource":"Wikipedia"
            }
         ],
         "warnings":[
            
         ]
      }
   ],
   "errors":[
      
   ],
   "modelVersion":"2020-02-01"
}
```

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

```bash
curl -X POST https://<your-text-analytics-endpoint-here>/text/analytics/v3.0/entities/linking \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <your-text-analytics-key-here>" \
-d '{ documents: [{ id: "1", language:"en", text: "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975."}]}'
```


### <a name="json-response"></a>JSON-Antwort

```json
{
   "documents":[
      {
         "id":"1",
         "entities":[
            {
               "name":"Microsoft",
               "matches":[
                  {
                     "text":"Microsoft",
                     "offset":0,
                     "length":9,
                     "confidenceScore":0.48
                  }
               ],
               "language":"en",
               "id":"Microsoft",
               "url":"https://en.wikipedia.org/wiki/Microsoft",
               "dataSource":"Wikipedia"
            },
            {
               "name":"Bill Gates",
               "matches":[
                  {
                     "text":"Bill Gates",
                     "offset":25,
                     "length":10,
                     "confidenceScore":0.52
                  }
               ],
               "language":"en",
               "id":"Bill Gates",
               "url":"https://en.wikipedia.org/wiki/Bill_Gates",
               "dataSource":"Wikipedia"
            },
            {
               "name":"Paul Allen",
               "matches":[
                  {
                     "text":"Paul Allen",
                     "offset":40,
                     "length":10,
                     "confidenceScore":0.54
                  }
               ],
               "language":"en",
               "id":"Paul Allen",
               "url":"https://en.wikipedia.org/wiki/Paul_Allen",
               "dataSource":"Wikipedia"
            },
            {
               "name":"April 4",
               "matches":[
                  {
                     "text":"April 4",
                     "offset":54,
                     "length":7,
                     "confidenceScore":0.38
                  }
               ],
               "language":"en",
               "id":"April 4",
               "url":"https://en.wikipedia.org/wiki/April_4",
               "dataSource":"Wikipedia"
            }
         ],
         "warnings":[
            
         ]
      }
   ],
   "errors":[
      
   ],
   "modelVersion":"2020-02-01"
}
```


---


## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

[!INCLUDE [REST API quickstart instructions](rest-api-instructions.md)]


#### <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

```bash
curl -X POST https://<your-text-analytics-endpoint-here>/text/analytics/v3.1/keyPhrases \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <your-text-analytics-key-here>" \
-d '{ documents: [{ id: "1", language:"en", text: "I had a wonderful trip to Seattle last week."}]}'
```

```json
{
   "documents":[
      {
         "id":"1",
         "keyPhrases":[
            "wonderful trip",
            "Seattle"
         ],
         "warnings":[
            
         ]
      }
   ],
   "errors":[
      
   ],
   "modelVersion":"2021-06-01"
}
```

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

```bash
curl -X POST https://<your-text-analytics-endpoint-here>/text/analytics/v3.0/keyPhrases \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <your-text-analytics-key-here>" \
-d '{ documents: [{ id: "1", language:"en", text: "I had a wonderful trip to Seattle last week."}]}'
```

### <a name="json-response"></a>JSON-Antwort

```json
{
   "documents":[
      {
         "id":"1",
         "keyPhrases":[
            "wonderful trip",
            "Seattle",
            "week"
         ],
         "warnings":[
            
         ]
      }
   ],
   "errors":[
      
   ],
   "modelVersion":"2020-07-01"
}
```

---
