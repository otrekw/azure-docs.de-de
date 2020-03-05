---
title: Azure Service Bus-Bindungen für Azure Functions
description: Erfahren Sie, wie Sie eine Azure-Funktion ausführen, wenn Azure Service Bus-Nachrichten erstellt werden.
author: craigshoemaker
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.topic: reference
ms.date: 02/19/2020
ms.author: cshoe
ms.openlocfilehash: 1ead7fcd9d474369e3a62e372a971d88d26f4e9c
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2020
ms.locfileid: "78273564"
---
# <a name="azure-service-bus-trigger-for-azure-functions"></a>Azure Service Bus-Trigger für Azure Functions

Verwenden Sie den Service Bus-Trigger, um auf Nachrichten von einer Service Bus-Warteschlange oder einem Thema zu reagieren.

Informationen zu Setup- und Konfigurationsdetails finden Sie in der [Übersicht](functions-bindings-service-bus-output.md).

## <a name="example"></a>Beispiel

# <a name="c"></a>[C#](#tab/csharp)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die [Nachrichtenmetadaten](#message-metadata) liest und eine Service Bus-Warteschlangennachricht protokolliert:

```cs
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
    string myQueueItem,
    Int32 deliveryCount,
    DateTime enqueuedTimeUtc,
    string messageId,
    ILogger log)
{
    log.LogInformation($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
    log.LogInformation($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.LogInformation($"DeliveryCount={deliveryCount}");
    log.LogInformation($"MessageId={messageId}");
}
```

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Das folgende Beispiel zeigt eine Service Bus-Triggerbindung in einer Datei vom Typ *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet. Die Funktion liest [Nachrichtenmetadaten](#message-metadata) und protokolliert eine Service Bus-Warteschlangennachricht.

Bindungsdaten in der Datei *function.json*:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
using System;

public static void Run(string myQueueItem,
    Int32 deliveryCount,
    DateTime enqueuedTimeUtc,
    string messageId,
    TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");

    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"DeliveryCount={deliveryCount}");
    log.Info($"MessageId={messageId}");
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Das folgende Beispiel zeigt eine Service Bus-Triggerbindung in einer Datei vom Typ *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion liest [Nachrichtenmetadaten](#message-metadata) und protokolliert eine Service Bus-Warteschlangennachricht. 

Bindungsdaten in der Datei *function.json*:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

Der JavaScript-Skriptcode sieht wie folgt aus:

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('DeliveryCount =', context.bindingData.deliveryCount);
    context.log('MessageId =', context.bindingData.messageId);
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Das folgende Beispiel zeigt, wie Sie eine Service Bus-Warteschlangennachricht mittels Trigger lesen.

Eine Service Bus-Bindung ist in *function.json* definiert, wobei *type* auf `serviceBusTrigger` festgelegt ist.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "msg",
      "type": "serviceBusTrigger",
      "direction": "in",
      "queueName": "inputqueue",
      "connection": "AzureServiceBusConnectionString"
    }
  ]
}
```

Der Code in *_\_init_\_.py* deklariert einen Parameter als `func.ServiceBusMessage`, was es Ihnen ermöglicht, die Warteschlangennachricht in ihrer Funktion zu lesen.

```python
import azure.functions as func

import logging
import json

def main(msg: func.ServiceBusMessage):
    logging.info('Python ServiceBus queue trigger processed message.')

    result = json.dumps({
        'message_id': msg.message_id,
        'body': msg.get_body().decode('utf-8'),
        'content_type': msg.content_type,
        'expiration_time': msg.expiration_time,
        'label': msg.label,
        'partition_key': msg.partition_key,
        'reply_to': msg.reply_to,
        'reply_to_session_id': msg.reply_to_session_id,
        'scheduled_enqueue_time': msg.scheduled_enqueue_time,
        'session_id': msg.session_id,
        'time_to_live': msg.time_to_live,
        'to': msg.to,
        'user_properties': msg.user_properties,
    })

    logging.info(result)
```

# <a name="java"></a>[Java](#tab/java)

Die folgende Java-Funktion verwendet die `@ServiceBusQueueTrigger`-Anmerkung aus der [Java-Funktions-Laufzeitbibliothek](/java/api/overview/azure/functions/runtime), um die Konfiguration für einen Service Bus-Warteschlangentrigger zu beschreiben. Die Funktion greift die Nachricht aus der Warteschlange ab und fügt sie den Protokollen hinzu.

```java
@FunctionName("sbprocessor")
 public void serviceBusProcess(
    @ServiceBusQueueTrigger(name = "msg",
                             queueName = "myqueuename",
                             connection = "myconnvarname") String message,
   final ExecutionContext context
 ) {
     context.getLogger().info(message);
 }
