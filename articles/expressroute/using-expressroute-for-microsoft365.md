---
title: Verwenden von ExpressRoute für Microsoft 365-Dienste | Microsoft-Dokumentation
description: In diesem Dokument wird die Verwendung einer ExpressRoute-Leitung für Microsoft 365-SaaS-Dienste ausführlich erläutert.
documentationcenter: na
services: expressroute
author: rambk
manager: tracsman
ms.service: expressroute
ms.topic: article
ms.workload: infrastructure-services
ms.date: 4/29/2021
ms.author: rambala
ms.openlocfilehash: f7ccad395dd473e2520bbcaefac8e34be6fb2443
ms.sourcegitcommit: 52491b361b1cd51c4785c91e6f4acb2f3c76f0d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108322292"
---
# <a name="using-expressroute-for-routing-microsoft-365-traffic"></a>Verwenden von ExpressRoute für das Routing von Microsoft 365-Datenverkehr

Eine ExpressRoute-Leitung bietet private Konnektivität mit dem Microsoft-Backbonenetzwerk. 
* Sie umfasst *privates Peering* zum Herstellen einer Verbindung mit den privaten Endpunkten Ihrer IaaS-Bereitstellung in Azure-Regionen. 
* Außerdem bietet sie *Microsoft-Peering* zum Herstellen einer Verbindung mit öffentlichen Endpunkten von IaaS-, PaaS- und SaaS-Diensten im Microsoft-Netzwerk. 

Weitere Informationen zu ExpressRoute finden Sie im Artikel [Einführung in ExpressRoute][ExR-Intro].


Es herrscht eine gewisse Unklarheit darüber, ob ExpressRoute für das Routing von Microsoft 365-SaaS-Datenverkehr verwendet werden kann. 

* Was dafür spricht ist, dass ExpressRoute Microsoft-Peering ermöglicht, mit dem die meisten öffentlichen Endpunkte im Microsoft-Netzwerk erreicht werden können. Mithilfe eines *Routenfilters* können Sie in der Tat Microsoft 365-Dienstpräfixe auswählen, die über das Microsoft-Peering für Ihr lokales Netzwerk angekündigt werden müssen. Durch diese Routenankündigung kann Microsoft 365-Dienstdatenverkehr über die ExpressRoute-Leitung geleitet werden. 
* Dagegen spricht, dass Microsoft 365 ein verteilter Dienst ist. Er ist so konzipiert, dass Kunden auf der ganzen Welt über das Internet eine Verbindung mit dem Dienst herstellen können. Daher empfiehlt es sich, ExpressRoute nicht für Microsoft 365 zu verwenden.

Dieser Artikel hat folgende Zielsetzungen: 
* die Argumente technisch zu begründen und 
* ausführlich zu erläutern, wann ExpressRoute für das Routing von Microsoft 365-Datenverkehr verwendet werden kann und wann nicht.

## <a name="network-requirements-of-microsoft-365-traffic"></a>Netzwerkanforderungen für Microsoft 365-Datenverkehr
Der Microsoft 365-Dienst umfasst häufig Echtzeitdatenverkehr, z. B. Sprach- und Videoanrufe, Onlinebesprechungen und Echtzeitzusammenarbeit. Für diesen Echtzeitdatenverkehr gelten strikte Anforderungen an die Netzwerkleistung in Bezug auf Latenz und Jitter. Innerhalb bestimmter Grenzen der Netzwerklatenz kann der Jitter effektiv mithilfe von Puffern auf dem Clientgerät verarbeitet werden. Die Netzwerklatenz ergibt sich aus der physischen Entfernung des Datenverkehrs, der Verbindungsbandbreite und der Netzwerkverarbeitungslatenz. 

## <a name="network-optimization-features-of-microsoft-365"></a>Netzwerkoptimierungsfunktionen von Microsoft 365 

