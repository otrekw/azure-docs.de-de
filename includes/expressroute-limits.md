---
title: include file
description: include file
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 07/25/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d1402bcc0c46003429e1809e8d09e9662218cd82
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335111"
---
| Resource | Begrenzung |
| --- | --- |
| ExpressRoute-Verbindungen pro Abonnement |10 |
| ExpressRoute-Verbindungen pro Region und Abonnement mit Azure Resource Manager |10 |
| Maximale Anzahl der für das private Azure-Peering angekündigten Routen mit ExpressRoute Standard |4\.000 |
| Maximale Anzahl der für das private Azure-Peering angekündigten Routen mit ExpressRoute Premium-Add-On |10.000 |
| Maximale Anzahl der vom privaten Azure-Peering angekündigten Routen aus dem VNET-Adressraum für eine ExpressRoute-Verbindung |200 |
| Maximale Anzahl der für das Microsoft-Peering angekündigten Routen mit ExpressRoute Standard |200 |
| Maximale Anzahl der für das Microsoft-Peering angekündigten Routen mit ExpressRoute Premium-Add-On |200 |
| Maximale Anzahl von ExpressRoute-Verbindungen, die mit dem gleichen virtuellen Netzwerk am selben Peeringstandort verknüpft sind |4 |
| Maximale Anzahl von ExpressRoute-Verbindungen, die mit dem gleichen virtuellen Netzwerk an unterschiedlichen Peeringstandorten verknüpft sind |4 |
| Anzahl der pro ExpressRoute-Leitung zulässigen virtuellen Netzwerkverbindungen |Siehe Tabelle [Anzahl von virtuellen Netzwerken pro ExpressRoute-Leitung](#vnetpercircuit).  |

#### <a name="number-of-virtual-networks-per-expressroute-circuit"></a><a name="vnetpercircuit"></a>Anzahl von virtuellen Netzwerken pro ExpressRoute-Leitung
| **Verbindungsgröße** | **Anzahl der Verknüpfungen eines virtuellen Netzwerks für Standard** | **Anzahl der Verknüpfungen eines virtuellen Netzwerks mit Premium-Add-on** |
| --- | --- | --- |
| 50 MBit/s |10 |20 |
| 100 MBit/s |10 |25 |
| 200 MBit/s |10 |25 |
| 500 MBit/s |10 |40 |
| 1 GBit/s |10 |50 |
| 2 GBit/s |10 |60 |
| 5 GBit/s |10 |75 |
| 10 GBit/s |10 |100 |
| 40 GBit/s* |10 |100 |
| 100 GBit/s* |10 |100 |

**100 GBit/s nur bei ExpressRoute Direct*

> [!NOTE]
> Global Reach-Verbindungen werden auf den Grenzwert virtueller Netzwerkverbindungen pro ExpressRoute-Verbindung angerechnet. Beispiel: Eine Premium-Verbindung mit 10 GBit/s ermöglicht 5 Global Reach-Verbindungen und 95 Verbindungen mit den ExpressRoute-Gateways oder 95 Global Reach-Verbindungen und 5 Verbindungen mit den ExpressRoute-Gateways oder eine beliebige andere Kombination bis zum Grenzwert von 100 Verbindungen für die Leitung.
