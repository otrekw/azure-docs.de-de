---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 642989df8dca9d4ae121d80e30a9fb6d8dd06286
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107800158"
---
### <a name="query-the-storage-queue"></a>Abfragen der Speicherwarteschlange

Sie können den Befehl [`az storage queue list`](/cli/azure/storage/queue#az_storage_queue_list) verwenden, um die Speicherwarteschlangen in Ihrem Konto anzuzeigen, wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az storage queue list --output tsv
```

Die Ausgabe dieses Befehls umfasst eine Warteschlange namens `outqueue`, wobei es sich um die Warteschlange handelt, die bei Ausführung der Funktion erstellt wurde.

Als Nächstes verwenden Sie den Befehl [`az storage message peek`](/cli/azure/storage/message#az_storage_message_peek), um die Nachrichten in dieser Warteschlange anzuzeigen, wie im folgenden Beispiel zu sehen:

```azurecli-interactive
echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
```

Die zurückgegebene Zeichenfolge sollte mit der Nachricht, die Sie zum Testen der Funktion gesendet haben, identisch sein.

> [!NOTE]  
> Im vorherigen Beispiel wird die zurückgegebene Zeichenfolge aus base64 decodiert. Der Grund hierfür ist, dass die Warteschlangenspeicherbindungen als [base64-Zeichenfolgen](../articles/azure-functions/functions-bindings-storage-queue-trigger.md#encoding) in Azure Storage geschrieben und daraus gelesen werden.