---
title: Verbessern der Durchsatzleistung von Python-Apps in Azure Functions
description: Hier erfahren Sie, wie Sie mithilfe von Python hochleistungsfähige Azure Functions-Apps erstellen, die sich bei hoher Last sehr gut skalieren lassen.
ms.topic: article
ms.date: 10/13/2020
ms.custom: devx-track-python
ms.openlocfilehash: e3bbdb8819062d45d071633e0208fb58a003da54
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98786105"
---
# <a name="improve-throughput-performance-of-python-apps-in-azure-functions"></a>Verbessern der Durchsatzleistung von Python-Apps in Azure Functions

Beim Entwickeln von Apps für Azure Functions mithilfe von Python müssen Sie wissen, wie leistungsfähig Ihre Funktionen sind und wie sich die Leistung auf die Skalierung Ihrer Funktions-App auswirkt. Beim Entwerfen von hochleistungsfähigen Apps ist dieser Aspekt besonders wichtig. Die wichtigsten Faktoren, die Sie beim Entwerfen, Schreiben und Konfigurieren Ihrer Funktions-Apps bedenken müssen, sind die Konfigurationen für die Aufskalierung und die Durchsatzleistung.

## <a name="horizontal-scaling"></a>Horizontale Skalierung
Standardmäßig überwacht Azure Functions automatisch die Auslastung Ihrer Anwendung und erstellt bei Bedarf zusätzliche Host Instanzen für Python. Um zu entscheiden, wann Instanzen hinzugefügt werden sollen, verwendet Azure Functions integrierte Schwellenwerte für verschiedene Triggertypen, beispielsweise in Bezug auf das Alter von Nachrichten und die Warteschlangengröße für QueueTrigger. Diese Schwellenwerte sind nicht vom Benutzer konfigurierbar. Weitere Informationen finden Sie unter [Ereignisgesteuerte Skalierung in Azure Functions](event-driven-scaling.md).

## <a name="improving-throughput-performance"></a>Verbessern der Durchsatzleistung

Die Standardkonfigurationen sind für die meisten Azure Functions-Anwendungen geeignet. Allerdings können Sie die Leistung des Durchsatzes Ihrer Anwendungen verbessern, indem Sie Konfigurationen einsetzen, die zu Ihrem Workloadprofil passen. Daher besteht der erste Schritt darin, zu verstehen, was für ein Typ von Workload ausgeführt wird.

| Workloadtyp | Merkmale der Funktions-App       | Beispiele                                          |
| ------------- | ---------------------------------- | ------------------------------------------------- |
| **E/A-Bindung**     | • Die App muss viele gleichzeitige Aufrufe verarbeiten.<br>• Die App verarbeitet eine große Anzahl von E/A-Ereignissen, wie z. B. Netzwerkaufrufe und Lese-/Schreibvorgänge auf Datenträgern. | • Web-APIs                                          |
| **CPU-Bindung**     | • Die App führt Langzeitberechnungen durch, wie z. B. die Größenänderung von Bildern.<br>• Die App führt Datentransformationen durch.                                                | • Datenverarbeitung<br>• Machine Learning-Rückschluss<br> |

 
Da Funktionsworkloads in der Praxis üblicherweise eine Mischung aus E/A- und CPU-gebundenen Workloads sind, sollten Sie das Profil der App unter realistischen Produktionslasten erstellen.


### <a name="performance-specific-configurations"></a>Leistungsspezifische Konfigurationen

Nachdem Sie das Workloadprofil ihrer Funktions-App nun kennen, können Sie die folgenden Konfigurationen zum Verbessern der Durchsatzleistung ihrer Funktionen einsetzen.

