---
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: cherylmc
ms.openlocfilehash: 622a2f69c2e7b82f2df10d6120ee2dd466961915
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "77211446"
---
Die alten VPN-Gateway-SKUs lauten wie folgt:

* Standardwert (Basic)
* Standard
* HighPerformance

VPN Gateway verwendet nicht die UltraPerformance-Gateway-SKU. Informationen zur UltraPerformance-SKU finden Sie in der Dokumentation zu [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md).

Beachten Sie bei Verwendung der alten SKUs Folgendes:

* Wenn Sie einen richtlinienbasierten VPN-Typ verwenden möchten, müssen Sie die Basic-SKU verwenden. Richtlinienbasierte VPNs (früher als statisches Routing bezeichnet) werden in anderen SKUs nicht unterstützt.
* BGP wird in der Basic-SKU nicht unterstützt.
* Die gemeinsame Verwendung von VPN Gateway und ExpressRoute wird in der Basic-SKU nicht unterstützt.
* Aktiv-Aktiv-S2S-VPN-Gatewayverbindungen können nur in der HighPerformance-SKU konfiguriert werden.