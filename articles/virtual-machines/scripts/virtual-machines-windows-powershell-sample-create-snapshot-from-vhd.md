---
title: 'Erstellen einer Momentaufnahme auf der Grundlage einer VHD, um mehrere identische verwaltete Datenträger zu erstellen: PowerShell-Beispiel'
description: 'Azure PowerShell-Skript-Beispiel: Erstellen einer Momentaufnahme aus einer VHD, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen'
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 9da8adb786baebcb2e798c7ffb5998aca0f68265
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81459252"
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>Erstellen einer Momentaufnahme aus einer VHD, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen

Dieses Skript erstellt eine Momentaufnahme aus einer VHD-Datei in einem Speicherkonto in demselben oder einem anderen Abonnement. Verwenden Sie dieses Skript, um eine spezialisierte (nicht generalisierte/sysprepped) VHD in eine Momentaufnahme zu importieren. Verwenden Sie dann die Momentaufnahme, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen. Außerdem verwenden Sie es zum Importieren einer Daten-VHD in eine Momentaufnahme und dann verwenden Sie die Momentaufnahme, um mehrere identisch verwaltete Datenträger in kürzester Zeit zu erstellen. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]


## <a name="next-steps"></a>Nächste Schritte

[Erstellen Sie einen verwalteten Datenträger aus der Momentaufnahme](virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[Erstellen eines virtuellen Computers mit einem vorhandenen verwalteten Betriebssystemdatenträger mit PowerShell](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
