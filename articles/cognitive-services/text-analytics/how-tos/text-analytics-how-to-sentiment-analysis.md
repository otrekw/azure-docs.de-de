---
title: Ausführen von Standpunktanalysen mit der Textanalyse-REST-API
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Sie die Textanalyse-REST-API von Azure Cognitive Services verwenden können, um einen Standpunkt (Stimmung) in Text zu erkennen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 08/25/2020
ms.author: aahi
ms.openlocfilehash: a0557c3ccf6510ab3ee2ae29cbef1fc754473345
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88933017"
---
# <a name="how-to-detect-sentiment-using-the-text-analytics-api"></a>Gewusst wie: Standpunktermittlung mithilfe der Textanalyse-API

Die Standpunktanalysefunktion der Textanalyse-API wertet Text aus und gibt für jeden Satz Standpunktergebnisse und -bezeichnungen zurück. Dies ist nützlich für die Erkennung positiver und negativer Standpunkte z. B. in sozialen Medien, Kundenbewertungen, Diskussionsforen. Die von der API verwendeten KI-Modelle werden vom Dienst bereitgestellt. Sie müssen lediglich Inhalte für die Analyse senden.

Nachdem eine Anforderung zur Stimmungsanalyse gesendet wurde, gibt die API Stimmungsbezeichnungen (z. B. „negativ“, „neutral“ und „positiv“) und Zuverlässigkeitsbewertungen auf Satz- und Dokumentebene zurück.

Die Standpunktanalyse unterstützt eine Vielzahl von Sprachen – weitere befinden sich in der Vorschau. Weitere Informationen finden Sie unter [Unterstützte Sprachen](../text-analytics-supported-languages.md).

## <a name="sentiment-analysis-versions-and-features"></a>Versionen und Features der Standpunktanalyse

[!INCLUDE [v3 region availability](../includes/v3-region-availability.md)]

| Funktion                                   | Standpunktanalyse v3 | Standpunktanalyse v3.1 (Vorschauversion) |
|-------------------------------------------|-----------------------|-----------------------------------|
| Methoden für Einzel- und Batchabfragen    | X                     | X                                 |
| Zuverlässigkeitsbewertungen und Bezeichnungen             | X                     | X                                 |
| Linux-basierte [Docker-Container](text-analytics-how-to-install-containers.md) | X  |  |
| Opinion Mining                            |                       | X                                 |

### <a name="sentiment-scoring-and-labeling"></a>Zuverlässigkeitsbewertung und Bezeichnung

In Version 3 wendet die Standpunktanalyse Stimmungsbezeichnungen auf Texte an, die auf Satz- und Dokumentebene zurückgegeben werden, jeweils begleitet von einer Zuverlässigkeitsbewertung. 

Die Bezeichnungen sind *positiv*, *negativ* und *neutral*. Auf Dokumentebene kann auch die Stimmungsbezeichnung *gemischt* zurückgegeben werden. Die Stimmung des Dokuments wird unten bestimmt:

| Stimmung von Sätzen                                                                            | Zurückgegebene Dokumentbezeichnung |
|-----------------------------------------------------------------------------------------------|-------------------------|
| Mindestens ein `positive`-Satz ist im Dokument enthalten. Die restlichen Sätze sind `neutral`. | `positive`              |
| Mindestens ein `negative`-Satz ist im Dokument enthalten. Die restlichen Sätze sind `neutral`. | `negative`              |
| Mindestens ein `negative`- und mindestens ein `positive`-Satz ist im Dokument enthalten.    | `mixed`                 |
| Alle Sätze im Dokument sind `neutral`.                                                  | `neutral`               |

Zuverlässigkeitsbewertungen liegen zwischen 1 und 0. Werte, die näher an 1 liegen, weisen auf eine höhere Zuverlässigkeit der Bezeichnungsklassifizierung hin, während niedrigere Bewertungen eine geringere Zuverlässigkeit bedeuten. Für jedes Dokument oder jeden Satz werden die vorhergesagten Bewertungen, die den Bezeichnungen zugeordnet sind (positiv, negativ und neutral), bis zu 1 hinzugefügt.

### <a name="opinion-mining"></a>Opinion Mining

Opinion Mining ist ein Feature der Standpunktanalyse ab Version 3.1-preview.1. Dieses Feature wird in der Verarbeitung natürlicher Sprache (Natural Language Processing, NLP) auch als aspektbasierte Standpunktanalyse bezeichnet und bietet feiner abgestufte Informationen zu den Meinungen in Bezug auf Aspekte (z. B. Attribute von Produkten oder Dienstleistungen) in Texten.

