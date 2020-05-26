---
title: Azure Event Grid-Ereignishandler
description: Informationen zu unterstützten Ereignishandlern für Azure Event Grid Azure Automation, Functions, Event Hubs, Hybrid Connections, Logic Apps, Service Bus, Queue Storage, Webhooks.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: c29526bdee5ad70b3acb23117ee003877f52a55e
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592456"
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
