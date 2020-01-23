---
title: -Peeringrichtlinie
titleSuffix: Azure
description: -Peeringrichtlinie
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: conceptual
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: a683ad71f5e80c91728262dc7bbabf36e9d68deb
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773949"
---
# <a name="peering-policy"></a>Peeringrichtlinie
Die allgemeinen Anforderungen von Microsoft an Ihr Netzwerk werden in den folgenden Abschnitten erläutert. Diese gelten sowohl für Anforderungen für Direct Peering als auch für Exchange Peering.

## <a name="technical-requirements"></a>Technische Anforderungen

* Ein vollständig redundantes Netzwerk mit ausreichender Kapazität zum Austauschen von Datenverkehr ohne Überlastung.
* Der Peer verfügt über eine öffentliche routingfähige Autonome Systemnummer (ASN).
* Sowohl IPv4 als auch IPv6 werden unterstützt, und Microsoft erwartet, dass an jedem Peeringstandort beide Sitzungstypen eingerichtet werden.
* MD5 wird nicht unterstützt.
* **ASN-Details**:
    * Microsoft verwaltet AS8075 zusammen mit den folgenden ASNs: AS8068, AS8069, AS12076. Eine umfassende Liste der ASNs mit AS8075-Peering finden Sie unter AS-SET MICROSOFT.
    * Alle Peeringpartner von Microsoft stimmen zu, Routen von AS12076 (Express Route) unter keinen Umständen zu akzeptieren und AS12076 auf allen Peers auszufiltern.
