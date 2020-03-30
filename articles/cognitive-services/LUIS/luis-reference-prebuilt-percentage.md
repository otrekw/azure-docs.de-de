---
title: 'Vordefinierte Percentage-Entität: LUIS'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erhalten Sie Informationen zur vorgefertigten Prozentsatzentität in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 31ea1c36139abcb1e102161ad76a203073ba4dfd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270510"
---
# <a name="percentage-prebuilt-entity-for-a-luis-app"></a>Vordefinierte Percentage-Entität für eine LUIS-App
Prozentzahlen können als Bruchzahlen (`3 1/2`) oder als Prozentzahlen (`2%`) angezeigt werden. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit Prozentsätzen hinzufügen. Die Prozentsatzentität wird in [vielen Kulturen](luis-reference-prebuilt-entities.md) unterstützt.

## <a name="types-of-percentage"></a>Prozentsatztypen
Die Entität „percentage“ wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L114) verwaltet.

## <a name="resolution-for-prebuilt-percentage-entity"></a>Auflösung der vorgefertigten Prozentsatzentität

Die folgenden Entitätsobjekte werden für die Abfrage zurückgegeben:

`set a trigger when my stock goes up 2%`

#### <a name="v3-response"></a>[V3-Antwort](#tab/V3)

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `false` festgelegt:

```json
"entities": {
    "percentage": [
        2
    ]
}
```
#### <a name="v3-verbose-response"></a>[Ausführliche V3-Antwort](#tab/V3-verbose)
Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `true` festgelegt:

```json
"entities": {
    "percentage": [
        2
    ],
    "$instance": {
        "percentage": [
            {
                "type": "builtin.percentage",
                "text": "2%",
                "startIndex": 36,
                "length": 2,
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

Im folgenden Beispiel wird die Auflösung der Entität **builtin.percentage** veranschaulicht.

```json
"entities": [
    {
        "entity": "2%",
        "type": "builtin.percentage",
        "startIndex": 36,
        "endIndex": 37,
        "resolution": {
        "value": "2%"
        }
    }
]
```
* * *

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).

Erfahren Sie mehr über die Entitäten [ordinal](luis-reference-prebuilt-ordinal.md), [number](luis-reference-prebuilt-number.md) und [temperature](luis-reference-prebuilt-temperature.md).
