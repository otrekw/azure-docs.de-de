---
title: Azure-Funktion als Ereignishandler für Azure Event Grid-Ereignisse
description: Beschreibt, wie Sie Azure-Funktionen als Ereignishandler für Event Grid-Ereignisse verwenden können.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 8e48949bb5fecdf370fdf23146209ad757ffa062
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86105760"
---
# <a name="azure-function-as-an-event-handler-for-event-grid-events"></a>Azure-Funktion als Ereignishandler für Event Grid-Ereignisse

Ein Ereignishandler ist der Ort, an den das Ereignis gesendet wird. Der Handler ergreift zur Verarbeitung des Ereignisses eine Maßnahme. Mehrere Azure-Dienste werden automatisch für die Behandlung von Ereignissen konfiguriert. **Azure Functions** ist einer dieser Dienste. 

Verwenden Sie **Azure Functions** in einer serverlosen Architektur, um auf Ereignisse aus Event Grid zu reagieren. Wenn eine Azure-Funktion als Handler eingesetzt wird, verwenden Sie den Event Grid-Trigger anstelle des generischen HTTP-Triggers. Event Grid überprüft Event Grid-Trigger automatisch. Bei generischen HTTP-Triggern müssen Sie die [Überprüfungsantwort](webhook-event-delivery.md) selbst implementieren.

Unter [Event Grid-Trigger für Azure Functions](../azure-functions/functions-bindings-event-grid.md) finden Sie eine Übersicht über die Verwendung des Event Grid-Triggers in Funktionen.

## <a name="tutorials"></a>Tutorials

|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Schnellstart: Verarbeiten von Ereignissen mit Funktionen](custom-event-to-function.md) | Beschreibt das Senden eines benutzerdefinierten Ereignisses an eine Funktion, damit dieses verarbeitet wird |
| [Tutorial: Automatisieren der Größenänderung von hochgeladenen Images per Event Grid](resize-images-on-storage-blob-upload-event.md) | Benutzer laden Bilder über eine Web-App in ein Speicherkonto hoch. Wenn ein Speicherblob erstellt wird, sendet Event Grid ein Ereignis an die Funktions-App, die daraufhin die Größe des hochgeladenen Bilds anpasst. |
| [Tutorial: Streamen von Big Data in ein Data Warehouse](event-grid-event-hubs-integration.md) | Wenn Event Hubs eine Capture-Datei erstellt, sendet Event Grid ein Ereignis an eine Funktions-App. Die App ruft die Capture-Datei ab und migriert Daten zu einem Data Warehouse. |
| [Tutorial: Beispiele für die Integration von Azure Service Bus in Azure Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid sendet Nachrichten von einem Service Bus-Thema an eine Funktions-App und an eine Logik-App. |

## <a name="rest-example-for-put"></a>REST-Beispiel (für PUT)

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "AzureFunction",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Web/sites/<FUNCTION APP NAME>/functions/<FUNCTION NAME>",
                "maxEventsPerBatch": 1,
                "preferredBatchSizeInKilobytes": 64
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der unterstützten Ereignishandler finden Sie im Artikel zu [Ereignishandlern](event-handlers.md). 
