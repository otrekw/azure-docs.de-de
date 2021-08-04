---
title: Einschränkungen für Azure VMware Solution
description: In diesem Artikel erhalten Sie Informationen zu Einschränkungen für Azure VMware Solution.
ms.topic: include
ms.date: 03/24/2021
ms.openlocfilehash: ce0f346c66c7eb982f0891b82e710affdcc956a8
ms.sourcegitcommit: b35c7f3e7f0e30d337db382abb7c11a69723997e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2021
ms.locfileid: "109719190"
---
<!-- Used in /azure/azure-resource-manager/management/azure-subscription-service-limits.md and concepts-networking.md -->

In der nachstehenden Tabelle werden die maximalen Grenzwerte für Azure VMware Solution beschrieben.

| **Ressource** | **Begrenzung** |
| :-- | :-- |
| Cluster pro privater Cloud | 12 |
| Mindestanzahl Knoten pro Cluster | 3 |
| Maximale Anzahl Knoten pro Cluster | 16 |
| Knoten pro privater Cloud | 96 |
| vCenter-Instanzen pro privater Cloud | 1  |
| HCX-Standortkopplungen | 3 mit Version „Advanced“, 10 mit Version „Enterprise“ |
| Maximale Anzahl verknüpfter privater AVS-ExpressRoute-Clouds | 4<br />Das verwendete Gateway für virtuelle Netzwerke bestimmt die tatsächliche maximale Anzahl von verknüpften privaten Clouds.  Weitere Informationen finden Sie unter [Informationen zu ExpressRoute-Gateways für virtuelle Netzwerke](../../expressroute/expressroute-about-virtual-network-gateways.md). | 
| AVS-ExpressRoute-Portgeschwindigkeit | 10 GBit/s<br />Das verwendete virtuelle Netzwerkgateway bestimmt die tatsächliche Bandbreite. Weitere Informationen finden Sie unter [Informationen zu ExpressRoute-Gateways für virtuelle Netzwerke](../../expressroute/expressroute-about-virtual-network-gateways.md). | 
| Über vWAN verfügbar gemachte öffentliche IPs | 100 |
| vSAN-Kapazitätsgrenzen | 75 Prozent der insgesamt nutzbaren Kapazität (25 Prozent werden für die SLA vorgehalten)  |

Verwenden Sie für andere VMware-spezifische Grenzwerte das Tool [VMware Configuration Maximum](https://configmax.vmware.com/).
