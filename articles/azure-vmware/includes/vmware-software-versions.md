---
title: Versionen von VMware-Software
description: Unterstützte VMware-Softwareversionen für Azure VMware Solution
ms.topic: include
ms.date: 12/29/2020
ms.openlocfilehash: c6ba2904bab6c6f44001cafed1bd4cbdeb786373
ms.sourcegitcommit: e7179fa4708c3af01f9246b5c99ab87a6f0df11c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/30/2020
ms.locfileid: "97825067"
---
<!-- Used in faq.md and concepts-private-clouds-clusters.md -->


Im Folgenden die aktuellen in Clustern privater Azure VMware Solution-Clouds verwendeten VMware-Softwareversionen:

| Software              |    Version   |
| :---                  |     :---:    |
| VCSA/vSphere/ESXi |    6.7 U3    | 
| ESXi                  |    6.7 U3    | 
| vSAN                  |    6.7 U3    |
| NSX-T                 |      2.5     |


>[!NOTE]
>NSX-T ist die einzige unterstützte Version von NSX.

Bei allen neuen Clustern in einer privaten Cloud stimmt die Softwareversion mit der aktuell ausgeführten überein. Für alle neuen privaten Clouds in einem Abonnement wird die aktuelle Version des Softwarestapels installiert. Weitere Informationen finden Sie in den [Anforderungen der VMware-Softwareversion](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-54E5293B-8707-4D29-BFE8-EE63539CC49B.html).

Die Upgrades für die Softwarepakete der privaten Cloud behalten eine Version der Software des neuesten Release des Softwarepakets von VMware bei. Die Softwareversionen der privaten Cloud können sich von den aktuellen Versionen der einzelnen Softwarekomponenten (ESXi, NSX-T, vCenter und vSAN) unterscheiden. Die allgemeinen Upgraderichtlinien und -prozesse für die Azure VMware Solution-Plattformsoftware werden unter [Updates und Upgrades für private Clouds](../concepts-upgrades.md) beschrieben.

