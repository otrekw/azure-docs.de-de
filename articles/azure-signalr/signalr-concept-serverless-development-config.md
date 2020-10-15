---
title: 'Azure SignalR Service: Entwickeln und Konfigurieren der Azure Functions-App'
description: Ausführliche Informationen zur Entwicklung und Konfiguration serverloser Echtzeitanwendungen unter Verwendung von Azure Functions und Azure SignalR Service
author: anthonychu
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: antchu
ms.custom: devx-track-js, devx-track-csharp
ms.openlocfilehash: ca7d28e3f964d486d9f860c355e88132ebb897a2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91327645"
---
# <a name="azure-functions-development-and-configuration-with-azure-signalr-service"></a>Azure Functions-Entwicklung und -Konfiguration mit Azure SignalR Service

Azure Functions-Anwendungen können mithilfe der [Azure SignalR Service-Bindungen](../azure-functions/functions-bindings-signalr-service.md) mit Echtzeitfunktionen versehen werden. Clientanwendungen verwenden in mehreren Sprachen verfügbare Client-SDKs, um eine Verbindung mit Azure SignalR Service herzustellen und Nachrichten in Echtzeit zu empfangen.

In diesem Artikel werden die Konzepte für die Entwicklung und Konfiguration einer Azure Functions-App mit SignalR Service-Integration beschrieben.

## <a name="signalr-service-configuration"></a>SignalR Service-Konfiguration

Azure SignalR Service kann in verschiedenen Modi konfiguriert werden. Bei Verwendung mit Azure Functions muss der Dienst im *serverlosen* Modus konfiguriert werden.

Navigieren Sie im Azure-Portal zu Seite *Einstellungen* Ihrer SignalR Service-Ressource. Legen Sie den *Dienstmodus* auf *Serverlos* fest.

![SignalR Service-Modus](media/signalr-concept-azure-functions/signalr-service-mode.png)

## <a name="azure-functions-development"></a>Azure Functions-Entwicklung

Eine serverlose Echtzeitanwendung, die mit Azure Functions und Azure SignalR Service erstellt wird, benötigt in der Regel zwei Azure-Funktionen:

* Eine Aushandlungsfunktion, die der Client aufruft, um ein gültiges SignalR Service-Zugriffstoken und eine Dienstendpunkt-URL zu erhalten.
* Eine oder mehrere Funktionen, die Nachrichten von SignalR Service verarbeiten und Nachrichten senden oder die Gruppenmitgliedschaft verwalten.

### <a name="negotiate-function"></a>Aushandlungsfunktion

Eine Clientanwendung benötigt ein gültiges Zugriffstoken, um eine Verbindung mit Azure SignalR Service herstellen zu können. Ein Zugriffstoken kann anonym oder für eine bestimmte Benutzer-ID authentifiziert sein. Serverlose SignalR Service-Anwendungen benötigen einen HTTP-Endpunkt namens „negotiate“, um ein Token und andere Verbindungsinformationen (etwa die SignalR Service-Endpunkt-URL) zu erhalten.

Verwenden Sie eine Azure-Funktion mit HTTP-Trigger sowie die Eingabebindung *SignalRConnectionInfo*, um das Verbindungsinformationenobjekt zu generieren. Die Funktion muss über eine HTTP-Route verfügen, die mit `/negotiate` endet.

