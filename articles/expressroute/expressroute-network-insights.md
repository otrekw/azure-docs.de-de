---
title: Azure ExpressRoute Insights mit Verwendung von Network Insights
description: Erfahren Sie mehr über Azure ExpressRoute Insights mit Verwendung von Network Insights.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 04/05/2021
ms.author: duau
ms.openlocfilehash: a5e01a4fc84f852b43e5a76114bebe145da10793
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106443582"
---
# <a name="azure-expressroute-insights-using-network-insights"></a>Azure ExpressRoute Insights mit Verwendung von Network Insights

In diesem Artikel wird erläutert, wie Sie mithilfe von Network Insights ihre ExpressRoute-Metriken und -Konfigurationen an einem Ort anzeigen können. Mithilfe von Network Insights können Sie topologische Zuordnungen und Integritätsdashboards anzeigen, die wichtige ExpressRoute-Informationen enthalten, ohne dass zusätzliche Einrichtungsschritte ausgeführt werden müssen.

:::image type="content" source="./media/expressroute-network-insights/network-monitor-page.png" alt-text="Screenshot der Monitor-Netzwerke-Landing Page." lightbox="./media/expressroute-network-insights/monitor-landing-page-expanded.png":::

## <a name="visualize-functional-dependencies"></a>Visualisieren funktionaler Abhängigkeiten

1. Navigieren Sie zur Seite *Azure Monitor*, und wählen Sie *Netzwerke* aus.

    :::image type="content" source="./media/expressroute-network-insights/monitor-page.png" alt-text="Screenshot der Monitor-Landing Page.":::

1. Wählen Sie die Karte *ExpressRoute-Leitungen* aus. 

1. Wählen Sie dann die Topologieschaltfläche für die Verbindung aus, die Sie anzeigen möchten.

   :::image type="content" source="./media/expressroute-network-insights/monitor-landing-page.png" alt-text="Screenshot der ExpressRoute-Monitor-Landing Page." lightbox="./media/expressroute-network-insights/monitor-landing-page-expanded.png"::: 

1. Die Ansicht der funktionalen Abhängigkeit bietet ein klares Bild Ihres ExpressRoute-Setups und skizziert die Beziehung zwischen verschiedenen ExpressRoute-Komponenten (Peerings, Verbindungen, Gateways).

    :::image type="content" source="./media/expressroute-network-insights/topology-view.png" alt-text="Screenshot der Topologieansicht für Network Insights." lightbox="./media/expressroute-network-insights/topology-view-expanded.png":::

1. Zeigen Sie auf eine beliebige Komponente in der Topologiekarte, um Konfigurationsinformationen anzuzeigen. Zeigen Sie z. B. auf eine ExpressRoute-Peeringkomponente, um Details wie die Verbindungsbandbreite und Global Reach-Aktivierung anzuzeigen.

    :::image type="content" source="./media/expressroute-network-insights/topology-hovered.png" alt-text="Screenshot des Zeigens auf die Topologieansichtsressourcen." lightbox="./media/expressroute-network-insights/topology-hovered-expanded.png":::

## <a name="view-a-detailed-and-pre-loaded-metrics-dashboard"></a>Anzeigen eines detaillierten und vorab geladenen Metrikdashboards

Nachdem Sie die Topologie Ihres ExpressRoute-Setups mithilfe der Ansicht der funktionalen Abhängigkeit überprüft haben, wählen Sie **Detaillierte Metriken anzeigen** aus, um zur Metrikendetailansicht zu navigieren, um die Leistung der Verbindung zu verstehen. Diese Ansicht bietet eine organisierte Liste verknüpfter Ressourcen und ein umfangreiches Dashboard wichtiger ExpressRoute-Metriken.

Im Abschnitt **Verknüpfte Ressourcen** sind die verbundenen ExpressRoute-Gateways und konfigurierten Peerings aufgelistet, die Sie auswählen können, um zur entsprechenden Ressourcenseite zu navigieren.

:::image type="content" source="./media/expressroute-network-insights/linked-resources.png" alt-text="Screenshot der verknüpften Ressourcen auf der Monitor-Seite.":::


Der Abschnitt **ExpressRoute-Metriken** enthält Diagramme wichtiger Verbindungsmetriken in den Kategorien **Verfügbarkeit**, **Durchsatz**, **Paketverluste** und **Gatewaymetriken**.

### <a name="availability"></a>Verfügbarkeit

Auf der Registerkarte *Verfügbarkeit* werden ARP- und BGP-Verfügbarkeit nachverfolgt und sowohl die Daten für die Verbindung als Ganzes als auch einzelne Verbindungen (primär und sekundär) gezeigt. 

:::image type="content" source="./media/expressroute-network-insights/arp-bgp-availability.png" alt-text="Screenshot der Metrikendiagramme für die Verfügbarkeit." lightbox="./media/expressroute-network-insights/arp-bgp-availability-expanded.png":::

### <a name="throughput"></a>Throughput

Auf ähnliche Weise wird auf der Registerkarte *Durchsatz* der Gesamtdurchsatz des eingehenden und ausgehenden Datenverkehrs für die Verbindung in Bits/Sekunde angezeigt. Sie können auch den Durchsatz für einzelne Verbindungen und jede Art konfigurierten Peerings anzeigen.

:::image type="content" source="./media/expressroute-network-insights/throughput.png" alt-text="Screenshot der Metrikdiagramme für den Durchsatz." lightbox="./media/expressroute-network-insights/throughput-expanded.png":::

### <a name="packet-drops"></a>Paketverluste

Die Registerkarte *Paketverluste* zeigt die verloren gegangenen Bits/Sekunde für den über die Verbindung eingehenden und ausgehenden Datenverkehr. Diese Registerkarte bietet eine einfache Möglichkeit zum Überwachen von Leistungsproblemen, die auftreten können, wenn Sie die Verbindungsbandbreite regelmäßig auslasten oder überschreiten.

:::image type="content" source="./media/expressroute-network-insights/dropped-packets.png" alt-text="Screenshot von Diagrammen verloren gegangener Pakete." lightbox="./media/expressroute-network-insights/dropped-packets-expanded.png":::

### <a name="gateway-metrics"></a>Gatewaymetriken

Schließlich wird die Registerkarte „Gatewaymetriken“ mit Diagrammen von Schlüsselmetriken für ein ausgewähltes ExpressRoute-Gateway (aus dem Abschnitt „Verknüpfte Ressourcen“) aufgefüllt. Verwenden Sie diese Registerkarte, wenn Sie die Konnektivität mit bestimmten virtuellen Netzwerken überwachen müssen.

:::image type="content" source="./media/expressroute-network-insights/gateway-metrics.png" alt-text="Screenshot von Gatewaydurchsatz und CPU-Metriken." lightbox="./media/expressroute-network-insights/gateway-metrics-expanded.png":::

## <a name="next-steps"></a>Nächste Schritte

Konfigurieren Sie Ihre ExpressRoute-Verbindung.
  
* Erfahren Sie mehr über [Azure ExpressRoute](expressroute-introduction.md), [Network Insights](../azure-monitor/insights/network-insights-overview.md) und [Network Watcher](../network-watcher/network-watcher-monitoring-overview.md).
* [Erstellen und Ändern einer Verbindung](expressroute-howto-circuit-arm.md)
* [Erstellen und Ändern einer Peeringkonfiguration](expressroute-howto-routing-arm.md)
* [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)
* [Anpassen Ihrer Metriken](expressroute-monitoring-metrics-alerts.md) und Erstellen eines [Verbindungsmonitors](../network-watcher/connection-monitor-overview.md)
