---
title: Protokollieren von Ereignissen in Azure Event Hubs mit Azure API Management | Microsoft Docs
description: Erfahren Sie, wie Sie mit Azure API Management Ereignisse in Azure Event Hubs protokollieren.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/29/2018
ms.author: apimpm
ms.openlocfilehash: 2f07f6a27e78ee4df8c64a09918758d02c28c6d4
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76898793"
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Protokollieren von Ereignissen in Azure Event Hubs mit Azure API Management
Azure Event Hubs ist ein hochgradig skalierbarer Dateneingangsdienst, der Millionen von Ereignissen pro Sekunde erfassen kann. Auf diese Weise können Sie riesige Datenmengen verarbeiten und analysieren, die von vernetzten Geräten und Anwendungen erzeugt werden. Event Hubs fungiert als „Eingangstür“ für eine Ereignispipeline. Nach der Erfassung in Event Hubs können Sie Daten mit einem beliebigen Echtzeit-Analyseanbieter oder mit Batchverarbeitungs-/Speicheradaptern umwandeln und speichern. Event Hubs entkoppelt die Erzeugung eines Datenstroms von Ereignissen von der Nutzung dieser Ereignisse, sodass  Ereignisconsumer nach einem eigenen Zeitplan auf Ereignisse zugreifen können.

Dies ist ein Begleitartikel zum Video [Integrate Azure API Management with Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) (Integrieren von Azure API Management in Event Hubs; in englischer Sprache). Hier wird beschrieben, wie API Management-Ereignisse mithilfe von Azure Event Hubs protokolliert werden.

## <a name="create-an-azure-event-hub"></a>Erstellen eines Azure Event Hubs

Ausführliche Schritte zum Erstellen eines Event Hubs und zum Abrufen von Verbindungszeichenfolgen, die Sie zum Senden und Empfangen von Ereignis aus dem bzw. im Event Hub benötigen, finden Sie unter [Erstellen eines Event Hubs-Namespace und eines Event Hubs mithilfe des Azure-Portals](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

## <a name="create-an-api-management-logger"></a>Erstellen eines API Management-Loggers
Der Event Hub ist nun vorhanden. Der nächste Schritt besteht darin, einen [Logger](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger) im API Management-Dienst zu konfigurieren, sodass Ereignisse im Event Hub protokolliert werden können.

API Management-Logger werden mit der [API Management-REST-API](https://aka.ms/apimapi)konfiguriert. Ausführliche Anforderungsbeispiele finden Sie unter [Erstellen von Protokollierungen](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger/createorupdate).

## <a name="configure-log-to-eventhubs-policies"></a>Konfigurieren von log-to-eventhub-Richtlinien

Nachdem Sie den Logger in API Management konfiguriert haben, können Sie die Richtlinien zum Protokollieren im Event Hub („log-to-eventhub-Richtlinien“) für die gewünschten Ereignisse konfigurieren. Die log-to-eventhub-Richtlinie kann im Abschnitt mit Richtlinien für eingehenden Datenverkehr oder im Abschnitt mit Richtlinien für ausgehenden Datenverkehr verwendet werden.

1. Navigieren Sie zu Ihrer APIM-Instanz.
2. Wählen Sie die Registerkarte „API“ aus.
3. Wählen Sie die API aus, der Sie die Richtlinie hinzufügen möchten. In diesem Beispiel fügen wir die Richtlinie zur **Echo-API** im Produkt **Unlimited** hinzu.
4. Wählen Sie **Alle Vorgänge** aus.
5. Klicken Sie im oberen Seitenbereich auf die Registerkarte „Entwurf“.
6. Klicken Sie im Fenster „Eingehende Verarbeitung“ bzw. „Ausgehende Verarbeitung“ auf das Dreieck (neben dem Stift).
7. Wählen Sie den Code-Editor aus. Weitere Informationen finden Sie unter [Einrichten oder Bearbeiten von Richtlinien](set-edit-policies.md).
8. Positionieren Sie den Cursor im Richtlinienabschnitt `inbound` oder `outbound`.
9. Wählen Sie im Fenster rechts die Optionen **Erweiterte Richtlinien** > **Bei EventHub anmelden** aus. Damit wird die Richtlinienanweisungsvorlage `log-to-eventhub` eingefügt.

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```
Ersetzen Sie `logger-id` durch den Wert, den Sie im vorherigen Schritt in der URL für `{new logger name}` verwendet haben, um die Protokollierung zu erstellen.

Sie können jeden Ausdruck verwenden, der eine Zeichenfolge als Wert für das Element `log-to-eventhub` zurückgibt. In diesem Beispiel wird eine Zeichenfolge protokolliert, die das Datum, die Uhrzeit, den Dienstnamen, die Anforderungs-ID, die IP-Adresse der Anforderung und den Vorgangsnamen enthält.

Klicken Sie auf **Speichern** , um die aktualisierte Richtlinienkonfiguration zu speichern. Die Richtlinie ist sofort nach dem Speichern aktiv, und im vorgesehenen Event Hub werden Ereignisse protokolliert.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu Azure Event Hubs
  * [Erste Schritte mit Azure Event Hubs](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Empfangen von Nachrichten mit EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Programmierleitfaden für Event Hubs](../event-hubs/event-hubs-programming-guide.md)
* Erfahren Sie mehr über die Integration der API-Verwaltung und Event Hubs
  * [Verweis zu Protokollierungstool](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger)
  * [log-to-eventhub policy reference](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [Überwachen von APIs mit Azure API Management, Event Hubs und Moesif](api-management-log-to-eventhub-sample.md)  
* Erfahren Sie mehr über die [Integration in Azure Application Insights](api-management-howto-app-insights.md).

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