```

Java-Funktionen können auch dadurch ausgelöst werden, dass eine Nachricht einem Service Bus-Thema hinzugefügt wird. Im folgenden Beispiel wird die `@ServiceBusTopicTrigger`-Anmerkung verwendet, um die Triggerkonfiguration zu beschreiben.

```java
@FunctionName("sbtopicprocessor")
    public void run(
        @ServiceBusTopicTrigger(
            name = "message",
            topicName = "mytopicname",
            subscriptionName = "mysubscription",
            connection = "ServiceBusConnection"
        ) String message,
        final ExecutionContext context
    ) {
        context.getLogger().info(message);
    }
```

---

## <a name="attributes-and-annotations"></a>Attribute und Anmerkungen

# <a name="c"></a>[C#](#tab/csharp)

Verwenden Sie in [C#-Klassenbibliotheken](functions-dotnet-class-library.md) die folgenden Attribute, um einen Service Bus-Trigger zu konfigurieren:

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-functions-servicebus-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusTriggerAttribute.cs)

  Der Konstruktor des Attributs nimmt den Namen der Warteschlange oder des Themas und des Abonnements an. In Version 1.x von Azure Functions können Sie auch die Zugriffsrechte der Verbindung angeben. Wenn Sie keine Zugriffsrechte angeben, ist der Standardwert `Manage`. Weitere Informationen finden Sie im Abschnitt [Trigger: Konfiguration](#configuration).

  Das folgende Beispiel zeigt das mit einem Zeichenfolgenparameter verwendete Attribut:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue")] string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  Sie können die Eigenschaft `Connection` festlegen, um den Namen einer App-Einstellung anzugeben, die die zu verwendende Service Bus-Verbindungszeichenfolge enthält, wie im folgenden Beispiel gezeigt:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
      string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  Ein vollständiges Beispiel finden Sie unter [Trigger – Beispiel](#example).

* [ServiceBusAccountAttribute](https://github.com/Azure/azure-functions-servicebus-extension/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusAccountAttribute.cs)

  Eine weitere Möglichkeit zum Angeben des zu verwendenden Service Bus-Kontos. Der Konstruktor nimmt den Namen einer App-Einstellung mit einer Service Bus-Verbindungszeichenfolge an. Das Attribut kann auf Parameter-, Methoden- oder Klassenebene angewendet werden. Das folgende Beispiel zeigt die Anwendung auf Klassen- und Methodenebene:

  ```csharp
  [ServiceBusAccount("ClassLevelServiceBusAppSetting")]
  public static class AzureFunctions
  {
      [ServiceBusAccount("MethodLevelServiceBusAppSetting")]
      [FunctionName("ServiceBusQueueTriggerCSharp")]
      public static void Run(
          [ServiceBusTrigger("myqueue", AccessRights.Manage)] 
          string myQueueItem, ILogger log)
  {
      ...
  }
  ```

Das zu verwendende Service Bus-Konto wird in der folgendem Reihenfolge bestimmt:

* Die Eigenschaft `Connection` des Attributs `ServiceBusTrigger`.
* Das Attribut `ServiceBusAccount`, das auf den gleichen Parameter angewendet wird wie das Attribut `ServiceBusTrigger`.
* Das Attribut `ServiceBusAccount`, das auf die Funktion angewendet wird.
* Das Attribut `ServiceBusAccount`, das auf die Klasse angewendet wird.
* Die App-Einstellung „AzureWebJobsServiceBus“.

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Attribute werden von C#-Skript nicht unterstützt.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Attribute werden von JavaScript nicht unterstützt.

# <a name="python"></a>[Python](#tab/python)

Attribute werden von Python nicht unterstützt.

# <a name="java"></a>[Java](#tab/java)

Mit der `ServiceBusQueueTrigger`-Anmerkung können Sie eine Funktion erstellen, die ausgeführt wird, wenn eine Service Bus-Warteschlangennachricht erstellt wird. Zu den verfügbaren Konfigurationsoptionen zählen Warteschlangenname und Verbindungszeichenfolgenname.

Mit der `ServiceBusTopicTrigger`-Anmerkung können Sie ein Thema und ein Abonnement festlegen, um zu bestimmen, welche Daten die Funktion auslösen.

Weitere Details finden Sie unter [Trigger – Beispiel](#example).

---

## <a name="configuration"></a>Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `ServiceBusTrigger` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type** | – | Muss auf „serviceBusTrigger“ festgelegt werden. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen.|
|**direction** | – | Muss auf „in“ festgelegt werden. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen. |
|**name** | – | Der Name der Variablen, die die Warteschlangen- oder Themanachricht im Funktionscode darstellt. |
|**queueName**|**QueueName**|Der Name der zu überwachenden Warteschlange.  Legen Sie diesen nur fest, wenn Sie eine Warteschlange überwachen (nicht für ein Thema).
|**topicName**|**TopicName**|Der Name des zu überwachenden Themas. Legen Sie diesen nur fest, wenn Sie ein Thema überwachen (nicht für eine Warteschlange).|
|**subscriptionName**|**SubscriptionName**|Der Name des zu überwachenden Abonnements. Legen Sie diesen nur fest, wenn Sie ein Thema überwachen (nicht für eine Warteschlange).|
|**connection**|**Connection**|Der Name einer App-Einstellung, die die Service Bus-Verbindungszeichenfolge für diese Bindung enthält. Falls der Name der App-Einstellung mit „AzureWebJobs“ beginnt, können Sie nur den Rest des Namens angeben. Wenn Sie `connection` also beispielsweise auf „MyServiceBus“ festlegen, sucht die Functions-Laufzeit nach einer App-Einstellung namens „AzureWebJobsMyServiceBus“. Ohne Angabe für `connection` verwendet die Functions-Laufzeit die standardmäßige Service Bus-Verbindungszeichenfolge aus der App-Einstellung „AzureWebJobsServiceBus“.<br><br>Um die Verbindungszeichenfolge zu erhalten, führen Sie die Schritte unter [Abrufen der Verwaltungsanmeldeinformationen](../service-bus-messaging/service-bus-quickstart-portal.md#get-the-connection-string) aus. Die Verbindungszeichenfolge muss für einen Service Bus-Namespace gelten und darf nicht auf eine bestimmte Warteschlange oder ein Thema beschränkt sein. |
|**accessRights**|**zugreifen**|Zugriffsberechtigungen für die Verbindungszeichenfolge. Verfügbare Werte sind `manage` und `listen`. Die Standardeinstellung ist `manage`, d.h. heißt, dass die `connection` die Berechtigung **Manage** hat. Wenn Sie eine Verbindungszeichenfolge verwenden, die nicht über die Berechtigung **Manage** verfügt, legen Sie `accessRights` auf „listen“ fest. Andernfalls versucht die Functions-Runtime ggf. erfolglos Vorgänge auszuführen, die Verwaltungsrechte erfordern. In Version 2.x und höheren Versionen von Azure Functions ist diese Eigenschaft nicht verfügbar, da die aktuelle Version des Service Bus SDK Verwaltungsvorgänge nicht unterstützt.|
|**isSessionsEnabled**|**IsSessionsEnabled**|`true`, wenn eine Verbindung mit einer [sitzungsabhängigen](../service-bus-messaging/message-sessions.md) Warteschlange oder einem Abonnement hergestellt wird. Andernfalls `false`, wobei es sich um den Standardwert handelt.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Verwendung

# <a name="c"></a>[C#](#tab/csharp)

Die folgenden Parametertypen sind für die Warteschlangen- oder Themanachricht verfügbar:

* `string`: Wenn es sich bei der Nachricht um Text handelt.
* `byte[]`: Nützlich für Binärdaten.
* Ein benutzerdefinierter Typ: Wenn die Nachricht JSON enthält, versucht Azure Functions, die JSON-Daten zu deserialisieren.
* `BrokeredMessage`: Gibt die deserialisierte Nachricht mit der [BrokeredMessage.GetBody\<T>()](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1)-Methode zurück.

Diese Parametertypen gelten für Azure Functions Version 1.x. Verwenden Sie für 2.x und höhere Versionen [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) anstelle von `BrokeredMessage`.

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

Die folgenden Parametertypen sind für die Warteschlangen- oder Themanachricht verfügbar:

* `string`: Wenn es sich bei der Nachricht um Text handelt.
* `byte[]`: Nützlich für Binärdaten.
* Ein benutzerdefinierter Typ: Wenn die Nachricht JSON enthält, versucht Azure Functions, die JSON-Daten zu deserialisieren.
* `BrokeredMessage`: Gibt die deserialisierte Nachricht mit der [BrokeredMessage.GetBody\<T>()](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1)-Methode zurück.

Diese Parameter gelten für Azure Functions Version 1.x. Verwenden Sie für 2.x und höhere Versionen [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) anstelle von `BrokeredMessage`.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Greifen Sie auf die Warteschlangen- oder Themanachricht mithilfe von `context.bindings.<name from function.json>` zu. Die Service Bus-Nachricht wird als Zeichenfolge oder als JSON-Objekt an die Funktion übergeben.

# <a name="python"></a>[Python](#tab/python)

Die Warteschlangennachricht ist über einen als `func.ServiceBusMessage` typisierten Parameter für die Funktion verfügbar. Die Service Bus-Nachricht wird als Zeichenfolge oder als JSON-Objekt an die Funktion übergeben.

# <a name="java"></a>[Java](#tab/java)

Die eingehende Service Bus-Nachricht ist über einen `ServiceBusQueueMessage`- oder `ServiceBusTopicMessage`-Parameter verfügbar.

[Detail finden Sie in dem Beispiel](#example).

---

## <a name="poison-messages"></a>Nicht verarbeitbare Nachrichten

Die Verarbeitung von nicht verarbeitbaren Nachricht kann nicht in Azure Functions gesteuert oder konfiguriert werden. Service Bus verarbeitet nicht verarbeitbare Nachrichten selbst.

## <a name="peeklock-behavior"></a>PeekLock-Verhalten

Die Functions-Laufzeit empfängt eine Nachricht im [PeekLock Modus](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode). Sie ruft bei erfolgreicher Ausführung der Funktion `Complete` für die Nachricht auf. Ist die Ausführung nicht erfolgreich, wird `Abandon` aufgerufen. Wenn die Funktion länger als im `PeekLock`-Timeout angegeben ausgeführt wird, wird die Sperre automatisch verlängert, solange die Funktion ausgeführt wird. 

Die `maxAutoRenewDuration` kann in der Datei *host.json* konfiguriert werden, die [OnMessageOptions.MaxAutoRenewDuration](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.messagehandleroptions.maxautorenewduration?view=azure-dotnet) zugeordnet ist. Der maximal zulässige Wert für diese Einstellung beträgt entsprechend der Service Bus-Dokumentation 5 Minuten, wohingegen Sie den Standardwert von 5 Minuten für das Functions-Zeitlimit auf 10 Minuten erhöhen können. Bei Service Bus-Funktionen sollten Sie dies nicht tun, da Sie den Service Bus-Verlängerungsgrenzwert übersteigen würden.

## <a name="message-metadata"></a>Metadaten von Nachrichten

Der Service Bus-Trigger stellt mehrere [Metadateneigenschaften](./functions-bindings-expressions-patterns.md#trigger-metadata) bereit. Diese Eigenschaften können als Teil der Bindungsausdrücke in anderen Bindungen oder als Parameter im Code verwendet werden. Diese Eigenschaften sind Member der [BrokeredMessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)-Klasse.

|Eigenschaft|type|Beschreibung|
|--------|----|-----------|
|`DeliveryCount`|`Int32`|Die Anzahl der Übermittlungen.|
|`DeadLetterSource`|`string`|Die Quelle von unzustellbaren Nachrichten.|
|`ExpiresAtUtc`|`DateTime`|Die Ablaufzeit in UTC.|
|`EnqueuedTimeUtc`|`DateTime`|Die in die Warteschlange eingereihte Uhrzeit in UTC.|
|`MessageId`|`string`|Benutzerdefinierter Wert, mit dem Service Bus doppelte Nachrichten ermitteln kann (sofern aktiviert).|
|`ContentType`|`string`|Ein Inhaltstypbezeichner, der vom Sender und Empfänger für anwendungsspezifische Logik verwendet wird.|
|`ReplyTo`|`string`|Die Antwort auf die Warteschlangenadresse.|
|`SequenceNumber`|`Int64`|Eindeutige Nummer, die vom Service Bus einer Nachricht zugewiesen wird.|
|`To`|`string`|Die Zieladresse.|
|`Label`|`string`|Die anwendungsspezifische Bezeichnung.|
|`CorrelationId`|`string`|Die Korrelations-ID.|

[Codebeispiele](#example) mit diesen Eigenschaften finden Sie weiter oben in diesem Artikel.

## <a name="next-steps"></a>Nächste Schritte

- [Senden von Azure Service Bus-Nachrichten mit Azure Functions (Ausgabebindung)](./functions-bindings-service-bus-output.md)
