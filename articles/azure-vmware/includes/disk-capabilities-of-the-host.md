---
title: Datenträgerfunktionen der Hosts
description: Hosts, die zum Erstellen oder Skalieren von Clustern verwendet werden, stammen aus einem isolierten Hostpool.
ms.topic: include
ms.date: 04/23/2021
author: shortpatti
ms.author: v-patsho
ms.service: azure-vmware
ms.openlocfilehash: 847f69950a47208b27976972da8dc3427727e774
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "121747622"
---
<!-- Used in plan-private-cloud-deployment.md and concepts-private-cloud-clusters.md -->


Azure VMware Solution-Cluster basieren auf einer hyperkonvergenten Bare-Metal-Infrastruktur. Die RAM-, CPU- und Datenträgerkapazitäten des Hosts sind in der folgenden Tabelle angegeben.

| Hosttyp | CPU   | RAM (GB)  | vSAN NVMe-Cacheebene (TB, Raw)  | vSAN SSD-Kapazitätsebene (TB, Raw)  |
| :---      | :---: | :---:     | :---:                           | :---:                             |
| AV36     |  Intel Dual Core, 18 Kerne 2,3 GHz  |     576      |                3.2               |                15,20               |

Hosts, die zum Erstellen oder Skalieren von Clustern verwendet werden, stammen aus einem isolierten Hostpool. Diese Hosts wurden Hardwaretests unterzogen. Darüber hinaus wurden alle Daten sicher gelöscht. 