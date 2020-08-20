---
title: Überwachen von Azure Virtual WAN mithilfe von Azure Monitor Insights
description: Hier erfahren Sie mehr über die Überwachung von Azure Virtual WAN mithilfe von Azure Monitor Insights.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 07/22/2020
ms.author: cherylmc
ms.openlocfilehash: e3316b4a2255652972a0b9deef813f894f993589
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87836070"
---
# <a name="azure-monitor-insights-for-virtual-wan-preview"></a>Azure Monitor Insights für Virtual WAN (Vorschau)

[Azure Monitor Insights](../azure-monitor/insights/network-insights-overview.md) für Virtual WAN bietet Benutzern und Operatoren die Möglichkeit, den Zustand und Status von Virtual WAN über ein automatisch erkanntes topologisches Diagramm anzuzeigen. Zustand und Status von Ressourcen werden im Diagramm überlagert, sodass Sie eine Momentaufnahmenansicht der Gesamtintegrität des virtuellen WAN erhalten. Die Ressourcennavigation wird im Diagramm per Zugriff auf die Seiten mit der Ressourcenkonfiguration im Virtual WAN-Portal mit nur einem Klick aktiviert.

Virtual WAN-Metriken auf Ressourcenebene werden über eine vorkonfigurierte Virtual WAN-Arbeitsmappe für Metriken erfasst und dargestellt. In der Arbeitsmappe werden die Metriken auf der Ebene des virtuellen WAN, des Hubs, des Gateways und der Verbindung angezeigt. In diesem Artikel erfahren Sie, wie Sie mit Azure Monitor Insights für Virtual WAN Virtual WAN-Topologien und -Metriken an einer zentralen Stelle anzeigen können.

> [!NOTE]
> Die Insights-Menüoption im Virtual WAN-Portal wird derzeit eingeführt. Das Insights-Menü für Virtual WAN wird gerade erst eingeführt, auf die Virtual WAN-Topologie und die Virtual WAN-Arbeitsmappe für Metriken kann jedoch direkt über Azure Monitor für Netzwerke zugegriffen werden. Weitere Informationen finden Sie unter [Azure Monitor für Netzwerke (Vorschau)](../azure-monitor/insights/network-insights-overview.md). 
>

## <a name="before-you-begin"></a>Voraussetzungen

Bei den Schritten in diesem Artikel wird davon ausgegangen, dass Sie bereits ein virtuelles WAN mit einem oder mehreren Hubs bereitgestellt haben. Führen Sie die Schritte in den folgenden Artikeln aus, um ein neues virtuelles WAN und einen neuen Hub zu erstellen:

