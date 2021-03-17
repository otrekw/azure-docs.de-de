---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: fe80a71125d43220e408eab7b07aeedcafa0a526
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102473821"
---
### <a name="functions-2x-and-higher"></a>Functions 2.x und höher

```json
{
    "version": "2.0",
    "extensions": {
        "eventHubs": {
            "batchCheckpointFrequency": 5,
            "eventProcessorOptions": {
                "maxBatchSize": 256,
                "prefetchCount": 512
            },
            "initialOffsetOptions": {
                "type": "fromStart",
                "enqueuedTimeUtc": ""
            }
        }
    }
}  
```

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------|
|batchCheckpointFrequency|1|Die Anzahl der zu verarbeitenden Ereignisbatches, bevor ein EventHub-Cursorprüfpunkt erstellt wird.|
|eventProcessorOptions/maxBatchSize|10|Die maximale Ereignisanzahl, die pro Empfangsschleife empfangen wird.|
|eventProcessorOptions/prefetchCount|300|Die standardmäßige Vorabrufanzahl, die vom zugrunde liegenden `EventProcessorHost` verwendet wird. Der minimal zulässige Wert ist 10.|
|initialOffsetOptions/type<sup>1</sup>|fromStart|Der Punkt innerhalb des Ereignisdatenstroms, von dem aus die Verarbeitung gestartet wird, wenn im Speicher kein Prüfpunkt vorhanden ist. Die verfügbaren Optionen sind `fromStart`, `fromEnd` und `fromEnqueuedTime`. Mit `fromEnd` werden neue Ereignisse verarbeitet, die nach dem Start der Ausführung der Funktions-App in die Warteschlange eingereiht wurden. Gilt für alle Partitionen.  Weitere Informationen finden Sie in der [EventProcessorOptions-Dokumentation](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.initialoffsetprovider).|
|initialOffsetOptions/enqueuedTimeUtc<sup>1</sup>|–| Gibt für das Ereignis des Datenstroms den Zeitpunkt der Einreihung in die Warteschlange an, ab dem mit der Verarbeitung begonnen werden soll. Wenn `initialOffsetOptions/type` als `fromEnqueuedTime` konfiguriert wird, ist diese Einstellung obligatorisch. Es werden Zeitangaben in allen Formaten unterstützt, die von [DateTime.Parse()](/dotnet/standard/base-types/parsing-datetime) unterstützt werden, z. B. `2020-10-26T20:31Z`. Der Eindeutigkeit halber sollten Sie auch eine Zeitzone angeben. Wenn keine Zeitzone angegeben wird, wird von Functions die lokale Zeitzone des Computers übernommen, auf dem die Funktions-App ausgeführt wird. Bei der Ausführung in Azure ist dies „UTC“. Weitere Informationen finden Sie in der [EventProcessorOptions-Dokumentation](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.initialoffsetprovider).|

<sup>1</sup> `intitialOffsetOptions` wird ab [EventHubs v4.2.0](https://github.com/Azure/azure-functions-eventhubs-extension/releases/tag/v4.2.0) unterstützt.

> [!NOTE]
> Eine Referenz für „host.json“ in Azure Functions 2x und höheren Versionen finden Sie in der [host.json-Referenz für Azure Functions 2.x oder höher](../articles/azure-functions/functions-host-json.md).

### <a name="functions-1x"></a>Functions 1.x

```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------| 
|maxBatchSize|64|Die maximale Ereignisanzahl, die pro Empfangsschleife empfangen wird.|
|prefetchCount|–|Die standardmäßige Vorabrufanzahl, die vom zugrunde liegenden `EventProcessorHost` verwendet wird| 
|batchCheckpointFrequency|1|Die Anzahl der zu verarbeitenden Ereignisbatches, bevor ein EventHub-Cursorprüfpunkt erstellt wird.| 

> [!NOTE]
> Eine Referenz für „host.json“ in Azure Functions 1.x finden Sie unter [host.json-Referenz für Azure Functions 1.x](../articles/azure-functions/functions-host-json-v1.md).
