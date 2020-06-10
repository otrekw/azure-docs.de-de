---
title: Was ist Azure Virtual Network NAT?
titlesuffix: Azure Virtual Network
description: Enthält eine Übersicht über Features, Ressourcen, Architektur und Implementierung von Virtual Network NAT. Es wird beschrieben, wie Virtual Network NAT funktioniert und wie Sie NAT-Gatewayressourcen in der Cloud nutzen.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
ms.subservice: nat
Customer intent: As an IT administrator, I want to learn more about Virtual Network NAT, its NAT gateway resources, and what I can use them for.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2020
ms.author: allensu
ms.openlocfilehash: 3ef219841769d7bdc5fc6e3f3aa50465b6159f82
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84341319"
---
# <a name="what-is-virtual-network-nat"></a>Was ist Virtual Network NAT?

Virtual Network NAT (Network Address Translation, Netzwerkadressenübersetzung) vereinfacht für virtuelle Netzwerke die Einrichtung von ausschließlich ausgehenden Internetverbindungen. Bei der Konfiguration in einem Subnetz werden für die gesamte Konnektivität in ausgehender Richtung die von Ihnen angegebenen statischen öffentlichen IP-Adressen verwendet.  Die ausgehende Konnektivität ist möglich, ohne dass ein Lastenausgleich oder öffentliche IP-Adressen direkt virtuellen Computern zugeordnet werden. NAT ist vollständig verwaltet und äußerst resilient.

<!-- 
<img src="./media/nat-overview/flow-map.svg" width="270" align="center">
![Virtual Network NAT](./media/nat-overview/flow-map.svg)
-->

<p align="center">
  <img src="./media/nat-overview/flow-map.svg" width="256" title="Virtual Network NAT">
</p>



*Abbildung: Virtual Network NAT*

## <a name="static-ip-addresses-for-outbound-only"></a>Statische IP-Adressen ausschließlich für den Ausgang

Die ausgehende Konnektivität kann für jedes Subnetz mit NAT definiert werden.  Mehrere Subnetze desselben virtuellen Netzwerks können über unterschiedliche NATs verfügen. Ein Subnetz wird konfiguriert, indem angegeben wird, welche NAT-Gatewayressource verwendet werden soll. Für alle ausgehenden UDP- und TCP-Datenflüsse einer VM-Instanz wird NAT genutzt. 

NAT ist mit Ressourcen für öffentliche IP-Adressen und Präfixressourcen für öffentliche IP-Adressen der Standard-SKU kompatibel (bzw. einer Kombination dieser Ressourcen).  Sie können das Präfix einer öffentlichen IP-Adresse direkt nutzen, oder die öffentlichen IP-Adressen des Präfixes auf mehrere NAT-Gatewayressourcen verteilen. Mit NAT wird der gesamte Datenverkehr auf den IP-Adressbereich des Präfixes bereinigt.  Das IP-Whitelisting ist für Ihre Bereitstellungen somit jetzt sehr einfach.

Der gesamte ausgehende Datenverkehr für das Subnetz wird per NAT ohne Konfiguration durch den Kunden automatisch verarbeitet.  Benutzerdefinierte Routen sind nicht erforderlich. NAT hat Vorrang vor anderen Ausgangsszenarien und ersetzt das Standard-Internetziel eines Subnetzes.

## <a name="on-demand-snat-with-multiple-ip-addresses-for-scale"></a>Bedarfsgesteuerte SNAT mit mehreren IP-Adressen zur Skalierung

Für NAT wird die „Port-Netzwerkadressenübersetzung“ (Port Network Address Translation, PNAT/PAT) verwendet. Diese Vorgehensweise wird für die meisten Workloads empfohlen. Dynamische oder divergente Workloads können über die bedarfsgesteuerte Datenflusszuteilung in ausgehender Richtung problemlos abgedeckt werden. Eine umfassende Vorabplanung, Vorabzuteilung und damit verbundene Überbereitstellung von Ausgangsressourcen wird vermieden. SNAT-Portressourcen werden gemeinsam genutzt und sind in allen Subnetzen verfügbar, indem eine spezifische NAT-Gatewayressource verwendet und bei Bedarf bereitgestellt wird.

