---
title: 'Auffächerungsszenarios (nach innen und außen) in Durable Functions: Azure'
description: In diesem Artikel wird beschrieben, wie ein Auffächerungsszenario (nach innen und außen) in der Erweiterung Durable Functions für Azure Functions implementiert wird.
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 91128033696af6a56488db7991987f1e384b719e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98027644"
---
# <a name="fan-outfan-in-scenario-in-durable-functions---cloud-backup-example"></a>Auffächerungsszenario (nach innen und außen) in Durable Functions – Beispiel der Cloudsicherung

*Auffächern/Auffächern nach innen* bezieht sich auf das Muster, bei dem mehrere Funktionen gleichzeitig ausgeführt werden und die Ergebnisse im Anschluss zusammengefasst werden. In diesem Artikel wird ein Beispiel beschrieben, in dem [Durable Functions](durable-functions-overview.md) zum Implementieren eines Auffächerungsszenarios (nach innen und außen) verwendet wird. Das Beispiel stellt eine langlebige Funktion dar, die alle oder einige der Websiteinhalte in Azure Storage sichert.

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>Übersicht über das Szenario

In diesem Beispiel laden die Funktionen alle Dateien unter einem angegebenen Verzeichnis rekursiv in den Blobspeicher hoch. Zudem zählen sie die Gesamtzahl der hochgeladenen Bytes.

Es ist möglich, eine einzelne Funktion zu schreiben, die alles übernimmt. Das Hauptproblem, mit dem Sie sich auseinandersetzen müssten, ist die **Skalierbarkeit**. Eine einzelne Funktion kann nur auf einem einzelnen virtuellen Computer ausgeführt werden. Der Durchsatz wird folglich durch den Durchsatz dieses virtuellen Computers beschränkt. Ein weiteres Problem stellt die **Zuverlässigkeit** dar. Wenn während des Vorgangs ein Fehler auftritt oder der gesamte Prozess mehr als 5 Minuten in Anspruch nimmt, kann bei der Sicherung in einem teilweise abgeschlossenen Zustand ein Fehler auftreten. Sie müsste anschließend neu gestartet werden.

Ein stabilerer Ansatz bestünde darin, zwei reguläre Funktionen zu schreiben: eine zum Aufzählen der Dateien und zum Hinzufügen der Dateinamen zu einer Warteschlange, und die andere zum Lesen aus der Warteschlange und zum Hochladen der Dateien in den Blobspeicher. Im Hinblick auf den Durchsatz und die Zuverlässigkeit ist diese Herangehensweise besser, Sie müssen hierfür jedoch eine Warteschlange bereitstellen und verwalten. Im Hinblick auf die **Zustandsverwaltung** und die **Koordinierung** bringt dies vor allem eine hohe Komplexität mit sich, falls Sie weitere Funktionen ausführen möchten, wie z.B. die Gesamtzahl der hochgeladenen Bytes melden.

Ein Durable Functions-Ansatz bietet Ihnen alle genannten Vorteile mit sehr geringem Mehraufwand.

## <a name="the-functions"></a>Die Funktionen

In diesem Artikel werden die folgenden Funktionen in der Beispiel-App beschrieben:

* `E2_BackupSiteContent`: Eine [Orchestratorfunktion](durable-functions-bindings.md#orchestration-trigger), die `E2_GetFileList` aufruft, um eine Liste der zu sichernden Dateien abzurufen, und dann `E2_CopyFileToBlob` aufruft, um die einzelnen Dateien zu sichern.
* `E2_GetFileList`: Eine [Aktivitäts Funktion](durable-functions-bindings.md#activity-trigger), die eine Liste der Dateien in einem Verzeichnis zurückgibt.
* `E2_CopyFileToBlob`: Eine Aktivitätsfunktion, die eine einzelne Datei in Azure Blob Storage sichert.

### <a name="e2_backupsitecontent-orchestrator-function"></a>Orchestratorfunktion „E2_BackupSiteContent“

Diese Orchestratorfunktion führt im Wesentlichen Folgendes aus:

1. Sie akzeptiert einen Wert des Typs `rootDirectory` als Eingabeparameter.
2. Sie ruft eine Funktion auf, um unter `rootDirectory` eine rekursive Liste mit Dateien abzurufen.
3. Sie führt mehrere parallele Funktionsaufrufe durch, um die einzelnen Dateien in Azure Blob Storage hochzuladen.
4. Sie wartet, bis alle Uploads abgeschlossen wurden.
5. Sie gibt die Gesamtzahl der Bytes zurück, die in Azure Blob Storage hochgeladen wurden.

# <a name="c"></a>[C#](#tab/csharp)

Im Folgenden wird der Code dargestellt, der die Orchestratorfunktion implementiert:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs?range=16-42)]

