---
title: Sprachunterstützung – maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: 'Dieser Artikel enthält eine Liste der natürlichen Sprachen, die von Features für maschinelles Sehen unterstützt werden: OCR, Bildanalyse.'
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.openlocfilehash: 649f1e6cf79aa17d053cc547debf5232120bb4fe
ms.sourcegitcommit: 5a37753456bc2e152c3cb765b90dc7815c27a0a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/04/2020
ms.locfileid: "87760633"
---
# <a name="language-support-for-computer-vision"></a>Sprachunterstützung für maschinelles Sehen

Einige Features des maschinellen Sehens unterstützen mehrere Sprachen; alle hier nicht erwähnten Features unterstützen nur Englisch.

## <a name="optical-character-recognition-ocr"></a>Optische Zeichenerkennung (OCR)

Die OCR-APIs für maschinelles Sehen unterstützen verschiedene Sprachen. Es muss kein Sprachcode angegeben werden. Weitere Informationen finden Sie unter [Optische Zeichenerkennung (OCR)](concept-recognizing-text.md).

|Sprache| Sprachcode | OCR-API | Read v3.0 | Read v3.1 Public Preview |
|:-----|:----:|:-----:|:---:|:---:|
|Arabisch | `ar`|✔ | | |
|Chinesisch (vereinfacht) | `zh-Hans`|✔ | |✔ |
|Chinesisch (traditionell) | `zh-Hant`|✔ | | |
|Tschechisch | `cs` |✔ | | |
|Dänisch | `da` |✔ | | |
|Niederländisch | `nl` |✔ |✔ |✔ |
|Englisch | `en` |✔ |✔ |✔ |
|Finnisch | `fi` |✔ | | |
|Französisch | `fr` |✔ |✔ |✔ |
|Deutsch | `de` |✔ |✔ |✔ |
|Griechisch | `el` |✔ | | |
|Ungarisch | `hu` |✔ | | |
|Italienisch | `it` |✔ |✔ |✔ |
|Japanisch | `ja` |✔ | | |
|Koreanisch | `ko` |✔ | | |
|Norwegisch | `nb` |✔ | | |
|Polnisch | `pl` |✔ | | |
|Portugiesisch | `pt` |✔ |✔ |✔ |
|Rumänisch | `ro` |✔ | | |
|Russisch | `ru` |✔ | | |
|Serbisch (Kyrillisch) | `sr-Cyrl` |✔ | | |
|Serbisch (Lateinisch) | `sr-Latn` |✔ | | |
|Slowakisch | `sk` |✔ | | |
|Spanisch | `es` |✔ |✔ |✔ |
|Schwedisch | `sw` |✔ | | |
|Türkisch | `tr` |✔ | | |

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
