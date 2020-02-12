---
title: Reagieren auf Kartenereignisse mithilfe von Event Grid | Microsoft Azure Maps
description: In diesem Artikel erfahren Sie, wie Sie mithilfe von Event Grid auf Microsoft Azure Maps-Ereignisse reagieren.
author: walsehgal
ms.author: v-musehg
ms.date: 02/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: a89983a9ae45f21deb7a823de049373b4ff9b935
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2020
ms.locfileid: "76989058"
---
# <a name="react-to-azure-maps-events-by-using-event-grid"></a>Reagieren auf Azure Maps-Ereignisse mithilfe von Event Grid 

Azure Maps ist in Azure Event Grid integriert, sodass Benutzer Ereignisbenachrichtigungen an andere Dienste senden und nachfolgende Prozesse auslösen können. Dieser Artikel soll Ihnen dabei helfen, Ihre Geschäftsanwendungen so zu konfigurieren, dass sie auf Azure Maps-Ereignisse lauschen. Mit diesem Dienst können Sie auf zuverlässige, skalierbare und sichere Weise schnell auf kritische Ereignisse reagieren. Benutzer können beispielsweise eine Anwendung zum Aktualisieren einer Datenbank, Erstellen eines Tickets und Übermitteln einer E-Mail-Benachrichtigung bei jedem Eintreten eines Geräts in einen Geofence erstellen.

Azure Event Grid ist ein vollständig verwalteter Ereignisroutingdienst, der ein Veröffentlichen-Abonnieren-Modell verwendet. Event Grid verfügt über eine integrierte Unterstützung für Azure-Dienste wie [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview) und [Azure Logic Apps](https://docs.microsoft.com/azure/azure-functions/functions-overview). Mithilfe von Webhooks können Ereigniswarnungen an Azure-fremde Dienste übertragen werden. Eine vollständige Liste der Ereignishandler, die Event Grid unterstützt, finden Sie unter [Einführung in Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview).


![Azure Event Grid – funktionales Modell](./media/azure-maps-event-grid-integration/azure-event-grid-functional-model.png)


## <a name="azure-maps-events-types"></a>Azure Maps-Ereignistypen

Event Grid verwendet [Ereignisabonnements](https://docs.microsoft.com/azure/event-grid/concepts#event-subscriptions) zum Weiterleiten von Ereignisnachrichten an Abonnenten. Ein Azure Maps-Konto gibt die folgenden Ereignistypen aus: 

| Ereignistyp | Beschreibung |
| ---------- | ----------- |
| Microsoft.Maps.GeofenceEntered | Wird ausgelöst, wenn die empfangenen Koordinaten sich von außerhalb eines bestimmten Geofence nach innerhalb bewegt haben |
| Microsoft.Maps.GeofenceExited | Wird ausgelöst, wenn die empfangenen Koordinaten sich von innerhalb eines bestimmten Geofence nach außerhalb bewegt haben |
| Microsoft.Maps.GeofenceResult | Wird jedes Mal ausgelöst, wenn eine Geofencingabfrage ein Ergebnis zurückgibt, unabhängig vom Status |

## <a name="event-schema"></a>Ereignisschema

Das folgende Beispiel zeigt das Schema für GeofenceResult:

```JSON
{   
   "id":"451675de-a67d-4929-876c-5c2bf0b2c000", 
   "topic":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Maps/accounts/{accountName}", 
   "subject":"/spatial/geofence/udid/{udid}/id/{eventId}", 
   "data":{   
      "geometries":[   
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"1", 
            "distance":999.0, 
            "nearestLat":47.609833, 
            "nearestLon":-122.148274 
         }, 
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"2", 
            "distance":999.0, 
            "nearestLat":47.621954, 
            "nearestLon":-122.131841 
         } 
      ], 
      "expiredGeofenceGeometryId":[   
      ], 
      "invalidPeriodGeofenceGeometryId":[   
      ] 
   }, 
   "eventType":"Microsoft.Maps.GeofenceResult", 
   "eventTime":"2018-11-08T00:52:08.0954283Z", 
   "metadataVersion":"1", 
   "dataVersion":"1.0" 
}
```

## <a name="tips-for-consuming-events"></a>Tipps zum Nutzen von Ereignissen

Anwendungen, die Azure Maps-Geofencingereignisse behandeln, sollten einige bewährte Methoden nutzen:

* Es können mehrere Abonnements konfiguriert werden, um Ereignisse an den gleichen Ereignishandler zu routen. Es ist wichtig, nicht davon auszugehen, dass Ereignisse aus einer bestimmten Quelle stammen. Überprüfen Sie in jedem Fall das Nachrichtenthema, um sicherzustellen, dass es von der Quelle stammt, die Sie erwarten.
* Nachrichten können in falscher Reihenfolge oder nach einer Verzögerung eingehen. Verwenden Sie das `X-Correlation-id`-Feld im Antwortheader, um zu überprüfen, ob Ihre Informationen zu Objekten aktuell sind.
* Wenn die Get- und POST-Geofence-API mit einem auf `EnterAndExit` festgelegten Modusparameter aufgerufen wird, wird für jede Geometrie im Geofence, deren Status sich gegenüber dem vorherigen Geofence-API-Aufruf geändert hat, ein Eintritts- oder Austrittsereignis erzeugt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von Geofencing zum Steuern von Vorgängen auf einer Baustelle finden Sie unter:

> [!div class="nextstepaction"] 
> [Einrichten eines Geofence mithilfe von Azure Maps](tutorial-geofence.md)
