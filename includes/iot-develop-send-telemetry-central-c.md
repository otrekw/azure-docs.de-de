---
title: Datei einfügen
description: Datei einfügen
author: timlt
ms.service: iot-develop
ms.topic: include
ms.date: 05/06/2021
ms.author: timlt
ms.custom: include file
ms.openlocfilehash: 9ba8a90dcd2043fc0f54278a3f1b807add819c66
ms.sourcegitcommit: e6de87b42dc320a3a2939bf1249020e5508cba94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2021
ms.locfileid: "114712992"
---
[![Code durchsuchen](../articles/iot-develop/media/common/browse-code.svg)](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/pnp/pnp_temperature_controller)

In dieser Schnellstartanleitung lernen Sie einen einfachen Entwicklungsworkflow für Azure IoT-Anwendungen kennen. Zunächst erstellen Sie eine Azure IoT Central-Anwendung zum Hosten von Geräten. Anschließend verwenden Sie ein Azure IoT-Geräte-SDK-Beispiel, um einen simulierten Temperaturregler auszuführen, ihn sicher mit IoT Central zu verbinden und Telemetriedaten zu senden.

## <a name="prerequisites"></a>Voraussetzungen
- Dieser Schnellstart kann unter Linux oder Windows ausgeführt werden. In den Shellbefehlen wird das Linux-Standardpfadtrennzeichen `/` verwendet. Ersetzen Sie bei Verwendung von Windows diese Trennzeichen durch das Windows-Pfadtrennzeichen `\`.

Installieren Sie die übrigen erforderlichen Komponenten für Ihr Betriebssystem.

### <a name="linux"></a>Linux
Die Schritte in diesem Tutorial wurden unter Ubuntu Linux 18.04 getestet.

Installieren Sie die folgende Software in Ihrer lokalen Linux-Umgebung, um diesen Schnellstart unter Linux auszuführen:

Installieren Sie **GCC**, **Git**, **CMake** und alle erforderlichen Abhängigkeiten mithilfe des Befehls `apt-get`:

```sh
sudo apt-get update
sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
```

Überprüfen Sie, ob die Version von `cmake` höher als **2.8.12** und die Version von **GCC** höher als **4.4.7** ist.

```sh
cmake --version
gcc --version
```

### <a name="windows"></a>Windows
Installieren Sie Visual Studio 2019 zum Abschließen dieser Schnellstartanleitung unter Windows, und fügen Sie die erforderlichen Komponenten für die C- und C++-Entwicklung hinzu.

1. Installieren Sie für neue Benutzer [Visual Studio (Community, Professional oder Enterprise) 2019](https://visualstudio.microsoft.com/downloads/). Laden Sie die gewünschte Edition für die Installation herunter, und starten Sie das Installationsprogramm.
    > [!NOTE]
    > Wählen Sie für vorhandene Visual Studio 2019-Benutzer das Windows-**Startmenü** aus, geben Sie *Visual Studio-Installer* ein, und starten Sie das Installationsprogramm.
1. Wählen Sie auf der Registerkarte **Workloads** des Installationsprogramms die Workload **Desktopentwicklung mit C++** aus.
1. Wählen Sie auf der Registerkarte **Einzelne Komponenten** des Installationsprogramms die Option **Git für Windows** aus.
1. Führen Sie die Installation aus.

[!INCLUDE [iot-develop-create-central-app-with-device](iot-develop-create-central-app-with-device.md)]

## <a name="run-a-simulated-device"></a>Ausführen eines simulierten Geräts
In diesem Abschnitt konfigurieren Sie Ihre lokale Umgebung, installieren das Azure IoT-C-Geräte-SDK und führen ein Beispiel aus, mit dem ein simulierter Temperaturregler erstellt wird.

### <a name="configure-your-environment"></a>Konfigurieren Ihrer Umgebung

1. Öffnen Sie eine Konsole, um das Azure IoT-C-Geräte-SDK zu installieren, und führen Sie das Codebeispiel aus. Wählen Sie unter Windows **Start** aus, geben Sie *Developer-Eingabeaufforderung für VS 2019* ein, und öffnen Sie die Konsole. Öffnen Sie unter Linux Bash. 

1. Legen Sie mit den entsprechenden Befehle für Ihre Konsole die folgenden Umgebungsvariablen fest. Das simulierte Gerät verwendet diese Werte, um eine Verbindung mit IoT Central herzustellen. Verwenden Sie für `IOTHUB_DEVICE_DPS_ID_SCOPE`, `IOTHUB_DEVICE_DPS_DEVICE_KEY` und `IOTHUB_DEVICE_DPS_DEVICE_ID` die Geräteverbindungswerte, die Sie zuvor gespeichert haben.

    **Befehlszeile**

    ```console
    set IOTHUB_DEVICE_SECURITY_TYPE=DPS
    set IOTHUB_DEVICE_DPS_ID_SCOPE=<application ID scope>
    set IOTHUB_DEVICE_DPS_DEVICE_KEY=<device primary key>
    set IOTHUB_DEVICE_DPS_DEVICE_ID=<your device ID>
    set IOTHUB_DEVICE_DPS_ENDPOINT=global.azure-devices-provisioning.net
    ```

    > [!NOTE]
    > In den Windows-CMD-Befehlen sind die Variablenwerte nicht in Anführungszeichen eingeschlossen.

    **Bash**

    ```bash
    export IOTHUB_DEVICE_SECURITY_TYPE='DPS'
    export IOTHUB_DEVICE_DPS_ID_SCOPE='<application ID scope>'
    export IOTHUB_DEVICE_DPS_DEVICE_KEY='<device primary key>'
    export IOTHUB_DEVICE_DPS_DEVICE_ID='<your device ID>'
    export IOTHUB_DEVICE_DPS_ENDPOINT='global.azure-devices-provisioning.net' 
    ```

### <a name="install-the-sdk-and-samples"></a>Installieren des SDK und der Beispiele

1. Kopieren Sie das Azure IoT-C-Geräte-SDK auf Ihren lokalen Computer.

    ```console
    git clone https://github.com/Azure/azure-iot-sdk-c.git
    ```

1. Navigieren Sie zum Stammordner des SDK, und führen Sie den folgenden Befehl aus, um Abhängigkeiten zu aktualisieren:
    ```console
    cd azure-iot-sdk-c
    git submodule update --init
    ```
    Dieser Vorgang dauert einige Minuten.

1. Führen Sie zum Erstellen des SDK und der Beispiele die folgenden Befehle aus:

    ```console
    cmake -Bcmake -Duse_prov_client=ON -Dhsm_type_symm_key=ON -Drun_e2e_tests=OFF
    cmake --build cmake
    ```

### <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie mit dem entsprechenden Befehl für Ihre Konsole den Beispielcode aus:

    **Befehlszeile**
    ```console
    cmake\iothub_client\samples\pnp\pnp_temperature_controller\Debug\pnp_temperature_controller.exe
    ```

    **Bash**
    ```bash
    cmake/iothub_client/samples/pnp/pnp_temperature_controller/Debug/pnp_temperature_controller
    ```

    Nachdem Ihr simuliertes Gerät eine Verbindung mit der IoT Central-Anwendung hergestellt hat, stellt es eine Verbindung mit der Geräteinstanz her, die Sie in der Anwendung erstellt haben, und beginnt mit dem Senden von Telemetriedaten. Die Verbindungsdetails und die Telemetrieausgabe werden in der Konsole angezeigt: 
    
    ```output
    Info: Initiating DPS client to retrieve IoT Hub connection information
    -> 17:03:08 CONNECT | VER: 4 | KEEPALIVE: 0 | FLAGS: 194 | USERNAME: xxxxxxxxxxxxxxx/registrations/my-sdk-device/api-version=2019-03-31&ClientVersion=1.6.0 | PWD: XXXX | CLEAN: 1
    <- 17:03:09 CONNACK | SESSION_PRESENT: false | RETURN_CODE: 0x0
    -> 17:03:10 SUBSCRIBE | PACKET_ID: 1 | TOPIC_NAME: $dps/registrations/res/# | QOS: 1
    <- 17:03:11 SUBACK | PACKET_ID: 1 | RETURN_CODE: 1
    Info: Provisioning callback indicates success.  iothubUri=iotc-xxxxxxxxxxxx-xxxx-xxxx-xxxxxxxxxxxx.azure-devices.net, deviceId=my-sdk-device
    -> 17:03:27 DISCONNECT
    Info: DPS successfully registered.  Continuing on to creation of IoTHub device client handle.
    Info: Successfully created device client.  Hit Control-C to exit program
    
    Info: Sending serialNumber property to IoTHub
    Info: Sending device information property to IoTHub.  propertyName=swVersion, propertyValue="1.0.0.0"
    Info: Sending device information property to IoTHub.  propertyName=manufacturer, propertyValue="Sample-Manufacturer"
    Info: Sending device information property to IoTHub.  propertyName=model, propertyValue="sample-Model-123"
    Info: Sending device information property to IoTHub.  propertyName=osName, propertyValue="sample-OperatingSystem-name"
    Info: Sending device information property to IoTHub.  propertyName=processorArchitecture, propertyValue="Contoso-Arch-64bit"
    Info: Sending device information property to IoTHub.  propertyName=processorManufacturer, propertyValue="Processor Manufacturer(TM)"
    Info: Sending device information property to IoTHub.  propertyName=totalStorage, propertyValue=10000
    Info: Sending device information property to IoTHub.  propertyName=totalMemory, propertyValue=200
    Info: Sending maximumTemperatureSinceLastReboot property to IoTHub for component=thermostat1
    Info: Sending maximumTemperatureSinceLastReboot property to IoTHub for component=thermostat2
    ```