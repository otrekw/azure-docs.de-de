---
title: 'Schnellstart: Senden von Gerätetelemetriedaten an Azure IoT Central (Python)'
description: In dieser Schnellstartanleitung verwenden Sie das Azure IoT Hub-Geräte-SDK für Python, um Telemetriedaten von einem Gerät an IoT Central zu senden.
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.devlang: python
ms.topic: quickstart
ms.date: 04/27/2021
ms.openlocfilehash: 2464d9d4e6a945620bdbf80728aa3e7df0e2776e
ms.sourcegitcommit: 49bd8e68bd1aff789766c24b91f957f6b4bf5a9b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2021
ms.locfileid: "108226704"
---
# <a name="quickstart-send-telemetry-from-a-device-to-azure-iot-central-python"></a>Schnellstart: Senden von Telemetriedaten von einem Gerät an Azure IoT Central (Python)

**Gilt für**: [Entwickler von Geräteanwendungen](about-iot-develop.md#device-application-development)<br>
**Dauer**: 12 Minuten

In dieser Schnellstartanleitung lernen Sie einen einfachen Entwicklungsworkflow für IoT-Anwendungen kennen. Zunächst erstellen Sie eine Cloudanwendung zum Verwalten von Geräten in Azure IoT Central. Anschließend erstellen Sie mit dem Azure IoT Python SDK ein simuliertes Thermostat, stellen eine Verbindung mit IoT Central her und senden Telemetriedaten.

## <a name="prerequisites"></a>Voraussetzungen
- [Python 3.7](https://www.python.org/downloads/) oder höher Führen Sie zum Überprüfen Ihrer Python-Version `python --version` aus. 

[!INCLUDE [iot-develop-create-central-app-with-device](../../includes/iot-develop-create-central-app-with-device.md)]

## <a name="configure-a-simulated-device"></a>Konfigurieren eines simulierten Geräts
In diesem Abschnitt verwenden Sie die Python SDK-Beispiele, um ein simuliertes Thermostat zu konfigurieren.

1. Öffnen Sie ein Terminal mithilfe von Windows-Eingabeaufforderung, PowerShell oder Bash (für Windows oder Linux). Sie verwenden das Terminal, um das Python SDK zu installieren, Umgebungsvariablen zu aktualisieren und das Python-Codebeispiel auszuführen.

1. Kopieren Sie die [Gerätebeispiele für das Azure IoT Python SDK](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples) auf Ihren lokalen Computer.

    ```console
    git clone https://github.com/Azure/azure-iot-sdk-python
    ```

1. Navigieren Sie zum Verzeichnis mit den Beispielen.

    ```console
    cd azure-iot-sdk-python/azure-iot-device/samples/pnp
    ```
1. Installieren Sie das Azure IoT Python SDK.
    ```console
    pip3 install azure-iot-device
    ```

1. Legen Sie die folgenden Umgebungsvariablen fest, damit das simulierte Gerät eine Verbindung mit IoT Central herstellen kann. Verwenden Sie für `IOTHUB_DEVICE_DPS_ID_SCOPE`, `IOTHUB_DEVICE_DPS_DEVICE_KEY` und `IOTHUB_DEVICE_DPS_DEVICE_ID` die Geräteverbindungswerte, die Sie gespeichert haben.

    **Windows-Eingabeaufforderung**

    ```console
    set IOTHUB_DEVICE_SECURITY_TYPE=DPS
    set IOTHUB_DEVICE_DPS_ID_SCOPE=<application ID scope>
    set IOTHUB_DEVICE_DPS_DEVICE_KEY=<device primary key>
    set IOTHUB_DEVICE_DPS_DEVICE_ID=<your device ID>
    set IOTHUB_DEVICE_DPS_ENDPOINT=global.azure-devices-provisioning.net
    ```

    > [!NOTE]
    > In den Windows-CMD-Befehlen sind die Variablenwerte nicht in Anführungszeichen eingeschlossen.

    **PowerShell**

    ```azurepowershell
    $env:IOTHUB_DEVICE_SECURITY_TYPE='DPS'
    $env:IOTHUB_DEVICE_DPS_ID_SCOPE='<application ID scope>'
    $env:IOTHUB_DEVICE_DPS_DEVICE_KEY='<device primary key>'
    $env:IOTHUB_DEVICE_DPS_DEVICE_ID='<your device ID>'
    $env:IOTHUB_DEVICE_DPS_ENDPOINT='global.azure-devices-provisioning.net'
    ```

    **Bash (Linux oder Windows)**

    ```bash
    export IOTHUB_DEVICE_SECURITY_TYPE='DPS'
    export IOTHUB_DEVICE_DPS_ID_SCOPE='<application ID scope>'
    export IOTHUB_DEVICE_DPS_DEVICE_KEY='<device primary key>'
    export IOTHUB_DEVICE_DPS_DEVICE_ID='<your device ID>'
    export IOTHUB_DEVICE_DPS_ENDPOINT='global.azure-devices-provisioning.net' 
    ```

## <a name="send-telemetry"></a>Senden von Telemetriedaten
Nach dem Konfigurieren Ihres Systems können Sie den Code ausführen. Der Code erstellt ein simuliertes Thermostat, stellt eine Verbindung mit Ihrer IoT Central-Anwendung und Geräteinstanz her und sendet Telemetriedaten.

1. Führen Sie in Ihrem Terminal das folgende Codebeispiel aus. Optional können Sie den Python-Beispielcode in der Python-IDE ausführen.
    ```console
    python temp_controller_with_thermostats.py
    ```

    Nachdem Ihr simuliertes Gerät eine Verbindung mit der IoT Central-Anwendung hergestellt hat, stellt es eine Verbindung mit der Geräteinstanz her, die Sie in der Anwendung erstellt haben, und beginnt mit dem Senden von Telemetriedaten. Die Verbindungsdetails und die Telemetrieausgabe werden in der Konsole angezeigt: 
    
    ```output
    c:\azure-iot-sdk-python\azure-iot-device\samples\pnp>python temp_controller_with_thermostats.py
    Device was assigned
    iotc-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.azure-devices.net
    my-sdk-device
    Updating pnp properties for root interface
    {'serialNumber': 'alohomora'}
    Updating pnp properties for thermostat1
    {'thermostat1': {'maxTempSinceLastReboot': 98.34, '__t': 'c'}}
    Updating pnp properties for thermostat2
    {'thermostat2': {'maxTempSinceLastReboot': 48.92, '__t': 'c'}}
    Updating pnp properties for deviceInformation
    {'deviceInformation': {'swVersion': '5.5', 'manufacturer': 'Contoso Device Corporation', 'model': 'Contoso 4762B-turbo', 'osName': 'Mac Os', 'processorArchitecture': 'x86-64', 'processorManufacturer': 'Intel', 'totalStorage': 1024, 'totalMemory': 32, '__t': 'c'}}
    Listening for command requests and property updates
    Press Q to quit
    Sending telemetry from various components
    Sent message
    {"temperature": 33}
    ```

1. Wählen Sie in IoT Central die Option **Geräte** aus, klicken Sie auf den Gerätenamen, und wählen Sie dann die Registerkarte **Rohdaten** aus. In dieser Ansicht werden die Telemetrierohdaten des simulierten Geräts angezeigt. 

    :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-telemetry-output.png" alt-text="IoT Central: Rohdatenausgabe der Gerätetelemetrie":::
    
    Ihr Gerät ist nun sicher verbunden und sendet Telemetriedaten an Azure IoT.
    
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Falls Sie die in dieser Schnellstartanleitung erstellten IoT Central-Ressourcen nicht mehr benötigen, können Sie sie löschen. Wenn Sie gemäß der Dokumentation in diesem Handbuch fortfahren möchten, können Sie die von Ihnen erstellte Anwendung beibehalten und für andere Beispiele wiederverwenden.

So entfernen Sie die Azure IoT Central-Beispielanwendung und alle zugehörigen Geräte und Ressourcen:
1. Wählen Sie **Verwaltung** > **Ihre Anwendung** aus.
1. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen grundlegenden Workflow für Azure IoT-Anwendungen zum sicheren Verbinden eines Geräts mit der Cloud und zum Senden von Telemetriedaten vom Gerät zur Cloud kennengelernt. Sie haben mit Azure IoT Central eine Anwendung und ein Gerät erstellt. Anschließend haben Sie mit dem Azure IoT Python SDK ein simuliertes Gerät erstellt und Telemetriedaten gesendet. Außerdem haben Sie mit IoT Central die Telemetrie überwacht.

Sehen Sie sich als Nächstes IoT Central als Lösung zum Hosten Ihrer Geräte sowie die Python-Codebeispiele des Azure SDK an.

> [!div class="nextstepaction"]
> [Erstellen einer IoT Central-Anwendung](../iot-central/core/quick-deploy-iot-central.md)
> [!div class="nextstepaction"]
> [Asynchrone Gerätebeispiele](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/async-hub-scenarios)
> [!div class="nextstepaction"]
> [Synchrone Gerätebeispiele](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/sync-samples)
