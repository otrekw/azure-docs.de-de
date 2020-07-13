---
title: 'Stimmungsanalyse: LUIS'
titleSuffix: Azure Cognitive Services
description: Wenn die Standpunktanalyse konfiguriert wurde, enthält die JSON-Antwort von LUIS eine Standpunktanalyse.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/01/2020
ms.author: diberry
ms.openlocfilehash: 2d15170e3785d8978b9cb21eae3b94b002f9172e
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85857175"
---
# <a name="sentiment-analysis"></a>Stimmungsanalyse
Wenn die Standpunktanalyse konfiguriert wurde, enthält die JSON-Antwort von LUIS eine Standpunktanalyse. Weitere Informationen zu Standpunktanalysen finden Sie in der Dokumentation zur [Textanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).

LUIS verwendet Textanalyse v2. 

## <a name="resolution-for-sentiment"></a>Lösung für Stimmung

Stimmungsdaten stellen eine Bewertung zwischen 1 und 0 dar, die eine eher positive (näher an 1) oder negative (näher an 0) Stimmung in den Daten angibt.

#### <a name="english-language"></a>[Englische Sprache](#tab/english)

Wenn die Kultur `en-us` ist, lautet die Antwort:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

#### <a name="other-languages"></a>[Weitere Sprachen](#tab/other-languages)

Für alle anderen Kulturen lautet die Antwort:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```
* * *

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).

