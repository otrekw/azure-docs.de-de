---
title: Azure SignalR als Event Grid-Quelle
description: Beschreibt die Eigenschaften, die mit Azure Event Grid für SignalR-Ereignisse bereitgestellt werden
services: event-grid
author: femila
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/23/2020
ms.author: femila
ms.openlocfilehash: 8fbae3fad4aeb85022c804e1ac648060360c6531
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84560627"
---
# <a name="azure-event-grid-event-schema-for-signalr-service"></a>Azure Event Grid-Ereignisschema für SignalR

In diesem Artikel werden die Eigenschaften und das Schema für SignalR-Ereignisse beschrieben. Eine Einführung in Ereignisschemas finden Sie unter [Azure Event Grid-Ereignisschema](event-schema.md). Außerdem erhalten Sie eine Liste mit Schnellstarts und Tutorials, die Azure SignalR als Ereignisquelle verwenden.

## <a name="event-grid-event-schema"></a>Event Grid-Ereignisschema

### <a name="available-event-types"></a>Verfügbare Ereignistypen

SignalR Service gibt die folgenden Ereignistypen aus:

| Ereignistyp | BESCHREIBUNG |
| ---------- | ----------- |
| Microsoft.SignalRService.ClientConnectionConnected | Wird ausgelöst, wenn eine Clientverbindung verbunden wird. |
| Microsoft.SignalRService.ClientConnectionDisconnected | Wird ausgelöst, wenn eine Clientverbindung getrennt wird. |

### <a name="example-event"></a>Beispielereignis

Das folgende Beispiel zeigt das Schema eines Ereignisses vom Typ „Clientverbindung verbunden“: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "eventType": "Microsoft.SignalRService.ClientConnectionConnected",
  "eventTime": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Das Schema für ein Ereignis vom Typ „Clientverbindung getrennt“ ist ähnlich: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "eventType": "Microsoft.SignalRService.ClientConnectionDisconnected",
  "eventTime": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23",
    "errorMessage": "Internal server error."
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

### <a name="event-properties"></a>Ereigniseigenschaften

Ein Ereignis weist die folgenden Daten auf oberster Ebene aus:

| Eigenschaft | type | BESCHREIBUNG |
| -------- | ---- | ----------- |
| topic | Zeichenfolge | Vollständiger Ressourcenpfaf zur Ereignisquelle. Dieses Feld ist nicht beschreibbar. Dieser Wert wird von Event Grid bereitgestellt. |
| subject | Zeichenfolge | Vom Herausgeber definierter Pfad zum Ereignisbetreff |
| eventType | Zeichenfolge | Einer der registrierten Ereignistypen für die Ereignisquelle. |
| eventTime | Zeichenfolge | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| id | Zeichenfolge | Eindeutiger Bezeichner für das Ereignis. |
| data | Objekt (object) | SignalR Service-Ereignisdaten. |
| dataVersion | Zeichenfolge | Die Schemaversion des Datenobjekts. Der Herausgeber definiert die Schemaversion. |
| metadataVersion | Zeichenfolge | Die Schemaversion der Ereignismetadaten. Event Grid definiert das Schema der Eigenschaften der obersten Ebene. Dieser Wert wird von Event Grid bereitgestellt. |

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | type | BESCHREIBUNG |
| -------- | ---- | ----------- |
| timestamp | Zeichenfolge | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| hubName | Zeichenfolge | Der Hub, zu dem die Clientverbindung gehört. |
| connectionId | Zeichenfolge | Der eindeutige Bezeichner für die Clientverbindung. |
| userId | Zeichenfolge | Der im Anspruch definierte Benutzerbezeichner. |
| errorMessage | Zeichenfolge | Der Fehler, der bewirkt, dass die Verbindung getrennt wird. |

## <a name="tutorials-and-how-tos"></a>Tutorials und Vorgehensweisen
|Titel | BESCHREIBUNG |
|---------|---------|
| [Reagieren auf Azure SignalR Service-Ereignisse](../azure-signalr/signalr-concept-event-grid-integration.md) | Übersicht über die Integration von Azure SignalR Service in Event Grid. |
| [Senden von Ereignissen von Azure SignalR Service an Event Grid](../azure-signalr/signalr-howto-event-grid-integration.md) | Hier wird gezeigt, wie Sie Azure SignalR Service-Ereignisse über Event Grid an eine Anwendung senden. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Azure Event Grid finden Sie unter [Einführung in Azure Event Grid](overview.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).
