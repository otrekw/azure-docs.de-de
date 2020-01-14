---
author: baanders
ms.author: baanders
ms.service: iot-pnp
ms.topic: include
ms.date: 10/24/2019
ms.openlocfilehash: 7a0ea2330e682e1e60eca6540151b0f5f620571c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453718"
---
## <a name="prepare-an-iot-hub"></a>Vorbereiten eines IoT-Hubs

Für diesen Schnellstart benötigen Sie außerdem eine Azure IoT Hub-Instanz in Ihrem Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. Falls Sie über keinen IoT-Hub verfügen, gehen Sie wie [hier](../articles/iot-hub/iot-hub-create-using-cli.md) beschrieben vor, um einen zu erstellen.

> [!IMPORTANT]
> Während der öffentlichen Vorschauphase sind die IoT Plug & Play-Funktionen nur für IoT-Hubs verfügbar, die in den Regionen **USA, Mitte**, **Europa, Norden** und **Japan, Osten** erstellt wurden.

Wenn Sie die Azure CLI lokal verwenden, melden Sie sich zunächst mithilfe des Befehls `az login` bei Ihrem Azure-Abonnement an. Wenn Sie diese Befehle in der Azure Cloud Shell ausführen, werden Sie automatisch angemeldet.

Wenn Sie die Azure-Befehlszeilenschnittstelle lokal verwenden, sollte mindestens `az` Version **2.0.73** verwendet werden. Für Azure Cloud Shell wird die neueste Version verwendet. Überprüfen Sie die auf dem Computer installierte Version mit dem Befehl `az --version`.

Führen Sie den folgenden Befehl aus, um Ihrer Instanz die Microsoft Azure IoT-Erweiterung für die Azure CLI hinzuzufügen:

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Führen Sie den folgenden Befehl aus, um die Geräteidentität in Ihrem IoT-Hub zu erstellen. Ersetzen Sie die Platzhalter **YourIoTHubName** und **YourDeviceID** durch Ihren eigenen _IoT-Hub-Namen_ und eine _Geräte-ID_ Ihrer Wahl.

```azurecli-interactive
az iot hub device-identity create --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```

Führen Sie den folgenden Befehl aus, um die _Geräteverbindungszeichenfolge_ für das soeben registrierte Gerät abzurufen (Notieren Sie sie für die spätere Verwendung.):

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```