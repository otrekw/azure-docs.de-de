---
title: Azure Load Balancer als Proxy für ausgehenden Datenverkehr
description: Beschreibt, wie Azure Load Balancer als Proxy für ausgehende Internetverbindungen verwendet wird.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: conceptual
ms.custom: contperfq1
ms.date: 10/13/2020
ms.author: allensu
ms.openlocfilehash: 185bb47677e978a3098f39024995da6399f90658
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2020
ms.locfileid: "93241768"
---
# <a name="outbound-proxy-azure-load-balancer"></a>Azure Load Balancer als Proxy für ausgehenden Datenverkehr

Eine Azure Load Balancer-Instanz kann als Proxy für ausgehende Internetverbindungen verwendet werden. Der Lastenausgleich stellt die ausgehende Konnektivität für die Back-End-Instanzen bereit. 

Diese Konfiguration verwendet **SNAT (Source Network Address Translation, Quell-Netzwerkadressenübersetzung)** . Die IP-Adresse des Back-Ends wird von SNAT in die öffentliche IP-Adresse Ihrer Load Balancer-Instanz umgeschrieben. 

SNAT ermöglicht **IP-Maskierung** der Back-End-Instanz. Diese Maskierung verhindert, dass externe Quellen eine direkte Adresse für die Back-End-Instanzen abrufen können. 

Wenn Sie eine IP-Adresse zwischen Back-End-Instanzen freigeben, werden die Kosten für statische öffentliche IPs reduziert, und es werden Szenarien wie das Vereinfachen von IP-Zulassungslisten mit Datenverkehr von bekannten öffentlichen IP-Adressen unterstützt. 

## <a name="sharing-ports-across-resources"></a><a name ="snat"></a> Ressourcenübergreifende gemeinsame Nutzung von Ports

Wenn die Back-End-Ressourcen eines Load Balancers keine öffentlichen IP-Adressen auf Instanzebene (ILPIP) aufweisen, richten sie die ausgehende Konnektivität über die Front-End-IP-Adresse des öffentlichen Load Balancers ein.

Ports werden verwendet, um eindeutige Bezeichner zu generieren, mit denen unterschiedliche Datenflüsse verwaltet werden. Das Internet verwendet ein 5-Tupel, um diesen Unterschied zu gewährleisten.

Das 5-Tupel besteht aus den folgenden Elementen:

* Ziel-IP
* Zielport
* Quell-IP
* Quellport und Protokoll, um diesen Unterschied zu gewährleisten.

Wenn ein Port für eingehende Verbindungen verwendet wird, verfügt er über einen **Listener** für eingehende Verbindungsanforderungen an diesem Port und kann nicht für ausgehende Verbindungen verwendet werden. 

Um eine ausgehende Verbindung herzustellen, muss ein **kurzlebiger Port** verwendet werden, um dem Ziel einen Port bereitzustellen, über den ein eindeutiger Datenverkehrsfluss kommuniziert und verwaltet werden kann. 

Jede IP-Adresse verfügt über 65.535 Ports. Die ersten 1024 Ports sind als **Systemports** reserviert. Jeder Port kann entweder für eingehende oder für ausgehende Verbindungen für TCP und UDP verwendet werden. 

Von den verbleibenden Ports stellt Azure 64.000 für die Verwendung als **kurzlebige Ports** bereit. Wenn eine IP-Adresse als Front-End-IP-Konfiguration hinzugefügt wird, können diese kurzlebigen Ports für SNAT verwendet werden.

Über Ausgangsregeln können diese SNAT-Ports auf Back-End-Instanzen verteilt werden, damit sie die öffentlichen IP-Adressen des Load Balancers für ausgehende Verbindungen freigeben können.

Das Netzwerk auf dem Host für jede Back-End-Instanz führt SNAT für Pakete aus, die Teil einer ausgehenden Verbindung sind. Der Host schreibt die IP-Quelladresse in eine der öffentlichen IP-Adressen neu. Der Host schreibt den Quellport jedes ausgehenden Pakets in einen der SNAT-Ports neu.

## <a name="exhausting-ports"></a><a name="scenarios"></a> Ausschöpfung von Ports

Für jede Verbindung mit derselben IP-Adresse und demselben Zielport wird ein SNAT-Port verwendet. Diese Verbindung verwaltet einen eindeutigen **Datenverkehrsfluss** von der Back-End-Instanz oder dem **Client** zu einem **Server**. Durch diesen Prozess erhält der Server einen eindeutigen Port, an den Datenverkehr adressiert werden soll. Ohne diesen Prozess weiß der Clientcomputer nicht, zu welchem Datenfluss ein Paket gehört.

Stellen Sie sich vor, dass mehrere Browser https://www.microsoft.com verwenden:

* IP-Zieladresse = 23.53.254.142
* Zielport = 443
* Protokoll = TCP

Ohne unterschiedliche Zielports für den Rückgabedatenverkehr (der SNAT-Port, der zum Herstellen der Verbindung verwendet wird) hat der Client keine Möglichkeit, ein Abfrageergebnis von einem anderen zu trennen.

Für ausgehende Verbindungen kann ein Burst auftreten. Einer Back-End-Instanz können nicht genügend Ports zugeordnet werden. Wenn **Wiederverwendung von Verbindungen** nicht aktiviert ist, wird das Risiko von SNAT- **Porterschöpfung** gesteigert.

