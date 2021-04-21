---
author: baanders
description: Includedatei zum Bereinigen einer Azure Digital Twins-Instanz
ms.service: digital-twins
ms.topic: include
ms.date: 2/4/2021
ms.author: baanders
ms.openlocfilehash: 0d8cc30c0511098caf7b6c47d7f7bd400dc32f1b
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107800192"
---
* **Falls Sie die in diesem Tutorial erstellten Ressourcen nicht benötigen**, können Sie mit dem Befehl [az group delete](/cli/azure/group#az_group_delete) die Azure Digital Twins-Instanz und alle anderen Ressourcen aus diesem Artikel löschen. Dadurch werden alle Azure-Ressourcen in einer Ressourcengruppe sowie die Ressourcengruppe selbst gelöscht.
    
    > [!IMPORTANT]
    > Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen.
    
    Öffnen Sie [Azure Cloud Shell](https://shell.azure.com), und führen Sie den folgenden Befehl aus, um die Ressourcengruppe und alle darin enthaltenen Elemente zu löschen.
    
    ```azurecli-interactive
    az group delete --name <your-resource-group>
    ```