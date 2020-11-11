---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/01/2020
ms.author: glenga
ms.openlocfilehash: 285c3bf37e9d6de042cb028745fc8b094d34c3a1
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2020
ms.locfileid: "93284393"
---
In Azure Functions auftretende Fehler können einen der folgenden Ursprünge haben:

- Verwendung integrierter Azure Functions-[Trigger und -Bindungen](..\articles\azure-functions\functions-triggers-bindings.md)
- Aufrufe an APIs zugrunde liegender Azure-Dienste
- Aufrufe an REST-Endpunkte
- Aufrufe an Clientbibliotheken, Pakete oder Drittanbieter-APIs

Die folgenden bewährten Methoden für die Fehlerbehandlung sind wichtig, um den Verlust von Daten oder verpasste Nachrichten zu vermeiden. Empfohlene Vorgehensweisen zur Fehlerbehandlung umfassen folgende Aktionen:

- [Application Insights aktivieren](../articles/azure-functions/functions-monitoring.md)
- [Verwenden strukturierter Fehlerbehandlung](#use-structured-error-handling)
- [Design für Idempotenz](../articles/azure-functions/functions-idempotent.md)
- [Implementieren von Wiederholungsrichtlinien](#retry-policies) (falls zutreffend)

### <a name="use-structured-error-handling"></a>Verwenden strukturierter Fehlerbehandlung

Das Erfassen und Protokollieren von Fehlern ist kritisch für die Überwachung der Integrität Ihrer Anwendung. Die oberste Ebene jedes Funktioncodes sollte einen try/catch-Block enthalten. Im catch-Block können Sie Fehler erfassen und protokollieren.

## <a name="retry-policies"></a>Wiederholungsrichtlinien

Eine Wiederholungsrichtlinie kann für beliebige Funktionen und beliebige Triggertypen in Ihrer Funktions-App definiert werden.  Die Wiederholungsrichtlinie führt eine Funktion so lange erneut aus, bis sie entweder erfolgreich ausgeführt oder die maximale Anzahl an Wiederholungen erreicht wurde.  Wiederholungsrichtlinien können für alle Funktionen in einer App oder für einzelne Funktionen definiert werden.  Standardmäßig werden Nachrichten von einer Funktions-App nicht wiederholt (eine Ausnahme bilden [spezielle Trigger mit eine Wiederholungsrichtlinie an der Triggerquelle](#trigger-specific-retry-support)).  Eine Wiederholungsrichtlinie wird immer dann ausgewertet, wenn eine Ausführung zu einer nicht abgefangenen Ausnahme führt.  Es hat sich bewährt, alle Ausnahmen in Ihrem Code abzufangen und alle Fehler erneut auszulösen, die zu einem Wiederholungsversuch führen sollten.  Event Hubs- und Azure Cosmos DB-Prüfpunkte werden erst geschrieben, wenn die Wiederholungsrichtlinie für die Ausführung abgeschlossen wurde. Das bedeutet, dass die Verarbeitung auf dieser Partition so lange angehalten wird, bis der aktuelle Batch abgeschlossen ist.

### <a name="retry-policy-options"></a>Optionen für Wiederholungsrichtlinien

Die folgenden Optionen sind für das Definieren einer Wiederholungsrichtlinie verfügbar.

**maxRetryCount** ist die maximale Anzahl von Wiederholungsversuchen für eine Ausführung, bis diese endgültig als fehlerhaft gilt. Der Wert `-1` bedeutet unbegrenzte Wiederholungen. Die aktuelle Wiederholungsanzahl wird im Arbeitsspeicher der Instanz gespeichert. Es kann vorkommen, dass bei einer Instanz zwischen Wiederholungsversuchen ein Fehler auftritt.  Wenn auf einer Instanz während einer Wiederholungsrichtlinie ein Fehler auftritt, geht die Zählung der Wiederholungsversuche verloren.  Bei Instanzfehlern können Trigger wie Event Hubs, Azure Cosmos DB und Queue Storage die Verarbeitung fortsetzen und den Batch auf einer neuen Instanz wiederholen. Dabei wird die Wiederholungsanzahl auf 0 (null) zurückgesetzt.  Andere Trigger, wie z. B. HTTP und Zeitgeber, werden nicht auf einer neuen Instanz fortgesetzt.  Dies bedeutet, dass die maximale Anzahl von Wiederholungsversuchen lediglich ein Ziel ist. In einigen seltenen Fällen kann eine Ausführung häufiger als der Maximalwert oder bei Triggern wie HTTP und Zeitgebern seltener als der Maximalwert wiederholt werden.

Die **Wiederholungsstrategie** steuert das Verhalten der Wiederholungen.  Im Folgenden finden Sie zwei unterstützte Wiederholungsoptionen:

| Option | Beschreibung|
|---|---|
|**`fixedDelay`**| Zwischen den einzelnen Wiederholungsversuchen kann eine angegebene Zeitspanne vergehen.|
| **`exponentialBackoff`**| Bei der ersten Wiederholung wird die minimale Verzögerung gewartet. Bei nachfolgenden Wiederholungen wird exponentiell Zeit zur anfänglichen Dauer addiert, bis die maximale Verzögerung erreicht ist.  Das exponentielle Backoff fügt den Verzögerungen einen geringfügigen Zufallswert hinzu, um Verzögerungen in Szenarien mit hohem Durchsatz zu staffeln.|

#### <a name="app-level-configuration"></a>Konfiguration auf App-Ebene

Eine Wiederholungsrichtlinie kann [mithilfe der Datei `host.json`](../articles/azure-functions/functions-host-json.md#retry) für alle Funktionen in einer App definiert werden. 

#### <a name="function-level-configuration"></a>Konfiguration auf Funktionsebene

Eine Wiederholungsrichtlinie kann für eine bestimmte Funktion definiert werden.  Die funktionsspezifische Konfiguration hat Vorrang vor der Konfiguration auf App-Ebene.

#### <a name="fixed-delay-retry"></a>Wiederholung mit fester Verzögerung

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("EventHubTrigger")]
[FixedDelayRetry(5, "00:00:10")]
public static async Task Run([EventHubTrigger("myHub", Connection = "EventHubConnection")] EventData[] events, ILogger log)
{
// ...
}
```

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Wiederholungsrichtlinie in der Datei *function.json* :

```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "fixedDelay",
        "maxRetryCount": 4,
        "delayInterval": "00:00:10"
    }
}
```
# <a name="javascript"></a>[JavaScript](#tab/javascript)

Wiederholungsrichtlinie in der Datei *function.json* :


```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "fixedDelay",
        "maxRetryCount": 4,
        "delayInterval": "00:00:10"
    }
}
```

# <a name="python"></a>[Python](#tab/python)

Wiederholungsrichtlinie in der Datei *function.json* :

```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "fixedDelay",
        "maxRetryCount": 4,
        "delayInterval": "00:00:10"
    }
}
```

# <a name="java"></a>[Java](#tab/java)

Wiederholungsrichtlinie in der Datei *function.json* :


```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "fixedDelay",
        "maxRetryCount": 4,
        "delayInterval": "00:00:10"
    }
}
```
---

#### <a name="exponential-backoff-retry"></a>Wiederholung mit exponentiellem Backoff

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("EventHubTrigger")]
[ExponentialBackoffRetry(5, "00:00:04", "00:15:00")]
public static async Task Run([EventHubTrigger("myHub", Connection = "EventHubConnection")] EventData[] events, ILogger log)
{
// ...
}
```

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Wiederholungsrichtlinie in der Datei *function.json* :

