---
title: Azure Event Grid-Ereignishandler
description: Informationen zu unterstützten Ereignishandlern für Azure Event Grid Azure Automation, Functions, Event Hubs, Hybrid Connections, Logic Apps, Service Bus, Queue Storage, Webhooks.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 76f7fff3e0a3e1c0c554e452987bf944503358b8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "86117014"
---
# <a name="event-handlers-in-azure-event-grid"></a>Ereignishandler in Azure Event Grid
Ein Ereignishandler ist der Ort, an den das Ereignis gesendet wird. Der Handler ergreift zur Verarbeitung des Ereignisses weitere Maßnahmen. Mehrere Azure-Dienste werden automatisch für die Behandlung von Ereignissen konfiguriert. Sie können aber auch einen beliebigen Webhook für die Behandlung von Ereignissen verwenden. Der Webhook muss zum Behandeln von Ereignissen nicht in Azure gehostet werden. Event Grid unterstützt nur HTTPS-Webhook-Endpunkte.

## <a name="supported-event-handlers"></a>Unterstützte Ereignishandler
Nachfolgend sind die unterstützten Ereignishandler aufgeführt: 

- [Webhooks](handler-webhooks.md). Azure Automation-Runbooks und Logik-Apps werden über Webhooks unterstützt. 
- [Azure-Funktionen](handler-functions.md)
- [Event Hubs](handler-event-hubs.md)
- [Relay-Hybridverbindungen](handler-relay-hybrid-connections.md)
- [Service Bus-Warteschlangen und -Themen](handler-service-bus.md)
- [Storage-Warteschlangen](handler-storage-queues.md)

## <a name="next-steps"></a>Nächste Schritte
- Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