* [Erstellen eines virtuellen WAN](virtual-wan-site-to-site-portal.md#openvwan)
* [Erstellen eines Hubs](virtual-wan-site-to-site-portal.md#hub)

## <a name="view-vwan-topology"></a><a name="topology"></a>Anzeigen einer VWAN-Topologie

Wählen Sie im **Azure-Portal ->Virtual WAN** im Menü **Monitor** auf der linken Seite die Option **Insights (Vorschau)** aus. Daraufhin wird die **Insights-Ansicht** eingeblendet. Hier werden das Virtual WAN-Abhängigkeitsdiagramm und eine Mini-Arbeitsmappe mit allgemeinen Metriken angezeigt.

**Abbildung 1: Menü „Monitor: Insights“**

:::image type="content" source="./media/azure-monitor-insights/monitor-menu.png" alt-text="Abbildung" lightbox="./media/azure-monitor-insights/monitor-menu.png":::

In der **Insights**-Ansicht können Sie automatisch erkannte Virtual WAN-Ressourcen wie Hubs, Gateways, Firewalls, Verbindungen und Spoke-VNets, NVAs von Drittanbietern und Branches in einem End-to-End-VWAN anzeigen (siehe **Abbildung 2**).

Der **Ressourcenzustand** und der **Ressourcenstatus** sind farbcodiert und in den Ressourcensymbolen im Diagramm überlagert. Allgemeine Virtual WAN-Metriken wie Hubkapazitäten und Gatewayauslastung werden auf der rechten Seite über eine Mini-Arbeitsmappe angezeigt.

**Abbildung 2: Insights-Ansicht**

:::image type="content" source="./media/azure-monitor-insights/insights-view.png" alt-text="Abbildung" lightbox="./media/azure-monitor-insights/insights-view.png":::

## <a name="dependency-view"></a><a name="dependency"></a>Abhängigkeitsansicht

Mit der Ansicht **Abhängigkeit** für Virtual WAN kann die vernetzte Ansicht aller in einer Hub-und-Spoke-Architektur strukturierten Virtual WAN-Ressourcen angezeigt werden.

**Abbildung 3: Ansicht der VWAN-Abhängigkeiten**

:::image type="content" source="./media/azure-monitor-insights/dependency-map.png" alt-text="Abhängigkeitsdiagramm" lightbox="./media/azure-monitor-insights/dependency-map.png":::

In der Ansicht mit dem Abhängigkeitsdiagramm werden die folgenden Ressourcen als verbundenes Diagramm angezeigt:

* Virtual WAN-Hubs in den verschiedenen Azure-Regionen
* Spoke-VNets, die direkt mit dem Hub verbunden sind
* VPN- und ExpressRoute-Zweigstellen und P2S-Benutzer, die mit den einzelnen Hubs über die jeweiligen ExpressRoute-, S2S- und P2S-Verbindungen und Gateways für virtuelle Netzwerke verbunden sind
* In einem Hub (geschützten Hub) bereitgestellte Azure Firewalls (wie Firewallproxys von Drittanbietern)
* In einem Spoke-VNet bereitgestellte NVAs (Network Virtual Appliances) von Drittanbietern

Im Abhängigkeitsdiagramm werden auch indirekt verbundene VNets angezeigt (d. h., VNets, die als Peers mit einem Virtual WAN-Spoke-VNet verbunden sind).

Das Abhängigkeitsdiagramm ermöglicht eine einfache Navigation zu den Konfigurationseinstellungen der einzelnen Ressourcen. Beispielsweise können Sie mit dem Mauszeiger auf die Hubressource zeigen, um die grundlegende Ressourcenkonfiguration etwa von Hubregion und Hubpräfix anzuzeigen. Klicken Sie mit der rechten Maustaste, um auf die Azure-Portal-Seite der Hubressource zuzugreifen.

**Abbildung 4: Navigieren zu ressourcenspezifischen Informationen**

:::image type="content" source="./media/azure-monitor-insights/resource-information.png" alt-text="Ressourceninformationen":::

Die Such- und Filterleiste in der Abhängigkeitsansicht bietet eine einfache Möglichkeit, das Diagramm zu durchsuchen. Verschiedene Filter bieten Hilfe beim Eingrenzen der Suche auf einen bestimmten Pfad und Zustand.

**Abbildung 5: Suchen und Filtern**

:::image type="content" source="./media/azure-monitor-insights/search-filter.png" alt-text="Such- und Filterleiste" lightbox="./media/azure-monitor-insights/search-filter.png":::

## <a name="detailed-metrics"></a><a name="detailed"></a>Detaillierte Metriken

Sie können **Detaillierte Metriken anzeigen** auswählen, um die Seite mit den detaillierten **Metriken** aufzurufen. Bei der Seite „Metriken“ handelt es sich um ein Dashboard, das mit separaten Registerkarten vorkonfiguriert ist und nützliche Informationen zu Kapazität, Leistung und Auslastung der VWAN-Ressource auf der Ebene des virtuellen WAN, des Hubs und der einzelnen Verbindungen bereitstellt.

**Abbildung 6: Dashboard „Detaillierte Metriken“**

:::image type="content" source="./media/azure-monitor-insights/detailed-metrics.png" alt-text="detaillierte Metriken" lightbox="./media/azure-monitor-insights/detailed-metrics.png":::

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Metriken in Azure Monitor finden Sie unter [Metriken in Azure Monitor](../azure-monitor/platform/data-platform-metrics.md).
* Eine umfassende Beschreibung aller Virtual WAN-Metriken finden Sie unter [Virtual WAN-Protokolle und -Metriken](logs-metrics.md).
