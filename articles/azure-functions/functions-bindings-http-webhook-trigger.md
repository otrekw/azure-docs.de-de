---
title: HTTP-Trigger in Azure Functions
description: Erfahren Sie, wie Sie eine Azure-Funktion über HTTP aufzurufen.
author: craigshoemaker
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: ce40a46d4c1da627930ef8de8813936b71dcc281
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83648921"
---
# <a name="azure-functions-http-trigger"></a>HTTP-Trigger in Azure Functions

Mit dem HTTP-Trigger können Sie eine Funktion mit einer HTTP-Anforderung aufrufen. Sie können einen HTTP-Trigger zum Erstellen von serverlosen APIs und zum Antworten auf Webhooks verwenden.

Der Standardrückgabewert für eine HTTP-ausgelöste Funktion ist:

- `HTTP 204 No Content` bei leerem Hauptteil in Functions 2.x und höher
- `HTTP 200 OK` bei leerem Hauptteil in Functions 1.x

Um die HTTP-Antwort zu ändern, konfigurieren Sie eine [Ausgabebindung](./functions-bindings-http-webhook-output.md).

Weitere Informationen zu HTTP-Bindungen finden Sie in der [Übersicht](./functions-bindings-http-webhook.md) und [Referenz für die Ausgabebindung](./functions-bindings-http-webhook-output.md).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="example"></a>Beispiel

# <a name="c"></a>[C#](#tab/csharp)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter sucht. Beachten Sie, dass der Rückgabewert für die Ausgabebindung verwendet wird, jedoch kein Attribut vom Rückgabewert erforderlich ist.

```cs
[FunctionName("HttpTriggerCSharp")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] 
    HttpRequest req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    name = name ?? data?.name;

    return name != null
        ? (ActionResult)new OkObjectResult($"Hello, {name}")
        : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
}
```

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Das folgende Beispiel zeigt eine Triggerbindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet. Die Funktion sucht in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter.

Die Datei *function.json* sieht wie folgt aus:

```json
{
    "disabled": false,
    "bindings": [
        {
            "authLevel": "function",
            "name": "req",
            "type": "httpTrigger",
            "direction": "in",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "name": "$return",
            "type": "http",
            "direction": "out"
        }
    ]
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#configuration).

C#-Skriptcode für die Bindung an `HttpRequest`:

```cs
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using Newtonsoft.Json;

public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    name = name ?? data?.name;

    return name != null
        ? (ActionResult)new OkObjectResult($"Hello, {name}")
        : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
}
```

Sie können anstelle von `HttpRequest` auch an ein benutzerdefiniertes Objekt binden. Dieses Objekt wird aus dem Text der Anforderung erstellt und als JSON analysiert. Analog dazu kann ein Typ an die Ausgabebindung der HTTP-Antwort übergeben und zusammen mit dem Statuscode `200` als Antworttext zurückgegeben werden.

```csharp
using System.Net;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static string Run(Person person, ILogger log)
{   
    return person.Name != null
        ? (ActionResult)new OkObjectResult($"Hello, {person.Name}")
        : new BadRequestObjectResult("Please pass an instance of Person.");
}

public class Person {
     public string Name {get; set;}
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Das folgende Beispiel zeigt eine Triggerbindung in einer Datei *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion sucht in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter.

Die Datei *function.json* sieht wie folgt aus:

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Das folgende Beispiel zeigt eine Triggerbindung in einer Datei *function.json* sowie eine [Python-Funktion](functions-reference-python.md), die die Bindung verwendet. Die Funktion sucht in der Abfragezeichenfolge oder im Text der HTTP-Anforderung nach einem `name`-Parameter.

Die Datei *function.json* sieht wie folgt aus:

```json
{
    "scriptFile": "__init__.py",
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#configuration).

Dies ist der Python-Code:

```python
import logging
import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
            "Please pass a name on the query string or in the request body",
            status_code=400
        )
