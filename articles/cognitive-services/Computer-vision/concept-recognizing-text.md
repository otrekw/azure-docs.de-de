---
title: Optische Zeichenerkennung (OCR) – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zur optischen Zeichenerkennung (OCR) in Bildern und Dokumenten mit gedrucktem und handschriftlichem Text unter Verwendung der Maschinelles Sehen-API.
services: cognitive-services
author: msbbonsu
manager: netahw
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 06/23/2020
ms.author: t-bebon
ms.custom: seodec18
ms.openlocfilehash: 6bc118145bec30085c2d9fbf726c40a20b312430
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86207039"
---
# <a name="optical-character-recognition-ocr"></a>Optische Zeichenerkennung (OCR)

Die Maschinelles Sehen-API von Microsoft umfasst Funktionen zur optischen Zeichenerkennung (Optical Character Recognition, OCR), mit denen gedruckter oder handschriftlicher Text aus Bildern und PDF-Dokumenten extrahiert wird. Die OCR-APIs extrahieren Text aus analogen Dokumenten (Bilder, gescannte Dokumente) sowie aus digitalisierten Dokumenten. Sie können Text aus Bildern in der Praxis, z. B. Fotos von Kfz-Kennzeichen oder von Containern mit Seriennummern, sowie aus Dokumenten, z. B. Rechnungen, Finanzberichten, Artikeln usw., extrahieren. Die neue Lese-OCR-API ist als Teil des verwalteten Diensts in der Cloud oder lokal (Container) verfügbar. Außerdem werden virtuelle Netzwerke und private Endpunkte unterstützt, um die Compliance- und Datenschutzanforderungen Ihres Unternehmens zu erfüllen.

## <a name="read-api"></a>Lese-API 

Die [Lese-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) der Maschinelles Sehen-API ist die neueste OCR-Technologie von Microsoft, mit der sich gedruckter Text (in sieben Sprachen), handschriftlicher Text (nur Englisch), Ziffern und Währungssymbole aus Bildern und mehrseitigen PDF-Dokumenten extrahieren lassen. Die API ist für das Extrahieren von Text aus praxisnahen textlastigen Bildern und mehrseitigen PDF-Dokumenten mit gemischten Sprachen optimiert. Sie unterstützt die Erkennung von gedrucktem und handschriftlichem Text (nur Englisch) im selben Bild oder Dokument. Eine vollständige Liste finden Sie auf der Seite [Sprachunterstützung für maschinelles Sehen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr).

### <a name="how-ocr-works"></a>Funktionsweise der optischen Zeichenerkennung

Die [Lese-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005) unterstützt textlastige Dokumente mit bis zu 2.000 Seiten und wird daher asynchron ausgeführt. Im ersten Schritt wird der Lesevorgang aufgerufen. Beim Lesevorgang wird ein Bild oder PDF-Dokument als Eingabe erfasst und eine Vorgangs-ID zurückgegeben. 

Im zweiten Schritt wird der Vorgang zum [Abrufen des Ergebnisses](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d9869604be85dee480c8750) aufgerufen. Bei diesem Vorgang wird die Vorgangs-ID erfasst, die beim Lesevorgang erstellt wurde. Anschließend wird der extrahierte Textinhalt aus dem Bild oder Dokument im JSON-Format zurückgegeben. In der JSON-Antwort werden die ursprünglichen Zeilengruppierungen der erkannten Wörter beibehalten. Sie enthält die extrahierten Textzeilen und die zugehörigen Begrenzungsrahmenkoordinaten. Jede Textzeile enthält alle extrahierten Wörter mit den zugehörigen Koordinaten und Zuverlässigkeitsbewertungen.

Bei Bedarf wird beim Lesevorgang die Drehung der erkannten Seite korrigiert, indem der Drehversatz in Grad wie in der folgenden Abbildung gezeigt um die horizontale Bildachse gedreht wird.

![So konvertiert die optische Zeichenerkennung Bilder und Dokumente in eine strukturierte Ausgabe mit extrahiertem Text](./Images/how-ocr-works.svg)

### <a name="sample-ocr-output"></a>Beispielausgabe der optischen Zeichenerkennung

Es wird eine Erfolgsantwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt:

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

### <a name="input-requirements"></a>Eingabeanforderungen