Microsoft ist bestrebt, die Netzwerkleistung aller Cloudanwendungen hinsichtlich der Architektur sowie der Funktionen zu optimieren. Zunächst besitzt Microsoft eines der größten globalen Netzwerke, das in Bezug auf das Kernziel optimiert ist, beste Netzwerkleistung zu bieten. Das Microsoft-Netzwerk ist softwaredefiniert und ein sogenanntes „Cold Potato“-Netzwerk. „Cold Potato“-Netzwerk in dem Sinne, dass ein- und ausgehender Datenverkehr so nah wie möglich am Clientgerät oder Kundennetzwerk erfolgt. Das Microsoft-Netzwerk ist außerdem hochredundant und hochverfügbar. Weitere Informationen zur Architekturoptimierung finden Sie unter [How Microsoft builds its fast and reliable global network][MGN] (Wie Microsoft sein schnelles und zuverlässiges globales Netzwerk aufbaut).

Um die strikten Anforderungen an die Netzwerklatenz zu erfüllen, wird die Routenlänge in Microsoft 365 wie folgt verkürzt:
* Endbenutzerverbindungen werden dynamisch an den nächstgelegenen Microsoft 365-Einstiegspunkt weitergeleitet und 
* vom Einstiegspunkt effizient innerhalb des globalen Microsoft-Netzwerks an das nächstgelegene (und autorisierte) Microsoft 365-Rechenzentrum weitergeleitet.

Die Microsoft 365-Einstiegspunkte werden über Azure Front Door (AFD) verarbeitet. AFD ist ein weit verteilter Dienst, der im globalen Microsoft-Umkreisnetzwerk arbeitet und die Erstellung schneller, sicherer und hochgradig skalierbarer SaaS-Anwendungen ermöglicht. Weitere Informationen zur Beschleunigung der Leistung von Webanwendungen durch AFD finden Sie unter [Was ist Azure Front Door?][AFD]. Bei der Auswahl des nächstgelegenen Microsoft 365-Rechenzentrums berücksichtigt Microsoft die Bestimmungen zur Datenhoheit innerhalb der geografischen Region.

## <a name="what-is-geo-pinning-connections"></a>Was sind Verbindungen mit geografischer Fixierung?

Wenn zwischen einem Client und einem Server erzwungen wird, dass der Datenverkehr über bestimmte Netzwerkgeräte erfolgt, die sich an einem geografischen Standort befinden, wird dies als geografische Fixierung der Netzwerkverbindungen bezeichnet. Bei der herkömmlichen Netzwerkarchitektur mit dem zugrunde liegenden Entwurfsprinzip, dass sich die Clients und Server an einem statischen Standort befinden, sind die Verbindungen üblicherweise geografisch fixiert.
Wenn Sie z. B. erzwingen, dass die Internetverbindungen Ihres Unternehmens über das Unternehmensnetzwerk erfolgen und von einem zentralen Standort ausgehen (in der Regel über eine Gruppe von Proxyservern oder Firewalls), fixieren Sie die Internetverbindungen geografisch.  

Ähnlich verhält es sich bei der Architektur von SaaS-Anwendungen: Wenn Sie hier erzwingen, dass der Datenverkehr über ein zwischengeschaltetes Rechenzentrum (z. B. für Cloudsicherheit) in einer Region oder über ein oder mehrere zwischengeschaltete Netzwerkgeräte (z. B. ExpressRoute) an einem bestimmten Standort weitergeleitet wird, fixieren Sie die SaaS-Verbindungen geografisch.

## <a name="when-not-to-use-expressroute-for-microsoft-365"></a>Wann sollte ExpressRoute nicht für Microsoft 365 verwendet werden?

Aufgrund der Möglichkeit, die Routenlänge dynamisch zu verkürzen und je nach Standort der Clients das nächstgelegene Serverrechenzentrum dynamisch auszuwählen, gilt Microsoft 365 als für das Internet konzipiert. Außerdem wird bestimmter Microsoft 365-Datenverkehr nur über das Internet geleitet.
Wenn die SaaS-Clients weit über eine Region oder global verteilt sind und Sie die Verbindungen für einen bestimmten Standort geografisch fixieren, wird erzwungen, dass die Clients weiter vom geografisch fixierten Standort entfernt sind, was eine höhere Netzwerklatenz zur Folge hat. Eine höhere Netzwerklatenz führt zu einer nicht optimalen Netzwerkleistung und schlechter Anwendungsleistung.

