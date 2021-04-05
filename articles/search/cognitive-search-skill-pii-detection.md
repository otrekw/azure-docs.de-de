---
title: Die kognitive Qualifikation „PII-Erkennung“ (Vorschau)
titleSuffix: Azure Cognitive Search
description: Extrahieren und maskieren Sie persönliche Informationen aus Text in einer Anreicherungspipeline in Azure Cognitive Search. Diese Qualifikation ist zurzeit als öffentliche Vorschauversion verfügbar.
manager: nitinme
author: careyjmac
ms.author: chalton
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/17/2020
ms.openlocfilehash: acacf617d3f1d9ab891d08b32fc2dfb14deb64a4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91540522"
---
# <a name="pii-detection-cognitive-skill"></a>Die kognitive Qualifikation „PII-Erkennung“

> [!IMPORTANT] 
> Diese Qualifikation ist zurzeit als öffentliche Vorschauversion verfügbar. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Derzeit werden weder das Portal noch das .NET SDK unterstützt.

Die Qualifikation **PII-Erkennung** extrahiert persönliche Informationen aus einem Eingabetext und bietet Ihnen die Möglichkeit, sie zu maskieren. Bei dieser Qualifikation werden die Machine Learning-Modelle verwendet, die in Cognitive Services über die [Textanalyse](../cognitive-services/text-analytics/overview.md) bereitgestellt werden.

