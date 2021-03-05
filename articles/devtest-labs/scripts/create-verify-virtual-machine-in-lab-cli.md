---
title: 'Azure CLI: Erstellen und Überprüfen eines virtuellen Computers in einem Lab'
description: Dieses Azure CLI-Skript erstellt einen virtuellen Computer in einem Lab, und überprüft, ob er verfügbar ist.
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/11/2020
ms.openlocfilehash: c7625f62d7897d61903f864b216ccf9aa13648ea
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102198420"
---
# <a name="use-azure-cli-to-create-and-verify-availability-of-a-virtual-machine-in-a-lab-in-azure-devtest-labs"></a>Erstellen eines virtuellen Computers in einem Lab in Azure DevTest Labs und Überprüfen der Verfügbarkeit mithilfe der Azure CLI

Dieses Azure CLI-Skript erstellt einen virtuellen Computer in einem Lab. Der virtuelle Computer basiert auf einem Marketplace-Image mit SSH-Authentifizierung. Das Skript überprüft dann, ob der virtuelle Computer für die Verwendung verfügbar ist. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/create-verify-virtual-machine-in-lab/create-verify-virtual-machine-in-lab.sh "Create and verify availability of a VM")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle:

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az lab vm create](/cli/azure/lab/vm#az-lab-vm-create) | Erstellt einen virtuellen Computer (virtual machine, VM) in einem Lab. |
| [az lab vm show](/cli/azure/lab/vm#az-lab-vm-show) | Zeigt den Status des virtuellen Computers in einem Lab an. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele für Azure Lab Services finden Sie unter [Azure Lab Services – Beispiele für die Befehlszeilenschnittstelle](../samples-cli.md).
