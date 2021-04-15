---
title: Einschränkungen für Azure VMware Solution
description: In diesem Artikel erhalten Sie Informationen zu Einschränkungen für Azure VMware Solution.
ms.topic: include
ms.date: 03/24/2021
ms.openlocfilehash: 997a5ae96ff30226d055b7b966b128d7ec0ae5bd
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105582905"
---
<!-- Used in /azure/azure-resource-manager/management/azure-subscription-service-limits.md -->

In der nachstehenden Tabelle werden die maximalen Grenzwerte für Azure VMware Solution beschrieben.

| **Ressource** | **Begrenzung** |
| :-- | :-- |
| Cluster pro privater Cloud | 12 |
| Mindestanzahl Knoten pro Cluster | 3 |
| Maximale Anzahl Knoten pro Cluster | 16 |
| Knoten pro privater Cloud | 96 |
| vCenter-Instanzen pro privater Cloud | 1  |
| HCX-Standortkopplungen | 3 mit Version „Advanced“, 10 mit Version „Enterprise“ |
| Maximale Anzahl verknüpfter AVS-ExpressRoute-SDDCs | 4 |
| AVS-ExpressRoute-Portgeschwindigkeit | 10 GBit/s<br />Das verwendete virtuelle Netzwerkgateway bestimmt die tatsächliche Bandbreite. Weitere Informationen finden Sie unter [Informationen zu ExpressRoute-Gateways für virtuelle Netzwerke](../../expressroute/expressroute-about-virtual-network-gateways.md). | 
| Über vWAN verfügbar gemachte öffentliche IPs | 100 |
| vSAN-Kapazitätsgrenzen | 75 Prozent der insgesamt nutzbaren Kapazität (25 Prozent werden für die SLA vorgehalten)  |

Verwenden Sie für andere VMware-spezifische Grenzwerte das Tool [VMware Configuration Maximum](https://configmax.vmware.com/).
