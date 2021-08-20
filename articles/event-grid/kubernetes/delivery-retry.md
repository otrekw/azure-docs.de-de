---
title: 'Azure Event Grid in Kubernetes: Ereignisübermittlung und Neuversuch'
description: In diesem Artikel wird beschrieben, wie Event Grid in Kubernetes Ereignisse übermittelt und wie nicht übermittelte Nachrichten verarbeitet werden.
author: jfggdl
ms.subservice: kubernetes
ms.author: jafernan
ms.date: 05/25/2021
ms.topic: conceptual
ms.openlocfilehash: 6a37945461a61167e2fee7d7d3ef6a8fbccf3372
ms.sourcegitcommit: e7d500f8cef40ab3409736acd0893cad02e24fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122340487"
---
# <a name="event-grid-on-kubernetes---event-delivery-and-retry"></a>Event Grid in Kubernetes: Ereignisübermittlung und Neuversuch
Event Grid in Kubernetes mit Azure Arc versucht, für jedes übereinstimmende Abonnement jede Nachricht mindestens einmal sofort zu übermitteln. Wenn keine erfolgreiche HTTP 200-Antwort vom Abonnenten eingeht oder ein Fehler auftritt, wiederholt Event Grid in Kubernetes die Übermittlung auf Basis eines festgelegten Wiederholungszeitplans und einer Wiederholungsrichtlinie. 

Standardmäßig übermittelt Event Grid in Kubernetes immer nur jeweils ein Ereignis an den Abonnenten. Die Nutzlast der Übermittlungsanforderung ist jedoch ohnehin ein Array mit einem einzelnen Ereignis. Es können jeweils mehrere Ereignisse übermittelt werden, wenn Sie die Ausgabebatchverarbeitungs-Funktion aktivieren. Ausführlichere Informationen zu dieser Funktion finden Sie unter [Batchereignisübermittlung](batch-event-delivery.md).

[!INCLUDE [event-grid-preview-feature-note.md](../includes/event-grid-preview-feature-note.md)]

> [!NOTE]
> Während der Vorschau werden die Funktionen von Event Grid in Kubernetes durch die API-Version [2020-10-15-Preview](/rest/api/eventgrid/version2021-06-01-preview/event-subscriptions/create-or-update) unterstützt. 


## <a name="retry-schedule"></a>Wiederholungszeitplan
Event Grid in Kubernetes wartet nach der Zustellung eines Ereignisses bis zu 60 Sekunden auf eine Antwort. Wenn der Endpunkt des Abonnenten keine Erfolgsantwort (HTTP 200 oder Ähnliches) sendet, wird erneut versucht, das Ereignis zu senden. Dies funktioniert folgendermaßen: 

1. Die Nachricht geht bei Event Grid in Kubernetes ein. Es wird versucht, sie sofort zu übermitteln.
1. Wenn bei der Übermittlung ein Fehler auftritt, wird die Nachricht in die 1-Minute-Warteschlange eingereiht, und nach einer Minute erfolgt ein neuer Versuch.
1. Wenn bei der Übermittlung weiterhin ein Fehler auftritt, wird die Nachricht in die 10-Minuten-Warteschlange eingereiht, und alle 10 Minuten erfolgt ein neuer Versuch.
1. Die Übermittlung wird so oft versucht, bis sie erfolgreich ist oder die Limits der Wiederholungsrichtlinien erreicht werden.
 
## <a name="retry-policy"></a>Wiederholungsrichtlinie
Es gibt zwei Konfigurationen, die die Wiederholungsrichtlinien regeln. Sie lauten wie folgt:

- Maximale Anzahl von Versuchen
- Gültigkeitsdauer des Ereignisses (TTL)

Ein Ereignis wird gelöscht, wenn eines dieser Limits der Wiederholungsrichtlinie erreicht wird. Die Konfiguration dieser Limits erfolgt auf Abonnementbasis. Im folgenden Abschnitt werden beide Möglichkeiten detaillierter beschrieben.

### <a name="configuring-defaults-per-subscriber"></a>Konfigurieren von Standardwerten pro Abonnent
Sie können auch Limits für Wiederholungsrichtlinien pro Abonnement angeben. Informationen zum Konfigurieren von Standardeinstellungen pro Abonnent finden Sie in unserer [API-Dokumentation](/rest/api/eventgrid/version2021-06-01-preview/event-subscriptions/create-or-update). Standardeinstellungen auf Abonnementebene setzen die Konfigurationen auf Ebene des Event Grid-Moduls in Kubernetes außer Kraft.

Im folgenden Beispiel wird für ein Webhookabonnement `maxNumberOfAttempts` auf 3 und `eventTimeToLiveInMinutes` auf 30 Minuten festgelegt.

```json
{
 "properties": {
  "destination": {
   "endpointType": "WebHook",
   "properties": {
    "endpointUrl": "<your_webhook_url>",
    "eventDeliverySchema": " CloudEventSchemaV1_0"
   }
  },
  "retryPolicy": {
   "eventTimeToLiveInMinutes": 30,
   "maxDeliveryAttempts": 3
  }
 }
```

## <a name="next-steps"></a>Nächste Schritte
Informationen zu Zielen und Handlern, die von Event Grid in Azure Arc für Kubernetes unterstützt werden, finden Sie unter [Event Grid in Kubernetes – Ereignishandler](event-handlers.md).
