---
title: Aufrufen der Lese-API
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie die Lese-API aufrufen und ihr Verhalten im Detail konfigurieren.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 03/31/2021
ms.author: pafarley
ms.openlocfilehash: 54ec669984488d5fe429050faefa0ff2d2e8eb9b
ms.sourcegitcommit: b8995b7dafe6ee4b8c3c2b0c759b874dff74d96f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106287514"
---
# <a name="call-the-read-api"></a>Aufrufen der Lese-API

In dieser Anleitung erfahren Sie, wie Sie die Lese-API zum Extrahieren von Text aus Bildern aufrufen. Sie erfahren mehr über die verschiedenen Möglichkeiten für die Konfiguration dieser API entsprechend Ihren Anforderungen.

## <a name="submit-data-to-the-service"></a>Übermitteln von Daten an den Dienst

Beim [Read-Aufruf](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005) der Read-API wird ein Bild oder PDF-Dokument als Eingabe erfasst und asynchron Text extrahiert.

`https://{endpoint}/vision/v3.2-preview.3/read/analyze[?language][&pages][&readingOrder]`

Der Aufruf wird mit einem Antwortheaderfeld namens `Operation-Location` zurückgegeben. Der `Operation-Location`-Wert ist eine URL, die die Vorgangs-ID enthält, die im nächsten Schritt verwendet werden soll.

|Antwortheader| Beispielwert |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/vision/v3.1/read/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

> [!NOTE]
> **Abrechnung** 
>
> Die Seite [Preise für maschinelles Sehen](https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/) enthält den Tarif für Read. Jedes analysierte Bild oder jede analysierte Seite ist eine Transaktion. Wenn Sie den Vorgang mit einem PDF- oder TIFF-Dokument mit 100 Seiten aufrufen, wird dies vom Read-Vorgang als 100 Transaktionen gezählt und Ihnen werden 100 Transaktionen in Rechnung gestellt. Wenn Sie für den Vorgang 50 Aufrufe durchgeführt haben und jeder Aufruf ein Dokument mit 100 Seiten übermittelt hat, werden Ihnen 5000 Transaktionen (50 x 100) in Rechnung gestellt.

## <a name="determine-how-to-process-the-data"></a>Festlegen, wie die Daten verarbeitet werden sollen

### <a name="language-specification"></a>Sprachspezifikation

Der [Read](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005)-Aufruf weist einen optionalen Anforderungsparameter für die Sprache auf. Read unterstützt die automatische Sprachidentifikation und mehrsprachige Dokumente. Geben Sie also nur einen Sprachcode an, wenn Sie erzwingen möchten, dass das Dokument in dieser spezifischen Sprache verarbeitet wird.

### <a name="natural-reading-order-output-latin-languages-only"></a>Ausgabe der natürlichen Leserichtung (nur lateinische Sprachen)
Mit der [Read 3.2-API (Vorschauversion)](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005) können Sie die Reihenfolge angeben, in der die Textzeilen mit dem `readingOrder`-Abfrageparameter ausgegeben werden. Verwenden Sie `natural` für eine benutzerfreundlichere Ausgabe der Lesereihenfolge, wie im folgenden Beispiel gezeigt. Dieses Feature wird nur für lateinische Sprachen unterstützt.

:::image border type="content" source="../Images/ocr-reading-order-example.png" alt-text="Beispiel für OCR-Leserichtung":::



### <a name="select-pages-or-page-ranges-for-text-extraction"></a>Auswählen von Seiten oder Seitenbereichen für die Textextraktion
Verwenden Sie mit der [Read 3.2-API (Vorschauversion)](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005) bei großen mehrseitigen Dokumenten den Abfrageparameter `pages` zur Angabe von Seitenzahlen oder Seitenbereichen, um Text nur von diesen Seiten zu extrahieren. Das folgende Beispiel zeigt ein Dokument mit 10 Seiten, wobei für beide Fälle – alle Seiten (1-10) und ausgewählte Seiten (3-6) – Text extrahiert wurde.

:::image border type="content" source="../Images/ocr-select-pages.png" alt-text="Ausgabe ausgewählter Seiten":::

## <a name="get-results-from-the-service"></a>Abrufen von Ergebnissen aus dem Dienst

