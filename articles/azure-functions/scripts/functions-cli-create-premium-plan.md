---
title: Erstellen einer Funktions-App in einem Premium-Tarif – Azure-Befehlszeilenschnittstelle
description: Erstellen einer Funktions-App in einem skalierbaren Premium-Tarif in Azure mithilfe der Azure-Befehlszeilenschnittstelle
ms.service: azure-functions
ms.topic: sample
ms.date: 11/23/2019
ms.openlocfilehash: 3587e5b5e46867ff3abe97b3593c9732be3afa22
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2020
ms.locfileid: "85117249"
---
# <a name="create-a-function-app-in-a-premium-plan---azure-cli"></a>Erstellen einer Funktions-App in einem Premium-Tarif – Azure-Befehlszeilenschnittstelle

Dieses Azure Functions-Beispielskript erstellt eine Funktions-App, die als Container für Ihre Funktionen fungiert. Die erstellte Funktions-App verwendet einen [skalierbaren Premium-Tarif](../functions-premium-plan.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittelle lokal installieren und verwenden möchten, erfordert dieses Thema mindestens die Azure CLI-Version 2.0. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Beispielskript

Dieses Skript erstellt eine Funktions-App mit einem [Premium-Tarif](../functions-premium-plan.md).

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-premium-plan/create-function-app-premium-plan.sh "Create an Azure Function on an App Service plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft. Das Skript verwendet die folgenden Befehle:

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Erstellt ein Azure Storage-Konto. |
| [az functionapp plan create](/cli/azure/functionapp/plan#az-functionapp-plan-create) | Erstellt einen Premium-Tarif in einer [bestimmten SKU](../functions-premium-plan.md#available-instance-skus). |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Erstellt eine Funktions-App im App Service-Plan. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).
