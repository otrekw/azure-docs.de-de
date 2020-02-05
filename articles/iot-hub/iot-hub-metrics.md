---
title: Überwachen von Azure IoT Hub mithilfe von Metriken | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von Azure IoT Hub-Metriken die allgemeine Integrität Ihrer IoT Hub-Instanzen bewerten und überwachen.
author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: jlian
ms.openlocfilehash: ec8a00460b4a750339f929eb6879ac6eb63cac8e
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705733"
---
# <a name="understand-iot-hub-metrics"></a>Grundlegendes zu IoT Hub-Metriken

IoT Hub-Metriken liefern bessere Daten zum Zustand der Azure IoT-Ressourcen in Ihrem Azure-Abonnement. Auf der Grundlage von IoT Hub-Metriken können Sie die allgemeine Integrität des IoT Hub-Diensts und der mit ihm verbundenen Geräte beurteilen. Benutzerorientierte Statistiken spielen eine wichtige Rolle. Sie bieten die Basis, um die Vorgänge in Ihrem IoT Hub zu erkennen und Probleme zu beheben, ohne den Azure-Support zu kontaktieren.

Metriken sind standardmäßig aktiviert. IoT Hub-Metriken können über das Azure-Portal angezeigt werden.

> [!NOTE]
> Sie können IoT Hub-Metriken verwenden, um Informationen über IoT Plug & Play-Geräte anzuzeigen, die mit Ihrem IoT Hub verbunden sind. IoT Plug & Play-Geräte sind Teil der [Public Preview von IoT Plug & Play](../iot-pnp/overview-iot-plug-and-play.md).

## <a name="how-to-view-iot-hub-metrics"></a>Anzeigen von IoT Hub-Metriken

1. Erstellen Sie einen IoT Hub. Anweisungen zum Erstellen eines IoT-Hubs finden Sie im Leitfaden [Senden von Telemetriedaten von einem Gerät an IoT Hub](quickstart-send-telemetry-dotnet.md).

2. Öffnen Sie das Blatt Ihres IoT Hubs. Klicken Sie dort auf **Metriken**.
   
    ![Screenshot, in dem dargestellt ist, wo sich die Option „Metriken“ auf der Seite für IoT Hub-Ressourcen befindet](./media/iot-hub-metrics/enable-metrics-1.png)

3. Auf dem Blatt „Metriken“ können Sie die Metriken für Ihren IoT-Hub anzeigen und benutzerdefinierte Metrikansichten erstellen. 
   
    ![Screenshot der Seite „Metriken“ für IoT Hub](./media/iot-hub-metrics/enable-metrics-2.png)
    
4. Sie können Ihre Metrikdaten an einen Event Hubs-Endpunkt oder an ein Azure Storage-Konto senden, indem Sie auf **Diagnoseeinstellungen** und dann auf **Diagnoseeinstellung hinzufügen** klicken.

   ![Screenshot, in dem dargestellt ist, wo sich die Schaltfläche „Diagnoseeinstellungen“ befindet](./media/iot-hub-metrics/enable-metrics-3.png)

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IoT Hub-Metriken und ihre Verwendung

