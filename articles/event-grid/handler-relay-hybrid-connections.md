---
title: Relay-Hybridverbindung als Ereignishandler für Azure Event Grid-Ereignisse
description: Hier wird beschrieben, wie Sie Azure Relay-Hybridverbindungen als Ereignishandler für Azure Event Grid-Ereignisse verwenden können.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 3807e2d125d652b8f5ed7c9dec1b972d69f699f6
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91270201"
---
# <a name="relay-hybrid-connection-as-an-event-handler-for-azure-event-grid-events"></a>Relay-Hybridverbindung als Ereignishandler für Azure Event Grid-Ereignisse
Ein Ereignishandler ist der Ort, an den das Ereignis gesendet wird. Der Handler ergreift zur Verarbeitung des Ereignisses weitere Maßnahmen. Mehrere Azure-Dienste werden automatisch für die Behandlung von Ereignissen konfiguriert. **Azure Relay** ist einer dieser Dienste. 

Verwenden Sie **Azure Relay-Hybridverbindungen** zum Senden von Ereignissen an Anwendungen, die sich in einem Unternehmensnetzwerk befinden und nicht über einen öffentlich zugänglichen Endpunkt verfügen.

## <a name="tutorials"></a>Tutorials
Im folgenden Tutorial finden Sie ein Beispiel für die Verwendung einer Azure Relay-Hybridverbindung als Ereignishandler: 

|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Tutorial: Senden von Ereignissen an eine Hybridverbindung](custom-event-to-hybrid-connection.md) | Sendet ein benutzerdefiniertes Ereignis zur Verarbeitung durch eine Listeneranwendung an eine bestehende Hybridverbindung. |

## <a name="rest-example-for-put"></a>REST-Beispiel (für PUT)

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "HybridConnection",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Relay/namespaces/<RELAY NAMESPACE NAME>/hybridconnections/<HYBRID CONNECTION NAME>"
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

> [!NOTE]
> Das Bereitstellen von Ereignissen in einer Azure Relay-Hybridverbindung in einem **anderen Mandanten** wird nicht unterstützt. 

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der unterstützten Ereignishandler finden Sie im Artikel zu [Ereignishandlern](event-handlers.md). 