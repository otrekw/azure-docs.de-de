---
title: 'VHD-Datei des verwalteten Datenträgers in das Konto einer anderen Region (Linux): PowerShell'
description: Azure PowerShell-Skriptbeispiel – Exportieren/Kopieren der VHD eines verwalteten Datenträgers in ein Speicherkonto in derselben oder einer anderen Region
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/17/2018
ms.author: ramankum
ms.openlocfilehash: 125779fcc547aa725f8c218663be23c177b42b97
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87028633"
---
# <a name="exportcopy-the-vhd-of-a-managed-disk-to-a-storage-account-in-different-region-with-powershell-linux"></a>Exportieren/Kopieren der VHD eines verwalteten Datenträgers in ein Speicherkonto in einer anderen Region mit PowerShell (Linux)

Mit diesem Skript wird die VHD eines verwalteten Datenträgers in ein Speicherkonto in einer anderen Region exportiert. Zuerst wird der SAS-URI des verwalteten Datenträgers generiert und anschließend verwendet, um die zugrunde liegende VHD in ein Speicherkonto in einer anderen Region zu kopieren. Verwenden Sie dieses Skript, um verwaltete Datenträger zur regionalen Erweiterung in eine andere Region zu kopieren.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-managed-disks-vhd-to-storage-account/copy-managed-disks-vhd-to-storage-account.ps1 "Copy the VHD of a managed disk")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Generieren des SAS-URI eines verwalteten Datenträgers und zum Kopieren der zugrunde liegenden VHD in ein Speicherkonto mithilfe des SAS-URI. Jeder Befehl in der Tabelle verfügt über einen Link zur zugehörigen Dokumentation.

| Get-Help | Notizen |
|---|---|
| [Grant-AzDiskAccess](/powershell/module/az.compute/grant-azdiskaccess) | Generiert einen SAS-URI für einen verwalteten Datenträger, der zum Kopieren der zugrunde liegenden VHD in ein Speicherkonto verwendet wird. |
| [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) | Erstellt einen Speicherkontokontext, der den Kontonamen und -schlüssel verwendet. Dieser Kontext kann verwendet werden, um Lese-/Schreibvorgänge im Speicherkonto auszuführen. |
| [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) | Kopiert die zugrunde liegende VHD einer Momentaufnahme in ein Speicherkonto. |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen verwalteter Datenträger aus einer VHD](virtual-machines-linux-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Erstellen eines virtuellen Computers aus einem verwalteten Datenträger](./virtual-machines-linux-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
