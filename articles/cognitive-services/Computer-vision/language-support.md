---
title: Sprachunterstützung – maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: 'Dieser Artikel enthält eine Liste der natürlichen Sprachen, die von Features für maschinelles Sehen unterstützt werden: OCR, Texterkennung und Lesen.'
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.openlocfilehash: 680e3647e7a5847e132a53986e2cf17150bceac3
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2020
ms.locfileid: "85560166"
---
# <a name="language-support-for-computer-vision"></a>Sprachunterstützung für maschinelles Sehen

Einige Features des maschinellen Sehens unterstützen mehrere Sprachen; alle hier nicht erwähnten Features unterstützen nur Englisch.

## <a name="text-detection-ocr"></a>Texterkennung (OCR)

Maschinelles Sehen kann Text in zahlreichen Sprachen erkennen. Bei Verwendung der Lese- und OCR-APIs muss kein Sprachcode angegeben werden. Unter [Lesen von Text aus Bildern und Dokumenten](concept-recognizing-text.md) finden Sie weitere Informationen zu dieser Funktionalität und den Vorteilen der einzelnen APIs.

|Sprache| Sprachcode | OCR-API | Lese-API |
|:-----|:----:|:-----:|:---:|
|Arabisch | `ar`|✔ | |
|Chinesisch (vereinfacht) | `zh-Hans`|✔ | |
|Chinesisch (traditionell) | `zh-Hant`|✔ | |
|Tschechisch | `cs` |✔ | |
|Dänisch | `da` |✔ | |
|Niederländisch | `nl` |✔ |✔ |
|Englisch | `en` |✔ |✔ |
|Finnisch | `fi` |✔ | |
|Französisch | `fr` |✔ |✔ |
|Deutsch | `de` |✔ |✔ |
|Griechisch | `el` |✔ | |
|Ungarisch | `hu` |✔ | |
|Italienisch | `it` |✔ |✔ |
|Japanisch | `ja` |✔ | |
|Koreanisch | `ko` |✔ | |
|Norwegisch | `nb` |✔ | |
|Polnisch | `pl` |✔ | |
|Portugiesisch | `pt` |✔ |✔ |
|Rumänisch | `ro` |✔ | |
|Russisch | `ru` |✔ | |
|Serbisch (Kyrillisch) | `sr-Cyrl` |✔ | |
|Serbisch (Lateinisch) | `sr-Latn` |✔ | |
|Slowakisch | `sk` |✔ | |
|Spanisch | `es` |✔ |✔ |
|Schwedisch | `sw` |✔ | |
|Türkisch | `tr` |✔ | |

## <a name="image-analysis"></a>Bildanalyse

Einige Aktionen der [Bildanalyse](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa)-API können Ergebnisse in anderen Sprachen zurückgeben, angegeben mit dem `language`-Abfrageparameter. Andere Aktionen geben unabhängig davon, welche Sprache angegeben ist, Ergebnisse in englischer Sprache zurück, und andere lösen bei nicht unterstützten Sprachen eine Ausnahme aus. Aktionen werden mit dem `visualFeatures`- und `details`-Abfrageparameter angegeben; in der [Übersicht](home.md) finden Sie eine Liste mit allen Aktionen, die Sie mit der Bildanalyse durchführen können.

|Sprache | Sprachcode | Kategorien | `Tags` | BESCHREIBUNG | Erwachsene | Marken | Color | Gesichtserkennung | ImageType | Objekte | Prominente | Besondere Merkmale |
|:---|:---:|:----:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Chinesisch | `zh`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Englisch | `en`   | ✔ | ✔| ✔|✔|✔|✔|✔|✔|✔|✔|✔|
|Japanisch | `ja`   | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Portugiesisch | `pt` | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Spanisch | `es`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|

## <a name="next-steps"></a>Nächste Schritte

Mit den in diesem Handbuch erwähnten Features für maschinelles Sehen können Sie die ersten Schritte ausführen.

* [Analysieren eines lokalen Bilds (REST)](./quickstarts/csharp-analyze.md)
* [Extrahieren von gedrucktem Text (REST)](./quickstarts/csharp-print-text.md)
