---
author: baanders
description: 'Includedatei für Azure Digital Twins: Einrichten von Cloud Shell und IoT-Erweiterung'
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: b7c91d648c06970d53799c6ff505919dea17b3c0
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87032209"
---
Gehen Sie wie folgt vor, um Azure Digital Twins in einem geöffneten [Azure Cloud Shell](https://shell.azure.com)-Fenster zu verwenden: Melden Sie sich an, und legen Sie zunächst den Shellkontext für diese Sitzung auf Ihr Abonnement fest. Führen Sie diese Befehle in Ihrer Cloud Shell aus:

```azurecli
az login
az account set --subscription "<your-Azure-subscription-ID>"
```
> [!TIP]
> Sie können im obigen Befehl anstelle der ID auch Ihren Abonnementnamen verwenden. 

Wenn Sie dieses Abonnement zum ersten Mal mit Azure Digital Twins nutzen, führen Sie diesen Befehl aus, um sich im Namespace von Azure Digital Twins zu registrieren. (Falls Sie sich nicht sicher sind, ist es in Ordnung, ihn erneut auszuführen, auch wenn Sie ihn irgendwann zuvor schon einmal ausgeführt haben.)

```azurecli
az provider register --namespace 'Microsoft.DigitalTwins'
```

Als Nächstes fügen Sie die [**Microsoft Azure IoT-Erweiterung für Azure CLI**](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot?view=azure-cli-latest) zu Ihrer Cloud Shell hinzu, um Befehle für die Interaktion mit Azure Digital Twins und anderen IoT-Diensten zu aktivieren. 

Führen Sie zuerst diesen Befehl aus, um eine Liste mit allen bereits installierten Erweiterungen anzuzeigen.

```azurecli-interactive
az extension list
```

Die Ausgabe enthält ein Array mit allen Erweiterungen, die Sie derzeit besitzen. Suchen Sie für jeden Listeneintrag nach dem Feld `"name"`, um die Namen der Erweiterungen anzuzeigen.

Ermitteln Sie anhand der Ausgabe, welcher der folgenden Befehle für die Einrichtung der Erweiterung ausgeführt werden muss (ggf. auch mehr als ein Befehl).
* Wenn `azure-iot` in der Liste enthalten ist: Die Erweiterung ist bereits vorhanden. Führen Sie diesen Befehl aus, um sicherzustellen, dass Sie über das neueste Update verfügen und keine weiteren Updates verfügbar sind:

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