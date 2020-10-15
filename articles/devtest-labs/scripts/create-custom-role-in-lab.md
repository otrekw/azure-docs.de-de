---
title: 'PowerShell: Erstellen einer benutzerdefinierte Rolle in einem Lab in Azure DevTest Labs'
description: Dieser Artikel enthält ein Azure PowerShell-Skript, das eine benutzerdefinierte Rolle in einem Lab in Azure DevTest Labs erstellt.
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/11/2020
ms.openlocfilehash: 3757d6c16a762a0f76f4718f117da93322af074c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91252798"
---
# <a name="use-powershell-to-create-a-custom-role-in-a-lab-in-azure-devtest-labs"></a>Erstellen einer benutzerdefinierte Rolle in einem Lab in Azure DevTest Labs mithilfe von PowerShell

Dieses PowerShell-Beispielskript erstellt eine benutzerdefinierte Rolle für ein Lab in Azure DevTest Labs. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Voraussetzungen
* **Ein Lab.** Das Skript erfordert ein vorhandenes Lab. 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-role-in-lab/create-custom-role-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) | Ruft die Vorgänge für einen Azure-Ressourcenanbieter ab, die mit Azure RBAC abgesichert werden können. |
| [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) | Dient zum Auflisten aller Azure-Rollen, die für die Zuweisung verfügbar sind. |
| [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) | Dient zum Erstellen einer benutzerdefinierten Rolle. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Lab Services finden Sie unter [Azure Lab Services – Beispiele für PowerShell](../samples-powershell.md).
