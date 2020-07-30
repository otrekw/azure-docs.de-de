---
title: 'Azure CLI: Beenden und Löschen eines virtuellen Computers in einem Lab'
description: Dieser Artikel enthält ein Azure CLI-Skript zum Beenden und Löschen eines virtuellen Computers in einem Lab in Azure DevTest Labs.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2020
ms.author: spelluru
ms.custom: mvc
ms.openlocfilehash: 3ba2b2c3924f7fea481907b825ba5a04ab50b0bf
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87290319"
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
