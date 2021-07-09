---
title: Intelligent zugeschnittene Miniaturbilder – maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zum Generieren von Miniaturbildern für Bilder mithilfe der Maschinelles Sehen-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 03/11/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: bcfaf250aa5ebf29f51fae22f87c14c29416107e
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110465329"
---
# <a name="generating-smart-cropped-thumbnails-with-computer-vision"></a>Generieren intelligent zugeschnittener Miniaturbilder mit maschinellem Sehen

Ein Miniaturbild ist eine verkleinerte Darstellung eines Bilds. Miniaturbilder werden verwendet, um Bilder und andere Daten auf wirtschaftliche und layoutfreundliche Weise darzustellen. Die Maschinelles Sehen-API verwendet intelligente Funktionen zum Zuschneiden und Ändern der Größe von Bildern, um intuitive Miniaturbilder für ein bestimmtes Bild zu erstellen.

Der Algorithmus zum Generieren der Miniaturbilder der Maschinelles Sehen-API funktioniert wie folgt:

1. Entfernen störender Elemente aus dem Bild und Identifizieren des _relevanten Bereichs_&mdash; das ist der Bildbereich, in dem die wichtigsten Objekte enthalten sind.
1. Das Bild wird auf Grundlage des erkannten _relevanten Bereichs_ zugeschnitten.
1. Das Seitenverhältnis wird entsprechend den Abmessungen des Zielminiaturbilds geändert.

## <a name="area-of-interest"></a>Relevanter Bereich

Wenn Sie ein Bild hochladen, analysiert die Maschinelles Sehen-API dieses, um den *relevanten Bereich* zu ermitteln. Anhand dieser Region kann dann bestimmt werden, wie das Bild zugeschnitten werden muss. Das Zuschneiden wird jedoch immer gemäß dem gewünschten Seitenverhältnis durchgeführt, sofern dieses angegeben wurde.

Sie können die Koordinaten des unbearbeiteten umgebenden Felds dieses *relevanten Bereichs* stattdessen auch durch Aufrufen der **areaOfInterest**-API abrufen. Mithilfe dieser Informationen können Sie dann das ursprüngliche Bild ändern, wie Sie möchten.

## <a name="examples"></a>Beispiele

Das generierte Miniaturbild kann je nach den Angaben für Höhe, Breite und intelligenter Zuschneidefunktion stark variieren, wie im folgenden Bild dargestellt.

![Ein Bild eines Bergs neben verschiedenen Zuschnittkonfigurationen](./Images/thumbnail-demo.png)

In der folgenden Tabelle sind typische Miniaturbilder dargestellt, die vom maschinellen Sehen für die Beispielbilder generiert wurden. Die Miniaturbilder wurden für eine bestimmte Zielhöhe und -breite von 50 Pixel generiert, wobei die intelligente Zuschneidefunktion aktiviert ist.

| Image | Miniaturansicht |
|-------|-----------|
|![Berg bei Sonnenuntergang mit Silhouette einer Person](./Images/mountain_vista.png) | ![Miniaturansicht eines Bergs bei Sonnenuntergang mit Silhouette einer Person](./Images/mountain_vista_thumbnail.png) |
|![Eine weiße Blume vor einem grünen Hintergrund](./Images/flower.png) | ![Bildanalyse: Miniaturbild Flower](./Images/flower_thumbnail.png) |
|![Eine Frau auf dem Dach eines Wohnblocks](./Images/woman_roof.png) | ![Miniaturansicht einer Frau auf dem Dach eines Wohnhauses](./Images/woman_roof_thumbnail.png) |

## <a name="use-the-api"></a>Verwenden der API

Das Feature zum Generieren von Miniaturansichten ist über die APIs [Get Thumbnail](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2/operations/56f91f2e778daf14a499f20c) (Miniaturansicht abrufen) und [Get Area of Interest](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2/operations/b156d0f5e11e492d9f64418d) (Relevanten Bereich abrufen) verfügbar. Sie können diese API über ein natives SDK oder REST-Aufrufe aufrufen. 

* [Schnellstart: Verwenden der Clientbibliothek für maschinelles Sehen](./quickstarts-sdk/image-analysis-client-library.md?pivots=programming-language-csharp)
