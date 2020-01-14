---
title: Übersicht über Azure Disk Storage für Linux-VMs
description: Enthält eine Übersicht über Azure Managed Disks. Hiermit werden bei Verwendung von Linux-VMs die Speicherkonten für Sie verwaltet.
author: roygara
ms.service: virtual-machines-linux
ms.topic: overview
ms.date: 12/02/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 2b25da174399b092fe821a46b235d7a2bd14572b
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355914"
---
# <a name="introduction-to-azure-managed-disks"></a>Einführung in verwaltete Azure-Datenträger

Verwaltete Azure-Datenträger sind Speichervolumes auf Blockebene, die von Azure verwaltet und mit Azure-Virtual Machines verwendet werden. Verwaltete Datenträger sind wie physische Datenträger in einem lokalen Server, aber der Unterschied besteht darin, dass sie virtualisiert sind. Bei verwalteten Datenträgern müssen Sie lediglich die Datenträgergröße sowie den Datenträgertyp angeben und den Datenträger bereitstellen. Nachdem Sie den Datenträger bereitgestellt haben, übernimmt Azure den Rest.

Die verfügbaren Datenträgertypen sind Ultra-Datenträger, SSD Premium, SSD Standard und Standard Hard Disk Drives (HDD). Informationen zu den einzelnen Datenträgertypen finden Sie unter [Auswählen eines Datenträgertyps für virtuelle IaaS-Computer](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Auswählen eines Datenträgertyps für virtuelle IaaS-Computer](disks-types.md)