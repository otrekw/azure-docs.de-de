---
title: 'Szenario: Benutzerdefinierte Isolation für VNETs'
titleSuffix: Azure Virtual WAN
description: 'Routingszenarien: Verhindern, dass ausgewählte VNETs einander erreichen können'
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 3719956df0dce62ee69d8e306ff2cad27197616d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85567659"
---
# <a name="scenario-custom-isolation-for-vnets"></a>Szenario: Benutzerdefinierte Isolation für VNETs

Wenn Sie mit Virtual WAN-Routing für virtuelle Hubs arbeiten, stehen Ihnen eine ganze Reihe von Szenarien zur Verfügung. In einem Szenario mit benutzerdefinierter Isolation für VNETs besteht das Ziel darin, zu verhindern, dass bestimmte VNETs eine andere bestimmte Gruppe von VNETs erreichen können. Allerdings müssen die VNETs alle Zweige (VPN, ExpressRoute und Benutzer-VPN) erreichen können.

In diesem Szenario sind alle VPN-, ExpressRoute- und Benutzer-VPN-Verbindungen (zusammen Zweige genannt) derselben Routingtabelle (der Standardroutingtabelle) zugeordnet. Alle VPN-, ExpressRoute- und Benutzer-VPN-Verbindungen geben Routen an dieselben Routingtabellen weiter. Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="scenario-workflow"></a><a name="architecture"></a>Szenarioworkflow

In **Abbildung 1** gibt es blaue und rote VNET-Verbindungen.

* Blaue VNETs können sich gegenseitig sowie alle Zweigverbindungen (VPN/ExpressRoute/P2S) erreichen.
* Rote VNETs können sich gegenseitig sowie alle Zweigverbindungen (VPN/ExpressRoute/P2S) erreichen.

Führen Sie zum Einrichten des Routings die folgenden Schritte aus.

1. Erstellen Sie im Azure-Portal die beiden benutzerdefinierten Routingtabellen **RT_BLUE** und **RT_RED**.
2. Gehen Sie für die Routingtabelle **RT_BLUE** so vor:
   * **Zuordnung:** Wählen Sie alle blauen VNETs aus.
   * **Weitergabe**: Für Zweige: Wählen Sie die Option für Zweige aus, was impliziert, dass Zweigverbindungen (VPN/ExpressRoute/P2S-Verbindungen) Routen an diese Routingtabelle weitergeben.
3. Wiederholen Sie diese Schritte für die Routingtabelle **RT_RED** für rote VNETs und Zweite (VPN/ExpressRoute/P2S).

Dies führt zu Änderungen der Routingkonfiguration, wie in der folgenden Abbildung dargestellt.

**Abbildung 1**

:::image type="content" source="./media/routing-scenarios/custom-isolation/custom.png" alt-text="Abbildung 1":::

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
* Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