Beachten Sie die Zeile `await Task.WhenAll(tasks);`. Sämtliche einzelnen Aufrufe der `E2_CopyFileToBlob`-Funktion wurden *nicht* erwartet, sodass sie parallel ausgeführt werden können. Wenn dieses Array von Aufgaben an `Task.WhenAll` übergeben wird, wird eine Aufgabe zurückgegeben, die erst *nach Abschluss aller Kopiervorgänge* abgeschlossen wird. Wenn Sie mit der Task Parallel Library (TPL) in .NET vertraut sind, ist dies nicht neu für Sie. Der Unterschied besteht darin, dass diese Aufgaben auf mehreren virtuellen Computern gleichzeitig ausgeführt werden könnten. Die Durable Functions-Erweiterung stellt dabei sicher, dass die End-to-End-Ausführung gegenüber der Prozesswiederverwendung robust ist.

Nach dem Warten von `Task.WhenAll` aus wissen wir, dass alle Funktionsaufrufe abgeschlossen wurden und Werte an uns zurückgegeben haben. Jeder Aufruf von `E2_CopyFileToBlob` gibt die Anzahl von hochgeladenen Bytes zurück. Die Summe der Gesamtbytezahl wird folglich berechnet, indem alle diese Rückgabewerte addiert werden.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Die Funktion verwendet die Standarddatei *function.json* für Orchestratorfunktionen.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E2_BackupSiteContent/function.json)]

Im Folgenden wird der Code dargestellt, der die Orchestratorfunktion implementiert:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_BackupSiteContent/index.js)]

Beachten Sie die Zeile `yield context.df.Task.all(tasks);`. Sämtliche einzelnen Aufrufe der `E2_CopyFileToBlob`-Funktion wurden *nicht* ausgegeben, sodass sie parallel ausgeführt werden können. Wenn dieses Array von Aufgaben an `context.df.Task.all` übergeben wird, wird eine Aufgabe zurückgegeben, die erst *nach Abschluss aller Kopiervorgänge* abgeschlossen wird. Wenn Sie mit [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) in JavaScript vertraut sind, ist dies nicht neu für Sie. Der Unterschied besteht darin, dass diese Aufgaben auf mehreren virtuellen Computern gleichzeitig ausgeführt werden könnten. Die Durable Functions-Erweiterung stellt dabei sicher, dass die End-to-End-Ausführung gegenüber der Prozesswiederverwendung robust ist.

> [!NOTE]
> Obwohl Aufgaben ein ähnliches Konzept zugrunde liegt wie JavaScript-Zusagen, sollten für Orchestratorfunktionen `context.df.Task.all` und `context.df.Task.any` anstelle von `Promise.all` und `Promise.race` verwendet werden, um die Parallelisierung von Aufgaben zu verwalten.

Nach der Rückgabe von `context.df.Task.all` wissen wir, dass alle Funktionsaufrufe abgeschlossen wurden und Werte an uns zurückgegeben haben. Jeder Aufruf von `E2_CopyFileToBlob` gibt die Anzahl von hochgeladenen Bytes zurück. Die Summe der Gesamtbytezahl wird folglich berechnet, indem alle diese Rückgabewerte addiert werden.

# <a name="python"></a>[Python](#tab/python)

Die Funktion verwendet die Standarddatei *function.json* für Orchestratorfunktionen.

[!code-json[Main](~/samples-durable-functions-python/samples/fan_in_fan_out/E2_BackupSiteContent/function.json)]

Im Folgenden wird der Code dargestellt, der die Orchestratorfunktion implementiert:

[!code-python[Main](~/samples-durable-functions-python/samples/fan_in_fan_out/E2_BackupSiteContent/\_\_init\_\_.py)]