* **Routingrichtlinie:**
    * Ein Peer hat mindestens eine öffentlich routbare /24.
    * Microsoft überschreibt empfangene Multi-Exit-Diskriminatoren (MED).
    * Microsoft zieht es vor, BGP-Communitytags von Peers zu empfangen, die den Ursprung von Routen anzeigen.
    * Von Peers wird erwartet, dass sie ihre Routen zum Zweck der Filterung in einer öffentlichen IRR-Datenbank (Internet Routing Registry) registrieren und sich nach besten Kräften bemühen, diese Informationen stets aktuell zu halten.
    * Wir schlagen vor, dass Peers ein max. Präfix von 1000 (IPv4) bzw. 100 (IPv6) bei Routen für Peeringsitzungen mit Microsoft festlegen.
    * Sofern nicht zuvor spezifisch anders vereinbart, wird von Peers erwartet, dass sie Routen an allen Standorten, an denen Peering mit Microsoft erfolgt, konsistent ankündigen.
    * Im Allgemeinen werden alle Peeringsitzungen mit AS8075 als AS-MICROSOFT-Routen angezeigt. AS8075-Verbindungen in Afrika und Asien sind möglicherweise auf Routen beschränkt, die für die jeweilige Region relevant sind.
    * Keine Partei richtet eine statische Route oder Rückfallroute ein oder sendet der anderen Partei in gleich welcher Weise Datenverkehr für eine Route, die nicht per BGP angekündigt wurde.
    * Von den Peers wird die Einhaltung der [MANRS](https://www.manrs.org/)-Branchenstandards für die Routensicherheit erwartet.

## <a name="operational-requirements"></a>Betriebsanforderungen
* Ein rund um die Uhr voll besetztes Network Operations Center (NOC), das bei der Behebung aller technischen und leistungsbezogenen Probleme, Sicherheitsverletzungen, DoS-Angriffen (Denial-of-Service) oder jeder anderen missbräuchlichen Verwendung beim Peer oder seinen Kunden helfen kann.
* Von Peers wird erwartet, ein vollständiges und aktuelles Profil auf [PeeringDB](https://www.peeringdb.com) zu unterhalten, einschließlich einer rund um die Uhr erreichbaren NOC-E-Mail-Adresse mit der Unternehmensdomäne sowie einer Telefonnummer. Wir verwenden diese Informationen, um die Detailangaben des Peers zu überprüfen, wie etwa die NOC-Informationen, Informationen zum technischen Kontakt, die Anwesenheit in den Peeringeinrichtungen usw. Persönliche Yahoo-, Gmail- und Hotmail-Konten werden nicht akzeptiert.

## <a name="physical-connection-requirements"></a>Physische Verbindungsanforderungen
* Die Standorte, an denen Sie Verbindungen mit Microsoft für Direct Peering oder Exchange Peering herstellen können, sind in [PeeringDB](https://www.peeringdb.com/net/694) aufgelistet.
* **Exchange Peering:**
    * Die Verbindung muss über eine Monomodefaser unter Verwendung der entsprechenden optischen 10 GBit/s-Komponenten erfolgen.
    * Von den Peers wird ein Upgrade ihrer Ports erwartet, wenn die Spitzenauslastung 50 % überschreitet.
* **Direct Peering:**
    * Die Verbindung muss über eine Monomodefaser unter Verwendung der entsprechenden optischen 10 GBit/s- oder 100 GBit/s-Komponenten erfolgen.
    * Microsoft richtet Direct Peering nur mit Internetdienstanbietern oder Netzwerkdienstanbietern ein.
    * Von Peers wird ein Upgrade ihrer Ports erwartet, wenn die Spitzenauslastung 50 % überschreitet sowie die Aufrechterhaltung separater Kapazitäten in jeder Metropolregion, entweder an einem einzelnen Standort oder verteilt auf mehrere Standorte in einer Metropolregion.
    * Jedes Direct Peering besteht aus zwei Verbindungen von zwei Microsoft-Edgeroutern zu den Routern des Peers, die sich am Standort des Peers befinden. Microsoft schreibt duale BGP-Sitzungen über diese Verbindungen vor. Der Peer kann sich dafür entscheiden, an seinem Ende keine redundanten Geräte bereitzustellen.

## <a name="traffic-requirements"></a>Datenverkehrsanforderungen
* Peers müssen für Exchange Peering min. 200 MBit und max. 2 GBit Datenverkehr bereitstellen können.  Für Datenverkehr, der 2 GBit überschreitet, sollte Direct Peering geprüft werden.
* Für Direct Peering muss Datenverkehr von Ihrem Netzwerk zu Microsoft die unten dargelegten Mindestanforderungen erfüllen.

    | geografischer Raum                      | Minimaler Datenverkehr an Microsoft   |
    | :----------------------- |:-------------------------------|
    | Afrika                   | 500 MBit/s                       |
    | APAC (ausgenommen Indien)      |   2 GBit/s                       |
    | APAC (nur Indien)        | 500 MBit/s                       |
    | Europa                   |   2 GBit/s                       |
    | LATAM                    |   2 GBit/s                       |
    | Naher Osten              | 500 MBit/s                       |
    | Nicht verfügbar                       |   2 GBit/s                       |

* **Diversität:**
    * In Nordamerika, Europa, dem asiatisch-pazifischen und dem lateinamerikanischen Wirtschaftsraum sollte die physische Verbindung mit mindestens drei geografisch verteilten Standorten erfolgen (sofern verfügbar) und die Aufrechterhaltung davon unabhängiger Kapazität unterstützen, um ein Failover des Datenverkehrs in den einzelnen Metropolregionen zu ermöglichen.
    * In Afrika, Nahost und Indien sollte die physische Verbindung mit so vielen verschiedenen Standorten wie möglich erfolgen. Es muss ausreichend unabhängige Kapazität aufrechterhalten werden, um sicherzustellen, dass der Datenverkehr in der Region bleibt.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Einrichten von Direct Peering mit Microsoft finden Sie in der [exemplarischen Vorgehensweise für Direct Peering](walkthrough-direct-all.md).
* Informationen zum Einrichten von Exchange-Peering mit Microsoft finden Sie in der [exemplarischen Vorgehensweise für Exchange Peering](walkthrough-exchange-all.md).