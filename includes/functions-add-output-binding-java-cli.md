---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 2b2c043e70aac14c7fc6f0b58aae257624b05d13
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "80673307"
---
In einem Java-Projekt werden die Bindungen als Bindungsanmerkungen für die Funktionsmethode definiert. Die Datei *function.json* wird dann automatisch auf der Grundlage dieser Anmerkungen generiert.

Navigieren Sie zum Speicherort Ihres Funktionscodes (unter _src/main/java_), öffnen Sie die Projektdatei *Function.java*, und fügen Sie der Definition der Methode `run` die folgenden Parameter hinzu:

```java
@QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") OutputBinding<String> msg
```

Der Parameter `msg` ist ein [`OutputBinding<T>`](/java/api/com.microsoft.azure.functions.outputbinding)-Typ. Dieser stellt eine Sammlung von Zeichenfolgen dar, die als Nachrichten in eine Ausgabebindung geschrieben werden, wenn die Funktion abgeschlossen wird. In diesem Fall ist die Ausgabe eine Speicherwarteschlange mit dem Namen `outqueue`. Die Verbindungszeichenfolge für das Storage-Konto wird durch die Methode `connection` festgelegt. Übergeben Sie anstelle der eigentlichen Verbindungszeichenfolge die Anwendungseinstellung, die die Verbindungszeichenfolge für das Storage-Konto enthält.

Die Definition der Methode `run` sollte nun wie im folgenden Beispiel aussehen:  

```java
@FunctionName("HttpTrigger-Java")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION)  
        HttpRequestMessage<Optional<String>> request, 
        @QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") 
        OutputBinding<String> msg, final ExecutionContext context) {
    ...
}
```