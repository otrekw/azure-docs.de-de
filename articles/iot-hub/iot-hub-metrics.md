---
title: Überwachen von Azure IoT Hub mithilfe von Metriken | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von Azure IoT Hub-Metriken die allgemeine Integrität Ihrer IoT Hub-Instanzen bewerten und überwachen.
author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: jlian
ms.openlocfilehash: c448d7e5a5e0bea29063930bed3a59a0461b8cf5
ms.sourcegitcommit: d9ba60f15aa6eafc3c5ae8d592bacaf21d97a871
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91767626"
---
<!--for build: for each metric, if you understand what it is, it's ok. otw add more info.  -->
# <a name="understand-iot-hub-metrics"></a>Grundlegendes zu IoT Hub-Metriken

IoT Hub-Metriken liefern Informationen zum Zustand der Azure IoT-Ressourcen in Ihrem Azure-Abonnement. IoT Hub-Metriken helfen Ihnen dabei, die allgemeine Integrität des IoT Hub-Diensts und der mit ihm verbundenen Geräte zu beurteilen. Anhand dieser benutzerorientierten Statistiken können Sie Vorgänge in Ihrem IoT Hub erkennen und die Ursache von Problemen analysieren, ohne den Azure-Support kontaktieren zu müssen.

Metriken sind standardmäßig aktiviert. IoT Hub-Metriken können über das Azure-Portal angezeigt werden.

> [!NOTE]
> Sie können mithilfe von IoT Hub-Metriken Informationen zu [IoT Plug & Play](../iot-pnp/overview-iot-plug-and-play.md)-Geräten anzeigen, die mit Ihrem IoT Hub verbunden sind.

## <a name="how-to-view-iot-hub-metrics"></a>Anzeigen von IoT Hub-Metriken

1. Erstellen Sie einen IoT Hub. Anweisungen zum Erstellen eines IoT-Hubs finden Sie im Leitfaden [Senden von Telemetriedaten von einem Gerät an IoT Hub](quickstart-send-telemetry-dotnet.md).

2. Öffnen Sie das Blatt Ihres IoT Hubs. Klicken Sie dort auf **Metriken**.
   
    ![Screenshot, in dem dargestellt ist, wo sich die Option „Metriken“ auf der Seite für IoT Hub-Ressourcen befindet](./media/iot-hub-metrics/enable-metrics-1.png)

3. Auf dem Blatt „Metriken“ können Sie die Metriken für Ihren IoT-Hub anzeigen und benutzerdefinierte Metrikansichten erstellen. 
   
    ![Screenshot der Seite „Metriken“ für IoT Hub](./media/iot-hub-metrics/enable-metrics-2.png)
    
4. Sie können Ihre Metrikdaten an einen Event Hubs-Endpunkt oder ein Azure Storage-Konto senden, indem Sie auf **Diagnoseeinstellungen** und dann auf **Diagnoseeinstellung hinzufügen** klicken.

   ![Screenshot, in dem dargestellt ist, wo sich die Schaltfläche „Diagnoseeinstellungen“ befindet](./media/iot-hub-metrics/enable-metrics-3.png)

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IoT Hub-Metriken und ihre Verwendung

