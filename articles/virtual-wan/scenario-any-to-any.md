---
title: 'Szenario: Any-to-Any'
titleSuffix: Azure Virtual WAN
description: 'Szenarien für das Routing: Any-to-Any'
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: ecc2b3cf236cb2a78fd595189649e7f6b176d709
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85567670"
---
# <a name="scenario-any-to-any"></a>Szenario: Any-to-Any

Wenn Sie mit Virtual WAN-Routing für virtuelle Hubs arbeiten, stehen Ihnen eine ganze Reihe von Szenarien zur Verfügung. In einem Any-to-Any-Szenario kann jeder Spoke einen anderen Spoke erreichen. Wenn mehrere Hubs vorhanden sind, ist das Hub-zu-Hub-Routing (auch als Routing zwischen Hubs bezeichnet) im virtuellen Standard-WAN standardmäßig aktiviert. 

In diesem Szenario sind alle VPN-, ExpressRoute- und Benutzer-VPN-Verbindungen derselben Routingtabelle zugeordnet. Alle VPN-, ExpressRoute- und Benutzer-VPN-Verbindungen geben Routen an denselben Satz von Routingtabellen weiter. Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="scenario-architecture"></a><a name="architecture"></a>Szenarioarchitektur

In **Abbildung 1** können alle VNETs und Branches (VPN, ExpressRoute, P2S) einander erreichen. In einem virtuellen Hub funktionieren die Verbindungen wie folgt:

* Eine VPN-Verbindung verbindet einen VPN-Standort mit einem VPN-Gateway.
* Eine VNET-Verbindung verbindet ein virtuelles Netzwerk mit einem virtuellen Hub. Der Router des virtuellen Hubs bietet die Transitfunktionalität zwischen VNETs.
* Eine ExpressRoute-Verbindung verbindet eine ExpressRoute-Leitung mit einem ExpressRoute-Gateway.

Diese Verbindungen werden (standardmäßig bei Erstellung) der Standardroutingtabelle zugeordnet, sofern Sie die Routingkonfiguration der Verbindung nicht als **Keine** oder eine benutzerdefinierte Routingtabelle einrichten. Diese Verbindungen übergeben Routen standardmäßig an die Standardroutingtabelle. Dadurch wird ein Any-to-Any-Szenario ermöglicht, in dem alle Spokes (VNET, VPN, ER, P2S) einander erreichen können.

**Abbildung 1**

:::image type="content" source="./media/routing-scenarios/any-any/figure-1.png" alt-text="Abbildung 1":::

## <a name="scenario-workflow"></a><a name="workflow"></a>Szenarioworkflow

Dieses Szenario ist für das virtuelle Standard-WAN standardmäßig aktiviert. Wenn die Einstellung für Branch-to-Branch in der WAN-Konfiguration deaktiviert ist, sind keine Verbindungen zwischen Branchspokes möglich. VPN/ExpressRoute/Benutzer-VPN werden in Virtual WAN als Branchspokes angesehen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
* Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