> [!NOTE]
> Wenn Sie den Umfang erweitern, indem Sie die Verarbeitungsfrequenz erhöhen oder weitere Dokumente oder KI-Algorithmen hinzufügen, müssen Sie [eine kostenpflichtige Cognitive Services-Ressource anfügen](cognitive-search-attach-cognitive-services.md). Gebühren fallen beim Aufrufen von APIs in Cognitive Services sowie für die Bildextraktion im Rahmen der Dokumententschlüsselungsphase in Azure Cognitive Search an. Für die Textextraktion aus Dokumenten fallen keine Gebühren an.
>
> Die Ausführung integrierter Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion sind in der [Preisübersicht für Azure Cognitive Search](https://azure.microsoft.com/pricing/details/search/) angegeben.

## <a name="odatatype"></a>@odata.type

Microsoft.Skills.Text.PIIDetectionSkill

## <a name="data-limits"></a>Datengrenzwerte

Die maximale Größe eines Datensatzes beträgt 50.000 Zeichen (gemessen durch [`String.Length`](/dotnet/api/system.string.length)). Wenn Sie Ihre Daten aufteilen müssen, bevor Sie sie an die Qualifikation senden, denken Sie daran, die [Qualifikation „Text teilen“](cognitive-search-skill-textsplit.md) zu verwenden.

## <a name="skill-parameters"></a>Skillparameter

Bei den Parametern, die alle optional sind, wird die Groß-/Kleinschreibung beachtet.

| Parametername     | BESCHREIBUNG |
|--------------------|-------------|
| `defaultLanguageCode` |    Sprachcode des Eingabetexts. Derzeit wird nur `en` unterstützt. |
| `minimumPrecision` | Ein Wert zwischen 0,0 und 1,0. Wenn die Zuverlässigkeitsbewertung (in der `piiEntities`-Ausgabe) unter dem festgelegten `minimumPrecision`-Wert liegt, wird die Entität nicht zurückgegeben oder maskiert. Der Standard ist 0,0. |
| `maskingMode` | Ein Parameter, der verschiedene Methoden bereitstellt, um die persönlichen Informationen zu maskieren, die im Eingabetext erkannt wurden. Die folgenden Optionen werden unterstützt: <ul><li>`none` (Standard): Es erfolgt keine Maskierung, und die `maskedText`-Ausgabe wird nicht zurückgegeben. </li><li> `redact`: Entfernt die erkannten Entitäten aus dem Eingabetext, ohne dass die gelöschten Werte ersetzt werden. In diesem Fall bezieht sich der Offset in der `piiEntities`-Ausgabe auf den ursprünglichen Text und nicht auf den maskierten Text. </li><li> `replace`: Ersetzt die erkannten Entitäten durch das im `maskingCharacter`-Parameter angegebene Zeichen. Das Zeichen wird auf die Länge der erkannten Entität wiederholt, sodass die Offsets ordnungsgemäß sowohl dem Eingabetext als auch dem ausgegebenen `maskedText` entsprechen.</li></ul> |
| `maskingCharacter` | Das Zeichen, das zum Maskieren des Texts verwendet wird, wenn der Parameter `maskingMode` auf `replace` festgelegt ist. Die folgenden Optionen werden unterstützt: `*` (Standard), `#`, `X`. Dieser Parameter kann nur `null` sein, wenn `maskingMode` nicht auf `replace` festgelegt ist. |

## <a name="skill-inputs"></a>Skilleingaben

| Eingabename      | BESCHREIBUNG                   |
|---------------|-------------------------------|
| `languageCode`    | Optional. Der Standardwert ist `en`.  |
| `text`          | Der zu analysierende Text          |

## <a name="skill-outputs"></a>Skillausgaben

| Ausgabename      | BESCHREIBUNG                   |
|---------------|-------------------------------|
| `piiEntities` | Ein Array mit komplexen Typen und den folgenden Feldern: <ul><li>Text (die tatsächlich extrahierten PII)</li> <li>type</li><li>subType</li><li>Bewertung (ein höherer Wert bedeutet, dass es sich mit höherer Wahrscheinlichkeit um eine echte Entität handelt)</li><li>Offset (in den Eingabetext)</li><li>length</li></ul> </br> [Mögliche Typen und Untertypen finden Sie hier.](../cognitive-services/text-analytics/named-entity-types.md?tabs=personal) |
| `maskedText` | Ist `maskingMode` auf einen anderen Wert als `none` festgelegt, ist diese Ausgabe ist das Zeichenfolgenergebnis der für den Eingabetext durchgeführten Maskierung, wie durch den gewählten `maskingMode` beschrieben.  Ist `maskingMode` auf `none` festgelegt, ist diese Ausgabe nicht vorhanden. |

## <a name="sample-definition"></a>Beispieldefinition

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

## <a name="sample-input"></a>Beispieleingabe

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

## <a name="sample-output"></a>Beispielausgabe

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

Die für Entitäten in der Ausgabe dieser Qualifikation zurückgegebenen Offsets werden direkt von der [Textanalyse-API](../cognitive-services/text-analytics/overview.md) zurückgegeben. Dies bedeutet, dass Sie, wenn Sie sie zum Indizieren in der ursprünglichen Zeichenfolge verwenden, die [StringInfo](/dotnet/api/system.globalization.stringinfo)-Klasse in .NET verwenden müssen, um den richtigen Inhalt zu extrahieren.  [Weitere Informationen finden Sie hier.](../cognitive-services/text-analytics/concepts/text-offsets.md)

## <a name="errors-and-warnings"></a>Fehler und Warnungen

Wird der Sprachcode für das Dokument nicht unterstützt, wird eine Warnung zurückgegeben, und es werden keine Entitäten extrahiert.
Wenn Ihr Text leer ist, wird eine Warnung zurückgegeben.
Wenn Ihre Text mehr als 50.000 Zeichen umfasst, werden nur die ersten 50.000 Zeichen analysiert und eine Warnung ausgegeben.

Wenn die Qualifikation eine Warnung zurückgibt, ist der ausgegebene `maskedText` möglicherweise leer, was sich auf alle nachfolgenden Qualifikationen auswirken kann, die die Ausgabe erwarten. Überprüfen Sie aus diesem Grund alle Warnungen, die es hinsichtlich fehlender Ausgabe gibt, wenn Sie eine Skillsetdefinition schreiben.

## <a name="see-also"></a>Weitere Informationen

+ [Integrierte Qualifikationen](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)