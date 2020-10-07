---
author: baanders
description: Includedatei zum Bereinigen einer grundlegenden Azure Digital Twins-Instanz und einer App-Registrierung
ms.service: digital-twins
ms.topic: include
ms.date: 8/13/2020
ms.author: baanders
ms.openlocfilehash: e8f167555434338416381aaf8978a3d551686fb2
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "90606220"
---
Wenn Sie die in diesem Tutorial erstellten Ressourcen nicht mehr benötigen, führen Sie die folgenden Schritte zum Löschen aus.

Bei Verwendung von [Azure Cloud Shell](https://shell.azure.com) können Sie alle Azure-Ressourcen in einer Ressourcengruppe mit dem Befehl [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest&preserve-view=true#az-group-delete) löschen. Dadurch werden die Ressourcengruppe und die Azure Digital Twins-Instanz entfernt.

> [!IMPORTANT]
> Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen. 

Öffnen Sie eine Azure Cloud Shell-Instanz, und führen Sie den folgenden Befehl aus, um die Ressourcengruppe und alle darin enthaltenen Elemente zu löschen.

```azurecli
az group delete --name <your-resource-group>
```

Löschen Sie als Nächstes mit dem folgenden Befehl die Azure Active Directory-App-Registrierung, die Sie für Ihre Client-App erstellt haben:

```azurecli
az ad app delete --id <your-application-ID>
```