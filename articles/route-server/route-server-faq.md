---
title: Häufig gestellte Fragen zu Azure Route Server
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zu Azure Route Server.
services: route-server
author: duongau
ms.service: route-server
ms.topic: article
ms.date: 03/02/2021
ms.author: duau
ms.openlocfilehash: 994fdf3f929f1f64ab4a064d7f2bd24c19322629
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678678"
---
# <a name="azure-route-server-preview-faq"></a>Häufig gestellte Fragen zu Azure Route Server (Vorschau)

> [!IMPORTANT]
> Azure Route Server (Vorschau) befindet sich derzeit in der öffentlichen Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="what-is-azure-route-server"></a>Was ist Azure Route Server?

Azure Route Server ist ein vollständig verwalteter Dienst, mit dem Sie das Routing zwischen Ihrem virtuellen Netzwerkgerät (Network Virtual Appliance, NVA) und Ihrem virtuellen Netzwerk auf einfache Weise verwalten können.

### <a name="is-azure-route-server-just-a-vm"></a>Ist Azure Route Server einfach nur ein virtueller Computer?

Nein. Azure Route Server ist ein Dienst, der mit hoher Verfügbarkeit entworfen wurde. Wenn er in einer Azure-Region bereitgestellt wird, die [Verfügbarkeitszonen](../availability-zones/az-overview.md) unterstützt, verfügt er über Redundanz auf Zonenebene.

### <a name="what-routing-protocols-does-azure-route-server-support"></a><a name = "protocol"></a>Welche Routingprotokolle werden von Azure Route Server unterstützt?

Azure Route Server unterstützt nur BGP (Border Gateway Protocol). Ihr NVA muss externes BGP mit mehreren Hops unterstützen, da Sie Azure Route Server in einem dedizierten Subnetz in Ihrem virtuellen Netzwerk bereitstellen müssen. Beim Konfigurieren von BGP auf Ihrem NVA muss sich die von Ihnen gewählte [ASN](https://en.wikipedia.org/wiki/Autonomous_system_(Internet)) von derjenigen unterscheiden, die von Azure Route Server verwendet wird.

### <a name="does-azure-route-server-route-data-traffic-between-my-nva-and-my-vms"></a>Leitet Azure Route Server den Datenverkehr zwischen meinem NVA und meinen VMs weiter?

Nein. Azure Route Server tauscht nur BGP-Routen mit Ihrem NVA aus. Der Datenverkehr wird direkt vom NVA an die gewählte VM und direkt von der VM zum NVA geleitet.

### <a name="if-azure-route-server-receives-the-same-route-from-more-than-one-nva-will-it-program-all-copies-of-the-route-but-each-with-a-different-next-hop-to-the-vms-in-the-virtual-network"></a>Wenn Azure Route Server dieselbe Route von mehreren NVAs empfängt, werden dann alle Kopien der Route (aber jeweils mit einem anderen nächsten Hop) programmgesteuert an die VMs im virtuellen Netzwerk gesendet?

Ja, aber nur wenn die Route dieselbe AS-Pfadlänge aufweist. Wenn die VMs Datenverkehr an das Ziel dieser Route senden, führen die VM-Hosts ein ECMP-Routing (Equal-Cost Multi Path) aus. Wenn jedoch ein NVA die Route mit einer kürzeren AS-Pfadlänge als andere NVAs sendet, sendet Azure Route Server programmgesteuert nur die Route, bei der der nächste Hop auf dieses NVA festgelegt ist, an die VMs im virtuellen Netzwerk.

### <a name="does-azure-route-server-support-vnet-peering"></a>Unterstützt Azure Route Server das VNET-Peering?

Ja. Wenn Sie ein VNET, das Azure Route Server hostet, per Peering mit einem anderen VNET verbinden und „Remotegateway verwenden“ in diesem VNET aktivieren. Azure Route Server lernt die Adressräume dieses VNET und sendet sie an alle per Peering verbundenen NVAs.

### <a name="what-autonomous-system-numbers-asns-can-i-use"></a>Welche autonomen Systemnummern (ASNs) kann ich verwenden?

Sie können Ihre eigenen öffentlichen ASNs oder privaten ASNs auf Ihrem virtuellen Netzwerkgerät verwenden. Die von Azure oder IANA reservierten Bereiche können nicht verwendet werden.
Die folgenden ASNs sind für Azure oder IANA reserviert:

* Von Azure reservierte ASNs:
    * Öffentliche ASNs: 8074, 8075, 12076
    * Private ASNs: 65515, 65517, 65518, 65519, 65520
* [Von IANA reservierte](http://www.iana.org/assignments/iana-as-numbers-special-registry/iana-as-numbers-special-registry.xhtml) ASNs:
    * 23456, 64496-64511, 65535-65551

### <a name="can-i-use-32-bit-4-byte-asns"></a>Kann ich 32-Bit-ASNs (4 Bytes) verwenden?

Nein, Azure Route Server unterstützt nur 16-Bit-ASNs (2 Bytes).

## <a name="route-server-limits"></a><a name = "limitations"></a>Grenzwerte für Route Server

Azure Route Server weist die folgenden Grenzwerte auf (pro Bereitstellung).

| Resource | Begrenzung |
|----------|-------|
| Anzahl unterstützter BGP-Peers | 8 |
| Anzahl von Routen, die jeder BGP-Peer für Azure Route Server ankündigen kann | 200 |
| Anzahl von Routen, die Azure Route Server für ein ExpressRoute- oder VPN-Gateway ankündigen kann | 200 |

Weitere Informationen finden Sie unter [Diagnose des Routingproblems einer Azure-VM](../virtual-network/diagnose-network-routing-problem.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Azure Route Server konfigurieren](quickstart-configure-route-server-powershell.md).
