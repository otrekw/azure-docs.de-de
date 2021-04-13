---
title: Azure Front Door Standard/Premium | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht zu Azure Front Door Standard/Premium.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: overview
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: duau
ms.openlocfilehash: 5c3ae5c7b1c45d170548f6fa00481094117e1737
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105937361"
---
# <a name="what-is-azure-front-door-standardpremium-preview"></a>Was ist Azure Front Door Standard/Premium (Vorschau)?

> [!IMPORTANT]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Lesen Sie die [Azure Front Door-Dokumente](../front-door-overview.md).

Azure-Front-Door Standard/Premium ist ein schnelles, zuverlässiges und sicheres, modernes Cloud-CDN, das das Microsoft Global Edge Network nutzt und sich in intelligentem Bedrohungsschutz integriert. Es kombiniert die Funktionen von Azure-Front-Door, Azure Content Delivery Network (CDN) Standard und Azure Web Application Firewall (WAF) zu einer einzigen sicheren Cloud-CDN-Plattform.

Mit Azure-Front-Door-Standard/Premium können Sie Ihre globalen Consumer-und Unternehmensanwendungen in sichere und hochleistungsfähige, moderne Anwendungen mit Inhalten transformieren, die eine globale Zielgruppe am Netzwerkrand in der Nähe des Benutzers erreichen. Außerdem ermöglicht Sie der Anwendung ein horizontales Hochskalieren ohne Aufwärmzeit, während gleichzeitig vom globalen http-Lastenausgleich mit sofortigem Failover profitiert wird.

   :::image type="content" source="../media/overview/front-door-overview.png" alt-text="Azure Front Door Standard/Premium Architektur" lightbox="../media/overview/front-door-overview-expanded.png":::

Azure Front Door Standard/Premium arbeitet in Schicht 7 (HTTP/HTTPS-Schicht) unter Verwendung des Anycast-Protokolls mit Split TCP sowie dem globalen Netzwerk von Microsoft zur Verbesserung der globalen Konnektivität. Basierend auf Ihrer Routingmethode können Sie sicherstellen, dass Azure Front Door Ihre Clientanforderungen an den schnellsten und am besten verfügbaren Anwendungsursprung weiterleitet. Ein Anwendungsursprung ist ein Dienst mit Internetzugriff, der innerhalb oder außerhalb von Azure gehostet wird. Front Door Standard/Premium bietet eine Reihe von Datenverkehrsrouting-Methoden und Ursprungsüberwachungsoptionen, die verschiedene Anwendungsanforderungen und automatische Failoverszenarien erfüllen. Ähnlich wie Traffic Manager zeichnet sich Front Door durch eine geringe Fehleranfälligkeit aus, selbst wenn es zum Ausfall einer ganzen Azure-Region kommt.

Azure Front Door schützt Ihre Anwendung auch an den Rändern mit integriertem Web Application Firewall-Schutz, Bot-Schutz und integriertem DDoS-Schutz auf Schicht 3 und 4. Außerdem werden Ihre privaten Back-Ends mit dem Private Link-Dienst gesichert. Azure-Front-Door bietet Ihnen die Best-in-Practice-Sicherheit von Microsoft auf globaler Ebene.  

>[!NOTE]
> Azure stellt eine Sammlung aus vollständig verwalteten Lastenausgleichslösungen für Ihre Szenarios bereit.
>
> * Wenn Sie DNS-basiertes globales Routing durchführen möchten und **keine** Anforderungen zur Beendigung der TLS-Protokollierung (Transport Layer Security, „SSL Offloading“) oder zur Verarbeitung der Anwendungsschicht pro HTTP/HTTPS-Anforderung vorliegen, lesen Sie den Artikel zu [Traffic Manager](../../traffic-manager/traffic-manager-overview.md).
> * Wenn Sie Lastenausgleich zwischen den Servern in einer Region auf Anwendungsebene durchführen möchten, lesen Sie den Artikel zu [Application Gateway](../../application-gateway/overview.md)
> * Um Lastenausgleich auf Netzwerkebene auszuführen, lesen Sie den Artikel zu [Load Balancer](../../load-balancer/load-balancer-overview.md).
>
> Für Ihre End-to-End-Szenarien kann es vorteilhaft sein, diese Lösungen nach Bedarf zu kombinieren.
> Einen Vergleich der Azure-Lastenausgleichsoptionen finden Sie unter [Übersicht über Lastenausgleichsoptionen in Azure](/azure/architecture/guide/technology-choices/load-balancing-overview).

> [!IMPORTANT]
> Azure Front Door Standard/Premium befindet sich derzeit in der Public Preview.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="why-use-azure-front-door-standardpremium-preview"></a>Was spricht für Azure Front Door Standard/Premium (Vorschauversion)?

Azure-Front-Door Standard/Premium bietet eine einheitliche Plattform, die sowohl dynamische als auch statische Beschleunigung mit integrierter, sofort einsatzbereiter Sicherheitsintegration und einem einfachen und berechenbaren Preismodell bietet. Mit Front Door können Sie das globale Routing Ihrer Anwendung definieren, verwalten und überwachen.

Schlüsselfunktionen von Azure Front Door Standard/Premium (Preview):

- VorschauversionBeschleunigte Anwendungsleistung durch Verwenden des auf **[Split TCP](../front-door-routing-architecture.md#splittcp)** basierenden Anycast-Protokolls.

- Intelligente **[Integritätstest](concept-health-probes.md)** -Überwachung und Lastenausgleich zwischen den **[Ursprüngen](concept-origin.md)** .

- Definieren Sie eine eigene **[benutzerdefinierte Domäne](how-to-add-custom-domain.md)** mit flexibler Domänenvalidierung.

- Anwendungssicherheit mit integrierter **[Web Application Firewall (WAF)](../../web-application-firewall/afds/afds-overview.md)** .

- SSL-Auslagerung und integrierte **[Zertifikatverwaltung](how-to-configure-https-custom-domain.md)** .

- Sichern Sie Ihre Ursprünge mit **[Private Link](concept-private-link.md)** .  

- Anpassbares Routing von Datenverkehr und Optimierungen per **[Regelsatz](concept-rule-set.md)** .

- **[Integrierte Berichte](how-to-reports.md)** mit All-in-One-Dashboards für Front Door und Sicherheitsmuster.

- **[Echtzeitüberwachung](how-to-monitor-metrics.md)** und Warnungen, die in das Azure-Monitoring integriert werden.

- **[Protokollierung](how-to-logs.md)** aller Front-Door-Anforderungen und fehlerhaften Integritätstests.

- Native Unterstützung von End-to-End-IPv6-Konnektivität und HTTP/2-Protokoll.

## <a name="pricing"></a>Preise

Azure-Front-Door Standard/Premium verfügt über zwei SKUs: Standard und Premium. Siehe [Vergleich der Dienstebenen](tier-comparison.md). Preisinformationen finden Sie unter [Front Door – Preise](https://azure.microsoft.com/pricing/details/frontdoor/). 

## <a name="whats-new"></a>Neuigkeiten

Abonnieren Sie den RSS-Feed, und zeigen Sie die neuesten Azure Front Door-Featureupdates auf der Seite [Azure-Updates](https://azure.microsoft.com/updates/?category=networking&query=Azure%20Front%20Door) an.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Erstellen einer Front Door-Instanz](create-front-door-portal.md).
