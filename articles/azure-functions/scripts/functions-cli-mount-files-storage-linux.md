---
title: 'Einbinden einer Dateifreigabe in eine Python-Funktions-App: Azure CLI'
description: Erstellen Sie eine serverlose Python-Funktions-App, und binden Sie eine vorhandene Dateifreigabe mithilfe der Azure-Befehlszeilenschnittstelle ein.
ms.topic: sample
ms.date: 03/01/2020
ms.openlocfilehash: 3d66f84d124b36f1be335c3a2204f21690510ee8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "79086287"
---
# <a name="mount-a-file-share-to-a-python-function-app-using-azure-cli"></a>Einbinden einer Dateifreigabe in eine Python-Funktions-App mithilfe der Azure CLI

Dieses Azure Functions-Beispielskript erstellt eine Funktions-App und eine Freigabe in Azure Files. Anschließend wird die Freigabe eingebunden, sodass Ihre Funktionen auf die Daten zugreifen können.  

>[!NOTE]
>Die erstellte Funktions-App wird unter Python-Version 3.7 ausgeführt. Azure Functions [unterstützt auch die Python-Versionen 3.6 und 3.8](../functions-reference-python.md#python-version).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli). Beispiele sind für die Bash-Shell geschrieben und müssen für die Ausführung in einer Windows-Eingabeaufforderung geändert werden. 

## <a name="sample-script"></a>Beispielskript

Dieses Skript erstellt eine Azure-Funktions-App unter Verwendung des [Verbrauchsplans](../functions-scale.md#consumption-plan).

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/functions-cli-mount-files-storage-linux/functions-cli-mount-files-storage-linux.sh "Create an Azure Function on a Consumption plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft. Das Skript verwendet die folgenden Befehle:

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Erstellt ein Azure Storage-Konto. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Erstellt eine Funktionen-App. |
| [az storage share create](/cli/azure/storage/share#az-storage-share-create) | Erstellt eine Azure Files-Freigabe im Speicherkonto. | 
| [az storage directory create](/cli/azure/storage/directory#az-storage-directory-create) | Erstellt ein Verzeichnis in der Freigabe. |
| [az webapp config storage-account add](/cli/azure/webapp/config/storage-account#az-webapp-config-storage-account-add) | Bindet die Freigabe in die Funktions-App ein. |
| [az webapp config storage-account list](/cli/azure/webapp/config/storage-account#az-webapp-config-storage-account-list) | Zeigt die in die Funktions-App eingebundenen Dateifreigaben an. | 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).