Daher sollten Sie in Szenarien mit weit verteilten SaaS-Clients oder sehr mobilen Clients die Verbindungen auf keinen Fall geografisch fixieren. Dazu zählt auch die Erzwingung des Datenverkehrs über eine ExpressRoute-Leitung an einem bestimmten Peeringstandort.


## <a name="when-to-use-expressroute-for-microsoft-365"></a>Wann sollte ExpressRoute für Microsoft 365 verwendet werden?

Im Folgenden sind einige Gründe aufgeführt, die für die Verwendung von ExpressRoute für das Routing von Microsoft 365-Datenverkehr sprechen:
* Die SaaS-Clients sind an einem geografischen Standort konzentriert. Die beste Möglichkeit, eine Verbindung mit dem globalen Microsoft-Netzwerk herzustellen, sind ExpressRoute-Leitungen.
* Die SaaS-Clients sind an mehreren globalen Standorten konzentriert. Die einzelnen Standorte verfügen jeweils über eigene ExpressRoute-Leitungen, die eine optimale Konnektivität mit dem globalen Microsoft-Netzwerk bieten.
* Sie sind per Gesetz verpflichtet, cloudgebundenen Datenverkehr über private Verbindungen weiterzuleiten.
* Sie müssen den gesamten SaaS-Datenverkehr an einen geografisch fixierten zentralen Standort (privates oder öffentliches Rechenzentrum) weiterleiten. Die optimale Möglichkeit für eine Verbindung zwischen dem zentralen Standort und dem globalen Microsoft-Netzwerk herzustellen stellt ExpressRoute dar.
* Für einige Ihrer statischen SaaS-Clients bietet nur ExpressRoute eine optimale Konnektivität. Für die anderen Clients wird dagegen das Internet verwendet.

Bei Verwendung von ExpressRoute können Sie den Routenfilter anwenden, der dem Microsoft-Peering von ExpressRoute zugeordnet ist, sodass nur eine Teilmenge von Microsoft 365-Diensten und/oder Azure-PaaS-Diensten über die ExpressRoute-Leitung geleitet wird. Weitere Informationen finden Sie unter [Tutorial: Konfigurieren von Routenfiltern für das Microsoft-Peering][ExRRF].

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Ablauf von Microsoft Teams-Anrufen und zum Optimieren der Netzwerkkonnektivität in verschiedenen Szenarien, einschließlich der Verwendung von ExpressRoute für optimale Ergebnisse, finden Sie unter [Flows von Microsoft Teams-Anrufen][Teams].
* Informationen zum Durchführen von Tests zum Ermitteln von Microsoft 365-Konnektivitätsproblemen an einzelnen Zweigstandorten finden Sie unter[Microsoft 365 network connectivity test][Microsoft 365-Test] (Netzwerkkonnektivitätstest für Microsoft 365).
* Informationen zum Einrichten von Baselines und eines Leistungsverlaufs zum Erkennen von sich abzeichnenden Problemen bei der Microsoft 365-Leistung finden Sie unter [Optimieren der Office 365-Leistung mit Baselines und Leistungsverlauf][Microsoft 365perf].

<!--Link References-->
[ExR-Intro]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[MGN]: https://azure.microsoft.com/blog/how-microsoft-builds-its-fast-and-reliable-global-network/
[AFD]: https://docs.microsoft.com/azure/frontdoor/front-door-overview
[ExRRF]: https://docs.microsoft.com/azure/expressroute/how-to-routefilter-portal
[Teams]: https://docs.microsoft.com/microsoftteams/microsoft-teams-online-call-flows
[Microsoft 365-Test]: https://connectivity.office.com/
[Microsoft 365perf]: https://docs.microsoft.com/microsoft-365/enterprise/performance-tuning-using-baselines-and-history?view=o365-worldwide


