---
title: Verbinden von IoT Plug & Play-Beispielgerätecode (Vorschauversion) mit IoT Hub (Linux) | Microsoft-Dokumentation
description: Erstellen Sie unter Linux IoT Plug & Play-Beispielgerätecode (Vorschauversion), der eine Verbindung mit einem IoT-Hub herstellt. Verwenden Sie die Azure-Befehlszeilenschnittstelle, um die vom Gerät an den Hub gesendeten Informationen anzuzeigen.
author: dominicbetts
ms.author: dobett
ms.date: 12/23/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 8134c0a97f6350cfa2cf616695c5990618455393
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "75531249"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-running-on-linux-to-iot-hub-c-linux"></a>Schnellstart: Verbinden einer unter Linux ausgeführten IoT Plug & Play-Beispielgeräteanwendung (Vorschauversion) mit IoT Hub (C unter Linux)

[!INCLUDE [iot-pnp-quickstarts-2-selector.md](../../includes/iot-pnp-quickstarts-2-selector.md)]

In diesem Schnellstart erfahren Sie, wie Sie eine IoT Plug & Play-Beispielgeräteanwendung unter Linux erstellen, sie mit Ihrem IoT-Hub verbinden und mit der Azure-Befehlszeilenschnittstelle die an den Hub gesendeten Informationen anzeigen. Die Beispielanwendung wird in C geschrieben und ist im Azure IoT-Geräte-SDK für C enthalten. Ein Lösungsentwickler kann mithilfe der Azure-Befehlszeilenschnittstelle mehr über die Funktionen eines IoT Plug & Play-Geräts erfahren, ohne Gerätecode anzeigen zu müssen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

In diesem Schnellstart wird davon ausgegangen, dass Sie Ubuntu Linux verwenden. Die Schritte in diesem Tutorial wurden unter Ubuntu 18.04 getestet.

Zum Abschließen dieses Schnellstarts müssen Sie auf Ihrem lokalen Linux-Computer die folgende Software installieren:

Installieren Sie **GCC**, **Git**, **cmake** und alle Abhängigkeiten mit dem Befehl `apt-get`:

```sh
sudo apt-get update
sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
```

Überprüfen Sie, ob die Version von `cmake` höher als **2.8.12** und die Version von **GCC** höher als **4.4.7** ist.

```sh
cmake --version
gcc --version
```

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung

In diesem Schnellstart bereiten Sie eine Entwicklungsumgebung vor, die Sie zum Klonen und Erstellen des Azure IoT Hub-Geräte-SDK für C verwenden können.

