---
title: Bindungen des SignalR-Diensts für Azure Functions
description: Erfahren Sie, wie Bindungen des SignalR-Diensts in Azure Functions verwendet werden.
author: craigshoemaker
ms.topic: reference
ms.date: 02/28/2019
ms.author: cshoe
ms.openlocfilehash: 208b5462efeb579e30550824bd7ba931db1825b2
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74925643"
---
# <a name="signalr-service-bindings-for-azure-functions"></a>Bindungen des SignalR-Diensts für Azure Functions

In diesem Artikel wird erläutert, wie mit dem [Azure SignalR-Dienst](https://azure.microsoft.com/services/signalr-service/) verbundene Clients mithilfe von Bindungen des SignalR-Diensts in Azure Functions authentifiziert und Nachrichten in Echtzeit an sie gesendet werden. Azure Functions unterstützt Eingabe- und Ausgabebindungen für den SignalR-Dienst.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x-and-higher"></a>Pakete: Functions 2.x oder höher

Die SignalR Service-Bindungen werden im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.SignalRService](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService) (Version 1.*) bereitgestellt. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-functions-signalrservice-extension](https://github.com/Azure/azure-functions-signalrservice-extension).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2-manual-portal.md)]

Ausführliche Informationen zum Konfigurieren und Verwenden von SignalR Service und Azure Functions finden Sie unter [Azure Functions-Entwicklung und -Konfiguration mit Azure SignalR Service](../azure-signalr/signalr-concept-serverless-development-config.md).

### <a name="annotations-library-java-only"></a>Anmerkungsbibliothek (nur Java)

Um die SignalR Service-Anmerkungen in Java-Funktionen verwenden zu können, müssen Sie in „pom.xml“ dem Artefakt *azure-functions-java-library-signalr* (Version 1.0 oder höher) eine Abhängigkeit hinzufügen.

```xml
<dependency>
    <groupId>com.microsoft.azure.functions</groupId>
    <artifactId>azure-functions-java-library-signalr</artifactId>
    <version>1.0.0</version>
</dependency>
```

## <a name="input"></a>Eingabe

Bevor ein Client eine Verbindung mit dem Azure SignalR-Dienst herstellen kann, muss er die Endpunkt-URL des Diensts und ein gültiges Zugriffstoken abrufen. Die *SignalRConnectionInfo*-Eingabebindung erzeugt die Endpunkt-URL des SignalR-Diensts und ein gültiges Token. Beide werden verwendet, um die Verbindung mit dem Dienst herzustellen. Da das Token zeitlich begrenzt ist und für die Authentifizierung eines bestimmten Benutzers gegenüber einer Verbindung verwendet werden kann, sollten Sie das Token nicht zwischenspeichern oder es für mehrere Clients verwenden. Ein HTTP-Trigger, der diese Bindung verwendet, kann von Clients dazu verwendet werden, die Verbindungsinformationen abzurufen.

Weitere Informationen zur Erstellung einer Aushandlungsfunktion mithilfe dieser Bindung, die von einem SignalR-Client-SDK genutzt werden kann, finden Sie in der SignalR Service-Konzeptdokumentation im Artikel [Azure Functions-Entwicklung und -Konfiguration mit Azure SignalR Service](../azure-signalr/signalr-concept-serverless-development-config.md).

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die SignalR-Verbindungsinformationen mithilfe der Eingabebindung erwirbt und sie über HTTP zurückgibt.

```cs
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req,
    [SignalRConnectionInfo(HubName = "chat")]SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

# <a name="c-scripttabcsharp-script"></a>[C#-Skript](#tab/csharp-script)

Das folgende Beispiel zeigt eine SignalR-Verbindungsinformations-Eingabebindung in der Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet, um die Verbindungsinformationen zurückzugeben.

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static SignalRConnectionInfo Run(HttpRequest req, SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Das folgende Beispiel zeigt eine SignalR-Verbindungsinfo-Eingabebindung in einer Datei *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet, um die Verbindungsinformationen zurückzugeben.

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = async function (context, req, connectionInfo) {
    context.res.body = connectionInfo;
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Das folgende Beispiel zeigt eine SignalR-Verbindungsinformations-Eingabebindung in der Datei *function.json* sowie eine [Python-Funktion](functions-reference-python.md), die die Bindung verwendet, um die Verbindungsinformationen zurückzugeben.

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Dies ist der Python-Code:

```python
def main(req: func.HttpRequest, connectionInfoJson: str) -> func.HttpResponse:
    return func.HttpResponse(
        connectionInfoJson,
        status_code=200,
        headers={
            'Content-type': 'application/json'
        }
    )
```

# <a name="javatabjava"></a>[Java](#tab/java)

Das folgende Beispiel zeigt eine [Java-Funktion](functions-reference-java.md), die SignalR-Verbindungsinformationen mithilfe der Eingabebindung abruft und sie über HTTP zurückgibt.

```java
@FunctionName("negotiate")
public SignalRConnectionInfo negotiate(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> req,
        @SignalRConnectionInfoInput(
            name = "connectionInfo",
            hubName = "chat") SignalRConnectionInfo connectionInfo) {
    return connectionInfo;
}
```

---

### <a name="authenticated-tokens"></a>Authentifizierte Token

Wenn die Funktion von einem authentifizierten Client ausgelöst wird, können Sie dem erzeugten Token einen Benutzer-ID-Anspruch hinzufügen. Mithilfe der [App Service-Authentifizierung](../app-service/overview-authentication-authorization.md) können Sie einer Funktions-App problemlos eine Authentifizierung hinzufügen.

App Service-Authentifizierung legt HTTP-Header mit den Namen `x-ms-client-principal-id` und `x-ms-client-principal-name` fest, die die Clientprinzipal-ID bzw. den Namen des authentifizierten Benutzers enthalten.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Sie können die `UserId`-Eigenschaft der Bindung mithilfe eines [Bindungsausdrucks](./functions-bindings-expressions-patterns.md) auf den Wert eines der beiden Header festlegen: `{headers.x-ms-client-principal-id}` oder `{headers.x-ms-client-principal-name}`.

```cs
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req, 
    [SignalRConnectionInfo
        (HubName = "chat", UserId = "{headers.x-ms-client-principal-id}")]
        SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier claim set to the authenticated user
    return connectionInfo;
}
```

# <a name="c-scripttabcsharp-script"></a>[C#-Skript](#tab/csharp-script)

Sie können die `userId`-Eigenschaft der Bindung mithilfe eines [Bindungsausdrucks](./functions-bindings-expressions-patterns.md) auf den Wert eines der beiden Header festlegen: `{headers.x-ms-client-principal-id}` oder `{headers.x-ms-client-principal-name}`.

Beispiel für „function.json“:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "userId": "{headers.x-ms-client-principal-id}",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static SignalRConnectionInfo Run(HttpRequest req, SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier
    // claim set to the authenticated user
    return connectionInfo;
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Sie können die `userId`-Eigenschaft der Bindung mithilfe eines [Bindungsausdrucks](./functions-bindings-expressions-patterns.md) auf den Wert eines der beiden Header festlegen: `{headers.x-ms-client-principal-id}` oder `{headers.x-ms-client-principal-name}`.

Beispiel für „function.json“:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "userId": "{headers.x-ms-client-principal-id}",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = async function (context, req, connectionInfo) {
    // connectionInfo contains an access key token with a name identifier
    // claim set to the authenticated user
    context.res.body = connectionInfo;
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Sie können die `userId`-Eigenschaft der Bindung mithilfe eines [Bindungsausdrucks](./functions-bindings-expressions-patterns.md) auf den Wert eines der beiden Header festlegen: `{headers.x-ms-client-principal-id}` oder `{headers.x-ms-client-principal-name}`.

Beispiel für „function.json“:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "userId": "{headers.x-ms-client-principal-id}",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Dies ist der Python-Code:

```python
def main(req: func.HttpRequest, connectionInfoJson: str) -> func.HttpResponse:
    # connectionInfo contains an access key token with a name identifier
    # claim set to the authenticated user
    return func.HttpResponse(
        connectionInfoJson,
        status_code=200,
        headers={
            'Content-type': 'application/json'
        }
    )
