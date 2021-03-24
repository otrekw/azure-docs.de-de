---
author: baanders
description: Includedatei zum Bereinigen einer Azure Digital Twins-Instanz
ms.service: digital-twins
ms.topic: include
ms.date: 2/4/2021
ms.author: baanders
ms.openlocfilehash: 9a02c4f5c5699b4a6308bfaa519fa9eb776414d6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102245028"
---
* **Falls Sie die in diesem Tutorial erstellten Ressourcen nicht benötigen**, können Sie mit dem Befehl [az group delete](/cli/azure/group#az-group-delete) die Azure Digital Twins-Instanz und alle anderen Ressourcen aus diesem Artikel löschen. Dadurch werden alle Azure-Ressourcen in einer Ressourcengruppe sowie die Ressourcengruppe selbst gelöscht.
    
    > [!IMPORTANT]
    > Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen.
    
    Öffnen Sie [Azure Cloud Shell](https://shell.azure.com), und führen Sie den folgenden Befehl aus, um die Ressourcengruppe und alle darin enthaltenen Elemente zu löschen.
    
    ```azurecli-interactive
    az group delete --name <your-resource-group>
    ```