---
title: Azure PowerShell-Beispiele – Skalierungsgruppe mit nur einer Zone
description: Mit diesem Skript wird eine VM-Skalierungsgruppe erstellt, in der Windows Server 2016 in einer einzelnen Verfügbarkeitszone ausgeführt wird.
author: mimckitt
ms.author: mimckitt
ms.topic: sample
ms.service: virtual-machine-scale-sets
ms.subservice: availability
ms.date: 04/05/2018
ms.reviewer: jushiman
ms.custom: mimckitt, devx-track-azurepowershell
ms.openlocfilehash: dc76798ff97aa49d358b639557255b41872bab4f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89070903"
---
# <a name="create-a-single-zone-virtual-machine-scale-set-with-powershell"></a>Erstellen einer VM-Skalierungsgruppe mit nur einer Zone mit PowerShell
Mit diesem Skript wird eine VM-Skalierungsgruppe erstellt, in der Windows Server 2016 in einer einzelnen Verfügbarkeitszone ausgeführt wird. Nach dem Ausführen des Skripts können Sie per RDP auf den virtuellen Computer zugreifen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.ps1 "Create single-zone scale set")]

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
| [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) | Ruft Informationen zur zugewiesenen öffentlichen IP-Adresse ab, die vom Load Balancer verwendet wird. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).