IoT Hub bietet mehrere Metriken, um Ihnen einen Überblick über die Integrität Ihres Hubs und die Gesamtzahl der verbundenen Geräte zu verschaffen. Sie können Informationen aus mehreren Metriken kombinieren, um sich ein umfassenderes Bild des Zustands des IoT Hubs zu machen. In der folgenden Tabelle werden die Metriken beschrieben, die für IoT Hubs nachverfolgt werden. Außerdem erfahren Sie, in welchem Bezug die einzelnen Metriken zum allgemeinen Status des IoT Hubs stehen.

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|Dimensionen|
|---|---|---|---|---|---|
|d2c.telemetry.Ingress.allProtocol|Telemetry message send attempts (Sendeversuche für Telemetrienachrichten)|Anzahl|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die an Ihren IoT Hub gesendet werden sollten|Keine|
|d2c.telemetry.ingress.success|Telemetry messages sent (Gesendete Telemetrienachrichten)|Anzahl|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die erfolgreich an Ihren IoT Hub gesendet wurden|Keine|
|c2d.commands.egress.complete.success|Abgeschlossene C2D-Nachrichtenübermittlungen|Anzahl|Gesamt|Anzahl von Cloud-zu-Gerät-Nachrichtenübermittlungen, die vom Gerät erfolgreich abgeschlossen wurden|Keine|
|c2d.commands.egress.abandon.success|Abgebrochene C2D-Nachrichten|Anzahl|Gesamt|Anzahl von Cloud-zu-Gerät-Nachrichten, die vom Gerät abgebrochen wurden|Keine|
|c2d.commands.egress.reject.success|Abgelehnte C2D-Nachrichten|Anzahl|Gesamt|Anzahl von Cloud-zu-Gerät-Nachrichten, die vom Gerät abgelehnt wurden|Keine|
|C2DMessagesExpired|Abgelaufene C2D-Nachrichten (Vorschau)|Anzahl|Gesamt|Anzahl von abgelaufenen Cloud-zu-Gerät-Nachrichten|Keine|
|devices.totalDevices|Geräte gesamt (veraltet)|Anzahl|Gesamt|Die Anzahl von Geräten, die beim IoT Hub registriert sind|Keine|
|devices.connectedDevices.allProtocol|Verbundene Geräte (veraltet) |Anzahl|Gesamt|Die Anzahl von Geräten, die mit dem IoT Hub verbunden sind|Keine|
|d2c.telemetry.egress.success|Routing: Übermittelte Telemetrienachrichten|Anzahl|Gesamt|Die Anzahl der erfolgreichen Nachrichtenübermittlungen an alle Endpunkte über das IoT Hub-Routing Wenn eine Nachricht an mehrere Endpunkte weitergeleitet wird, erhöht sich dieser Wert für jede erfolgreiche Übermittlung um eins Wenn eine Nachricht mehrmals an denselben Endpunkt übermittelt wird, erhöht sich dieser Wert für jede erfolgreiche Übermittlung um eins|Keine|
|d2c.telemetry.egress.dropped|Routing: Verworfene Telemetrienachrichten |Anzahl|Gesamt|Die Anzahl der Nachrichten, die vom IoT Hub-Routing aufgrund von inaktiven Endpunkten gelöscht wurden. Dieser Wert zählt nicht die Nachrichten, die an die Fallbackroute übermittelt werden, da gelöschte Nachrichten dort nicht übermittelt werden.|Keine|
|d2c.telemetry.egress.orphaned|Routing: Verwaiste Telemetrienachrichten |Anzahl|Gesamt|Die Häufigkeit, mit der Nachrichten durch das IoT Hub-Routing verwaist wurden, da sie mit keinen Routingregeln (einschließlich der Fallbackregel) übereinstimmten. |Keine|
|d2c.telemetry.egress.invalid|Routing: Nicht kompatible Telemetrienachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing Nachrichten aufgrund einer Inkompatibilität mit dem Endpunkt nicht übermitteln konnte. Dieser Wert umfasst keine Wiederholungen.|Keine|
|d2c.telemetry.egress.fallback|Routing: An den Fallback übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing Nachrichten an den mit der Fallbackroute verbundenen Endpunkt übermittelt hat.|Keine|
|d2c.endpoints.egress.eventHubs|Routing: An Event Hub übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Event Hub-Endpunkte übermittelt hat.|Keine|
|d2c.endpoints.latency.eventHubs|Routing: Nachrichtenwartezeit für Event Hub|Millisekunden|Average|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Nachricht bei einem Event Hub-Endpunkt.|Keine|
|d2c.endpoints.egress.serviceBusQueues|Routing: An Service Bus-Warteschlange übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Service Bus-Warteschlangenendpunkte übermittelt hat.|Keine|
|d2c.endpoints.latency.serviceBusQueues|Routing: Nachrichtenwartezeit für Service Bus-Warteschlange|Millisekunden|Average|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Nachricht bei einem Service Bus-Warteschlangenendpunkt.|Keine|
|d2c.endpoints.egress.serviceBusTopics|Routing: An Service Bus-Thema übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Service Bus-Themaendpunkte übermittelt hat.|Keine|
|d2c.endpoints.latency.serviceBusTopics|Routing: Nachrichtenwartezeit für Service Bus-Thema|Millisekunden|Average|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Nachricht bei einem Service Bus-Themaendpunkt.|Keine|
|d2c.endpoints.egress.builtIn.events|Routing: An Nachrichten/Ereignisse übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an den integrierter Endpunkt (Nachrichten/Ereignisse) übermittelt hat.|Keine|
|d2c.endpoints.latency.builtIn.events|Routing: Nachrichtenwartezeit für Nachrichten/Ereignisse|Millisekunden|Average|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Telemetrienachricht beim integrierten Endpunkt (Nachrichten/Ereignisse).|Keine|
|d2c.endpoints.egress.storage|Routing: An den Speicher übermittelte Nachrichten|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing erfolgreich Nachrichten an Speicherendpunkte übermittelt hat.|Keine|
|d2c.endpoints.latency.storage|Routing: Nachrichtenwartezeit für Speicher|Millisekunden|Average|Durchschnittliche Wartezeit (Millisekunden) zwischen dem Eingang der Nachricht beim IoT Hub und dem Eingang der Nachricht bei einem Speicherendpunkt.|Keine|
|d2c.endpoints.egress.storage.bytes|Routing: An den Speicher übermittelte Daten|Byte|Gesamt|Die Datenmenge (Bytes), die das IoT Hub-Routing an die Speicherendpunkte übermittelt.|Keine|
|d2c.endpoints.egress.storage.blobs|Routing: An den Speicher übermittelte Blobs|Anzahl|Gesamt|Die Häufigkeit, mit der das IoT Hub-Routing Blobs an Speicherendpunkte übermittelt hat.|Keine|
|EventGridDeliveries|Event Grid-Übermittlungen (Vorschau)|Anzahl|Gesamt|Die Anzahl von IoT Hub-Ereignissen, die in Event Grid veröffentlicht werden. Verwenden Sie die Dimension „Result“ für die Anzahl der erfolgreichen und fehlerhaften Anforderungen. Die „EventType“-Dimension zeigt den Typ des Ereignisses an(https://aka.ms/ioteventgrid).|ResourceId,<br/>Result,<br/>EventType|
|EventGridLatency|Event Grid-Wartezeit (Vorschau)|Millisekunden|Average|Die durchschnittliche Wartezeit (in Millisekunden) für den Zeitraum zwischen der Generierung des Iot Hub-Ereignisses und der Veröffentlichung des Ereignisses in Event Grid. Diese Zahl ist ein Durchschnittswert für alle Ereignistypen. Verwenden Sie die Dimension „EventType“, um die Wartezeit für einen bestimmten Ereignistyp anzuzeigen.|ResourceId,<br/>EventType|
|d2c.twin.read.success|Successful twin reads from devices (Erfolgreiche Zwillingslesevorgänge von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.read.failure|Failed twin reads from devices (Nicht erfolgreiche Zwillingslesevorgänge von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.read.size|Response size of twin reads from devices (Antwortgröße von Zwillingslesevorgängen von Geräten)|Byte|Average|Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Zwillingslesevorgänge, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.update.success|Successful twin updates from devices (Erfolgreiche Zwillingsaktualisierungen von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.update.failure|Failed twin updates from devices (Nicht erfolgreiche Zwillingsaktualisierungen von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|Keine|
|d2c.twin.update.size|Size of twin updates from devices (Größe der Zwillingsaktualisierungen von Geräten)|Byte|Average|Durchschnitts-, Minimal- und Maximalgröße für alle erfolgreichen Zwillingsaktualisierungen, die vom Gerät initiiert wurden.|Keine|
|c2d.methods.success|Successful direct method invocations (Erfolgreiche direkte Methodenaufrufvorgänge)|Anzahl|Gesamt|Gibt an, wie viele direkte Methodenaufrufe erfolgreich durchgeführt wurden.|Keine|
|c2d.methods.failure|Failed direct method invocations (Nicht erfolgreiche direkte Methodenaufrufe)|Anzahl|Gesamt|Gibt an, wie viele direkte Methodenaufrufe nicht erfolgreich waren.|Keine|
|c2d.methods.requestSize|Request size of direct method invocations (Anforderungsgröße von direkten Methodenaufrufen)|Byte|Average|Gibt den Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen direkten Methodenaufrufe an.|Keine|
|c2d.methods.responseSize|Response size of direct method invocations (Antwortgröße von direkten Methodenaufrufen)|Byte|Average|Gibt den Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Antworten für die direkte Methode an.|Keine|
|c2d.twin.read.success|Successful twin reads from back end (Erfolgreiche Zwillingslesevorgänge vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.read.failure|Failed twin reads from back end (Nicht erfolgreiche Zwillingslesevorgänge vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.read.size|Response size of twin reads from back end (Antwortgröße von Zwillingslesevorgängen vom Back-End)|Byte|Average|Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Zwillingslesevorgänge, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.update.success|Successful twin updates from back end (Erfolgreiche Zwillingsaktualisierungen vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.update.failure|Failed twin updates from back end (Nicht erfolgreiche Zwillingsaktualisierungen vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|Keine|
|c2d.twin.update.size|Size of twin updates from back end (Größe der Zwillingsaktualisierungen vom Back-End)|Byte|Average|Durchschnitts-, Minimal- und Maximalgröße für alle erfolgreichen Zwillingsaktualisierungen, die vom Back-End initiiert wurden.|Keine|
|twinQueries.success|Successful twin queries (Erfolgreiche Zwillingsabfragen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Zwillingsabfragen durchgeführt wurden.|Keine|
|twinQueries.failure|Failed twin queries (Nicht erfolgreiche Zwillingsabfragen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Zwillingsabfragen durchgeführt wurden.|Keine|
|twinQueries.resultSize|Twin queries result size (Ergebnisgröße von Zwillingsabfragen)|Byte|Average|Durchschnitts-, Minimal- und Maximalwert der Ergebnisgröße aller erfolgreichen Zwillingsabfragen.|Keine|
|jobs.createTwinUpdateJob.success|Successful creations of twin update jobs (Erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|Keine|
|jobs.createTwinUpdateJob.failure|Failed creations of twin update jobs (Nicht erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von allen nicht erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|Keine|
|jobs.createDirectMethodJob.success|Successful creations of method invocation jobs (Erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|Keine|
|jobs.createDirectMethodJob.failure|Failed creations of method invocation jobs (Nicht erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|Keine|
|jobs.listJobs.success|Successful calls to list jobs (Erfolgreiche Aufrufe von Auflistungsaufträgen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|Keine|
|jobs.listJobs.failure|Failed calls to list jobs (Nicht erfolgreiche Aufrufe von Auflistungsaufträgen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|Keine|
|jobs.cancelJob.success|Successful job cancellations (Erfolgreiche Auftragsabbrüche)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|Keine|
|jobs.cancelJob.failure|Failed job cancellations (Nicht erfolgreiche Auftragsabbrüche)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|Keine|
|jobs.queryJobs.success|Successful job queries (Erfolgreiche Auftragsabfragen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|Keine|
|jobs.queryJobs.failure|Failed job queries (Nicht erfolgreiche Auftragsabfragen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|Keine|
|jobs.completed|Abgeschlossene Aufträge|Anzahl|Gesamt|Gibt die Anzahl von abgeschlossenen Aufträgen an.|Keine|
|jobs.failed|Fehlerhafte Aufträge|Anzahl|Gesamt|Gibt die Anzahl aller fehlerhaften Aufträge an.|Keine|
|d2c.telemetry.ingress.sendThrottle|Anzahl von Drosselungsfehlern|Anzahl|Gesamt|Anzahl von Drosselungsfehlern aufgrund von Drosselungen des Gerätedurchsatzes|Keine|
|dailyMessageQuotaUsed|Gesamtzahl verwendeter Nachrichten|Anzahl|Average|Gesamtzahl der heute verwendeten Nachrichten. Dies ist ein kumulativer Wert, der jeden Tag um 00:00 Uhr UTC auf 0 zurückgesetzt wird.|Keine|
|deviceDataUsage|Gerätedatennutzung gesamt|Byte|Gesamt|Zu und von allen mit IotHub verbundenen Geräten übertragene Bytes|Keine|
|deviceDataUsageV2|Gerätedatennutzung gesamt (Vorschau)|Byte|Gesamt|Zu und von allen mit IotHub verbundenen Geräten übertragene Bytes|Keine|
|totalDeviceCount|Geräte gesamt (Vorschau)|Anzahl|Average|Die Anzahl von Geräten, die beim IoT Hub registriert sind|Keine|
|connectedDeviceCount|Verbundene Geräte (Vorschau)|Anzahl|Average|Die Anzahl von Geräten, die mit dem IoT Hub verbunden sind|Keine|
|Konfigurationen|Konfigurationsmetriken|Anzahl|Gesamt|Metriken für Konfigurationsvorgänge|Keine|

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun einen Überblick über IoT Hub-Metriken verschafft haben, folgen Sie dem folgenden Link, um mehr über die Verwaltung von IoT Hub zu erfahren:

* [Einrichten von Diagnoseprotokollen](iot-hub-monitor-resource-health.md)

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Entwicklungsleitfaden für IoT Hub](iot-hub-devguide.md)

* [Bereitstellen von KI auf Edge-Geräten mit Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
