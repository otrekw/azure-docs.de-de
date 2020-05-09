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
ms.date: 04/27/2020
ms.author: aahi
ms.openlocfilehash: 99a62daf6dced88efd9bda591a0ca44a8b259a75
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82195637"
---
# <a name="how-to-detect-sentiment-using-the-text-analytics-api"></a>Gewusst wie: Standpunktermittlung mithilfe der Textanalyse-API

Die Standpunktanalysefunktion der Textanalyse-API wertet Text aus und gibt für jeden Satz Standpunktergebnisse und -bezeichnungen zurück. Dies ist nützlich für die Erkennung positiver und negativer Standpunkte z. B. in sozialen Medien, Kundenbewertungen, Diskussionsforen. Die von der API verwendeten KI-Modelle werden vom Dienst bereitgestellt. Sie müssen lediglich Inhalte für die Analyse senden.

> [!TIP]
> Die Textanalyse bietet darüber hinaus ein Linux-basiertes Docker-Containerimage für die Spracherkennung, damit Sie [den Textanalysecontainer nah bei Ihren Daten installieren und ausführen können](text-analytics-how-to-install-containers.md).

Die Standpunktanalyse unterstützt eine Vielzahl von Sprachen – weitere befinden sich in der Vorschau. Weitere Informationen finden Sie unter [Unterstützte Sprachen](../text-analytics-supported-languages.md).

## <a name="concepts"></a>Konzepte

Die Textanalyse-API generiert mithilfe eines Machine Learning-Klassifizierungsalgorithmus ein Standpunktergebnis zwischen 0 und 1. Eine gegen 1 tendierende Punktzahl deutet auf einen positiven Standpunkt hin, eine gegen 0 tendierende Punktzahl auf einen negativen. Die Standpunktanalyse wird für das gesamte Dokument statt für einzelne Entitäten im Text ausgeführt. Dies bedeutet, dass Standpunktergebnisse auf Dokument- oder Satzebene zurückgegeben werden. 

Das verwendete Modell wurde vorab mit einem umfangreichen Korpus von Text- und Standpunktassoziationen trainiert. Es verwendet zur Analyse eine Kombination aus verschiedenen Verfahren. Hierzu zählen etwa Textverarbeitung, Wortartanalyse, Wortanordnung und Wortassoziationen. Weitere Informationen zum Algorithmus finden Sie unter [Introducing Text Analytics in the Azure ML Marketplace](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/) (Vorstellung der Textanalyse im Azure Machine Learning-Marketplace). Eigene Trainingsdaten können derzeit nicht bereitgestellt werden. 

Die Genauigkeit der Bewertung verbessert sich tendenziell, wenn Dokumente anstatt eines großen Textblocks weniger Sätze enthalten. Im Rahmen einer Objektivitätsbewertungsphase bestimmt das Modell, ob ein Dokument insgesamt objektiv ist oder einen Standpunkt enthält. Für ein überwiegend objektives Dokument wird keine Standpunkterkennungsphase gestartet: Es erhält die Punktzahl 0,50 und wird nicht weiter verarbeitet. Für Dokumente, die die Pipeline weiter durchlaufen, generiert die nächste Phase eine Bewertung über oder unter 0,50. Das Ergebnis hängt vom im Dokument erkannten Grad der Stimmung ab.

## <a name="sentiment-analysis-versions-and-features"></a>Versionen und Features der Standpunktanalyse

Der Textanalyse-API bietet zwei Versionen der Standpunktanalyse – v2 und v3. Standpunktanalyse v3 (Public Preview) bietet deutliche Verbesserungen hinsichtlich der Genauigkeit und Details der Textkategorisierung und -bewertung der API.

