---
title: Der kognitive Skill „Spracherkennung“
titleSuffix: Azure Cognitive Search
description: Bewertet unstrukturierten Text und gibt für jeden Datensatz einen Sprachbezeichner mit einer Punktzahl zurück, die die Stärke der Analyse in einer KI-Anreicherungspipeline in der kognitiven Azure-Suche angibt.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/17/2020
ms.openlocfilehash: 078a9312a7ee1b3b0eafd000928ed74348a540c3
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102548052"
---
#   <a name="language-detection-cognitive-skill"></a>Der kognitive Skill „Spracherkennung“

Der Skill **Spracherkennung** erkennt die Sprache von Eingabetexten und meldet einen einzigen Sprachcode für jedes Dokument, das auf Anforderung gesendet wird. Der Sprachcode ist mit einem Wert kombiniert, der die Stärke der Analyse angibt. Bei dieser Qualifikation werden die Machine Learning-Modelle verwendet, die in Cognitive Services über die [Textanalyse](../cognitive-services/text-analytics/overview.md) bereitgestellt werden.

Diese Funktion ist besonders nützlich, wenn Sie die Sprache des Texts als Eingabe für andere Skills (z.B. den [Skill „Standpunktanalyse“](cognitive-search-skill-sentiment.md) oder den [Skill „Text teilen“](cognitive-search-skill-textsplit.md)) benötigen.

„Spracherkennung“ nutzt die Bibliotheken von Bing zur Verarbeitung natürlicher Sprache, wodurch die Anzahl von [unterstützten Sprachen und Regionen](../cognitive-services/text-analytics/language-support.md) überschritten wird, die für „Textanalyse“ aufgeführt sind. Die genaue Sprachenliste wird nicht veröffentlicht. Sie enthält aber alle verbreiteten Sprachen, plus Varianten, Dialekte sowie einige Regional- und Kultursprachen. Bei Inhalten in einer seltener verwendeten Sprache können Sie [die Sprachenerkennungs-API ausprobieren](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/Languages), um zu sehen, ob sie einen Code zurückgibt. Die Antwort bei Sprachen, die nicht erkannt werden können, lautet `(Unknown)`.

> [!NOTE]
> Wenn Sie den Umfang erweitern, indem Sie die Verarbeitungsfrequenz erhöhen oder weitere Dokumente oder KI-Algorithmen hinzufügen, müssen Sie [eine kostenpflichtige Cognitive Services-Ressource anfügen](cognitive-search-attach-cognitive-services.md). Gebühren fallen beim Aufrufen von APIs in Cognitive Services sowie für die Bildextraktion im Rahmen der Dokumententschlüsselungsphase in Azure Cognitive Search an. Für die Textextraktion aus Dokumenten fallen keine Gebühren an.
>
> Die Ausführung integrierter Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion sind in der [Preisübersicht für Azure Cognitive Search](https://azure.microsoft.com/pricing/details/search/) angegeben.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.LanguageDetectionSkill

## <a name="data-limits"></a>Datengrenzwerte
Die maximale Größe eines Datensatzes beträgt 50.000 Zeichen (gemessen durch [`String.Length`](/dotnet/api/system.string.length)). Wenn Sie Ihre Daten teilen müssen, bevor Sie sie an den Skill für Sprachenerkennung senden, können Sie den [Skill „Text teilen“](cognitive-search-skill-textsplit.md) verwenden.

## <a name="skill-parameters"></a>Skillparameter

Bei den Parametern wird zwischen Groß- und Kleinschreibung unterschieden.

| Eingaben | BESCHREIBUNG |
|---------------------|-------------|
| `defaultCountryHint` | (Optional) Es kann ein ISO 3166-1-Alpha-2-Ländercode aus zwei Buchstaben angegeben werden, der als Hinweis für das Spracherkennungsmodell verwendet wird, wenn es die Sprache nicht eindeutig zuordnen kann. Weitere Details finden Sie in der [Textanalyse-Dokumentation](../cognitive-services/text-analytics/how-tos/text-analytics-how-to-language-detection.md#ambiguous-content) zu diesem Thema. Insbesondere wird der Parameter `defaultCountryHint` mit Dokumenten verwendet, die die `countryHint`-Eingabe nicht explizit festlegen.  |
| `modelVersion`   | (Optional) Die Version des Modells, die beim Aufruf des Textanalysediensts verwendet werden soll. Wenn nichts angegeben ist, wird standardmäßig die neueste verfügbare Version verwendet. Es empfiehlt sich, diesen Wert nur anzugeben, wenn es unbedingt notwendig ist. Weitere Einzelheiten finden Sie unter [Versionsverwaltung der Modelle in der Textanalyse-API](../cognitive-services/text-analytics/concepts/model-versioning.md). |

## <a name="skill-inputs"></a>Skilleingaben

Bei den Parametern wird zwischen Groß- und Kleinschreibung unterschieden.

| Eingaben     | BESCHREIBUNG |
|--------------------|-------------|
| `text` | Der zu analysierende Text|
| `countryHint` | Ein ISO 3166-1-Alpha-2-Ländercode aus zwei Buchstaben, der als Hinweis für das Spracherkennungsmodell verwendet wird, wenn es die Sprache nicht eindeutig zuordnen kann. Weitere Details finden Sie in der [Textanalyse-Dokumentation](../cognitive-services/text-analytics/how-tos/text-analytics-how-to-language-detection.md#ambiguous-content) zu diesem Thema. |

## <a name="skill-outputs"></a>Skillausgaben

| Ausgabename    | BESCHREIBUNG |
|--------------------|-------------|
| `languageCode` | Der für die Sprache identifizierte ISO-6391-Sprachcode. Beispiel: „en“. |
| `languageName` | Der Name der Sprache. Beispiel: „Englisch“. |
| `score` | Ein Wert zwischen 0 und 1 ein. Die Wahrscheinlichkeit, dass die Sprache korrekt identifiziert ist. Der Wert kann kleiner als 1 sein, wenn im Satz verschiedene Sprachen kombiniert sind.  |

##  <a name="sample-definition"></a>Beispieldefinition

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      },
      {
        "name": "countryHint",
        "source": "/document/countryHint"
      }
    ],
    "outputs": [
      {
        "name": "languageCode",
        "targetName": "myLanguageCode"
      },
      {
        "name": "languageName",
        "targetName": "myLanguageName"
      },
      {
        "name": "score",
        "targetName": "myLanguageScore"
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
             "text": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. "
           }
      },
      {
        "recordId": "2",
        "data":
           {
             "text": "Estamos muy felices de estar con ustedes."
           }
      },
      {
        "recordId": "3",
        "data":
           {
             "text": "impossible",
             "countryHint": "fr"
           }
      }
    ]
```


##  <a name="sample-output"></a>Beispielausgabe

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
            {
              "languageCode": "en",
              "languageName": "English",
              "score": 1,
            }
      },
      {
        "recordId": "2",
        "data":
            {
              "languageCode": "es",
              "languageName": "Spanish",
              "score": 1,
            }
      },
      {
        "recordId": "3",
        "data":
            {
              "languageCode": "fr",
              "languageName": "French",
              "score": 1,
            }
      }
    ]
}
```

## <a name="see-also"></a>Weitere Informationen

+ [Integrierte Qualifikationen](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)