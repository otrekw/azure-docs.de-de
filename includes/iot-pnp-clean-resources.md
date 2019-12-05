---
author: baanders
ms.author: baanders
ms.service: iot-pnp
ms.topic: include
ms.date: 11/15/2019
ms.openlocfilehash: e6ca830a7224326c17003924b76192b95ba6bcb2
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74152012"
---
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit weiteren Artikeln zu IoT Plug & Play fortfahren möchten, können Sie die Ressourcen, die Sie in diesem Schnellstart verwendet haben, behalten und wiederverwenden. Andernfalls können Sie die Ressourcen löschen, die Sie für diesen Schnellstart erstellt haben, um zusätzliche Gebühren zu vermeiden.

Sie können den Hub und das registrierte Gerät gleichzeitig löschen, indem Sie die gesamte Ressourcengruppe mit dem folgenden Befehl in der Azure-Befehlszeilenschnittstelle löschen. (Verwenden Sie dies jedoch nicht, wenn diese Ressourcen eine Ressourcengruppe mit anderen Ressourcen für andere Zwecke gemeinsam nutzen.)

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

Sie können auch die geklonten SDK-Dateien von Ihrem Entwicklungscomputer entfernen.