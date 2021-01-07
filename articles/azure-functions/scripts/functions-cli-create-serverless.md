---
title: Erstellen einer serverlosen Funktions-App mit der Azure CLI
description: Erstellen Sie mithilfe der Azure CLI eine Funktions-App für die serverlose Ausführung in Azure.
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.topic: sample
ms.date: 07/03/2018
ms.author: glenga
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 3a52d7ea8c940a33f4fbd2b9ad69f4f889615e7f
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2021
ms.locfileid: "97934338"
---
# <a name="create-a-function-app-for-serverless-code-execution"></a>Erstellen einer Funktions-App für die serverlose Codeausführung 

Dieses Azure Functions-Beispielskript erstellt eine Funktions-App, die als Container für Ihre Funktionen fungiert. Die Funktions-App wird unter Verwendung des [Verbrauchsplans](../consumption-plan.md) erstellt, der sich ideal für ereignisgesteuerte serverlose Workloads eignet.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Für dieses Tutorial ist mindestens Version 2.0 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert. 

## <a name="sample-script"></a>Beispielskript

Dieses Skript erstellt eine Azure-Funktions-App unter Verwendung des [Verbrauchsplans](../consumption-plan.md).

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "Create an Azure Function on a Consumption plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft. Das Skript verwendet die folgenden Befehle:

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Erstellt ein Azure Storage-Konto. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Erstellt eine Funktionen-App. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).
