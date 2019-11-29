---
title: Durch maschinelles Lernen erworbener Entitätstyp – LUIS
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 11/11/2019
ms.author: diberry
ms.openlocfilehash: 2289e8ac7744a7b4cbee300e77efda89d29ee2f5
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74017207"
---
# <a name="machine-learned-entity"></a>Durch maschinelles Lernen erworbene Entität 



**Diese Entität ist gut geeignet, wenn für die Textdaten Folgendes gilt**:


![Entität vom Typ „List“](./media/luis-concept-entities/list-entity.png)

## <a name="example-json"></a>JSON-Beispiel

Angenommen, die App enthält die Liste `Cities`, die Variationen von Städtenamen einschließlich Ort des Flughafens (Sea-tac), Flughafencode (SEA), Postleitzahl (98101) und Vorwahl (206) ermöglicht.

|Listenelement|Elementsynonyme|
|---|---|
|`Seattle`|`sea-tac`, `sea`, `98101`, `206`, `+1` |
|`Paris`|`cdg`, `roissy`, `ory`, `75001`, `1`, `+33`|

`book 2 tickets to paris`

In der Äußerung oben wird das Wort `paris` dem Element Paris als Teil der Listenentität `Cities` zugeordnet. Die Listenentität findet Übereinstimmungen sowohl des normalisierten Elementnamens als auch der Synonyme des Elements.

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2 – Antwort für Vorhersageendpunkt](#tab/V2)

```JSON
  "entities": [
    {
      "entity": "paris",
      "type": "Cities",
      "startIndex": 18,
      "endIndex": 22,
      "resolution": {
        "values": [
          "Paris"
        ]
      }
    }
  ]
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3 – Antwort für Vorhersageendpunkt](#tab/V3)


Dies ist der JSON-Code, wenn `verbose=false` in der Abfragezeichenfolge festgelegt ist:

```json
"entities": {
    "Cities": [
        [
            "Paris"
        ]
    ]
}
```

Dies ist der JSON-Code, wenn `verbose=true` in der Abfragezeichenfolge festgelegt ist:

```json
"entities": {
    "Cities": [
        [
            "Paris"
        ]
    ],
    "$instance": {
        "Cities": [
            {
                "type": "Cities",
                "text": "paris",
                "startIndex": 18,
                "length": 5,
                "modelTypeId": 5,
                "modelType": "List Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * * 

|Datenobjekt|Name der Entität|Wert|
|--|--|--|
|Entität vom Typ „List“|`Cities`|`paris`|


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Entität vom Typ [List](reference-entity-list.md) und die Entität vom Typ [Regular Expression](reference-entity-regular-expression.md).