Eine mit NAT verknüpfte öffentliche IP-Adresse stellt bis zu 64.000 gleichzeitige Datenflüsse für UDP und TCP bereit. Sie können mit einer einzelnen IP-Adresse beginnen und bis auf 16 öffentliche IP-Adressen hochskalieren.

Bei NAT können Datenflüsse aus dem virtuellen Netzwerk ins Internet erstellt werden. Aus dem Internet zurückfließender Datenverkehr ist nur als Antwort auf einen aktiven Datenfluss zulässig.

Im Gegensatz zu SNAT in ausgehender Richtung für Load Balancer gelten bei NAT keine Einschränkungen in Bezug darauf, welche private IP-Adresse einer VM-Instanz ausgehende Verbindungen herstellen kann.  Für sekundäre IP-Konfigurationen kann eine ausgehende Internetverbindung mit NAT erstellt werden.

## <a name="coexistence-of-inbound-and-outbound"></a>Koexistenz von eingehenden und ausgehenden Verbindungen

NAT ist mit den folgenden Ressourcen der Standard-SKU kompatibel:

- Load Balancer
- Öffentliche IP-Adresse
- Präfix für öffentliche IP-Adressen

Bei der gemeinsamen Verwendung mit NAT stellen diese Ressourcen für Ihre Subnetze die Internetkonnektivität in eingehender Richtung bereit. Per NAT wird die gesamte Internetkonnektivität in ausgehender Richtung aus Ihren Subnetzen bereitgestellt.

NAT und kompatible Features der Standard-SKU sind über die Richtung informiert, in der der Datenfluss gestartet wurde. Eine Koexistenz von Eingangs- und Ausgangsszenarien ist möglich. Bei diesen Szenarien werden die richtigen Netzwerkadressenübersetzungen empfangen, weil diese Features die Datenflussrichtung kennen. 

<!-- 
<img src="./media/nat-overview/flow-direction4.svg" width="500" align="center">
![Virtual Network NAT flow direction](./media/nat-overview/flow-direction4.svg)
-->
<p align="center">
  <img src="./media/nat-overview/flow-direction4.svg" width="512" title="Richtung von Virtual Network NAT-Datenflüssen">
</p>

*Abbildung: Richtung von Virtual Network NAT-Datenflüssen*

## <a name="fully-managed-highly-resilient"></a>Vollständig verwaltet, äußerst resilient

NAT ist von Beginn an vollständig horizontal hochskaliert. Es ist keine Vorbereitung bzw. kein Vorgang zum horizontalen Hochskalieren erforderlich.  Der NAT-Vorgang wird von Azure für Sie verwaltet.  NAT verfügt immer über mehrere Fehlerdomänen und kann mehrere Fehler ohne Dienstausfall überstehen.

## <a name="tcp-reset-for-unrecognized-flows"></a>TCP-Zurücksetzung für nicht erkannte Datenflüsse

Die private Seite von NAT sendet TCP-Zurücksetzungspakete, wenn versucht wird, über eine nicht vorhandene TCP-Verbindung zu kommunizieren. Ein Beispiel hierfür sind Verbindungen, für die das Leerlauftimeout erreicht wurde. Für das nächste empfangene Paket wird eine TCP-Zurücksetzung an die private IP-Adresse zurückgegeben, um das Schließen der Verbindung zu signalisieren und zu erzwingen.

Die öffentliche Seite von NAT generiert keine TCP-Zurücksetzungspakete oder anderen Datenverkehr.  Es wird nur Datenverkehr ausgegeben, der aus dem virtuellen Netzwerk des Kunden stammt.

## <a name="configurable-tcp-idle-timeout"></a>Konfigurierbares TCP-Leerlauftimeout

Standardmäßig wird ein TCP-Leerlauftimeout von vier Minuten verwendet, das auf bis zu 120 Minuten erhöht werden kann. Bei allen Aktivitäten eines Datenflusses kann auch der Leerlaufzeitgeber, einschließlich TCP-Keep-Alives, zurückgesetzt werden.

