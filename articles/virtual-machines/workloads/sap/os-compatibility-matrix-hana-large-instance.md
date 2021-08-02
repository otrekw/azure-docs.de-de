---
title: Betriebssystem-Kompatibilitätsmatrix für SAP HANA (große Instanzen) | Microsoft-Dokumentation
description: In der Kompatibilitätsmatrix wird die Kompatibilität verschiedener Betriebssystemversionen mit verschiedenen Hardwaretypen (große Instanzen) dargestellt.
services: virtual-machines-linux
documentationcenter: ''
author: Ajayan1008
manager: hrushib
editor: ''
ms.service: virtual-machines-sap
ms.subservice: baremetal-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/18/2021
ms.author: madhukan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ad1eb1428493d8488e803560e0dc421765aab465
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110579444"
---
# <a name="compatible-operating-systems-for-hana-large-instances"></a>Kompatible Betriebssysteme für HANA (große Instanzen)

## <a name="hana-large-instance-type-i"></a>Große HANA-Instanz, Typ I     
  | Betriebssystem | Verfügbarkeit        | SKUs                                                          |
  |------------------|---------------------|---------------------------------------------------------------|
  | SLES 12 SP2      | Wird nicht mehr angeboten | S72, S72m, S96, S144, S144m, S192, S192m, S192xm              |
  | SLES 12 SP3      | Verfügbar           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm              |
  | SLES 12 SP4      | Verfügbar           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224, S224m |
  | SLES 12 SP5      | Verfügbar           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224, S224m |
  | SLES 15 SP1      | Verfügbar           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224, S224m |
  | RHEL 7.6         | Verfügbar           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224, S224m |

  
### <a name="persistent-memory-skus"></a>SKUs für persistenten Speicher

  | Betriebssystem | Verfügbarkeit | SKUs                             |
  |------------------|--------------|----------------------------------|
  | SLES 12 SP4      | Verfügbar    | S224oo, S224om, S224ooo, S224oom |
  
## <a name="hana-large-instance-type-ii"></a>Große HANA-Instanz, Typ II     
  |  Betriebssystem       | Verfügbarkeit        | SKUs                                                                     |
  |-------------------------|---------------------|--------------------------------------------------------------------------|
  | SLES 12 SP2             | Wird nicht mehr angeboten | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m        |
  | SLES 12 SP3             | Verfügbar           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m        |
  | SLES 12 SP4             | Verfügbar           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m        |
  | SLES 12 SP5             | Verfügbar           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S896m, S960m |
  | SLES 15 SP1             | Verfügbar           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S896m, S960m |
  | RHEL 7.6                | Verfügbar           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S896m, S960m |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:

- [Verfügbare SKUs](hana-available-skus.md)
- [Upgraden des Betriebssystems](os-upgrade-hana-large-instance.md)
- [Unterstützte Szenarien für große HANA-Instanzen](hana-supported-scenario.md)
  