```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "exponentialBackoff",
        "maxRetryCount": 5,
        "minimumInterval": "00:00:10",
        "maximumInterval": "00:15:00"
    }
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Wiederholungsrichtlinie in der Datei *function.json* :

```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "exponentialBackoff",
        "maxRetryCount": 5,
        "minimumInterval": "00:00:10",
        "maximumInterval": "00:15:00"
    }
}
```

# <a name="python"></a>[Python](#tab/python)

Wiederholungsrichtlinie in der Datei *function.json* :

```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "exponentialBackoff",
        "maxRetryCount": 5,
        "minimumInterval": "00:00:10",
        "maximumInterval": "00:15:00"
    }
}
```

# <a name="java"></a>[Java](#tab/java)

Wiederholungsrichtlinie in der Datei *function.json* :

```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "exponentialBackoff",
        "maxRetryCount": 5,
        "minimumInterval": "00:00:10",
        "maximumInterval": "00:15:00"
    }
}
```
---

|Eigenschaft von „function.json“  |Attributeigenschaft | Beschreibung |
|---------|---------|---------| 
|strategy|–|Erforderlich. Die Wiederholungsstrategie, die verwendet werden soll. Gültige Werte sind `fixedDelay` und `exponentialBackoff`.|
|maxRetryCount|–|Erforderlich. Die maximale Anzahl zulässiger Wiederholungen pro Funktionsausführung. `-1` bedeutet unbegrenzte Wiederholungen.|
|delayInterval|–|Die Verzögerung, die zwischen Wiederholungsversuchen bei Verwendung der Strategie `fixedDelay`.|
|MinimumIntervall|–|Die geringste Wiederholungsverzögerung bei Verwendung der Strategie `exponentialBackoff`.|
|Maximumintervall|–|Die höchste Wiederholungsverzögerung bei Verwendung der Strategie `exponentialBackoff`.| 

## <a name="trigger-specific-retry-support"></a>Unterstützung für triggerspezifische Wiederholungen

Einige Trigger bieten Wiederholungen an der Triggerquelle.  Diese Triggerwiederholungen können die Wiederholungsrichtlinie der Funktions-App ergänzen oder ersetzen.  Wenn eine bestimmte Anzahl von Wiederholungen gewünscht wird, sollten Sie anstelle der generischen Hostwiederholungsrichtlinie eine triggerspezifische Wiederholungsrichtlinie verwenden.  Die folgenden Trigger unterstützen Wiederholungen an der Triggerquelle:

* [Azure Blob Storage](../articles/azure-functions/functions-bindings-storage-blob.md)
* [Azure Queue Storage](../articles/azure-functions/functions-bindings-storage-queue.md)
* [Azure Service Bus (Warteschlange/Thema)](../articles/azure-functions/functions-bindings-service-bus.md)

Standardmäßig wiederholen diese Trigger Anforderungen bis zu fünfmal. Nach dem fünften Wiederholungsversuch schreiben sowohl der Azure Queue Storage- als auch der Azure Service Bus-Trigger eine Nachricht in die [Warteschlange für nicht verarbeitbare Nachrichten](../articles/azure-functions/functions-bindings-storage-queue-trigger.md#poison-messages).
