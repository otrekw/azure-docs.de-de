---
title: Funktionsverkettung in Durable Functions – Azure
description: Es wird beschrieben, wie Sie ein Durable Functions-Beispiel mit einer Funktionssequenz ausführen.
author: cgillum
ms.topic: conceptual
ms.date: 11/29/2019
ms.author: azfuncdf
ms.openlocfilehash: 8da4ce7801cc98f9ffb32eb7b506eaf1ccd877dd
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2020
ms.locfileid: "77562059"
---
# <a name="function-chaining-in-durable-functions---hello-sequence-sample"></a>Funktionsverkettung in Durable Functions – „Hello Sequence“-Beispiel

Bei der Funktionsverkettung geht es um das Muster für die Ausführung einer Funktionssequenz in einer bestimmten Reihenfolge. Häufig muss die Ausgabe einer Funktion auf die Eingabe einer anderen Funktion angewendet werden. Dieser Artikel beschreibt die Verkettungssequenz, die Sie durch Abschließen des Durable Functions-Schnellstarts ([C#](durable-functions-create-first-csharp.md) oder [JavaScript](quickstart-js-vscode.md)) erstellen. Weitere Informationen zu Durable Functions finden Sie unter [Übersicht zu Durable Functions](durable-functions-overview.md).

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="the-functions"></a>Die Funktionen

In diesem Artikel werden die folgenden Funktionen in der Beispiel-App beschrieben:

* `E1_HelloSequence`: Eine [Orchestratorfunktion](durable-functions-bindings.md#orchestration-trigger), die `E1_SayHello` mehrmals hintereinander aufruft. Die Ausgaben der `E1_SayHello`-Aufrufe werden gespeichert, und die Ergebnisse werden aufgezeichnet.
* `E1_SayHello`: Eine [Aktivitätsfunktion](durable-functions-bindings.md#activity-trigger), die einer Zeichenfolge das Wort „Hello“ voranstellt.
* `HttpStart`: Eine per HTTP ausgelöste Funktion, mit der eine Instanz der Orchestrierung gestartet wird.

### <a name="e1_hellosequence-orchestrator-function"></a>Orchestratorfunktion „E1_HelloSequence“

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs?range=13-25)]

Alle C#-Orchestrierungsfunktionen müssen über einen Parameter vom Typ `DurableOrchestrationContext` verfügen, der in der Assembly `Microsoft.Azure.WebJobs.Extensions.DurableTask` enthalten ist. Mit diesem Kontextobjekt können Sie andere Funktionen vom Typ *Aktivität* aufrufen und Eingabeparameter mit der zugehörigen `CallActivityAsync`-Methode übergeben.

Im Code wird `E1_SayHello` dreimal nacheinander mit unterschiedlichen Parameterwerten aufgerufen. Der Rückgabewert jedes Aufrufs wird der Liste `outputs` hinzugefügt, die am Ende der Funktion zurückgegeben wird.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

> [!NOTE]
> JavaScript Durable Functions sind nur für die Functions 2.0-Runtime verfügbar.

#### <a name="functionjson"></a>function.json

Hier ist der Inhalt der Datei *function.json* für die Orchestratorfunktion angegeben, falls Sie Visual Studio Code oder das Azure-Portal für die Entwicklung verwenden. Die meisten *function.json*-Dateien für Orchestratoren ähneln dieser Datei stark.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/function.json)]

Ein wichtiger Punkt ist der Bindungstyp `orchestrationTrigger`. Alle Orchestratorfunktionen müssen diesen Triggertyp verwenden.

> [!WARNING]
> Um die Regel „keine E/A“ für Orchestratorfunktionen zu beachten, sollten Sie keine Eingabe- oder Ausgabebindungen verwenden, wenn Sie die Triggerbindung `orchestrationTrigger` einsetzen.  Falls andere Eingabe- oder Ausgabebindungen erforderlich sind, sollten sie stattdessen im Kontext von `activityTrigger`-Funktionen verwendet werden, die vom Orchestrator aufgerufen werden. Weitere Informationen finden Sie im Artikel [Codeeinschränkungen für Orchestratorfunktionen](durable-functions-code-constraints.md).