```

# <a name="javatabjava"></a>[Java](#tab/java)

Sie können die `userId`-Eigenschaft der Bindung mithilfe eines [Bindungsausdrucks](./functions-bindings-expressions-patterns.md) auf den Wert eines der beiden Header festlegen: `{headers.x-ms-client-principal-id}` oder `{headers.x-ms-client-principal-name}`.

```java
@FunctionName("negotiate")
public SignalRConnectionInfo negotiate(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> req,
        @SignalRConnectionInfoInput(
            name = "connectionInfo",
            hubName = "chat",
            userId = "{headers.x-ms-client-principal-id}") SignalRConnectionInfo connectionInfo) {
    return connectionInfo;
}
```

---

## <a name="output"></a>Output

Verwenden Sie die *SignalR*-Ausgabebindung, um eine oder mehrere Nachrichten mithilfe des Azure SignalR-Diensts zu senden. Sie können eine Nachricht als Broadcast an alle verbundenen Clients senden oder den Broadcast auf verbundene Clients beschränken, die für einem bestimmten Benutzer authentifiziert wurden.

Darüber hinaus können Sie damit die Gruppen verwalten, denen ein Benutzer angehört.

### <a name="broadcast-to-all-clients"></a>Broadcast an alle Clients

Das folgende Beispiel zeigt eine Funktion, die eine Nachricht mithilfe der Ausgabebindung an alle verbundenen Clients sendet. Die *target*-Eigenschaft entspricht dem Namen der Methode, die auf allen Clients aufgerufen wird. *Arguments* bezeichnet ein Array von null (0) oder mehreren Objekten, die an die Clientmethode übergeben werden sollen.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message, 
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

# <a name="c-scripttabcsharp-script"></a>[C#-Skript](#tab/csharp-script)

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    object message,
    IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
  "type": "signalR",
  "name": "out_message",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Dies ist der Python-Code:

```python
def main(req: func.HttpRequest, out_message: func.Out[str]) -> func.HttpResponse:
    message = req.get_json()
    out_message.set(json.dumps({
        'target': 'newMessage',
        'arguments': [ message ]
    }))
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

