---
title: 'PowerShell: Hinzufügen eines Marketplace-Images zu einem Lab in Azure DevTest Labs'
description: Dieses PowerShell-Skript fügt einem Lab in Azure DevTest Labs ein Marketplace-Image hinzu.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2020
ms.author: spelluru
ms.openlocfilehash: 1c221f86d721a14f82f75d1cd9c81e37bba32a60
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87282618"
---
# <a name="use-powershell-to-add-a-marketplace-image-to-a-lab-in-azure-devtest-labs"></a>Hinzufügen eines Marketplace-Images zu einem Lab in Azure DevTest Labs mithilfe von PowerShell

Dieses PowerShell-Beispielskript fügt einem Lab in Azure DevTest Labs ein Marketplace-Image hinzu. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Voraussetzungen
* **Ein Lab.** Das Skript erfordert ein vorhandenes Lab. 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-marketplace-images-to-lab/add-marketplace-images-to-lab.ps1 "Add marketplace images to a lab")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| Find-AzResource | Dient zum Suchen nach Ressourcen basierend auf den angegebenen Parametern. |
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Dient zum Abrufen von Ressourcen. |
| [Set-AzResource](/powershell/module/az.resources/set-azresource) | Dient zum Ändern einer Ressource. |
| [New-AzResource](/powershell/module/az.resources/new-azresource) | Dient zum Erstellen einer Ressource. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Lab Services finden Sie unter [Azure Lab Services – Beispiele für PowerShell](../samples-powershell.md).
