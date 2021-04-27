---
title: Häufigkeit von VMware-Softwareupdates
description: Unterstützte Häufigkeit von VMware-Softwareupdates für Azure VMware Solution.
ms.topic: include
ms.date: 03/22/2021
ms.openlocfilehash: a3290ed704b493fae3e86223857ff9f1c5617f1a
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104869861"
---
<!-- Used in faq.md and concepts-private-clouds-clusters.md -->

Microsoft ist für die Lebenszyklusverwaltung von VMware-Software (ESXi, vCenter, PSC und NXS) in der privaten Azure VMware Solution-Cloud zuständig.

Das Upgrade der Software der privaten Cloud erfolgt nach einem Zeitplan, der sich nach der Veröffentlichung des Softwarepakets von VMware richtet. Ihre private Cloud erfordert keine Ausfallzeiten für Upgrades.

Die Upgrades für die Softwarepakete der privaten Cloud behalten eine Version der Software des neuesten Release des Softwarepakets von VMware bei. Die Softwareversionen der privaten Cloud können sich von den aktuellen Versionen der einzelnen Softwarekomponenten (ESXi, NSX-T, vCenter und vSAN) unterscheiden.

Softwareupdates umfassen:

- **Patches**: Sicherheitspatches oder Fehlerbehebungen, die von VMware freigegeben werden
- **Updates**: Änderungen der Nebenversion einer VMware-Stack-Komponente
- **Upgrades**: Änderungen der Hauptversion einer VMware-Stack-Komponente

Microsoft testet einen wichtigen Sicherheitspatch, sobald er von VMware zur Verfügung gestellt wird.

Bis die Bereitstellung der nächsten geplanten Updates erfolgt, werden dokumentierte VMware-Problemumgehungen implementiert, statt einen entsprechenden Patch zu installieren. 