```

# <a name="java"></a>[Java](#tab/java)

* [Lesen eines Parameters aus der Abfragezeichenfolge](#read-parameter-from-the-query-string)
* [Lesen des Hauptteils aus einer POST-Anforderung](#read-body-from-a-post-request)
* [Lesen eines Parameters aus einer Route](#read-parameter-from-a-route)
* [Lesen des POJO-Hauptteils aus einer POST-Anforderung](#read-pojo-body-from-a-post-request)

In den folgenden Beispielen wird die HTTP-Triggerbindung veranschaulicht.

#### <a name="read-parameter-from-the-query-string"></a>Lesen eines Parameters aus der Abfragezeichenfolge

Dieses Beispiel liest einen Parameter namens `id` aus der Abfragezeichenfolge und verwendet ihn zum Erstellen eines JSON-Dokuments mit dem Inhaltstyp `application/json`, das an den Client zurückgegeben wird.

```java
@FunctionName("TriggerStringGet")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", 
            methods = {HttpMethod.GET}, 
            authLevel = AuthorizationLevel.ANONYMOUS)
        HttpRequestMessage<Optional<String>> request,
        final ExecutionContext context) {
    
    // Item list
    context.getLogger().info("GET parameters are: " + request.getQueryParameters());

    // Get named parameter
    String id = request.getQueryParameters().getOrDefault("id", "");

    // Convert and display
    if (id.isEmpty()) {
        return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                        .body("Document not found.")
                        .build();
    } 
    else {
        // return JSON from to the client
        // Generate document
        final String name = "fake_name";
        final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                        "\"description\": \"" + name + "\"}";
        return request.createResponseBuilder(HttpStatus.OK)
                        .header("Content-Type", "application/json")
                        .body(jsonDocument)
                        .build();
    }
}
```

#### <a name="read-body-from-a-post-request"></a>Lesen des Hauptteils aus einer POST-Anforderung

Dieses Beispiel liest den Hauptteil einer POST-Anforderung als `String` und verwendet ihn zum Erstellen eines JSON-Dokuments mit dem Inhaltstyp `application/json`, das an den Client zurückgegeben wird.

```java
    @FunctionName("TriggerStringPost")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Request body is: " + request.getBody().orElse(""));

        // Check request body
        if (!request.getBody().isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from to the client
            // Generate document
            final String body = request.getBody().get();
            final String jsonDocument = "{\"id\":\"123456\", " + 
                                         "\"description\": \"" + body + "\"}";
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(jsonDocument)
                          .build();
        }
    }
```

#### <a name="read-parameter-from-a-route"></a>Lesen eines Parameters aus einer Route

Dieses Beispiel liest einen obligatorischen Parameter namens `id` sowie einen optionalen Parameter `name` aus dem Routenpfad und verwendet sie zum Erstellen eines JSON-Dokuments mit dem Inhaltstyp `application/json`, das an den Client zurückgegeben wird. T

```java
@FunctionName("TriggerStringRoute")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", 
            methods = {HttpMethod.GET}, 
            authLevel = AuthorizationLevel.ANONYMOUS,
            route = "trigger/{id}/{name=EMPTY}") // name is optional and defaults to EMPTY
        HttpRequestMessage<Optional<String>> request,
        @BindingName("id") String id,
        @BindingName("name") String name,
        final ExecutionContext context) {
    
    // Item list
    context.getLogger().info("Route parameters are: " + id);

    // Convert and display
    if (id == null) {
        return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                        .body("Document not found.")
                        .build();
    } 
    else {
        // return JSON from to the client
        // Generate document
        final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                        "\"description\": \"" + name + "\"}";
        return request.createResponseBuilder(HttpStatus.OK)
                        .header("Content-Type", "application/json")
                        .body(jsonDocument)
                        .build();
    }
}
```

#### <a name="read-pojo-body-from-a-post-request"></a>Lesen des POJO-Hauptteils aus einer POST-Anforderung

Hier sehen Sie den Code für die `ToDoItem`-Klasse, auf die in diesem Beispiel verwiesen wird:

```java

public class ToDoItem {

  private String id;
  private String description;  

  public ToDoItem(String id, String description) {
    this.id = id;
    this.description = description;
  }

  public String getId() {
    return id;
  }

  public String getDescription() {
    return description;
  }
  
  @Override
  public String toString() {
    return "ToDoItem={id=" + id + ",description=" + description + "}";
  }
}

