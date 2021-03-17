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
ms.openlocfilehash: e35fcd94ef2fbdde0ac3bb7c71f85a3afd15add4
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102487286"
---
# <a name="troubleshooting-in-azure-communication-services"></a>Problembehandlung in Azure Communication Services

Dieses Dokument hilft Ihnen bei der Behandlung von Problemen, die möglicherweise in Ihrer Communication Services-Lösung auftreten. Bei der Behandlung von Problemen mit SMS können Sie [Übermittlungsberichte mit Event Grid](../quickstarts/telephony-sms/handle-sms-events.md) aktivieren, um SMS-Übermittlungsdetails zu erfassen.

## <a name="getting-help"></a>Hilfe

Wir empfehlen Entwicklern, Fragen zu stellen, Features vorzuschlagen und Probleme als Issues zu melden. Zur Unterstützung steht eine [dedizierte Seite mit Support- und Hilfeoptionen](../support.md) zur Verfügung, auf der Ihre Supportoptionen aufgeführt sind.

Um Ihnen bei der Behandlung bestimmter Arten von Problemen zu helfen, werden Sie möglicherweise nach den folgenden Informationen gefragt:

* **MS-CV-ID**: Diese ID wird für die Problembehandlung von Anrufen und Nachrichten verwendet.
* **Anruf-ID**: Diese ID wird zum Identifizieren von Communication Services-Anrufen verwendet.
* **SMS-Nachrichten-ID**: Diese ID wird zum Identifizieren von SMS-Nachrichten verwendet.
* **Anrufprotokolle**: Diese Protokolle enthalten ausführliche Informationen, die zur Behandlung von Anruf- und Netzwerkproblemen verwendet werden können.


## <a name="access-your-ms-cv-id"></a>Zugreifen auf Ihre MS-CV-ID

Der Zugriff auf die MS-CV-ID kann durch Konfigurieren der Diagnose in der `clientOptions`-Objektinstanz erfolgen, wenn Sie die Clientbibliotheken initialisieren. Die Diagnose kann für jede der Azure-Clientbibliotheken konfiguriert werden, einschließlich Chat, Identität und VoIP-Anrufe.

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

## <a name="enable-and-access-call-logs"></a>Aktivieren und Zugreifen auf Anrufprotokolle

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Der folgende Code kann verwendet werden, um `AzureLogger` für die Ausgabe von Protokollen an die Konsole mithilfe der JavaScript-Clientbibliothek zu konfigurieren:

```javascript
import { AzureLogger } from '@azure/logger';

AzureLogger.verbose = (...args) => { console.info(...args); }
AzureLogger.info = (...args) => { console.info(...args); }
AzureLogger.warning = (...args) => { console.info(...args); }
AzureLogger.error = (...args) => { console.info(...args); }

callClient = new CallClient({logger: AzureLogger});
```

# <a name="ios"></a>[iOS](#tab/ios)

Wenn Sie für iOS entwickeln, werden Ihre Protokolle in `.blog`-Dateien gespeichert. Beachten Sie, dass Sie die Protokolle nicht direkt anzeigen können, weil sie verschlüsselt sind.

Auf diese kann durch Öffnen von Xcode zugegriffen werden. Navigieren Sie zu „Windows > Geräte und Simulatoren > Geräte“. Wählen Sie Ihr Gerät aus. Wählen Sie unter „Installierte Apps“ Ihre Anwendung aus, und klicken Sie auf „Container herunterladen“.

Dadurch erhalten Sie eine `xcappdata`-Datei. Klicken Sie mit der rechten Maustaste auf diese Datei, und wählen Sie „Paketinhalt anzeigen“ aus. Die `.blog`-Dateien werden angezeigt, die Sie dann an Ihre Azure-Supportanfrage anfügen können.

# <a name="android"></a>[Android](#tab/android)

Wenn Sie für Android entwickeln, werden Ihre Protokolle in `.blog`-Dateien gespeichert. Beachten Sie, dass Sie die Protokolle nicht direkt anzeigen können, weil sie verschlüsselt sind.

Navigieren Sie in Android Studio zum Gerätedatei-Explorer, indem Sie im Simulator und auf dem Gerät „Anzeigen > Toolfenster > Gerätedatei-Explorer“ auswählen. Die `.blog`-Datei befindet sich im Verzeichnis Ihrer Anwendung, das in etwa wie `/data/data/[app_name_space:com.contoso.com.acsquickstartapp]/files/acs_sdk.blog` aussehen sollte. Sie können diese Datei an Ihre Supportanfrage anfügen.


---

## <a name="calling-client-library-error-codes"></a>Fehlercodes der Clientbibliothek für Telefonie

Die Clientbibliothek für Telefonie von Azure Communication Services verwendet die folgenden Fehlercodes, um Sie beim Beheben von Anrufproblemen zu unterstützen. Diese Fehlercodes werden durch die `call.callEndReason`-Eigenschaft bereitgestellt, nachdem ein Anruf beendet wurde.

| Fehlercode | BESCHREIBUNG | Auszuführende Aktion |
| -------- | ---------------| ---------------|
| 403 | Unzulässig/Authentifizierungsfehler. | Stellen Sie sicher, dass Ihr Communication Services-Token gültig und nicht abgelaufen ist. |
| 404 | Anruf nicht gefunden. | Stellen Sie sicher, dass die Telefonnummer, die Sie anrufen (oder die Telefonkonferenz, der Sie beitreten möchten), vorhanden ist. |
| 408 | Timeout des Anrufcontrollers. | Timeout des Anrufcontrollers beim Warten auf Protokollmeldungen von Benutzerendpunkten. Stellen Sie sicher, dass die Clients verbunden und verfügbar sind. |
| 410 | Fehler des lokalen Medienstapels oder der Medieninfrastruktur. | Stellen Sie sicher, dass Sie die neueste Clientbibliothek in einer unterstützten Umgebung verwenden. |
| 430 | Nachricht kann nicht an die Clientanwendung übermittelt werden. | Stellen Sie sicher, dass die Clientanwendung ausgeführt wird und verfügbar ist. |
| 480 | Remoteclientendpunkt ist nicht registriert. | Stellen Sie sicher, dass der Remoteendpunkt verfügbar ist. |
| 481 | Fehler beim Verarbeiten des eingehenden Anrufs. | Reichen Sie eine Supportanfrage über das Azure-Portal ein. |
| 487 | Der Anruf wurde abgebrochen, lokal abgelehnt, aufgrund eines Endpunktkonfliktfehlers beendet, oder das Medienangebot konnte nicht generiert werden. | Erwartetes Verhalten. |
| 490, 491, 496, 487, 498 | Netzwerkprobleme des lokalen Endpunkts. | Überprüfen Sie Ihr Netzwerk. |
| 500, 503, 504 | Communication Services-Infrastrukturfehler. | Reichen Sie eine Supportanfrage über das Azure-Portal ein. |
| 603 | Anruf global durch Communication Services-Remoteteilnehmer abgelehnt. | Erwartetes Verhalten. |

## <a name="related-information"></a>Verwandte Informationen
- [Protokolle und Diagnose](logging-and-diagnostics.md)
- [Metriken](metrics.md)
