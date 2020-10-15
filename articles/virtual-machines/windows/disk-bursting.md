---
title: Verwaltetes Datenträgerbursting
description: Hier erfahren Sie mehr über Datenträgerbursting für Azure-Datenträger und Datenträgerbursting für virtuelle Azure-Computer.
author: albecker1
ms.author: albecker
ms.date: 09/22/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 547834542b605b226ebffd68e05296ee847dc6de
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91974327"
---
# <a name="disk-bursting"></a>Datenträgerbursting
[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting.md)]

## <a name="virtual-machine-level-bursting"></a>Bursting auf VM-Ebene
Burstingunterstützung auf VM-Ebene ist in allen Regionen in der öffentlichen Cloud für die folgenden unterstützten Größen aktiviert: 
- [Lsv2-Serie](../lsv2-series.md)

Bursting auf VM-Ebene ist auch in „USA, Westen-Mitte“ für die folgenden unterstützten Größen erhältlich:
- [Dsv3-Serie](../dv3-dsv3-series.md)
- [Esv3-Serie](../ev3-esv3-series.md)

Bursting ist für virtuelle Computer, die diese Technologie unterstützen, standardmäßig aktiviert.

## <a name="disk-level-bursting"></a>Bursting auf Datenträgerebene
Bursting steht auch für unsere [Premium-SSDs](../disks-types.md#premium-ssd) für Datenträgergrößen P20 und kleiner in allen Regionen zur Verfügung. Das Datenträgerbursting ist bei neuen Bereitstellungen der unterstützten Datenträgergrößen standardmäßig aktiviert. Bei vorhandenen Datenträgergrößen, die das Datenträgerbursting unterstützen, kann die Funktion durch eine der folgenden Methoden aktiviert werden: 
- **Neustarten der VM** 
- **Trennen und erneutes Anfügen des Datenträgers**


[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting-2.md)]