---
title: Anzeigen von Metriken mit Azure Monitor
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Azure Digital Twins-Metriken in Azure Monitor angezeigt werden.
author: baanders
ms.author: baanders
ms.date: 7/24/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: c6db571d64b0fd276519f15a3984848e80c4e18a
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87387675"
---
# <a name="view-and-understand-azure-digital-twins-metrics"></a>Anzeigen und Verstehen von Azure Digital Twins-Metriken

Diese Metriken liefern Informationen zum Zustand der Azure Digital Twins-Ressourcen in Ihrem Azure-Abonnement. Azure Digital Twins-Metriken helfen Ihnen, die allgemeine Integrität des Azure Digital Twins-Diensts und der damit verbundenen Ressourcen zu bewerten. Anhand dieser benutzerorientierten Statistiken können Sie Vorgänge in Azure Digital Twins erkennen und die Ursache von Problemen analysieren, ohne sich an den Azure-Support wenden zu müssen.

Metriken sind standardmäßig aktiviert. Azure Digital Twins-Metriken können über das [Azure-Portal](https://portal.azure.com) angezeigt werden.

## <a name="how-to-view-azure-digital-twins-metrics"></a>Anzeigen von Azure Digital Twins-Metriken

1. Erstellen Sie eine Azure Digital Twins-Instanz. Anweisungen zum Einrichten einer Azure Digital Twins-Instanz finden Sie unter [*Vorgehensweise: Einrichten einer Instanz und der Authentifizierung*](how-to-set-up-instance-scripted.md).

2. Suchen Sie im [Azure-Portal](https:/portal.azure.com) nach Ihrer Azure Digital Twins-Instanz (Sie können die Seite für die Instanz öffnen, indem Sie ihren Namen in die Suchleiste des Portals eingeben). 

    Wählen Sie im Menü der Instanz **Metriken** aus.
   
    :::image type="content" source="media/how-to-view-metrics/azure-digital-twins-metrics.png" alt-text="Screenshot der Seite „Metriken“ für Azure Digital Twins":::

    Auf dieser Seite werden die Metriken für Ihre Azure Digital Twins-Instanz angezeigt. Sie können auch benutzerdefinierte Ansichten Ihrer Metriken erstellen, indem Sie die gewünschten Metriken in der Liste auswählen.
    
3. Sie können Ihre Metrikdaten an einen Event Hubs-Endpunkt oder ein Azure Storage-Konto senden, indem Sie im Menü auf **Diagnoseeinstellungen** und dann auf **Diagnoseeinstellung hinzufügen** klicken.

    :::image type="content" source="media/how-to-view-metrics/diagnostic-settings.png" alt-text="Screenshot: Seite „Diagnoseeinstellungen“und Schaltfläche zum Hinzufügen":::

    Weitere Informationen zu diesem Vorgang finden Sie unter [*Problembehandlung: Einrichten der Diagnose*](troubleshoot-diagnostics.md).

## <a name="azure-digital-twins-metrics-and-how-to-use-them"></a>Azure Digital Twins-Metriken und deren Verwendung

Azure Digital Twins bietet mehrere Metriken, um Ihnen einen Überblick über die Integrität Ihrer Instanz und der zugehörigen Ressourcen zu ermöglichen. Sie können auch Informationen aus mehreren Metriken kombinieren, um sich ein umfassenderes Bild des Zustands Ihrer Instanz zu machen. 

In den folgenden Tabellen werden die Metriken beschrieben, die von jeder Azure Digital Twins-Instanz nachverfolgt werden, und es wird erläutert, wie sich jede Metrik auf den Gesamtstatus Ihrer Instanz bezieht.

#### <a name="api-request-metrics"></a>API-Anforderungsmetriken

Metriken im Zusammenhang mit API-Anforderungen:

| Metrik | Anzeigename der Metrik | Einheit | Aggregationstyp| BESCHREIBUNG | Dimensionen |
| --- | --- | --- | --- | --- | --- |
| ApiRequests | API-Anforderungen (Vorschau) | Anzahl | Gesamt | Die Anzahl der API-Anforderungen, die für Lese-, Schreib-, Lösch- und Abfragevorgänge für Digital Twins durchgeführt wurden. |  Authentifizierung </br>Vorgang </br>Protocol </br>Statuscode </br>Statuscodeklasse </br>Statustext |
| ApiRequestsLatency | API-Anforderungslatenz (Vorschau) | Millisekunden | Average | Die Antwortzeit für API-Anforderungen. Dies bezieht sich auf den Zeitraum zwischen dem Eingang der Anforderung bei Azure Digital Twins und dem Zeitpunkt, zu dem der Dienst ein Erfolgs- bzw. Fehlerergebnis für Lese-, Schreib-, Lösch- und Abfragevorgänge von Digital Twins sendet. | Authentifizierung </br>Vorgang </br>Protocol |
| ApiRequestsFailureRate | API-Anforderungsfehlerrate (Vorschau) | Percent | Average | Der Prozentsatz der API-Anforderungen, die der Dienst für Ihre Instanz erhält und die einen internen Fehler (500) als Antwortcode für Lese-, Schreib-, Lösch- und Abfragevorgänge von Digital Twins angeben. | Authentifizierung </br>Vorgang </br>Protocol </br>Statuscode </br>Statuscodeklasse </br>Statustext

#### <a name="routing-metrics"></a>Routingmetriken

Metriken im Zusammenhang mit Routing:

| Metrik | Anzeigename der Metrik | Einheit | Aggregationstyp| BESCHREIBUNG | Dimensionen |
| --- | --- | --- | --- | --- | --- |
| Routing | Routing (Vorschau) | Anzahl | Gesamt | Die Anzahl der Nachrichten, die an einen Azure-Endpunktdienst (z. B. Event Hub, Service Bus oder Event Grid) weitergeleitet werden. | Vorgang </br>Ergebnis |
| RoutingLatency | Routinglatenz (Vorschau) | Millisekunden | Average | Die Zeit, die zwischen der Weiterleitung eines Ereignisses von Azure Digital Twins bis zu dem Zeitpunkt verstrichen ist, an dem es an den Azure-Endpunktdienst wie Event Hub, Service Bus oder Event Grid gesendet wird. | Vorgang </br>Ergebnis |
| RoutingFailureRate | Routingfehlerrate (Vorschau) | Percent | Average | Der Prozentsatz der Ereignisse, die zu einem Fehler führen, wenn sie von Azure Digital Twins zu einem Azure-Endpunktdienst wie Event Hub, Service Bus oder Event Grid weitergeleitet werden. | Vorgang </br>Ergebnis |

#### <a name="billing-metrics"></a>Abrechnungsmetriken

Metriken im Zusammenhang mit Abrechnung:

>[!NOTE]
> Während der Vorschauphase **fallen für die Abrechnung keine Kosten an**. Diese Metriken werden zwar noch in der auswählbaren Liste angezeigt, sie gelten jedoch während der Vorschauphase nicht und bleiben bei Null, bis die Vorschau des Diensts beendet wurde.

| Metrik | Anzeigename der Metrik | Einheit | Aggregationstyp| BESCHREIBUNG | Dimensionen |
| --- | --- | --- | --- | --- | --- |
| BillingApiOperations | API-Abrechnungsvorgänge (Vorschau) | Anzahl | Gesamt | Abrechnungsmetrik für die Anzahl aller API-Anforderungen, die für den Azure Digital Twins-Dienst durchgeführt wurden. | `MeterId` |
| BillingQueryUnits | Abrechnungsabfrageeinheiten (Vorschau) | Anzahl | Gesamt | Die Anzahl der Abfrageeinheiten (ein intern berechnetes Measure der Dienstressourcennutzung), die zum Ausführen von Abfragen genutzt werden. Es ist auch eine Hilfs-API zum Messen von Abfrageeinheiten verfügbar: [QueryChargeHelper-Klasse](https://docs.microsoft.com/dotnet/api/azure.digitaltwins.core.querychargehelper?view=azure-dotnet-preview) | `MeterId` |
| BillingMessagesProcessed | Verarbeitete Abrechnungsnachrichten (Vorschau) | Anzahl | Gesamt | Abrechnungsmetrik für die Anzahl von Nachrichten, die von Azure Digital Twins Zwillingen an externe Endpunkte gesendet werden. | `MeterId` |

## <a name="dimensions"></a>Dimensionen

Mit Dimensionen lassen sich genauere Details zu den Metriken ermitteln. Einige Routingmetriken bieten Informationen zu den einzelnen Endpunkten. In der folgenden Tabelle sind die möglichen Werte für diese Dimensionen aufgeführt.

| Dimension | Werte |
| --- | --- |
| Authentifizierung | OAuth |
| Vorgang (für API-Anforderungen) | Microsoft.DigitalTwins/digitaltwins/delete</br>Microsoft.DigitalTwins/digitaltwins/write</br>Microsoft.DigitalTwins/digitaltwins/read </br>Microsoft.DigitalTwins/eventroutes/read </br>Microsoft.DigitalTwins/eventroutes/write </br>Microsoft.DigitalTwins/eventroutes/delete </br>Microsoft.DigitalTwins/models/read </br>Microsoft.DigitalTwins/models/write </br>Microsoft.DigitalTwins/models/delete </br>Microsoft.DigitalTwins/query/action |
Vorgang (für Routing) | Event Grid </br>Event Hub </br>Service Bus |
| Protocol | HTTPS |
| Ergebnis | Erfolg </br>Fehler |
| Statuscode | 200, 404, 500 usw. |
| Statuscodeklasse | 2xx, 4xx, 5xx usw. |
| Statustext | Interner Serverfehler, Nicht gefunden, usw. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwalten von aufgezeichneten Metriken für Azure Digital Twins finden Sie unter [*Problembehandlung: Einrichten der Diagnose*](troubleshoot-diagnostics.md).

Nachdem Sie nun einen Überblick über die Metriken von Azure Digital Twins erhalten haben, können Sie auch diesen Links folgen, um mehr über die Verwaltung von Azure Digital Twins zu erfahren:

* [*Verwenden der Azure Digital Twins-APIs und SDKs*](how-to-use-apis-sdks.md)
* [*Verwenden Verwalten von Azure Digital Twins-Modellen*](how-to-manage-model.md)
* [*Verwenden Verwalten digitaler Zwillinge*](how-to-manage-twin.md)
* [*Verwenden Verwalten des Zwillingsgraphen mit Beziehungen*](how-to-manage-graph.md)
* [*Verwenden von Endpunkten und Routen in Azure Digital Twins*](how-to-manage-routes.md)