IoT Hub bietet mehrere Metriken, um Ihnen einen Überblick über die Integrität Ihres Hubs und die Gesamtzahl der verbundenen Geräte zu verschaffen. Sie können Informationen aus mehreren Metriken kombinieren, um sich ein umfassenderes Bild des Zustands des IoT Hubs zu machen. In der folgenden Tabelle werden die Metriken beschrieben, die für IoT Hubs nachverfolgt werden. Außerdem erfahren Sie, in welchem Bezug die einzelnen Metriken zum allgemeinen Status des IoT Hubs stehen.

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|BESCHREIBUNG|Dimensionen|
|---|---|---|---|---|---|
|RoutingDeliveries | Routingzustellversuche (Vorschau) | Anzahl | Gesamt |Dies ist die Metrik zur Routingzustellung. Identifizieren Sie anhand der Dimensionen den Zustellungsstatus für einen bestimmten Endpunkt oder eine bestimmte Routingquelle.| ResourceID,<br>Result,<br>RoutingSource,<br>EndpointType,<br>FailureReasonCategory,<br>EndpointName<br>*Weitere Details zu Dimensionen finden Sie [**hier**](#dimensions)* . |
|RoutingDeliveryLatency| Routinglatenz (Vorschau) | Millisekunden | Average |Dies ist die Metrik zur Routingzustellungslatenz. Identifizieren Sie anhand der Dimensionen die Latenz für einen bestimmten Endpunkt oder eine bestimmte Routingquelle.| ResourceID,<br>RoutingSource,<br>EndpointType,<br>EndpointName<br>*Weitere Details zu Dimensionen finden Sie [**hier**](#dimensions)* .|
|RoutingDataSizeInBytesDelivered| Größe der Routingzustellungsdaten in Bytes (Vorschau)| Byte | Gesamt |Die Gesamtanzahl von Bytes, die von IoT Hub an einen benutzerdefinierten und einen integrierten Endpunkt weitergeleitet wurden. Identifizieren Sie anhand der Dimensionen die Größe der Daten, die an einen bestimmten Endpunkt oder eine bestimmte Routingquelle weitergeleitet wurden.| ResourceID,<br>RoutingSource,<br>EndpointType<br>EndpointName<br>*Weitere Details zu Dimensionen finden Sie [**hier**](#dimensions)* .|
|d2c.telemetry.ingress.<br>allProtocol|Telemetry message send attempts (Sendeversuche für Telemetrienachrichten)|Anzahl|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die an Ihren IoT Hub gesendet werden sollten|Keine|
|d2c.telemetry.ingress.<br>success|Telemetry messages sent (Gesendete Telemetrienachrichten)|Anzahl|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die erfolgreich an Ihren IoT Hub gesendet wurden|Keine|
|c2d.commands.egress.<br>complete.success|Abgeschlossene C2D-Nachrichtenübermittlungen|Anzahl|Gesamt|Anzahl von Cloud-zu-Gerät-Nachrichtenübermittlungen, die vom Gerät erfolgreich abgeschlossen wurden|Keine|
|c2d.commands.egress.<br>abandon.success|Abgebrochene C2D-Nachrichten|Anzahl|Gesamt|Anzahl von Cloud-zu-Gerät-Nachrichten, die vom Gerät abgebrochen wurden|Keine|
|c2d.commands.egress.<br>reject.success|Abgelehnte C2D-Nachrichten|Anzahl|Gesamt|Anzahl von Cloud-zu-Gerät-Nachrichten, die vom Gerät abgelehnt wurden|Keine|
|C2DMessagesExpired|Abgelaufene C2D-Nachrichten (Vorschau)|Anzahl|Gesamt|Anzahl von abgelaufenen Cloud-zu-Gerät-Nachrichten|Keine|
|devices.totalDevices|Geräte gesamt (veraltet)|Anzahl|Gesamt|Die Anzahl von Geräten, die beim IoT Hub registriert sind|Keine|
|devices.connectedDevices.<br>allProtocol|Verbundene Geräte (veraltet) |Anzahl|Gesamt|Die Anzahl von Geräten, die mit dem IoT Hub verbunden sind|Keine|
|d2c.telemetry.egress.<br>success|Routing: Übermittelte Telemetrienachrichten|Anzahl|Gesamt|Die Anzahl der erfolgreichen Nachrichtenübermittlungen an alle Endpunkte über das IoT Hub-Routing Wenn eine Nachricht an mehrere Endpunkte weitergeleitet wird, erhöht sich dieser Wert für jede erfolgreiche Übermittlung um eins Wenn eine Nachricht mehrmals an denselben Endpunkt übermittelt wird, erhöht sich dieser Wert für jede erfolgreiche Übermittlung um eins|Keine|
|d2c.telemetry.egress.<br>dropped|Routing: Verworfene Telemetrienachrichten |Anzahl|Gesamt|Die Anzahl der Nachrichten, die vom IoT Hub-Routing aufgrund von inaktiven Endpunkten gelöscht wurden. Dieser Wert zählt nicht die Nachrichten, die an die Fallbackroute übermittelt werden, da gelöschte Nachrichten dort nicht übermittelt werden.|Keine|
|d2c.telemetry.egress.<br>orphaned|Routing: Verwaiste Telemetrienachrichten |Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing zu verwaisten Nachrichten führte, weil sie keiner Routingabfrage entsprachen (wenn die Fallbackroute deaktiviert wurde).|Keine|
|d2c.telemetry.egress.<br>ungültig|Routing: Nicht kompatible Telemetrienachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing Nachrichten aufgrund einer Inkompatibilität mit dem Endpunkt nicht übermitteln konnte. Eine Nachricht ist mit einem Endpunkt inkompatibel, wenn IoT Hub versucht, die Nachricht an den Endpunkt zu übermitteln und ein nicht vorübergehender Fehler auftritt. Es wird nicht versucht, ungültige Nachrichten erneut zuzustellen. Dieser Wert umfasst keine Wiederholungen.|Keine|
|d2c.telemetry.egress.<br>fallback|Routing: An den Fallback übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing Nachrichten an den mit der Fallbackroute verbundenen Endpunkt übermittelt hat.|Keine|
|d2c.endpoints.egress.<br>eventHubs|Routing: An Event Hub übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an benutzerdefinierte Endpunkt vom Typ „Event Hub“ übermittelt hat. Diese Metrik enthält keine an integrierte Endpunkte übermittelten Nachrichten (Ereignisse).|Keine|
|d2c.endpoints.latency.<br>eventHubs|Routing: Nachrichtenwartezeit für Event Hub|Millisekunden|Average|Die durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang einer Nachricht bei IoT Hub und dem Eingang der Nachricht bei benutzerdefinierten Endpunkten vom Typ „Event Hub“. Diese Metrik enthält keine an integrierte Endpunkte übermittelten Nachrichten (Ereignisse).|Keine|
|d2c.endpoints.egress.<br>serviceBusQueues|Routing: An Service Bus-Warteschlange übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Service Bus-Warteschlangenendpunkte übermittelt hat.|Keine|
|d2c.endpoints.latency.<br>serviceBusQueues|Routing: Nachrichtenwartezeit für Service Bus-Warteschlange|Millisekunden|Average|Die durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang einer Nachricht bei IoT Hub und dem Eingang der Nachricht bei einem Service Bus-Warteschlangenendpunkt.|Keine|
|d2c.endpoints.egress.<br>serviceBusTopics|Routing: An Service Bus-Thema übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Service Bus-Themaendpunkte übermittelt hat.|Keine|
|d2c.endpoints.latency.<br>serviceBusTopics|Routing: Nachrichtenwartezeit für Service Bus-Thema|Millisekunden|Average|Die durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang einer Nachricht bei IoT Hub und dem Eingang der Nachricht bei einem Service Bus-Themenendpunkt.|Keine|
|d2c.endpoints.egress.<br>builtIn.events|Routing: An Nachrichten/Ereignisse übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an den integrierter Endpunkt (Nachrichten/Ereignisse) und die Fallbackroute übermittelt hat.|Keine|
|d2c.endpoints.latency.<br>builtIn.events|Routing: Nachrichtenwartezeit für Nachrichten/Ereignisse|Millisekunden|Average|Die durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang einer Nachricht bei IoT Hub und dem Eingang der Nachricht beim integrierten Endpunkt (Nachrichten/Ereignisse) und der Fallbackroute.|Keine|
|d2c.endpoints.egress.<br>storage|Routing: An den Speicher übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Speicherendpunkte übermittelt hat.|Keine|
|d2c.endpoints.latency.<br>storage|Routing: Nachrichtenwartezeit für Speicher|Millisekunden|Average|Die durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang einer Nachricht bei IoT Hub und dem Eingang der Nachricht bei einem Speicherendpunkt.|Keine|
|d2c.endpoints.egress.<br>storage.bytes|Routing: An den Speicher übermittelte Daten|Byte|Gesamt|Die Datenmenge (Bytes), die das IoT Hub-Routing an die Speicherendpunkte übermittelt.|Keine|
|d2c.endpoints.egress.<br>storage.blobs|Routing: An den Speicher übermittelte Blobs|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing Blobs an Speicherendpunkte übermittelt hat.|Keine|
|EventGridDeliveries|Event Grid-Übermittlungen (Vorschau)|Anzahl|Gesamt|Die Anzahl von IoT Hub-Ereignissen, die in Event Grid veröffentlicht werden. Verwenden Sie die Dimension „Result“ für die Anzahl der erfolgreichen und fehlerhaften Anforderungen. Die „EventType“-Dimension zeigt den Typ des Ereignisses an(https://aka.ms/ioteventgrid).|ResourceId,<br/>Result,<br/>EventType|
|EventGridLatency|Event Grid-Wartezeit (Vorschau)|Millisekunden|Average|Die durchschnittliche Wartezeit (in Millisekunden) für den Zeitraum zwischen der Generierung des Iot Hub-Ereignisses und der Veröffentlichung des Ereignisses in Event Grid. Diese Zahl ist ein Durchschnittswert für alle Ereignistypen. Verwenden Sie die Dimension „EventType“, um die Wartezeit für einen bestimmten Ereignistyp anzuzeigen.|ResourceId,<br/>EventType|
|d2c.twin.read.success|Successful twin reads from devices (Erfolgreiche Zwillingslesevorgänge von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.read.failure|Failed twin reads from devices (Nicht erfolgreiche Zwillingslesevorgänge von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.read.size|Response size of twin reads from devices (Antwortgröße von Zwillingslesevorgängen von Geräten)|Byte|Average|Die Anzahl aller erfolgreichen Zwillingslesevorgängen, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.update.success|Successful twin updates from devices (Erfolgreiche Zwillingsaktualisierungen von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.update.failure|Failed twin updates from devices (Nicht erfolgreiche Zwillingsaktualisierungen von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.update.size|Size of twin updates from devices (Größe der Zwillingsaktualisierungen von Geräten)|Byte|Average|Die Gesamtgröße aller erfolgreichen Zwillingsaktualisierungen, die vom Gerät initiiert wurden.|Keine|
|c2d.methods.success|Successful direct method invocations (Erfolgreiche direkte Methodenaufrufvorgänge)|Anzahl|Gesamt|Gibt an, wie viele direkte Methodenaufrufe erfolgreich durchgeführt wurden.|Keine|
|c2d.methods.failure|Failed direct method invocations (Nicht erfolgreiche direkte Methodenaufrufe)|Anzahl|Gesamt|Gibt an, wie viele direkte Methodenaufrufe nicht erfolgreich waren.|Keine|
|c2d.methods.requestSize|Request size of direct method invocations (Anforderungsgröße von direkten Methodenaufrufen)|Byte|Average|Die Anzahl aller erfolgreichen direkten Methodenaufrufe.|Keine|
|c2d.methods.responseSize|Response size of direct method invocations (Antwortgröße von direkten Methodenaufrufen)|Byte|Average|Die Anzahl aller erfolgreichen direkten Methodenantworten.|Keine|
|c2d.twin.read.success|Successful twin reads from back end (Erfolgreiche Zwillingslesevorgänge vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.read.failure|Failed twin reads from back end (Nicht erfolgreiche Zwillingslesevorgänge vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.read.size|Response size of twin reads from back end (Antwortgröße von Zwillingslesevorgängen vom Back-End)|Byte|Average|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.update.success|Successful twin updates from back end (Erfolgreiche Zwillingsaktualisierungen vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.update.failure|Failed twin updates from back end (Nicht erfolgreiche Zwillingsaktualisierungen vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.update.size|Size of twin updates from back end (Größe der Zwillingsaktualisierungen vom Back-End)|Byte|Average|Die Anzahl aller erfolgreichen Zwillingsaktualisierungen, die vom Back-End initiiert wurden.|Keine|
|twinQueries.success|Successful twin queries (Erfolgreiche Zwillingsabfragen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Zwillingsabfragen durchgeführt wurden.|Keine|
|twinQueries.failure|Failed twin queries (Nicht erfolgreiche Zwillingsabfragen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Zwillingsabfragen durchgeführt wurden.|Keine|
|twinQueries.resultSize|Twin queries result size (Ergebnisgröße von Zwillingsabfragen)|Byte|Average|Die Gesamtgröße der Ergebnisse aller erfolgreichen Zwillingsabfragen.|Keine|
|jobs.createTwinUpdateJob.<br>success|Successful creations of twin update jobs (Erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|Keine|
|jobs.createTwinUpdateJob.<br>failure|Failed creations of twin update jobs (Nicht erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von allen nicht erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|Keine|
|jobs.createDirectMethodJob.<br>success|Successful creations of method invocation jobs (Erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|Keine|
|jobs.createDirectMethodJob.<br>failure|Failed creations of method invocation jobs (Nicht erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|Keine|
|jobs.listJobs.success|Successful calls to list jobs (Erfolgreiche Aufrufe von Auflistungsaufträgen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|Keine|
|jobs.listJobs.failure|Failed calls to list jobs (Nicht erfolgreiche Aufrufe von Auflistungsaufträgen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|Keine|
|jobs.cancelJob.success|Successful job cancellations (Erfolgreiche Auftragsabbrüche)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|Keine|
|jobs.cancelJob.failure|Failed job cancellations (Nicht erfolgreiche Auftragsabbrüche)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|Keine|
|jobs.queryJobs.success|Successful job queries (Erfolgreiche Auftragsabfragen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|Keine|
|jobs.queryJobs.failure|Failed job queries (Nicht erfolgreiche Auftragsabfragen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|Keine|
|jobs.completed|Abgeschlossene Aufträge|Anzahl|Gesamt|Gibt die Anzahl von abgeschlossenen Aufträgen an.|Keine|
|jobs.failed|Fehlerhafte Aufträge|Anzahl|Gesamt|Gibt die Anzahl aller fehlerhaften Aufträge an.|Keine|
|d2c.telemetry.ingress.<br>sendThrottle|Anzahl von Drosselungsfehlern|Anzahl|Gesamt|Anzahl von Drosselungsfehlern aufgrund von Drosselungen des Gerätedurchsatzes|Keine|
|dailyMessageQuotaUsed|Gesamtzahl verwendeter Nachrichten|Anzahl|Average|Gesamtzahl der heute verwendeten Nachrichten. Dies ist ein kumulativer Wert, der jeden Tag um 00:00 Uhr UTC auf 0 zurückgesetzt wird.|Keine|
|deviceDataUsage|Gerätedatennutzung gesamt|Byte|Gesamt|Zu und von allen mit IotHub verbundenen Geräten übertragene Bytes|Keine|
|deviceDataUsageV2|Gerätedatennutzung gesamt (Vorschau)|Byte|Gesamt|Zu und von allen mit IotHub verbundenen Geräten übertragene Bytes|Keine|
|totalDeviceCount|Geräte gesamt (Vorschau)|Anzahl|Average|Die Anzahl von Geräten, die beim IoT Hub registriert sind|Keine|
|connectedDeviceCount|Verbundene Geräte (Vorschau)|Anzahl|Average|Die Anzahl von Geräten, die mit dem IoT Hub verbunden sind|Keine|
|Konfigurationen|Konfigurationsmetriken|Anzahl|Gesamt|Die Gesamtanzahl aller CRUD-Vorgänge, die für die Gerätekonfiguration und die IoT Edge-Bereitstellung auf einer Gruppe von Zielgeräten ausgeführt wurden. Diese Metrik enthält auch die Anzahl von Vorgängen, die den Geräte- bzw. Modulzwilling aufgrund dieser Konfigurationen ändern.|Keine|

### <a name="dimensions"></a>Dimensionen
Mit Dimensionen lassen sich genauere Details zu den Metriken ermitteln. Einige Routingmetriken bieten Informationen zu den einzelnen Endpunkten. In der folgenden Tabelle sind die möglichen Werte für diese Dimensionen aufgeführt.

|Dimension|Werte|
|---|---|
|ResourceID|Ihre IoT Hub-Ressource|
|Ergebnis|success<br>failure|
|RoutingSource|Gerätemeldungen<br>Änderungsereignisse für Zwillinge<br>Ereignisse zum Gerätelebenszyklus|
|EndpointType|eventHubs<br>serviceBusQueues<br>cosmosDB<br>serviceBusTopics<br>builtin<br>blobStorage|
|FailureReasonCategory|ungültig<br>dropped<br>orphaned<br>NULL|
|EndpointName|Endpunktname|

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun einen Überblick über IoT Hub-Metriken verschafft haben, folgen Sie dem folgenden Link, um mehr über die Verwaltung von IoT Hub zu erfahren:

* [Einrichten von Diagnoseprotokollen](iot-hub-monitor-resource-health.md)

* [Informationen zur Problembehandlung beim Nachrichtenrouting](troubleshoot-message-routing.md)

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Entwicklungsleitfaden für IoT Hub](iot-hub-devguide.md)

* [Bereitstellen von KI auf Edge-Geräten mit Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
