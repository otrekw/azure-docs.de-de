---
title: Azure Event Grid-Abonnementschema
description: In diesem Artikel werden die Eigenschaften für das Abonnieren eines Ereignisses mit Azure Event Grid beschrieben. Event Grid-Abonnementschema
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: reference
ms.date: 01/23/2020
ms.author: babanisa
ms.openlocfilehash: 4bb04d22b762f31a02515549b698030a5267e4cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720757"
---
# <a name="event-grid-subscription-schema"></a>Event Grid-Abonnementschema

Zum Erstellen eines Event Grid-Abonnements senden Sie eine Anforderung an den Vorgang zum Erstellen eines Ereignisabonnements. Verwenden Sie das folgende Format:

```HTTP
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Verwenden Sie also beispielsweise Folgendes, um ein Ereignisabonnement für ein Speicherkonto namens `examplestorage` in einer Ressourcengruppe namens `examplegroup` zu erstellen:

```HTTP
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Der Name des Ereignisabonnements muss 3 bis 64 Zeichen umfassen und darf nur die Zeichen a–Z, A–Z, 0–9 und „-“ enthalten. Der Artikel beschreibt die Eigenschaften und das Schema für den Hauptteil der Anforderung.
 
## <a name="event-subscription-properties"></a>Eigenschaften für Ereignisabonnements

| Eigenschaft | type | BESCHREIBUNG |
| -------- | ---- | ----------- |
| destination | Objekt (object) | Das Objekt, das den Endpunkt definiert. |
| filter | Objekt (object) | Ein optionales Feld zum Filtern der Ereignistypen. |

### <a name="destination-object"></a>destination-Objekt

| Eigenschaft | type | BESCHREIBUNG |
| -------- | ---- | ----------- |
| endpointType | string | Die Art des Endpunkts für das Abonnement (Webhook/HTTP, Event Hub oder Warteschlange). | 
| endpointUrl | string | Die Ziel-URL für Ereignisse in diesem Ereignisabonnement. | 

### <a name="filter-object"></a>filter-Objekt

| Eigenschaft | type | BESCHREIBUNG |
| -------- | ---- | ----------- |
| includedEventTypes | array | Übereinstimmung, wenn der Ereignistyp in der Ereignisnachricht eine exakte Übereinstimmung für einen der Ereignistypnamen ist. Fehler, wenn der Ereignisname nicht den registrierten Ereignistypnamen für die Ereignisquelle entspricht. Der Standardwert entspricht allen Ereignistypen. |
| subjectBeginsWith | string | Ein Präfixabgleichfilter für das Betrefffeld in der Ereignisnachricht. Der Standardwert oder eine leere Zeichenfolge entspricht allen Optionen. | 
| subjectEndsWith | string | Ein Suffixabgleichfilter für das Betrefffeld in der Ereignisnachricht. Der Standardwert oder eine leere Zeichenfolge entspricht allen Optionen. |
| isSubjectCaseSensitive | string | Steuert, ob beim Abgleich von Filtern die Groß-/Kleinschreibung beachtet wird. |


## <a name="example-subscription-schema"></a>Beispiel für das Abonnementschema

```json
{
  "properties": {
    "destination": {
      "endpointType": "webhook",
      "properties": {
          "endpointUrl": "https://example.azurewebsites.net/api/HttpTriggerCSharp1?code=VXbGWce53l48Mt8wuotr0GPmyJ/nDT4hgdFj9DpBiRt38qqnnm5OFg=="
      }
    },
    "filter": {
      "includedEventTypes": [ "Microsoft.Storage.BlobCreated", "Microsoft.Storage.BlobDeleted" ],
      "subjectBeginsWith": "/blobServices/default/containers/mycontainer/log",
      "subjectEndsWith": ".jpg",
      "isSubjectCaseSensitive ": "true"
    }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
