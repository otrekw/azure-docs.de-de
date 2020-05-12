---
title: 'Azure Event Grid: Aktivieren von Diagnoseprotokollen für Themen oder Domänen'
description: Dieser Artikel enthält schrittweise Anleitungen zum Aktivieren von Diagnoseprotokollen für ein Azure Event Grid-Thema.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: how-to
ms.date: 04/27/2020
ms.author: spelluru
ms.openlocfilehash: 13a2168c854475b841b0ebc52bb678c7ca22a1bb
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82626461"
---
#  <a name="enable-diagnostic-logs-for-azure-event-grid-topics-or-domains"></a>Aktivieren von Diagnoseprotokollen für Azure Event Grid-Themen oder -Domänen
Diagnoseeinstellungen ermöglichen es Event Grid-Benutzern, Protokolle zu **Veröffentlichungs- und Übermittlungsfehlern** entweder in einem Speicherkonto, einem Event Hub oder einem Log Analytics-Arbeitsbereich zu erfassen und anzuzeigen. Dieser Artikel enthält schrittweise Anleitungen zum Aktivieren dieser Einstellungen für ein Event Grid-Thema.

## <a name="prerequisites"></a>Voraussetzungen

- Ein bereitgestelltes Event Grid-Thema
- Ein bereitgestelltes Ziel zum Erfassen von Diagnoseprotokollen Es kann eines der folgenden Ziele am gleichen Speicherort wie das Event Grid-Thema sein:
    - Azure-Speicherkonto
    - Event Hub
    - Log Analytics-Arbeitsbereich

## <a name="steps-for-enabling-diagnostic-logs-for-a-topic"></a>Schritte zum Aktivieren von Diagnoseprotokollen für ein Thema

> [!NOTE]
> Das folgende Verfahren bietet eine schrittweise Anleitung zum Aktivieren von Diagnoseprotokollen für ein Thema. Schritte zum Aktivieren von Diagnoseprotokollen für eine Domäne sind sehr ähnlich. Navigieren Sie in Schritt 2 zur Event Grid-**Domäne** im Azure-Portal.  

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Navigieren Sie zum Event Grid-Thema, für das Sie Diagnoseprotokolleinstellungen aktivieren möchten. 
3. Wählen Sie unter **Überwachung** im linken Menü die Option **Diagnoseeinstellungen** aus.
4. Wählen Sie auf der Seite **Diagnoseeinstellungen** den Eintrag **Diagnoseeinstellung hinzufügen** aus. 
    
    ![Schaltfläche „Diagnoseeinstellung hinzufügen“](./media/enable-diagnostic-logs-topic/diagnostic-settings-add.png)
5. Geben Sie einen **Namen** für die Diagnoseeinstellung an. 
7. Wählen Sie im Abschnitt **Protokoll** die Optionen **DeliveryFailures** und **PublishFailures** aus. 
    ![Auswählen der Fehler](./media/enable-diagnostic-logs-topic/log-failures.png)
6. Aktivieren Sie mindestens eines der Erfassungsziele für die Protokolle, und konfigurieren Sie diese dann, indem Sie eine zuvor erstellte Erfassungsressource auswählen. 
    - Wenn Sie **In ein Speicherkonto archivieren** auswählen, wählen Sie **Speicherkonto - Konfigurieren** aus, und wählen Sie dann das Speicherkonto in Ihrem Azure-Abonnement aus. 

        ![Archivieren in ein Azure-Speicherkonto](./media/enable-diagnostic-logs-topic/archive-storage.png)
    - Wenn Sie **An einen Event Hub streamen** auswählen, wählen Sie **Event Hub - Konfigurieren** aus, und wählen Sie dann den Event Hubs-Namespace, den Event Hub und die Zugriffsrichtlinie aus. 
        ![An einen Event Hub streamen](./media/enable-diagnostic-logs-topic/archive-event-hub.png)
    - Wenn Sie **An Log Analytics senden** auswählen, wählen Sie den Log Analytics-Arbeitsbereich aus.
        ![An Log Analytics senden](./media/enable-diagnostic-logs-topic/send-log-analytics.png)
8. Wählen Sie **Speichern** aus. Wählen Sie dann in der rechten oberen Ecke **X** aus, um die Seite zu schließen. 
9. Vergewissern Sie sich nun auf der Seite **Diagnoseeinstellungen**, dass ein neuer Eintrag in der Tabelle **Diagnoseeinstellungen** angezeigt wird. 
    ![Diagnoseeinstellung in der Liste](./media/enable-diagnostic-logs-topic/diagnostic-setting-list.png)

     Sie können auch die Erfassung aller Metriken für das Thema aktivieren. 

## <a name="view-diagnostic-logs-in-azure-storage"></a>Anzeigen von Diagnoseprotokollen in Azure Storage 

1. Nachdem Sie ein Speicherkonto als Erfassungsziel aktiviert haben und Event Grid mit der Ausgabe von Diagnoseprotokollen beginnt, sollten Sie neue Container mit den Namen **insights-logs-deliveryfailures** und **insights-logs-publishfailures** im Speicherkonto sehen. 

    ![Speicher: Container für Diagnoseprotokolle](./media/enable-diagnostic-logs-topic/storage-containers.png)
2. Wenn Sie durch einen der Container navigieren, gelangen Sie zu einem Blob im JSON-Format. Die Datei enthält Protokolleinträge entweder für einen Zustellungs- oder Veröffentlichungsfehler. Der Navigationspfad stellt die **Ressourcen-ID** des Event Grid-Themas und den Zeitstempel (auf Minutenebene) dar, wann die Protokolleinträge ausgegeben wurden. Die Blob/JSON-Datei, die heruntergeladen werden kann, hält sich letztlich an das im nächsten Abschnitt beschriebene Schema. 

    [ ![JSON-Datei im Speicher](./media/enable-diagnostic-logs-topic/select-json.png) ](./media/enable-diagnostic-logs-topic/select-json.png)
3. Sie sollten den Inhalt in der JSON-Datei ähnlich wie im folgenden Beispiel sehen: 

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
Informationen zum Protokollschema und andere konzeptionelle Informationen zu Diagnoseprotokollen für Themen oder Domänen finden Sie unter [Diagnoseprotokolle](diagnostic-logs.md).