> [!NOTE]
> * Anforderungsformat und [Datenlimits](../overview.md#data-limits) der Standpunktanalyse v3 haben sich gegenüber der vorherigen Version nicht geändert.
> * Standpunktanalyse v3 in den folgenden Regionen verfügbar: `Australia East`, `Central Canada`, `Central US`, `East Asia`, `East US`, `East US 2`, `North Europe`, `Southeast Asia`, `South Central US`, `UK South`, `West Europe` und `West US 2`.

| Funktion                                   | Standpunktanalyse v2 | Standpunktanalyse v3 |
|-------------------------------------------|-----------------------|-----------------------|
| Methoden für Einzel- und Batchabfragen    | X                     | X                     |
| Standpunktergebnisse für das gesamte Dokument  | X                     | X                     |
| Standpunktergebnisse für einzelne Sätze |                       | X                     |
| Bezeichnung von Stimmungen                        |                       | X                     |
| Versionsverwaltung der Modelle                   |                       | X                     |

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

### <a name="sentiment-scoring"></a>Standpunktbewertung

Standpunktanalyse v3 klassifiziert Text mit Stimmungsbezeichnungen (siehe unten). Die zurückgegebenen Ergebnisse stellen das Vertrauen des Modells dar, dass der Text entweder positiv, negativ oder neutral ist. Höhere Werte deuten auf ein höheres Vertrauen hin. 

### <a name="sentiment-labeling"></a>Bezeichnung von Stimmungen

Standpunktanalyse v3 gibt Stimmungsbezeichnungen auf Satz- und Dokumentebene (`positive`, `negative` und `neutral`) sowie Zuverlässigkeitsbewertungen zurück. Die Stimmungsbezeichnung `mixed` kann ebenfalls auf Dokumentebene zurückgegeben werden. 

Die Stimmung des Dokuments wird unten bestimmt:

| Stimmung von Sätzen                                                                            | Zurückgegebene Dokumentbezeichnung |
|-----------------------------------------------------------------------------------------------|-------------------------|
| Mindestens ein `positive`-Satz ist im Dokument enthalten. Die restlichen Sätze sind `neutral`. | `positive`              |
| Mindestens ein `negative`-Satz ist im Dokument enthalten. Die restlichen Sätze sind `neutral`. | `negative`              |
| Mindestens ein `negative`- und mindestens ein `positive`-Satz ist im Dokument enthalten.    | `mixed`                 |
| Alle Sätze im Dokument sind `neutral`.                                                  | `neutral`               |

### <a name="model-versioning"></a>Versionsverwaltung der Modelle

> [!NOTE]
> Die Modellversionsverwaltung für die Stimmungsanalyse ist ab Version `v3.0-preview.1` verfügbar.

[!INCLUDE [v3-model-versioning](../includes/model-versioning.md)]

### <a name="example-c-code"></a>C#-Beispielcode

Eine C#-Beispielanwendung, die diese Version der Standpunktanalyse aufruft, finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/dotnet/Language/TextAnalyticsSentiment.cs).


#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

### <a name="sentiment-scoring"></a>Standpunktbewertung

Die Standpunktanalyse klassifiziert den Text als vorwiegend positiv oder negativ. Hierzu wird dem Text eine Punktzahl zwischen 0 und 1 zugewiesen. Gegen 0,5 tendierende Werte sind neutral oder ungewiss. Die Punktzahl 0,5 steht für Neutralität. Wenn für eine Zeichenfolge keine Standpunktanalyse möglich ist oder eine Zeichenfolge keinen Standpunkt beinhaltet, ist die Punktzahl immer genau 0,5. Wenn Sie also beispielsweise eine spanische Zeichenfolge mit einem Sprachcode für Englisch übergeben, ist die Punktzahl 0,5.

---

## <a name="sending-a-rest-api-request"></a>Senden einer REST-API-Anforderung 

### <a name="preparation"></a>Vorbereitung

Die Standpunktanalyse liefert bessere Ergebnisse, wenn Sie ihr kleinere Textmengen zuführen. Bei der Schlüsselbegriffserkennung verhält es sich genau umgekehrt: Sie funktioniert besser, wenn sie für große Textblöcke durchgeführt wird. Um für beide Vorgänge optimale Ergebnisse zu erzielen, empfiehlt es sich ggf., die Eingaben entsprechend umzustrukturieren.

Sie benötigen JSON-Dokumente im folgenden Format: ID, Text und Sprache.

Ein Dokument darf maximal 5.120 Zeichen enthalten. Pro Sammlung können bis zu 1.000 Elemente (IDs) vorhanden sein. Die Sammlung wird im Hauptteil der Anforderung übermittelt.

## <a name="structure-the-request"></a>Strukturieren der Anforderung

Erstellen Sie eine POST-Anforderung. Um eine Anforderung schnell zu strukturieren und zu senden, können Sie in den folgenden Verweislinks [Postman](text-analytics-how-to-call-api.md) oder die **API-Testkonsole** verwenden. 

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

[Referenz zu Standpunktanalyse v3](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/Sentiment)

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

[Referenz zu Standpunktanalyse v2](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9)

---

Legen Sie den HTTPS-Endpunkt für die Standpunktanalyse entweder mithilfe einer Textanalyseressource in Azure oder mithilfe eines instanziierten [Textanalysecontainers](text-analytics-how-to-install-containers.md) fest. Sie müssen die richtige URL für die Version einschließen, die Sie verwenden möchten. Beispiel:

> [!NOTE]
> Den Schlüssel und den Endpunkt für Ihre Textanalyseressource finden Sie im Azure-Portal. Sie befinden sich auf der Seite **Schnellstart** der Ressource unter **Ressourcenverwaltung**. 

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/sentiment`

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/sentiment`

---

Legen Sie einen Anforderungsheader fest, um Ihren Textanalyse-API-Schlüssel einzubeziehen. Geben Sie im Anforderungstext die JSON-Dokumentsammlung an, die Sie für diese Analyse vorbereitet haben.

### <a name="example-sentiment-analysis-request"></a>Beispiel einer Standpunktanalyseanforderung 

Das folgende Beispiel zeigt Inhalte, die Sie ggf. für die Standpunktanalyse übermitteln können: Das Anforderungsformat ist für beide Versionen der API identisch.
    
```json
{
    "documents": [
    {
        "language": "en",
        "id": "1",
        "text": "Hello world. This is some input text that I love."
    },
    {
        "language": "en",
        "id": "2",
        "text": "It's incredibly sunny outside! I'm so happy."
    }
    ],
}
```

### <a name="post-the-request"></a>Übermitteln der Anforderung

Die Analyse erfolgt, wenn die Anforderung eingeht. Informationen zur Größe und Anzahl von Anforderungen, die Sie pro Minute und Sekunde senden können, finden Sie in der Übersicht im Abschnitt [Datengrenzwerte](../overview.md#data-limits).

Die Textanalyse-API ist zustandslos. Auf Ihrem Konto werden keine Daten gespeichert, und die Ergebnisse werden sofort in der Antwort zurückgegeben.


### <a name="view-the-results"></a>Zeigen Sie die Ergebnisse an

Die Standpunktanalyse klassifiziert den Text als vorwiegend positiv oder negativ. Hierzu wird dem Text eine Punktzahl zwischen 0 und 1 zugewiesen. Gegen 0,5 tendierende Werte sind neutral oder ungewiss. Die Punktzahl 0,5 steht für Neutralität. Wenn für eine Zeichenfolge keine Standpunktanalyse möglich ist oder eine Zeichenfolge keinen Standpunkt beinhaltet, ist die Punktzahl immer genau 0,5. Wenn Sie also beispielsweise eine spanische Zeichenfolge mit einem Sprachcode für Englisch übergeben, ist die Punktzahl 0,5.

Die Ausgabe wird umgehend zurückgegeben. Sie können die Ergebnisse an eine Anwendung streamen, die JSON akzeptiert, oder die Ausgabe in einer Datei im lokalen System speichern. Importieren Sie dann die Ausgabe in eine Anwendung, mit der Sie die Daten sortieren, durchsuchen und bearbeiten können. Aufgrund der Unterstützung von Emojis und mehreren Sprachen enthält der Antworttext unter Umständen Textversätze. Weitere Informationen finden Sie unter [Textversätze in der Ausgabe der Textanalyse-API](../concepts/text-offsets.md).

#### <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

### <a name="sentiment-analysis-v3-example-response"></a>Beispielantwort der Standpunktanalyse v3

Antworten von Standpunktanalyse v3 enthalten Stimmungsbezeichnungen und Standpunktergebnisse für jeden analysierten Satz und jedes analysierte Dokument. `documentScores` wird nicht zurückgegeben, wenn die Stimmungsbezeichnung für das Dokument `mixed` ist.

```json
{
    "documents": [
        {
            "id": "1",
            "sentiment": "positive",
            "documentScores": {
                "positive": 0.98570585250854492,
                "neutral": 0.0001625834556762,
                "negative": 0.0141316400840878
            },
            "sentences": [
                {
                    "sentiment": "neutral",
                    "sentenceScores": {
                        "positive": 0.0785155147314072,
                        "neutral": 0.89702343940734863,
                        "negative": 0.0244610067456961
                    },
                    "offset": 0,
                    "length": 12
                },
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.98570585250854492,
                        "neutral": 0.0001625834556762,
                        "negative": 0.0141316400840878
                    },
                    "offset": 13,
                    "length": 36
                }
            ]
        },
        {
            "id": "2",
            "sentiment": "positive",
            "documentScores": {
                "positive": 0.89198976755142212,
                "neutral": 0.103382371366024,
                "negative": 0.0046278294175863
            },
            "sentences": [
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.78401315212249756,
                        "neutral": 0.2067587077617645,
                        "negative": 0.0092281140387058
                    },
                    "offset": 0,
                    "length": 30
                },
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.99996638298034668,
                        "neutral": 0.0000060341349126,
                        "negative": 0.0000275444017461
                    },
                    "offset": 31,
                    "length": 13
                }
            ]
        }
    ],
    "errors": []
}
```

#### <a name="version-21"></a>[Version 2.1](#tab/version-2)

### <a name="sentiment-analysis-v2-example-response"></a>Beispielantwort der Standpunktanalyse v2

Antworten von Standpunktanalyse v2 enthalten Standpunktergebnisse für jedes gesendete Dokument.

```json
{
  "documents": [{
    "id": "1",
    "score": 0.98690706491470337
  }, {
    "id": "2",
    "score": 0.95202046632766724
  }],
  "errors": []
}
```

---

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie sich mit Konzepten und dem Workflow für die Standpunktanalyse unter Verwendung der Textanalyse-API vertraut gemacht. Zusammenfassung:

+ Die Standpunktanalyse ist für ausgewählte Sprachen in zwei Versionen verfügbar.
+ JSON-Dokumente im Anforderungstext umfassen eine ID, Text und einen Sprachcode.
+ Die POST-Anforderung wird an einen Endpunkt vom Typ `/sentiment` gesendet. Dabei werden ein personalisierter [Zugriffsschlüssel und ein Endpunkt](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) verwendet, der für Ihr Abonnement gültig ist.
+ Bei der Antwortausgabe handelt es sich um eine Stimmungspunktzahl für die jeweilige Dokument-ID. Sie kann an eine beliebige JSON-fähige App gestreamt werden. Beispielsweise Excel und Power BI.

## <a name="see-also"></a>Weitere Informationen

* [Übersicht über die Textanalyse](../overview.md)
* [Verwenden der Textanalyse-Clientbibliothek](../quickstarts/text-analytics-sdk.md)
* [Neuigkeiten](../whats-new.md)
