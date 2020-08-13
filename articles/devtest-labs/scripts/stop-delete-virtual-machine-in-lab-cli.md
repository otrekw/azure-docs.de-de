---
title: 'Azure CLI: Beenden und Löschen eines virtuellen Computers in einem Lab'
description: Dieser Artikel enthält ein Azure CLI-Skript zum Beenden und Löschen eines virtuellen Computers in einem Lab in Azure DevTest Labs.
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/11/2020
ms.openlocfilehash: 45d5b3453e29571760d47d52a1a21288be31ea90
ms.sourcegitcommit: 1aef4235aec3fd326ded18df7fdb750883809ae8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2020
ms.locfileid: "88136121"
---
# <a name="use-azure-cli-to-stop-and-delete-a-virtual-machine-in-a-lab-in-azure-devtest-labs"></a>Beenden und Löschen eines virtuellen Computers in einem Lab in Azure DevTest Labs mithilfe der Azure CLI

Dieses Azure CLI-Skript beendet und löscht einen virtuellen Computer (virtual machine, VM) in einem Lab. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/stop-delete-virtual-machine-in-lab/stop-delete-virtual-machine-in-lab.sh "Stop and delete a VM in a lab")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle:

| Get-Help | Notizen |
|---|---|
| [az lab vm stop](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-stop) | Beendet einen virtuellen Computer (virtual machine, VM) in einem Lab. Dieser Vorgang kann eine Weile dauern. |
| [az lab vm delete](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-delete) | Löscht einen virtuellen Computer (virtual machine, VM) in einem Lab. Dieser Vorgang kann eine Weile dauern. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele für Azure Lab Services finden Sie unter [Azure Lab Services – Beispiele für die Befehlszeilenschnittstelle](../samples-cli.md).
