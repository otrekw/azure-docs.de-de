---
title: Versionen von VMware-Software
description: Unterstützte VMware-Softwareversionen für Azure VMware Solution
ms.topic: include
ms.date: 03/13/2021
ms.openlocfilehash: 9a1781d163e67c2f9b6a4f756a616e59a969696f
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/14/2021
ms.locfileid: "103462512"
---
<!-- Used in faq.md and concepts-private-clouds-clusters.md -->


Die VMware-Softwareversionen in neuen Bereitstellungen von Private Cloud-Clustern von Azure VMware Solution sind wie folgt:

| Software              |    Version   |
| :---                  |     :---:    |
| VCSA/vSphere/ESXi |    6.7 U3    | 
| ESXi                  |    6.7 U3    | 
| vSAN                  |    6.7 U3    |
| NSX-T <br />**HINWEIS:** NSX-T ist die einzige unterstützte Version von NSX.               |      2.5     |


Bei neuen Clustern, die einer vorhandenen privaten Cloud hinzugefügt werden, wird die aktuell ausgeführte Softwareversion übernommen. Weitere Informationen finden Sie in den [Anforderungen der VMware-Softwareversion](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-54E5293B-8707-4D29-BFE8-EE63539CC49B.html).

Die Upgrades für die Softwarepakete der privaten Cloud behalten eine Version der Software des neuesten Release des Softwarepakets von VMware bei. Die Softwareversionen der privaten Cloud können sich von den aktuellen Versionen der einzelnen Softwarekomponenten (ESXi, NSX-T, vCenter und vSAN) unterscheiden. Die allgemeinen Upgraderichtlinien und -prozesse für die Azure VMware Solution-Plattformsoftware werden unter [Updates und Upgrades für private Clouds](../concepts-upgrades.md) beschrieben.

