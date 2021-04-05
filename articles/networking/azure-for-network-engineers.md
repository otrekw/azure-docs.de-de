---
title: 'Azure ExpressRoute: Azure für Netzwerktechniker'
description: Auf dieser Seite erfahren traditionelle Netzwerkentwickler, wie Netzwerke in Azure funktionieren.
documentationcenter: na
services: networking
author: osamaz
manager: tracsman
ms.service: virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 06/25/2020
ms.author: osamaz
ms.openlocfilehash: 9a22e58e4407897fb9418cae0ba9f32408cda8e1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98234204"
---
# <a name="azure-for-network-engineers"></a>Azure für Netzwerktechniker
Als herkömmlicher Netzwerktechniker sind Sie mit physischen Ressourcen wie Routern, Switches, Kabeln und Firewalls zum Erstellen einer Infrastruktur umgegangen. Auf logischer Ebene haben Sie virtuelles LAN (VLAN), STP (Spanning Tree Protocol) und Routingprotokolle (RIP, OSPF, BGP) konfiguriert. Sie haben Ihr Netzwerk mithilfe von Verwaltungstools und einer Befehlszeilenschnittstelle verwaltet. Netzwerke in der Cloud unterscheiden sich darin, dass die Netzwerkendpunkte logisch sind und Routingprotokolle nur minimal zum Einsatz kommen. In Azure konfigurieren und verwalten Sie Ressourcen mit der Azure Resource Manager-API, der Azure-Befehlszeilenschnittstelle und PowerShell. Sie beginnen Ihre Netzwerkjourney, indem Sie sich über grundlegende Mandanten der Azure-Netzwerkstruktur informieren. 
## <a name="virtual-network"></a>Virtuelles Netzwerk
Wenn Sie ein Netzwerk von unten nach oben entwerfen, erfassen Sie einige grundlegende Informationen. Diese Informationen können etwa die Anzahl von Hosts, Netzwerkgeräte, die Anzahl von Subnetzen, das Routing zwischen Subnetzen oder Isolationsdomänen wie VLANs umfassen. Sie helfen bei der Größenanpassung der Netzwerk- und Sicherheitsgeräte sowie beim Erstellen der Architektur zur Unterstützung von Anwendungen und Diensten.

Wenn Sie Ihre Anwendungen und Dienste in Azure bereitstellen möchten, erstellen Sie zunächst eine logische Grenze in Azure, die als virtuelles Netzwerk bezeichnet wird. Dieses virtuelle Netzwerk ähnelt einer physischen Netzwerkgrenze. Da es sich um ein virtuelles Netzwerk handelt, benötigen Sie keine physische Ausrüstung, müssen jedoch weiterhin die logischen Entitäten wie IP-Adressen, IP-Subnetze, Routing und Richtlinien einplanen.

Wenn Sie ein virtuelles Netzwerk in Azure erstellen, wird es mit einem IP-Adressbereich (10.0.0.0/16) vorkonfiguriert. Dieser Bereich ist nicht fest, Sie können auch einen eigenen IP-Adressbereich definieren. Sie können sowohl IPv4- als auch IPv6-Adressbereiche definieren. IP-Bereiche, die für das virtuelle Netzwerk definiert sind, werden nicht im Internet offengelegt. Sie können mehrere Subnetze in Ihrem IP-Adressbereich erstellen. Diese Subnetze werden zum Zuweisen von IP-Adressen zu virtuellen Netzwerkschnittstellen (vNICs) verwendet. Die ersten vier IP-Adressen aus den einzelnen Subnetzen sind reserviert und können nicht für die IP-Zuordnung verwendet werden. In einer öffentlichen Cloud gibt es kein Konzept von VLANs. Sie können jedoch eine Isolation innerhalb eines virtuellen Netzwerks basierend auf Ihren definierten Subnetzen erstellen.

Sie können ein großes Subnetz mit dem gesamten Adressraum des virtuellen Netzwerks erstellen, oder Sie können mehrere Subnetze erstellen. Wenn Sie jedoch ein Gateway für ein virtuelles Netzwerk verwenden, müssen Sie in Azure ein Subnetz mit dem Namen „Gatewaysubnetz“ erstellen. Azure verwendet dieses Subnetz zum Zuweisen von IP-Adressen zu Gateways für virtuelle Netzwerke. 

## <a name="ip-allocation"></a>IP-Adresszuteilung

Wenn Sie einem Host eine IP-Adresse zuweisen, weisen Sie sie eigentlich einer Netzwerkschnittstellenkarte (NIC) zu. Sie können einer NIC in Azure zwei Typen von IP-Adressen zuweisen:

