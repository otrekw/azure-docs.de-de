---
title: 'Kopieren der Momentaufnahme des verwalteten Datenträgers in ein Abonnement (Windows): PowerShell'
description: Azure PowerShell-Beispielskript – Kopieren (Verschieben) einer Momentaufnahme eines verwalteten Datenträgers in das gleiche oder ein anderes Abonnement
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
ms.date: 02/28/2019
ms.author: ramankum
ms.openlocfilehash: a2fd6f8b5f2ba0771449178a2d8d3958329e83eb
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89651601"
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell-windows"></a>Kopieren einer Momentaufnahme eines verwalteten Datenträgers in das gleiche oder ein anderes Abonnement mit PowerShell (Windows)

Dieses Skript kopiert eine Momentaufnahme eines verwalteten Datenträgers in dasselbe oder ein anderes Abonnement. Verwenden Sie dieses Skript für folgende Szenarien:

1. Migrieren einer Momentaufnahme in Storage Premium (Premium_LRS) zu Storage Standard (Standard_LRS oder Standard_ZRS), um Ihre Kosten zu senken
1. Migrieren einer Momentaufnahme aus lokal redundantem Speicher (Premium_LRS, Standard_LRS) zu zonenredundantem Speicher (Standard_ZRS), um von der höheren Zuverlässigkeit von ZRS-Speicher zu profitieren
1. Verschieben einer Momentaufnahme in ein anderes Abonnement in der gleichen Region zur längeren Aufbewahrung

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen einer Momentaufnahme im Zielabonnement mit der ID der Quellmomentaufnahme. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzSnapshotConfig](/powershell/module/az.compute/new-azsnapshotconfig) | Erstellt die Momentaufnahmenkonfiguration, die für die Erstellung der Momentaufnahme verwendet wird. Enthält die Ressourcen-ID der übergeordneten Momentaufnahme und des Speicherorts, der mit dem Speicherort der übergeordneten Momentaufnahme übereinstimmt.  |
| [New-AzSnapshot](/powershell/module/az.compute/new-azsnapshot) | Erstellt eine Momentaufnahme mit Momentaufnahmenkonfiguration, Momentaufnahmenname und Name der Ressourcengruppe, die als Parameter übergeben werden. |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines virtuellen Computers aus einer Momentaufnahme](./virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
