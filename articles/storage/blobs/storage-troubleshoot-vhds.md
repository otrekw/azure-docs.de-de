---
title: Problembehandlung bei an virtuelle Azure-Computer angeschlossenen Datenträgern | Microsoft-Dokumentation
description: Enthält Links zu Problembehandlungsressourcen für virtuelle Festplatten (VHDs) virtueller Azure-Computer.
services: storage
author: roygara
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: rogarana
ms.reviewer: wmgries
ms.openlocfilehash: b81c0947327dc8e84c5d3fbbecde3aa31313cc70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061180"
---
# <a name="troubleshoot-disks-attached-to-azure-vms"></a>Problembehandlung bei an virtuelle Azure-Computer angeschlossenen Datenträgern 

Virtuelle Azure-Computer (VMs) nutzen virtuelle Festplatten (VHDs) als Betriebssystemdatenträger und angefügte Datenträger. VHDs werden als Seitenblobs in einem oder mehreren Azure Storage-Konten gespeichert. Dieser Artikel verweist auf Inhalte zur Problembehandlung bei allgemeinen Problemen, die bei VHDs auftreten können. 

## <a name="troubleshoot-storage-deletion-errors-for-a-vm"></a>Problembehandlung bei Speicherlöschfehlern für eine VM

In bestimmten Fällen kann ein Fehler beim Löschen einer Speicherressource auftreten, wenn ein virtueller Computer in einer Resource Manager-Bereitstellung angefügte VHDs enthält. Hilfe zum Beheben dieses Problems finden Sie in den folgenden Artikeln: 

  * Bei Linux-VMs: [Speicherbezogene Fehler beim Löschen in der Resource Manager-Bereitstellung](../../virtual-machines/linux/storage-resource-deletion-errors.md)  
  * Bei Windows-VMs: [Speicherbezogene Fehler beim Löschen in der Resource Manager-Bereitstellung](../../virtual-machines/windows/storage-resource-deletion-errors.md)  

## <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Problembehandlung bei unerwarteten Neustarts von virtuellen Computern mit angefügten VHDs

Wenn unerwartete Neustarts eines virtuellen Computers mit einer großen Anzahl angefügter VHDs auftreten, lesen Sie folgende Artikel:

  * Bei Linux-VMs: [Unerwartete Neustarts von virtuellen Computern mit angefügten VHDs](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
  * Bei Windows-VMs: [Unerwartete Neustarts von virtuellen Computern mit angefügten VHDs](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
