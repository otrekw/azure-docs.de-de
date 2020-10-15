---
title: VHD-Momentaufnahme zum Erstellen vieler identischer verwalteter Datenträger (Windows) – PowerShell
description: 'Azure PowerShell-Skript-Beispiel: Erstellen einer Momentaufnahme aus einer VHD, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen'
documentationcenter: storage
author: ramankumarlive
manager: kavithag
ms.service: virtual-machines
ms.subservice: disks
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 47ade94ad2e0be0ce44d346336c6c7eea4fca78b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89322737"
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell-windows"></a>Erstellen einer Momentaufnahme aus einer VHD, um mehrere identisch verwaltete Datenträger in kürzester Zeit mit PowerShell zu erstellen (Windows)

Dieses Skript erstellt eine Momentaufnahme aus einer VHD-Datei in einem Speicherkonto in demselben oder einem anderen Abonnement. Verwenden Sie dieses Skript, um eine spezialisierte (nicht generalisierte/sysprepped) VHD in eine Momentaufnahme zu importieren. Verwenden Sie dann die Momentaufnahme, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen. Außerdem verwenden Sie es zum Importieren einer Daten-VHD in eine Momentaufnahme und dann verwenden Sie die Momentaufnahme, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]


## <a name="next-steps"></a>Nächste Schritte

[Erstellen Sie einen verwalteten Datenträger aus der Momentaufnahme](virtual-machines-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[Erstellen eines virtuellen Computers mit einem vorhandenen verwalteten Betriebssystemdatenträger mit PowerShell](./virtual-machines-powershell-sample-create-vm-from-managed-os-disks.md)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
