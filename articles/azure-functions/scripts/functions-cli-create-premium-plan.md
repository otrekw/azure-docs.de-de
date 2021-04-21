---
title: Erstellen einer Funktions-App in einem Premium-Tarif – Azure-Befehlszeilenschnittstelle
description: Erstellen einer Funktions-App in einem skalierbaren Premium-Tarif in Azure mithilfe der Azure-Befehlszeilenschnittstelle
ms.service: azure-functions
ms.topic: sample
ms.date: 11/23/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: 7d9f72fa433364f8d71ba44207d570bb827cd243
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107786201"
---
# <a name="create-a-function-app-in-a-premium-plan---azure-cli"></a>Erstellen einer Funktions-App in einem Premium-Tarif – Azure-Befehlszeilenschnittstelle

Dieses Azure Functions-Beispielskript erstellt eine Funktions-App, die als Container für Ihre Funktionen fungiert. Die erstellte Funktions-App verwendet einen [skalierbaren Premium-Tarif](../functions-premium-plan.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Für dieses Tutorial ist mindestens Version 2.0 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="sample-script"></a>Beispielskript

Dieses Skript erstellt eine Funktions-App mit einem [Premium-Tarif](../functions-premium-plan.md).

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-premium-plan/create-function-app-premium-plan.sh "Create an Azure Function on an App Service plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft. Das Skript verwendet die folgenden Befehle:

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | Erstellt ein Azure Storage-Konto. |
| [az functionapp plan create](/cli/azure/functionapp/plan#az_functionapp_plan_create) | Erstellt einen Premium-Tarif in einer [bestimmten SKU](../functions-premium-plan.md#available-instance-skus). |
| [az functionapp create](/cli/azure/functionapp#az_functionapp_create) | Erstellt eine Funktions-App im App Service-Plan. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).
