---
title: Problembehandlung in Azure Communication Services
description: Erfahren Sie, wie Sie die Informationen ermitteln, die Sie zum Beheben von Problemen mit der Communication Services-Lösung benötigen.
author: manoskow
manager: jken
services: azure-communication-services
ms.author: manoskow
ms.date: 10/23/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 4921078e9e7b5d9de06fbbc9a97dc4a964569e04
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92754667"
---
# <a name="troubleshooting-in-azure-communication-services"></a>Problembehandlung in Azure Communication Services

Dieses Dokument unterstützt Sie bei der Ermittlung der Informationen, die Sie für die Problembehandlung Ihrer Communication Services-Lösung benötigen.

## <a name="getting-help"></a>Hilfe

Wir empfehlen Entwicklern, Fragen zu stellen, Features vorzuschlagen und Probleme als Issues im [GitHub-Repository](https://github.com/Azure/communication) von Communication Services zu melden. Weitere Foren sind:

* [Microsoft Q&A (Fragen und Antworten)](https://docs.microsoft.com/answers/questions/topics/single/101418.html)
* [StackOverflow](https://stackoverflow.com/questions/tagged/azure+communication)

Abhängig vom [Supportplan](https://azure.microsoft.com/support/plans/) Ihres Azure-Abonnements können Sie ein Supportticket direkt über das [Azure-Portal](https://azure.microsoft.com/support/create-ticket/) übermitteln.

Um Ihnen bei der Behandlung bestimmter Arten von Problemen zu helfen, werden Sie möglicherweise nach den folgenden Informationen gefragt:

* **MS-CV-ID** : Diese ID wird für die Problembehandlung von Anrufen und Nachrichten verwendet. 
* **Anruf-ID** : Diese ID wird zum Identifizieren von Communication Services-Anrufen verwendet.
* **SMS-Nachrichten-ID** : Diese ID wird zum Identifizieren von SMS-Nachrichten verwendet.

## <a name="access-your-ms-cv-id"></a>Zugreifen auf Ihre MS-CV-ID

Der Zugriff auf die MS-CV-ID kann durch Konfigurieren der Diagnose in der `clientOptions`-Objektinstanz erfolgen, wenn Sie die Clientbibliotheken initialisieren. Die Diagnose kann für jede der Azure-Clientbibliotheken konfiguriert werden, einschließlich Chat, Verwaltung und VoIP-Anrufe.

### <a name="client-options-example"></a>Beispiel für Clientoptionen

Die folgenden Codeausschnitte veranschaulichen die Diagnosekonfiguration. Wenn die Clientbibliotheken mit aktivierter Diagnose verwendet werden, werden Diagnosedetails an den konfigurierten Ereignislistener ausgegeben:

# <a name="c"></a>[C#](#tab/csharp)
``` 
// 1. Import Azure.Core.Diagnostics
using Azure.Core.Diagnostics;

// 2. Initialize an event source listener instance
using var listener = AzureEventSourceListener.CreateConsoleLogger();
Uri endpoint = new Uri("https://<RESOURCE-NAME>.communication.azure.net");
var (token, communicationUser) = await GetCommunicationUserAndToken();
CommunicationUserCredential communicationUserCredential = new CommunicationUserCredential(token);

// 3. Setup diagnostic settings
var clientOptions = new ChatClientOptions()
{
    Diagnostics =
    {
        LoggedHeaderNames = { "*" },
        LoggedQueryParameters = { "*" },
        IsLoggingContentEnabled = true,
    }
};

// 4. Initialize the ChatClient instance with the clientOptions 
ChatClient chatClient = new ChatClient(endpoint, communicationUserCredential, clientOptions);
ChatThreadClient chatThreadClient = await chatClient.CreateChatThreadAsync("Thread Topic", new[] { new ChatThreadMember(communicationUser) });
```

# <a name="python"></a>[Python](#tab/python)
``` 
from azure.communication.chat import ChatClient, CommunicationUserCredential
endpoint = "https://communication-services-sdk-live-tests-for-python.communication.azure.com"
chat_client = ChatClient(
    endpoint,
    CommunicationUserCredential(token),
    http_logging_policy=your_logging_policy)
```
---

## <a name="access-your-call-id"></a>Zugreifen auf Ihre Anruf-ID

Wenn Sie eine Supportanfrage über das Azure-Portal einreichen, der sich auf Anrufprobleme bezieht, werden Sie möglicherweise aufgefordert, die ID des Anrufs anzugeben, auf den Sie verweisen. Auf diese kann über die anrufende Clientbibliothek zugegriffen werden:

# <a name="javascript"></a>[JavaScript](#tab/javascript)
```javascript
// `call` is an instance of a call created by `callAgent.call` or `callAgent.join` methods 
console.log(call.id)
```

# <a name="ios"></a>[iOS](#tab/ios)
```objc
// The `call id` property can be retrieved by calling the `call.getCallId()` method on a call object after a call ends 
// todo: the code snippet suggests it's a property while the comment suggests it's a method call
print(call.callId) 
```

# <a name="android"></a>[Android](#tab/android)
```java
// The `call id` property can be retrieved by calling the `call.getCallId()` method on a call object after a call ends
// `call` is an instance of a call created by `callAgent.call(…)` or `callAgent.join(…)` methods 
Log.d(call.getCallId()) 
```
---


## <a name="access-your-sms-message-id"></a>Zugreifen auf Ihre SMS-Nachrichten-ID

Bei SMS-Problemen können Sie die Nachrichten-ID aus dem Antwortobjekt erfassen.

# <a name="net"></a>[.NET](#tab/dotnet)
```
// Instantiate the SMS client
const smsClient = new SmsClient(connectionString);
async function main() {
  const result = await smsClient.send({
    from: "+18445792722",
    to: ["+1972xxxxxxx"],
    message: "Hello World 👋🏻 via Sms"
  }, {
    enableDeliveryReport: true // Optional parameter
  });
console.log(result); // your message ID will be in the result
}
```
---

## <a name="related-information"></a>Verwandte Informationen
- [Protokolle und Diagnose](logging-and-diagnostics.md)
- [Metriken](metrics.md)