Neue ausgehende Verbindungen mit einer IP-Zieladresse schlagen fehl, wenn Porterschöpfung auftritt. Verbindungen werden erfolgreich hergestellt, wenn ein Port verfügbar wird. Diese Erschöpfung tritt auf, wenn die 64.000 Ports von einer IP-Adresse dünn über viele Back-End-Instanzen verteilt sind. Anleitungen zur Entschärfung von SNAT-Porterschöpfung finden Sie im [Leitfaden zur Problembehandlung](https://docs.microsoft.com/azure/load-balancer/troubleshoot-outbound-connection).  

Bei TCP-Verbindungen verwendet der Load Balancer einen einzelnen SNAT-Port für jede Ziel-IP und jeden -Port. Diese Mehrfachnutzung ermöglicht mehrere Verbindungen mit derselben IP-Zieladresse und demselben SNAT-Port. Diese Mehrfachnutzung ist eingeschränkt, wenn die Verbindung nicht mit verschiedenen Zielports erfolgt.

Für UDP-Verbindungen verwendet der Load Balancer einen **porteingeschränkten Cone NAT** -Algorithmus, der einen SNAT-Port pro IP-Zieladresse für den Zielport verwendet. 

Ein Port wird für eine unbegrenzte Anzahl von Verbindungen wiederverwendet. Der Port wird nur wiederverwendet, wenn die IP-Zieladresse oder der -port unterschiedlich ist.

## <a name="port-allocation"></a><a name="preallocatedports"></a> Portzuordnung

Jede öffentliche IP-Adresse, die als Front-End-IP-Adresse Ihres Load Balancers zugewiesen ist, erhält 64.000 SNAT-Ports für die zugehörigen Back-End-Elemente. Ports können nicht für Back-End-Poolelemente freigegeben werden. Ein Bereich von SNAT-Ports kann nur von einer einzelnen Back-End-Instanz verwendet werden, um sicherzustellen, dass Rückgabepakete ordnungsgemäß weitergeleitet werden. 

Es wird empfohlen, eine explizite Ausgangsregel zum Konfigurieren der SNAT-Portzuordnung zu verwenden. Diese Regel maximiert die Anzahl der SNAT-Ports, die jeder Back-End-Instanz für ausgehende Verbindungen zur Verfügung stehen. 

Sollten Sie automatische Zuordnung von ausgehendem SNAT durch eine Lastausgleichsregel verwenden, definiert die Zuordnungstabelle Ihre Portzuordnung.

In der folgenden <a name="snatporttable"></a>-Tabelle werden die SNAT-Port-Vorabzuordnungen für die Ebenen der Back-End-Poolgrößen angegeben:

| Poolgröße (VM-Instanzen) | Vorab zugeordnete SNAT-Ports pro IP-Konfiguration |
| --- | --- |
| 1-50 | 1\.024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1.000 | 32 | 

>[!NOTE]
> Wenn Sie über einen Back-End-Pool mit einer maximalen Größe von 10 verfügen, kann jede Instanz 64.000/10 = 6.400 Ports aufweisen, wenn Sie eine explizite Ausgangsregel definieren. Gemäß der Tabelle oben verfügt jede Instanz nur über 1.024 Ports, wenn Sie automatische Zuordnung auswählen.

## <a name="outbound-rules-and-virtual-network-nat"></a><a name="outboundrules"></a> Ausgangsregeln und Virtual Network NAT

Ausgangsregeln von Azure Load Balancer und Virtual Network NAT sind Optionen, die für ausgehenden Datenverkehr aus einem virtuellen Netzwerk verfügbar sind.

Weitere Informationen zu Ausgangsregeln finden Sie unter [Ausgangsregeln](outbound-rules.md).

Weitere Informationen zu Azure Virtual Network NAT finden Sie unter [Was ist Azure Virtual Network NAT?](../virtual-network/nat-overview.md)

## <a name="constraints"></a>Einschränkungen

*   Ports werden nach 15 Sekunden freigegeben, wenn **TCP RST** empfangen oder gesendet wird.
*   Ports werden nach 240 Sekunden freigegeben, wenn **FINACK** empfangen oder gesendet wird.
*   Wenn sich eine Verbindung im Leerlauf befindet und keine neuen Pakete gesendet werden, werden die Ports nach 4 bis 120 Minuten freigegeben.
  * Dieser Schwellenwert kann über Ausgangsregeln konfiguriert werden.
*   Jede IP-Adresse stellt 64.000 Ports bereit, die für SNAT verwendet werden können.
*   Jeder Port kann sowohl für TCP- als auch für UDP-Verbindungen mit einer IP-Zieladresse verwendet werden.
  * Ein UDP-SNAT-Port ist unabhängig davon erforderlich, ob der Zielport eindeutig ist. Für jede UDP-Verbindung mit einer IP-Zieladresse wird ein UDP-SNAT-Port verwendet.
  * Ein TCP-SNAT-Port kann für mehrere Verbindungen mit derselben IP-Zieladresse verwendet werden, vorausgesetzt, die Zielports unterscheiden sich.
*   SNAT-Erschöpfung tritt auf, wenn einer Back-End-Instanz die angegebenen SNAT-Ports ausgehen. Ein Load Balancer kann weiterhin nicht verwendete SNAT-Ports aufweisen. Wenn die von einer Back-End-Instanz verwendeten SNAT-Ports die angegebenen SNAT-Ports überschreiten, können keine neuen ausgehenden Verbindungen hergestellt werden.

## <a name="next-steps"></a>Nächste Schritte

*   [Problembehandlung von Fehlern bei ausgehenden Verbindungen aufgrund von SNAT-Erschöpfung](https://docs.microsoft.com/azure/load-balancer/troubleshoot-outbound-connection)
*   [Überprüfen Sie die SNAT-Metriken](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#how-do-i-check-my-snat-port-usage-and-allocation), und machen Sie sich mit den richtigen Verfahren zum Filtern, Aufteilen und Anzeigen dieser Metriken vertraut.