```

In diesem Beispiel wird der Hauptteil einer POST-Anforderung gelesen. Der Anforderungstext wird automatisch in ein `ToDoItem`-Objekt deserialisiert und mit dem Inhaltstyp `application/json` an den Client zurückgegeben. Der Parameter `ToDoItem` wird durch die Functions-Runtime serialisiert, wenn er der Eigenschaft `body` der `HttpMessageResponse.Builder`-Klasse zugewiesen wird.

```java
@FunctionName("TriggerPojoPost")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", 
            methods = {HttpMethod.POST}, 
            authLevel = AuthorizationLevel.ANONYMOUS)
        HttpRequestMessage<Optional<ToDoItem>> request,
        final ExecutionContext context) {
    
    // Item list
    context.getLogger().info("Request body is: " + request.getBody().orElse(null));

    // Check request body
    if (!request.getBody().isPresent()) {
        return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                        .body("Document not found.")
                        .build();
    } 
    else {
        // return JSON from to the client
        // Generate document
        final ToDoItem body = request.getBody().get();
        return request.createResponseBuilder(HttpStatus.OK)
                        .header("Content-Type", "application/json")
                        .body(body)
                        .build();
    }
}
```

---

## <a name="attributes-and-annotations"></a>Attribute und Anmerkungen

In [C#-Klassenbibliotheken](functions-dotnet-class-library.md) und Java ist das `HttpTrigger`-Attribut verfügbar, um die Funktion zu konfigurieren.

Sie können die Autorisierungsebene und die zulässigen HTTP-Methoden in Attributkonstruktorparametern, den Webhooktyp und eine Routenvorlage festlegen. Weitere Informationen zu diesen Einstellungen finden Sie unter [Konfiguration](#configuration).

# <a name="c"></a>[C#](#tab/csharp)

In diesem Beispiel wird veranschaulicht, wie das [HttpTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs)-Attribut verwendet wird.

```csharp
[FunctionName("HttpTriggerCSharp")]
public static Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous)] HttpRequest req)
{
    ...
}
```

Ein vollständiges Beispiel finden Sie unter [Triggerbeispiel](#example).

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Attribute werden von C#-Skript nicht unterstützt.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Attribute werden von JavaScript nicht unterstützt.

# <a name="python"></a>[Python](#tab/python)

Attribute werden von Python nicht unterstützt.

# <a name="java"></a>[Java](#tab/java)

In diesem Beispiel wird veranschaulicht, wie das [HttpTrigger](https://github.com/Azure/azure-functions-java-library/blob/dev/src/main/java/com/microsoft/azure/functions/annotation/HttpTrigger.java)-Attribut verwendet wird.

```java
@FunctionName("HttpTriggerJava")
public HttpResponseMessage<String> HttpTrigger(
        @HttpTrigger(name = "req",
                     methods = {"get"},
                     authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<String> request,
        final ExecutionContext context) {

    ...
}
```

Ein vollständiges Beispiel finden Sie unter [Triggerbeispiel](#example).

---

## <a name="configuration"></a>Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `HttpTrigger` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
| **type** | –| Erforderlich – muss auf `httpTrigger` festgelegt sein. |
| **direction** | –| Erforderlich – muss auf `in` festgelegt sein. |
| **name** | –| Erforderlich – der Variablenname, der im Funktionscode für die Anforderung oder den Anforderungstext verwendet wird. |
| <a name="http-auth"></a>**authLevel** |  **AuthLevel** |Bestimmt, welche Schlüssel (sofern erforderlich) in der Anforderung vorhanden sein müssen, um die Funktion aufzurufen. Bei den Autorisierungsebenen kann es sich um einen der folgenden Werte handeln: <ul><li><code>anonymous</code>: Es ist kein API-Schlüssel erforderlich.</li><li><code>function</code>: Es ist ein funktionsspezifischer API-Schlüssel erforderlich. Dies ist der Standardwert, wenn kein anderer Wert angegeben wird.</li><li><code>admin</code>: Der Hauptschlüssel ist erforderlich.</li></ul> Weitere Informationen finden Sie im Abschnitt zu [Autorisierungsschlüsseln](#authorization-keys). |
| **methods** |**Methoden** | Ein Array der HTTP-Methoden, auf die diese Funktion antwortet. Wird dieses Array nicht angegeben, antwortet die Funktion auf alle HTTP-Methoden. Siehe [Anpassen des HTTP-Endpunkts](#customize-the-http-endpoint). |
| **route** | **Route** | Definiert die Routenvorlage, mit der gesteuert wird, auf welche Anforderungs-URLs die Funktion antwortet. Wenn kein anderer Wert angegeben wird, lautet der Standardwert `<functionname>`. Weitere Informationen finden Sie unter [Anpassen des HTTP-Endpunkts](#customize-the-http-endpoint). |
| **webHookType** | **WebHookType** | _Nur für die Laufzeit der Version 1.x unterstützt._<br/><br/>Konfiguriert den HTTP-Trigger so, dass er als [Webhookempfänger](https://en.wikipedia.org/wiki/Webhook) für den angegebenen Anbieter fungiert. Legen Sie die `methods` Eigenschaft nicht fest, wenn Sie diese Eigenschaft festlegen. Der Webhooktyp kann folgende Werte annehmen:<ul><li><code>genericJson</code>: Ein allgemeiner Webhookendpunkt ohne Logik für einen bestimmten Anbieter. Diese Einstellung beschränkt Anforderungen auf solche, die HTTP POST verwenden und den Inhaltstyp `application/json` aufweisen.</li><li><code>github</code>: Die Funktion antwortet auf [GitHub-Webhooks](https://developer.github.com/webhooks/). Verwenden Sie die _authLevel_-Eigenschaft nicht mit GitHub-Webhooks. Weitere Informationen finden Sie im Abschnitt zu GitHub-Webhooks weiter unten in diesem Artikel.</li><li><code>slack</code>: Die Funktion antwortet auf [Slack-Webhooks](https://api.slack.com/outgoing-webhooks). Verwenden Sie die _authLevel_-Eigenschaft nicht mit Slack-Webhooks. Weitere Informationen finden Sie im Abschnitt zu Slack-Webhooks weiter unten in diesem Artikel.</li></ul>|

## <a name="payload"></a>Nutzlast

Der Triggereingabetyp wird entweder als `HttpRequest` oder als benutzerdefinierter Typ deklariert. Wenn Sie `HttpRequest` auswählen, erhalten Sie Vollzugriff auf das Anforderungsobjekt. Bei einem benutzerdefinierten Typ versucht die Laufzeit, den JSON-Anforderungstext zu analysieren, um die Objekteigenschaften festzulegen.

## <a name="customize-the-http-endpoint"></a>Anpassen des HTTP-Endpunkts

Wenn Sie eine Funktion für einen HTTP-Trigger erstellen, ist die Funktion mit einer Route der folgenden Form erreichbar:

    http://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>

Sie können diese Route mit der optionalen `route`-Eigenschaft in der Eingabebindung des HTTP-Triggers anpassen. In diesem Beispiel wird mit der Datei *function.json* eine `route`-Eigenschaft für einen HTTP-Trigger definiert:

```json
{
    "bindings": [
    {
        "type": "httpTrigger",
        "name": "req",
        "direction": "in",
        "methods": [ "get" ],
        "route": "products/{category:alpha}/{id:int?}"
    },
    {
        "type": "http",
        "name": "res",
        "direction": "out"
    }
    ]
}
```

Mit dieser Konfiguration ist die Funktion jetzt über die folgende Route erreichbar, anstatt über die ursprüngliche Route.

```
http://<APP_NAME>.azurewebsites.net/api/products/electronics/357
```

Diese Konfiguration gestattet dem Funktionscode, zwei Parameter in der Adresse unterstützen: _category_ und _id_.

# <a name="c"></a>[C#](#tab/csharp)

Sie können für Ihre Parameter eine beliebige [Web-API-Routeneinschränkung](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) verwenden. Im folgenden C#-Funktionscode werden beide Parameter verwendet.

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;

public static IActionResult Run(HttpRequest req, string category, int? id, ILogger log)
{
    var message = String.Format($"Category: {category}, ID: {id}");
    return (ActionResult)new OkObjectResult(message);
}
```

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Sie können für Ihre Parameter eine beliebige [Web-API-Routeneinschränkung](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) verwenden. Im folgenden C#-Funktionscode werden beide Parameter verwendet.

```csharp
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;

public static IActionResult Run(HttpRequest req, string category, int? id, ILogger log)
{
    var message = String.Format($"Category: {category}, ID: {id}");
    return (ActionResult)new OkObjectResult(message);
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

In Node stellt die Functions-Laufzeit den Anforderungstext aus dem `context`-Objekt bereit. Weitere Informationen finden Sie im [JavaScript-Triggerbeispiel](#example).

Im folgenden Beispiel wird veranschaulicht, wie Sie Routenparameter aus `context.bindingData` lesen.

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;
    var message = `Category: ${category}, ID: ${id}`;

    context.res = {
        body: message;
    }

    context.done();
}
```

# <a name="python"></a>[Python](#tab/python)

Der Funktionsausführungskontext wird über einen als `func.HttpRequest` deklarierten Parameter bereitgestellt. Diese Instanz ermöglicht einer Funktion den Zugriff auf Datenroutenparameter, Abfragezeichenfolgenwerte und Methoden, mit denen Sie HTTP-Antworten zurückgeben können.

Nach der Definition sind die Routenparameter für die Funktion verfügbar, indem die `route_params`-Methode aufgerufen wird.

```python
import logging

import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:

    category = req.route_params.get('category')
    id = req.route_params.get('id')
    message = f"Category: {category}, ID: {id}"

    return func.HttpResponse(message)
```

# <a name="java"></a>[Java](#tab/java)

Der Funktionsausführungskontext wird durch Eigenschaften wie im `HttpTrigger`-Attribut deklariert festgelegt. Das Attribut ermöglicht Ihnen das Definieren von Routenparametern, Autorisierungsebenen, HTTP-Verben und der Instanz für eingehende Anforderungen.

Routenparameter werden über das `HttpTrigger`-Attribut definiert.

```java
package com.function;

import java.util.*;
import com.microsoft.azure.functions.annotation.*;
import com.microsoft.azure.functions.*;

public class HttpTriggerJava {
    public HttpResponseMessage<String> HttpTrigger(
            @HttpTrigger(name = "req",
                         methods = {"get"},
                         authLevel = AuthorizationLevel.FUNCTION,
                         route = "products/{category:alpha}/{id:int}") HttpRequestMessage<String> request,
            @BindingName("category") String category,
            @BindingName("id") int id,
            final ExecutionContext context) {

        String message = String.format("Category  %s, ID: %d", category, id);
        return request.createResponseBuilder(HttpStatus.OK).body(message).build();
    }
}
```

---

Standardmäßig verfügen alle Funktionsrouten über das Präfix *api*. Sie können das Präfix auch mit der `http.routePrefix`-Eigenschaft in der Datei [host.json](functions-host-json.md) anpassen oder entfernen. Im folgenden Beispiel wird das Routenpräfix *api* entfernt, indem in der Datei *host.json* eine leere Zeichenfolge als Präfix verwendet wird.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

## <a name="using-route-parameters"></a>Verwenden von Routenparametern

Routenparameter, die das Muster `route` einer Funktion definiert haben, sind für jede Bindung verfügbar. Wenn Sie beispielsweise eine Route als `"route": "products/{id}"` definiert haben, kann eine Tabellenspeicherbindung den Wert des Parameters `{id}` in der Bindungskonfiguration verwenden.

Die folgende Konfiguration zeigt, wie der Parameter `{id}` an das `rowKey`-Element der Bindung übergeben wird:

```json
{
    "type": "table",
    "direction": "in",
    "name": "product",
    "partitionKey": "products",
    "tableName": "products",
    "rowKey": "{id}"
}
```

## <a name="working-with-client-identities"></a>Arbeiten mit Clientidentitäten

Wenn Ihre Funktions-App [App Service-Authentifizierung/-Autorisierung](../app-service/overview-authentication-authorization.md) verwendet, können Sie Informationen über authentifizierte Clients aus Ihrem Code anzeigen. Diese Informationen sind als [von der Plattform eingefügter Anforderungsheader](../app-service/app-service-authentication-how-to.md#access-user-claims) verfügbar. 

Sie können diese Informationen auch aus Datenbindungen auslesen. Diese Funktion ist nur für die Functions-Runtime 2.x und höher verfügbar. Sie ist derzeit auch nur für .NET-Sprachen verfügbar.

# <a name="c"></a>[C#](#tab/csharp)

Informationen zu authentifizierten Clients sind als [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) verfügbar. „ClaimsPrincipal“ ist als Teil des Anforderungskontextes verfügbar, wie im folgenden Beispiel gezeigt:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;

public static IActionResult Run(HttpRequest req, ILogger log)
{
    ClaimsPrincipal identities = req.HttpContext.User;
    // ...
    return new OkObjectResult();
}
```

Alternativ kann „ClaimsPrincipal“ einfach als zusätzlicher Parameter in die Funktionssignatur aufgenommen werden:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;
using Newtonsoft.Json.Linq;

public static void Run(JObject input, ClaimsPrincipal principal, ILogger log)
{
    // ...
    return;
}
```

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Informationen zu authentifizierten Clients sind als [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) verfügbar. „ClaimsPrincipal“ ist als Teil des Anforderungskontextes verfügbar, wie im folgenden Beispiel gezeigt:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;

public static IActionResult Run(HttpRequest req, ILogger log)
{
    ClaimsPrincipal identities = req.HttpContext.User;
    // ...
    return new OkObjectResult();
}
```

Alternativ kann „ClaimsPrincipal“ einfach als zusätzlicher Parameter in die Funktionssignatur aufgenommen werden:

```csharp
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;
using Newtonsoft.Json.Linq;

public static void Run(JObject input, ClaimsPrincipal principal, ILogger log)
{
    // ...
    return;
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Der authentifizierte Benutzer ist über [HTTP-Header](../app-service/app-service-authentication-how-to.md#access-user-claims) verfügbar.

# <a name="python"></a>[Python](#tab/python)

Der authentifizierte Benutzer ist über [HTTP-Header](../app-service/app-service-authentication-how-to.md#access-user-claims) verfügbar.

# <a name="java"></a>[Java](#tab/java)

Der authentifizierte Benutzer ist über [HTTP-Header](../app-service/app-service-authentication-how-to.md#access-user-claims) verfügbar.

---

## <a name="function-access-keys"></a><a name="authorization-keys"></a>Funktionszugriffsschlüssel

> [!IMPORTANT]
> Schlüssel helfen zwar Ihre HTTP-Endpunkte während der Entwicklung zu verschleiern, sie sind jedoch nicht als Möglichkeit zum Schützen eines HTTP-Triggers in einer Produktionsumgebung vorgesehen. Weitere Informationen hierzu finden Sie unter [Schützen eines HTTP-Endpunkts in einer Produktionsumgebung](#secure-an-http-endpoint-in-production).

[!INCLUDE [functions-authorization-keys](../../includes/functions-authorization-keys.md)]

## <a name="obtaining-keys"></a>Abrufen von Schlüsseln

Schlüssel werden als Teil Ihrer Funktionen-App in Azure gespeichert und sind im Ruhezustand verschlüsselt. Um vorhandene Schlüssel anzuzeigen, neue Schlüssel zu erstellen oder Schlüsseln neue Werte zuzuweisen, navigieren Sie im [Azure-Portal](https://portal.azure.com) zu einer Ihrer über HTTP ausgelösten Funktionen, und wählen Sie **Verwalten** aus.

![Verwalten Sie Funktionsschlüssel im Portal.](./media/functions-bindings-http-webhook/manage-function-keys.png)

Funktionsschlüssel können mithilfe der [Schlüsselverwaltungs-APIs](https://github.com/Azure/azure-functions-host/wiki/Key-management-API) programmgesteuert abgerufen werden.

## <a name="api-key-authorization"></a>Autorisierung per API-Schlüssel

Für die meisten HTTP-Triggervorlagen muss in der Anforderung ein API-Schlüssel vorhanden sein. Ihre HTTP-Anforderung sieht daher normalerweise wie folgende URL aus:

    https://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?code=<API_KEY>

Der Schlüssel kann in einer Abfragezeichenfolgenvariablen namens `code` (wie oben zu sehen) enthalten sein. Er kann auch in einem `x-functions-key`-HTTP-Header enthalten sein. Der Wert des Schlüssels kann ein beliebiger für die Funktion definierter Funktionsschlüssel oder ein beliebiger Hostschlüssel sein.

Sie können anonyme Anforderungen zulassen, die keine Schlüssel erfordern. Sie können auch anfordern, dass der Hauptschlüssel verwendet wird. Zum Ändern der Standardautorisierungsstufe verwenden Sie die `authLevel`-Eigenschaft in der Bindungs-JSON. Weitere Informationen finden Sie unter [Trigger: Konfiguration](#configuration).

> [!NOTE]
> Beim lokalen Ausführen von Funktionen ist die Autorisierung unabhängig von der festgelegten Autorisierungsebene deaktiviert. Die `authLevel`-Einstellung in Ihrem Trigger wird nach der Veröffentlichung in Azure angewendet. Schlüssel sind auch bei [lokaler Ausführung in einem Container](functions-create-function-linux-custom-image.md#build-the-container-image-and-test-locally) weiterhin erforderlich.


## <a name="secure-an-http-endpoint-in-production"></a>Schützen eines HTTP-Endpunkts in einer Produktionsumgebung

Wenn Sie Ihre Funktionsendpunkte in einer Produktionsumgebung umfassend schützen möchten, sollten Sie eine der folgenden Sicherheitsoptionen auf Funktions-App-Ebene implementieren. Wenn Sie eine dieser Sicherheitsmethoden auf Funktions-App-Ebene verwenden, sollten Sie die Autorisierungsebene der über HTTP ausgelösten Funktion auf `anonymous` festlegen.

[!INCLUDE [functions-enable-auth](../../includes/functions-enable-auth.md)]

#### <a name="deploy-your-function-app-in-isolation"></a>Isoliertes Bereitstellen Ihrer Funktions-App

[!INCLUDE [functions-deploy-isolation](../../includes/functions-deploy-isolation.md)]

## <a name="webhooks"></a>webhooks

> [!NOTE]
> Der Webhookmodus ist nur für die Version 1.x der Functions-Laufzeit verfügbar. Diese Änderung wurde vorgenommen, um die Leistung von HTTP-Triggern in Version 2.x und höheren Versionen zu verbessern.

In Version 1.x stellen Webhookvorlagen zusätzliche Überprüfungsoptionen für Webhooknutzlasten bereit. In Version 2.x und höheren Versionen kann der HTTP-Basistrigger weiterhin verwendet werden. Zudem wird er für Webhooks empfohlen. 

### <a name="github-webhooks"></a>GitHub-Webhooks

Erstellen Sie zum Antworten auf GitHub-Webhooks zuerst eine Funktion mit einem HTTP-Trigger, und legen Sie die **webHookType**-Eigenschaft auf `github` fest. Kopieren Sie dann die URL und den API-Schlüssel auf die Seite **Webhook hinzufügen** Ihres GitHub-Repositorys. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

### <a name="slack-webhooks"></a>Slack-Webhooks

Der Slack-Webhook lässt Sie das Token nicht angeben, sondern generiert es für Sie. Daher müssen Sie einen funktionsspezifischen Schlüssel mit dem Token aus Slack konfigurieren. Weitere Informationen finden Sie unter [Autorisierungsschlüssel](#authorization-keys).

## <a name="webhooks-and-keys"></a>Webhooks und Schlüssel

Die Webhookautorisierung wird von der Empfangskomponente für Webhooks verarbeitet, die zum HTTP-Trigger gehört. Der Mechanismus variiert je nach Webhooktyp. Für jeden Mechanismus wird ein Schlüssel benötigt. Standardmäßig wird der Funktionsschlüssel namens „default“ verwendet. Um einen anderen Schlüssel zu verwenden, konfigurieren Sie den Webhookanbieter, sodass der Schlüsselname auf eine der folgenden Arten mit der Anforderung gesendet wird:

* **Abfragezeichenfolge**: Der Anbieter übergibt den Schlüsselnamen im `clientid`-Abfragezeichenfolgenparameter, z. B. `https://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?clientid=<KEY_NAME>`.
* **Anforderungsheader:** Der Anbieter übergibt den Schlüsselnamen im `x-functions-clientid`-Header.

## <a name="limits"></a>Einschränkungen

Die Länge der HTTP-Anforderung ist auf 100 MB (104.857.600 Bytes) und die URL-Länge auf 4 KB (4.096 Bytes) beschränkt. Diese Grenzwerte werden durch das `httpRuntime`-Element der Datei [Web.config](https://github.com/Azure/azure-functions-host/blob/3.x/src/WebJobs.Script.WebHost/web.config) der Runtime angegeben.

Wenn eine Funktion, die den HTTP-Trigger verwendet, nicht innerhalb von etwa 230 Sekunden abgeschlossen ist, tritt bei [Azure Load Balancer](../app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds) ein Timeout auf, und es wird ein HTTP 502-Fehler zurückgegeben. Die Funktion wird weiterhin ausgeführt, kann aber keine HTTP-Antwort zurückgeben. Bei Funktionen mit langer Ausführungsdauer empfiehlt es sich, asynchrone Muster zu befolgen und einen Speicherort zurückzugeben, von dem aus Sie den Status der Anforderung pingen können. Informationen dazu, wie lang eine Funktion ausgeführt werden kann, finden Sie unter [Skalierung und Hosting – Verbrauchsplan](functions-scale.md#timeout).


## <a name="next-steps"></a>Nächste Schritte

- [Zurückgeben einer HTTP-Antwort von einer Funktion](./functions-bindings-http-webhook-output.md)
