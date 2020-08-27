---
title: Optische Zeichenerkennung (OCR) – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte, die im Zusammenhang mit optischen Zeichenerkennung (OCR) in Bildern und Dokumenten mit gedrucktem und handschriftlichem Text stehen unter Verwendung der API für maschinelles Sehen.
services: cognitive-services
author: PatrickFarley
manager: netahw
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/11/2020
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 9f9ebff77f54d86c3c4ed45fb5190de1900934e9
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88207218"
---
# <a name="optical-character-recognition-ocr"></a>Optische Zeichenerkennung (OCR)

Die API für maschinelles Sehen von Azure umfasst Funktionen zur optischen Zeichenerkennung (Optical Character Recognition, OCR), mit denen gedruckter oder handschriftlicher Text aus Bildern extrahiert wird. Sie können Text aus Bildern, z. B. Fotos von Kfz-Kennzeichen oder von Containern mit Seriennummern, sowie aus Dokumenten, z. B. Rechnungen, Finanzberichten, Artikeln usw., extrahieren. 

## <a name="read-api"></a>Lese-API 

Die [Read-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) für maschinelles Sehen ist die neueste OCR-Technologie von Azure, mit der sich gedruckter Text in mehreren Sprachen, handschriftlicher Text (nur Englisch), Ziffern und Währungssymbole aus Bildern und mehrseitigen PDF-Dokumenten extrahieren lassen. Die API ist für das Extrahieren von Text aus textlastigen Bildern und mehrseitigen PDF-Dokumenten mit gemischten Sprachen optimiert. Sie unterstützt sowohl die Erkennung von gedrucktem als auch von handschriftlichem Text im selben Bild oder Dokument.

![So konvertiert die optische Zeichenerkennung Bilder und Dokumente in eine strukturierte Ausgabe mit extrahiertem Text](./Images/how-ocr-works.svg)

## <a name="input-requirements"></a>Eingabeanforderungen
Der **Lesevorgang** der Lese-API akzeptiert Bilder und Dokumente als Eingabe. Es bestehen die folgenden Anforderungen:

* Unterstützte Dateiformate: JPEG, PNG, BMP, PDF und TIFF
* In den Formaten PDF und TIFF werden bis zu 2.000 Seiten verarbeitet. Bei Abonnements im Free-Tarif werden nur die ersten beiden Seiten verarbeitet.
* Die Dateigröße muss weniger als 50 MB betragen und eine Größe von mindestens 50 × 50 Pixel und höchstens 10.000 × 10.000 Pixel aufweisen.
* Die Abmessungen bei PDF-Dateien dürfen maximal 17 × 17 Zoll betragen. Dies entspricht den Papierformaten Legal oder DIN A3 und kleineren Formaten.

> [!NOTE]
> **Spracheneingabe** 
>
> Der [Read-Vorgang](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) weist einen optionalen Anforderungsparameter für die Sprache auf. Dies ist der BCP-47-Sprachcode des Texts im Dokument. Read unterstützt die automatische Sprachidentifikation und mehrsprachige Dokumente. Geben Sie also nur einen Sprachcode an, wenn Sie erzwingen möchten, dass das Dokument in dieser spezifischen Sprache verarbeitet wird.

## <a name="the-read-operation"></a>Der Read-Vorgang

Beim [Read-Vorgang](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) wird ein Bild oder PDF-Dokument als Eingabe erfasst und asynchron Text extrahiert. Der Aufruf wird mit einem Antwortheaderfeld namens `Operation-Location` zurückgegeben. Der `Operation-Location`-Wert ist eine URL, die die Vorgangs-ID enthält, die im nächsten Schritt verwendet werden soll.

|Antwortheader| Ergebnis-URL |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/vision/v3.0/read/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

## <a name="the-get-read-results-operation"></a>Der „Get Read Results“-Vorgang

