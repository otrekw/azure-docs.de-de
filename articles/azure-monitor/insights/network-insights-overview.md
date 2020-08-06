---
title: Azure Monitor für Netzwerke (Vorschau)
description: Eine kurze Übersicht über Azure Monitor für Netzwerke, das eine umfassende Ansicht der Integrität und Metriken für alle bereitgestellten Netzwerkressourcen ohne jegliche Konfiguration bietet.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/07/2019
ms.openlocfilehash: ccc5578944dc6eea9a62360045272896d78e2fac
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87128674"
---
# <a name="azure-monitor-for-networks-preview"></a>Azure Monitor für Netzwerke (Vorschau)
Azure Monitor für Netzwerke bietet eine umfassende Ansicht der Integrität und Metriken für alle bereitgestellten Netzwerkressourcen ohne jegliche Konfiguration. Die erweiterte Suchfunktion hilft beim Identifizieren von Ressourcenabhängigkeiten und ermöglicht Szenarien wie das Identifizieren von Ressourcen, auf denen Ihre Website gehostet wird, indem einfach nach dem Namen der gehosteten Website gesucht wird.

Die Seite **Übersicht** von Azure Monitor für Netzwerke bietet eine einfache Möglichkeit, Ihren Bestand an Netzwerkressourcen zusammen mit der Ressourcenintegrität und Warnungen visuell darzustellen. Diese Seite ist in vier Hauptfunktionsbereiche unterteilt:

- Suchen und Filtern
- Ressourcenintegrität und Metriken
- Alerts 
- Abhängigkeitsansicht

![Seite „Übersicht“](media/network-insights-overview/overview.png)

## <a name="search-and-filtering"></a>Suchen und Filtern
Die Ansicht für Ressourcenintegrität und Warnungen kann mithilfe von Filtern wie **Abonnement**, **Ressourcengruppe** und **Ressourcentyp** angepasst werden. Das Suchfeld bietet die Möglichkeit, Ressourceneigenschaften zu durchsuchen.

Mit dem Suchfeld kann nach Ressourcen und zugeordneten Ressourcen gesucht werden. Beispielsweise ist einer Application Gateway-Instanz eine öffentliche IP-Adresse zugeordnet. Bei der Suche nach dem DNS-Namen der öffentlichen IP-Adresse werden sowohl die öffentliche IP-Adresse als auch die zugeordnete Application Gateway-Instanz angegeben.

![Suchen,](media/network-insights-overview/search.png)


## <a name="resource-health-and-metric"></a>Ressourcenintegrität und Metriken
Jede Kachel stellt einen Ressourcentyp dar und gibt die Anzahl der in allen ausgewählten Abonnements bereitgestellten Instanzen zusammen mit dem Ressourcenintegritätsstatus an. Im folgenden Beispiel sind 105 ER- und VPN-Verbindungen bereitgestellt, von denen 103 fehlerfrei und 2 nicht verfügbar sind.

![Ressourcenintegrität](media/network-insights-overview/resource-health.png)

Wenn Sie auf die beiden nicht verfügbaren ER- und VPN-Verbindungen klicken, wird eine Metrikansicht geöffnet. 

![Metrikansicht](media/network-insights-overview/metric-view.png)

Sie können auf jedes Element in der Rasteransicht klicken. Klicken Sie auf das Symbol für „Integrität“, um zur Ressourcenintegrität für diese Verbindung zu gelangen. Klicken Sie auf „Warnungen“, um zur Seite mit Warnungen und Metriken für diese Verbindung zu gelangen. 

## <a name="alerts"></a>Alerts
Das Raster **Warnungen** auf der rechten Seite bietet eine Übersicht über alle Warnungen, die für die ausgewählten Ressourcen in allen Abonnements generiert wurden. Klicken Sie auf die Warnungsanzahl, um zur Detailseite für die Warnungen zu navigieren.

## <a name="dependency-view"></a>Abhängigkeitsansicht
Mithilfe der **Abhängigkeitsansicht** wird die Konfiguration der Ressource visuell veranschaulicht. Derzeit wird die Abhängigkeitsansicht für Application Gateway, Virtual WAN und Load Balancer unterstützt. Im Fall von Application Gateway können Sie beispielsweise auf die Abhängigkeitsansicht zugreifen, indem Sie in der Rasteransicht der Metriken auf den Namen der Application Gateway-Ressource klicken. Dies gilt auch für Virtual WAN und Load Balancer. 

![Application Gateway-Ansicht](media/network-insights-overview/application-gateway.png)

Die **Abhängigkeitsansicht** für Application Gateway bietet eine vereinfachte Übersicht darüber, wie die Front-End-IP-Adressen mit den Listenern, den Regeln und dem Back-End-Pool verbunden sind. Die Verbindungsstellen sind farbcodiert und stellen zusätzliche Details basierend auf der Integrität des Back-End-Pools bereit. Die Ansicht bietet auch eine detaillierte Übersicht über Application Gateway-Metriken und Metriken für alle zugehörigen Back-End-Pools wie VMSS- und VM-Instanzen.

![Abhängigkeitsansicht](media/network-insights-overview/dependency-view.png)

Das Abhängigkeitsdiagramm ermöglicht eine einfache Navigation zu Konfigurationseinstellungen. Klicken Sie mit der rechten Maustaste auf einen Back-End-Pool, um auf andere Funktionen zuzugreifen. Wenn es sich beim Back-End-Pool beispielsweise um eine VM handelt, können Sie direkt auf VM Insights und die Fehlerbehebung für Network Watcher-Verbindungen zugreifen, um Verbindungsprobleme zu ermitteln.

![Menü in der Abhängigkeitsansicht](media/network-insights-overview/dependency-view-menu.png)

Die Such- und Filterleiste in der Abhängigkeitsansicht bietet eine einfache Möglichkeit, das Diagramm zu durchsuchen. Wenn Sie im folgenden Beispiel nach *AppGWTestRule* suchen, wird die grafische Ansicht auf alle über *AppGWTestRule* verbundenen Knoten eingegrenzt. 

![Suchbeispiel](media/network-insights-overview/search-example.png)

Verschiedene Filter bieten Hilfe beim Eingrenzen auf einen bestimmten Pfad und Zustand. Wählen Sie z.B. in der Dropdownliste **Integritätsstatus** nur *Fehlerhaft* aus, um alle Edge-Geräte mit dem Status *Fehlerhaft* anzuzeigen.

Klicken Sie auf **Detaillierte Metriken anzeigen**, um eine vorkonfigurierte Arbeitsmappe mit detaillierten Metriken für Application Gateway, alle Back-End-Poolressourcen und Front-End-IP-Adressen zu öffnen. 

## <a name="next-steps"></a>Nächste Schritte 

- Weitere Informationen zur Netzwerküberwachung finden Sie unter [Was ist Azure Network Watcher?](../../network-watcher/network-watcher-monitoring-overview.md).
