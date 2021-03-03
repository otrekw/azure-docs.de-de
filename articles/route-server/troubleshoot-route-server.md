---
title: Beheben von Problemen mit Azure Route Server
description: Hier erfahren Sie, wie Sie Probleme mit Azure Route Server beheben.
services: route-server
author: duongau
ms.service: route-server
ms.topic: how-to
ms.date: 03/02/2021
ms.author: duau
ms.openlocfilehash: 02dd9aa74da42f0a5d70de4513b88756a97bea1e
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678758"
---
# <a name="troubleshooting-azure-route-server-issues"></a>Beheben von Problemen mit Azure Route Server

> [!IMPORTANT]
> Azure Route Server (Vorschau) befindet sich derzeit in der öffentlichen Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="bgp-connectivity-issues"></a>BGP-Konnektivitätsprobleme

### <a name="why-is-the-bgp-peering-between-my-nva-and-the-azure-route-server-going-up-and-down-flapping"></a>Warum wird das BGP-Peering zwischen meinem NVA und der Azure Route Server-Instanz hoch- und herunterskaliert („Fluktuation“)?

Der Grund für die Fluktuation könnte die BGP-Timereinstellung sein. Standardmäßig ist der Keep-Alive-Timer in Azure Route Server auf 60 Sekunden und der Hold-Down-Timer auf 180 Sekunden festgelegt.

### <a name="why-can-i-ping-from-my-nva-to-the-bgp-peer-ip-on-azure-route-server-but-after-i-set-up-the-bgp-peering-between-them-i-cant-ping-the-same-ip-anymore-why-does-the-bgp-peering-goes-down"></a>Warum kann ich von meinem NVA die BGP-Peer-IP von Azure Route Server anpingen, aber dieselbe IP-Adresse nicht mehr anpingen, nachdem ich das BGP-Peering zwischen ihnen eingerichtet habe? Warum wird das BGP-Peering herunterskaliert?

Bei einigen NVAs müssen Sie eine statische Route für das Azure Route Server-Subnetz hinzufügen. Wenn sich beispielsweise Azure Route Server in 10.0.255.0/27 und Ihr NVA in 10.0.1.0/24 befindet, müssen Sie der Routingtabelle auf dem NVA die folgende Route hinzufügen:

| Route | Nächster Hop |
|-------|----------|
| 10.0.255.0/27 | 10.0.1.1 |

10.0.1.1 ist die Standardgateway-IP-Adresse in dem Subnetz, in dem Ihr NVA (oder genauer gesagt eine der NICs) gehostet wird.

## <a name="bgp-route-issues"></a>BGP-Routenprobleme

### <a name="why-does-my-nva-not-receive-routes-from-azure-route-server-even-though-the-bgp-peering-is-up"></a>Warum empfängt mein NVA keine Routen von Azure Route Server, obwohl das BGP-Peering aktiv ist?

Die von Azure Route Server verwendete ASN lautet 65515. Konfigurieren Sie unbedingt eine andere ASN für Ihr NVA, damit eine „eBGP“-Sitzung zwischen Ihrem NVA und Azure Route Server eingerichtet werden kann, sodass die Routenverteilung automatisch erfolgen kann.

### <a name="the-bgp-peering-between-my-nva-and-azure-route-server-is-up-i-can-see-routes-exchanged-correctly-between-them-why-arent-the-nva-routes-in-the-effective-routing-table-of-my-vm"></a>Das BGP-Peering zwischen meinem NVA und Azure Route Server ist aktiv. Ich kann sehen, dass die Routen zwischen ihnen richtig ausgetauscht werden. Warum sind die NVA-Routen nicht in der gültigen Routingtabelle meines virtuellen Computers enthalten? 

* Wenn sich Ihr virtueller Computer in demselben VNet befindet wie Ihr NVA und Azure Route Server:

     Azure Route Server macht zwei BGP-Peer-IPs verfügbar, die auf zwei VMs gehostet werden, die für das Senden der Routen an alle anderen VMs in Ihrem virtuellen Netzwerk zuständig sind. Jedes Ihrer virtuellen Netzwerkgeräte muss für die beiden VMs zwei identische BGP-Sitzungen einrichten (z. B. dieselbe ASN und denselben AS-Pfad verwenden und dieselben Routen ankündigen), damit Ihre VMs im virtuellen Netzwerk konsistente Routinginformationen von Azure Route Server erhalten können. Siehe folgendes Diagramm.

    ![Diagramm eines virtuellen Netzwerkgeräts mit Route Server.](./media/faq/network-virtual-appliances.png)

    Wenn Sie über zwei oder mehr Instanzen des NVAs verfügen, *können* Sie verschiedene AS-Pfade für dieselbe Route von verschiedenen NVA-Instanzen ankündigen, wenn Sie eine NVA-Instanz als aktiv und die andere als passiv festlegen möchten.

* Wenn sich Ihr virtueller Computer in einem anderen virtuellen Netzwerk befindet als dem, in dem Ihr NVA und Azure Route Server gehostet werden: Überprüfen Sie, ob das VNet-Peering zwischen den beiden VNets aktiviert ist *und* ob „Remote-Route Server verwenden“ für das VNet Ihres virtuellen Computers aktiviert ist.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [eine Azure Route Server-Instanz konfigurieren](quickstart-configure-route-server-powershell.md).