Beachten Sie die Zeile `yield context.task_all(tasks);`. Sämtliche einzelnen Aufrufe der `E2_CopyFileToBlob`-Funktion wurden *nicht* ausgegeben, sodass sie parallel ausgeführt werden können. Wenn dieses Array von Aufgaben an `context.task_all` übergeben wird, wird eine Aufgabe zurückgegeben, die erst *nach Abschluss aller Kopiervorgänge* abgeschlossen wird. Wenn Sie mit [`asyncio.gather`](https://docs.python.org/3/library/asyncio-task.html#asyncio.gather) in Python vertraut sind, ist dies nicht neu für Sie. Der Unterschied besteht darin, dass diese Aufgaben auf mehreren virtuellen Computern gleichzeitig ausgeführt werden könnten. Die Durable Functions-Erweiterung stellt dabei sicher, dass die End-to-End-Ausführung gegenüber der Prozesswiederverwendung robust ist.

> [!NOTE]
> Obwohl Aufgaben ein ähnliches Konzept zugrunde liegt wie Python-Awaitables, sollten für Orchestratorfunktionen `yield` und die APIs `context.task_all` und `context.task_any` verwendet werden, um die Parallelisierung von Aufgaben zu verwalten.

Nach der Rückgabe von `context.task_all` wissen wir, dass alle Funktionsaufrufe abgeschlossen wurden und Werte an uns zurückgegeben haben. Jeder Aufruf von `E2_CopyFileToBlob` gibt die Anzahl von hochgeladenen Bytes zurück. Die Summe der Gesamtbytezahl wird folglich berechnet, indem alle diese Rückgabewerte addiert werden.

---

### <a name="helper-activity-functions"></a>Hilfsaktivitätsfunktionen

Bei den Hilfsaktivitätsfunktionen handelt es sich, wie bei anderen Beispielen, um normale Funktionen, die die Triggerbindung `activityTrigger` verwenden.

#### <a name="e2_getfilelist-activity-function"></a>Aktivitätsfunktion „E2_GetFileList“

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs?range=44-54)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Die Datei *function.json* für `E2_GetFileList` sieht wie folgt aus:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E2_GetFileList/function.json)]

Und hier ist die Implementierung:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_GetFileList/index.js)]

Die Funktion verwendet das `readdirp`-Modul (Version 2.x), um die Verzeichnisstruktur rekursiv zu lesen.

# <a name="python"></a>[Python](#tab/python)

Die Datei *function.json* für `E2_GetFileList` sieht wie folgt aus:

[!code-json[Main](~/samples-durable-functions-python/samples/fan_in_fan_out/E2_GetFileList/function.json)]

Und hier ist die Implementierung:

[!code-python[Main](~/samples-durable-functions-python/samples/fan_in_fan_out/E2_GetFileList/\_\_init\_\_.py)]

---

> [!NOTE]
> Sie wundern sich vielleicht, weshalb Sie diesen Code nicht einfach direkt in die Orchestratorfunktion einfügen konnten. Das können Sie, aber damit würden Sie eine der Grundregeln der Orchestratorfunktionen brechen. Diese besagt, dass die Funktionen niemals eine Eingabe/Ausgabe vornehmen dürfen, den lokalen Dateisystemzugriff inbegriffen. Weitere Informationen finden Sie unter [Codeeinschränkungen für Orchestratorfunktionen](durable-functions-code-constraints.md).

#### <a name="e2_copyfiletoblob-activity-function"></a>Aktivitätsfunktion „E2_CopyFileToBlob“

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs?range=56-81)]

> [!NOTE]
> Sie müssen das NuGet-Paket `Microsoft.Azure.WebJobs.Extensions.Storage` installieren, um den Beispielcode auszuführen.