## <a name="regional-or-zone-isolation-with-availability-zones"></a>Regionale oder Zonenisolation mit Verfügbarkeitszonen

NAT ist standardmäßig regional. Bei der Erstellung von Szenarien mit [Verfügbarkeitszonen](../availability-zones/az-overview.md) kann NAT in einer bestimmten Zone isoliert werden (Zonenbereitstellung).

<!-- 
<img src="./media/nat-overview/az-directions.svg" width="500" align="center">
![Virtual Network NAT with availability zones](./media/nat-overview/az-directions.svg)
-->

<p align="center">
  <img src="./media/nat-overview/az-directions.svg" width="512" title="Virtual Network NAT mit Verfügbarkeitszonen">
</p>

*Abbildung: Virtual Network NAT mit Verfügbarkeitszonen*

## <a name="multi-dimensional-metrics-for-observability"></a>Einblick dank mehrdimensionaler Metriken

Sie können den Betrieb von NAT mit mehrdimensionalen Metriken überwachen, die in Azure Monitor verfügbar gemacht werden. Diese Metriken können verwendet werden, um die Nutzung zu verfolgen und die Problembehandlung durchzuführen.  Von NAT-Gatewayressourcen werden die folgenden Metriken verfügbar gemacht:
- Byte
- Pakete
- Verworfene Pakete
- Gesamte SNAT-Verbindungen
- Der SNAT-Verbindungsstatus ändert sich pro Intervall.

<!-- "ADD when PM is done" Learn more about [NAT gateway metrics](./nat-gateway-metrics.md) -->

## <a name="sla"></a>SLA

Bei allgemeiner Verfügbarkeit werden für den NAT-Datenpfad mindestens 99,9 % erreicht.


## <a name="pricing"></a>Preise

Für die Abrechnung des NAT-Gateways werden zwei separate Verbrauchseinheiten verwendet:

| Zähler | Rate |
| --- | --- |
| Ressourcenstunden | 0,045 USD/Stunde |
| Verarbeitete Datenmenge | 0,045 USD/GB |

Die Ressourcenstunden geben den Zeitraum an, in dem eine NAT-Gatewayressource vorhanden ist.
Die verarbeitete Datenmenge steht für den gesamten Datenverkehr, der von einer NAT-Gatewayressource verarbeitet wird.

## <a name="availability"></a>Verfügbarkeit

Virtual Network NAT und die NAT-Gatewayressource sind in allen [Regionen](https://azure.microsoft.com/global-infrastructure/regions/) der öffentlichen Azure-Cloud verfügbar.

## <a name="support"></a>Support

Support für NAT ist über die üblichen Kanäle verfügbar.

## <a name="feedback"></a>Feedback

Wir möchten wissen, wie wir den Dienst verbessern können. Unter [UserVoice für NAT](https://aka.ms/natuservoice) können Sie Vorschläge zu den nächsten Entwicklungsschritten machen und darüber abstimmen.


## <a name="limitations"></a>Einschränkungen

* NAT ist mit öffentlichen IP-Adressen, Präfixen für öffentliche IP-Adressen und Lastenausgleichsressourcen der Standard-SKU kompatibel. Basic-Ressourcen, z. B. Load Balancer im Tarif „Basic“, und alle davon abgeleiteten Produkte sind nicht mit NAT kompatibel.  Basic-Ressourcen müssen in einem Subnetz angeordnet sein, für das NAT nicht konfiguriert ist.
* Die Familie der IPv4-Adressen wird unterstützt.  NAT interagiert nicht mit der Familie der IPv6-Adressen.  Die NAT kann nicht in einem Subnetz mit IPv6-Präfix bereitgestellt werden.
* Die Protokollierung von NSG-Datenflüssen wird bei Verwendung von NAT nicht unterstützt.
* NAT kann übergreifend für mehrere virtuelle Netzwerke genutzt werden.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über [NAT-Gatewayressourcen](./nat-gateway-resource.md).
* [Teilen Sie uns bei UserVoice mit, welche Funktionen wir als Nächstes für Virtual Network NAT entwickeln sollen.](https://aka.ms/natuservoice)