- Öffentliche IP-Adressen: werden für die eingehende und ausgehende Kommunikation (ohne Netzwerkadressübersetzung) mit dem Internet und anderen Azure-Ressourcen verwendet, die nicht mit einem virtuellen Netzwerk verbunden sind. Das Zuweisen einer öffentlichen IP-Adresse zu einer NIC ist optional. Öffentliche IP-Adressen gehören zum Microsoft-IP-Adressraum.
- Private IP-Adressen: werden für die Kommunikation in einem virtuellen Netzwerk, Ihrem lokalen Netzwerk und dem Internet (mit Netzwerkadressübersetzung) verwendet. Der IP-Adressraum, den Sie im virtuellen Netzwerk definieren, gilt auch dann als privat, wenn Sie Ihren öffentlichen IP-Adressraum konfigurieren. Microsoft gibt diesen Bereich nicht im Internet bekannt. Sie müssen einer VM mindestens eine private IP-Adresse zuweisen.

Wie bei physischen Hosts oder Geräten gibt es zwei Möglichkeiten, einer Ressource eine IP-Adresse zuzuordnen: dynamisch oder statisch. In Azure ist die Standardzuordnungsmethode dynamisch. Dabei wird eine IP-Adresse zugewiesen, wenn Sie einen virtuellen Computer (VM) erstellen oder eine beendete VM starten. Die IP-Adresse wird freigegeben, wenn Sie die VM beenden oder löschen. Damit die IP-Adresse für die VM unverändert bleibt, können Sie die Zuordnungsmethode explizit auf „Statisch“ festlegen. In diesem Fall wird sofort eine IP-Adresse zugewiesen. Sie wird nur freigegeben, wenn Sie die VM löschen oder deren Zuordnungsmethode in „Dynamisch“ ändern. 

Private IP-Adressen werden aus den Subnetzen zugewiesen, die Sie in einem virtuellen Netzwerk definiert haben. Für eine VM wählen Sie ein Subnetz für die IP-Zuordnung aus. Wenn eine VM mehrere NICs enthält, können Sie für jede NIC ein anderes Subnetz auswählen.

## <a name="routing"></a>Routing
Wenn Sie ein virtuelles Netzwerk erstellen, erstellt Azure dafür eine Routingtabelle. Diese Routingtabelle enthält die folgenden Routentypen.
- Systemrouten
- Subnetz-Standardrouten
- Routen aus anderen virtuellen Netzwerken
- BGP-Routen
- Dienstendpunktrouten
- Benutzerdefinierte Routen (UDR)

Wenn Sie erstmalig ein virtuelles Netzwerk erstellen, ohne Subnetze zu definieren, erstellt Azure Routingeinträge in der Routingtabelle. Diese Routen werden als Systemrouten bezeichnet. Systemrouten werden an dieser Stelle definiert. Sie können diese Routen nicht ändern. Sie können Systemrouten jedoch außer Kraft setzen, indem Sie UDRs konfigurieren.

Wenn Sie in einem virtuellen Netzwerk Subnetze erstellen, erstellt Azure Standardeinträge in der Routingtabelle, um die Kommunikation zwischen diesen Subnetzen in einem virtuellen Netzwerk zu ermöglichen. Diese Routen können mithilfe statischer Routen geändert werden. Dabei handelt es sich um benutzerdefinierte Routen (User Defined Routes, UDR) in Azure.

Wenn Sie ein VNET-Peering zwischen zwei virtuellen Netzwerken erstellen, wird eine Route für jeden Adressbereich innerhalb des Adressraums der einzelnen virtuellen Netzwerke hinzugefügt, für die ein Peering erstellt wird.

Wenn Ihr Gateway des lokalen Netzwerks BGP-Routen (Border Gateway Protocol) mit einem Azure-Gateway für virtuelle Netzwerke austauscht, wird für jede Route, die vom lokalen Netzwerkgateway weiterverteilt wird, eine Route hinzugefügt. Diese Routen werden in der Routingtabelle als BGP-Routen angezeigt.

Die öffentlichen IP-Adressen für bestimmte Dienste werden der Routentabelle von Azure hinzugefügt, wenn Sie einen Dienstendpunkt für den Dienst aktivieren. Dienstendpunkte werden für einzelne Subnetze in einem virtuellen Netzwerk aktiviert. Wenn Sie einen Dienstendpunkt aktivieren, wird die Route nur der Routingtabelle für das Subnetz hinzugefügt, das zu diesem Dienst gehört. Azure verwaltet die Adressen in der Routentabelle automatisch, wenn sich die Adressen ändern.

