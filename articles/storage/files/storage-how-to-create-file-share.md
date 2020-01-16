---
title: Erstellen einer Azure-Dateifreigabe
titleSuffix: Azure Files
description: Hier erfahren Sie, wie Sie über das Azure-Portal, mithilfe von PowerShell oder mit der Azure CLI eine Azure-Dateifreigabe erstellen.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 09/19/2017
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: ca4904940c7974e83445892b9597a9d3c36b44ee
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75452887"
---
# <a name="create-a-file-share"></a>Erstellen einer Dateifreigabe
Sie können eine Freigabe in Azure Files mit dem  [Azure-Portal](https://portal.azure.com/), den Azure Storage-PowerShell-Cmdlets, den Azure Storage-Clientbibliotheken oder der Azure Storage-REST-API erstellen. In diesem Artikel lernen Sie Folgendes:
- Erstellen einer Azure-Dateifreigabe über das Azure-Portal
- Erstellen einer Azure-Dateifreigabe mithilfe von PowerShell
- Erstellen einer Azure-Dateifreigabe mithilfe der Azure CLI

## <a name="prerequisites"></a>Voraussetzungen
Sie können eine Azure-Dateifreigabe entweder unter Verwendung eines bereits vorhandenen Speicherkontos erstellen oder ein [neues Azure-Speicherkonto erstellen](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Wenn Sie eine Azure-Dateifreigabe mithilfe von PowerShell erstellen möchten, benötigen Sie den Kontoschlüssel und -namen Ihres Speicherkontos. Wenn Sie PowerShell oder die CLI verwenden möchten, benötigen Sie einen Speicherkontoschlüssel.

> [!NOTE]
> Wenn Sie Dateifreigaben über 5 TiB erstellen möchten, finden Sie weitere Informationen unter [Aktivieren großer Dateifreigaben](storage-files-how-to-create-large-file-share.md).

## <a name="create-a-file-share-through-the-azure-portal"></a>Erstellen einer Dateifreigabe über das Azure-Portal

1. Navigieren Sie im Azure-Portal zum Bereich **Speicherkonto**.
    ![Bereich „Speicherkonto“](./media/storage-how-to-create-file-share/create-file-share-portal1.png)

1. Wählen Sie die Schaltfläche **+ Dateifreigabe** aus.
    ![Schaltfläche zum Hinzufügen einer Dateifreigabe](./media/storage-how-to-create-file-share/create-file-share-portal2.png)

1. Geben Sie die Informationen für **Name** und **Kontingent** ein.
    ![Namen und Kontingent für die neue Dateifreigabe](./media/storage-how-to-create-file-share/create-file-share-portal3.png)

1. Zeigen Sie die neue Dateifreigabe an.
    ![Neue Dateifreigabe auf der Benutzeroberfläche](./media/storage-how-to-create-file-share/create-file-share-portal4.png)

1. Laden Sie eine Datei hoch.
    ![Schaltfläche „Hochladen“ in der Navigationsleiste](./media/storage-how-to-create-file-share/create-file-share-portal5.png)

1. Navigieren Sie zu Ihrer Dateifreigabe, und verwalten Sie Ihre Verzeichnisse und Dateien.
    ![Ordneransicht von Dateifreigaben](./media/storage-how-to-create-file-share/create-file-share-portal6.png)

## <a name="create-a-file-share-through-powershell"></a>Erstellen einer Dateifreigabe mithilfe von PowerShell

Laden Sie die Azure PowerShell-Cmdlets herunter, und installieren Sie sie. Informationen zum Installationspunkt und zu Installationsanweisungen finden Sie unter  [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

> [!Note]  
> Es wird empfohlen, das neueste Azure PowerShell-Modul herunterzuladen und zu installieren bzw. ein Upgrade durchzuführen.

1. Erstellen Sie ein neues Speicherkonto Der Name des Speicherkontos muss eindeutig und 3 bis 24 Zeichen lang sein.
    Ein Speicherkonto ist ein gemeinsam genutzter Pool mit Speicherplatz, den Sie zum Bereitstellen von Azure-Dateifreigaben und Speicherressourcen wie Blobs oder Warteschlangen verwenden können.

    ```PowerShell
    $resourceGroup = "myresourcegroup"
    $storAcctName = "myuniquestorageaccount"
    $region = "westus2"
    $storAcct = New-AzStorageAccount -ResourceGroupName $resourceGroup -Name $storAcctName -SkuName Standard_LRS -Location $region -Kind StorageV2
    ```

1. Erstellen Sie eine neue Dateifreigabe.        
    ```PowerShell
    $shareName = "myshare"
    $share = New-AzStorageShare -Context $storAcct.Context -Name $shareName
    ```

> [!Note]  
> Der Name der Dateifreigabe darf nur Kleinbuchstaben enthalten. Ausführliche Informationen zur Benennung von Dateifreigaben und Dateien finden Sie unter  [Benennen und Referenzieren von Freigaben, Verzeichnissen, Dateien und Metadaten](https://msdn.microsoft.com/library/azure/dn167011.aspx).

## <a name="create-a-file-share-through-the-cli"></a>Erstellen einer Dateifreigabe mithilfe der CLI

1. Laden Sie die Azure CLI herunter, und installieren Sie sie.
    Informationen finden Sie unter  [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) sowie unter [Erste Schritte mit der Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

1. Erstellen Sie eine Verbindungszeichenfolge für das Speicherkonto, unter dem Sie die Freigabe erstellen möchten.
    Ersetzen Sie im folgenden Beispiel  ```<storage-account>``` und  ```<resource_group>``` durch Ihren Speicherkontonamen und Ihre Ressourcengruppe:

   ```azurecli
    current_env_conn_string=$(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve the connection string."
    fi
    ```

1. Erstellen Sie die Dateifreigabe.
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string > /dev/null
    ```

## <a name="next-steps"></a>Nächste Schritte

* [Einbinden einer Dateifreigabe unter Windows](storage-how-to-use-files-windows.md)
* [Einbinden einer Dateifreigabe unter Linux](storage-how-to-use-files-linux.md)
* [Einbinden einer Dateifreigabe unter macOS](storage-how-to-use-files-mac.md)

Weitere Informationen zu Azure Files finden Sie unter diesen Links:

* [Häufig gestellte Fragen zu Speicherdateien](storage-files-faq.md)
* [Problembehandlung für Azure Files unter Windows](storage-troubleshoot-windows-file-connection-problems.md)
* [Problembehandlung für Azure Files unter Linux](storage-troubleshoot-linux-file-connection-problems.md)
