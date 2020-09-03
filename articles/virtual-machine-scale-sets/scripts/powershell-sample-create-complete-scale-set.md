---
title: 'Azure PowerShell-Beispiele: Erstellen einer vollständigen VM-Skalierungsgruppe'
description: Mit diesem Skript wird eine VM-Skalierungsgruppe mit Windows Server 2016 erstellt, in der einzelne Ressourcen konfiguriert und erstellt werden.
author: mimckitt
ms.author: mimckitt
ms.topic: sample
ms.service: virtual-machine-scale-sets
ms.subservice: powershell
ms.date: 05/29/2018
ms.reviewer: jushiman
ms.custom: mimckitt, devx-track-azurepowershell
ms.openlocfilehash: d37f5f624459db6bc336884987a16c60503492a8
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89078502"
---
# <a name="create-a-complete-virtual-machine-scale-set-with-powershell"></a>Erstellen einer vollständigen VM-Skalierungsgruppe mit PowerShell

Dieses Skript erstellt eine VM-Skalierungsgruppe unter Windows Server 2016. Statt die [hier in „New-AzVmss“ verfügbaren integrierten Optionen zur Ressourcenerstellung](powershell-sample-create-simple-scale-set.md) zu verwenden, werden einzelne Ressourcen konfiguriert und erstellt. Nach dem Ausführen des Skripts können Sie per RDP auf die VM-Instanzen zugreifen.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/complete-scale-set/complete-scale-set.ps1 "Create a complete virtual machine scale set")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, die Skalierungsgruppe und alle dazugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="script-explanation"></a>Erläuterung des Skripts
Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Erstellt eine Subnetzkonfiguration. Diese Konfiguration wird mit dem Prozess der Erstellung des virtuellen Netzwerks verwendet. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Erstellen Sie ein virtuelles Netzwerk. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Erstellt eine öffentliche IP-Adresse. |
| [New-AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig) | Erstellt eine Front-End-IP-Konfiguration für ein Lastenausgleichsmodul. |
| [New-AzLoadBalancerBackendAddressPoolConfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) | Erstellt eine Back-End-Adresspoolkonfiguration für einen Lastenausgleich |
| [New-AzLoadBalancerInboundNatRuleConfig](/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig) | Erstellt eine eingehende NAT-Regelkonfiguration für einen Lastenausgleich. |
| [New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer) | Erstellt ein Lastenausgleichsmodul. |
| [Add-AzLoadBalancerProbeConfig](/powershell/module/az.network/new-azloadbalancerprobeconfig) | Erstellt eine Testkonfiguration für ein Lastenausgleichsmodul. |
| [Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/new-azloadbalancerruleconfig) | Erstellt eine Regelkonfiguration für ein Lastenausgleichsmodul. |
| [Set-AzLoadBalancer](/powershell/module/az.Network/Set-azLoadBalancer) | Aktualisiert den Lastenausgleich mit den angegebenen Informationen. |
| [New-AzVmssIpConfig](/powershell/module/az.Compute/New-azVmssIpConfig) | Erstellt eine IP-Konfiguration für die VM-Instanzen der Skalierungsgruppe. Die VM-Instanzen werden mit dem Back-End-Pool des Lastenausgleichs, dem NAT-Pool und dem Subnetz des virtuellen Netzwerks verbunden. |
| [New-AzVmssConfig](/powershell/module/az.Compute/New-azVmssConfig) | Erstellt eine Skalierungsgruppenkonfiguration. Diese Konfiguration enthält Informationen wie die Anzahl der zu erstellenden VM-Instanzen, die VM-SKU (Größe) und den Upgraderichtlinienmodus. Die Konfiguration wird mithilfe zusätzlicher Cmdlets ergänzt und bei der Erstellung der Skalierungsgruppe verwendet. |
| [Set-AzVmssStorageProfile](/powershell/module/az.Compute/Set-azVmssStorageProfile) | Definiert das Image, das für die VM-Instanzen verwendet werden soll, und fügt es der Skalierungsgruppenkonfiguration hinzu. |
| [Set-AzVmssOsProfile](/powershell/module/az.Compute/Set-azVmssStorageProfile) | Definiert die Anmeldeinformationen für Administratorbenutzername und -kennwort und das VM-Namenspräfix. Fügen Sie diese Werte der Skalierungsgruppenkonfiguration hinzu. |
| [Add-AzVmssNetworkInterfaceConfiguration](/powershell/module/az.Compute/Add-azVmssNetworkInterfaceConfiguration) | Fügt den VM-Instanzen basierend auf der IP-Konfiguration eine virtuelle Netzwerkschnittstelle hinzu. Fügen Sie diese Werte der Skalierungsgruppenkonfiguration hinzu. |
| [New-AzVmss](/powershell/module/az.Compute/New-azVmss) | Erstellt anhand der Informationen in der Skalierungsgruppenkonfiguration die Skalierungsgruppe. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).
