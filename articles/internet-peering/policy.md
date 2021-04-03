---
title: -Peeringrichtlinie
titleSuffix: Azure
description: -Peeringrichtlinie
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: conceptual
ms.date: 12/15/2020
ms.author: prmitiki
ms.openlocfilehash: bee41bb8e5beb4df3086ab50499cb185a83e4efe
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97592328"
---
# <a name="peering-policy"></a>Peeringrichtlinie
Microsoft verwaltet eine selektive Peeringrichtlinie, die eine optimale Kundenfreundlichkeit sicherstellen soll. Sie nutzt Branchenstandards und bewährten Methoden, kann für die zukünftige Nachfrage skaliert werden und ermöglicht eine strategische Platzierung des Peerings. Daher behält sich Microsoft das Recht vor, bei Bedarf Ausnahmen für die Richtlinie festzulegen. Die allgemeinen Anforderungen von Microsoft an Ihr Netzwerk werden in den folgenden Abschnitten erläutert. Diese gelten sowohl für Anforderungen für Direct Peering als auch für Exchange Peering. 

## <a name="technical-requirements"></a>Technische Anforderungen

* Ein vollständig redundantes Netzwerk mit ausreichender Kapazität zum Austauschen von Datenverkehr ohne Überlastung.
* Der Peer verfügt über eine öffentliche routingfähige Autonome Systemnummer (ASN).
* Sowohl IPv4 als auch IPv6 werden unterstützt, und Microsoft erwartet, dass an jedem Peeringstandort beide Sitzungstypen eingerichtet werden.
* MD5 wird nicht unterstützt.
* **ASN-Details**:

    * Microsoft verwaltet AS8075 zusammen mit den folgenden ASNs: AS8068, AS8069, AS12076. Eine umfassende Liste der ASNs mit AS8075-Peering finden Sie unter AS-SET MICROSOFT.
    * Alle Peeringpartner von Microsoft stimmen zu, Routen von AS12076 (ExpressRoute) unter keinen Umständen zu akzeptieren und AS12076 auf allen Peers auszufiltern.

* **Routingrichtlinie:**
    * Ein Peer hat mindestens eine öffentlich routbare /24.
    * Microsoft überschreibt empfangene Multi-Exit-Diskriminatoren (MED).
    * Microsoft zieht es vor, BGP-Communitytags von Peers zu empfangen, die den Ursprung von Routen anzeigen.
    * Es wird empfohlen, dass Peers bei Routen für Peeringsitzungen mit Microsoft als maximale Anzahl von Präfixen 2.000 (IPv4) bzw. 500 (IPv6) festlegen.
    * Sofern nicht zuvor spezifisch anders vereinbart, wird von Peers erwartet, dass sie Routen an allen Standorten, an denen Peering mit Microsoft erfolgt, konsistent ankündigen.
    * Im Allgemeinen werden alle Peeringsitzungen mit AS8075 als AS-MICROSOFT-Routen angezeigt. Microsoft kündigt möglicherweise einige regionale Besonderheiten an.
    * Keine Partei richtet eine statische Route oder Rückfallroute ein oder sendet der anderen Partei in gleich welcher Weise Datenverkehr für eine Route, die nicht per BGP angekündigt wurde.
    * Von Peers wird erwartet, dass sie ihre Routen zum Zweck der Filterung in einer öffentlichen IRR-Datenbank (Internet Routing Registry) registrieren und diese Informationen stets aktuell halten.      
    * Von den Peers wird die Einhaltung der MANRS-Branchenstandards für die Routensicherheit erwartet.  Microsoft kann nach eigenem Ermessen Folgendes entscheiden: 1.) kein Peering mit Unternehmen einzurichten, die keine Routen signiert und registriert haben; 2.) ungültige RPKI-Routen zu entfernen; 3.) keine Routen von eingerichteten Peers zu akzeptieren, die nicht registriert und signiert sind. 

## <a name="operational-requirements"></a>Betriebsanforderungen
* Ein rund um die Uhr voll besetztes Network Operations Center (NOC), das bei der Behebung aller technischen und leistungsbezogenen Probleme, Sicherheitsverletzungen, DoS-Angriffen (Denial-of-Service) oder jeder anderen missbräuchlichen Verwendung beim Peer oder seinen Kunden helfen kann.
* Von Peers wird erwartet, ein vollständiges und aktuelles Profil auf [PeeringDB](https://www.peeringdb.com) zu unterhalten, einschließlich einer rund um die Uhr erreichbaren NOC-E-Mail-Adresse mit der Unternehmensdomäne sowie einer Telefonnummer. Wir verwenden diese Informationen, um die Detailangaben des Peers zu überprüfen, wie etwa die NOC-Informationen, Informationen zum technischen Kontakt, die Anwesenheit in den Peeringeinrichtungen usw. Persönliche Yahoo-, Gmail- und Hotmail-Konten werden nicht akzeptiert.

## <a name="physical-connection-requirements"></a>Physische Verbindungsanforderungen
* Die Standorte, an denen Sie Verbindungen mit Microsoft für Direct Peering oder Exchange Peering herstellen können, sind in [PeeringDB](https://www.peeringdb.com/net/694) aufgelistet.

* **Exchange Peering:**
    * Es wird von Peers erwartet, dass sie für den Austausch eine Verbindung mit mindestens 10 GBit/s bereitstellen.
    * Von den Peers wird ein Upgrade ihrer Ports erwartet, wenn die Spitzenauslastung 50 % überschreitet.
    * Microsoft fordert Peers auf, verschiedene Verbindungen für den Austausch aufrechtzuerhalten, um Failoverszenarien zu unterstützen.

* **Direct Peering:**
    * Die Verbindung muss über eine Monomodefaser unter Verwendung optischer Komponenten mit 100 GBit/s erfolgen.
    * Microsoft richtet Direct Peering nur mit Internetdienstanbietern oder Netzwerkdienstanbietern ein.
    * Von Peers wird ein Upgrade ihrer Ports erwartet, wenn die Spitzenauslastung 50 % überschreitet sowie die Aufrechterhaltung separater Kapazitäten in jeder Metropolregion, entweder an einem einzelnen Standort oder verteilt auf mehrere Standorte in einer Metropolregion.
    * Jedes Direct Peering besteht aus zwei Verbindungen von zwei Microsoft-Edgeroutern zu den Routern des Peers, die sich am Standort des Peers befinden. Microsoft schreibt duale BGP-Sitzungen über diese Verbindungen vor. Der Peer kann sich dafür entscheiden, an seinem Ende keine redundanten Geräte bereitzustellen.


## <a name="traffic-requirements"></a>Datenverkehrsanforderungen

* Peers müssen für das Austauschpeering mindestens 500 MBit und höchstens 2 GBit Datenverkehr bereitstellen können. Für Datenverkehr, der 2 GBit überschreitet, sollte direktes Peering eingerichtet werden.
* Für das direkte Peering benötigt Microsoft mindestens 2 GBit. Jeder gegenseitig vereinbarte Peeringstandort muss Failover unterstützen, um sicherzustellen, dass das Peering auch in einem Failoverszenario lokalisierbar bleibt. 

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Einrichten von Direct Peering mit Microsoft finden Sie in der [exemplarischen Vorgehensweise für Direct Peering](walkthrough-direct-all.md).
* Informationen zum Einrichten von Exchange-Peering mit Microsoft finden Sie in der [exemplarischen Vorgehensweise für Exchange Peering](walkthrough-exchange-all.md).
