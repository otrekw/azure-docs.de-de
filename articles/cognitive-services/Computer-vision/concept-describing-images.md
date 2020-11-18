---
title: 'Bildbeschreibungen: maschinelles Sehen'
titleSuffix: Azure Cognitive Services
description: Machen Sie sich mit Konzepten des Bildbeschreibungsfeatures der Maschinelles Sehen-API vertraut.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 7d2b87710b2590f9bcf511ec0cbd5637913fb5c4
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2020
ms.locfileid: "94538443"
---
# <a name="describe-images-with-human-readable-language"></a>Beschreiben von Bildern in lesbarer Sprache

Die Maschinelles Sehen-API kann ein Bild analysieren und einen für Menschen lesbaren Satz mit der Beschreibung des Inhalts generieren. Der Algorithmus gibt eigentlich mehrere Beschreibungen zurück, die auf unterschiedlichen visuellen Merkmalen basieren, wobei jede Beschreibung eine Zuverlässigkeitsbewertung erhält. Die endgültige Ausgabe ist eine Liste mit Beschreibungen, die absteigend nach Zuverlässigkeitsbewertung sortiert ist.

## <a name="image-description-example"></a>Beispiel zur Bildbeschreibung

Die folgende JSON-Antwort veranschaulicht, was das maschinelle Sehen bei der Beschreibung des Beispielbilds anhand seiner visuellen Merkmale zurückgibt.

![Ein Schwarzweißbild von Gebäuden in Manhattan](./Images/bw_buildings.png)

```json
{
    "description": {
        "tags": ["outdoor", "building", "photo", "city", "white", "black", "large", "sitting", "old", "water", "skyscraper", "many", "boat", "river", "group", "street", "people", "field", "tall", "bird", "standing"],
        "captions": [
            {
                "text": "a black and white photo of a city",
                "confidence": 0.95301952483304808
            },
            {
                "text": "a black and white photo of a large city",
                "confidence": 0.94085190563213816
            },
            {
                "text": "a large white building in a city",
                "confidence": 0.93108362931954824
            }
        ]
    },
    "requestId": "b20bfc83-fb25-4b8d-a3f8-b2a1f084b159",
    "metadata": {
        "height": 300,
        "width": 239,
        "format": "Jpeg"
    }
}
```

## <a name="use-the-api"></a>Verwenden der API

Das Bildbeschreibungsfeature ist Teil der [Bildanalyse-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f21b). Sie können diese API über ein natives SDK oder REST-Aufrufe aufrufen. Beziehen Sie `Description` in den Abfrageparameter **visualFeatures** ein. Nachdem Sie die vollständige JSON-Antwort erhalten haben, analysieren Sie einfach die Zeichenfolge auf den Inhalt im Abschnitt `"description"`.

* [Schnellstart: Maschinelles Sehen: .NET SDK](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Schnellstart: Analysieren eines Bilds (REST-API)](./quickstarts/csharp-analyze.md)

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den verwandten Konzepten [Taggen von Bildern](concept-tagging-images.md) und [Kategorisieren von Bildern](concept-categorizing-images.md) vertraut.