#### <a name="indexjs"></a>index.js

Dies ist die Funktion:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/index.js)]

Alle JavaScript-Orchestrierungsfunktionen müssen das [Modul `durable-functions`](https://www.npmjs.com/package/durable-functions) enthalten. Es handelt sich um eine Bibliothek, mit der Sie Durable Functions in JavaScript schreiben können. Zwischen einer Orchestrierungsfunktion und anderen JavaScript-Funktionen bestehen drei wesentliche Unterschiede:

1. Die Funktion ist eine [Generatorfunktion](https://docs.microsoft.com/scripting/javascript/advanced/iterators-and-generators-javascript).
2. Die Funktion wird in einem Aufruf der `orchestrator`-Methode des Moduls `durable-functions` umschlossen (hier `df`).
3. Die Funktion muss synchron sein. Da die orchestrator-Methode den Aufruf von „context.done“ behandelt, sollte die Funktion lediglich „zurückkehren“.

Das `context`-Objekt enthält ein dauerhaftes `df`-Orchestrierungskontextobjekt, mit dem Sie andere *Aktivität*sfunktionen aufrufen und Eingabeparameter mit der zugehörigen `callActivity`-Methode übergeben können. Im Code wird `E1_SayHello` dreimal nacheinander mit unterschiedlichen Parameterwerten aufgerufen. Dabei wird `yield` verwendet, um anzugeben, dass für die Ausführung auf die Rückgabe der asynchronen Aufrufe der Aktivitätsfunktion gewartet werden muss. Der Rückgabewert jedes Aufrufs wird dem Array `outputs` hinzugefügt, das am Ende der Funktion zurückgegeben wird.

---

### <a name="e1_sayhello-activity-function"></a>Aktivitätsfunktion „E1_SayHello“

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs?range=27-32)]

Aktivitäten verwenden das `ActivityTrigger`-Attribut. Verwenden Sie den bereitgestellten `IDurableActivityContext`, um aktivitätsbezogene Aktionen auszuführen, z. B. den Zugriff auf den Eingabewert mit `GetInput<T>`.

Die Implementierung von `E1_SayHello` ist ein relativ einfacher Zeichenfolgen-Formatierungsvorgang.

Anstatt an einen `IDurableActivityContext` zu binden, können Sie direkt an den Typ binden, der an die Aktivitätsfunktion übergeben wird. Beispiel:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs?range=34-38)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

#### <a name="e1_sayhellofunctionjson"></a>E1_SayHello/function.json

Die Datei *function.json* für die Aktivitätsfunktion `E1_SayHello` ähnelt der Datei von `E1_HelloSequence`, aber es wird nicht der Bindungstyp `orchestrationTrigger`, sondern `activityTrigger` verwendet.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/function.json)]

> [!NOTE]
> Alle Funktionen, die von einer Orchestrierungsfunktion aufgerufen werden, müssen die Bindung `activityTrigger` verwenden.

Die Implementierung von `E1_SayHello` ist ein relativ einfacher Zeichenfolgen-Formatierungsvorgang.

#### <a name="e1_sayhelloindexjs"></a>E1_SayHello/index.js

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/index.js)]

Anders als eine JavaScript-Orchestrierungsfunktion erfordert eine Aktivitätsfunktion keine spezielle Einrichtung. Die von der Orchestratorfunktion übergebene Eingabe befindet sich im Objekt `context.bindings` unter dem Namen der Bindung `activityTrigger`, in diesem Fall `context.bindings.name`. Der Name der Bindung kann als Parameter der exportierten Funktion eingerichtet werden und der Zugriff darauf direkt erfolgen. Dies ist bei dem Beispielcode der Fall.

---

### <a name="httpstart-client-function"></a>Clientfunktion „HttpStart“

Sie können eine Instanz der Orchestratorfunktion mithilfe einer Clientfunktion starten. Sie verwenden die per HTTP ausgelöste `HttpStart`-Funktion, um Instanzen von `E1_HelloSequence` zu starten.

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs?range=13-30)]

