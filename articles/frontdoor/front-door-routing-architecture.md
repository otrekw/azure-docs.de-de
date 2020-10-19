---
title: Azure Front Door – Routingarchitektur | Microsoft-Dokumentation
description: In diesem Artikel wird der Aspekt der globalen Ansicht der Architektur von Azure Front Door Service erläutert.
services: front-door
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/28/2020
ms.author: duau
ms.openlocfilehash: 948cf3c65dfdc912f2f807dfac34076985f1bc89
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91449197"
---
# <a name="routing-architecture-overview"></a>Übersicht über die Routingarchitektur

Wenn Azure Front-Door Ihre Clientanforderungen empfängt, wird eine von zwei Aktionen ausgeführt. Wenn Zwischenspeichern aktiviert ist, werden diese beantwortet, oder sie werden an das entsprechende Anwendungs-Back-End als Reverseproxy weitergeleitet.

## <a name="selecting-the-front-door-environment-for-traffic-routing-anycast"></a><a name = "anycast"></a>Auswählen der Azure Front Door Service-Umgebung für das Datenverkehrsrouting (Anycast)

Beim Routing von Datenverkehr in die Azure Front Door Service-Umgebungen wird [Anycast](https://en.wikipedia.org/wiki/Anycast) sowohl für DNS-Datenverkehr (Domain Name System) als auch HTTP-Datenverkehr (Hypertext Transfer-Protokoll) verwendet. Diese Architektur bietet bessere Roundtripzeiten für Endbenutzer durch Maximierung der Vorteile von Split TCP. Azure Front Door Service unterteilt die Umgebungen in Primär- und Fallback-„Ringe“. Der äußere Ring umfasst Umgebungen, die sich näher bei Benutzern befinden und daher niedrigere Wartezeiten bieten.  Der innere Ring enthält Umgebungen, die im Fall eines Problems das Failover für die Umgebung im äußeren Ring behandeln können. Der äußere Ring ist das bevorzugte Ziel für den gesamten Datenverkehr, der innere Ring verarbeitet den Datenverkehrsüberlauf vom äußeren Ring. Jedem Front-End-Host oder jeder Domäne, die von Front Door bereitgestellt wird, wird eine primäre VIP (Virtual Internet Protocol-Adresse) zugewiesen, die von Umgebungen im inneren und äußeren Ring angekündigt wird. Eine Fallback-VIP wird nur von Umgebungen im inneren Ring angekündigt. 

Diese Architektur stellt sicher, dass Anforderungen von Ihren Endbenutzern immer die nächstgelegene Front Door-Umgebung erreichen. Selbst wenn die bevorzugte Front Door-Umgebung fehlerhaft ist, wird der gesamte Datenverkehr automatisch in die nächstgelegene Umgebung verschoben.

## <a name="connecting-to-front-door-environment-split-tcp"></a><a name = "splittcp"></a>Herstellen einer Verbindung mit der Azure Front Door Service-Umgebung (Split TCP)

[Split TCP](https://en.wikipedia.org/wiki/Performance-enhancing_proxy) ist ein Verfahren, bei dem eine Verbindung, die eine hohe Roundtripzeit haben würde, in kleine Teile aufgeteilt wird, um die Wartezeiten und TCP-Probleme zu reduzieren. Bei Front Door-Umgebungen, die näher an Endbenutzern liegen, werden TCP-Verbindungen innerhalb der Front Door-Umgebung beendet. Eine TCP-Verbindung, die eine große Roundtripzeit (RTT) zum Anwendungs-Back-End aufweist, wird in zwei separate Verbindungen aufgeteilt. Aufgrund der „kurzen Verbindung“ zwischen dem Endbenutzer und der Azure Front Door-Umgebung wird die Verbindung nicht über drei lange Roundtrips hergestellt, sondern über drei kurze Roundtrips, wodurch die Latenz reduziert wird. Die „lange Verbindung“ zwischen der Front Door-Umgebung und dem Back-End kann vorab eingerichtet und dann bei weiteren Endbenutzeranforderungen wiederverwendet werden, wodurch sich die Verbindungszeit verkürzt. Beim Herstellen einer SSL-/TLS-Verbindung (Transport Layer Security) vervielfacht sich der Effekt von Split TCP aufgrund der größeren Anzahl von Roundtrips zum Sichern einer Verbindung.

## <a name="processing-request-to-match-a-routing-rule"></a>Verarbeiten der Anforderung zum Zuordnen einer Routingregel
Wenn eine Anforderung nach dem Herstellen einer Verbindung und Durchführen eines TLS-Handshakes in eine Front Door-Umgebung gelangt, ist die Zuordnung einer Routingregel der erste Schritt. Bei dieser Zuordnung wird auf Grundlage aller Konfigurationen in Front Door bestimmt, welcher spezifischen Routingregel die Anforderung entspricht. Erfahren Sie mehr darüber, wie Azure Front Door Service die [Routenzuordnung](front-door-route-matching.md) vornimmt.

## <a name="identifying-available-backends-in-the-backend-pool-for-the-routing-rule"></a>Identifizieren der verfügbaren Back-Ends im Back-End-Pool für die Routingregel
Wenn Front Door für eine eingehende Anforderung eine übereinstimmende Routingregel ermittelt hat, besteht der nächste Schritt darin, den Integritätsteststatus für den Back-End-Pool der zugeordneten Route abzurufen, wenn keine Zwischenspeicherung aktiviert ist. Erfahren Sie, wie Azure Front Door Service die Back-End-Integrität mithilfe von [Integritätstests](front-door-health-probes.md) überwacht.

## <a name="forwarding-the-request-to-your-application-backend"></a>Weiterleiten der Anforderung an das Anwendungs-Back-End
Schließlich wird die Benutzeranforderung (sofern keine Zwischenspeicherung konfiguriert ist) basierend auf Ihrer Konfiguration der [Routingmethode](front-door-routing-methods.md) zum „besten“ Back-End weitergeleitet.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Front Door-Instanz](quickstart-create-front-door.md).