Im zweiten Schritt wird der Vorgang [Get Read Results](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d9869604be85dee480c8750) aufgerufen. Dieser Vorgang erfasst die Vorgangs-ID, die beim Read-Vorgang erstellt wurde, als Eingabe. Er gibt eine JSON-Antwort zurück, die ein **Status**-Feld mit den folgenden möglichen Werten enthält. Sie rufen diesen Vorgang iterativ auf, bis er mit dem Wert **succeeded** (erfolgreich) zurückgegeben wird. Verwenden Sie ein Intervall von 1 bis 2 Sekunden, um zu vermeiden, dass die Rate der Anforderungen pro Sekunde (RPS) überschritten wird.

|Feld| type | Mögliche Werte |
|:-----|:----:|:----|
|status | Zeichenfolge | notStarted: Der Vorgang wurde noch nicht gestartet. |
| |  | running: Der Vorgang wird verarbeitet. |
| |  | failed: Beim Vorgang ist ein Fehler aufgetreten. |
| |  | succeeded: Der Vorgang war erfolgreich. |

> [!NOTE]
> Im kostenlosen Tarif wird die Anforderungsrate auf 20 Aufrufe pro Minute beschränkt. Im kostenpflichtige Tarif sind 10 Anforderungen pro Sekunde (RPS) zulässig, die auf Anforderung erhöht werden können. Verwenden Sie den Azure-Supportkanal oder Ihr Kontoteam, um eine höheren Anforderungswert pro Sekunde (RPS) anzufordern.

Wenn das Feld **Status** den Wert **succeeded** aufweist, enthält die JSON-Antwort den extrahierten Textinhalt aus Ihrem Bild oder Dokument. In der JSON-Antwort werden die ursprünglichen Zeilengruppierungen der erkannten Wörter beibehalten. Sie enthält die extrahierten Textzeilen und die zugehörigen Begrenzungsrahmenkoordinaten. Jede Textzeile enthält alle extrahierten Wörter mit den zugehörigen Koordinaten und Zuverlässigkeitsbewertungen.

### <a name="sample-json-output"></a>JSON-Beispielausgabe