Die Funktion verwendet einige erweiterte Features von Azure Functions-Bindungen (d. h. die Verwendung des [Parameters `Binder`](../functions-dotnet-class-library.md#binding-at-runtime)). Sie müssen sich zum Zwecke dieser ausführlichen Anleitung jedoch nicht um diese Details kümmern.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Die Datei *function.json* für `E2_CopyFileToBlob` ist ebenso einfach:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E2_CopyFileToBlob/function.json)]

Die JavaScript-Implementierung verwendet das [Azure Storage SDK für Node](https://github.com/Azure/azure-storage-node), um die Dateien in Azure Blob Storage hochzuladen.

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_CopyFileToBlob/index.js)]

# <a name="python"></a>[Python](#tab/python)

Die Datei *function.json* für `E2_CopyFileToBlob` ist ebenso einfach:

[!code-json[Main](~/samples-durable-functions-python/samples/fan_in_fan_out/E2_CopyFileToBlob/function.json)]

Die Python-Implementierung verwendet das [Azure Storage SDK für Python](https://github.com/Azure/azure-storage-python), um die Dateien in Azure Blob Storage hochzuladen.

[!code-python[Main](~/samples-durable-functions-python/samples/fan_in_fan_out/E2_CopyFileToBlob/\_\_init\_\_.py)]

---

Bei der Implementierung wird die Datei vom Datenträger geladen, und die Inhalte werden asynchron in ein Blob mit dem gleichen Namen im Container „backups“ gestreamt. Der Rückgabewert ist die Anzahl der Bytes, die in den Speicher kopiert und anschließend von der Orchestratorfunktion für die Berechnung der Summe verwendet werden.

> [!NOTE]
> Dies ist ein gutes Beispiel für das Verschieben von Eingabe-/Ausgabevorgängen in die Funktion `activityTrigger`. Die Arbeit kann nicht nur auf viele verschiedene virtuelle Computer verteilt werden, sondern Sie können auch den Fortschritt überprüfen. Wenn der Hostprozess aus irgendeinem Grund beendet wird, wissen Sie, welche Uploads bereits abgeschlossen wurden.

## <a name="run-the-sample"></a>Ausführen des Beispiels

Sie können die Orchestrierung unter Windows starten, indem Sie die folgende HTTP POST-Anforderung senden.

```
POST http://{host}/orchestrators/E2_BackupSiteContent
Content-Type: application/json
Content-Length: 20

"D:\\home\\LogFiles"
```

Alternativ können Sie die Orchestrierung in einer Linux-Funktions-App (Python kann derzeit nur unter Linux für App Service genutzt werden) wie folgt starten:

```
POST http://{host}/orchestrators/E2_BackupSiteContent
Content-Type: application/json
Content-Length: 20

"/home/site/wwwroot"
```

> [!NOTE]
> Die Funktion `HttpStart`, die Sie aufrufen, kann nur mit JSON-formatiertem Inhalt verwendet werden. Aus diesem Grund ist der Header `Content-Type: application/json` erforderlich und der Verzeichnispfad als JSON-Zeichenfolge codiert. Darüber hinaus wird im HTTP-Ausschnitt davon ausgegangen, dass ein Eintrag in der Datei `host.json` vorhanden ist, der das Standardpräfix `api/` aus den URLs aller HTTP-Triggerfunktionen entfernt. Sie finden das Markup für diese Konfiguration in der Datei `host.json` in den Beispielen.

Diese HTTP-Anforderung löst den Orchestrator `E2_BackupSiteContent` aus und übergibt die Zeichenfolge `D:\home\LogFiles` als Parameter. Die Antwort enthält einen Link, über den der Status des Sicherungsvorgangs abgerufen werden kann:

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/runtime/webhooks/durabletask/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

Je nachdem, wie viele Protokolldateien in Ihrer Funktions-App enthalten sind, könnte dieser Vorgang einige Minuten in Anspruch nehmen. Sie können den aktuellen Status abrufen, indem Sie die URL im Header `Location` der vorherigen HTTP 202-Antwort abfragen.

```
GET http://{host}/runtime/webhooks/durabletask/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

```
HTTP/1.1 202 Accepted
Content-Length: 148
Content-Type: application/json; charset=utf-8
Location: http://{host}/runtime/webhooks/durabletask/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"runtimeStatus":"Running","input":"D:\\home\\LogFiles","output":null,"createdTime":"2019-06-29T18:50:55Z","lastUpdatedTime":"2019-06-29T18:51:16Z"}
```

In diesem Fall wird die Funktion weiterhin ausgeführt. Sie können die Eingabe, die in den Orchestratorzustand gespeichert wurde, sowie die Zeit der letzten Aktualisierung anzeigen. Sie können weiterhin die `Location`-Headerwerte zum Abfragen der Beendigung verwenden. Wenn der Status „Abgeschlossen“ lautet, wird Ihnen ein HTTP-Antwortwert wie der folgende angezeigt:

```
HTTP/1.1 200 OK
Content-Length: 152
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"D:\\home\\LogFiles","output":452071,"createdTime":"2019-06-29T18:50:55Z","lastUpdatedTime":"2019-06-29T18:51:26Z"}
```

Jetzt können Sie sehen, dass die Orchestrierung abgeschlossen ist und wie viel Zeit für den Abschluss benötigt wurde. Zudem wird Ihnen ein Wert für das Feld `output` angezeigt, der angibt, dass etwa 450 KB Protokolle hochgeladen wurden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Beispiel wurde das Implementieren das Auffächerungsmusters angezeigt. Das nächste Beispiel zeigt, wie man das Überwachungsmuster mithilfe von [dauerhaften Timern](durable-functions-timers.md) implementiert.

> [!div class="nextstepaction"]
> [Führen Sie den Überwachungsbeispiels](durable-functions-monitor.md)