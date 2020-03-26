---
title: Übersicht über Azure Disk Storage für Windows-VMs
description: Enthält eine Übersicht über verwaltete Azure-Datenträger. Hiermit werden bei Verwendung von Azure-Windows-VMs die Speicherkonten für Sie verwaltet.
author: roygara
ms.service: virtual-machines-windows
ms.topic: overview
ms.date: 12/02/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 7b2c2c1289a40d63b2f396ee59000c3aedb14c3d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "75460030"
---
# <a name="introduction-to-azure-managed-disks"></a>Einführung in verwaltete Azure-Datenträger

Verwaltete Azure-Datenträger sind Speichervolumes auf Blockebene, die von Azure verwaltet und mit Azure-Virtual Machines verwendet werden. Verwaltete Datenträger sind wie physische Datenträger in einem lokalen Server, aber der Unterschied besteht darin, dass sie virtualisiert sind. Bei verwalteten Datenträgern müssen Sie lediglich die Datenträgergröße sowie den Datenträgertyp angeben und den Datenträger bereitstellen. Nachdem Sie den Datenträger bereitgestellt haben, übernimmt Azure den Rest.

Die verfügbaren Datenträgertypen sind Ultra-Datenträger, SSD Premium, SSD Standard und Standard Hard Disk Drives (HDD). Informationen zu den einzelnen Datenträgertypen finden Sie unter [Auswählen eines Datenträgertyps für virtuelle IaaS-Computer](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Auswählen eines Datenträgertyps für virtuelle IaaS-Computer](disks-types.md)