Um mit Orchestratoren zu interagieren, muss die Funktion eine `DurableClient`-Eingabebindung enthalten. Mit dem Client starten Sie die Orchestrierung. Er kann Ihnen außerdem helfen, eine HTTP-Antwort zurückzugeben, die URLs zum Überprüfen des Status der neuen Orchestrierung enthält.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

#### <a name="httpstartfunctionjson"></a>HttpStart/function.json

[!code-json[Main](~/samples-durable-functions/samples/javascript/HttpStart/function.json?highlight=16-20)]

Um mit Ochestratoren zu interagieren, muss die Funktion eine `durableClient`Eingabebindung enthalten.

#### <a name="httpstartindexjs"></a>HttpStart/index.js

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpStart/index.js)]

Verwenden Sie `df.getClient`, um ein `DurableOrchestrationClient`-Objekt zu erhalten. Mit dem Client starten Sie die Orchestrierung. Er kann Ihnen außerdem helfen, eine HTTP-Antwort zurückzugeben, die URLs zum Überprüfen des Status der neuen Orchestrierung enthält.

---

## <a name="run-the-sample"></a>Ausführen des Beispiels

Senden Sie die folgende HTTP POST-Anforderung an die `HttpStart`-Funktion, um die `E1_HelloSequence`-Orchestrierung auszuführen.

```
POST http://{host}/orchestrators/E1_HelloSequence
```

> [!NOTE]
> Im vorherigen HTTP-Ausschnitt wird davon ausgegangen, dass ein Eintrag in der Datei `host.json` vorhanden ist, der das Standardpräfix `api/` aus den URLs aller HTTP-Triggerfunktionen entfernt. Sie finden das Markup für diese Konfiguration in der Datei `host.json` in den Beispielen.

Wenn Sie das Beispiel z. B. in einer Funktions-App namens „myfunctionapp“ ausführen, ersetzen Sie „{host}“ durch „myfunctionapp.azurewebsites.net“.

Das Ergebnis ist eine HTTP 202-Antwort, die wie folgt aussehen kann (hier gekürzt):

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/runtime/webhooks/durabletask/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

An diesem Punkt wird die Orchestrierung in die Warteschlange eingereiht und sofort ausgeführt. Die URL im Header `Location` kann verwendet werden, um den Status der Ausführung zu überprüfen.

```
GET http://{host}/runtime/webhooks/durabletask/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

Das Ergebnis ist der Status der Orchestrierung. Die Ausführung und der Abschluss werden schnell durchgeführt, sodass Sie den Status *Completed* (Abgeschlossen) mit dieser Antwort sehen (hier gekürzt):

```
HTTP/1.1 200 OK
Content-Length: 179
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":null,"output":["Hello Tokyo!","Hello Seattle!","Hello London!"],"createdTime":"2017-06-29T05:24:57Z","lastUpdatedTime":"2017-06-29T05:24:59Z"}
```

Sie sehen, dass das `runtimeStatus`-Element der Instanz *Completed* lautet und dass `output` das per JSON serialisierte Ergebnis der Ausführung der Orchestratorfunktion enthält.

> [!NOTE]
> Sie können eine ähnliche Startlogik auch für andere Triggertypen wie `queueTrigger`, `eventHubTrigger` oder `timerTrigger` implementieren.

Sehen Sie sich die Protokolle zur Funktionsausführung an. Die Funktion `E1_HelloSequence` wurde mehrmals gestartet und abgeschlossen. Dies liegt am Wiedergabeverhalten, das im Thema [Orchestrierungszuverlässigkeit](durable-functions-orchestrations.md#reliability) beschrieben wird. Andererseits ist es nur zu drei Ausführungen von `E1_SayHello` gekommen, da diese Funktionsausführungen nicht wiedergegeben werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Beispiel wird eine einfache Funktionsverkettungsorchestrierung veranschaulicht. Im nächsten Beispiel wird die Implementierung des Musters „Auffächern nach außen/innen“ gezeigt.

> [!div class="nextstepaction"]
> [Beispiel für „Auffächern nach außen/innen“ ausführen](durable-functions-cloud-backup.md)