---

### <a name="send-to-a-user"></a>Senden an einen Benutzer

Sie können eine Nachricht ausschließlich an Verbindungen senden, die für einen Benutzer authentifiziert wurden, indem Sie die *Benutzer-ID* in der SignalR-Nachricht festlegen.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message, 
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            // the message will only be sent to this user ID
            UserId = "userId1",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

# <a name="c-scripttabcsharp-script"></a>[C#-Skript](#tab/csharp-script)

Beispiel für „function.json“:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    object message,
    IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            // the message will only be sent to this user ID
            UserId = "userId1",
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Beispiel für „function.json“:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to this user ID
        "userId": "userId1",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
  "type": "signalR",
  "name": "out_message",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Dies ist der Python-Code:

```python
def main(req: func.HttpRequest, out_message: func.Out[str]) -> func.HttpResponse:
    message = req.get_json()
    out_message.set(json.dumps({
        #message will only be sent to this user ID
        'userId': 'userId1',
        'target': 'newMessage',
        'arguments': [ message ]
    }))
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.userId = "userId1";
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

---

### <a name="send-to-a-group"></a>Senden an eine Gruppe

Sie können eine Nachricht ausschließlich an Verbindungen senden, die einer Gruppe hinzugefügt wurden, indem Sie den *Gruppennamen* in der SignalR-Nachricht festlegen.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message,
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage
        {
            // the message will be sent to the group with this name
            GroupName = "myGroup",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

# <a name="c-scripttabcsharp-script"></a>[C#-Skript](#tab/csharp-script)

Beispiel für „function.json“:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    object message,
    IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            // the message will be sent to the group with this name
            GroupName = "myGroup",
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Beispiel für „function.json“:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to this group
        "groupName": "myGroup",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Die Bindungsdaten in der Datei *function.json*:

Beispiel für „function.json“:

```json
{
  "type": "signalR",
  "name": "out_message",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Dies ist der Python-Code:

```python
def main(req: func.HttpRequest, out_message: func.Out[str]) -> func.HttpResponse:
    message = req.get_json()
    out_message.set(json.dumps({
        #message will only be sent to this group
        'groupName': 'myGroup',
        'target': 'newMessage',
        'arguments': [ message ]
    }))
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.groupName = "myGroup";
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

---

### <a name="group-management"></a>Gruppenverwaltung

SignalR Service ermöglicht das Hinzufügen von Benutzern zu Gruppen. Nachrichten können dann an eine Gruppe gesendet werden. Sie können die Ausgabebindung `SignalR` verwenden, um die Gruppenmitgliedschaft eines Benutzers zu verwalten.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

#### <a name="add-user-to-a-group"></a>Hinzufügen eines Benutzers zu einer Gruppe

Im folgenden Beispiel wird ein Benutzer einer Gruppe hinzugefügt:

```csharp
[FunctionName("addToGroup")]
public static Task AddToGroup(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequest req,
    ClaimsPrincipal claimsPrincipal,
    [SignalR(HubName = "chat")]
        IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    var userIdClaim = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier);
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userIdClaim.Value,
            GroupName = "myGroup",
            Action = GroupAction.Add
        });
}
```

#### <a name="remove-user-from-a-group"></a>Entfernen eines Benutzers aus einer Gruppe

Im folgenden Beispiel wird ein Benutzer aus einer Gruppe entfernt:

```csharp
[FunctionName("removeFromGroup")]
public static Task RemoveFromGroup(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequest req,
    ClaimsPrincipal claimsPrincipal,
    [SignalR(HubName = "chat")]
        IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    var userIdClaim = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier);
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userIdClaim.Value,
            GroupName = "myGroup",
            Action = GroupAction.Remove
        });
}
```

> [!NOTE]
> Um `ClaimsPrincipal` ordnungsgemäße zu binden, müssen Sie die Authentifizierungseinstellungen in Azure Functions konfiguriert haben.

# <a name="c-scripttabcsharp-script"></a>[C#-Skript](#tab/csharp-script)

#### <a name="add-user-to-a-group"></a>Hinzufügen eines Benutzers zu einer Gruppe

Im folgenden Beispiel wird ein Benutzer einer Gruppe hinzugefügt:

Beispiel für *function.json*

```json
{
    "type": "signalR",
    "name": "signalRGroupActions",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*Run.csx*

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    HttpRequest req,
    ClaimsPrincipal claimsPrincipal,
    IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    var userIdClaim = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier);
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userIdClaim.Value,
            GroupName = "myGroup",
            Action = GroupAction.Add
        });
}
```

#### <a name="remove-user-from-a-group"></a>Entfernen eines Benutzers aus einer Gruppe

Im folgenden Beispiel wird ein Benutzer aus einer Gruppe entfernt:

Beispiel für *function.json*

```json
{
    "type": "signalR",
    "name": "signalRGroupActions",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*Run.csx*

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    HttpRequest req,
    ClaimsPrincipal claimsPrincipal,
    IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    var userIdClaim = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier);
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userIdClaim.Value,
            GroupName = "myGroup",
            Action = GroupAction.Remove
        });
}
```

> [!NOTE]
> Um `ClaimsPrincipal` ordnungsgemäße zu binden, müssen Sie die Authentifizierungseinstellungen in Azure Functions konfiguriert haben.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

#### <a name="add-user-to-a-group"></a>Hinzufügen eines Benutzers zu einer Gruppe

Im folgenden Beispiel wird ein Benutzer einer Gruppe hinzugefügt:

Beispiel für *function.json*

```json
{
    "type": "signalR",
    "name": "signalRGroupActions",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*index.js*

```javascript
module.exports = async function (context, req) {
  context.bindings.signalRGroupActions = [{
    "userId": req.query.userId,
    "groupName": "myGroup",
    "action": "add"
  }];
};
```

#### <a name="remove-user-from-a-group"></a>Entfernen eines Benutzers aus einer Gruppe

Im folgenden Beispiel wird ein Benutzer aus einer Gruppe entfernt:

Beispiel für *function.json*

```json
{
    "type": "signalR",
    "name": "signalRGroupActions",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*index.js*

```javascript
module.exports = async function (context, req) {
  context.bindings.signalRGroupActions = [{
    "userId": req.query.userId,
    "groupName": "myGroup",
    "action": "remove"
  }];
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

#### <a name="add-user-to-a-group"></a>Hinzufügen eines Benutzers zu einer Gruppe

Im folgenden Beispiel wird ein Benutzer einer Gruppe hinzugefügt:

Beispiel für *function.json*

```json
{
    "type": "signalR",
    "name": "action",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*\_\_init.py__*

```python
def main(req: func.HttpRequest, action: func.Out[str]) -> func.HttpResponse:
    action.set(json.dumps({
        'userId': 'userId1',
        'groupName': 'myGroup',
        'action': 'add'
    }))
```

#### <a name="remove-user-from-a-group"></a>Entfernen eines Benutzers aus einer Gruppe

Im folgenden Beispiel wird ein Benutzer aus einer Gruppe entfernt:

Beispiel für *function.json*

```json
{
    "type": "signalR",
    "name": "action",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*\_\_init.py__*

```python
def main(req: func.HttpRequest, action: func.Out[str]) -> func.HttpResponse:
    action.set(json.dumps({
        'userId': 'userId1',
        'groupName': 'myGroup',
        'action': 'remove'
    }))
```

# <a name="javatabjava"></a>[Java](#tab/java)

#### <a name="add-user-to-a-group"></a>Hinzufügen eines Benutzers zu einer Gruppe

Im folgenden Beispiel wird ein Benutzer einer Gruppe hinzugefügt:

```java
@FunctionName("addToGroup")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRGroupAction addToGroup(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req,
        @BindingName("userId") String userId) {

    SignalRGroupAction groupAction = new SignalRGroupAction();
    groupAction.action = "add";
    groupAction.userId = userId;
    groupAction.groupName = "myGroup";
    return action;
}
```

#### <a name="remove-user-from-a-group"></a>Entfernen eines Benutzers aus einer Gruppe

Im folgenden Beispiel wird ein Benutzer aus einer Gruppe entfernt:

```java
@FunctionName("removeFromGroup")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRGroupAction removeFromGroup(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req,
        @BindingName("userId") String userId) {

    SignalRGroupAction groupAction = new SignalRGroupAction();
    groupAction.action = "remove";
    groupAction.userId = userId;
    groupAction.groupName = "myGroup";
    return action;
}
```

---

## <a name="configuration"></a>Konfiguration

### <a name="signalrconnectioninfo"></a>SignalRConnectionInfo

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `SignalRConnectionInfo` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type**|| Muss auf `signalRConnectionInfo` festgelegt sein.|
|**direction**|| Muss auf `in` festgelegt sein.|
|**name**|| Variablenname, der im Funktionscode für das Verbindungsinfoobjekt verwendet wird. |
|**hubName**|**HubName**| Dieser Wert muss auf den Namen des SignalR-Hubs festgelegt werden, für den die Verbindungsinformationen erzeugt werden.|
|**userId**|**UserId**| Optional: Der Wert des Benutzer-ID-Anspruchs, der im Zugriffsschlüsseltoken festgelegt werden soll. |
|**connectionStringSetting**|**ConnectionStringSetting**| Der Name der Anwendungseinstellung, die die Verbindungszeichenfolge des SignalR-Diensts enthält (standardmäßig „AzureSignalRConnectionString“) |

### <a name="signalr"></a>SignalR

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `SignalR` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type**|| Muss auf `signalR` festgelegt sein.|
|**direction**|| Muss auf `out` festgelegt sein.|
|**name**|| Variablenname, der im Funktionscode für das Verbindungsinfoobjekt verwendet wird. |
|**hubName**|**HubName**| Dieser Wert muss auf den Namen des SignalR-Hubs festgelegt werden, für den die Verbindungsinformationen erzeugt werden.|
|**connectionStringSetting**|**ConnectionStringSetting**| Der Name der Anwendungseinstellung, die die Verbindungszeichenfolge des SignalR-Diensts enthält (standardmäßig „AzureSignalRConnectionString“) |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Azure Functions-Entwicklung und -Konfiguration mit Azure SignalR Service](../azure-signalr/signalr-concept-serverless-development-config.md)
