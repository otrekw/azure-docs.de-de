---
title: Datenträgerfunktionen der Hosts
description: Hosts, die zum Erstellen oder Skalieren von Clustern verwendet werden, stammen aus einem isolierten Hostpool.
ms.topic: include
ms.date: 04/23/2021
ms.openlocfilehash: 7ee15fbfd668d5db24282a6e4de8e4dfc6639c3e
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107945749"
---
<!-- Used in production-ready-deployment-steps.md and concepts-private-cloud-clusters.md -->


Azure VMware Solution-Cluster basieren auf einer hyperkonvergenten Bare-Metal-Infrastruktur. Die RAM-, CPU- und Datenträgerkapazitäten des Hosts sind in der folgenden Tabelle angegeben.

| Hosttyp | CPU   | RAM (GB)  | vSAN NVMe-Cacheebene (TB, Raw)  | vSAN SSD-Kapazitätsebene (TB, Raw)  |
| :---      | :---: | :---:     | :---:                           | :---:                             |
| AV36     |  Intel Dual Core, 18 Kerne 2,3 GHz  |     576      |                3.2               |                15,20               |

Hosts, die zum Erstellen oder Skalieren von Clustern verwendet werden, stammen aus einem isolierten Hostpool. Diese Hosts wurden Hardwaretests unterzogen. Darüber hinaus wurden alle Daten sicher gelöscht. 