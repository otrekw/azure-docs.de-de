---
title: 'Azure Event Grid: Diagnoseprotokolle für Themen oder Domänen'
description: In diesem Artikel erhalten Sie konzeptionelle Informationen über Diagnoseprotokolle für ein Azure Event Grid-Thema oder eine -Domäne.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: c8f6734d9d43887d0eb0bb90bb08f727732feac3
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86116742"
---
#  <a name="diagnostic-logs-for-azure-event-grid-topicsdomains"></a>Diagnoseprotokolle für Azure Event Grid-Themen und -Domänen
Diagnoseeinstellungen ermöglichen es Event Grid-Benutzern, Protokolle zu **Veröffentlichungs- und Zustellungsfehlern** entweder in einem Speicherkonto, einer Event Hub-Instanz oder einem Log Analytics-Arbeitsbereich zu erfassen und anzuzeigen. In diesem Artikel werden das Schema für die Protokolle und ein Beispielprotokolleintrag bereitgestellt.


## <a name="schema-for-publishdelivery-failure-logs"></a>Schema für Veröffentlichungs-/Zustellungsfehlerprotokolle

| Eigenschaftenname | Datentyp | BESCHREIBUNG |
| ------------- | --------- | ----------- | 
| Time | Datetime | Der Zeitpunkt, zu dem der Protokolleintrag generiert wurde. <p>**Beispielwert:**  01-29-2020 09:52:02.700</p> |
| EventSubscriptionName | String | Der Name des Ereignisabonnements. <p>**Beispielwert:** „EVENTSUB1“</p> <p>Diese Eigenschaft ist nur für Zustellungsfehlerprotokolle vorhanden.</p>  |
| Category | String | Der Name der Protokollkategorie. <p>**Beispielwerte:** „DeliveryFailures“ oder „PublishFailures“ | 
| Vorgangsname | String | Der Name des Vorgangs, der beim Auftreten des Fehlers ausgeführt wurde.<p>**Beispielwerte:** „Deliver“ für Zustellungsfehler. |
| `Message` | String | Die Protokollnachricht für den Benutzer, in der der Grund für den Fehler und andere zusätzliche Details erläutert werden. |
| resourceId | String | Die Ressourcen-ID für die Themen-/Domänenressource.<p>**Beispielwerte:** `/SUBSCRIPTIONS/SAMPLE-SUBSCRIPTION-ID/RESOURCEGROUPS/SAMPLE-RESOURCEGROUP/PROVIDERS/MICROSOFT.EVENTGRID/TOPICS/TOPIC1` |

## <a name="example"></a>Beispiel

```json
{
    "time": "2019-11-01T00:17:13.4389048Z",
    "resourceId": "/SUBSCRIPTIONS/SAMPLE-SUBSCTIPTION-ID /RESOURCEGROUPS/SAMPLE-RESOURCEGROUP-NAME/PROVIDERS/MICROSOFT.EVENTGRID/TOPICS/SAMPLE-TOPIC-NAME ",
    "eventSubscriptionName": "SAMPLEDESTINATION",
    "category": "DeliveryFailures",
    "operationName": "Deliver",
    "message": "Message:outcome=NotFound, latencyInMs=2635, systemId=17284f7c-0044-46fb-84b7-59fda5776017, state=FilteredFailingDelivery, deliveryTime=11/1/2019 12:17:10 AM, deliveryCount=0, probationCount=0, deliverySchema=EventGridEvent, eventSubscriptionDeliverySchema=EventGridEvent, fields=InputEvent, EventSubscriptionId, DeliveryTime, State, Id, DeliverySchema, LastDeliveryAttemptTime, SystemId, fieldCount=, requestExpiration=1/1/0001 12:00:00 AM, delivered=False publishTime=11/1/2019 12:17:10 AM, eventTime=11/1/2019 12:17:09 AM, eventType=Type, deliveryTime=11/1/2019 12:17:10 AM, filteringState=FilteredWithRpc, inputSchema=EventGridEvent, publisher=DIAGNOSTICLOGSTEST-EASTUS.EASTUS-1.EVENTGRID.AZURE.NET, size=363, fields=Id, PublishTime, SerializedBody, EventType, Topic, Subject, FilteringHashCode, SystemId, Publisher, FilteringTopic, TopicCategory, DataVersion, MetadataVersion, InputSchema, EventTime, fieldCount=15, url=sb://diagnosticlogstesting-eastus.servicebus.windows.net/, deliveryResponse=NotFound: The messaging entity 'sb://diagnosticlogstesting-eastus.servicebus.windows.net/eh-diagnosticlogstest' could not be found. TrackingId:c98c5af6-11f0-400b-8f56-c605662fb849_G14, SystemTracker:diagnosticlogstesting-eastus.servicebus.windows.net:eh-diagnosticlogstest, Timestamp:2019-11-01T00:17:13, referenceId: ac141738a9a54451b12b4cc31a10dedc_G14:"
}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Aktivieren von Diagnoseprotokollen für Themen oder Domänen finden Sie unter [Aktivieren von Diagnoseprotokollen](enable-diagnostic-logs-topic.md).
