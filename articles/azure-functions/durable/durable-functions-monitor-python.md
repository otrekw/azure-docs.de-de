---
title: Monitore in Durable Functions (Python) – Azure
description: Hier erfahren Sie, wie Sie mithilfe der Durable Functions-Erweiterung für Azure Functions (Python) einen Statusmonitor implementieren.
author: davidmrdavid
ms.topic: conceptual
ms.date: 12/02/2020
ms.author: azfuncdf
ms.openlocfilehash: 62b3c9bb1c6fd53d9f11227a9d7e774d56859d04
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102434762"
---
# <a name="monitor-scenario-in-durable-functions---github-issue-monitoring-sample"></a>Überwachungsszenario in Durable Functions: Beispiel für die Überwachung von Issues in GitHub

Das Monitormuster bezieht sich auf einen flexiblen wiederkehrenden Prozess in einem Workflow – wenn beispielsweise bestimmte Elemente solange abgerufen werden, bis bestimmte Bedingungen erfüllt sind. In diesem Artikel wird ein Beispiel beschrieben, in dem Durable Functions zum Implementieren der Überwachung verwendet wird.

## <a name="prerequisites"></a>Voraussetzungen

* [Schließen Sie den Schnellstartartikel ab.](quickstart-python-vscode.md)
* [Klonen Sie das Beispielprojekt auf GitHub, oder laden Sie es herunter.](https://github.com/Azure/azure-functions-durable-python/tree/main/samples/)


## <a name="scenario-overview"></a>Übersicht über das Szenario

Dieses Beispiel überwacht die Anzahl von Issues in einem GitHub-Repository und warnt den Benutzer, wenn mehr als drei offene Issues vorhanden sind. Sie könnten auch eine reguläre, durch einen Timer ausgelöste Funktion verwenden, um die Anzahl geöffneter Issues in regelmäßigen Abständen anzufordern. Ein Problem bei diesem Ansatz ist jedoch die **Lebensdauerverwaltung**. Wenn nur eine einzige Benachrichtigung gesendet werden soll, muss sich der Monitor selbst deaktivieren, nachdem mindestens drei Issues erkannt wurden. Das Überwachungsmuster bietet neben weiteren Vorteilen den Vorzug, dass es die eigene Ausführung beenden kann:

* Monitore werden in Intervallen, nicht nach Zeitplänen ausgeführt: Ein Timertrigger wird jede Stunde *ausgeführt*; ein Monitor *wartet* zwischen Aktionen eine Stunde. Die Aktionen eines Monitors überschneiden sich nicht, sofern dies nicht explizit angegeben wird. Dies kann bei Tasks mit langer Ausführungsdauer wichtig sein.
* Monitore können in dynamischen Intervallen ausgeführt werden: Die Wartezeit kann sich aufgrund einer Bedingung ändern.
* Monitore können beendet werden, wenn eine bestimmte Bedingung erfüllt ist. Sie können auch von einem anderen Prozess beendet werden.
* Für Monitore können Parameter angegeben werden. Das Beispiel zeigt, wie ein und derselbe Prozess zur Repositoryüberwachung auf jedes angeforderte öffentliche GitHub-Repository und jede angeforderte Telefonnummer angewendet werden kann.
* Monitore sind skalierbar. Da jeder Monitor eine Orchestrierungsinstanz ist, können mehrere Monitore erstellt werden, ohne neue Funktionen erstellen oder weiteren Code definieren zu müssen.
* Monitore lassen sich problemlos in größere Workflows integrieren. Ein Monitor kann Teil einer komplexeren Orchestrierungsfunktion oder eine [untergeordnete Orchestrierung](durable-functions-sub-orchestrations.md) sein.

## <a name="configuration"></a>Konfiguration

### <a name="configuring-twilio-integration"></a>Konfigurieren der Twilio-Integration

[!INCLUDE [functions-twilio-integration](../../../includes/functions-twilio-integration.md)]

## <a name="the-functions"></a>Die Funktionen

In diesem Artikel werden die folgenden Funktionen in der Beispiel-App beschrieben:

* `E3_Monitor`: Eine [Orchestratorfunktion](durable-functions-bindings.md#orchestration-trigger), die `E3_TooManyOpenIssues` in regelmäßigen Abständen aufruft. Sie ruft `E3_SendAlert` auf, wenn `E3_TooManyOpenIssues` den Wert `True` zurückgibt.
* `E3_TooManyOpenIssues`: Eine [Aktivitätsfunktion](durable-functions-bindings.md#activity-trigger), die überprüft, ob in einem Repository zu viele offene Issues vorhanden sind. Zu Demonstrationszwecken legen wir hier fest, dass mehr als drei offene Issues als „zu viele“ gelten.
* `E3_SendAlert`: Eine Aktivitätsfunktion, die eine SMS-Nachricht über Twilio sendet.

### <a name="e3_monitor-orchestrator-function"></a>Orchestratorfunktion „E3_Monitor“


Die Funktion **E3_Monitor** verwendet die Standarddatei *function.json* für Orchestratorfunktionen.

[!code-json[Main](~/samples-durable-functions-python/samples/monitor/E3_Monitor/function.json)]

Im Folgenden wird der Code dargestellt, der die Funktion implementiert:

[!code-python[Main](~/samples-durable-functions-python/samples/monitor/E3_Monitor/\_\_init\_\_.py)]


Diese Orchestratorfunktion führt die folgenden Aktionen aus:

1. Sie ruft das zu überwachende *Repository* sowie die *Telefonnummer* ab, an die eine SMS-Benachrichtigung gesendet werden soll.
2. Sie bestimmt den Ablaufzeitpunkt des Monitors. Aus Gründen der Übersichtlichkeit verwendet dieses Beispiel einen hartcodierten Wert.
3. Die Funktion ruft **E3_TooManyOpenIssues** auf, um zu ermitteln, ob im angeforderten Repository zu viele offene Issues vorliegen.
4. Wenn dies der Fall ist, ruft sie **E3_SendAlert** auf, um eine SMS-Benachrichtigung an die angeforderte Telefonnummer zu senden.
5. Sie erstellt einen permanenten Timer, um die Orchestrierung im nächsten Abrufintervall fortzusetzen. Aus Gründen der Übersichtlichkeit verwendet dieses Beispiel einen hartcodierten Wert.
6. Sie wird ausgeführt, bis ^die aktuelle UTC-Zeit den Ablaufzeitpunkt des Monitors überschreitet oder eine SMS-Benachrichtigung gesendet wird.

Mehrere Orchestratorinstanzen können gleichzeitig ausgeführt werden, indem die Orchestratorfunktion mehrmals aufgerufen wird. Das zu überwachende Repository und die Telefonnummer, an die eine SMS-Warnung gesendet werden soll, können angegeben werden. Beachten Sie schließlich, dass die Orchestratorfunktion während des Wartens auf den Timer *nicht* ausgeführt wird, daher werden Ihnen keine Gebühren dafür berechnet.


### <a name="e3_toomanyopenissues-activity-function"></a>Aktivitätsfunktion „E3_TooManyOpenIssues“

Bei den Hilfsaktivitätsfunktionen handelt es sich, wie bei anderen Beispielen, um reguläre Funktionen, die die Triggerbindung `activityTrigger` verwenden. Die Funktion **E3_TooManyOpenIssues** ruft eine Liste der aktuell offenen Issues im Repository an und bestimmt, ob es gemäß Festlegung zu viele sind, in unserem Beispiel also mehr als drei.


Die Datei *function.json* wird wie folgt definiert:

[!code-json[Main](~/samples-durable-functions-python/samples/monitor/E3_TooManyOpenIssues/function.json)]

Hier ist die Implementierung.

[!code-python[Main](~/samples-durable-functions-python/samples/monitor/E3_TooManyOpenIssues/\_\_init\_\_.py)]


### <a name="e3_sendalert-activity-function"></a>Aktivitätsfunktion „E3_SendAlert“

Die Funktion **E3_SendAlert** verwendet die Twilio-Bindung, um eine SMS zu senden, die den Endbenutzer darüber informiert, dass mindestens drei offene Issues auf eine Lösung warten.


Die *function.json* ist einfach:

[!code-json[Main](~/samples-durable-functions-python/samples/monitor/E3_TooManyOpenIssues/function.json)]

Hier sehen Sie den Code, der die SMS-Nachricht sendet:

[!code-python[Main](~/samples-durable-functions-python/samples/monitor/E3_SendAlert/\_\_init\_\_.py)]


## <a name="run-the-sample"></a>Ausführen des Beispiels

Sie benötigen ein [GitHub](https://github.com/)-Konto. Erstellen Sie damit ein temporäres öffentliches Repository, in dem Sie Issue eröffnen können.

Wenn Sie die über HTTP ausgelösten Funktionen verwenden, die im Beispiel enthalten sind, können Sie mit der Orchestrierung beginnen, indem Sie folgende HTTP POST-Anforderung senden:

```
POST https://{host}/orchestrators/E3_Monitor
Content-Length: 77
Content-Type: application/json

{ "repo": "<your github handle>/<a new github repo under your user>", "phone": "+1425XXXXXXX" }
```

Wenn Ihr GitHub-Benutzername beispielsweise `foo` lautet und `bar` der Name Ihres Repositorys ist, muss `"foo/bar"` der Wert für `"repo"` sein.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={SystemKey}
RetryAfter: 10

{"id": "f6893f25acf64df2ab53a35c09d52635", "statusQueryGetUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "sendEventPostUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/raiseEvent/{eventName}?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "terminatePostUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason={text}&taskHub=SampleHubVS&connection=Storage&code={systemKey}"}
```

Die **E3_Monitor**-Instanz startet und fragt das angegebene Repository auf offene Issues ab. Wenn mindestens drei offene Issues gefunden werden, ruft die Instanz eine Aktivitätsfunktion auf, die eine Benachrichtigung sendet. Andernfalls legt sie einen Timer fest. Wenn der Timer abläuft, wird die Orchestrierung fortgesetzt.

Sie können sich die Aktivität der Orchestrierung in den Funktionsprotokollen im Azure Functions-Portal ansehen.

```
[2020-12-04T18:24:30.007Z] Executing 'Functions.HttpStart' (Reason='This function was programmatically 
called via the host APIs.', Id=93772f6b-f4f0-405a-9d7b-be9eb7a38aa6)
[2020-12-04T18:24:30.769Z] Executing 'Functions.E3_Monitor' (Reason='(null)', Id=058e656e-bcb1-418c-95b3-49afcd07bd08)
[2020-12-04T18:24:30.847Z] Started orchestration with ID = '788420bb31754c50acbbc46e12ef4f9c'.
[2020-12-04T18:24:30.932Z] Executed 'Functions.E3_Monitor' (Succeeded, Id=058e656e-bcb1-418c-95b3-49afcd07bd08, Duration=174ms)
[2020-12-04T18:24:30.955Z] Executed 'Functions.HttpStart' (Succeeded, Id=93772f6b-f4f0-405a-9d7b-be9eb7a38aa6, Duration=1028ms)
[2020-12-04T18:24:31.229Z] Executing 'Functions.E3_TooManyOpenIssues' (Reason='(null)', Id=6fd5be5e-7f26-4b0b-98df-c3ac39125da3)
[2020-12-04T18:24:31.772Z] Executed 'Functions.E3_TooManyOpenIssues' (Succeeded, Id=6fd5be5e-7f26-4b0b-98df-c3ac39125da3, Duration=555ms)
[2020-12-04T18:24:40.754Z] Executing 'Functions.E3_Monitor' (Reason='(null)', Id=23915e4c-ddbf-46f9-b3f0-53289ed66082)
[2020-12-04T18:24:40.789Z] Executed 'Functions.E3_Monitor' (Succeeded, Id=23915e4c-ddbf-46f9-b3f0-53289ed66082, Duration=38ms)
(...trimmed...)
```

Die Orchestrierung wird abgeschlossen, sobald das Zeitlimit erreicht ist oder mehr als drei offene Issues erkannt werden. Sie können auch die `terminate`-API in einer anderen Funktion verwenden oder den HTTP POST-Webhook **terminatePostUri** aufrufen, auf den oben in der 202-Antwort verwiesen wird. Ersetzen Sie `{text}` durch den Grund für die vorzeitige Beendigung, um den Webhook zu verwenden. Die HTTP POST-URL sieht in etwa wie folgt aus:

```
POST https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason=Because&taskHub=SampleHubVS&connection=Storage&code={systemKey}
```

## <a name="next-steps"></a>Nächste Schritte

Dieses Beispiel veranschaulichte, wie Sie Durable Functions verwenden, um den Status einer externen Quelle mithilfe von [permanenten Timern](durable-functions-timers.md) und Bedingungslogik zu überwachen. Das nächste Beispiel zeigt, wie externe Ereignisse und [permanente Timer](durable-functions-timers.md) verwendet werden, um Benutzerinteraktionen zu verarbeiten.

> [!div class="nextstepaction"]
> [Ausführen des Beispiels für Benutzerinteraktion](durable-functions-phone-verification.md)