Der zweite Schritt umfasst das Aufrufen des Vorgangs [Get Read Results](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d9869604be85dee480c8750). Dieser Vorgang erfasst die Vorgangs-ID, die beim Read-Vorgang erstellt wurde, als Eingabe. 

`https://{endpoint}/vision/v3.2-preview.3/read/analyzeResults/{operationId}`

Er gibt eine JSON-Antwort zurück, die ein **Status**-Feld mit den folgenden möglichen Werten enthält. 

|Wert | Bedeutung |
|:-----|:----|
| `notStarted`| Der Vorgang wurde noch nicht gestartet. |
| `running`| Der Vorgang wird verarbeitet. |
| `failed`| Beim Vorgang ist ein Fehler aufgetreten. |
| `succeeded`| Der Vorgang war erfolgreich. |

Sie rufen diesen Vorgang iterativ auf, bis er mit dem Wert **succeeded** (erfolgreich) zurückgegeben wird. Verwenden Sie ein Intervall von 1 bis 2 Sekunden, um zu vermeiden, dass die Rate der Anforderungen pro Sekunde (RPS) überschritten wird.

> [!NOTE]
> Im kostenlosen Tarif wird die Anforderungsrate auf 20 Aufrufe pro Minute beschränkt. Im kostenpflichtige Tarif sind 10 Anforderungen pro Sekunde (RPS) zulässig, die auf Anforderung erhöht werden können. Verwenden Sie den Azure-Supportkanal oder Ihr Kontoteam, um eine höheren Anforderungswert pro Sekunde (RPS) anzufordern.

Wenn das Feld **Status** den Wert `succeeded` aufweist, enthält die JSON-Antwort den extrahierten Textinhalt aus Ihrem Bild oder Dokument. In der JSON-Antwort werden die ursprünglichen Zeilengruppierungen der erkannten Wörter beibehalten. Sie enthält die extrahierten Textzeilen und die zugehörigen Begrenzungsrahmenkoordinaten. Jede Textzeile enthält alle extrahierten Wörter mit den zugehörigen Koordinaten und Zuverlässigkeitsbewertungen.

> [!NOTE]
> Die an den Vorgang `Read` übermittelten Daten werden vorübergehend verschlüsselt, für eine kurze Dauer im Ruhezustand gespeichert und dann gelöscht. Dadurch können Ihre Anwendungen den extrahierten Text als Teil der Dienstantwort abrufen.

### <a name="sample-json-output"></a>JSON-Beispielausgabe

Eine erfolgreiche JSON-Antwort sieht in etwa wie folgendes Beispiel aus:

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-05-28T05:13:21Z",
  "lastUpdatedDateTime": "2020-05-28T05:13:22Z",
  "analyzeResult": {
    "version": "3.1.0",
    "readResults": [
      {
        "page": 1,
        "angle": 0.8551,
        "width": 2661,
        "height": 1901,
        "unit": "pixel",
        "lines": [
          {
            "boundingBox": [
              67,
              646,
              2582,
              713,
              2580,
              876,
              67,
              821
            ],
            "text": "The quick brown fox jumps",
            "words": [
              {
                "boundingBox": [
                  143,
                  650,
                  435,
                  661,
                  436,
                  823,
                  144,
                  824
                ],
                "text": "The",
                "confidence": 0.958
              }
            ]
          }
        ]
      }
    ]
  }
}
```

### <a name="handwritten-classification-for-text-lines-latin-languages-only"></a>Handschriftliche Klassifizierung für Textzeilen (nur lateinische Sprachen)
Die Antwort der [Read 3.2-API (Vorschauversion)](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005) umfasst die Klassifizierung, ob es sich bei den einzelnen Textzeilen um einen handschriftlichen Stil handelt, zusammen mit einer Zuverlässigkeitsbewertung. Dieses Feature wird nur für lateinische Sprachen unterstützt. Das folgende Beispiel zeigt die handschriftliche Klassifizierung für den Text im Bild.

:::image border type="content" source="../Images/ocr-handwriting-classification.png" alt-text="Beispiel für OCR-Handschriftklassifizierung":::

## <a name="next-steps"></a>Nächste Schritte

Um die REST-API zu verwenden, wechseln Sie zur [Referenz für die Lese-API](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005).