Wenn ein Kunde beispielsweise Feedback zu einem Hotel wie „das Zimmer war toll, aber das Personal war unfreundlich“ hinterlässt, werden durch Opinion Mining Aspekte im Text mit den zugehörigen Meinungen und Standpunkten gesucht:

| Aspekt | Meinung    | Stimmung |
|--------|------------|-----------|
| room   | toll      | Positiv  |
| staff  | unfreundlich | Negativ  |

Um Opinion Mining in Ihre Ergebnisse einzubeziehen, müssen Sie in eine Anforderung zur Standpunktanalyse das Flag `opinionMining=true` einschließen. Die Ergebnisse des Opinion Mining sind in der Antwort der Standpunktanalyse enthalten.

## <a name="sending-a-rest-api-request"></a>Senden einer REST-API-Anforderung 

### <a name="preparation"></a>Vorbereitung

Die Standpunktanalyse liefert bessere Ergebnisse, wenn Sie ihr kleinere Textmengen zuführen. Bei der Schlüsselbegriffserkennung verhält es sich genau umgekehrt: Sie funktioniert besser, wenn sie für große Textblöcke durchgeführt wird. Um für beide Vorgänge optimale Ergebnisse zu erzielen, empfiehlt es sich ggf., die Eingaben entsprechend umzustrukturieren.

Sie benötigen JSON-Dokumente im folgenden Format: ID, Text und Sprache.

Ein Dokument darf maximal 5.120 Zeichen enthalten. Pro Sammlung können bis zu 1.000 Elemente (IDs) vorhanden sein. Die Sammlung wird im Hauptteil der Anforderung übermittelt.

## <a name="structure-the-request"></a>Strukturieren der Anforderung

Erstellen Sie eine POST-Anforderung. Um eine Anforderung schnell zu strukturieren und zu senden, können Sie in den folgenden Verweislinks [Postman](text-analytics-how-to-call-api.md) oder die **API-Testkonsole** verwenden. 

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

[Referenz zu Standpunktanalyse v3](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/Sentiment)

#### <a name="version-31-preview1"></a>[Version 3.1-preview.1](#tab/version-3-1)

[Referenz zu Standpunktanalyse v3.1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-1/operations/Sentiment)

---

### <a name="request-endpoints"></a>Anforderungsendpunkte

Legen Sie den HTTPS-Endpunkt für die Standpunktanalyse entweder mithilfe einer Textanalyseressource in Azure oder mithilfe eines instanziierten [Textanalysecontainers](text-analytics-how-to-install-containers.md) fest. Sie müssen die richtige URL für die Version einschließen, die Sie verwenden möchten. Beispiel:

> [!NOTE]
> Den Schlüssel und den Endpunkt für Ihre Textanalyseressource finden Sie im Azure-Portal. Sie befinden sich auf der Seite **Schnellstart** der Ressource unter **Ressourcenverwaltung**. 

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/sentiment`

#### <a name="version-31-preview1"></a>[Version 3.1-preview.1](#tab/version-3-1)

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.1/sentiment`

Um die Ergebnisse vom Opinion Mining zu erhalten, müssen Sie den Parameter `opinionMining=true` einschließen. Beispiel:

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.1/sentiment?opinionMining=true`

Dieser Parameter ist standardmäßig auf `false` festgelegt. 

---

Legen Sie einen Anforderungsheader fest, um Ihren Textanalyse-API-Schlüssel einzubeziehen. Geben Sie im Anforderungstext die JSON-Dokumentsammlung an, die Sie für diese Analyse vorbereitet haben.

### <a name="example-sentiment-analysis-request"></a>Beispiel einer Standpunktanalyseanforderung 

Das folgende Beispiel zeigt Inhalte, die Sie ggf. für die Standpunktanalyse übermitteln können: Das Anforderungsformat ist für beide Versionen identisch.
    
```json
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "The restaurant had great food and our waiter was friendly."
    }
  ]
}
```

### <a name="post-the-request"></a>Übermitteln der Anforderung

Die Analyse erfolgt, wenn die Anforderung eingeht. Informationen zur Größe und Anzahl von Anforderungen, die Sie pro Minute und Sekunde senden können, finden Sie in der Übersicht im Abschnitt [Datengrenzwerte](../overview.md#data-limits).

Die Textanalyse-API ist zustandslos. Auf Ihrem Konto werden keine Daten gespeichert, und die Ergebnisse werden sofort in der Antwort zurückgegeben.


### <a name="view-the-results"></a>Zeigen Sie die Ergebnisse an

Die Standpunktanalyse gibt eine Stimmungsbezeichnung und eine Zuverlässigkeitsbewertung für das gesamte Dokument und jeden Satz darin zurück. Werte, die näher an 1 liegen, weisen auf eine höhere Zuverlässigkeit der Bezeichnungsklassifizierung hin, während niedrigere Bewertungen eine geringere Zuverlässigkeit bedeuten. Ein Dokument kann mehrere Sätze enthalten, und die Zuverlässigkeitsbewertungen in jedem Dokument oder Satz ergeben addiert 1.

Die Ausgabe wird umgehend zurückgegeben. Sie können die Ergebnisse an eine Anwendung streamen, die JSON akzeptiert, oder die Ausgabe in einer Datei im lokalen System speichern. Importieren Sie dann die Ausgabe in eine Anwendung, mit der Sie die Daten sortieren, durchsuchen und bearbeiten können. Aufgrund der Unterstützung von Emojis und mehreren Sprachen enthält der Antworttext unter Umständen Textversätze. Weitere Informationen finden Sie unter [Textversätze in der Ausgabe der Textanalyse-API](../concepts/text-offsets.md).

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

### <a name="sentiment-analysis-v30-example-response"></a>Beispielantwort der Standpunktanalyse v3.0

Antworten von Standpunktanalyse v3 enthalten Stimmungsbezeichnungen und Standpunktergebnisse für jeden analysierten Satz und jedes analysierte Dokument.

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
                    "length": 58,
                    "text": "The restaurant had great food and our waiter was friendly."
                }
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2020-04-01"
}
```

