---
title: Azure PowerShell-Skriptbeispiel – Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken | Microsoft-Dokumentation
description: Erstellen Sie zwei virtuelle Netzwerke in derselben Region, und stellen Sie eine Verbindung zwischen diesen her. Verwenden Sie das Azure-Skript für zwei virtuelle Peernetzwerke, um die Netzwerke über das Azure-Netzwerk zu verbinden.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: kumud
ms.openlocfilehash: 135a91d24220cee2a3e902941e5ade3fd3fc84a7
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288834"
---
# <a name="peer-two-virtual-networks"></a>Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken

Dieses Skript erstellt zwei virtuelle Netzwerke in der gleichen Region und stellt über das Azure-Netzwerk eine Verbindung zwischen ihnen her. Nach dem Ausführen des Skripts erstellen Sie ein Peering zwischen zwei virtuellen Netzwerken.

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](https://docs.microsoft.com/powershell/azure/), und führen Sie dann `Connect-AzAccount` aus, um eine Verbindung mit Azure herzustellen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell[main](../../../powershell_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.ps1 "Peer two networks")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. | 
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)| Erstellt ein virtuelles Azure-Netzwerk und ein Subnetz. |
| [Add-AzVirtualNetworkPeering](/powershell/module/az.network/add-azvirtualnetworkpeering) | Erstellt ein Peering zwischen virtuellen Netzwerken.  |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azure/).

Zusätzliche Netzwerk-PowerShell-Skriptbeispiele finden Sie unter [Azure-Dokumentation für Netzwerke – Übersicht](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
