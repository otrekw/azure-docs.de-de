---
title: Verwaltetes Datenträgerbursting
description: Hier erfahren Sie mehr über Datenträgerbursting für Azure-Datenträger und Datenträgerbursting für virtuelle Azure-Computer.
author: albecker1
ms.author: albecker
ms.date: 06/02/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: f92fae38d49c51dfe87a68b023ba779e89b0e0bc
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2020
ms.locfileid: "84295455"
---
# <a name="disk-bursting"></a>Datenträgerbursting
[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting.md)]

## <a name="virtual-machine-level-bursting"></a>Bursting auf VM-Ebene
Burstingunterstützung auf VM-Ebene ist in allen Regionen in der öffentlichen Cloud für die folgenden unterstützten Größen aktiviert: 
- [Lsv2-Serie](../lsv2-series.md)

Bursting ist für virtuelle Computer, die diese Technologie unterstützen, standardmäßig aktiviert.

## <a name="disk-level-bursting"></a>Bursting auf Datenträgerebene
Bursting steht auch für unsere [Premium-SSDs](disks-types.md#premium-ssd) für die Datenträgergrößen P20 und kleiner in allen Regionen („Azure, öffentlich“, Government und China-Clouds) zur Verfügung. Das Datenträgerbursting ist standardmäßig in allen neuen und vorhandenen Bereitstellungen der unterstützten Datenträgergrößen aktiviert. 

[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting-2.md)]