#### <a name="version-31-preview1"></a>[Version 3.1-preview.1](#tab/version-3-1)

### <a name="sentiment-analysis-v31-example-response"></a>Beispielantwort der Standpunktanalyse v3.1

Die Standpunktanalyse v3.1 bietet Opinion Mining zusätzlich zum Antwortobjekt auf der Registerkarte **Version 3.0**. In der nachstehenden Antwort weist der Satz *Im Restaurant gab es großartiges Essen, und der Kellner war freundlich* zwei Aspekte auf: *Essen* und *Kellner*. Die `relations`-Eigenschaft jedes Aspekts enthält einen `ref`-Wert mit dem URI-Verweis auf die zugeordneten Objekte `documents`, `sentences` und `opinions`.

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
                    "length": 58,
                    "text": "The restaurant had great food and our waiter was friendly.",
                    "aspects": [
                        {
                            "sentiment": "positive",
                            "confidenceScores": {
                                "positive": 1.0,
                                "negative": 0.0
                            },
                            "offset": 25,
                            "length": 4,
                            "text": "food",
                            "relations": [
                                {
                                    "relationType": "opinion",
                                    "ref": "#/documents/0/sentences/0/opinions/0"
                                }
                            ]
                        },
                        {
                            "sentiment": "positive",
                            "confidenceScores": {
                                "positive": 1.0,
                                "negative": 0.0
                            },
                            "offset": 38,
                            "length": 6,
                            "text": "waiter",
                            "relations": [
                                {
                                    "relationType": "opinion",
                                    "ref": "#/documents/0/sentences/0/opinions/1"
                                }
                            ]
                        }
                    ],
                    "opinions": [
                        {
                            "sentiment": "positive",
                            "confidenceScores": {
                                "positive": 1.0,
                                "negative": 0.0
                            },
                            "offset": 19,
                            "length": 5,
                            "text": "great",
                            "isNegated": false
                        },
                        {
                            "sentiment": "positive",
                            "confidenceScores": {
                                "positive": 1.0,
                                "negative": 0.0
                            },
                            "offset": 49,
                            "length": 8,
                            "text": "friendly",
                            "isNegated": false
                        }
                    ]
                }
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2020-04-01"
}
```

---

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie sich mit Konzepten und dem Workflow für die Standpunktanalyse unter Verwendung der Textanalyse-API vertraut gemacht. Zusammenfassung:

+ Die Standpunktanalyse ist für ausgewählte Sprachen verfügbar.
+ JSON-Dokumente im Anforderungstext umfassen eine ID, Text und einen Sprachcode.
+ Die POST-Anforderung wird an einen Endpunkt vom Typ `/sentiment` gesendet. Dabei werden ein personalisierter [Zugriffsschlüssel und ein Endpunkt](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) verwendet, der für Ihr Abonnement gültig ist.
+ Bei der Antwortausgabe handelt es sich um eine Stimmungspunktzahl für die jeweilige Dokument-ID. Sie kann an eine beliebige JSON-fähige App gestreamt werden. Beispielsweise Excel und Power BI.

## <a name="see-also"></a>Weitere Informationen

* [Übersicht über die Textanalyse](../overview.md)
* [Verwenden der Textanalyse-Clientbibliothek](../quickstarts/text-analytics-sdk.md)
* [Neuigkeiten](../whats-new.md)