Öffnen Sie eine Eingabeaufforderung in einem Verzeichnis Ihrer Wahl. Führen Sie den folgenden Befehl zum Klonen des GitHub-Repositorys für das [Azure IoT-C-SDK und die zugehörigen Bibliotheken](https://github.com/Azure/azure-iot-sdk-c) an diesem Speicherort aus:

```bash
git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
```

Sie sollten damit rechnen, dass die Ausführung dieses Vorgangs mehrere Minuten in Anspruch nimmt.

## <a name="build-the-code"></a>Erstellen des Codes

Sie verwenden das Geräte-SDK, um den enthaltenen Beispielcode zu erstellen. Die von Ihnen erstellte Anwendung simuliert ein Gerät, mit dem eine Verbindung mit einem IoT-Hub hergestellt wird. Die Anwendung sendet Telemetriedaten und Eigenschaften und empfängt Befehle.

1. Erstellen Sie im Stammordner des Geräte-SDK den Unterordner `cmake`, und navigieren Sie zu diesem Ordner:

    ```bash
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Führen Sie die folgenden Befehle aus, um das Geräte-SDK und den generierten Code-Stub zu erstellen:

    ```bash
    cmake ..
    cmake --build .
    ```

## <a name="update-your-model-repository"></a>Aktualisieren Ihres Modellrepositorys

Bevor Sie das Beispiel ausführen, fügen Sie dem Unternehmensmodellrepository das Gerätefunktionsmodell und die Schnittstellendefinitionen hinzu:

1. Melden Sie sich mit Ihrem Microsoft-Geschäfts-, Schul- oder Unikonto oder ggf. Ihrer Microsoft-Partner-ID beim [Azure Certified for IoT-Portal](https://preview.catalog.azureiotsolutions.com) an.

1. Wählen Sie **Company repository** (Unternehmensrepository) und dann **Capability models** (Funktionsmodelle) aus.

1. Wählen Sie **Neu** und dann **Hochladen** aus.

1. Wählen Sie die Datei `SampleDevice.capabilitymodel.json` im Ordner `digitaltwin_client/samples` im Stammordner des Geräte-SDK aus. Wählen Sie **Öffnen** und dann **Speichern** aus, um die Modelldatei in Ihr Repository hochzuladen.

1. Wählen Sie **Company repository** (Unternehmensrepository) und dann **Schnittstellen** aus.

1. Wählen Sie **Neu** und dann **Hochladen** aus.

1. Wählen Sie die Datei `EnvironmentalSensor.interface.json` im Ordner `digitaltwin_client/samples/digitaltwin_sample_environmental_sensor` im Stammordner des Geräte-SDK aus. Wählen Sie **Öffnen** und dann **Speichern** aus, um die Schnittstellendatei in Ihr Repository hochzuladen.

1. Wählen Sie die Option **Company repository** (Unternehmensrepository) und dann **Verbindungszeichenfolgen**. Notieren Sie sich die erste _Verbindungszeichenfolge für das Unternehmensmodellrepository_, da Sie diese später in diesem Schnellstart verwenden.

## <a name="run-the-device-sample"></a>Ausführen des Gerätebeispiels

Führen Sie eine Beispielanwendung im SDK aus, um ein IoT Plug & Play-Gerät zu simulieren, das Telemetriedaten an Ihren IoT-Hub sendet. So führen Sie die Beispielanwendung aus

1. Navigieren Sie im Ordner `cmake` zu dem Ordner, der die ausführbare Datei enthält:

    ```bash
    cd digitaltwin_client/samples/digitaltwin_sample_device
    ```

1. Führen Sie die ausführbare Datei aus:

    ```bash
    ./digitaltwin_sample_device "<YourDeviceConnectionString>"
    ```

Das Gerät ist jetzt bereit zum Empfangen von Befehlen und Eigenschaftenaktualisierungen, und es hat damit begonnen, Telemetriedaten an den Hub zu senden. Behalten Sie die Ausführung des Beispiels während der nächsten Schritte bei.

### <a name="use-the-azure-iot-cli-to-validate-the-code"></a>Verwenden der Azure IoT-Befehlszeilenschnittstelle zum Überprüfen des Codes

Vergewissern Sie sich nach dem Starten des Geräteclientbeispiels, dass es mit der Azure-Befehlszeilenschnittstelle funktioniert.

Verwenden Sie den folgenden Befehl, um die Telemetriedaten anzuzeigen, die das Beispielgerät sendet. Möglicherweise müssen Sie ein oder zwei Minuten warten, bevor Telemetriedaten in der Ausgabe aufgeführt werden:

```azurecli-interactive
az iot dt monitor-events --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```

Verwenden Sie den folgenden Befehl, um die vom Gerät gesendeten Eigenschaften anzuzeigen:

```azurecli-interactive
az iot dt list-properties --hub-name <YourIoTHubName> --device-id <YourDeviceID> --interface sensor --source private --repo-login "<YourCompanyModelRepositoryConnectionString>"
```
[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einem IoT-Hub verbinden. Weitere Informationen zum Erstellen einer Lösung, die mit Ihren IoT Plug & Play-Geräten interagiert, finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Gewusst wie: Herstellen einer Verbindung und Interagieren mit einem Gerät](howto-develop-solution.md)
