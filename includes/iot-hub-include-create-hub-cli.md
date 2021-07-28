---
title: Datei einfügen
description: include file
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.topic: include
ms.date: 01/14/2021
ms.openlocfilehash: 45a9a0821dec98637ade4940bee915483d5e9aa1
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112039782"
---
## <a name="create-an-iot-hub"></a>Erstellen eines IoT-Hubs
In diesem Abschnitt verwenden Sie die Azure CLI zum Erstellen eines IoT-Hubs und einer Ressourcengruppe.  Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Ein IoT-Hub fungiert als zentraler Nachrichtenhub für die bidirektionale Kommunikation zwischen Ihrer IoT-Anwendung und den Geräten.

So erstellen Sie einen IoT-Hub und eine Ressourcengruppe:

1. Starten der CLI-App.  Kopieren Sie zum Ausführen der CLI-Befehle im restlichen Teil dieses Schnellstarts die Befehlssyntax, fügen Sie sie in Ihre CLI-App ein, bearbeiten Sie die Variablenwerte und drücken Sie Enter.
    - Wählen Sie bei Verwendung von Cloud Shell die Schaltfläche **Ausprobieren** in den CLI-Befehlen aus, um Cloud Shell in einem geteilten Browserfenster zu starten. Alternativ können Sie [Cloud Shell](https://shell.azure.com/bash) auch in einem separaten Browsertab öffnen.
    - Gehen Sie wie folgt vor, wenn Sie die Azure CLI lokal verwenden: Starten Sie Ihre CLI-Konsolen-App, und melden Sie sich bei der Azure CLI an.

1. Führen Sie [az extension add](/cli/azure/extension?view=azure-cli-latest#az_extension_add) aus, um die Erweiterung *azure-iot* zu installieren bzw. auf die aktuelle Version zu aktualisieren.

    ```azurecli-interactive
    az extension add --upgrade --name azure-iot
    ```

1. Führen Sie in Ihrer CLI-App den Befehl [az group create](/cli/azure/group#az_group_create) aus, um eine Ressourcengruppe zu erstellen. Mit dem folgenden Befehl wird eine Ressourcengruppe mit dem Namen *MyResourceGroup* am Standort *eastus* erstellt. 
    >[!NOTE]
    > Optional können Sie auch einen alternativen Standort festlegen. Führen Sie zum Anzeigen der verfügbaren Standorte `az account list-locations` aus. In diesem Tutorial wird *eastus* verwendet, wie im Beispielbefehl gezeigt. 

    ```azurecli-interactive
    az group create --name MyResourceGroup --location eastus
    ```

1. Führen Sie den Befehl [az iot hub create](/cli/azure/iot/hub#az_iot_hub_create) aus, um einen IoT-Hub zu erstellen. Es kann einige Minuten dauern, bis ein IoT-Hub erstellt wurde. 

    *YourIotHubName*: Ersetzen Sie diesen Platzhalter und die umgebenden geschweiften Klammern im folgenden Befehl durch den Namen, den Sie für Ihren IoT-Hub ausgewählt haben. Der Name eines IoT-Hubs muss in Azure global eindeutig sein. Verwenden Sie im weiteren Verlauf dieser Schnellstartanleitung den Namen des IoT-Hubs für alle Vorkommen dieses Platzhalters.

    ```azurecli-interactive
    az iot hub create --resource-group MyResourceGroup --name {YourIoTHubName}
    ```
    > [!TIP]
    > Nach dem Erstellen eines IoT Hubs können Sie im restlichen Teil dieser Schnellstartanleitung weiterhin CLI-Befehle verwenden, um mit dem Hub zu interagieren. Optional können Sie den Azure IoT-Explorer statt der CLI-Befehle verwenden. IoT Explorer ist eine GUI-Anwendung, mit der Sie eine Verbindung mit einer vorhandenen IoT Hub sowie Geräte hinzufügen, verwalten und überwachen können. Weitere Informationen finden Sie unter [Installieren und Verwenden des Azure IoT-Explorers](../articles/iot-pnp/howto-use-iot-explorer.md).

## <a name="create-a-simulated-device"></a>Erstellen Sie ein simuliertes Gerät.
In diesem Abschnitt erstellen Sie ein simuliertes IoT-Gerät, das mit Ihrem IoT-Hub verbunden ist. 

So erstellen Sie ein simuliertes Gerät:
1. Führen Sie in der CLI-Shell den Befehl [az iot hub device-identity create](/cli/azure/iot/hub/device-identity#az_iot_hub_device_identity_create) aus. Die Identität des simulierten Geräts wird erstellt. 

    *YourIotHubName*: Ersetzen Sie diesen Platzhalter unten durch den Namen, den Sie für Ihren IoT-Hub ausgewählt haben. 

    *myDevice*. Sie können diesen Namen im weiteren Verlauf dieses Artikels direkt für die ID des simulierten Geräts verwenden. Optional können Sie auch einen anderen Namen nutzen. 

    ```azurecli-interactive
    az iot hub device-identity create --device-id myDevice --hub-name {YourIoTHubName} 
    ```

1.  Führen Sie den Befehl [az iot hub device-identity connection-string show](/cli/azure/iot/hub/device-identity/connection-string#az_iot_hub_device_identity_connection_string_show) aus. 

    ```azurecli-interactive
    az iot hub device-identity connection-string show --device-id myDevice --hub-name {YourIoTHubName}
    ```

    Die Ausgabe der Verbindungszeichenfolge hat folgendes Format:

    ```Output
    HostName=<your IoT Hub name>.azure-devices.net;DeviceId=<your device id>;SharedAccessKey=<some value>
    ```

1. Speichern Sie die Verbindungszeichenfolge an einem sicheren Ort. 

> [!NOTE]
> Lassen Sie Ihre CLI-App geöffnet. Sie werden ihn später benötigen.