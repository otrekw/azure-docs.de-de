---
title: Vordefinierte Ordinal-Entität – LUIS
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erhalten Sie Informationen zur vorgefertigten Ordinalentität in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/14/2019
ms.openlocfilehash: 1fa86c8960ea9f32163ebd7991260a19ef7a5d79
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "91535456"
---
# <a name="ordinal-prebuilt-entity-for-a-luis-app"></a>Vordefinierte Ordinal-Entität für eine LUIS-App
Eine Ordnungszahl ist eine numerische Darstellung eines Objekts in einer Menge: `first`, `second`, `third`. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit Ordnungszahlen hinzufügen. Die Ordinalentität wird in [vielen Kulturen](luis-reference-prebuilt-entities.md) unterstützt.

## <a name="types-of-ordinal"></a>Ordinaltypen
Die Entität „ordinal“ wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L45) verwaltet.

## <a name="resolution-for-prebuilt-ordinal-entity"></a>Auflösung der vorgefertigten Ordinalentität

Die folgenden Entitätsobjekte werden für die Abfrage zurückgegeben:

`Order the second option`

#### <a name="v3-response"></a>[V3-Antwort](#tab/V3)

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `false` festgelegt:

```json
"entities": {
    "ordinal": [
        2
    ]
}
```
#### <a name="v3-verbose-response"></a>[Ausführliche V3-Antwort](#tab/V3-verbose)
Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `true` festgelegt:

```json
"entities": {
    "ordinal": [
        2
    ],
    "$instance": {
        "ordinal": [
            {
                "type": "builtin.ordinal",
                "text": "second",
                "startIndex": 10,
                "length": 6,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

#### <a name="v2-response"></a>[V2-Antwort](#tab/V2)

Im folgenden Beispiel wird die Auflösung der Entität **builtin.ordinal** veranschaulicht.

```json
"entities": [
  {
    "entity": "second",
    "type": "builtin.ordinal",
    "startIndex": 10,
    "endIndex": 15,
    "resolution": {
      "value": "2"
    }
  }
]
```
* * *

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).

Erfahren Sie mehr über die Entitäten [OrdinalV2](luis-reference-prebuilt-ordinal-v2.md), [phone number](luis-reference-prebuilt-phonenumber.md) und [temperature](luis-reference-prebuilt-temperature.md).
