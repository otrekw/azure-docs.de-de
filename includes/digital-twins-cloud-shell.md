---
author: baanders
description: 'Includedatei für Azure Digital Twins: Einrichten von Cloud Shell und IoT-Erweiterung'
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 76480959d94b9e2a2e04857c85b7359105e73e4f
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84611473"
---
[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]

### <a name="set-up-cloud-shell-session"></a>Einrichten einer Cloud Shell-Sitzung

Nach dem Öffnen eines Cloud Shell-Fensters müssen Sie sich zunächst anmelden und den Shellkontext für diese Sitzung auf Ihr Abonnement festlegen. 

```azurecli
az login
az account set --subscription <your-Azure-subscription-ID>
```

Wenn Sie dieses Abonnement zum ersten Mal mit Azure Digital Twins nutzen, führen Sie diesen Befehl aus, um sich im Namespace von Azure Digital Twins zu registrieren. (Falls Sie sich nicht sicher sind, ist es in Ordnung, ihn erneut auszuführen, auch wenn Sie ihn irgendwann zuvor schon einmal ausgeführt haben.)

```azurecli
az provider register --namespace 'Microsoft.DigitalTwins'
```

Führen Sie den folgenden Befehl in Ihrer Cloud Shell-Instanz aus, um die IoT-Erweiterung von Microsoft Azure für die Azure CLI hinzuzufügen.

   ```azurecli-interactive
   az extension add --name azure-iot
   ```

> [!NOTE]
> In diesem Artikel wird die neueste Version der Azure IoT-Erweiterung namens `azure-iot` verwendet. Die Legacy-Version heißt `azure-iot-cli-ext`. Es sollte jedoch jeweils nur eine Version installiert sein. Sie können den Befehl `az extension list` verwenden, um die derzeit installierten Erweiterungen zu überprüfen.
> Verwenden Sie `az extension remove --name azure-cli-iot-ext`, um die Legacy-Version der Erweiterung zu entfernen.
> Verwenden Sie `az extension add --name azure-iot`, um die neue Version der Erweiterung hinzuzufügen. Verwenden Sie `az extension list`, um die installierten Erweiterungen anzuzeigen.

> [!TIP]
> Sie können `az dt -h` ausführen, um die Azure Digital Twins-Befehle oberster Ebene anzuzeigen.