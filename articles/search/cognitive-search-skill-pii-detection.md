---
title: Die kognitive Qualifikation „PII-Erkennung“ (Vorschau)
titleSuffix: Azure Cognitive Search
description: Extrahieren und maskieren Sie persönlich identifizierbare Informationen aus Text in einer Anreicherungspipeline in Azure Cognitive Search. Diese Qualifikation ist zurzeit als öffentliche Vorschauversion verfügbar.
manager: nitinme
author: careyjmac
ms.author: chalton
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 7011d971ddb1888b312173a42ecd4a50f0954395
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76851109"
---
#   <a name="pii-detection-cognitive-skill"></a>Die kognitive Qualifikation „PII-Erkennung“

> [!IMPORTANT] 
> Diese Qualifikation ist zurzeit als öffentliche Vorschauversion verfügbar. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Derzeit werden weder das Portal noch das .NET SDK unterstützt.

Die Qualifikation **PII-Erkennung** extrahiert persönlich identifizierbare Informationen aus einem Eingabetext und bietet Ihnen die Möglichkeit, diese auf verschiedene Weise zu maskieren. Bei dieser Qualifikation werden die Machine Learning-Modelle verwendet, die in Cognitive Services über die [Textanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) bereitgestellt werden.

> [!NOTE]
> Wenn Sie den Umfang erweitern, indem Sie die Verarbeitungsfrequenz erhöhen oder weitere Dokumente oder KI-Algorithmen hinzufügen, müssen Sie [eine kostenpflichtige Cognitive Services-Ressource anfügen](cognitive-search-attach-cognitive-services.md). Gebühren fallen beim Aufrufen von APIs in Cognitive Services sowie für die Bildextraktion im Rahmen der Dokumententschlüsselungsphase in Azure Cognitive Search an. Für die Textextraktion aus Dokumenten fallen keine Gebühren an.
>
> Die Ausführung integrierter Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion sind in der [Preisübersicht für Azure Cognitive Search](https://go.microsoft.com/fwlink/?linkid=2042400) angegeben.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.PIIDetectionSkill

## <a name="data-limits"></a>Datengrenzwerte
Die maximale Größe eines Datensatzes beträgt 50.000 Zeichen (gemessen durch [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)). Wenn Sie Ihre Daten teilen müssen, bevor Sie sie an die Qualifikation senden, denken Sie daran, die [Qualifikation „Text teilen“](cognitive-search-skill-textsplit.md) zu verwenden.

## <a name="skill-parameters"></a>Skillparameter

Bei den Parametern, die alle optional sind, wird die Groß-/Kleinschreibung beachtet.

| Parametername     | Beschreibung |
|--------------------|-------------|
| defaultLanguageCode | Sprachcode des Eingabetexts. Derzeit wird nur `en` unterstützt. |
| minimumPrecision | Ein Wert zwischen 0,0 und 1,0. Wenn die Zuverlässigkeitsbewertung (in der `piiEntities`-Ausgabe) unter dem festgelegten `minimumPrecision`-Wert liegt, wird die Entität nicht zurückgegeben oder maskiert. Der Standard ist 0,0. |
| maskingMode | Ein Parameter, der verschiedene Methoden zum Maskieren der erkannten PII im Eingabetext bereitstellt. Die folgenden Optionen werden unterstützt: <ul><li>`none` (Standard): Dies bedeutet, dass keine Maskierung erfolgt und die `maskedText`-Ausgabe nicht zurückgegeben wird. </li><li> `redact`: Mit dieser Option werden die erkannten Entitäten aus dem Eingabetext entfernt und nicht durch andere Inhalte ersetzt. Beachten Sie, dass sich der Offset in der `piiEntities`-Ausgabe in diesem Fall auf den ursprünglichen Text und nicht auf den maskierten Text bezieht. </li><li> `replace`: Mit dieser Option werden die erkannten Entitäten durch das im `maskingCharacter`-Parameter angegebene Zeichen ersetzt.  Das Zeichen wird auf die Länge der erkannten Entität wiederholt, sodass die Offsets ordnungsgemäß sowohl dem Eingabetext als auch dem ausgegebenen `maskedText` entsprechen.</li></ul> |
| maskingCharacter | Das Zeichen, das zur Maskierung des Texts verwendet wird, wenn der Parameter `maskingMode` auf `replace` festgelegt ist. Die folgenden Optionen werden unterstützt: `*` (Standard), `#`, `X`. Dieser Parameter kann nur `null` sein, wenn `maskingMode` nicht auf `replace` festgelegt ist. |


## <a name="skill-inputs"></a>Skilleingaben

| Eingabename      | Beschreibung                   |
|---------------|-------------------------------|
| languageCode  | Optional. Der Standardwert ist `en`.  |
| text          | Der zu analysierende Text          |

## <a name="skill-outputs"></a>Skillausgaben

| Ausgabename     | Beschreibung                   |
|---------------|-------------------------------|
| piiEntities | Ein Array mit komplexen Typen und den folgenden Feldern: <ul><li>Text (die tatsächlich extrahierten PII)</li> <li>type</li><li>subType</li><li>Bewertung (ein höherer Wert bedeutet, dass es sich mit höherer Wahrscheinlichkeit um eine echte Entität handelt)</li><li>Offset (in den Eingabetext)</li><li>length</li></ul> </br> [Mögliche Typen und Untertypen finden Sie hier.](https://docs.microsoft.com/azure/cognitive-services/text-analytics/named-entity-types?tabs=personal) |
| maskedText | Ist `maskingMode` auf einen anderen Wert als `none` festgelegt, ist diese Ausgabe ist das Zeichenfolgenergebnis der für den Eingabetext durchgeführten Maskierung, wie durch den gewählten `maskingMode` beschrieben.  Ist `maskingMode` auf `none` festgelegt, ist diese Ausgabe nicht vorhanden. |

##  <a name="sample-definition"></a>Beispieldefinition

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.PIIDetectionSkill",
    "defaultLanguageCode": "en",
    "minimumPrecision": 0.5,
    "maskingMode": "replace",
    "maskingCharacter": "*",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "piiEntities"
      },
      {
        "name": "maskedText"
      }
    ]
  }
```
##  <a name="sample-input"></a>Beispieleingabe

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Microsoft employee with ssn 859-98-0987 is using our awesome API's."
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Beispielausgabe

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "piiEntities":[ 
           { 
              "text":"859-98-0987",
              "type":"U.S. Social Security Number (SSN)",
              "subtype":"",
              "offset":28,
              "length":11,
              "score":0.65
           }
        ],
        "maskedText": "Microsoft employee with ssn *********** is using our awesome API's."
      }
    }
  ]
}
```


## <a name="error-and-warning-cases"></a>Fälle für Fehler und Warnungen
Wird der Sprachcode für das Dokument nicht unterstützt, wird eine Warnung zurückgegeben, und es werden keine Entitäten extrahiert.
Wenn Ihr Text leer ist, wird eine Warnung angezeigt.
Wenn Ihre Text mehr als 50.000 Zeichen umfasst, werden nur die ersten 50.000 Zeichen analysiert und eine Warnung ausgegeben.

Wenn die Qualifikation eine Warnung zurückgibt, kann die Ausgabe `maskedText` leer sein.  Das bedeutet, wenn Sie davon ausgehen, dass diese Ausgabe für die Eingabe in spätere Qualifikationen vorhanden ist, wird sie nicht wie beabsichtigt funktionieren. Beachten Sie dies beim Schreiben Ihrer Skillsetdefinition.

## <a name="see-also"></a>Weitere Informationen

+ [Integrierte Qualifikationen](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
