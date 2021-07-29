---
title: Der kognitive Skill „Entitätserkennung“ (V3)
titleSuffix: Azure Cognitive Search
description: Extrahieren verschiedener Entitätstypen aus Text mit Textanalyse V3 in einer Anreicherungspipeline in Azure Cognitive Search.
manager: jennmar
author: ayokande
ms.author: aakande
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/19/2021
ms.openlocfilehash: 9edb5a194f79d7b3fff1caf1672a656a85bd4766
ms.sourcegitcommit: 70ce9237435df04b03dd0f739f23d34930059fef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2021
ms.locfileid: "111528743"
---
#    <a name="entity-recognition-cognitive-skill-v3"></a>Der kognitive Skill „Entitätserkennung“ (V3)

> [!IMPORTANT] 
> Diese Qualifikation ist zurzeit als öffentliche Vorschauversion verfügbar. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Derzeit werden weder das Portal noch das .NET SDK unterstützt.

Mit der Qualifikation **Entitätserkennung** (EntityRecognitionSkill) können Sie Entitäten aus verschiedenen Arten von Text extrahieren. Diese Entitäten sind in 14 verschiedene Kategorien unterteilt: von Personen und Organisationen bis hin zu URLs und Telefonnummern. Bei dieser Qualifikation werden die Machine Learning-Modelle verwendet, die in Cognitive Services über die [Textanalyse](../cognitive-services/text-analytics/overview.md) bereitgestellt werden.

> [!NOTE]
> Wenn Sie den Umfang erweitern, indem Sie die Verarbeitungsfrequenz erhöhen oder weitere Dokumente oder KI-Algorithmen hinzufügen, müssen Sie [eine kostenpflichtige Cognitive Services-Ressource anfügen](cognitive-search-attach-cognitive-services.md). Gebühren fallen beim Aufrufen von APIs in Cognitive Services sowie für die Bildextraktion im Rahmen der Dokumententschlüsselungsphase in Azure Cognitive Search an. Für die Textextraktion aus Dokumenten fallen keine Gebühren an.
>
> Die Ausführung integrierter Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion sind in der [Preisübersicht für Azure Cognitive Search](https://azure.microsoft.com/pricing/details/search/) angegeben.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.V3.EntityRecognitionSkill

## <a name="data-limits"></a>Datengrenzwerte
Die maximale Größe eines Datensatzes beträgt 50.000 Zeichen (gemessen durch [`String.Length`](/dotnet/api/system.string.length)). Wenn Sie Ihre Daten aufteilen müssen, bevor Sie sie an den Skill „Entitätserkennung“ senden, denken Sie daran, den [Skill „Textaufteilung“](cognitive-search-skill-textsplit.md) zu verwenden.

## <a name="skill-parameters"></a>Skillparameter

Bei den Parametern, die alle optional sind, wird die Groß-/Kleinschreibung beachtet.

| Parametername     | BESCHREIBUNG |
|--------------------|-------------|
| `categories`    | Array von zu extrahierenden Kategorien.  Mögliche Kategorietypen: `"Person"`, `"Location"`, `"Organization"`, `"Quantity"`, `"DateTime"`, `"URL"`, `"Email"`, `"PersonType"`, `"Event"`, `"Product"`, `"Skill"`, `"Address"`, `"Phone Number"` und `"IP Address"`. Wenn keine Kategorie angegeben ist, werden alle Typen zurückgegeben.|
| `defaultLanguageCode` |    Sprachcode des Eingabetexts. Wenn kein Standardsprachcode festgelegt ist, wird Englisch (en) als Standardsprachcode verwendet. <br/> Siehe die [vollständige Liste der unterstützten Sprachen](../cognitive-services/text-analytics/language-support.md). Nicht alle Entitätskategorien werden für alle Sprachen unterstützt (siehe Anmerkung unten).|
| `minimumPrecision` | Ein Wert zwischen 0 und 1 ein. Wenn die Zuverlässigkeitsbewertung (in der `namedEntities`-Ausgabe) unter diesem Wert liegt, wird die Entität nicht zurückgegeben. Die Standardeinstellung ist 0. |
| `modelVersion` | (Optional) Die Version des Modells, die beim Aufruf des Textanalysediensts verwendet werden soll. Wenn nichts angegeben ist, wird standardmäßig die neueste verfügbare Version verwendet. Es empfiehlt sich, diesen Wert nur anzugeben, wenn es unbedingt notwendig ist. Weitere Einzelheiten finden Sie unter [Versionsverwaltung der Modelle in der Textanalyse-API](../cognitive-services/text-analytics/concepts/model-versioning.md).|


## <a name="skill-inputs"></a>Skilleingaben

| Eingabename      | BESCHREIBUNG                   |
|---------------|-------------------------------|
| `languageCode`    | Eine Zeichenfolge, die die Sprache der Datensätze angibt. Wenn dieser Parameter nicht angegeben ist, wird der Standardsprachcode zur Analyse der Datensätze verwendet. <br/>Siehe die [vollständige Liste der unterstützten Sprachen](../cognitive-services/text-analytics/language-support.md). |
| `text`          | Der zu analysierende Text          |

## <a name="skill-outputs"></a>Skillausgaben

> [!NOTE]
>Nicht alle Entitätskategorien werden für alle Sprachen unterstützt. Unter [Unterstützte Entitätskategorien in der Textanalyse-API v3](../cognitive-services/text-analytics/named-entity-types.md) erfahren Sie, welche Entitätskategorien für die Sprache unterstützt werden, die Sie verwenden.

| Ausgabename      | BESCHREIBUNG                   |
|---------------|-------------------------------|
| `persons`       | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge den Namen einer Person darstellt. |
| `locations`  | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge einen Ort darstellt. |
| `organizations`  | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge eine Organisation darstellt. |
| `quantities`  | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge eine Menge darstellt. |
| `dateTimes`  | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge einen DateTime-Wert darstellt (wie im Text gezeigt). |
| `urls` | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge eine URL darstellt. |
| `emails` | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge eine E-Mail-Adresse darstellt. |
| `personTypes` | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge einen Personentyp darstellt |
| `events` | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge ein Ereignis darstellt |
| `products` | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge ein Produkt darstellt |
| `skills` | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge einen Skill darstellt |
| `addresses` | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge eine Adresse darstellt |
| `phoneNumbers` | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge eine Telefonnummer darstellt |
| `ipAddresses` | Ein Array von Zeichenfolgen, wobei jede Zeichenfolge eine IP-Adresse darstellt |
| `namedEntities` | Ein Array mit komplexen Typen und den folgenden Feldern: <ul><li>category</li> <li>subcategory</li> <li>confidenceScore (ein höherer Wert für die Konfidenz bedeutet, dass es sich mit höherer Wahrscheinlichkeit um eine echte Entität handelt)</li> <li>length (die Länge (Anzahl von Zeichen) dieser Entität)</li> <li>offset (die Fundstelle im Text)</li> <li>text (der tatsächliche Entitätsname, wie er im Text angezeigt wird)</li></ul> |


##    <a name="sample-definition"></a>Beispieldefinition

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.V3.EntityRecognitionSkill",
    "context": "/document",
    "categories": [ "Person", "Email"],
    "defaultLanguageCode": "en", 
    "minimumPrecision": 0.5, 
    "inputs": [
        {
            "name": "text", 
            "source": "/document/content"
        },
        {
            "name": "languageCode", 
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "persons", 
            "targetName": "people"
        },
        {
            "name": "emails", 
            "targetName": "emails"
        },
        {
            "name": "namedEntities", 
            "targetName": "namedEntities"
        }
    ]
  }
