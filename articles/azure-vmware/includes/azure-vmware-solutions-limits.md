---
title: Einschränkungen für Azure VMware Solution
description: In diesem Artikel erhalten Sie Informationen zu Einschränkungen für Azure VMware Solution.
ms.topic: include
ms.date: 03/04/2021
ms.openlocfilehash: 99c27694a300284cfd99b8411306c90ad4d8a12e
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102193917"
---
<!-- Used in /azure/azure-resource-manager/management/azure-subscription-service-limits.md -->

In der nachstehenden Tabelle werden die maximalen Grenzwerte für Azure VMware Solution beschrieben.

| **Ressource** | **Begrenzung** |
| :-- | :-- |
| Cluster pro privater Cloud | 4 |
| Mindestanzahl Knoten pro Cluster | 3 |
| Maximale Anzahl Knoten pro Cluster | 16 |
| Knoten pro privater Cloud | 64 |
| vCenter-Instanzen pro privater Cloud | 1  |
| HCX-Standortkopplungen | 3 mit Version „Advanced“, 10 mit Version „Enterprise“ |
| Maximale Anzahl verknüpfter AVS-ExpressRoute-SDDCs | 4 |
| AVS-ExpressRoute-Portgeschwindigkeit | 10 GBit/s | 
| Über vWAN verfügbar gemachte öffentliche IPs | 100 |
| vSAN-Kapazitätsgrenzen | 75 Prozent der insgesamt nutzbaren Kapazität (25 Prozent werden für die SLA vorgehalten)  |

Verwenden Sie für andere VMware-spezifische Grenzwerte das Tool [VMware Configuration Maximum](https://configmax.vmware.com/).
