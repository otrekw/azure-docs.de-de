---
title: SignalR Service-Auslöserbindung für Azure Functions
description: Erfahren Sie, wie Sie SignalR Service-Nachrichten aus Azure Functions senden.
author: chenyl
ms.topic: reference
ms.custom: devx-track-csharp
ms.date: 05/11/2020
ms.author: chenyl
ms.openlocfilehash: 2482a26987ec142880acc51bf470d844655b6e3f
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97763511"
---
# <a name="signalr-service-trigger-binding-for-azure-functions"></a>SignalR Service-Auslöserbindung für Azure Functions

Verwenden Sie die *SignalR*-Auslöserbindung, um auf Nachrichten zu antworten, die von Azure SignalR Service gesendet werden. Beim Auslösen der Funktion werden die an die Funktion übergebenen Nachrichten als JSON-Objekt analysiert.

Im serverlosen Modus des SignalR Service verwendet SignalR Service das Feature [Upstream](../azure-signalr/concept-upstream.md), um Nachrichten vom Client an die Funktions-App zu senden. Und die Funktions-App verwendet die SignalR Service-Triggerbindung, um diese Nachrichten zu verarbeiten. Die allgemeine Architektur wird unten dargestellt: :::image type="content" source="media/functions-bindings-signalr-service/signalr-trigger.png" alt-text="SignalR-Triggerarchitektur":::

Informationen zu Setup- und Konfigurationsdetails finden Sie in der [Übersicht](functions-bindings-signalr-service.md).

## <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine Funktion, die eine Nachricht über die Auslöserbindung empfängt und die Nachricht protokolliert.

# <a name="c"></a>[C#](#tab/csharp)

Die SignalR Service-Auslöserbindung für C# hat zwei Programmiermodelle: das klassenbasierte Modell und das herkömmliche Modell. Das klassenbasierte Modell kann eine einheitliche SignalR-Server-seitige Programmiererfahrung bieten. Das herkömmliche Modell bietet mehr Flexibilität und ist mit anderen Funktionsbindungen vergleichbar.

### <a name="with-class-based-model"></a>Klassenbasiertes Modell

