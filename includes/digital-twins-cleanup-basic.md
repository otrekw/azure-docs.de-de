---
author: baanders
description: Includedatei zum Bereinigen einer grundlegenden Azure Digital Twins-Instanz
ms.service: digital-twins
ms.topic: include
ms.date: 8/13/2020
ms.author: baanders
ms.openlocfilehash: 29a07ffa917153c0cb062d34e2807d43d039a373
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2020
ms.locfileid: "92494704"
---
Wenn Sie die in diesem Tutorial erstellten Ressourcen nicht mehr benötigen, führen Sie die folgenden Schritte zum Löschen aus.

Bei Verwendung von [Azure Cloud Shell](https://shell.azure.com) können Sie alle Azure-Ressourcen in einer Ressourcengruppe mit dem Befehl [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest&preserve-view=true#az-group-delete) löschen. Dadurch werden die Ressourcengruppe und die Azure Digital Twins-Instanz entfernt.

> [!IMPORTANT]
> Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen. 

Öffnen Sie eine Azure Cloud Shell-Instanz, und führen Sie den folgenden Befehl aus, um die Ressourcengruppe und alle darin enthaltenen Elemente zu löschen.

```azurecli-interactive
az group delete --name <your-resource-group>
```