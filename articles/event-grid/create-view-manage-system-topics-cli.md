---
title: Erstellen, Anzeigen und Verwalten von Systemthemen in Azure Event Grid mithilfe der CLI
description: In diesem Artikel wird erläutert, wie Sie mithilfe der Azure CLI Systemthemen erstellen, anzeigen und löschen.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 354afb89b145e288f525e40ad700e8f8a67c6dad
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86115042"
---
# <a name="create-view-and-manage-event-grid-system-topics-using-azure-cli"></a>Erstellen, Anzeigen und Verwalten von Systemthemen in Azure Event Grid mithilfe der Azure CLI
In diesem Artikel wird beschrieben, wie Sie Systemthemen mithilfe der Azure CLI im Azure-Portal erstellen und verwalten. Eine Übersicht zu Systemthemen finden Sie unter [Systemthemen](system-topics.md).

## <a name="install-extension-for-azure-cli"></a>Installieren der Erweiterung für Azure CLI
Für die Azure CLI benötigen Sie die [Event Grid-Erweiterung](/cli/azure/azure-cli-extensions-list).

In Cloud Shell:

- Wenn Sie die Erweiterung bereits installiert haben, aktualisieren Sie sie mit `az extension update -n eventgrid`.
- Wenn Sie die Erweiterung noch nicht installiert haben, installieren Sie sie mit `az extension add -n eventgrid`.

Für eine lokale Installation:

1. [Installieren Sie die Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli). Vergewissern Sie sich mit `az --version`, dass Sie über die aktuelle Version verfügen.
2. Deinstallieren Sie frühere Versionen der Erweiterung mit `az extension remove -n eventgrid`.
3. Installieren Sie die eventgrid-Erweiterung mit `az extension add -n eventgrid`.

## <a name="create-a-system-topic"></a>Erstellen eines Systemthemas

- Wenn Sie zunächst ein Systemthema in einer Azure-Quelle und anschließend ein Ereignisabonnement für das Thema erstellen möchten, lesen Sie die folgenden Referenzartikel:
    - [az eventgrid system-topic create](/cli/azure/ext/eventgrid/eventgrid/system-topic?view=azure-cli-latest#ext-eventgrid-az-eventgrid-system-topic-create)

        ```azurecli-interactive
        # Get the ID of the Azure source (for example: Azure Storage account)
        storageid=$(az storage account show \
                --name <AZURE STORAGE ACCOUNT NAME> \
                --resource-group <AZURE RESOURCE GROUP NAME> \
                    --query id --output tsv)
    
        # Create the system topic on the Azure source (example: Azure Storage account)
        az eventgrid system-topic create \
            -g <AZURE RESOURCE GROUP NAME> \
            --name <SPECIFY SYSTEM TOPIC NAME> \
            --location <LOCATION> \
            --topic-type microsoft.storage.storageaccounts \
            --source $storageid
        ```           

        Wenn Sie eine Liste der Werte von `topic-type` abrufen möchten, mit denen Sie ein Systemthema erstellen können, führen Sie den folgenden Befehl aus. Diese topic-type-Werte stellen die Ereignisquellen dar, die die Erstellung von Systemthemen unterstützen. Die Listenwerte `Microsoft.EventGrid.Topics` und `Microsoft.EventGrid.Domains` können Sie ignorieren. 

        ```azurecli-interactive
        az eventgrid topic-type  list --output json | grep -w id
        ```
    - [az eventgrid system-topic event-subscription create](/cli/azure/ext/eventgrid/eventgrid/system-topic/event-subscription?view=azure-cli-latest#ext-eventgrid-az-eventgrid-system-topic-event-subscription-create)

        ```azurecli-interactive
        az eventgrid system-topic event-subscription create --name <SPECIFY EVENT SUBSCRIPTION NAME> \
            -g rg1 --system-topic-name <SYSTEM TOPIC NAME> \
            --endpoint <ENDPOINT URL>         
        ```
- Wenn Sie ein Systemthema (implizit) beim Erstellen eines Ereignisabonnements für eine Azure-Quelle erstellen möchten, verwenden Sie die Methode [az eventgrid event-subscription create](/cli/azure/ext/eventgrid/eventgrid/event-subscription?view=azure-cli-latest#ext-eventgrid-az-eventgrid-event-subscription-create). Hier sehen Sie ein Beispiel:
    
    ```azurecli-interactive
    storageid=$(az storage account show --name <AZURE STORAGE ACCOUNT NAME> --resource-group <AZURE RESOURCE GROUP NAME> --query id --output tsv)
    endpoint=<ENDPOINT URL>

    az eventgrid event-subscription create \
      --source-resource-id $storageid \
      --name <EVENT SUBSCRIPTION NAME> \
      --endpoint $endpoint
    ```
    Ein Tutorial mit einer ausführlichen Anleitung finden Sie unter [Abonnieren eines Speicherkontos](../storage/blobs/storage-blob-event-quickstart.md?toc=%2Fazure%2Fevent-grid%2Ftoc.json#subscribe-to-your-storage-account).

## <a name="view-all-system-topics"></a>Anzeigen aller Systemthemen
Wenn Sie alle Systemthemen sowie die Details zu einem bestimmten System anzeigen möchten, führen Sie die folgenden Befehle aus:

- [az eventgrid system-topic list](/cli/azure/ext/eventgrid/eventgrid/system-topic?view=azure-cli-latest#ext-eventgrid-az-eventgrid-system-topic-list)

    ```azurecli-interactive
    az eventgrid system-topic list   
     ```
- [az eventgrid system-topic show](/cli/azure/ext/eventgrid/eventgrid/system-topic?view=azure-cli-latest#ext-eventgrid-az-eventgrid-system-topic-show)

    ```azurecli-interactive
    az eventgrid system-topic show -g <AZURE RESOURCE GROUP NAME> -n <SYSTEM TOPIC NAME>     
     ```

## <a name="delete-a-system-topic"></a>Löscht ein Systemthema
Wenn Sie ein Thema löschen möchten, führen Sie den folgenden Befehl aus: 

- [az eventgrid system-topic delete](/cli/azure/ext/eventgrid/eventgrid/system-topic?view=azure-cli-latest#ext-eventgrid-az-eventgrid-system-topic-delete)

    ```azurecli-interactive
    az eventgrid system-topic delete -g <AZURE RESOURCE GROUP NAME> --name <SYSTEM TOPIC NAME>   
     ```

## <a name="next-steps"></a>Nächste Schritte
Im Abschnitt [Systemthemen in Azure Event Grid](system-topics.md) erfahren Sie mehr über Systemthemen und Thementypen, die von Azure Event Grid unterstützt werden. 