---
title: 'Azure Front Door: DDoS-Schutz'
description: Diese Seite enthält Informationen darüber, wie Azure Front Door Standard/Premium vor DDoS-Angriffen schützt.
services: frontdoor
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.topic: conceptual
ms.date: 02/18/2021
ms.author: jodowns
ms.openlocfilehash: 9c67944717888439c0d6bd84e1615f51dd91fcac
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101098061"
---
# <a name="ddos-protection-on-azure-front-door-standardpremium-preview"></a>DDoS-Schutz in Azure Front Door Standard/Premium (Vorschau)

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

Azure Front Door bietet mehrere Features und Merkmale, die helfen können, DDoS-Angriffe (Denial of Service) zu verhindern. Diese Features können verhindern, dass Angreifer auf Ihre Anwendung zugreifen und deren Verfügbarkeit und Leistung beeinträchtigen.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="integration-with-azure-ddos-protection-basic"></a>Integration in Azure DDoS Protection Basic

Front Door wird durch Azure DDoS Protection Basic geschützt. Das Feature ist standardmäßig und ohne zusätzliche Kosten in die Front Door-Plattform integriert. Die vollständige Skalierung und Kapazität des global bereitgestellten Front Door-Netzwerks bietet über AlwaysOn-Datenverkehrsüberwachung und Risikominderung in Echtzeit Schutz vor häufigen Vermittlungsschichtangriffen. Basic DDos-Schutz schützt auch vor den gängigsten, häufig auftretenden DNS-Abfrage-Floods (Schicht 7) und volumetrischen Angriffen (Schicht 3 und 4), die auf öffentliche Endpunkte abzielen. Dieser Dienst hat sich außerdem in der Praxis beim Schutz von Unternehmens- und Endkundendiensten von Microsoft vor großangelegten Angriffen bewährt. Weitere Informationen finden Sie unter [Azure DDoS Protection](../../security/fundamentals/ddos-best-practices.md).

## <a name="protocol-blocking"></a>Protokollblockierung

Front Door akzeptiert nur Datenverkehr über die HTTP- und HTTPS-Protokolle und verarbeitet nur gültige Anforderungen mit einem bekannten `Host`-Header. Dieses Verhalten trägt dazu bei, einige gängige DDoS-Angriffstypen abzuschwächen, darunter volumetrische Angriffe, die auf verschiedene Protokolle und Ports verteilt werden, DNS-Verstärkungsangriffe und TCP-Poisoningangriffe.

## <a name="capacity-absorption"></a>Kapazitätsabsorption

Front Door ist ein hochgradig skalierter, global verteilter Dienst. Wir haben viele Kunden, einschließlich der großen Cloudprodukte von Microsoft, die pro Sekunde Hunderttausende von Anforderungen erhalten. Front Door befindet sich am Rand des Azure-Netzwerks und absorbiert großvolumige Angriffe und isoliert sie geografisch. Dadurch kann verhindert werden, dass schädlicher Datenverkehr über den Rand des Azure-Netzwerks hinausgeht.

## <a name="caching"></a>Zwischenspeicherung

Die [Zwischenspeicherungsfunktionen von Front Door](concept-caching.md) können verwendet werden, um Back-Ends vor durch einen Angriff generierten großen Datenverkehrsvolumen zu schützen. Zwischengespeicherte Ressourcen werden von den Front Door-Edgeknoten zurückgegeben, sodass sie nicht an Ihr Back-End weitergeleitet werden. Sogar kurze Cacheablaufzeiten (Sekunden oder Minuten) bei dynamischen Antworten können die Auslastung von Back-End-Diensten erheblich verringern. Weitere Informationen zum Zwischenspeichern von Konzepten und Mustern finden Sie unter [Caching](/azure/architecture/best-practices/caching) und [Cachefremdes Muster](/azure/architecture/patterns/cache-aside).

## <a name="web-application-firewall-waf"></a>Web Application Firewall (WAF)

Die [Azure Web Application Firewall für Azure Front Door](../../web-application-firewall/afds/afds-overview.md) kann verwendet werden, um viele unterschiedliche Arten von Angriffen abzuschwächen:

* Die Verwendung des verwalteten Regelsatzes bietet Schutz gegen viele häufige Angriffe.
* Datenverkehr von außerhalb einer definierten geografischen Region oder innerhalb einer definierten Region kann blockiert oder an eine statische Webseite umgeleitet werden. Weitere Informationen finden Sie unter [Was ist die Geofilterung in einer Domäne für Azure Front Door Service?](../../web-application-firewall/afds/waf-front-door-geo-filtering.md).
* IP-Adressen und -Bereiche, die Sie als bösartig identifizieren, können blockiert werden.
* Die Ratenbegrenzung kann angewendet werden, um zu verhindern, dass IP-Adressen ihren Dienst zu häufig aufrufen.
* Sie können [benutzerdefinierte WAF-Regeln](../../web-application-firewall/afds/waf-front-door-custom-rules.md) erstellen, um HTTP- oder HTTPS-Angriffe mit bekannten Signaturen automatisch zu blockieren sowie eine Ratenbegrenzung anzuwenden.

## <a name="for-further-protection"></a>Für weiteren Schutz

Wenn Sie weiteren Schutz benötigen, können Sie [Azure DDoS Protection Standard](../../security/fundamentals/ddos-best-practices.md#ddos-protection-standard) für das VNET aktivieren, in dem Ihre Back-Ends bereitgestellt werden. DDoS Protection Standard-Kunden erhalten weitere Vorteile, u. a.:

* Kostenschutz
* SLA-Garantie
* Zugang zu Experten im DDoS Rapid Response-Team, um bei einem Angriff unverzüglich Hilfe zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das [Erstellen einer Instanz von Front Door Standard/Premium](create-front-door-portal.md).
