---
title: 'Azure CLI-Skriptbeispiel: Starten eines virtuellen Computers in einem Lab | Microsoft-Dokumentation'
description: Dieses Azure CLI-Skript startet einen virtuellen Computer in einem Lab in Azure DevTest Labs.
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
ms.date: 04/12/2018
ms.author: spelluru
ms.custom: mvc
ms.openlocfilehash: a38e0eacdba2c37a18c41f307e4a8120e0e6285d
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87290337"
---
# <a name="use-azure-cli-to-start-a-virtual-machine-in-a-lab-in-azure-devtest-labs"></a>Starten eines virtuellen Computers in einem Lab in Azure DevTest Labs mithilfe der Azure CLI

Dieses Azure CLI-Skript startet einen virtuellen Computer in einem Lab. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/start-connect-virtual-machine-in-lab/start-connect-virtual-machine-in-lab.sh "Start a VM")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle:

| Get-Help | Notizen |
|---|---|
| [az lab vm start](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-start) | Startet einen virtuellen Computer (virtual machine, VM) in einem Lab. Dieser Vorgang kann eine Weile dauern. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele für Azure Lab Services finden Sie unter [Azure Lab Services – Beispiele für die Befehlszeilenschnittstelle](../samples-cli.md).