Eine erfolgreiche JSON-Antwort sieht in etwa wie folgendes Beispiel aus:

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-05-28T05:13:21Z",
  "lastUpdatedDateTime": "2020-05-28T05:13:22Z",
  "analyzeResult": {
    "version": "3.0.0",
    "readResults": [
      {
        "page": 1,
        "language": "en",
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

Anweisungen zum Implementieren der optischen Zeichenerkennung mithilfe von C# und der REST-API finden Sie im Schnellstart zum [Extrahieren von gedrucktem und handschriftlichem Text](./QuickStarts/CSharp-hand-text.md).

## <a name="language-support"></a>Sprachunterstützung

### <a name="printed-text"></a>Gedruckter Text
Mit der [Read 3.0-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) kann gedruckter Text in englischer, spanischer, deutscher, französischer, italienischer, portugiesischer und niederländischer Sprache extrahiert werden. 

Die [Öffentliche Vorschau der Read 3.1-API](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-1/operations/5d986960601faab4bf452005) unterstützt ab sofort Chinesisch (vereinfacht). Wenn Sie für Ihr Szenario Unterstützung für weitere Sprachen benötigen, finden Sie entsprechende Informationen im Abschnitt [OCR-API](#ocr-api). 

Eine vollständige Liste der für OCR unterstützten Sprachen finden Sie unter [Unterstützte Sprachen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr).

### <a name="handwritten-text"></a>Handschriftlicher Text
Der Read-Vorgang unterstützt zurzeit das Extrahieren von handschriftlichem Text ausschließlich in englischer Sprache.

## <a name="integration-options"></a>Integrationsoptionen

### <a name="use-the-rest-api-or-client-sdk"></a>Verwenden von REST-API oder Client-SDK
Die [Read 3.x-REST-API](./QuickStarts/CSharp-hand-text.md) ist die bevorzugte Option der meisten Kunden, da sie sich einfach integrieren und schnell vorgefertigt produktiv einsetzen lässt. Azure und der Dienst für maschinelles Sehen verarbeiten Skalierungs-, Leistungs-, Datensicherheits- und Complianceanforderungen, während Sie sich auf die Erfüllung der Anforderungen Ihrer Kunden konzentrieren.

### <a name="use-containers-for-on-premise-deployment"></a>Verwenden von Containern für die lokale Bereitstellung
Der [Read 2.0-Docker-Container (Vorschau)](https://docs.microsoft.com/azure/cognitive-services/computer-vision/computer-vision-how-to-install-containers) ermöglicht Ihnen die Bereitstellung der neuen OCR-Funktionen in Ihrer eigenen, lokalen Umgebung. Container eignen sich hervorragend für bestimmte Sicherheits- und Datengovernanceanforderungen.

## <a name="read-ocr-examples"></a>Read-OCR-Beispiele

### <a name="text-from-images"></a>Text in Bildern

Die folgende Ausgabe der Read-API zeigt den aus einem Bild extrahierten Text in unterschiedlichen Textwinkeln, -farben und -schriftarten.

![Ein Bild mit mehreren Wörtern in unterschiedlichen Farben und Winkeln mit Auflistung des extrahierten Texts](./Images/text-from-images-example.png)

### <a name="text-from-documents"></a>Text in Dokumenten

Die Read-API akzeptiert auch PDF-Dokumente als Eingabe.

![Ein Rechnungsdokument mit Auflistung des extrahierten Texts](./Images/text-from-pdf-example.png)

### <a name="handwritten-text"></a>Handschriftlicher Text

Der Read-Vorgang extrahiert handschriftlichen Text aus Bildern (zurzeit nur in Englisch).

![Eine Abbildung einer handschriftlichen Notiz mit Auflistung des extrahierten Texts](./Images/handwritten-example.png)

### <a name="printed-text"></a>Gedruckter Text

Der Read-Vorgang kann gedruckten Text in verschiedenen Sprachen extrahieren.

![Eine Abbildung eines Spanisch-Lehrbuchs mit Auflistung des extrahierten Texts](./Images/supported-languages-example.png)

### <a name="mixed-language-documents"></a>Dokumente mit gemischten Sprachen

Die Read-API unterstützt Bilder und Dokumente, die mehrere verschiedene Sprachen enthalten, im Allgemeinen als Dokumente mit gemischten Sprachen bezeichnet. Dabei wird jede einzelne Textzeile in einem Dokument in die erkannte Sprache klassifiziert, bevor die Textinhalte extrahiert werden.

![Ein Abbildung von Ausdrücken in mehreren Sprachen mit Auflistung des extrahierten Texts](./Images/mixed-language-example.png)

## <a name="ocr-api"></a>OCR-API

Die [OCR-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) verwendet ein älteres Erkennungsmodell, unterstützt nur Bilder und wird synchron ausgeführt, sodass der erkannte Text direkt zurückgegeben wird. Informationen finden Sie unter [Für OCR unterstützte Sprachen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr) in der Read-API.

## <a name="data-privacy-and-security"></a>Datenschutz und Sicherheit

Wie bei allen Cognitive Services-Diensten müssen Entwickler, die Read/OCR-Dienste nutzen, die Microsoft-Richtlinien zu Kundendaten beachten. Weitere Informationen finden Sie im [Microsoft Trust Center](https://www.microsoft.com/trust-center/product-overview) auf der Seite zu Cognitive Services.

> [!NOTE]
> Die RecognizeText-Vorgänge der Maschinelles Sehen-API 2.0 werden allmählich zugunsten der neuen in diesem Artikel erläuterten Lese-API eingestellt. Bestehende Kunden sollten dazu [übergehen, Lesevorgänge zu nutzen](upgrade-api-versions.md).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Lese-REST-API 3.0](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005).
- Erfahren Sie mehr über die [Public Preview der Read 3.1-REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-1/operations/5d986960601faab4bf452005) und die Unterstützung von Chinesisch (vereinfacht).
- Befolgen Sie die Anweisungen im Schnellstart zum [Extrahieren von Text](./QuickStarts/CSharp-hand-text.md), um die optische Zeichenerkennung mithilfe von C#, Java, JavaScript oder Python zusammen mit der REST-API zu implementieren.
