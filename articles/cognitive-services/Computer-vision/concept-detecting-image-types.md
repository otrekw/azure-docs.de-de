---
title: Erkennung von Bildtypen – maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Machen Sie sich mit Konzepten der Bildtyperkennung der Maschinelles Sehen-API vertraut.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 4e6c2db5333962d7ae43534998ffc1c48b0dba45
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "80244561"
---
# <a name="detecting-image-types-with-computer-vision"></a>Erkennen von Bildtypen mit maschinellem Sehen

Mit der [Analyze Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa)-API kann das maschinelle Sehen den Inhaltstyp von Bildern analysieren und gibt an, ob es sich bei einem Bild um eine ClipArt oder eine Linienzeichnung handelt.

## <a name="detecting-clip-art"></a>Erkennen von ClipArts

Das maschinelle Sehen analysiert ein Bild und bewertet die Wahrscheinlichkeit, dass es sich bei dem Bild um eine ClipArt handelt auf einer Skala von 0 bis 3, wie in der folgenden Tabelle beschrieben.

| Wert | Bedeutung |
|-------|---------|
| 0 | Keine ClipArt |
| 1 | Nicht eindeutig |
| 2 | Normale ClipArt |
| 3 | Gute ClipArt |

### <a name="clip-art-detection-examples"></a>Beispiele für die Erkennung von ClipArts

Die folgende JSON-Antwort veranschaulicht, was das maschinelle Sehen bei der Bewertung der Wahrscheinlichkeit, dass es sich bei den Beispielbildern um ClipArts handelt, zurückgibt.

![Ein ClipArt-Bild einer Käsescheibe](./Images/cheese_clipart.png)

```json
{
    "imageType": {
        "clipArtType": 3,
        "lineDrawingType": 0
    },
    "requestId": "88c48d8c-80f3-449f-878f-6947f3b35a27",
    "metadata": {
        "height": 225,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Ein blaues Haus mit Vorgarten](./Images/house_yard.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "a9c8490a-2740-4e04-923b-e8f4830d0e47",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="detecting-line-drawings"></a>Erkennen von Strichzeichnungen

Das maschinelle Sehen analysiert ein Bild und gibt einen booleschen Wert zurück, der angibt, ob es sich bei dem Bild um eine Strichzeichnung handelt.

### <a name="line-drawing-detection-examples"></a>Beispiele für die Erkennung von Strichzeichnungen

Die folgende JSON-Antwort veranschaulicht, was das maschinelle Sehen zurückgibt, um anzugeben, ob es sich bei den Beispielbildern um Strichzeichnungen handelt.

![Eine Strichzeichnung eines Löwen](./Images/lion_drawing.png)

```json
{
    "imageType": {
        "clipArtType": 2,
        "lineDrawingType": 1
    },
    "requestId": "6442dc22-476a-41c4-aa3d-9ceb15172f01",
    "metadata": {
        "height": 268,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Eine weiße Blume vor einem grünen Hintergrund](./Images/flower.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "98437d65-1b05-4ab7-b439-7098b5dfdcbf",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="use-the-api"></a>Verwenden der API

Die Funktion zur Erkennung von Bildtypen ist Teil der [Bildanalyse](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa)-API. Sie können diese API über ein natives SDK oder REST-Aufrufe aufrufen. Beziehen Sie `ImageType` in den Abfrageparameter **visualFeatures** ein. Nachdem Sie die vollständige JSON-Antwort erhalten haben, analysieren Sie einfach die Zeichenfolge auf den Inhalt im Abschnitt `"imageType"`.

* [Schnellstart: Maschinelles Sehen: .NET SDK](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Schnellstart: Analysieren eines Bilds (REST-API)](./quickstarts/csharp-analyze.md)