Weitere Informationen finden Sie unter [Klassenbasiertes Modell](../azure-signalr/signalr-concept-serverless-development-config.md#class-based-model).

```cs
public class SignalRTestHub : ServerlessHub
{
    [FunctionName("SignalRTest")]
    public async Task SendMessage([SignalRTrigger]InvocationContext invocationContext, string message, ILogger logger)
    {
        logger.LogInformation($"Receive {message} from {invocationContext.ConnectionId}.");
    }
}
```

### <a name="with-traditional-model"></a>Herkömmliches Modell

Das herkömmliche Modell folgt der mit C# entwickelten Azure Functions-Konvention. Wenn Sie damit nicht vertraut sind, lesen Sie die [Dokumentation](./functions-dotnet-class-library.md), um mehr darüber zu erfahren.

```cs
[FunctionName("SignalRTest")]
public static async Task Run([SignalRTrigger("SignalRTest", "messages", "SendMessage", parameterNames: new string[] {"message"})]InvocationContext invocationContext, string message, ILogger logger)
{
    logger.LogInformation($"Receive {message} from {invocationContext.ConnectionId}.");
}
```

#### <a name="use-attribute-signalrparameter-to-simplify-parameternames"></a>Verwenden des Attributs `[SignalRParameter]` zur Vereinfachung von `ParameterNames`

Da die Verwendung von `ParameterNames` etwas umständlich ist, wird zur Erreichung desselben Zwecks `SignalRParameter` bereitgestellt.

```cs
[FunctionName("SignalRTest")]
public static async Task Run([SignalRTrigger("SignalRTest", "messages", "SendMessage")]InvocationContext invocationContext, [SignalRParameter]string message, ILogger logger)
{
    logger.LogInformation($"Receive {message} from {invocationContext.ConnectionId}.");
}
```

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
    "type": "signalRTrigger",
    "name": "invocation",
    "hubName": "SignalRTest",
    "category": "messages",
    "event": "SendMessage",
    "parameterNames": [
        "message"
    ],
    "direction": "in"
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using System;
using Microsoft.Azure.WebJobs.Extensions.SignalRService;
using Microsoft.Extensions.Logging;

public static void Run(InvocationContext invocation, string message, ILogger logger)
{
    logger.LogInformation($"Receive {message} from {invocationContext.ConnectionId}.");
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
    "type": "signalRTrigger",
    "name": "invocation",
    "hubName": "SignalRTest",
    "category": "messages",
    "event": "SendMessage",
    "parameterNames": [
        "message"
    ],
    "direction": "in"
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, invocation) {
    context.log(`Receive ${context.bindingData.message} from ${invocation.ConnectionId}.`)
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
    "type": "signalRTrigger",
    "name": "invocation",
    "hubName": "SignalRTest",
    "category": "messages",
    "event": "SendMessage",
    "parameterNames": [
        "message"
    ],
    "direction": "in"
}
```

Dies ist der Python-Code:

```python
import logging
import json
import azure.functions as func

def main(invocation) -> None:
    invocation_json = json.loads(invocation)
    logging.info("Receive {0} from {1}".format(invocation_json['Arguments'][0], invocation_json['ConnectionId']))
```

---

## <a name="configuration"></a>Konfiguration

### <a name="signalrtrigger"></a>SignalRTrigger

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `SignalRTrigger` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type**| – | Muss auf `SignalRTrigger` festgelegt sein.|
|**direction**| – | Muss auf `in` festgelegt sein.|
|**name**| – | Im Funktionscode für das Kontextobjekt des Auslöseraufrufs verwendeter Variablenname. |
|**hubName**|**HubName**| Dieser Wert muss auf den Namen des SignalR-Hubs festgelegt werden, damit die Funktion ausgelöst werden kann.|
|**category**|**Kategorie**| Dieser Wert muss als die Kategorie von Nachrichten festgelegt werden, damit die Funktion ausgelöst wird. Die Kategorie kann einen der folgenden Werte aufweisen: <ul><li>**connections**: Einschließlich der Ereignisse *connected* und *disconnected*</li><li>**messages**: Einschließlich aller anderen Ereignisse außer denen in der Kategorie *connections*</li></ul> |
|**event**|**Event**| Dieser Wert muss als das Ereignis von Nachrichten festgelegt werden, damit die Funktion ausgelöst wird. Bei der Kategorie *messages* ist das Ereignis das *Ziel* in der [Aufrufnachricht](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocation-message-encoding), die Clients senden. Für die Kategorie *connections* wird nur *connected* und *disconnected* verwendet. |
|**parameterNames**|**ParameterNames**| (Optional) Eine Liste von Namen, die an die Parameter gebunden werden. |
|**connectionStringSetting**|**ConnectionStringSetting**| Der Name der Anwendungseinstellung, die die Verbindungszeichenfolge des SignalR-Diensts enthält (standardmäßig „AzureSignalRConnectionString“) |

## <a name="payload"></a>Nutzlast

Der Triggereingabetyp wird entweder als `InvocationContext` oder als benutzerdefinierter Typ deklariert. Wenn Sie `InvocationContext` wählen, erhalten Sie Vollzugriff auf den Inhalt der Anforderung. Bei einem benutzerdefinierten Typ versucht die Laufzeit, den JSON-Anforderungstext zu analysieren, um die Objekteigenschaften festzulegen.

### <a name="invocationcontext"></a>InvocationContext

InvocationContext enthält den gesamten Inhalt der von SignalR Service gesendeten Nachricht.

|Eigenschaft in InvocationContext | BESCHREIBUNG|
|------------------------------|------------|
|Argumente| Verfügbar für die Kategorie *messages*. Enthält *Argumente* in der [Aufrufnachricht](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md#invocation-message-encoding).|
|Fehler| Verfügbar für das Ereignis *disconnected*. Kann leer sein, wenn die Verbindung ohne Fehler geschlossen wurde, oder enthält die Fehlermeldungen.|
|Hub| Der Name des Hubs, zu dem die Nachricht gehört.|
|Category| Die Kategorie der Nachricht.|
|Ereignis| Das Ereignis der Nachricht.|
|ConnectionId| Die Verbindungs-ID des Clients, der die Nachricht sendet.|
|UserId| Die Benutzeridentität des Clients, der die Nachricht sendet.|
|Header| Die Header der Anforderung.|
|Abfrage| Die Abfrage der Anforderung, wenn sich Clients mit dem Dienst verbinden.|
|Ansprüche| Die Ansprüche des Clients.|

## <a name="using-parameternames"></a>Verwenden von `ParameterNames`

Die Eigenschaft `ParameterNames` in `SignalRTrigger` erlaubt Ihnen, Argumente von Aufrufnachrichten an die Parameter von Funktionen zu binden. Der von Ihnen definierte Name kann als Teil der [Bindungsausdrücke](../azure-functions/functions-bindings-expressions-patterns.md) in anderen Bindungen oder als Parameter im Code verwendet werden. Dadurch erhalten Sie einen bequemeren Zugriff auf die Argumente von `InvocationContext`.

Angenommen, Sie haben einen JavaScript-SignalR-Client, der versucht, die Methode `broadcast` in Azure Functions mit zwei Argumenten (`message1`, `message2`) aufzurufen.

```javascript
await connection.invoke("broadcast", message1, message2);
```

Nachdem Sie `parameterNames` festgelegt haben, entspricht der von Ihnen definierte Name den Argumenten, die auf der Clientseite gesendet werden. 

```cs
[SignalRTrigger(parameterNames: new string[] {"arg1, arg2"})]
```

Anschließend enthält `arg1` den Inhalt von `message1` und `arg2` den Inhalt von `message2`.


### <a name="remarks"></a>Bemerkungen

Für die Parameterbindung ist die Reihenfolge wichtig. Wenn Sie `ParameterNames` verwenden, entspricht die Reihenfolge in `ParameterNames` der Reihenfolge der Argumente, die Sie im Client aufrufen. Wenn Sie das Attribut `[SignalRParameter]` in C# verwenden, stimmt die Reihenfolge der Argumente in Azure Functions-Methoden mit der Reihenfolge der Argumente in Clients überein.

`ParameterNames` und das Attribut `[SignalRParameter]` **können nicht** gleichzeitig verwendet werden, da Sie sonst eine Ausnahme erhalten.

## <a name="signalr-service-integration"></a>SignalR Service-Integration

Der signalr-Dienst benötigt eine URL für den Zugriff auf die Funktions-App, wenn Sie die SignalR Service-Triggerbindung verwenden. Die URL sollte in **Upstreameinstellungen** auf der SignalR Service-Seite konfiguriert werden. 

:::image type="content" source="../azure-signalr/media/concept-upstream/upstream-portal.png" alt-text="Upstreamportal":::

Bei Verwendung des SignalR Service-Triggers kann die URL einfach und wie unten dargestellt formatiert sein:

```http
<Function_App_URL>/runtime/webhooks/signalr?code=<API_KEY>
```

`Function_App_URL` finden Sie auf der Übersichtsseite der Funktions-App, `API_KEY` wird von der Azure-Funktion generiert. Sie können den `API_KEY` von `signalr_extension` auf dem Blatt **App-Schlüssel** der Funktions-App abrufen.
:::image type="content" source="media/functions-bindings-signalr-service/signalr-keys.png" alt-text="API-Schlüssel":::

Wenn Sie mehrere Funktions-Apps in Verbindung mit einem SignalR Service verwenden möchten, kann der Upstream auch komplexe Routingregeln unterstützen. Weitere Informationen finden Sie unter [Upstreameinstellungen](../azure-signalr/concept-upstream.md).

## <a name="step-by-step-sample"></a>Schritt-für-Schritt-Beispiel

Sie können das Beispiel in GitHub befolgen, um einen Chatraum für die Funktions-App mit der SignalR Service-Triggerbindung und der Upstreamfunktion bereitzustellen: [Beispiel für bidirektionalen Chatraum](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/BidirectionChat)

## <a name="next-steps"></a>Nächste Schritte

* [Azure Functions-Entwicklung und -Konfiguration mit Azure SignalR Service](../azure-signalr/signalr-concept-serverless-development-config.md)
* [Beispiel einer SignalR Service-Auslöserbindung](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/BidirectionChat)
