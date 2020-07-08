---
title: 'Abrufen der verwalteten Ressourcengruppe und Ändern der Größe von virtuellen Computern: Azure CLI'
description: Hier finden Sie ein Azure CLI-Beispielskript, mit dem eine verwaltete Ressourcengruppe in einer verwalteten Azure-Anwendung abgerufen wird. Das Skript ändert die Größe der virtuellen Computer.
author: tfitzmac
ms.devlang: azurecli
ms.topic: sample
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: f119fe4b4547bda8249a3620baf938dd8b83c235
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86056039"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-azure-cli"></a>Abrufen von Ressourcen in einer verwalteten Ressourcengruppe und Ändern der Größe von virtuellen Computern mit der Azure CLI

Dieses Skript ruft Ressourcen aus einer verwalteten Ressourcengruppe ab und ändert die Größe der virtuellen Computer in dieser Ressourcengruppe.


[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../../cli_scripts/managed-applications/get-application/get-application.sh "Get application")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Bereitstellen der verwalteten Anwendung. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az managedapp list](/cli/azure/managedapp#az-managedapp-list) | Auflisten der verwalteten Anwendungen. Geben Sie Abfragewerte an, um die Ergebnisse einzugrenzen. |
| [az resource list](/cli/azure/resource#az-resource-list) | Auflisten der Ressourcen. Geben Sie eine Ressourcengruppe und Abfragewerte an, um die Ergebnisse einzugrenzen. |
| [az vm resize](/cli/azure/vm#az-vm-resize) | Aktualisieren der Größe eines virtuellen Computers |


## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](../overview.md).
* Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).
