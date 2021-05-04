---
title: Häufigkeit von VMware-Softwareupdates
description: Unterstützte Häufigkeit von VMware-Softwareupdates für Azure VMware Solution.
ms.topic: include
ms.date: 04/23/2021
ms.openlocfilehash: 5adf381e3c27197b6791dbd389d3b7359fa30248
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107950992"
---
<!-- Used in faq.md and concepts-private-clouds-clusters.md -->

Die Hostwartung und Lebenszyklusverwaltung haben keine Auswirkungen auf die Kapazität oder Leistung von Clustern in privaten Clouds.  Das Upgrade der Software der privaten Cloud erfolgt nach einem Zeitplan, der sich nach der Veröffentlichung des Softwarepakets von VMware richtet. Ihre private Cloud erfordert keine Ausfallzeiten für Upgrades.

Die Upgrades für die Softwarepakete der privaten Cloud behalten eine Version der Software des neuesten Release des Softwarepakets von VMware bei. Die Softwareversionen der privaten Cloud können sich von den aktuellen Versionen der einzelnen Softwarekomponenten (ESXi, NSX-T, vCenter und vSAN) unterscheiden. Updates umfassen auch Treiber, Software auf den Netzwerkswitches und Firmware auf den Bare-Metal-Knoten.

Sie werden benachrichtigt, bevor und nachdem Patches auf Ihre privaten Clouds angewendet werden. Wir arbeiten auch mit Ihnen zusammen, um ein Wartungsfenster zu planen, bevor wir Updates oder Upgrades auf Ihre private Cloud anwenden. 

Softwareupdates umfassen:

- **Patches**: Sicherheitspatches oder Fehlerbehebungen, die von VMware freigegeben werden
- **Updates**: Änderungen der Nebenversion einer VMware-Stack-Komponente
- **Upgrades**: Änderungen der Hauptversion einer VMware-Stack-Komponente

>[!NOTE]
>Microsoft testet einen wichtigen Sicherheitspatch, sobald er von VMware zur Verfügung gestellt wird.

Bis die Bereitstellung der nächsten geplanten Updates erfolgt, werden dokumentierte VMware-Problemumgehungen implementiert, statt einen entsprechenden Patch zu installieren.
