---
title: Integration in andere Dienste
titleSuffix: Azure Digital Twins
description: Grundlegendes zu den Ein- und Ausgabeanforderungen beim Bereitstellen von Azure Digital Twins.
author: baanders
ms.author: baanders
ms.date: 3/16/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 9c7c1000ed5229886d4a964161042eca6517c230
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87367434"
---
# <a name="integrate-azure-digital-twins-with-other-services"></a>Integrieren von Azure Digital Twins in andere Dienste

Azure Digital Twins wird in der Regel in Verbindung mit anderen Diensten verwendet. Unter Verwendung von [**Ereignisrouten**](concepts-route-events.md) empfängt Azure Digital Twins Daten von Upstreamdiensten wie [IoT Hub](../iot-hub/about-iot-hub.md), die zur Bereitstellung von Telemetrie und Benachrichtigungen verwendet werden. Azure Digital Twins kann Daten auch an Downstreamdienste zur Speicherung, Workflowintegration, Analyse und für andere Zwecke weiterleiten. 

## <a name="data-ingress"></a>Dateneingang

Azure Digital Twins kann mit Daten und Ereignissen aus jedem Dienst gespeist werden – IoT Hub, Logic Apps, Ihrem eigenen benutzerdefinierten Dienst usw. Dies ermöglicht Ihnen, Telemetrie von physischen Geräten in Ihrer Umgebung zu erfassen und diese Daten mithilfe des Azure Digital Twins-Graphen in der Cloud zu verarbeiten.

Azure Digital Twins verfügt über keinen integrierten IoT Hub. Sie können einen vorhandenen IoT Hub verwenden, den Sie derzeit in der Produktionsumgebung einsetzen, oder einen neuen bereitstellen. Dadurch erhalten Sie vollen Zugriff auf alle Geräteverwaltungsfunktionen des IoT Hubs.

Um Daten von einer beliebigen Quelle in Azure Digital Twins zu erfassen, verwenden Sie eine [Azure-Funktion](../azure-functions/functions-overview.md). Weitere Informationen zu diesem Muster finden Sie unter [*Gewusst wie: Erfassen von Telemetriedaten aus IoT Hub*](how-to-ingest-iot-hub-data.md), oder testen Sie es selbst im Azure Digital Twins-[*Tutorial: Verbinden einer End-to-End-Lösung*](tutorial-end-to-end.md).

## <a name="data-egress-services"></a>Datenausgangsdienste

Azure Digital Twins kann Daten an verbundene **Endpunkte** senden. Folgende Endpunkte können unterstützt werden:
* [Event Hub](../event-hubs/event-hubs-about.md)
* [Event Grid](../event-grid/overview.md)
* [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)

Endpunkte werden über Verwaltungs-APIs oder das Azure-Portal an Azure Digital Twins angefügt. Weitere Informationen zum Anfügen eines Endpunkts an Azure Digital Twins finden Sie unter [*Gewusst wie: Verwalten von Endpunkten und Routen*](how-to-manage-routes.md).

Es gibt viele andere Dienste, an die Sie Ihre Daten letztendlich leiten möchten, z. B. [Azure Storage](../storage/common/storage-introduction.md) oder [Time Series Insights](../time-series-insights/time-series-insights-update-overview.md). Fügen Sie den Zieldienst an einen Endpunkt an, um Ihre Daten an Dienste wie diese zu senden.

Wenn Sie z. B. auch [Azure Maps](../azure-maps/about-azure-maps.md) verwenden und den Standort mit Ihrem Azure Digital Twins-[Zwillingsgraphen](concepts-twins-graph.md) korrelieren möchten, können Sie Azure Functions mit Event Grid verwenden, um die Kommunikation zwischen allen Diensten in Ihrer Bereitstellung herzustellen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Endpunkten und zur Weiterleitung von Ereignissen an externe Dienste:
* [*Konzepte: Weiterleiten von Azure Digital Twins-Ereignissen*](concepts-route-events.md)

Erfahren Sie, wie Sie Azure Digital Twins zum Erfassen von Daten aus IoT Hub einrichten:
* [*Verwenden Erfassen von Telemetriedaten aus IoT Hub*](how-to-ingest-iot-hub-data.md)