Die Lese-API erfasst die folgenden Eingaben:
* Unterstützte Dateiformate: JPEG, PNG, BMP, PDF und TIFF
* In den Formaten PDF und TIFF werden bis zu 2.000 Seiten verarbeitet. Bei Abonnements im Free-Tarif werden nur die ersten beiden Seiten verarbeitet.
* Die Dateigröße muss weniger als 50 MB betragen und eine Größe von mindestens 50 × 50 Pixel und höchstens 10.000 × 10.000 Pixel aufweisen.
* Die Abmessungen bei PDF-Dateien dürfen maximal 17 × 17 Zoll betragen. Dies entspricht den Papierformaten Legal oder DIN A3 und kleineren Formaten.

### <a name="text-from-images"></a>Text in Bildern

Die folgende Ausgabe der Lese-API zeigt die extrahierten Textzeilen und Wörter aus einem Bild mit Text in unterschiedlichen Winkeln, Farben und Schriftarten.

![Ein Bild wird gedreht und der darauf befindliche Text gelesen und beschrieben](./Images/text-from-images-example.png)

### <a name="text-from-documents"></a>Text in Dokumenten

Die Lese-API erfasst zusätzlich zu den Bildern ein PDF-Dokument als Eingabe.

![Ein Bild wird gedreht und der darauf befindliche Text gelesen und beschrieben](./Images/text-from-documents-example.png)


### <a name="handwritten-text-in-english"></a>Handschriftlicher Text in englischer Sprache

Derzeit unterstützt der Lesevorgang das Extrahieren von handschriftlichem Text ausschließlich in englischer Sprache.

![Ein Bild wird gedreht und der darauf befindliche Text gelesen und beschrieben](./Images/handwritten-example.png)

### <a name="printed-text-in-supported-languages"></a>Gedruckter Text in unterstützten Sprachen

Mit der Lese-API kann gedruckter Text in englischer, spanischer, deutscher, französischer, italienischer, portugiesischer und niederländischer Sprache extrahiert werden. Wenn Sie für Ihr Szenario weitere Sprachen benötigen, finden Sie entsprechende Informationen in der Übersicht zur OCR-API im Artikel [Sprachunterstützung für maschinelles Sehen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr).

![Ein Bild wird gedreht und der darauf befindliche Text gelesen und beschrieben](./Images/supported-languages-example.png)

### <a name="mixed-languages-support"></a>Unterstützung für gemischte Sprachen

Die Lese-API unterstützt Bilder und Dokumente, die mehrere Sprachen enthalten. Diese werden im Allgemeinen als Dokumente mit gemischten Sprachen bezeichnet. Dabei werden die einzelnen Textzeilen in einem Dokument jeweils in der erkannten Sprache klassifiziert, bevor die Textinhalte extrahiert werden.

![Ein Bild wird gedreht und der darauf befindliche Text gelesen und beschrieben](./Images/mixed-language-example.png)

### <a name="data-privacy-and-security"></a>Datenschutz und Sicherheit

Wie bei allen Cognitive Services-Diensten müssen Entwickler, die den Lesedienst nutzen, die Microsoft-Richtlinien zu Kundendaten beachten. Weitere Informationen finden Sie im [Microsoft Trust Center](https://www.microsoft.com/en-us/trust-center/product-overview) auf der Seite zu Cognitive Services.

### <a name="deploy-on-premises"></a>Lokale Bereitstellung

Die Lese-API steht auch als Docker-Container (Vorschauversion) zur Verfügung, damit Sie die neuen OCR-Funktionen in Ihrer Umgebung bereitstellen können. Container eignen sich hervorragend für bestimmte Sicherheits- und Datengovernanceanforderungen. Informationen finden Sie unter [Installieren und Ausführen von Lesecontainern](https://docs.microsoft.com/azure/cognitive-services/computer-vision/computer-vision-how-to-install-containers).


## <a name="ocr-api"></a>OCR-API

Die [OCR-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) verwendet ein älteres Erkennungsmodell, unterstützt nur Bilder und wird synchron ausgeführt, sodass der erkannte Text direkt zurückgegeben wird. Informationen zur Lese-API finden Sie unter [Sprachunterstützung für maschinelles Sehen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Lese-REST-API 3.0](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-ga/operations/5d986960601faab4bf452005).
- Befolgen Sie die Anweisungen im Schnellstart zum [Extrahieren von Text](./QuickStarts/CSharp-hand-text.md), um die optische Zeichenerkennung mithilfe von C#, Java, JavaScript oder Python zusammen mit der REST-API zu implementieren.
