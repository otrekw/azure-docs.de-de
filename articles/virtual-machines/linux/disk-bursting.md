---
title: Verwaltetes Datenträgerbursting
description: Hier erfahren Sie mehr über Datenträgerbursting für Azure-Datenträger und Datenträgerbursting für virtuelle Azure-Computer.
author: albecker1
ms.author: albecker
ms.date: 04/27/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 25aa9fc166e831acd2ed0389bbbe4d2dc7e04b19
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82594745"
---
# <a name="disk-bursting"></a>Datenträgerbursting
[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting.md)]

## <a name="virtual-machine-level-bursting"></a>Bursting auf VM-Ebene
Burstingunterstützung auf VM-Ebene ist in allen Regionen in der öffentlichen Cloud für die folgenden unterstützten Größen aktiviert: 
- [Lsv2-Serie](../lsv2-series.md)

Bursting ist für virtuelle Computer, die diese Technologie unterstützen, standardmäßig aktiviert.

## <a name="disk-level-bursting"></a>Bursting auf Datenträgerebene
Bursting steht auch für unsere [Premium-SSDs](disks-types.md#premium-ssd) für Datenträgergrößen P20 und kleiner in allen Regionen zur Verfügung. Das Datenträgerbursting ist bei neuen Bereitstellungen der unterstützten Datenträgergrößen standardmäßig aktiviert. Bei vorhandenen Datenträgergrößen, die das Datenträgerbursting unterstützen, kann die Funktion durch eine der folgenden Methoden aktiviert werden: 
- **Neustarten der VM** 
- **Trennen und erneutes Anfügen des Datenträgers**


[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting-2.md)]
