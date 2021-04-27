---
title: Versionen von VMware-Software
description: Unterstützte VMware-Softwareversionen für Azure VMware Solution
ms.topic: include
ms.date: 03/31/2021
ms.openlocfilehash: a6441b55bbc6a8f694c50bbf022a6a2ae52d60bf
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106097646"
---
<!-- Used in faq.md and concepts-private-clouds-clusters.md -->


Die VMware-Softwareversionen in neuen Bereitstellungen von Private Cloud-Clustern von Azure VMware Solution sind wie folgt:

| Software              |    Version   |
| :---                  |     :---:    |
| VCSA/vSphere/ESXi |    6.7 U3l    | 
| ESXi                  |    6.7 U3l    | 
| vSAN                  |    6.7 U3l    |
| NSX-T <br />**HINWEIS:** NSX-T ist die einzige unterstützte Version von NSX.               |      3.1.1     |


Bei neuen Clustern, die einer vorhandenen privaten Cloud hinzugefügt werden, wird die aktuell ausgeführte Softwareversion übernommen. Weitere Informationen finden Sie in den [Anforderungen der VMware-Softwareversion](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-54E5293B-8707-4D29-BFE8-EE63539CC49B.html).

Die Upgrades für die Softwarepakete der privaten Cloud behalten eine Version der Software des neuesten Release des Softwarepakets von VMware bei. Die Softwareversionen der privaten Cloud können sich von den aktuellen Versionen der einzelnen Softwarekomponenten (ESXi, NSX-T, vCenter und vSAN) unterscheiden. Die allgemeinen Upgraderichtlinien und -prozesse für die Azure VMware Solution-Plattformsoftware werden unter [Updates und Upgrades für private Clouds](../concepts-upgrades.md) beschrieben.

