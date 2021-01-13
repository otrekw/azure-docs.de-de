---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 894a89126d1ee3ed909134f3e0dd914166568654
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90606508"
---
Sie können die Warteschlange im [Azure-Portal](../articles/storage/queues/storage-quickstart-queues-portal.md) oder im [Microsoft Azure Storage-Explorer](https://storageexplorer.com/) anzeigen. Sie haben auch die Möglichkeit, die Warteschlange per Azure CLI anzuzeigen. Dies ist in den folgenden Schritten beschrieben:

1. Öffnen Sie die Datei *local.setting.json* des Funktionsprojekts, und kopieren Sie den Wert der Verbindungszeichenfolge. Führen Sie in einem Terminal oder Befehlsfenster den folgenden Befehl aus, um eine Umgebungsvariable mit dem Namen `AZURE_STORAGE_CONNECTION_STRING` zu erstellen, und fügen Sie anstelle von `<MY_CONNECTION_STRING>` Ihre spezifische Verbindungszeichenfolge ein. (Die Verwendung dieser Umgebungsvariablen bewirkt, dass Sie die Verbindungszeichenfolge nicht für jeden weiteren Befehl per `--connection-string`-Argument angeben müssen.)

    # <a name="bash"></a>[Bash](#tab/bash)
    
    ```bash
    export AZURE_STORAGE_CONNECTION_STRING="<MY_CONNECTION_STRING>"
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    $env:AZURE_STORAGE_CONNECTION_STRING = "<MY_CONNECTION_STRING>"
    ```
    
    # <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/cmd)
    
    ```azurecli
    set AZURE_STORAGE_CONNECTION_STRING="<MY_CONNECTION_STRING>"
    ```
    
    ---
    
1. (Optional) Verwenden Sie den Befehl [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list), um die Storage-Warteschlangen in Ihrem Konto anzuzeigen. Die Ausgabe dieses Befehls sollte eine Warteschlange mit dem Namen `outqueue` enthalten. Sie wurde erstellt, als die Funktion ihre erste Nachricht in diese Warteschlange geschrieben hat.
    
    ```azurecli
    az storage queue list --output tsv
    ```

1. Verwenden Sie den Befehl [`az storage message get`](/cli/azure/storage/message#az-storage-message-get), um die Nachricht aus dieser Warteschlange zu lesen. Dies sollte der erste Name sein, den Sie beim zuvor durchgeführten Testen der Funktion genutzt haben. Mit dem Befehl wird die erste Nachricht der Warteschlange gelesen und daraus entfernt. 

    # <a name="bash"></a>[Bash](#tab/bash)
    
    ```bash
    echo `echo $(az storage message get --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($(az storage message get --queue-name outqueue -o tsv --query '[].{Message:content}')))
    ```
    
    # <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/cmd)
    
    ```azurecli
    az storage message get --queue-name outqueue -o tsv --query [].{Message:content} > %TEMP%out.b64 && certutil -decode -f %TEMP%out.b64 %TEMP%out.txt > NUL && type %TEMP%out.txt && del %TEMP%out.b64 %TEMP%out.txt /q
    ```

    In diesem Skript wird certutil verwendet, um die Base64-codierte Nachrichtensammlung aus einer lokalen temporären Datei zu decodieren. Versuchen Sie bei einer fehlenden Ausgabe, `> NUL` aus dem Skript zu entfernen, um die Unterdrückung der certutil-Ausgabe zu beenden, falls ein Fehler auftritt. 
    
    ---
    
    Da der Nachrichtentext im [Base64-codierten](../articles/azure-functions/functions-bindings-storage-queue-trigger.md#encoding) Format gespeichert ist, muss die Nachricht vor dem Anzeigen decodiert werden. Nachdem Sie `az storage message get` ausgeführt haben, wird die Nachricht aus der Warteschlange entfernt. Falls in `outqueue` nur eine Nachricht enthalten war, rufen Sie keine Nachricht ab, wenn Sie diesen Befehl zum zweiten Mal ausführen, sondern erhalten einen Fehler.
