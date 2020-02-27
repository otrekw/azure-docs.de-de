---
title: Verwenden der Entitätserkennung mit der Textanalyse-API
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Identität einer im Text gefundenen Entität mit der Textanalyse-REST-API identifizieren und unterscheiden können.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 02/10/2020
ms.author: aahi
ms.openlocfilehash: 0622aca5579c64c6d840761abb151665af559eea
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2020
ms.locfileid: "77367637"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Verwenden der Erkennung benannter Entitäten in der Textanalyse

Mit der Textanalyse-API kann aus unstrukturiertem Text eine Liste mit eindeutig unterscheidbaren Entitäten und Links zu weiteren Informationen im Web zurückgegeben werden. Die API unterstützt sowohl die Erkennung benannter Entitäten (Named Entity Recognition, NER) als auch die Entitätsverknüpfung.

### <a name="entity-linking"></a>Entitätsverknüpfung

Die Entitätsverknüpfung ist die Möglichkeit, die Identität einer im Text gefundenen Entität zu identifizieren und eindeutig zu machen (um beispielsweise zu ermitteln, ob das Wort `Mars` den Planeten oder den römischen Kriegsgott bezeichnet). Um in diesem Prozess erkannte Entitäten im Text zu verknüpfen, ist eine Wissensdatenbank in der entsprechenden Sprache erforderlich. Die Entitätsverknüpfung verwendet [Wikipedia](https://www.wikipedia.org/) als diese Wissensdatenbank.


### <a name="named-entity-recognition-ner"></a>Erkennung benannter Entitäten (NER)

Die NER ist die Möglichkeit, unterschiedliche Entitäten im Text zu identifizieren und sie in vordefinierte Klassen oder Typen zu kategorisieren, z. B. Person, Standort, Ereignis, Produkt und Organisation.  

Ab Version 3 kann dieses Feature der Textanalyse-API auch persönliche und vertrauliche Datentypen identifizieren, z. B.: Telefonnummer, Sozialversicherungsnummer, E-Mail-Adresse und Bankkontonummer.  Die Identifizierung dieser Entitäten kann bei der Klassifizierung vertraulicher Dokumente und bei der Bearbeitung persönlicher Informationen helfen.

## <a name="named-entity-recognition-versions-and-features"></a>Versionen und Features der Erkennung benannter Entitäten

Der Textanalyse-API bietet zwei Versionen der Erkennung benannter Entitäten – v2 und v3. Version 3 (Öffentliche Vorschau) bietet mehr Details zu den Entitäten, die erkannt und kategorisiert werden können.

| Funktion                                                         | NER v2 | NER v3 |
|-----------------------------------------------------------------|--------|--------|
| Methoden für Einzel- und Batchabfragen                          | X      | X      |
| Grundlegende Entitätserkennung in verschiedenen Kategorien              | X      | X      |
| Erweiterte Klassifizierung für erkannte Entitäten                 |        | X      |
| Separate Endpunkte zum Senden von Entitätsverknüpfungs- und NER-Anforderungen. |        | X      |
| Versionsverwaltung der Modelle                                                |        | X      |

Weitere Informationen finden Sie unter [Sprachunterstützung](../language-support.md#sentiment-analysis-key-phrase-extraction-and-named-entity-recognition).


#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

### <a name="entity-types"></a>Entitätstypen

Named Entity Recognition v3 bietet erweiterte Erkennung über mehrere Typen hinweg. Aktuell kann NER v3 die folgenden Kategorien von Entitäten erkennen:

* Allgemein
* Personenbezogene Informationen 

Eine ausführliche Liste der unterstützten Entitäten und Sprachen finden Sie im Artikel [Unterstützte Entitätstypen für NER v3 ](../named-entity-types.md).

### <a name="request-endpoints"></a>Anforderungsendpunkte

Named Entity Recognition v3 verwendet separate Endpunkte für NER-Anforderungen und Entitätsverknüpfungsanforderungen. Verwenden Sie basierend auf Ihrer Anforderung ein URL-Format unten:

NER
* Allgemeine Entitäten: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/general`

* Personenbezogene Informationen – `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/pii`

Entitätsverknüpfung
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/linking`

### <a name="model-versioning"></a>Versionsverwaltung der Modelle

[!INCLUDE [v3-model-versioning](../includes/model-versioning.md)]

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

### <a name="entity-types"></a>Entitätstypen

> [!NOTE]
> Die Named Entity Recognition (NER) Version 2 unterstützt nur die folgenden Entitäten. NER v3 befindet sich in der öffentlichen Vorschauphase und erweitert die Anzahl und Tiefe der in Text erkannten Entitäten erheblich.   

| type  | SubType | Beispiel |
|:-----------   |:------------- |:---------|
| Person        | N/V\*         | „Jeff“, „Bill Gates“     |
| Location      | N/V\*         | „Redmond, Washington“, „Paris“  |
| Organization  | N/V\*         | „Microsoft“   |
| Menge      | Number        | „6“, „sechs“     |
| Menge      | Prozentwert    | „50 %“, „fünfzig Prozent“|
| Menge      | Ordinal       | „2.“, „zweite“     |
| Menge      | Age           | „90 Tage alt“, „30 Jahre alt“    |
| Menge      | Währung      | „€10,99“     |
| Menge      | Dimension     | „10 Kilometer“, „40 cm“     |
| Menge      | Temperatur   | „32 Grad“    |
| Datetime      | N/V\*         | „18:30 4. Februar 2012“      |
| Datetime      | Date          | „2. Mai 2017“ und „02/05/2017“   |
| Datetime      | Time          | „8:00“, „8 Uhr“  |
| Datetime      | DateRange     | „2. Mai bis 5. Mai“    |
| Datetime      | TimeRange     | „18: 00 Uhr bis 19 Uhr“     |
| Datetime      | Duration      | „1 Minute und 45 Sekunden“   |
| Datetime      | Set           | „jeden Dienstag“     |
| URL           | N/V\*         | "https:\//www.bing.com"    |
| Email         | N/V\*         | "support@contoso.com" |
| US-Telefonnummer  | N/V\*         | (nur US-Telefonnummern) "(312) 555-0176" |
| IP-Adresse    | N/V\*         | "10.0.0.100" |

\* Je nach Eingabe und extrahierten Entitäten können bestimmte Entitäten den `SubType` auslassen.  Alle aufgelisteten unterstützten Entitätstypen sind nur für Englisch, Chinesisch (vereinfacht), Französisch, Deutsch und Spanisch verfügbar.

### <a name="request-endpoints"></a>Anforderungsendpunkte

Die Named Entity Recognition v2 verwendet separate Endpunkte für NER-Anforderungen und Entitätsverknüpfungsanforderungen.

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/entities`

---

## <a name="sending-a-rest-api-request"></a>Senden einer REST-API-Anforderung

### <a name="preparation"></a>Vorbereitung

Sie benötigen JSON-Dokumente im folgenden Format: ID, Text, Sprache.

Jedes Dokument darf jeweils maximal 5.120 Zeichen groß sein, und pro Sammlung sind bis zu 1.000 Elemente (IDs) zulässig. Die Sammlung wird im Hauptteil der Anforderung übermittelt.

### <a name="structure-the-request"></a>Strukturieren der Anforderung

Erstellen Sie eine POST-Anforderung. Um eine Anforderung schnell zu strukturieren und zu senden, können Sie in den folgenden Links [Postman](text-analytics-how-to-call-api.md) oder die **API-Testkonsole** verwenden. 

> [!NOTE]
> Den Schlüssel und den Endpunkt für Ihre Textanalyseressource finden Sie im Azure-Portal. Sie befinden sich auf der Seite **Schnellstart** der Ressource unter **Ressourcenverwaltung**. 

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

[Verweis für Named Entity Recognition (NER) v3](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionGeneral)

Version 3 verwendet separate Endpunkte für NER-Anforderungen und Entitätsverknüpfungsanforderungen. Verwenden Sie basierend auf Ihrer Anforderung ein URL-Format unten:

NER
* Allgemeine Entitäten: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/general`

* Entitäten für personenbezogene Informationen: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/pii`

Entitätsverknüpfung
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/linking`

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

[Verweis für Named Entity Recognition (NER) v2](https://eastus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

Version 2 verwendet den folgenden Endpunkt für die Entitätsverknüpfungs- und NER-Anforderungen: 

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/entities`

---

Legen Sie einen Anforderungsheader fest, um Ihren Textanalyse-API-Schlüssel einzubeziehen. Geben Sie im Anforderungstext die von Ihnen vorbereiteten JSON-Dokumente an.

### <a name="example-ner-request"></a>Beispiel für eine NER-Anforderung 

Das folgende Beispiel zeigt Inhalte, die Sie ggf. an die API senden können: Das Anforderungsformat ist für beide Versionen der API identisch.

```json
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "I had a wonderful trip to Seattle last week."
    }
  ]
}
```

## <a name="post-the-request"></a>Übermitteln der Anforderung

Die Analyse erfolgt, wenn die Anforderung eingeht. Weitere Informationen zur Größe und Anzahl von Anforderungen, die Sie pro Minute und Sekunde senden können, finden Sie in der Übersicht im Abschnitt [Datengrenzwerte](../overview.md#data-limits).

Die Textanalyse-API ist zustandslos. Auf Ihrem Konto werden keine Daten gespeichert, und die Ergebnisse werden sofort in der Antwort zurückgegeben.

## <a name="view-results"></a>Anzeigen der Ergebnisse

Alle POST-Anforderungen geben eine Antwort im JSON-Format mit den IDs und erkannten Entitätseigenschaften zurück.

Die Ausgabe wird umgehend zurückgegeben. Sie können die Ergebnisse an eine Anwendung streamen, die JSON akzeptiert, oder die Ausgabe in einer Datei auf dem lokalen System speichern und sie anschließend in eine Anwendung importieren, in der Sie die Daten sortieren, durchsuchen und bearbeiten können.


#### <a name="version-30-preview"></a>[Version 3.0-Preview](#tab/version-3)

### <a name="example-v3-responses"></a>Beispiel für eine Antwort von Version 3

Version 3 bietet separate Endpunkte für NER und Entitätsverknüpfung. Die Antworten für beide Vorgänge sind unten aufgeführt.

#### <a name="example-ner-response"></a>Beispiel für eine NER-Antwort

```json
{
    "documents": [{
    "id": "1",
    "entities": [{
        "text": "Seattle",
        "type": "Location",
        "offset": 26,
        "length": 7,
        "score": 0.80624294281005859
    }, {
        "text": "last week",
        "type": "DateTime",
        "subtype": "DateRange",
        "offset": 34,
        "length": 9,
        "score": 0.8
    }]
    }],
    "errors": [],
    "modelVersion": "2019-10-01"
}
```

#### <a name="example-entity-linking-response"></a>Beispiel für eine Antwort der Entitätsverknüpfung

```json
{
  "documents": [{
    "id": "1",
    "entities": [{
      "name": "Seattle",
      "matches": [{
        "text": "Seattle",
        "offset": 26,
        "length": 7,
        "score": 0.15046201222847677
      }],
      "language": "en",
      "id": "Seattle",
      "url": "https://en.wikipedia.org/wiki/Seattle",
      "dataSource": "Wikipedia"
    }]
  }],
  "errors": [],
  "modelVersion": "2019-10-01"
}
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

