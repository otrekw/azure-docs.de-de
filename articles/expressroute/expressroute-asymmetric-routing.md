---
title: 'Azure ExpressRoute: Asymmetrisches Routing'
description: Dieser Artikel behandelt Probleme, die bei asymmetrischem Routing in einem Netzwerk mit mehreren Verbindungen mit einem Ziel auftreten können.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: article
ms.date: 12/14/2020
ms.author: duau
ms.openlocfilehash: 0713c52c7f07db93d9ae9084062ef2c3b25a9074
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97560507"
---
# <a name="asymmetric-routing-with-multiple-network-paths"></a>Asymmetrisches Routing mit mehreren Netzwerkpfaden
In diesem Artikel wird erläutert, wie Netzwerkdatenverkehr über unterschiedliche Pfade geleitet werden kann, wenn zwischen Netzwerkquelle und -ziel mehrere Routen verfügbar sind.

Um das asymmetrische Routing zu verstehen, müssen Sie zwei Konzepte kennen. Das erste Konzept betrifft die Auswirkung mehrerer Netzwerkpfade. Beim zweiten Konzept geht es darum, wie Geräte – beispielsweise eine Firewall – ihren Status beibehalten. Geräte dieser Art werden als „zustandsbehaftete Geräte“ bezeichnet. Wenn diese beiden Faktoren kombiniert werden, kann ein Szenario entstehen, in dem Netzwerkdatenverkehr vom zustandsbehafteten Gerät verworfen wird.  Der Datenverkehr wird verworfen, weil das Gerät nicht erkennt, dass er aus dem Gerät selbst stammt.

## <a name="multiple-network-paths"></a>Mehrere Netzwerkpfade
Wenn ein Unternehmensnetzwerk nur über eine einzige Internetverbindung (über einen Internetdienstanbieter) verfügt, wird der gesamte ein- und ausgehende Internetdatenverkehr über den gleichen Pfad geleitet. Es ist gängige Praxis, dass Unternehmen mehrere Leitungen erwerben, um redundante Pfade zu erstellen und so die Netzwerkverfügbarkeit zu verbessern. In einer solchen Konfiguration ist es möglich, dass der Datenverkehr über eine Leitung zum Internet gelangt und über eine andere Leitung zurückkommt. Dieses Szenario wird allgemein als asymmetrisches Routing bezeichnet. Beim asymmetrischen Routing nimmt der zurückkehrende Netzwerkdatenverkehr einen anderen Pfad als der ursprüngliche ausgehende Datenfluss.

![Netzwerk mit mehreren Pfaden](./media/expressroute-asymmetric-routing/AsymmetricRouting3.png)

Asymmetrisches Routing ist bei Verbindungen mit dem Internet gang und gäbe. Allerdings tritt es auch auf, wenn eine Kombination aus mehreren Pfaden verwendet wird. Ein Beispiel wären ein Internetpfad und ein privater Pfad zum selben Ziel. Ein zweites Beispiel wären mehrere private Pfade, die ebenfalls dasselbe Ziel haben.

Jeder Router im Pfad zwischen Quelle und Ziel berechnet den besten Pfad zum Erreichen des Ziels. Der Router bestimmt den bestmöglichen Pfad anhand von zwei Hauptfaktoren:

* Das Routing zwischen externen Netzwerken basiert auf einem Routingprotokoll, und zwar dem Border Gateway Protocol (BGP). BGP nimmt Ankündigungen von Nachbarn entgegen und führt eine Reihe von Schritten aus, um den optimalen Pfad zum Ziel zu bestimmen. Der optimale Pfad wird in die eigene Routingtabelle aufgenommen.
* Die Länge der Subnetzmaske einer Route wirkt sich auf die Routingpfade aus. Wenn ein Router mehrere Ankündigungen für dieselbe IP-Adresse empfängt, wählt er den Pfad mit der längeren Subnetzmaske aus, weil dies als die spezifischere Route angesehen wird.

## <a name="stateful-devices"></a>Zustandsbehaftete Geräte
Router analysieren zu Routingzwecken den IP-Header eines Pakets. Einige Geräte sehen sich das Paket noch eingehender an. Normalerweise untersuchen diese Geräte Layer 4-Header wie Transmission Control Protocol (TCP) oder User Datagram Protocol (UDP). Auch Layer 7-Header (Anwendungsschicht) werden untersucht. Bei dieser Art von Geräten handelt es sich entweder um Sicherheitsgeräte oder um Geräte zur Bandbreitenoptimierung. 