* [Asynchron](#async)
* [Mehrere Sprachworker](#use-multiple-language-worker-processes)
* [Maximale Anzahl von Workern in einem Sprachworkerprozess](#set-up-max-workers-within-a-language-worker-process)
* [Ereignisschleife](#managing-event-loop)
* [Vertikale Skalierung](#vertical-scaling)



#### <a name="async"></a>Async

Da [Python eine Single-Threading-Runtime ist](https://wiki.python.org/moin/GlobalInterpreterLock), kann eine Hostinstanz für Python standardmäßig jeweils nur einen Funktionsaufruf gleichzeitig verarbeiten. Bei Anwendungen, die eine große Anzahl von E/A-Ereignissen verarbeiten und/oder E/A-gebunden sind, können Sie die Leistung erheblich verbessern, indem Sie Funktionen asynchron ausführen.

Um eine Funktion asynchron auszuführen, verwenden Sie die `async def`-Anweisung, die die Funktion mit [asyncio](https://docs.python.org/3/library/asyncio.html) direkt ausführt:

```python
async def main():
    await some_nonblocking_socket_io_op()
```
Im Folgenden finden Sie ein Beispiel für eine Funktion mit HTTP-Triggern, die [aiohttp](https://pypi.org/project/aiohttp/)-HTTP-Clients verwendet:

```python
import aiohttp

import azure.functions as func

async def main(req: func.HttpRequest) -> func.HttpResponse:
    async with aiohttp.ClientSession() as client:
        async with client.get("PUT_YOUR_URL_HERE") as response:
            return func.HttpResponse(await response.text())

    return func.HttpResponse(body='NotFound', status_code=404)
```


Eine Funktion ohne das Schlüsselwort `async` wird automatisch in einem ThreadPoolExecutor-Threadpool ausgeführt:

```python
# Runs in an ThreadPoolExecutor threadpool. Number of threads is defined by PYTHON_THREADPOOL_THREAD_COUNT. 
# The example is intended to show how default synchronous function are handled.

def main():
    some_blocking_socket_io()
```

Um den vollen Nutzen aus der asynchronen Ausführung von Funktionen zu ziehen, muss der E/A-Vorgang bzw. die Bibliothek, die in Ihrem Code verwendet wird, ebenfalls asynchron implementiert sein. Die Verwendung synchroner E/A-Vorgänge in Funktionen, die als asynchron definiert sind, **kann die Gesamtleistung beeinträchtigen**. Auch wenn für die von Ihnen verwendeten Bibliotheken keine asynchrone Version implementiert ist, können Sie möglicherweise von der asynchronen Ausführung Ihres Codes profitieren, indem Sie in Ihrer App [Ereignisschleifen verwenden](#managing-event-loop). 

Hier sind einige Beispiele von Clientbibliotheken, die asynchrone Muster implementiert haben:
- [aiohttp](https://pypi.org/project/aiohttp/): HTTP-Client/-Server für asyncio 
- [Streams API](https://docs.python.org/3/library/asyncio-stream.html): Allgemeine async/await-fähige Primitiven für die Arbeit mit Netzwerkverbindungen
- [Janus Queue](https://pypi.org/project/janus/): Threadsichere asyncio-fähige Warteschlange für Python
- [pyzmq](https://pypi.org/project/pyzmq/): Python-Bindungen für ZeroMQ
 
##### <a name="understanding-async-in-python-worker"></a>Asynchrone Verarbeitung in Python-Workern

Wenn Sie `async` vor einer Funktionssignatur definieren, markiert Python die Funktion als Coroutine. Die Coroutine kann beim Aufruf als Task in einer Ereignisschleife geplant werden. Wenn Sie `await` in einer asynchronen Funktion aufrufen, wird eine Fortsetzung in der Ereignisschleife registriert, und die Ereignisschleife kann den nächsten Task während der Wartezeit verarbeiten.

Unser Python-Worker nutzt die Ereignisschleife gemeinsam mit der `async`-Funktion des Kunden und kann mehrere Anforderungen gleichzeitig verarbeiten. Wir empfehlen unseren Kunden dringend die Verwendung von Bibliotheken, die mit asyncio kompatibel sind (z. B. [aiohttp](https://pypi.org/project/aiohttp/) oder [pyzmq](https://pypi.org/project/pyzmq/)). Durch Umsetzung dieser Empfehlungen können Sie den Durchsatz Ihrer Funktion im Vergleich zu synchron implementierten Bibliotheken erheblich steigern.

> [!NOTE]
>  Wenn Ihre Funktion als `async` ohne `await` in der Implementierung deklariert ist, ist die Leistung der Funktion wesentlich beeinträchtigt, da die Ereignisschleife blockiert wird. Dadurch wiederum wird der Python-Worker daran gehindert, gleichzeitige Anforderungen zu verarbeiten.

#### <a name="use-multiple-language-worker-processes"></a>Verwenden mehrerer Sprachworkerprozesse

Standardmäßig verfügt jede Functions-Hostinstanz über einen einzigen Sprachworkerprozess. Sie können die Anzahl der Workerprozesse pro Host erhöhen (bis zu 10), indem Sie die Anwendungseinstellung [FUNCTIONS_WORKER_PROCESS_COUNT](functions-app-settings.md#functions_worker_process_count) verwenden. Azure Functions versucht dann, gleichzeitige Funktionsaufrufe gleichmäßig auf diese Worker zu verteilen.

Für CPU-gebundene Apps sollten Sie die Anzahl der Sprachworker gleich oder höher einstellen als die Anzahl der Kerne, die pro Funktions-App verfügbar sind. Weitere Informationen finden Sie unter [Verfügbare Instanz-SKUs](functions-premium-plan.md#available-instance-skus). 

E/A-gebundene Apps können auch davon profitieren, eine höhere Anzahl von Workerprozessen im Vergleich zu den verfügbaren Kerne festzulegen. Denken Sie daran, dass eine zu hohe Anzahl von Workern die Gesamtleistung aufgrund der höheren Anzahl der erforderlichen Kontextwechsel beeinträchtigen kann. 

Die FUNCTIONS_WORKER_PROCESS_COUNT gilt für jeden Host, der von Functions erstellt wird, wenn Ihre Anwendung horizontal skaliert wird, um die Anforderungen zu erfüllen.

#### <a name="set-up-max-workers-within-a-language-worker-process"></a>Festlegen der maximalen Workeranzahl in einem Sprachworkerprozess

Wie bereits im Abschnitt zur [asynchronen Verarbeitung](#understanding-async-in-python-worker) erwähnt, behandelt der Python-Sprachworker Funktionen und [Coroutinen](https://docs.python.org/3/library/asyncio-task.html#coroutines) unterschiedlich. Eine Coroutine wird in derselben Ereignisschleife ausgeführt, in der auch der Sprachworker ausgeführt wird. Ein Funktionsaufruf dagegen wird als Thread in einem [ThreadPoolExecutor](https://docs.python.org/3/library/concurrent.futures.html#threadpoolexecutor) ausgeführt, der vom Sprachworker verwaltet wird.

Sie können den Wert für die maximale Anzahl von Workern, die für die Ausführung von synchronen Funktionen zulässig sind, mithilfe der Anwendungseinstellung [PYTHON_THREADPOOL_THREAD_COUNT](functions-app-settings.md#python_threadpool_thread_count) festlegen. Dieser Wert legt das `max_worker`-Argument des ThreadPoolExecutor-Objekts fest, sodass Python zur asynchronen Ausführung von Aufrufen einen Pool mit höchstens so vielen Threads verwenden kann, wie durch `max_worker` festgelegt. Die Einstellung `PYTHON_THREADPOOL_THREAD_COUNT` gilt für jeden Worker, den der Functions-Host erstellt, und Python entscheidet, wann ein neuer Thread erstellt und wann der vorhandene, im Leerlauf befindliche Thread wiederverwendet werden soll. In älteren Python-Versionen (also `3.8`, `3.7` und `3.6`), ist der `max_worker`-Wert auf 1 festgelegt. In der Python-Version `3.9`, ist `max_worker` auf `None` festgelegt.

Bei CPU-gebundenen Apps sollten Sie mit einem niedrigen Wert (1) beginnen und den Wert erhöhen, wenn Sie mit der Workload experimentieren. Dieser Vorschlag dient dazu, die benötigte Zeit für Kontextwechsel zu reduzieren und den Abschluss von CPU-gebundenen Tasks zu ermöglichen.

Bei E/A-gebundenen Apps sollten Sie beträchtliche Vorteile erzielen können, indem Sie die Anzahl von Threads für die Verarbeitung jedes Aufrufs erhöhen. Hier empfiehlt es sich, mit dem Python-Standardwert minus Anzahl von Kernen plus 4 zu beginnen, und die Einstellung entsprechend den erzielten Durchsatzwerten anzupassen.

Bei Apps mit gemischten Workloads sollten Sie die Konfigurationen für `FUNCTIONS_WORKER_PROCESS_COUNT` und `PYTHON_THREADPOOL_THREAD_COUNT` ausbalancieren, um den Durchsatz zu maximieren. Um herauszufinden, wofür Ihre Funktions-Apps am meisten Zeit benötigen, empfiehlt es sich, Profile für die Apps zu erstellen und die Werte entsprechend dem beobachteten Verhalten festzulegen. Weitere Informationen zu den Anwendungseinstellungen für FUNCTIONS_WORKER_PROCESS_COUNT finden Sie in [diesem Abschnitt](#use-multiple-language-worker-processes).

> [!NOTE]
>  Obwohl diese Empfehlungen sowohl für über HTTP ausgelöste als auch für nicht über HTTP ausgelöste Funktionen gelten, müssen Sie möglicherweise weitere triggerspezifische Konfigurationen für nicht über HTTP ausgelöste Funktionen anpassen, um die erwartete Leistung Ihrer Funktions-Apps zu erzielen. Weitere Informationen dazu finden Sie in [diesem Artikel](functions-best-practices.md).


#### <a name="managing-event-loop"></a>Verwalten der Ereignisschleife

Sie sollten asyncio-kompatible Drittanbieterbibliotheken verwenden. Wenn keine Drittanbieterbibliothek Ihre Anforderungen erfüllt, können Sie die Ereignisschleifen auch in Azure Functions verwalten. Durch Ereignisschleifen erhalten Sie mehr Flexibilität bei der Verwaltung von Computeressourcen, und Sie können synchrone E/A-Bibliotheken mit Coroutinen umschließen.

Es gibt eine Vielzahl nützlicher offizieller Python-Dokumente zu [Coroutinen und Tasks](https://docs.python.org/3/library/asyncio-task.html) sowie zu [Ereignisschleifen](https://docs.python.org/3.8/library/asyncio-eventloop.html) unter Verwendung der integrierten **asyncio**-Bibliothek.

Ein Beispiel ist die folgende [requests](https://github.com/psf/requests)-Bibliothek: Der folgende Codeausschnitt verwendet die **asyncio**-Bibliothek, um die `requests.get()`-Methode mit einer Coroutine zu umschließen und mehrere Webanforderungen an SAMPLE_URL gleichzeitig ausführen zu können.


```python
import asyncio
import json
import logging

import azure.functions as func
from time import time
from requests import get, Response


async def invoke_get_request(eventloop: asyncio.AbstractEventLoop) -> Response:
    # Wrap requests.get function into a coroutine
    single_result = await eventloop.run_in_executor(
        None,  # using the default executor
        get,  # each task call invoke_get_request
        'SAMPLE_URL'  # the url to be passed into the requests.get function
    )
    return single_result

async def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    eventloop = asyncio.get_event_loop()

    # Create 10 tasks for requests.get synchronous call
    tasks = [
        asyncio.create_task(
            invoke_get_request(eventloop)
        ) for _ in range(10)
    ]

    done_tasks, _ = await asyncio.wait(tasks)
    status_codes = [d.result().status_code for d in done_tasks]

    return func.HttpResponse(body=json.dumps(status_codes),
                             mimetype='application/json')
```
#### <a name="vertical-scaling"></a>Vertikale Skalierung
Weitere Verarbeitungseinheiten – insbesondere in CPU-gebundenen Vorgängen – können Sie möglicherweise durch ein Upgrade auf einen Premium-Plan mit höheren Spezifikationen erhalten. Wenn Ihnen mehr Verarbeitungseinheiten zur Verfügung stehen, können Sie die Anzahl von Workerprozessen an die Anzahl verfügbarer Kerne anpassen und auf diese Weise ein höheres Maß an Parallelität erzielen. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Python-Entwicklung für Azure Functions finden Sie in den folgenden Ressourcen:

* [Python-Entwicklerhandbuch für Azure Functions](functions-reference-python.md)
* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [Entwicklerreferenz zu Azure Functions](functions-reference.md)