Benutzerdefinierte Routen werden auch als Benutzerrouten bezeichnet. Sie können benutzerdefinierte Routen in Azure erstellen, um die Standardsystemrouten von Azure außer Kraft zu setzen oder der Routingtabelle eines Subnetzes zusätzliche Routen hinzuzufügen. In Azure erstellen Sie eine Routingtabelle und ordnen diese dann Subnetzen eines virtuellen Netzwerks zu.

Wenn konkurrierende Einträge in einer Routingtabelle vorliegen, wählt Azure den nächsten Hop basierend auf der längsten Präfixübereinstimmung aus, ähnlich wie bei herkömmlichen Routern. Wenn jedoch mehrere Einträge für den nächsten Hop mit demselben Adresspräfix vorhanden sind, wählt Azure die Routen in der folgenden Reihenfolge aus.
1. Benutzerdefinierte Routen (UDR)
1. BGP-Routen
1. Systemrouten

## <a name="security"></a>Sicherheit

Sie können den Netzwerkdatenverkehr zu und von Ressourcen in einem virtuellen Netzwerk mithilfe von Netzwerksicherheitsgruppen filtern. Sie können auch virtuelle Netzwerkgeräte (Network Virtual Appliances, NVAs) wie Azure Firewall oder Firewalls von anderen Anbietern verwenden. Sie können steuern, wie in Azure Datenverkehr aus Subnetzen weitergeleitet wird. Zudem können Sie einschränken, welche Benutzer in Ihrer Organisation Ressourcen in virtuellen Netzwerken verwenden können.

Eine Netzwerksicherheitsgruppe (NSG) enthält eine Liste mit Regeln für eine Zugriffssteuerungsliste (Access Control List, ACL) zum Zulassen oder Verweigern von Netzwerkdatenverkehr an Subnetze, NICs oder beides. NSGs können entweder Subnetzen oder einzelnen NICs zugeordnet werden, die mit einem Subnetz verbunden sind. Wenn eine NSG einem Subnetz zugeordnet ist, gelten die ACL-Regeln für alle VMs in diesem Subnetz. Darüber hinaus kann Datenverkehr zu einer einzelnen NIC beschränkt werden, indem eine NSG direkt einer NIC zugewiesen wird.

NGSs enthalten zwei Regelsätze: eingehende und ausgehende Regeln. Die Priorität für eine Regel muss innerhalb jedes Satzes eindeutig sein. Jede Regel verfügt über Eigenschaften für Protokoll, Quell- und Zielportbereiche, Adresspräfixe, Richtung des Datenverkehrs, Priorität und Zugriffstyp. Alle NSGs enthalten eine Gruppe von Standardregeln. Die Standardregeln können zwar nicht gelöscht werden, haben aber niedrigste Priorität und können somit durch selbst erstellte Regeln außer Kraft gesetzt werden.

## <a name="verification"></a>Überprüfung
### <a name="routes-in-virtual-network"></a>Routen in einem virtuellen Netzwerk
Aus der Kombination von Routen, die Sie erstellen, Standardrouten von Azure und allen Routen, die von Ihrem lokalen Netzwerk durch ein Azure-VPN-Gateway (sofern das virtuelle Netzwerk mit Ihrem lokalen Netzwerk verbunden ist) über das Border Gateway Protocol (BGP) verteilt werden, resultieren die effektiven Routen für alle Netzwerkschnittstellen in einem Subnetz. Sie können diese effektiven Routen anzeigen, indem Sie über das Portal, PowerShell oder die Befehlszeilenschnittstelle zur NIC navigieren.
### <a name="network-security-groups"></a>Netzwerksicherheitsgruppen
Die geltenden Sicherheitsregeln, die auf eine Netzwerkschnittstelle angewendet werden, stellen eine Aggregation der Regeln, die in der einer Netzwerkschnittstelle zugeordneten NSG vorhanden sind, und des Subnetzes der Netzwerkschnittstelle dar. Sie können alle effektiven Sicherheitsregeln der NSGs anzeigen, die auf die Netzwerkschnittstellen Ihrer VM angewandt werden. Navigieren Sie dazu über das Portal, PowerShell oder die Befehlszeilenschnittstelle zur NIC.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [virtuelle Netzwerke][VNet].

Erfahren Sie mehr über das [Routing in virtuellen Netzwerken][vnet-routing].

Erfahren Sie mehr über [Netzwerksicherheitsgruppen][network-security].

<!--Link References-->
[VNet]: ../virtual-network/tutorial-connect-virtual-networks-portal.md
[vnet-routing]: ../virtual-network/virtual-networks-udr-overview.md
[network-security]: ../virtual-network/network-security-groups-overview.md