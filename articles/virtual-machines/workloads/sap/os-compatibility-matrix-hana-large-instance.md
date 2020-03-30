---
title: Betriebssystem-Kompatibilitätsmatrix für SAP HANA (große Instanzen) | Microsoft-Dokumentation
description: In der Kompatibilitätsmatrix wird die Kompatibilität verschiedener Betriebssystemversionen mit verschiedenen Hardwaretypen (große Instanzen) dargestellt.
services: virtual-machines-linux
documentationcenter: ''
author: sasarava
manager: hrushib
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/03/2020
ms.author: sasarava
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: aa19433ef6446932da3509694ccccd08538b964f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78675639"
---
# <a name="compatible-operating-systems-for-hana-large-instances"></a>Kompatible Betriebssysteme für HANA (große Instanzen)

## <a name="hana-large-instance-type-i"></a>Große HANA-Instanz, Typ I     
  | Betriebssystem | Verfügbarkeit        | SKUs                                                          |
  |------------------|---------------------|---------------------------------------------------------------|
  | SLES 12 SP2      | Wird nicht mehr angeboten | S72, S72m, S96, S144, S144m, S192, S192m, S192xm              |
  | SLES 12 SP3      | Verfügbar           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm              |
  | SLES 12 SP4      | Verfügbar           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224, S224m |
  
### <a name="persistent-memory-skus"></a>SKUs für persistenten Speicher
  | Betriebssystem | Verfügbarkeit | SKUs                             |
  |------------------|--------------|----------------------------------|
  | SLES 12 SP4      | Verfügbar    | S224oo, S224om, S224ooo, S224oom |
  
## <a name="hana-large-instance-type-ii"></a>Große HANA-Instanz, Typ II     
  |  Betriebssystem       | Verfügbarkeit        | SKUs                                                              |
  |-------------------------|---------------------|-------------------------------------------------------------------|
  | SLES 12 SP2             | Wird nicht mehr angeboten | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m |
  | SLES 12 SP3             | Verfügbar           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m |
  
## <a name="related-documents"></a>Verwandte Dokumente

- Weitere Informationen zu verfügbaren SKUs finden Sie [hier](hana-available-skus.md).
- Informationen zum Upgraden des Betriebssystems finden Sie [hier](os-upgrade-hana-large-instance.md).
  

  
  