```
##    <a name="sample-input"></a>Beispieleingabe

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Contoso Corporation was founded by Jean Martin. They can be reached at contact@contoso.com",
             "languageCode": "en"
           }
      }
    ]
}
```

##    <a name="sample-output"></a>Beispielausgabe

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "people": [ "Jean Martin"],
        "emails":["contact@contoso.com"],
        "namedEntities": 
        [
          {
            "category": "Person",
            "subcategory": null,
            "length": 11,
            "offset": 35,
            "confidenceScore": 0.98,
            "text": "Jean Martin"
          },
          {
            "category": "Email",
            "subcategory": null,
            "length": 19,
            "offset": 71,
            "confidenceScore": 0.8,
            "text": "contact@contoso.com"
          }
        ],
      }
    }
  ]
}
```

Beachten Sie, dass die für Entitäten in der Ausgabe dieses Skills zurückgegebenen Offsets direkt von der [Textanalyse-API](../cognitive-services/text-analytics/overview.md) zurückgegeben werden. Dies bedeutet, dass Sie, wenn Sie sie zum Indizieren in der ursprünglichen Zeichenfolge verwenden, die [StringInfo](/dotnet/api/system.globalization.stringinfo)-Klasse in .NET verwenden müssen, um den richtigen Inhalt zu extrahieren.  [Weitere Informationen finden Sie hier.](../cognitive-services/text-analytics/concepts/text-offsets.md)

## <a name="warning-cases"></a>Warnungsfälle
Wird der Sprachcode für das Dokument nicht unterstützt, wird eine Warnung zurückgegeben, und es werden keine Entitäten extrahiert.

## <a name="see-also"></a>Weitere Informationen

+ [Integrierte Qualifikationen](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)