Mit einem [klassenbasierten Modell](#class-based-model) in C# benötigen Sie keine *SignalRConnectionInfo*-Eingangsbindung und können benutzerdefinierte Ansprüche viel einfacher hinzufügen. Weitere Informationen finden Sie unter [Aushandlung im klassenbasierten Modell](#negotiate-experience-in-class-based-model).

Weitere Informationen zum Erstellen der Aushandlungsfunktion finden Sie in der [Referenz für die Eingabebindung *SignalRConnectionInfo*](../azure-functions/functions-bindings-signalr-service-input.md).

Informationen zum Erstellen eines authentifizierten Tokens finden Sie unter [Verwenden der App Service-Authentifizierung](#using-app-service-authentication).

### <a name="handle-messages-sent-from-signalr-service"></a>Verarbeiten von Nachrichten, die von SignalR Service gesendet wurden

Verwenden Sie die *SignalR Trigger*-Bindung, um von SignalR Service gesendete Nachrichten zu verarbeiten. Die Auslösung ist möglich, wenn Clients Nachrichten senden oder wenn eine Clientverbindung hergestellt oder getrennt wird.

Weitere Informationen finden Sie in der [Referenz zu *SignalR Trigger*-Bindungen](../azure-functions/functions-bindings-signalr-service-trigger.md).

Außerdem müssen Sie den Funktionsendpunkt als Upstream konfigurieren, damit der Dienst die Funktion auslöst, wenn eine Nachricht vom Client vorliegt. Weitere Informationen zum Konfigurieren von Upstream finden Sie in diesem [Dokument](concept-upstream.md).

### <a name="sending-messages-and-managing-group-membership"></a>Senden von Nachrichten und Verwalten der Gruppenmitgliedschaft

Verwenden Sie die Ausgabebindung *SignalR*, um Nachrichten an Clients zu senden, die mit Azure SignalR Service verbunden sind. Sie können Nachrichten an alle Clients übertragen oder nur an einen Teil der Clients senden, die mit einer bestimmten Benutzer-ID authentifiziert oder einer bestimmten Gruppe hinzugefügt wurden.

Benutzer können einer einzelnen Gruppe oder mehreren Gruppen hinzugefügt werden. Mit der Ausgabebindung *SignalR* können Benutzer auch zu Gruppen hinzugefügt oder aus Gruppen entfernt werden.

Weitere Informationen finden Sie in der [Referenz für die Ausgabebindung *SignalR*](../azure-functions/functions-bindings-signalr-service-output.md).

### <a name="signalr-hubs"></a>SignalR-Hubs

SignalR verfügt über ein Hub-Konzept. Jede Clientverbindung und jede aus Azure Functions gesendete Nachricht ist auf einen bestimmten Hub ausgerichtet. Mithilfe von Hubs können Sie Ihre Verbindungen und Nachrichten in logische Namespaces aufteilen.

## <a name="class-based-model"></a>Klassenbasiertes Modell

Das klassenbasierte Modell ist dediziert für C# bestimmt. Das klassenbasierte Modell kann eine einheitliche SignalR-Server-seitige Programmiererfahrung bieten. Es bietet die folgenden Features.

* Geringerer Konfigurationsaufwand: Der Klassenname wird als `HubName` verwendet, der Methodenname wird als `Event` verwendet, und `Category` wird automatisch gemäß dem Methodennamen festgelegt.
* Automatische Parameterbindung: Weder `ParameterNames` noch Attribut `[SignalRParameter]` ist erforderlich. Parameter werden in der angegebenen Reihenfolge automatisch an Argumente der Azure Function-Methode gebunden.
* Praktische Ausgabe-und Aushandlungserfahrung.

Die folgenden Codebeispiele veranschaulichen diese Features:

```cs
public class SignalRTestHub : ServerlessHub
{
    [FunctionName("negotiate")]
    public SignalRConnectionInfo Negotiate([HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req)
    {
        return Negotiate(req.Headers["x-ms-signalr-user-id"], GetClaims(req.Headers["Authorization"]));
    }

    [FunctionName(nameof(OnConnected))]
    public async Task OnConnected([SignalRTrigger]InvocationContext invocationContext, ILogger logger)
    {
        await Clients.All.SendAsync(NewConnectionTarget, new NewConnection(invocationContext.ConnectionId));
        logger.LogInformation($"{invocationContext.ConnectionId} has connected");
    }

    [FunctionName(nameof(Broadcast))]
    public async Task Broadcast([SignalRTrigger]InvocationContext invocationContext, string message, ILogger logger)
    {
        await Clients.All.SendAsync(NewMessageTarget, new NewMessage(invocationContext, message));
        logger.LogInformation($"{invocationContext.ConnectionId} broadcast {message}");
    }

    [FunctionName(nameof(OnDisconnected))]
    public void OnDisconnected([SignalRTrigger]InvocationContext invocationContext)
    {
    }
}
```

Alle Funktionen, die das klassenbasierte Modell nutzen sollen, müssen die Methode der Klasse aufweisen, die von **ServerlessHub** erbt. Der Klassenname `SignalRTestHub` im Beispiel ist der Hubname.

### <a name="define-hub-method"></a>Definieren der Hubmethode

Alle Hubmethoden **müssen** über das Argument `InvocationContext` verfügen, das um das Attribut `[SignalRTrigger]` ergänzt sein muss, und einen parameterlosen Konstruktor verwenden. Anschließend wird der **Methodenname** als Parameter**ereignis** behandelt.

Standardmäßig ist `category=messages` (mit Ausnahme des Methodennamens)gleich einem der folgenden Namen:

* **OnConnected**: Wird als `category=connections, event=connected` behandelt.
* **OnDisconnected**: Wird als `category=connections, event=disconnected` behandelt.

### <a name="parameter-binding-experience"></a>Parameterbindung

Im klassenbasierten Modell ist `[SignalRParameter]` unnötig, da alle Argumente standardmäßig als `[SignalRParameter]` markiert sind, außer in den folgenden Situationen:

* Das-Argument wird durch ein Bindungsattribut ergänzt.
* Der Argumenttyp ist `ILogger` oder `CancellationToken`.
* Das Argument wird durch Attribut `[SignalRIgnore]` ergänzt.

### <a name="negotiate-experience-in-class-based-model"></a>Aushandlung im klassenbasierten Modell

Anstatt die SignalR-Eingangsbindung `[SignalR]` zu verwenden, kann die Aushandlung im klassenbasierten Modell flexibler erfolgen. Basisklasse `ServerlessHub` verfügt über eine Methode

```cs
SignalRConnectionInfo Negotiate(string userId = null, IList<Claim> claims = null, TimeSpan? lifeTime = null)
```

Dieses Feature passt `userId` oder `claims` während der Funktionsausführung benutzerdefiniert an.

## <a name="use-signalrfilterattribute"></a>Verwenden Sie `SignalRFilterAttribute`

Der Benutzer kann die abstrakte Klasse `SignalRFilterAttribute` erben und implementieren. Werden in `FilterAsync` Ausnahmen ausgelöst, werden `403 Forbidden` an Clients zurückgesendet.

Im folgenden Beispiel wird veranschaulicht, wie ein Kundenfilter implementiert wird, der nur `admin` das Aufrufen von `broadcast`gestattet.

```cs
[AttributeUsage(AttributeTargets.Method, AllowMultiple = true, Inherited = true)]
internal class FunctionAuthorizeAttribute: SignalRFilterAttribute
{
    private const string AdminKey = "admin";

    public override Task FilterAsync(InvocationContext invocationContext, CancellationToken cancellationToken)
    {
        if (invocationContext.Claims.TryGetValue(AdminKey, out var value) &&
            bool.TryParse(value, out var isAdmin) &&
            isAdmin)
        {
            return Task.CompletedTask;
        }

        throw new Exception($"{invocationContext.ConnectionId} doesn't have admin role");
    }
}
```

Verwenden Sie das Attribut zum Autorisieren der Funktion.

```cs
[FunctionAuthorize]
[FunctionName(nameof(Broadcast))]
public async Task Broadcast([SignalRTrigger]InvocationContext invocationContext, string message, ILogger logger)
{
}
```

## <a name="client-development"></a>Cliententwicklung

SignalR-Clientanwendungen können das für mehrere Sprachen verfügbare SignalR-Client-SDK nutzen, um problemlos eine Verbindung mit Azure SignalR Service herzustellen und Nachrichten von Azure SignalR Service zu empfangen.

### <a name="configuring-a-client-connection"></a>Konfigurieren einer Clientverbindung

Für die Verbindungsherstellung mit SignalR Service ist eine erfolgreiche Verbindungsaushandlung durch den Client erforderlich. Diese umfasst folgende Schritte:

1. Senden einer Anforderung an den weiter oben besprochenen HTTP-Endpunkt *negotiate*, um gültige Verbindungsinformationen zu erhalten
1. Herstellen der Verbindung mit SignalR Service unter Verwendung der Dienstendpunkt-URL und des Zugriffstokens, die vom Endpunkt *negotiate* bezogen wurden

Die für den Aushandlungs-Handshake erforderliche Logik ist bereits in den SignalR-Client-SDKs enthalten. Übergeben Sie die URL des Aushandlungsendpunkts (ohne das Segment `negotiate`) an `HubConnectionBuilder` des SDKs. Beispiel in JavaScript:

```javascript
const connection = new signalR.HubConnectionBuilder()
  .withUrl('https://my-signalr-function-app.azurewebsites.net/api')
  .build()
```

Konventionsgemäß fügt das SDK automatisch `/negotiate` an die URL an und verwendet sie, um mit der Aushandlung zu beginnen.

> [!NOTE]
> Falls Sie das JavaScript/TypeScript SDK in einem Browser verwenden, müssen Sie in Ihrer Funktionen-App die [Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) aktivieren](#enabling-cors).

Weitere Informationen zur Verwendung des SignalR-Client-SDK finden Sie in der Dokumentation für die verwendete Sprache:

* [.NET Standard](https://docs.microsoft.com/aspnet/core/signalr/dotnet-client)
* [JavaScript](https://docs.microsoft.com/aspnet/core/signalr/javascript-client)
* [Java](https://docs.microsoft.com/aspnet/core/signalr/java-client)

### <a name="sending-messages-from-a-client-to-the-service"></a>Senden von Nachrichten von einem Client an den Dienst

Wenn Sie für Ihre SignalR-Ressource [Upstream](concept-upstream.md) konfiguriert haben, können Sie Nachrichten vom Client mithilfe beliebiger SignalR-Clients an Ihre Azure Functions senden. Beispiel in JavaScript:

```javascript
connection.send('method1', 'arg1', 'arg2');
```

## <a name="azure-functions-configuration"></a>Azure Functions-Konfiguration

Azure-Funktions-Apps mit Azure SignalR Service-Integration können wie jede typische Azure-Funktions-App mit Techniken wie [Continuous Deployment](../azure-functions/functions-continuous-deployment.md), [ZIP-Bereitstellung](../azure-functions/deployment-zip-push.md) und [Ausführen aus einem Paket](../azure-functions/run-functions-from-deployment-package.md) bereitgestellt werden.

Bei Apps mit SignalR Service-Bindungen müssen allerdings ein paar Besonderheiten berücksichtigt werden. Wenn der Client in einem Browser ausgeführt wird, muss CORS aktiviert werden. Sollte die App eine Authentifizierung erfordern, können Sie den Aushandlungsendpunkt in die App Service-Authentifizierung integrieren.

### <a name="enabling-cors"></a>Aktivieren von CORS

Der JavaScript/TypeScript-Client sendet HTTP-Anforderungen an die Aushandlungsfunktion, um die Verbindungsaushandlung zu initiieren. Wird die Clientanwendung in einer anderen Domäne gehostet als die Azure-Funktions-App, muss für die Funktions-App die Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) aktiviert werden, da der Browser die Anforderungen andernfalls blockiert.

#### <a name="localhost"></a>Localhost

Wenn Sie die Funktions-App auf Ihrem lokalen Computer ausführen, können Sie *local.settings.json* einen Abschnitt vom Typ `Host` hinzufügen, um CORS zu aktivieren. Fügen Sie im Abschnitt `Host` zwei Eigenschaften hinzu:

* `CORS`: Geben Sie die Basis-URL (also den Ursprung der Clientanwendung) ein.
* `CORSCredentials`: Legen Sie diese Eigenschaft auf `true` fest, um Anforderungen vom Typ „withCredentials“ zu ermöglichen.

Beispiel:

```json
{
  "IsEncrypted": false,
  "Values": {
    // values
  },
  "Host": {
    "CORS": "http://localhost:8080",
    "CORSCredentials": true
  }
}
```

#### <a name="cloud---azure-functions-cors"></a>Cloud – Azure Functions-CORS

Wenn Sie CORS für eine Azure-Funktions-App aktivieren möchten, navigieren Sie im Azure-Portal unter der Registerkarte *Plattformfeatures* Ihrer Funktions-App zum CORS-Konfigurationsbildschirm.

> [!NOTE]
> Die CORS-Konfiguration ist im Linux-Verbrauchstarif für Azure Functions noch nicht verfügbar. Verwenden Sie [Azure API Management](#cloud---azure-api-management), um CORS zu aktivieren.

CORS muss mit „Access-Control-Allow-Credentials“ aktiviert werden, damit der SignalR-Client die Aushandlungsfunktion aufrufen kann. Aktivieren Sie das entsprechende Kontrollkästchen.

Fügen Sie im Abschnitt *Zulässige Ursprünge* einen Eintrag mit der Basis-URL des Ursprungs Ihrer Webanwendung hinzu.

![Konfigurieren von CORS](media/signalr-concept-serverless-development-config/cors-settings.png)

#### <a name="cloud---azure-api-management"></a>Cloud – Azure API Management

Azure API Management bietet ein API-Gateway, mit dem vorhandenen Back-End-Diensten Funktionen hinzugefügt werden. Sie können damit Ihrer Funktions-App CORS hinzufügen. Es bietet einen Verbrauchstarif mit aktionsbasierten Preisen und einem monatlichen kostenlosen Verbrauch.

Informationen zum [Importieren einer Azure-Funktions-App](../api-management/import-function-app-as-api.md) finden Sie in der API Management-Dokumentation. Nach dem Importieren können Sie eine Richtlinie für eingehenden Datenverkehr hinzufügen, um CORS mit der Unterstützung von „Access-Control-Allow-Credentials“ zu aktivieren.

```xml
<cors allow-credentials="true">
  <allowed-origins>
    <origin>https://azure-samples.github.io</origin>
  </allowed-origins>
  <allowed-methods>
    <method>GET</method>
    <method>POST</method>
  </allowed-methods>
  <allowed-headers>
    <header>*</header>
  </allowed-headers>
  <expose-headers>
    <header>*</header>
  </expose-headers>
</cors>
```

Konfigurieren Sie die SignalR-Clients für die Verwendung der API Management-URL.

### <a name="using-app-service-authentication"></a>Verwenden der App Service-Authentifizierung

Azure Functions verfügt über eine integrierte Authentifizierung und unterstützt gängige Anbieter wie Facebook, Twitter, Microsoft-Konto, Google und Azure Active Directory. Dieses Feature kann in die Bindung *SignalRConnectionInfo* integriert werden, um Verbindungen mit Azure SignalR Service zu erstellen, die für eine Benutzer-ID authentifiziert wurden. Ihre Anwendung kann unter Verwendung der Ausgabebindung *SignalR* Nachrichten senden, die für diese Benutzer-ID bestimmt sind.

Öffnen Sie im Azure-Portal auf der Registerkarte *Plattformfeatures* Ihrer Funktions-App das Fenster mit den*Authentifizierungs-/Autorisierungseinstellungen*. Gehen Sie gemäß der Dokumentation für die [App Service-Authentifizierung](../app-service/overview-authentication-authorization.md) vor, um die Authentifizierung mit einem Identitätsanbieter Ihrer Wahl zu konfigurieren.

Nach Abschluss der Konfiguration enthalten authentifizierte HTTP-Anforderungen Header vom Typ `x-ms-client-principal-name` und `x-ms-client-principal-id`, die wiederum den Benutzernamen bzw. die Benutzer-ID der authentifizierten Identität enthalten.

Diese Header können in der Konfiguration der Bindung *SignalRConnectionInfo* verwendet werden, um authentifizierte Verbindungen zu erstellen. Das folgende Beispiel zeigt eine C#-Aushandlungsfunktion mit dem Header `x-ms-client-principal-id`:

```csharp
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

Anschließend können Sie Nachrichten an diesen Benutzer senden, indem Sie die Eigenschaft `UserId` einer SignalR-Nachricht festlegen.

```csharp
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message,
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage
        {
            // the message will only be sent to these user IDs
            UserId = "userId1",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

Informationen zu anderen Sprachen finden Sie unter [Bindungen des SignalR-Diensts für Azure Functions](../azure-functions/functions-bindings-signalr-service.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie mithilfe von Azure Functions serverlose SignalR Service-Anwendungen entwickeln und konfigurieren. Versuchen Sie als Nächstes, anhand einer der Schnellstartanleitungen oder eines der Tutorials auf der [Übersichtsseite für SignalR Service](index.yml) selbst eine Anwendung zu erstellen.