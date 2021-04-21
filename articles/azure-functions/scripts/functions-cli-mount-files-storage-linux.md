---
title: 'Einbinden einer Dateifreigabe in eine Python-Funktions-App: Azure CLI'
description: Erstellen Sie eine serverlose Python-Funktions-App, und binden Sie eine vorhandene Dateifreigabe mithilfe der Azure-Befehlszeilenschnittstelle ein.
ms.topic: sample
ms.date: 03/01/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: d0037cea24b1989c4f7a4d2ddd6bf3f8f7e812b3
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107762279"
---
# <a name="mount-a-file-share-to-a-python-function-app-using-azure-cli"></a>Einbinden einer Dateifreigabe in eine Python-Funktions-App mithilfe der Azure CLI

Dieses Azure Functions-Beispielskript erstellt eine Funktions-App und eine Freigabe in Azure Files. Anschließend wird die Freigabe eingebunden, sodass Ihre Funktionen auf die Daten zugreifen können.  

>[!NOTE]
>Die erstellte Funktions-App wird unter Python-Version 3.7 ausgeführt. Azure Functions [unterstützt auch die Python-Versionen 3.6 und 3.8](../functions-reference-python.md#python-version).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Für dieses Tutorial ist mindestens Version 2.0 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert. 

## <a name="sample-script"></a>Beispielskript

Dieses Skript erstellt unter Verwendung des [Verbrauchsplans](../consumption-plan.md) eine Funktions-App in Azure Functions.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/functions-cli-mount-files-storage-linux/functions-cli-mount-files-storage-linux.sh "Create a function app on a Consumption plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft. Das Skript verwendet die folgenden Befehle:

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | Erstellt ein Azure Storage-Konto. |
| [az functionapp create](/cli/azure/functionapp#az_functionapp_create) | Erstellt eine Funktionen-App. |
| [az storage share create](/cli/azure/storage/share#az_storage_share_create) | Erstellt eine Azure Files-Freigabe im Speicherkonto. | 
| [az storage directory create](/cli/azure/storage/directory#az_storage_directory_create) | Erstellt ein Verzeichnis in der Freigabe. |
| [az webapp config storage-account add](/cli/azure/webapp/config/storage-account#az_webapp_config_storage_account_add) | Bindet die Freigabe in die Funktions-App ein. |
| [az webapp config storage-account list](/cli/azure/webapp/config/storage-account#az_webapp_config_storage_account_list) | Zeigt die in die Funktions-App eingebundenen Dateifreigaben an. | 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).