### <a name="example-ner-v2-response"></a>Beispiel für eine Antwort von NER v2
```json
{
  "documents": [{
    "id": "1",
    "entities": [{
      "name": "Seattle",
      "matches": [{
        "wikipediaScore": 0.15046201222847677,
        "entityTypeScore": 0.80624294281005859,
        "text": "Seattle",
        "offset": 26,
        "length": 7
      }],
      "wikipediaLanguage": "en",
      "wikipediaId": "Seattle",
      "wikipediaUrl": "https://en.wikipedia.org/wiki/Seattle",
      "bingId": "5fbba6b8-85e1-4d41-9444-d9055436e473",
      "type": "Location"
    }, {
      "name": "last week",
      "matches": [{
        "entityTypeScore": 0.8,
        "text": "last week",
        "offset": 34,
        "length": 9
      }],
      "type": "DateTime",
      "subType": "DateRange"
    }]
  }],
  "errors": []
}
```

---

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie sich mit Konzepten und mit dem Workflow für die Entitätsverknüpfung unter Verwendung der Textanalyse in Cognitive Services vertraut gemacht. Zusammenfassung:

* Die Named Entity Recognition ist für ausgewählte Sprachen in zwei Versionen verfügbar.
* JSON-Dokumente im Anforderungstext umfassen eine ID, Text und einen Sprachcode.
* POST-Anforderungen werden an einen oder mehrere Endpunkte gesendet. Dabei werden ein personalisierter [Zugriffsschlüssel und ein Endpunkt](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) verwendet, der für Ihr Abonnement gültig ist.
* Die Antwortausgabe, die aus verknüpften Entitäten besteht (z.B. Zuverlässigkeitsbewertungen, Offsets und Weblinks für jede Dokument-ID), kann in allen Anwendungen verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über die Textanalyse](../overview.md)
* [Verwenden der Textanalyse-Clientbibliothek](../quickstarts/text-analytics-sdk.md)
* [Neuigkeiten](../whats-new.md)
