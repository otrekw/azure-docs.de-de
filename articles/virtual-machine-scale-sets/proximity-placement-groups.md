---
title: Näherungsplatzierungsgruppen (Vorschau) für VM-Skalierungsgruppen
description: Erfahren Sie mehr über das Erstellen und Verwenden von Näherungsplatzierungsgruppen für Windows-VM-Skalierungsgruppen in Azure.
author: cynthn
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/01/2019
ms.author: cynthn
ms.openlocfilehash: 4fa2949e2a7e1b99ac26caa35f967e9dc9cf359a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76273618"
---
# <a name="preview-creating-and-using-proximity-placement-groups-using-powershell"></a>Vorschau: Erstellen und Verwenden von Näherungsplatzierungsgruppen mit PowerShell

Um den Abstand zwischen den VMs so stark wie möglich zu verringern und somit die geringstmögliche Latenz zu erzielen, sollten Sie Ihre Skalierungsgruppe in einer [Näherungsplatzierungsgruppe](co-location.md#preview-proximity-placement-groups) bereitstellen.

Eine Näherungsplatzierungsgruppe ist eine logische Gruppierung, mit der ein möglichst geringer Abstand zwischen Azure-Compute-Ressourcen sichergestellt wird. Näherungsplatzierungsgruppen sind für Workloads hilfreich, die eine geringe Latenz erfordern.

> [!IMPORTANT]
> Näherungsplatzierungsgruppen sind zurzeit als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> Näherungsplatzierungsgruppen sind in der Vorschauversion in den folgenden Regionen nicht verfügbar: **Japan, Osten**, **Australien, Osten** und **Indien, Mitte**.


## <a name="create-a-proximity-placement-group"></a>Erstellen einer Näherungsplatzierungsgruppe
Erstellen Sie mithilfe des Cmdlets [New-AzProximityPlacementGroup ](https://docs.microsoft.com/powershell/module/az.compute/new-azproximityplacementgroup) eine Näherungsplatzierungsgruppe. 

```azurepowershell-interactive
$resourceGroup = "myPPGResourceGroup"
$location = "East US"
$ppgName = "myPPG"
New-AzResourceGroup -Name $resourceGroup -Location $location
$ppg = New-AzProximityPlacementGroup `
   -Location $location `
   -Name $ppgName `
   -ResourceGroupName $resourceGroup `
   -ProximityPlacementGroupType Standard
```

## <a name="list-proximity-placement-groups"></a>Auflisten von Näherungsplatzierungsgruppen

Mit dem Cmdlet [Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup) können Sie alle Näherungsplatzierungsgruppen auflisten.

```azurepowershell-interactive
Get-AzProximityPlacementGroup
```


## <a name="create-a-scale-set"></a>Erstellen einer Skalierungsgruppe

Erstellen Sie mit `-ProximityPlacementGroup $ppg.Id` eine Skalierung in der Näherungsplatzierungsgruppe, um auf die Näherungsplatzierungsgruppen-ID zu verweisen, wenn Sie mit [New-AzVMSS](https://docs.microsoft.com/powershell/module/az.compute/new-azvmss) die Skalierungsgruppe erstellen.

```azurepowershell-interactive
$scalesetName = "myVM"

New-AzVmss `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -VMScaleSetName $scalesetName `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic" `
  -ProximityPlacementGroup $ppg.Id
```

Sie können die Instanz in der Platzierungsgruppe mit [Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup) anzeigen.

```azurepowershell-interactive
  Get-AzProximityPlacementGroup `
   -ResourceId $ppg.Id | Format-Table `
   -Wrap `
   -Property VirtualMachineScaleSets
```

## <a name="next-steps"></a>Nächste Schritte

Sie können auch die [Azure CLI](../virtual-machines/linux/proximity-placement-groups.md) zum Erstellen von Näherungsplatzierungsgruppen verwenden.
