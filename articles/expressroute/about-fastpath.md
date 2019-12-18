---
title: Informationen zu Azure ExpressRoute FastPath
description: Hier erfahren Sie, wie Sie mit Azure ExpressRoute FastPath Netzwerkdatenverkehr senden, indem Sie das Gateway umgehen.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: cherylmc
ms.openlocfilehash: 6ff1dac312bcb4df1e1afc9679df09fc8a2b28ff
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2019
ms.locfileid: "74896888"
---
# <a name="about-expressroute-fastpath"></a>Informationen zu ExpressRoute FastPath

Das virtuelle Netzwerkgateway ExpressRoute wurde entwickelt, um Netzwerkrouten auszutauschen und den Netzwerkdatenverkehr zu steuern. FastPath wurde entwickelt, um die Datenpfadleistung zwischen Ihrem lokalen und dem virtuellen Netzwerk zu verbessern. Wenn aktiviert, sendet FastPath den Netzwerkdatenverkehr direkt an virtuelle Computer im virtuellen Netzwerk und umgeht dabei das Gateway.

## <a name="requirements"></a>Requirements (Anforderungen)

### <a name="circuits"></a>Leitungen

FastPath ist unter allen ExpressRoute-Verbindungen verfügbar.

### <a name="gateways"></a>Gateways

FastPath erfordert weiterhin die Erstellung eines virtuellen Netzwerkgateways für den Austausch von Routen zwischen virtuellem und lokalem Netzwerk. Weitere Informationen zu Gateways für virtuelle Netzwerk und ExpressRoute finden Sie unter [Informationen zu ExpressRoute-Gateways für virtuelle Netzwerke](expressroute-about-virtual-network-gateways.md).

Für das Gateway für virtuelle Netzwerke muss eine der folgenden Optionen festgelegt werden:

* Höchstleistung
* ErGw3AZ

## <a name="supported-features"></a>Unterstützte Features

FastPath unterstützt zwar die meisten Konfigurationen, aber nicht die folgenden Features:

* UDR für das Gatewaysubnetz: Wenn Sie ein UDR auf das Gatewaysubnetz Ihres virtuellen Netzwerks anwenden, wird der Netzwerkdatenverkehr aus Ihrem lokalen Netzwerk weiterhin an das Gateway für virtuelle Netzwerke gesendet.

* VNET-Peering: Wenn andere virtuelle Netzwerke per Peering mit dem Netzwerk verbunden sind, das mit ExpressRoute verbunden ist, wird der Netzwerkdatenverkehr von Ihrem lokalen Netzwerk zu den anderen virtuellen Netzwerken (d. h. den sogenannten virtuellen Spoke-Netzwerken) weiterhin an das Gateway für virtuelle Netzwerke gesendet. Die Problemumgehung besteht darin, alle virtuellen Netzwerke direkt mit der ExpressRoute-Verbindung zu verbinden.

* Load Balancer im Tarif „Basic“: Wenn Sie einen internen Lastenausgleich im Tarif „Basic“ in Ihrem virtuellen Netzwerk bereitstellen oder der Azure-PaaS-Dienst, den Sie in Ihrem virtuellen Netzwerk bereitstellen, einen internen Lastenausgleich im Tarif „Basic“ verwendet, wird der Netzwerkdatenverkehr von Ihrem lokalen Netzwerk zu den virtuellen IP-Adressen, die auf dem Lastenausgleich im Tarif „Basic“ gehostet werden, an das Gateway für virtuelle Netzwerke gesendet. Die Lösung besteht darin, den Basic-Load Balancer auf einen [Standard-Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) zu aktualisieren.

* Private Link: Wenn Sie von Ihrem lokalen Netzwerk eine Verbindung mit einem [privaten Endpunkt](../private-link/private-link-overview.md) in Ihrem virtuellen Netzwerk herstellen, erfolgt die Verbindung über das Gateway für virtuelle Netzwerke.
 
## <a name="next-steps"></a>Nächste Schritte

Informationen zum Aktivieren von FastPath finden Sie unter [Verbinden eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md#configure-expressroute-fastpath).