Eine Firewall ist ein gängiges Beispiel für ein zustandsbehaftetes Gerät. Eine Firewall entscheidet anhand verschiedener Kriterien, ob die Weiterleitung von Paketen über ihre Schnittstellen zugelassen oder abgelehnt wird. Zu diesen Kriterien gehören beispielsweise Protokoll, TCP/UDP-Port oder URL-Header. Dieses Maß an Paketuntersuchung kann eine hohe Verarbeitungslast für das Gerät bedeuten. 

Zur Verbesserung der Leistung prüft die Firewall das erste Paket eines Datenflusses. Wenn die Firewall die Weiterleitung des Pakets über ihre Schnittstellen zulässt, werden die Datenflussinformationen in der Zustandstabelle gespeichert. Alle folgenden Pakete, die zu diesem Datenfluss gehören, werden auf Grundlage der ursprünglichen Entscheidung zugelassen. Bei der Firewall kommt möglicherweise ein Paket an, das zu einem vorhandenen Datenfluss gehört, aber die Firewall noch nicht passiert hat. Da die Firewall nicht über vorherige Zustandsinformationen zum ursprünglichen Datenfluss verfügt, verwirft sie das Paket.

## <a name="asymmetric-routing-with-expressroute"></a>Asymmetrisches Routing mit ExpressRoute
Wenn Sie mit Azure ExpressRoute eine Verbindung mit Microsoft herstellen, ändert sich Ihr Netzwerk wie folgt:

* Sie verfügen über mehrere Verbindungen mit Microsoft: Ihre vorhandene Internetverbindung und Ihre ExpressRoute-Verbindung. Bestimmter Datenverkehr an Microsoft wird möglicherweise über die Internetverbindung geleitet, kehrt jedoch über Ihre ExpressRoute-Verbindung zu Ihnen zurück. Dasselbe kann passieren, wenn Datenverkehr Ihre Umgebung über ExpressRoute verlässt, aber über den Internetpfad zurückkehrt.
* Von der ExpressRoute-Leitung haben Sie spezifischere IP-Adressen erhalten. Wenn also Datenverkehr aus Ihrem Netzwerk für über ExpressRoute angebotene Dienste an Microsoft gesendet wird, bevorzugen Ihre Router immer die ExpressRoute-Verbindung.

Sehen wir uns nun einige Szenarien an, um die Auswirkungen dieser beiden Änderungen auf das Netzwerk zu verdeutlichen. Ein Beispiel: Sie verfügen über eine Internetleitung und nutzen alle Microsoft-Dienste über das Internet. Der Datenverkehr zwischen Ihrem Netzwerk und Microsoft wird über die gleiche Internetverbindung geleitet und durchläuft die Firewall. Die Firewall zeichnet den Datenfluss auf, wenn sie das erste Paket registriert. Alle nachfolgenden Pakete in dieser Kommunikation sind zulässig, weil der Datenfluss in der Zustandstabelle vorhanden ist.

![Asymmetrisches Routing mit ExpressRoute](./media/expressroute-asymmetric-routing/AsymmetricRouting1.png)

Anschließend aktivieren Sie eine ExpressRoute-Leitung, um Dienste zu nutzen, die von Microsoft über ExpressRoute angeboten werden. Alle anderen Dienste von Microsoft werden über die Internetverbindung genutzt. Sie stellen am Edge eine separate Firewall bereit, die mit der ExpressRoute-Verbindung verknüpft ist. Microsoft kündigt Ihrem Netzwerk über ExpressRoute spezifischere Präfixe für bestimmte Dienste an. Für diese Präfixe verwendet Ihre Routinginfrastruktur ExpressRoute als bevorzugten Pfad. 

Wenn Sie Microsoft Ihre öffentlichen IP-Adressen nicht über ExpressRoute ankündigen, kommuniziert Microsoft über das Internet mit Ihren öffentlichen IP-Adressen. Datenverkehr, der von Ihrem Netzwerk an Microsoft gesendet wird, verwendet die ExpressRoute-Verbindung, aber der Rückgabedatenverkehr von Microsoft verwendet dann den Internetpfad. Wenn die Firewall an Ihrem Edge ein Antwortpaket für einen Datenfluss ermittelt, den sie nicht kennt, verwirft sie das Paket.

