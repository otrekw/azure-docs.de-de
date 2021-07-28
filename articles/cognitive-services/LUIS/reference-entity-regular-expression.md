---
title: Entität vom Typ „RegEx“ (Regulärer Ausdruck) – LUIS
description: Ein regulärer Ausdruck ist am besten für unformatierten Text von Äußerungen geeignet. Die Groß-/Kleinschreibung sowie die Kultur werden ignoriert.  Reguläre Ausdrücke werden nach Änderungen durch die Rechtschreibprüfung auf Zeichenebene (nicht auf Tokenebene) angewandt.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 05/05/2021
ms.openlocfilehash: e53d21cc4e28f6c388b4e4f980a3352021fc2bcb
ms.sourcegitcommit: 2cb7772f60599e065fff13fdecd795cce6500630
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108803402"
---
# <a name="regular-expression-entity"></a>Entität vom Typ „RegEx“

Eine RegEx-Entität extrahiert eine Entität anhand eines regulären Ausdrucks, den Sie bereitstellen.

Ein regulärer Ausdruck ist am besten für unformatierten Text von Äußerungen geeignet. Die Groß-/Kleinschreibung sowie die Kultur werden ignoriert.  Reguläre Ausdrücke werden nach Änderungen durch die Rechtschreibprüfung auf Tokenebene angewandt. Wenn der reguläre Ausdruck zu komplex ist (z.B. mit zu vielen Klammern), können Sie den Ausdruck dem Modell nicht hinzufügen. Es wird ein Teil der [.NET Regex](/dotnet/standard/base-types/regular-expressions)-Bibliothek verwendet, aber nicht die gesamte Bibliothek.

**Diese Entität ist gut geeignet, wenn Folgendes gilt**:

* Die Daten sind einheitlich formatiert, und auch bei den Abweichungen herrscht Einheitlichkeit.
* Für den regulären Ausdruck sind nicht mehr als zwei Schachtelungsebenen erforderlich.

![Entität vom Typ „RegEx“](./media/luis-concept-entities/regex-entity.png)

## <a name="example-json"></a>JSON-Beispiel

Wenn Sie `kb[0-9]{6}` als Definition der RegEx-Entität verwenden, stellt die folgende JSON-Antwort eine Beispieläußerung für die zurückgegebenen RegEx-Entitäten für die folgende Abfrage dar:

`When was kb123456 published?`:

#### <a name="v2-prediction-endpoint-response"></a>[V2 – Antwort für Vorhersageendpunkt](#tab/V2)

```JSON
"entities": [
  {
    "entity": "kb123456",
    "type": "KB number",
    "startIndex": 9,
    "endIndex": 16
  }
]
```


#### <a name="v3-prediction-endpoint-response"></a>[V3 – Antwort für Vorhersageendpunkt](#tab/V3)


Dies ist der JSON-Code, wenn `verbose=false` in der Abfragezeichenfolge festgelegt ist:

```json
"entities": {
    "KB number": [
        "kb123456"
    ]
}
```

Dies ist der JSON-Code, wenn `verbose=true` in der Abfragezeichenfolge festgelegt ist:

```json
"entities": {
    "KB number": [
        "kb123456"
    ],
    "$instance": {
        "KB number": [
            {
                "type": "KB number",
                "text": "kb123456",
                "startIndex": 9,
                "length": 8,
                "modelTypeId": 8,
                "modelType": "Regex Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * *

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Entitäten finden Sie hier:

* [Konzepte](luis-concept-entity-types.md)
* [Anleitung zum Erstellen](luis-how-to-add-entities.md)
