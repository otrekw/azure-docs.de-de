---
author: baanders
description: Includedatei zum Bereinigen einer grundlegenden Azure Digital Twins-Instanz
ms.service: digital-twins
ms.topic: include
ms.date: 8/13/2020
ms.author: baanders
ms.openlocfilehash: ab179c5f0c56a7d6fd2ebd41e13d27a832512b54
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2020
ms.locfileid: "94358077"
---
Wenn Sie die in diesem Tutorial erstellten Ressourcen nicht mehr benötigen, führen Sie die folgenden Schritte zum Löschen aus.

Bei Verwendung von [Azure Cloud Shell](https://shell.azure.com) können Sie alle Azure-Ressourcen in einer Ressourcengruppe mit dem Befehl [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest&preserve-view=true#az-group-delete) löschen. Mit diesem Befehl werden die Ressourcengruppe und die Azure Digital Twins-Instanz entfernt.

> [!IMPORTANT]
> Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen.

Öffnen Sie Azure Cloud Shell, und führen Sie den folgenden Befehl aus, um die Ressourcengruppe und alle darin enthaltenen Elemente zu löschen.

```azurecli-interactive
az group delete --name <your-resource-group>
```