Wenn Sie denselben NAT-Pool (Network Address Translation, Netzwerkadressenübersetzung) für ExpressRoute und das Internet ankündigen, werden Sie bei privaten IP-Adressen ähnliche Probleme mit den Clients in Ihrem Netzwerk feststellen. Anforderungen für Dienste wie Windows Update werden über die Internetverbindung geleitet, weil IP-Adressen für diese Dienste nicht über ExpressRoute angekündigt werden. Der Rückgabedatenverkehr wird jedoch über ExpressRoute geleitet. Da Microsoft über die Internet- und die ExpressRoute-Verbindung eine IP-Adresse mit der gleichen Subnetzmaske erhalten hat, ist ExpressRoute immer der bevorzugte Pfad. Wenn eine Firewall oder ein anderes zustandsbehaftetes Gerät an Ihrem Netzwerkedge, das für die ExpressRoute-Verbindung zugänglich ist, zuvor keine Informationen zu einem Datenfluss erhalten hat, werden diese Pakete verworfen.

## <a name="asymmetric-routing-solutions"></a>Lösungen für asymmetrisches Routing
Sie haben zwei Möglichkeiten, wie Sie das Problem des asymmetrischen Routings lösen können. Sie können entweder Routing oder eine quellenbasierte Netzwerkadressenübersetzung (Source Network Address Translation, SNAT) nutzen.

### <a name="routing"></a>Routing
Stellen Sie sicher, dass Ihre öffentlichen IP-Adressen den entsprechenden WAN-Verbindungen (Wide Area Network) angekündigt werden. Ein Beispiel: Sie möchten das Internet für den Authentifizierungsdatenverkehr und ExpressRoute für den E-Mail-Datenverkehr verwenden. In diesem Fall kündigen Sie die öffentlichen IP-Adressen Ihrer Active Directory-Verbunddienste (AD FS) nicht über ExpressRoute an. Stellen Sie auch sicher, dass Sie Ihren lokalen AD FS-Server nicht für IP-Adressen verfügbar machen, die der Router über ExpressRoute empfängt. Über ExpressRoute eingehende Routen sind spezifischer, sodass ExpressRoute als Pfad für Authentifizierungsdatenverkehr bevorzugt wird, der für Microsoft bestimmt ist. Wenn Sie nicht sehr genau darauf achten, wie das Routing in Ihrem Netzwerk funktioniert, können Probleme durch asymmetrisches Routing entstehen.

Wenn Sie ExpressRoute für die Authentifizierung verwenden möchten, müssen Sie öffentliche IP-Adressen für AD FS ohne Netzwerkadressenübersetzung über ExpressRoute ankündigen. Bei dieser Konfiguration wird der von Microsoft stammende Datenverkehr an Ihren lokalen AD FS-Server über ExpressRoute geleitet. Der zurückkehrende Datenverkehr von Ihrem Netzwerk an Microsoft wird über ExpressRoute geleitet, da diese Route der Internetverbindung vorgezogen wird.

### <a name="source-based-nat"></a>Quellenbasierte NAT
Eine andere Möglichkeit zum Lösen des Problems mit asymmetrischem Routing ist die Verwendung von SNAT. Ein Beispiel: Sie entscheiden sich, die öffentliche IP-Adresse eines lokalen SMTP-Servers (Simple Mail Transfer Protocol) nicht über ExpressRoute anzukündigen. Stattdessen möchten Sie für diese Art der Kommunikation das Internet verwenden. Eine Anforderung, die von Microsoft stammt und für Ihren lokalen SMTP-Server bestimmt ist, wird über die Internetverbindung geleitet. Die eingehende Anforderung wird von der SNAT verarbeitet und an eine interne IP-Adresse weitergeleitet. Der zurückkehrende Datenverkehr vom SMTP-Server wird über die Edgefirewall (die Sie zur Netzwerkadressenübersetzung verwenden) geleitet, nicht über ExpressRoute. Daher verläuft der zurückkehrende Datenverkehr über den Internetpfad.

![Netzwerkkonfiguration mit quellenbasierter NAT](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## <a name="asymmetric-routing-detection"></a>Erkennung von asymmetrischem Routing
Wenn Sie sich vergewissern möchten, dass der Netzwerkdatenverkehr den erwarteten Pfad durchläuft, verwenden Sie am besten Traceroute. Wenn Sie davon ausgehen, dass Datenverkehr von Ihrem lokalen SMTP-Server an Microsoft über den Internetpfad geleitet wird, verläuft die erwartete Traceroute zwischen SMTP-Server und Microsoft 365. Das Ergebnis gibt Aufschluss darüber, ob der Datenverkehr Ihr Netzwerk tatsächlich in Richtung Internet und nicht Richtung ExpressRoute verlässt.

