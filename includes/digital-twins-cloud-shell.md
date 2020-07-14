---
author: baanders
description: 'Includedatei für Azure Digital Twins: Einrichten von Cloud Shell und IoT-Erweiterung'
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 8be070826de0334483f4150925c05cb4dfb73f2c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85805532"
---
[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]

### <a name="set-up-cloud-shell-session"></a>Einrichten einer Cloud Shell-Sitzung

Nach dem Öffnen eines Cloud Shell-Fensters müssen Sie sich zunächst anmelden und den Shellkontext für diese Sitzung auf Ihr Abonnement festlegen. Führen Sie diese Befehle in Ihrer Cloud Shell aus:

```azurecli
az login
az account set --subscription <your-Azure-subscription-ID>
```

Wenn Sie dieses Abonnement zum ersten Mal mit Azure Digital Twins nutzen, führen Sie diesen Befehl aus, um sich im Namespace von Azure Digital Twins zu registrieren. (Falls Sie sich nicht sicher sind, ist es in Ordnung, ihn erneut auszuführen, auch wenn Sie ihn irgendwann zuvor schon einmal ausgeführt haben.)

```azurecli
az provider register --namespace 'Microsoft.DigitalTwins'
```

Als Nächstes fügen Sie die [**Microsoft Azure IoT-Erweiterung für Azure CLI**](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot?view=azure-cli-latest) zu Ihrer Cloud Shell hinzu, um Befehle für die Interaktion mit Azure Digital Twins und anderen IoT-Diensten zu aktivieren. 

Führen Sie zuerst diesen Befehl aus, um eine Liste mit allen bereits installierten Erweiterungen anzuzeigen.

```azurecli-interactive
az extension list
```

Suchen Sie in der Ausgabe für jeden Listeneintrag nach dem Feld `"name"`, um die Namen der Erweiterungen anzuzeigen.

Ermitteln Sie anhand der Ausgabe, welcher der folgenden Befehle für die Einrichtung der Erweiterung ausgeführt werden muss (ggf. auch mehr als ein Befehl).
* Wenn `azure-iot` in der Liste enthalten ist: Die Erweiterung ist bereits vorhanden. Führen Sie diesen Befehl aus, um sicherzustellen, dass Sie über das neueste Update verfügen:

   ```azurecli-interactive
   az extension update --name azure-iot
   ```

* Wenn `azure-iot` **nicht** in der Liste enthalten ist: Sie müssen die Erweiterung installieren. Verwenden Sie diesen Befehl:

    ```azurecli-interactive
    az extension add --name azure-iot
    ```

* Wenn `azure-iot-cli-ext` in der Liste enthalten ist: Dies ist die Legacy-Version der Erweiterung. Da jeweils nur eine Version der Erweiterung installiert sein sollte, sollten Sie die Legacy-Erweiterung deinstallieren. Verwenden Sie diesen Befehl:

   ```azurecli-interactive
   az extension remove --name azure-cli-iot-ext
   ```

Jetzt können Sie in der Cloud Shell mit Azure Digital Twins arbeiten.

Sie können dies durch Ausführung von `az dt -h` jederzeit überprüfen, um eine Liste der verfügbaren Azure Digital Twins-Befehle oberster Ebene anzuzeigen.