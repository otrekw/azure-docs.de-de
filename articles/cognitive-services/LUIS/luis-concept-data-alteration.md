---
title: 'Ändern von Daten: LUIS'
description: Erfahren Sie, wie Daten vor der Vorhersage in Language Understanding Intelligent Service (LUIS) geändert werden können.
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: b3b36351a64a4e1a0bd13d5785a4e0609a80901d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80292053"
---
# <a name="alter-utterance-data-before-or-during-prediction"></a>Ändern von Äußerungsdaten vor oder während der Vorhersage
LUIS bietet Möglichkeiten zum Bearbeiten einer Äußerung vor oder während der Vorhersage. Dazu gehören eine [Korrektur der Rechtschreibung](luis-tutorial-bing-spellcheck.md) und das Beheben von Zeitzonenproblemen für die vordefinierte Entität [datetimeV2](luis-reference-prebuilt-datetimev2.md).

## <a name="correct-spelling-errors-in-utterance"></a>Korrigieren von Rechtschreibfehlern in Äußerungen


### <a name="v3-runtime"></a>V3-Runtime

Führen Sie eine Vorverarbeitung von Text zwecks Rechtschreibkorrektur durch, bevor Sie die Äußerungen an LUIS senden. Verwenden Sie Beispieläußerungen mit richtiger Schreibung, um sicherzustellen, dass Sie die richtigen Vorhersagen erhalten.

Verwenden Sie die [Bing-Rechtschreibprüfung](../bing-spell-check/overview.md), um Text vor dem Senden an LUIS zu korrigieren.

### <a name="prior-to-v3-runtime"></a>Vor der V3-Runtime

LUIS verwendet die [Bing-Rechtschreibprüfungs-API V7](../Bing-Spell-Check/overview.md) um Rechtschreibfehler in Äußerungen zu korrigieren. LUIS benötigt den diesem Dienst zugewiesenen Schlüssel. Erstellen Sie den Schlüssel, und fügen Sie den Schlüssel dann als Parameter der Abfragezeichenfolge an den [Endpunkt](https://go.microsoft.com/fwlink/?linkid=2092356) hinzu.

Der Endpunkt benötigt zwei Parameter, damit Rechtschreibkorrekturen funktionieren:

|Parameter|value|
|--|--|
|`spellCheck`|boolean|
|`bing-spell-check-subscription-key`|[Bing-Rechtschreibprüfungs-API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) – Endpunktschlüssel|

Wenn die [Bing-Rechtschreibprüfungs-API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) einen Fehler erkennt, werden die ursprüngliche Äußerung und die korrigierte Äußerung zusammen mit Vorhersagen vom Endpunkt zurückgegeben.

#### <a name="v2-prediction-endpoint-response"></a>[V2 – Antwort für Vorhersageendpunkt](#tab/V2)

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 – Antwort für Vorhersageendpunkt](#tab/V3)

```JSON
{
    "query": "Book a flite to London?",
    "prediction": {
        "normalizedQuery": "book a flight to london?",
        "topIntent": "BookFlight",
        "intents": {
            "BookFlight": {
                "score": 0.780123
            }
        },
        "entities": {},
    }
}
```

* * *

### <a name="list-of-allowed-words"></a>Liste zulässiger Wörter
Die in LUIS verwendete Bing-Rechtschreibprüfungs-API unterstützt keine Liste mit Wörtern, die bei den Änderungen durch die Rechtschreibprüfung ignoriert werden sollen. Wenn Sie eine Liste für Wörter oder Akronyme benötigen, verarbeiten Sie die Äußerung im Client, bevor Sie die Äußerung zur Absichtsvorhersage an LUIS senden.

## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Ändern der Zeitzone von vordefinierten datetimeV2-Entitäten
Wenn eine LUIS-App die vordefinierte Entität [datetimeV2](luis-reference-prebuilt-datetimev2.md) verwendet, kann ein datetime-Wert in der Vorhersageantwort zurückgegeben werden. Der richtige datetime-Wert für die Rückgaben wird anhand der Zeitzone der Anforderung bestimmt. Wenn die Anforderung von einem Bot oder einer anderen zentralen Anwendung vor dem Empfang durch LUIS stammt, korrigieren Sie die von LUIS verwendete Zeitzone.

### <a name="endpoint-querystring-parameter"></a>QueryString-Parameter für den Endpunkt
Sie korrigieren die Zeitzone, indem Sie am [Endpunkt](https://go.microsoft.com/fwlink/?linkid=2092356) mithilfe des `timezoneOffset`-Parameters die Zeitzone des Benutzers hinzufügen. Der Wert von `timezoneOffset` sollte die positive oder negative Zahl in Minuten sein, um die die Zeit geändert werden soll.

|Parameter|value|
|--|--|
|`timezoneOffset`|Positive oder negative Zahl, in Minuten|

### <a name="daylight-savings-example"></a>Sommerzeitbeispiel
Wenn Sie den zurückgegebenen vordefinierten datetimeV2-Wert für die Anpassung an die Sommerzeit benötigen, sollten Sie den QueryString-Parameter `timezoneOffset` mit einem +/-Wert in Minuten für die Abfrage an den [Endpunkt](https://go.microsoft.com/fwlink/?linkid=2092356) verwenden.

#### <a name="v2-prediction-endpoint-request"></a>[V2 – Anforderung für Vorhersageendpunkt](#tab/V2)

Hinzufügen von 60 Minuten:

`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q=Turn the lights on?**timezoneOffset=60**&verbose={boolean}&spellCheck={boolean}&staging={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

Entfernen von 60 Minuten:

`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q=Turn the lights on?**timezoneOffset=-60**&verbose={boolean}&spellCheck={boolean}&staging={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

#### <a name="v3-prediction-endpoint-request"></a>[V3 – Anforderung für Vorhersageendpunkt](#tab/V3)

Hinzufügen von 60 Minuten:

`https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/{appId}/slots/production/predict?query=Turn the lights on?**timezoneOffset=60**&spellCheck={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

Entfernen von 60 Minuten:

`https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/{appId}/slots/production/predict?query=Turn the lights on?**timezoneOffset=-60**&spellCheck={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).

* * *

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>C#-Code zur Bestimmung des richtigen Werts für timezoneOffset
Der folgende C#-Code verwendet die [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo)-Methode der [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid#examples)-Klasse, um das richtige `timezoneOffset` basierend auf der Systemzeit zu bestimmen:

```csharp
// Get CST zone id
TimeZoneInfo targetZone = TimeZoneInfo.FindSystemTimeZoneById("Central Standard Time");

// Get local machine's value of Now
DateTime utcDatetime = DateTime.UtcNow;

// Get Central Standard Time value of Now
DateTime cstDatetime = TimeZoneInfo.ConvertTimeFromUtc(utcDatetime, targetZone);

// Find timezoneOffset
int timezoneOffset = (int)((cstDatetime - utcDatetime).TotalMinutes);
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Korrigieren Sie Rechtschreibfehler mithilfe dieses Tutorials.](luis-tutorial-bing-spellcheck.md)
