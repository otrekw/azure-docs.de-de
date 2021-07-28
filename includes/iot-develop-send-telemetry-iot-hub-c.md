---
title: Datei einfügen
description: include file
author: timlt
ms.service: iot-develop
ms.topic: include
ms.date: 05/05/2021
ms.author: timlt
ms.custom: include file
ms.openlocfilehash: 1bc45a2f5ff07b64ace84e6c74953deeab5c6aa8
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112041814"
---
## <a name="prerequisites"></a>Voraussetzungen
- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.
- [Git](https://git-scm.com/downloads).
- Azure-Befehlszeilenschnittstelle. In dieser Schnellstartanleitung gibt es zwei Möglichkeiten zum Ausführen von Azure CLI-Befehlen:
    - Verwenden Sie Azure Cloud Shell. Dabei handelt es sich um eine interaktive Shell, mit der CLI-Befehle im Browser ausgeführt werden. Diese Option wird empfohlen, da Sie nichts installieren müssen. Wenn Sie Cloud Shell zum ersten Mal verwenden, melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Führen Sie in der [Cloud Shell-Schnellstartanleitung](../articles/cloud-shell/quickstart.md) die Schritte zum **Starten von Cloud Shell** und **Auswählen der Bash-Umgebung** aus.
    - Führen Sie optional die Azure CLI auf dem lokalen Computer aus. Wenn Azure CLI bereits installiert ist, führen Sie `az upgrade` aus, um die CLI und Erweiterungen auf die aktuelle Version zu aktualisieren. Informationen zur Installation der Azure CLI finden Sie unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

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
1. Führen Sie die Installation aus.

[!INCLUDE [iot-hub-include-create-hub-cli](iot-hub-include-create-hub-cli.md)]

## <a name="run-a-simulated-device"></a>Ausführen eines simulierten Geräts
In diesem Abschnitt verwenden Sie das C SDK zum Senden von Nachrichten vom simulierten Gerät an den IoT-Hub.

### <a name="build-the-sample"></a>Erstellen des Beispiels
1. Öffnen Sie eine Konsole, um das Azure IoT-C-Geräte-SDK zu installieren, und führen Sie das Codebeispiel aus. Wählen Sie unter Windows **Start** aus, geben Sie *Developer-Eingabeaufforderung für VS 2019* ein, und öffnen Sie die Konsole. Öffnen Sie unter Linux Bash.
    > [!NOTE]
    > Sie sollten nun zwei Konsolenfenster geöffnet haben: das soeben geöffnete und die Cloud Shell- oder CLI-Konsole, die Sie zuvor zum Eingeben von CLI-Befehlen verwendet haben.

1. Klonen Sie in Ihrer C-Konsole das Azure IoT-C-Geräte-SDK auf Ihren lokalen Computer:
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
1. Legen Sie die folgenden Umgebungsvariablen fest, damit das simulierte Gerät eine Verbindung mit Azure IoT herstellen kann:
    * Legen Sie eine Umgebungsvariable mit dem Namen `IOTHUB_DEVICE_CONNECTION_STRING` fest. Verwenden Sie als Variablenwert die Geräteverbindungszeichenfolge, die Sie im vorherigen Abschnitt gespeichert haben.
    * Legen Sie eine Umgebungsvariable mit dem Namen `IOTHUB_DEVICE_SECURITY_TYPE` fest. Verwenden Sie als Variable den Literalzeichenfolgenwert `connectionString`.

    **Befehlszeile**

    ```console
    set IOTHUB_DEVICE_CONNECTION_STRING=<your connection string here>
    set IOTHUB_DEVICE_SECURITY_TYPE=connectionString
    ```
    > [!NOTE]
    > In den Windows-CMD-Befehlen sind die Zeichenfolgenwerte nicht in Anführungszeichen eingeschlossen.

    **Bash**

    ```bash
    export IOTHUB_DEVICE_CONNECTION_STRING="<your connection string here>"
    export IOTHUB_DEVICE_SECURITY_TYPE="connectionString"
    ```

### <a name="run-the-code"></a>Ausführen des Codes
1. Führen Sie in der CLI-App den Befehl [az iot hub monitor-events](/cli/azure/iot/hub#az_iot_hub_monitor_events) aus, um mit der Überwachung von Ereignissen auf dem simulierten IoT-Gerät zu beginnen.  Ereignismeldungen werden im Terminal angezeigt, sobald sie eintreffen.

    ```azurecli-interactive
    az iot hub monitor-events --output table --hub-name {YourIoTHubName}
    ```
1. Führen Sie mit dem entsprechenden Befehl für Ihre Konsole den Beispielcode aus:

    **Befehlszeile**
    ```console
    cmake\iothub_client\samples\pnp\pnp_temperature_controller\Debug\pnp_temperature_controller.exe
    ```

    **Bash**
    ```bash
    cmake/iothub_client/samples/pnp/pnp_temperature_controller/Debug/pnp_temperature_controller
    ```
    > [!NOTE]
    > In diesem Codebeispiel wird Azure IoT Plug & Play verwendet. Dadurch wird die Integration intelligenter Geräte in Ihre Lösungen ohne manuelle Konfiguration ermöglicht.  In den meisten Beispielen in dieser Dokumentation wird standardmäßig IoT Plug & Play verwendet. Weitere Informationen zu den Vorteilen und Einsatzmöglichkeiten von IoT Plug & Play finden Sie unter [Was ist IoT Plug & Play?](../articles/iot-pnp/overview-iot-plug-and-play.md).

    Nachdem Ihr simuliertes Gerät eine Verbindung mit der IoT Central-Anwendung hergestellt hat, stellt es eine Verbindung mit der Geräteinstanz her, die Sie in der Anwendung erstellt haben, und beginnt mit dem Senden von Telemetriedaten. Die Verbindungsdetails und die Telemetrieausgabe werden in der Konsole angezeigt: 
    
    ```output
    Starting event monitor, use ctrl-c to stop...
    event:
      component: ''
      interface: dtmi:com:example:TemperatureController;1
      module: ''
      origin: myDevice
      payload: '{"workingSet":1251}'
    
    event:
      component: thermostat1
      interface: dtmi:com:example:TemperatureController;1
      module: ''
      origin: myDevice
      payload: '{"temperature":22.00}'
    ```