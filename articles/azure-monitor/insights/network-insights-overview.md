---
title: Azure Monitor für Netzwerke (Vorschau)
description: Eine kurze Übersicht über Azure Monitor für Netzwerke, das eine umfassende Ansicht der Integrität und Metriken für alle bereitgestellten Netzwerkressourcen ohne jegliche Konfiguration bietet.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/24/2020
ms.openlocfilehash: 2559c4f54aa19df248ddf756e376809dea516997
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91330955"
---
# <a name="azure-monitor-for-networks-preview"></a>Azure Monitor für Netzwerke (Vorschau)
Azure Monitor für Netzwerke bietet eine umfassende Ansicht der [Integrität](https://docs.microsoft.com/azure/service-health/resource-health-checks-resource-types) und [Metriken](../platform/metrics-supported.md) für alle bereitgestellten Netzwerkressourcen ohne jegliche Konfiguration.  Außerdem erhalten Sie Zugriff auf alle Netzwerküberwachungsfunktionen, z. B. [Verbindungsmonitor](../../network-watcher/connection-monitor-preview.md), [Datenflussprotokollierung für Netzwerksicherheitsgruppen (NSGs)](../../network-watcher/network-watcher-nsg-flow-logging-overview.md), [Traffic Analytics](../../network-watcher/traffic-analytics.md) und andere Features für die [Netzwerkdiagnose](../../network-watcher/network-watcher-monitoring-overview.md#diagnostics).

Azure Monitor für Netzwerke umfasst die folgenden Hauptkomponenten der Überwachung:
- [Netzwerkintegrität und Metriken](#networkhealth)
- [Konnektivität](#connectivity)
- [Verkehr](#traffic)
- [Diagnosetoolkit](#diagnostictoolkit)

## <a name="network-health-and-metrics"></a><a name="networkhealth"></a>Netzwerkintegrität und Metriken

Die Seite **Übersicht** von Azure Monitor für Netzwerke bietet eine einfache Möglichkeit, Ihren Bestand an Netzwerkressourcen zusammen mit der Ressourcenintegrität und Warnungen visuell darzustellen. Sie enthält vier Hauptfunktionsbereiche: „Suchen und Filtern“, „Ressourcenintegrität und Metriken“, „Warnungen“ und „Abhängigkeitsansicht“.

![Seite „Übersicht“](media/network-insights-overview/overview.png)

### <a name="search-and-filtering"></a>Suchen und Filtern
Die Ansicht für Ressourcenintegrität und Warnungen kann mithilfe von Filtern wie **Abonnement**, **Ressourcengruppe** und **Ressourcentyp** angepasst werden. Das Suchfeld bietet die Möglichkeit, Ressourceneigenschaften zu durchsuchen.

Mit dem Suchfeld kann nach Ressourcen und zugeordneten Ressourcen gesucht werden. Beispielsweise ist einer Application Gateway-Instanz eine öffentliche IP-Adresse zugeordnet. Bei der Suche nach dem DNS-Namen der öffentlichen IP-Adresse werden sowohl die öffentliche IP-Adresse als auch die zugeordnete Application Gateway-Instanz angegeben.

![Azure Monitor für Netzwerke – Suchen](media/network-insights-overview/search.png)


### <a name="resource-health-and-metric"></a>Ressourcenintegrität und Metriken
Jede Kachel stellt einen Ressourcentyp dar und gibt die Anzahl der in allen ausgewählten Abonnements bereitgestellten Instanzen zusammen mit dem Ressourcenintegritätsstatus an. Im folgenden Beispiel sind 45 ER- und VPN-Verbindungen bereitgestellt, von denen 44 fehlerfrei und 1 nicht verfügbar sind.

![Azure Monitor für Netzwerke – Ressourcenintegrität](media/network-insights-overview/resource-health.png)

Wenn Sie auf die nicht verfügbaren ER- und VPN-Verbindungen klicken, wird eine Metrikansicht geöffnet. 

![Azure Monitor für Netzwerke – Metrikansicht](media/network-insights-overview/metric-view.png)

Sie können auf jedes Element in der Rasteransicht klicken. Klicken Sie auf das Symbol für „Integrität“, um zur Ressourcenintegrität für diese Verbindung zu gelangen. Klicken Sie auf „Warnungen“, um zur Seite mit Warnungen und Metriken für diese Verbindung zu gelangen. 

### <a name="alerts"></a>Alerts
Das Raster **Warnungen** auf der rechten Seite bietet eine Übersicht über alle Warnungen, die für die ausgewählten Ressourcen in allen Abonnements generiert wurden. Klicken Sie auf die Warnungsanzahl, um zur Detailseite für die Warnungen zu navigieren.

### <a name="dependency-view"></a>Abhängigkeitsansicht
Mithilfe der **Abhängigkeitsansicht** wird die Konfiguration der Ressource visuell veranschaulicht. Derzeit wird die Abhängigkeitsansicht für Application Gateway, Virtual WAN und Load Balancer unterstützt. Im Fall von Application Gateway können Sie beispielsweise auf die Abhängigkeitsansicht zugreifen, indem Sie in der Rasteransicht der Metriken auf den Namen der Application Gateway-Ressource klicken. Dies gilt auch für Virtual WAN und Load Balancer.

![Azure Monitor für Netzwerke – Application Gateway-Ansicht](media/network-insights-overview/application-gateway.png)

Die **Abhängigkeitsansicht** für Application Gateway bietet eine vereinfachte Übersicht darüber, wie die Front-End-IP-Adressen mit den Listenern, den Regeln und dem Back-End-Pool verbunden sind. Die Verbindungsstellen sind farbcodiert und stellen zusätzliche Details basierend auf der Integrität des Back-End-Pools bereit. Die Ansicht bietet auch eine detaillierte Übersicht über Application Gateway-Metriken und Metriken für alle zugehörigen Back-End-Pools wie VM-Skalierungsgruppen- und VM-Instanzen.

![Azure Monitor für Netzwerke – Abhängigkeitsansicht](media/network-insights-overview/dependency-view.png)

Das Abhängigkeitsdiagramm ermöglicht eine einfache Navigation zu Konfigurationseinstellungen. Klicken Sie mit der rechten Maustaste auf einen Back-End-Pool, um auf andere Funktionen zuzugreifen. Wenn es sich beim Back-End-Pool beispielsweise um eine VM handelt, können Sie direkt auf VM Insights und die Fehlerbehebung für Network Watcher-Verbindungen zugreifen, um Verbindungsprobleme zu ermitteln.

![Azure Monitor für Netzwerke – Menü der Abhängigkeitsansicht](media/network-insights-overview/dependency-view-menu.png)

Die Such- und Filterleiste in der Abhängigkeitsansicht bietet eine einfache Möglichkeit, das Diagramm zu durchsuchen. Wenn Sie im folgenden Beispiel nach *AppGWTestRule* suchen, wird die grafische Ansicht auf alle über *AppGWTestRule* verbundenen Knoten eingegrenzt.

![Azure Monitor für Netzwerke – Beispiel für Suche](media/network-insights-overview/search-example.png)

Verschiedene Filter bieten Hilfe beim Eingrenzen auf einen bestimmten Pfad und Zustand. Wählen Sie z.B. in der Dropdownliste **Integritätsstatus** nur *Fehlerhaft* aus, um alle Edge-Geräte mit dem Status *Fehlerhaft* anzuzeigen.

Klicken Sie auf **Detaillierte Metriken anzeigen**, um eine vorkonfigurierte Arbeitsmappe mit detaillierten Metriken für Application Gateway, alle Back-End-Poolressourcen und Front-End-IP-Adressen zu öffnen. 

## <a name="connectivity"></a><a name="connectivity"></a>Konnektivität

Auf der Registerkarte **Konnektivität** können Sie mühelos alle mit Verbindungsmonitor und [Verbindungsmonitor (Vorschau)](../../network-watcher/connection-monitor-preview.md) konfigurierten Tests für den ausgewählten Satz von Abonnements visualisieren.

![Registerkarte „Konnektivität“ in Azure Monitor für Netzwerke](media/network-insights-overview/azure-monitor-for-networks-connectivity-tab.png)

Die Tests sind in den Kacheln „Quellen“ und „Ziele“ gruppiert und zeigen den Status der Erreichbarkeit für jeden Test an. Mit den Erreichbarkeitseinstellungen lassen sich die Erreichbarkeitskriterien auf Grundlage der Überprüfungen mit Fehlern (%) und RTT (ms) einfach konfigurieren. Nachdem die Werte festgelegt wurden, wird der Status für jeden Test basierend auf den Auswahlkriterien aktualisiert.

![Konnektivitätstests in Azure Monitor für Netzwerke](media/network-insights-overview/azure-monitor-for-networks-connectivity-tests.png)

Durch Klicken auf eine Quell- oder Zielkachel wird eine Metrikansicht geöffnet.

![Konnektivitätsmetriken in Azure Monitor für Netzwerke](media/network-insights-overview/azure-monitor-for-networks-connectivity-metrics.png)


Sie können auf jedes Element in der Rasteransicht klicken. Klicken Sie auf das Symbol **Erreichbarkeit**, um zur Portalseite **Verbindungsmonitor** zu gelangen. Dort können Sie die Hop-by-Hop-Topologie sowie die identifizierten Konnektivitätsprobleme anzeigen. Klicken Sie auf **Warnungen**, um zu Warnungen zu gelangen, und auf **Fehler bei Überprüfungen in Prozent/Roundtripzeit**, um zur Seite mit den Metriken für den ausgewählten Verbindungsmonitor zu gelangen.

Das Raster  **Warnungen**  auf der rechten Seite bietet eine Übersicht über alle Warnungen, die für die konfigurierten Konnektivitätstests in allen Abonnements generiert wurden. Klicken Sie auf die Warnungsanzahl, um zur Detailseite für die Warnungen zu navigieren.

## <a name="traffic"></a><a name="traffic"></a>Verkehr
Die Registerkarte „Datenverkehr“ ermöglicht den Zugriff auf alle für [NSG-Datenflussprotokolle](../../network-watcher/network-watcher-nsg-flow-logging-overview.md) und [Traffic Analytics](../../network-watcher/traffic-analytics.md) konfigurierten NSGs für den ausgewählten Satz von Abonnements, nach Standorten gruppiert. Die Suchfunktion auf dieser Registerkarte ermöglicht die Identifizierung der NSGs, die für die gesuchte IP-Adresse konfiguriert sind. Sie können eine beliebige IP-Adresse in Ihrer Umgebung suchen. In der regionalen Kachelansicht werden alle NSGs zusammen mit den NSG-Datenflussprotokollen und dem Konfigurationsstatus von Traffic Analytics angezeigt.

![Datenverkehrsansicht in Azure Monitor für Netzwerke](media/network-insights-overview/azure-monitor-for-networks-traffic-view.png)

Durch Klicken auf eine Regionskachel wird eine Rasteransicht geöffnet, die einfach anzuzeigende und zu konfigurierende NSG-Datenflussprotokolle und Datenverkehrsanalysen enthält.  

![Regionsansicht des Datenverkehrs in Azure Monitor für Netzwerke](media/network-insights-overview/azure-monitor-for-networks-traffic-region-view.png)

Sie können auf jedes Element in der Rasteransicht klicken. Klicken Sie auf den Konfigurationsstatus, um das NSG-Datenflussprotokoll und die Traffic Analytics-Konfiguration zu bearbeiten. Klicken Sie auf Warnungen, um zu den für die ausgewählte NSG konfigurierten Datenverkehrswarnungen zu gelangen. Sie können auch zur Traffic Analytics-Ansicht navigieren, indem Sie auf den Arbeitsbereich klicken.  

Das Raster  **Warnungen**  auf der rechten Seite bietet eine Übersicht über alle auf Warnungen basierende Traffic Analytics-Arbeitsbereiche für alle Abonnements. Klicken Sie auf die Warnungsanzahl, um zur Detailseite für die Warnungen zu navigieren.

## <a name="diagnostic-toolkit"></a><a name="diagnostictoolkit"></a> Diagnosetoolkit
Das Diagnosetoolkit bietet Zugriff auf alle Diagnosefeatures, die für das Behandeln von Netzwerkproblemen verfügbar sind. In dieser Dropdownliste können Sie auf Features wie [Paketerfassung](../../network-watcher/network-watcher-packet-capture-overview.md), [VPN-Problembehandlung](../../network-watcher/network-watcher-troubleshoot-overview.md), [Problembehandlung für Verbindungen](../../network-watcher/network-watcher-connectivity-overview.md), [Nächster Hop](../../network-watcher/network-watcher-next-hop-overview.md) und [IP-Datenflussüberprüfung](../../network-watcher/network-watcher-ip-flow-verify-overview.md) zugreifen.

![Registerkarte „Diagnosetoolkit“](media/network-insights-overview/azure-monitor-for-networks-diagnostic-toolkit.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Netzwerküberwachung finden Sie unter [Was ist Azure Network Watcher?](../../network-watcher/network-watcher-monitoring-overview.md).
