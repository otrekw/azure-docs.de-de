---
title: 'Azure PowerShell-Beispiele: Erstellen einer einfachen VM-Skalierungsgruppe'
description: Dieses Skript erstellt eine Azure-VM-Skalierungsgruppe mit Windows Server 2016, auf die Sie über RDP zugreifen können.
author: mimckitt
ms.author: mimckitt
ms.topic: sample
ms.service: virtual-machine-scale-sets
ms.subservice: powershell
ms.date: 03/27/2018
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: 1093d6df3bef052df2dbe6df04f52d6a119357db
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87087083"
---
# <a name="create-a-basic-virtual-machine-scale-set-with-powershell"></a>Erstellen einer einfachen VM-Skalierungsgruppe mit PowerShell
Dieses Skript erstellt eine VM-Skalierungsgruppe unter Windows Server 2016. Nach dem Ausführen des Skripts können Sie per RDP auf die VM-Instanzen zugreifen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Beispielskript


[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/simple-scale-set/simple-scale-set.ps1 "Create a simple virtual machine scale set")]

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
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).
