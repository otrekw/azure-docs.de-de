---
title: 'Szenario: Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät'
titleSuffix: Azure Virtual WAN
description: Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 0716ca8f0457ca801098c97dd7a5e68751822d4d
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85848101"
---
# <a name="scenario-route-traffic-through-an-nva"></a>Szenario: Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät

Wenn Sie mit Virtual WAN-Routing für virtuelle Hubs arbeiten, stehen Ihnen eine ganze Reihe von Szenarien zur Verfügung. In diesem NVA-Szenario besteht das Ziel darin, den Datenverkehr durch ein virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) für Branch-zu-VNet und VNet-zu-Branch zu leiten. Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

## <a name="scenario-architecture"></a><a name="architecture"></a>Szenarioarchitektur

In **Abbildung 1** gibt es zwei Hubs, **Hub 1** und **Hub 2**.

* **Hub 1** und **Hub 2** sind direkt mit den virtuellen NVA-Netzwerken **VNET 2** und **VNET 4** verbunden.

* **VNET 5** und **VNET 6** sind mit **VNET 2** per Peering verbunden.

* **VNET 7** und **VNET 8** sind mit **VNET 4** per Peering verbunden.

* **VNET 5, 6, 7 und 8** sind indirekte Spokes, die nicht direkt mit einem virtuellen Hub verbunden sind.

**Abbildung 1**

:::image type="content" source="./media/routing-scenarios/nva/nva.png" alt-text="Abbildung 1":::

## <a name="scenario-workflow"></a><a name="workflow"></a>Szenarioworkflow

Zur Einrichtung des Routings über das virtuelle Netzwerkgerät sind die folgenden Schritte zu beachten:

1. Identifizieren Sie die NVA-Spoke-VNet-Verbindung. In **Abbildung 1** sind dies **VNET 2-Verbindung (eastusconn)** und **VNET 4-Verbindung (weconn)** .

   Stellen Sie sicher, dass UDRs eingerichtet sind:
   * Von VNET 5 und 6 zu VNET 2 NVA IP
   * Von VNET 7 und 8 zu VNET 4 NVA IP 
   
   Sie müssen VNET 5, 6, 7 und 8 nicht direkt mit den virtuellen Hubs verbinden. Stellen Sie sicher, dass die Netzwerksicherheitsgruppen in den VNETs 5, 6, 7 und 8 den Datenverkehr für den Branch (VPN/ER/P2S) oder für VNETs zulassen, die mit ihren Remote-VNETs verbunden sind. Beispielsweise muss VNET 5, 6 sicherstellen, dass die Netzwerksicherheitsgruppen den Datenverkehr für lokale Adresspräfixe und die VNETs 7, 8 zulassen, die mit dem Remotehub 2 verbunden sind. 

2. Fügen Sie der Standardroutingtabelle von Hub 1 einen aggregierten statischen Routeneintrag für die VNETs 2, 5, 6 hinzu. 

   :::image type="content" source="./media/routing-scenarios/nva/nva-static-expand.png" alt-text="Beispiel":::

3. Konfigurieren Sie eine statische Route für VNETs 5, 6 in der virtuellen Netzwerkverbindung von VNET 2. Informationen zum Einrichten der Routingkonfiguration für eine virtuelle Netzwerkverbindung finden Sie unter [Routing virtueller Hubs](how-to-virtual-hub-routing.md#routing-configuration).

4. Fügen Sie der Standardroutingtabelle von Hub 1 einen aggregierten statischen Routeneintrag für die VNETs 4, 7, 8 hinzu.

5. Wiederholen Sie die Schritte 2, 3 und 4 für die Standardroutingtabelle von Hub 2.

Dies führt zu Änderungen der Routingkonfiguration, wie in der folgenden Abbildung dargestellt.

   :::image type="content" source="./media/routing-scenarios/nva/nva-result.png" alt-text="Ergebnis":::

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
* Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
