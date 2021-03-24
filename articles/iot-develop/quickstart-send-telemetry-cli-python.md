---
title: 'Schnellstart: Senden von Gerätetelemetriedaten an Azure IoT Hub (Python)'
description: In dieser Schnellstartanleitung verwenden Sie das Azure IoT Hub-Geräte-SDK für Python, um Telemetriedaten von einem Gerät an einen IoT Hub zu senden.
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.devlang: python
ms.topic: quickstart
ms.date: 01/11/2021
ms.openlocfilehash: d73f8eeb7b69440f8db67d0b95b40ed6258ee8e7
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102201786"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-azure-iot-hub-python"></a>Schnellstart: Senden von Telemetriedaten von einem Gerät an eine Azure IoT Hub-Instanz (Python)

**Gilt für**: [Geräteanwendungsentwicklung](about-iot-develop.md#device-application-development)

In dieser Schnellstartanleitung lernen Sie einen einfachen Anwendungsentwicklungsworkflow für IoT-Geräte kennen. Sie verwenden die Azure CLI, um einen Azure IoT Hub und ein Gerät zu erstellen. Anschließend verwenden Sie das Azure IoT Python SDK, um ein simuliertes Clientgerät zu erstellen und Telemetriedaten an den Hub zu senden. 

## <a name="prerequisites"></a>Voraussetzungen
- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.
- Azure-Befehlszeilenschnittstelle. Sie können alle Befehle dieser Schnellstartanleitung über die Azure Cloud Shell ausführen. Hierbei handelt es sich um eine interaktive CLI-Shell, die in Ihrem Browser ausgeführt wird. Bei Verwendung der Cloud Shell müssen Sie nichts installieren. Falls Sie die lokale Nutzung der CLI vorziehen, müssen Sie für diese Schnellstartanleitung mindestens Version 2.0.76 der Azure CLI verwenden. Führen Sie „az --version“ aus, um die Version zu ermitteln. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).
- [Python 3.7 oder höher](https://www.python.org/downloads/). Informationen zu anderen unterstützten Python-Versionen finden Sie im Artikel zu den [Azure IoT-Gerätefeatures](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device#azure-iot-device-features).
    
    Um sicherzustellen, dass Ihre Python-Version auf dem neuesten Stand ist, führen Sie `python --version` aus. Wenn Sie sowohl Python 2 als auch Python 3 installiert haben und eine Python 3-Umgebung verwenden, installieren Sie alle Bibliotheken mithilfe von `pip3`. Dadurch wird sichergestellt, dass die Bibliotheken in Ihrer Python 3-Runtime installiert werden.
    > [!IMPORTANT]
    > Wählen Sie im Python-Installationsprogramm die Option für das **Hinzufügen von Python zu PATH** aus. Wenn bereits Python 3.7 oder höher installiert ist, vergewissern Sie sich, dass Sie den Python-Installationsordner der Umgebungsvariablen `PATH` hinzugefügt haben.

[!INCLUDE [iot-hub-include-create-hub-cli](../../includes/iot-hub-include-create-hub-cli.md)]

## <a name="use-the-python-sdk-to-send-messages"></a>Verwenden des Python SDK zum Senden von Nachrichten
In diesem Abschnitt verwenden Sie das Python SDK zum Senden von Nachrichten vom simulierten Gerät an den IoT Hub.

1. Öffnen Sie ein neues Terminalfenster. Sie verwenden dieses Terminal, um das Python SDK zu installieren und mit Python-Beispielcode zu arbeiten. Jetzt sollten zwei Terminals geöffnet sein: das Terminal, das Sie gerade zum Arbeiten mit Python geöffnet haben, und die CLI-Shell, die Sie in den vorherigen Abschnitten für die Eingabe von Azure CLI-Befehlen verwendet haben.       

1. Kopieren Sie die [Gerätebeispiele für das Azure IoT Python SDK](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples) auf Ihren lokalen Computer:

    ```console
    git clone https://github.com/Azure/azure-iot-sdk-python
    ```

    und navigieren Sie zum Verzeichnis *azure-iot-sdk-python/azure-iot-device/samples*:

    ```console
    cd azure-iot-sdk-python/azure-iot-device/samples
    ```
1. Installieren Sie das Azure IoT Python SDK:

    ```console
    pip install azure-iot-device
    ```
1. Legen Sie die Geräteverbindungszeichenfolge als Umgebungsvariable `IOTHUB_DEVICE_CONNECTION_STRING` fest. Dies ist die Zeichenfolge, die Sie im vorherigen Abschnitt nach dem Erstellen des simulierten Python-Geräts erhalten haben.

    **Windows (cmd)**

    ```console
    set IOTHUB_DEVICE_CONNECTION_STRING=<your connection string here>
    ```

    > [!NOTE]
    > In den Windows-CMD-Befehlen ist die Verbindungszeichenfolge nicht in Anführungszeichen eingeschlossen.

    **Linux (bash)**

    ```bash
    export IOTHUB_DEVICE_CONNECTION_STRING="<your connection string here>"
    ```

1. Führen Sie in der geöffneten CLI-Shell den Befehl [az iot hub monitor-events](/cli/azure/ext/azure-iot/iot/hub#ext-azure-iot-az-iot-hub-monitor-events) aus, um mit der Überwachung von Ereignissen auf dem simulierten IoT-Gerät zu beginnen.  Ereignismeldungen werden im Terminal angezeigt, sobald sie eintreffen.

    ```azurecli
    az iot hub monitor-events --output table --hub-name {YourIoTHubName}
    ```

1. Führen Sie im Python-Terminal den Code für die installierte Beispieldatei *simple_send_message.py* aus. Dieser Code greift auf das simulierte IoT-Gerät zu und sendet eine Nachricht an den IoT Hub.

    So führen Sie das Python-Beispiel im Terminal aus:
    ```console
    python ./simple_send_message.py
    ```

    Optional können Sie den Python-Code aus dem Beispiel in der Python-IDE ausführen:
    ```python
    import os
    import asyncio
    from azure.iot.device.aio import IoTHubDeviceClient


    async def main():
        # Fetch the connection string from an environment variable
        conn_str = os.getenv("IOTHUB_DEVICE_CONNECTION_STRING")

        # Create instance of the device client using the authentication provider
        device_client = IoTHubDeviceClient.create_from_connection_string(conn_str)

        # Connect the device client.
        await device_client.connect()

        # Send a single message
        print("Sending message...")
        await device_client.send_message("This is a message that is being sent")
        print("Message successfully sent!")

        # finally, disconnect
        await device_client.disconnect()


    if __name__ == "__main__":
        asyncio.run(main())
    ```

Wenn der Python-Code eine Nachricht von Ihrem Gerät an den IoT Hub sendet, wird die Meldung in der CLI-Shell angezeigt, in der Ereignisse überwacht werden:

```output
Starting event monitor, use ctrl-c to stop...
event:
origin: <your Device name>
payload: This is a message that is being sent
```

Ihr Gerät ist nun sicher verbunden und sendet Telemetriedaten an Azure IoT Hub.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Falls Sie die in dieser Schnellstartanleitung erstellten Azure-Ressourcen nicht mehr benötigen, können Sie sie mit der Azure CLI löschen.

> [!IMPORTANT]
> Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen.

So löschen Sie eine Ressourcengruppe anhand des Namens:
1. Führen Sie den Befehl [az group delete](/cli/azure/group#az-group-delete) aus. Die von Ihnen erstellte Ressourcengruppe, der IoT-Hub und die Geräteregistrierung werden entfernt.

    ```azurecli
    az group delete --name MyResourceGroup
    ```
1. Führen Sie den Befehl [az group list](/cli/azure/group#az-group-list) aus, um sich zu vergewissern, dass die Ressourcengruppe gelöscht wurde.  

    ```azurecli
    az group list
    ```

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie einen grundlegenden Workflow für Azure IoT-Anwendungen zum sicheren Verbinden eines Geräts mit der Cloud und zum Senden von Telemetriedaten vom Gerät zur Cloud kennengelernt. Sie haben mit der Azure CLI einen IoT Hub und ein Gerät erstellt. Anschließend haben Sie mit dem Azure IoT Python SDK ein simuliertes Gerät erstellt und Telemetriedaten an den Hub gesendet. 

Im nächsten Schritt erkunden Sie das Azure IoT Python SDK mithilfe von Anwendungsbeispielen.

- [Asynchrone Beispiele](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/async-hub-scenarios): Dieses Verzeichnis enthält asynchrone Python-Beispiele für zusätzliche IoT Hub-Szenarien.
- [Synchrone Beispiele](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/sync-samples): Dieses Verzeichnis enthält Python-Beispiele für die Verwendung mit Python 2.7 und synchrone Kompatibilitätsszenarien für Python 3.5 und höher.
- [IoT Edge-Beispiele](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/async-edge-scenarios): Dieses Verzeichnis enthält Python-Beispiele zum Arbeiten mit Edge-Modulen und nachgeschalteten Geräten.