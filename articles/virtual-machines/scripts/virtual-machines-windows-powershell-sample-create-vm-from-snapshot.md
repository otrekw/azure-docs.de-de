---
title: 'Erstellen eines virtuellen Computers auf der Grundlage einer Momentaufnahme (Windows): PowerShell-Beispiel'
description: Azure PowerShell-Skriptbeispiel – Erstellen eines virtuellen Computers aus einer Momentaufnahme
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankumarlive
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 8ec7b8655a499057e91f138411529a5f271ea511
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87082355"
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-powershell-windows"></a>Erstellen eines virtuellen Computers auf der Grundlage einer Momentaufnahme mit PowerShell (Windows)

Dieses Skript erstellt einen virtuellen Computer aus einer Momentaufnahme eines Betriebssystemdatenträgers. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from managed os disk")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um die Eigenschaften einer Momentaufnahme abzurufen, einen verwalteten Datenträger aus einer Momentaufnahme zu erstellen und einen virtuellen Computer zu erstellen. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzSnapshot](/powershell/module/az.compute/get-azsnapshot) | Ruft eine Momentaufnahme unter Verwendung ihres Namens ab. |
| [New-AzDiskConfig](/powershell/module/az.compute/new-azdiskconfig) | Erstellt eine Datenträgerkonfiguration. Diese Konfiguration wird mit dem Prozess der Erstellung des Datenträgers verwendet. |
| [New-AzDisk](/powershell/module/az.compute/new-azdisk) | Erstellt einen verwalteten Datenträger. |
| [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig) | Erstellt eine VM-Konfiguration. Diese Konfiguration umfasst Informationen wie VM-Name, Betriebssystem und Administratoranmeldeinformationen. Die Konfiguration wird während der VM-Erstellung verwendet. |
| [Set-AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk) | Fügt den verwalteten Datenträger als Betriebssystemdatenträger an den virtuellen Computer an. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Erstellt eine öffentliche IP-Adresse. |
| [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Erstellt eine Netzwerkschnittstelle. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Erstellt einen virtuellen Computer. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
