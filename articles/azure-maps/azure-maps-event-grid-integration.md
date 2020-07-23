---
title: Reagieren auf Azure Maps-Ereignisse mithilfe von Event Grid
description: In diesem Artikel erfahren Sie, wie Sie mithilfe von Event Grid auf Microsoft Azure Maps-Ereignisse reagieren.
author: anastasia-ms
ms.author: v-stharr
ms.date: 07/16/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: eb64634f25564abc4044364950b4d462a22608aa
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86499510"
---
# <a name="react-to-azure-maps-events-by-using-event-grid"></a>Reagieren auf Azure Maps-Ereignisse mithilfe von Event Grid

Azure Maps ist in Azure Event Grid integriert, sodass Benutzer Ereignisbenachrichtigungen an andere Dienste senden und nachfolgende Prozesse auslösen können. Dieser Artikel soll Ihnen dabei helfen, Ihre Geschäftsanwendungen so zu konfigurieren, dass sie auf Azure Maps-Ereignisse lauschen. Hiermit können Benutzer auf zuverlässige, skalierbare und sichere Weise schnell auf kritische Ereignisse reagieren. Benutzer können beispielsweise eine Anwendung zum Aktualisieren einer Datenbank, Erstellen eines Tickets und Übermitteln einer E-Mail-Benachrichtigung bei jedem Eintreten eines Geräts in einen Geofence erstellen.

Azure Event Grid ist ein vollständig verwalteter Ereignisroutingdienst, der ein Veröffentlichen-Abonnieren-Modell verwendet. Event Grid verfügt über eine integrierte Unterstützung für Azure-Dienste wie [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview) und [Azure Logic Apps](https://docs.microsoft.com/azure/azure-functions/functions-overview). Mithilfe von Webhooks können Ereigniswarnungen an Azure-fremde Dienste übertragen werden. Eine vollständige Liste der Ereignishandler, die Event Grid unterstützt, finden Sie unter [Einführung in Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview).


![Azure Event Grid – funktionales Modell](./media/azure-maps-event-grid-integration/azure-event-grid-functional-model.png)


## <a name="azure-maps-events-types"></a>Azure Maps-Ereignistypen

Event Grid verwendet [Ereignisabonnements](https://docs.microsoft.com/azure/event-grid/concepts#event-subscriptions) zum Weiterleiten von Ereignisnachrichten an Abonnenten. Ein Azure Maps-Konto gibt die folgenden Ereignistypen aus: 

| Ereignistyp | BESCHREIBUNG |
| ---------- | ----------- |
| Microsoft.Maps.GeofenceEntered | Wird ausgelöst, wenn empfangene Koordinaten sich von außerhalb eines bestimmten Geofence nach innerhalb bewegt haben. |
| Microsoft.Maps.GeofenceExited | Wird ausgelöst, wenn empfangene Koordinaten sich von innerhalb eines bestimmten Geofence nach außerhalb bewegt haben. |
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

* Konfigurieren von mehreren Abonnements, um Ereignisse an denselben Ereignishandler zu routen. Es ist wichtig, nicht davon auszugehen, dass Ereignisse aus einer bestimmten Quelle stammen. Überprüfen Sie in jedem Fall das Nachrichtenthema, um sicherzustellen, dass die Nachricht von der Quelle stammt, die Sie erwarten.
* Verwenden Sie das `X-Correlation-id`-Feld im Antwortheader, um zu überprüfen, ob Ihre Informationen zu Objekten aktuell sind. Nachrichten können in falscher Reihenfolge oder nach einer Verzögerung eingehen.
* Wenn eine GET- oder POST-Anforderung in der Geofence-API mit dem auf `EnterAndExit` festgelegten mode-Parameter (Modus) aufgerufen wird, wird für jede Geometrie im Geofence, deren Status sich gegenüber dem vorherigen Geofence-API-Aufruf geändert hat, ein Eintritts- oder Austrittsereignis erzeugt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von Geofencing zum Steuern von Vorgängen auf einer Baustelle finden Sie unter:

> [!div class="nextstepaction"] 
> [Einrichten eines Geofence mithilfe von Azure Maps](tutorial-geofence.md)
