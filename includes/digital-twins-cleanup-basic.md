---
author: baanders
description: Includedatei zum Bereinigen einer Azure Digital Twins-Instanz
ms.service: digital-twins
ms.topic: include
ms.date: 2/4/2021
ms.author: baanders
ms.openlocfilehash: 0783f25813fa8e4a65a33975017e2b2d03a4dd2c
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110075477"
---
* **Falls Sie die in diesem Tutorial erstellten Ressourcen nicht benötigen**, können Sie mit dem Befehl [az group delete](/cli/azure/group?view=azure-cli-latest&preserve-view=true#az_group_delete) die Azure Digital Twins-Instanz und alle anderen Ressourcen aus diesem Artikel löschen. Dadurch werden alle Azure-Ressourcen in einer Ressourcengruppe sowie die Ressourcengruppe selbst gelöscht.
    
    > [!IMPORTANT]
    > Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen.
    
    Öffnen Sie [Azure Cloud Shell](https://shell.azure.com), und führen Sie den folgenden Befehl aus, um die Ressourcengruppe und alle darin enthaltenen Elemente zu löschen.
    
    ```azurecli-interactive
    az group delete --name <your-resource-group>
    ```