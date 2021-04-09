---
author: dominicbetts
ms.author: dominicbetts
ms.service: iot-pnp
ms.topic: include
ms.date: 11/15/2019
ms.openlocfilehash: 647226091c3e15a19bf8262c23e62d95018488b3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "99831201"
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
