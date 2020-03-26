---
author: baanders
ms.author: baanders
ms.service: iot-pnp
ms.topic: include
ms.date: 10/24/2019
ms.openlocfilehash: a3340eb9e53afa83c35109bad7d22f81413dd644
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "80234136"
---
## <a name="prepare-an-iot-hub"></a>Vorbereiten eines IoT-Hubs

Für diesen Schnellstart benötigen Sie außerdem eine Azure IoT Hub-Instanz in Ihrem Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen. Falls Sie über keinen IoT-Hub verfügen, gehen Sie wie [hier](../articles/iot-hub/iot-hub-create-using-cli.md) beschrieben vor, um einen zu erstellen.

Wenn Sie die Azure CLI lokal verwenden, melden Sie sich zunächst mithilfe des Befehls `az login` bei Ihrem Azure-Abonnement an. Wenn Sie diese Befehle in der Azure Cloud Shell ausführen, werden Sie automatisch angemeldet.

Wenn Sie die Azure-Befehlszeilenschnittstelle lokal verwenden, sollte mindestens `az` Version **2.0.73** verwendet werden. Für Azure Cloud Shell wird die neueste Version verwendet. Überprüfen Sie die auf dem Computer installierte Version mit dem Befehl `az --version`.

Führen Sie den folgenden Befehl aus, um Ihrer Instanz die Microsoft Azure IoT-Erweiterung für die Azure CLI hinzuzufügen:

```azurecli-interactive
az extension add --name azure-iot
```

Führen Sie den folgenden Befehl aus, um die Geräteidentität in Ihrem IoT-Hub zu erstellen. Ersetzen Sie die Platzhalter **YourIoTHubName** und **YourDeviceID** durch Ihren eigenen _IoT-Hub-Namen_ und eine _Geräte-ID_ Ihrer Wahl.

```azurecli-interactive
az iot hub device-identity create --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```

Führen Sie den folgenden Befehl aus, um die _Geräteverbindungszeichenfolge_ für das soeben registrierte Gerät abzurufen (Notieren Sie sie für die spätere Verwendung.):

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```
