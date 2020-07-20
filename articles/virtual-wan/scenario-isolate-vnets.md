---
title: 'Szenario: Isolieren von virtuellen Netzwerken (VNETs)'
titleSuffix: Azure Virtual WAN
description: 'Szenarien für Routing: Isolieren von virtuellen Netzwerken (VNETs)'
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: f43f17a0f3742831920836e448de3ef757f2dfa6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85567653"
---
# <a name="scenario-isolating-vnets"></a>Szenario: Isolieren von virtuellen Netzwerken (VNETs)

Wenn Sie mit Virtual WAN-Routing für virtuelle Hubs arbeiten, stehen Ihnen eine ganze Reihe von Szenarien zur Verfügung. In diesem Szenario besteht das Ziel darin zu verhindern, dass VNETs andere VNETs erreichen können. Dies wird als Isolieren von VNETs bezeichnet. Die Workload im jeweiligen VNET bleibt isoliert und kann nicht mit anderen VNETs kommunizieren, wie dies in einem Any-to-Any-Szenario der Fall ist. Allerdings müssen die VNETs alle Zweige (VPN, ER und Benutzer-VPN) erreichen können. In diesem Szenario sind alle VPN-, ExpressRoute- und Benutzer-VPN-Verbindungen derselben (einzigen) Routingtabelle zugeordnet. Alle VPN-, ExpressRoute- und Benutzer-VPN-Verbindungen geben Routen an dieselben Routingtabellen weiter. Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="scenario-workflow"></a><a name="workflow"></a>Szenarioworkflow

Führen Sie zur Konfiguration dieses Szenarios die folgenden Schritte aus:

1. Erstellen Sie eine benutzerdefinierte Routingtabelle. Im Beispiel ist **RT_VNet** die Routingtabelle. Informationen, wie eine Routingtabelle erstellt wird, finden Sie unter [Konfigurieren des Routings für virtuelle Hubs](how-to-virtual-hub-routing.md). Weitere Informationen über Routingtabellen finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
2. Wenn Sie die **RT_VNet**-Routingtabelle erstellen, konfigurieren Sie die folgenden Einstellungen:

   * **Zuordnung:** Wählen Sie die VNETs aus, die Sie isolieren möchten.
   * **Weitergabe**: Wählen Sie die Option für Zweige aus, was impliziert, dass Zweigverbindungen (VPN/ER/P2S-Verbindungen) Routen an diese Routingtabelle weitergeben.

:::image type="content" source="./media/routing-scenarios/isolated/isolated-vnets.png" alt-text="Isolierte VNETs":::

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
* Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
