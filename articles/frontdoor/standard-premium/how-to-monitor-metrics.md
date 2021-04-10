---
title: Überwachungsmetriken für Azure Front Door Standard/Premium
description: In diesem Artikel werden die Überwachungsmetriken für Azure Front Door Standard/Premium beschrieben.
services: frontdoor
author: duau
manager: KumudD
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: yuajia
ms.openlocfilehash: 72388eb8006ff1b9628db5066dc63e6a0811f3d5
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105557325"
---
# <a name="real-time-monitoring-in-azure-front-door-standardpremium"></a>Echtzeitüberwachung in Azure Front Door Standard/Premium

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

Azure Front Door Standard/Premium ist in Azure Monitor integriert und bietet 11 Metriken, um Probleme in Echtzeit zu verfolgen, zu beheben und zu debuggen.  

Die Metriken von Azure Front Door Standard/Premium werden in Intervallen von 60 Sekunden gemessen und gesendet. Es kann bis zu 3 Minuten dauern, bis die Metriken im Portal angezeigt werden. Metriken können in Diagrammen oder einem Raster Ihrer Wahl angezeigt und über das Portal, PowerShell, die CLI und die API aufgerufen werden. Weitere Informationen finden Sie unter  [Überblick über Metriken in Azure Monitor](../../azure-monitor/essentials/data-platform-metrics.md).  

Die Standardmetriken sind kostenlos verfügbar. Sie können kostenpflichtig zusätzliche Metriken aktivieren. 

Sie können Warnungen für jede Metrik konfigurieren, z. B. einen Schwellenwert für „4XXErrorRate“ oder „5XXErrorRate“. Wenn die Fehlerrate den Schwellenwert überschreitet, wird konfigurationsgemäß eine Warnung ausgelöst. Weitere Informationen finden Sie unter [Erstellen, Anzeigen und Verwalten von Metrikwarnungen mit Azure Monitor](../../azure-monitor/alerts/alerts-metric.md). 

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="metrics-supported-in-azure-front-door-standardpremium"></a>Unterstützte Metriken in Azure Front Door Standard/Premium

| Metriken  | BESCHREIBUNG | Dimensionen |
| ------------- | ------------- | ------------- |
| Bytetrefferquote | Der Prozentsatz der ausgehenden Daten aus dem AFD-Cache, berechnet im Vergleich zu den ausgehenden Gesamtdaten </br> **Bytetrefferquote** = (vom Edge ausgehende Daten – vom Ursprung ausgehende Daten)/vom Edge ausgehende Daten </br> **Von der Berechnung der Bytetrefferquote ausgeschlossene Szenarien**:</br> 1. Sie konfigurieren entweder über die Regel-Engine oder das Verhalten beim Zwischenspeichern von Abfragezeichenfolgen explizit keinen Cache. </br> 2. Sie konfigurieren die Cache-Control-Direktive explizit mit „no-store“ oder „private cache“. </br>3. Die Bytetrefferquote kann niedrig sein, wenn der größte Teil des Datenverkehrs an den Ursprung weitergeleitet wird, anstatt über die Zwischenspeicherung basierend auf Ihren Konfigurationen oder Szenarien bereitgestellt zu werden. | Endpunkt |
| RequestCount | Die Anzahl der in CDN bereitgestellten Clientanforderungen | Endpunkt, Land des Clients, Region des Clients, HTTP-Status, HTTP-Statusgruppe |
| ResponseSize | Die Anzahl der in AFD bereitgestellten Clientanforderungen |Endpunkt, Land des Clients, Region des Clients, HTTP-Status, HTTP-Statusgruppe |
| TotalLatency | Die Gesamtzeit zwischen dem Empfang der Clientanforderung in CDN **und dem letzten von CDN an den Client gesendeten Antwortbyte** |Endpunkt, Land des Clients, Region des Clients, HTTP-Status, HTTP-Statusgruppe |
| RequestSize | Die Anzahl der von Clients als Anforderungen an AFD gesendeten Bytes. | Endpunkt, Land des Clients, Region des Clients, HTTP-Status, HTTP-Statusgruppe |
| 4XX % ErrorRate | Der Prozentsatz aller Clientanforderungen, deren Antwortstatuscode 4XX lautet | Endpunkt, Land des Clients, Region des Clients |
| 5XX % ErrorRate | Der Prozentsatz aller Clientanforderungen, deren Antwortstatuscode 5XX lautet | Endpunkt, Land des Clients, Region des Clients |
| OriginRequestCount  | Die Anzahl von Anforderungen, die von AFD zum Ursprung gesendet wurden | Endpunkt, Ursprung, HTTP-Status, HTTP-Statusgruppe |
| OriginLatency | Die Zeitspanne zwischen dem Senden der Anforderung durch AFD an das Back-End und dem Empfang des letzten Antwortbytes durch AFD vom Back-End | Endpunkt, Ursprung |
| OriginHealth% | Der Prozentsatz der erfolgreichen Integritätstests zwischen AFD und dem Ursprung| Ursprung, Ursprungsgruppe |
| Anzahl von WAF-Anforderungen | Übereinstimmende WAF-Anforderung | Aktion, Regelname, Richtlinienname |

## <a name="access-metrics-in-azure-portal"></a>Zugreifen auf Metriken im Azure-Portal

1. Wählen Sie im Menü des Azure-Portals **Alle Ressourcen** >>  **\<your-AFD Standard/Premium (Preview) -profile>** aus.

2. Wählen Sie unter **Überwachung** die Option **Metriken** aus:

3. Wählen Sie in **Metriken** die hinzuzufügende Metrik aus:

   :::image type="content" source="../media/how-to-monitoring-metrics/front-door-metrics-1.png" alt-text="Screenshot der Seite „Metriken“" lightbox="../media/how-to-monitoring-metrics/front-door-metrics-1-expanded.png":::

4. Wählen Sie **Filter hinzufügen** aus, um einen Filter hinzuzufügen:

    :::image type="content" source="../media/how-to-monitoring-metrics/front-door-metrics-2.png" alt-text="Screenshot: Hinzufügen von Filtern zu Metriken" lightbox="../media/how-to-monitoring-metrics/front-door-metrics-2-expanded.png":::
    
5. Wählen Sie **Teilung anwenden** aus, um Daten nach verschiedenen Dimensionen anzuzeigen:

   :::image type="content" source="../media/how-to-monitoring-metrics/front-door-metrics-4.png" alt-text="Screenshot: Hinzufügen von Dimensionen zu Metriken" lightbox="../media/how-to-monitoring-metrics/front-door-metrics-4-expanded.png":::

6. Wählen Sie **Neues Diagramm** aus, um ein neues Diagramm hinzuzufügen:

## <a name="configure-alerts-in-azure-portal"></a>Konfigurieren von Warnungen im Azure-Portal

1. Richten Sie Warnungen für Azure Front Door Standard/Premium (Vorschau) ein, indem Sie **Überwachung** >> **Warnungen** auswählen.

1. Wählen Sie **Neue Warnungsregel** für die im Abschnitt „Metriken“ aufgeführten Metriken aus.

Die Warnung wird basierend auf Azure Monitor abgerechnet. Weitere Informationen zu Warnungen finden Sie unter [Azure Monitor-Warnungen](../../azure-monitor/alerts/alerts-overview.md).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu [Berichten in Azure Front Door Standard/Premium (Vorschau)](how-to-reports.md)
- Informationen zur [Protokollierung in Azure Front Door Standard/Premium (Vorschau)](how-to-logs.md)