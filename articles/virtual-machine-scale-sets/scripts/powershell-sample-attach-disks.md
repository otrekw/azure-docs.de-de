---
title: 'Azure PowerShell-Beispiele: Anfügen und Verwenden von Datenträgern'
description: Dieses Skript erstellt eine Azure-VM-Skalierungsgruppe, fügt mit PowerShell Datenträger an und bereitet sie vor.
author: mimckitt
ms.author: mimckitt
ms.topic: sample
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 06/25/2020
ms.reviewer: jushiman
ms.custom: mimckitt, devx-track-azurepowershell
ms.openlocfilehash: 2a2c93578d8cb4b40e504dd83e3f603bffcccaf6
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "89069645"
---
# <a name="attach-and-use-data-disks-with-a-virtual-machine-scale-set-with-powershell"></a>Anfügen und Verwenden von Datenträgern mit einer VM-Skalierungsgruppe mit PowerShell
Dieses Skript erstellt eine VM-Skalierungsgruppe, fügt Datenträger an und bereitet sie vor.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/use-data-disks/use-data-disks.ps1 "Create a virtual machine scale set with data disks")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, die Skalierungsgruppe und alle dazugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts
Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzVmss](/powershell/module/az.compute/new-azvmss) | Erstellt die VM-Skalierungsgruppe und alle unterstützenden Ressourcen, einschließlich des virtuellen Netzwerks, des Lastenausgleichs und der NAT-Regeln. |
| [Get-AzVmss](/powershell/module/az.compute/get-azvmss) | Ruft Informationen zu einer VM-Skalierungsgruppe ab. |
| [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) | Fügt eine VM-Erweiterung für das benutzerdefinierte Skript hinzu, um eine einfache Webanwendung zu installieren. |
| [Update-AzVmss](/powershell/module/az.compute/update-azvmss) | Aktualisiert das Modell der VM-Skalierungsgruppe, um die VM-Erweiterung anzuwenden. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).
