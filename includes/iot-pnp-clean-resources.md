---
author: dominicbetts
ms.author: dominicbetts
ms.service: iot-develop
ms.topic: include
ms.date: 11/15/2019
ms.openlocfilehash: c4e4d6858ce03c0a7015f6754018c509c738f8da
ms.sourcegitcommit: 8669087bcbda39e3377296c54014ce7b58909746
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2021
ms.locfileid: "114400038"
---
Wenn Sie mit weiteren Artikeln zu IoT Plug & Play fortfahren möchten, können Sie die Ressourcen, die Sie in diesem Artikel verwendet haben, beibehalten und wiederverwenden. Andernfalls können Sie die in diesem Artikel erstellten Ressourcen löschen, um zusätzliche Gebühren zu vermeiden.

Sie können den Hub und das registrierte Gerät gleichzeitig löschen, indem Sie die gesamte Ressourcengruppe mit dem folgenden Azure CLI-Befehl löschen. Verwenden Sie diesen Befehl nicht, wenn für diese Ressourcen eine Ressourcengruppe genutzt wird, in der auch andere Ressourcen enthalten sind, die Sie beibehalten möchten.

```azurecli-interactive
az group delete --name <YourResourceGroupName>
```

Um nur den IoT-Hub zu löschen, führen Sie den folgenden Befehl über die Azure-Befehlszeilenschnittstelle aus:

```azurecli-interactive
az iot hub delete --name <YourIoTHubName>
```

Führen Sie mit der Azure-Befehlszeilenschnittstelle die folgenden Befehle aus, um nur die bei Ihrem IoT-Hub registrierte Geräte-ID zu löschen:

```azurecli-interactive
az iot hub device-identity delete --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```

Möglicherweise sollten Sie auch die geklonten Beispieldateien von Ihrem Entwicklungscomputer entfernen.
