---
title: 'Erstellen eines virtuellen Computers auf der Grundlage einer Momentaufnahme (Linux): PowerShell-Beispiel'
description: Azure PowerShell-Skriptbeispiel – Erstellen eines virtuellen Computers aus einer Momentaufnahme
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankumarlive
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc, devx-track-azurepowershell
ms.openlocfilehash: 42c9903b783df4b3392b6aab3a69bbb66da90a75
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89078162"
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-powershell-linux"></a>Erstellen eines virtuellen Computers aus einer Momentaufnahme mit PowerShell (Linux)

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

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
