---
title: Verbinden von IoT Plug & Play Preview-Gerätecode mit IoT Hub | Microsoft-Dokumentation
description: Verwenden Sie C# (.NET), um IoT Plug & Play Preview-Beispielgerätecode zu erstellen und auszuführen, der eine Verbindung mit einer IoT Hub-Instanz herstellt. Verwenden Sie das Tool Azure IoT-Explorer, um die vom Gerät an den Hub gesendeten Informationen anzuzeigen.
author: dominicbetts
ms.author: dobett
ms.date: 12/27/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 90d39635ac6302f816f39ca19cc00a39cfbbf850
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "77121018"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-to-iot-hub-c"></a>Schnellstart: Verbinden einer IoT Plug & Play Preview-Beispielgeräteanwendung mit IoT Hub (C#)

[!INCLUDE [iot-pnp-quickstarts-2-selector.md](../../includes/iot-pnp-quickstarts-2-selector.md)]

In diesem Schnellstart erfahren Sie, wie Sie eine IoT Plug & Play-Beispielgeräteanwendung erstellen, mit Ihrem IoT-Hub verbinden und mit dem Azure IoT-Explorer-Tool die an den Hub gesendeten Informationen anzeigen. Die Beispielanwendung ist in C# (mit .NET) geschrieben und wird im Rahmen der Sammlung „Azure IoT-Beispiele für C# (.NET)“ bereitgestellt. Ein Lösungsentwickler kann das Azure IoT-Explorer-Tool verwenden, um mehr über die Funktionen eines IoT Plug & Play-Geräts zu erfahren, ohne Gerätecode anzeigen zu müssen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

Um diese Schnellstartanleitung abzuschließen, müssen Sie .NET Core 3.0 auf Ihrem Entwicklungscomputer installieren. Sie können diese Version des .NET Core SDK für verschiedene Plattformen unter [Herunterladen von .NET Core 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0) herunterladen.

Überprüfen Sie die auf Ihrem Entwicklungscomputer installierte .NET-Version, indem Sie in einem lokalen Terminalfenster den folgenden Befehl ausführen: 

```cmd/sh
dotnet --version
```

### <a name="install-the-azure-iot-explorer"></a>Installieren von Azure IoT-Explorer

Laden Sie das neueste Release von **Azure IoT Explorer** von der [Repositoryseite](https://github.com/Azure/azure-iot-explorer/releases) des Tools herunter, und installieren Sie es, indem Sie die MSI-Datei unter „Assets“ für das neueste Update auswählen.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Führen Sie den folgenden Befehl aus, um die _IoT-Hub-Verbindungszeichenfolge_ für Ihren Hub abzurufen (notieren Sie sie für die spätere Verwendung):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung

In dieser Schnellstartanleitung bereiten Sie eine Entwicklungsumgebung vor, die Sie zum Klonen und Erstellen der Azure IoT-Beispiele für C# (.NET) verwenden können.

Öffnen Sie eine Eingabeaufforderung in einem Verzeichnis Ihrer Wahl. Führen Sie den folgenden Befehl aus, um das GitHub-Repository [Azure IoT Samples for C# (.NET)](https://github.com/Azure-Samples/azure-iot-samples-csharp) an diesem Speicherort zu klonen:

```cmd/sh
git clone https://github.com/Azure-Samples/azure-iot-samples-csharp
```

Dieser Vorgang kann mehrere Minuten dauern.

## <a name="run-the-device-sample"></a>Ausführen des Gerätebeispiels

Mit dem geklonten Beispielcode erstellen Sie eine Anwendung, die ein Gerät simuliert, das eine Verbindung mit einem IoT-Hub herstellt. Die Anwendung sendet Telemetriedaten und Eigenschaften und empfängt Befehle.

1. Wechseln Sie in einem lokalen Terminalfenster zum Ordner Ihres geklonten Repositorys, und navigieren Sie zum Ordner **azure-iot-samples-csharp/digitaltwin/Samples/device/EnvironmentalSensorSample**. 

1. Konfigurieren Sie die _Geräte-Verbindungszeichenfolge_:

    ```cmd/sh
    set DIGITAL_TWIN_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Führen Sie eine Beispielanwendung aus, um ein IoT Plug & Play-Gerät zu simulieren, das Telemetriedaten an Ihren IoT-Hub sendet. Verwenden Sie im selben Terminalfenster den folgenden Befehl, um die erforderlichen Pakete zu erstellen und die Beispielanwendung auszuführen:

    ```cmd\sh
    dotnet run --framework=netcoreapp3.0
    ```

Sie werden in Meldungen darüber informiert, dass das Gerät erfolgreich registriert wurde und auf Updates aus der Cloud wartet. Das bedeutet, dass das Gerät jetzt zum Empfangen von Befehlen und Eigenschaftenaktualisierungen bereit ist und mit dem Senden von Telemetriedaten an den Hub begonnen hat. Behalten Sie die Ausführung des Beispiels während der nächsten Schritte bei.

## <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Verwenden von Azure IoT-Explorer zum Überprüfen des Codes

[!INCLUDE [iot-pnp-iot-explorer-1.md](../../includes/iot-pnp-iot-explorer-1.md)]

4. Um sicherzustellen, dass das Tool die Modelldefinitionen der Schnittstelle von Ihrem Gerät lesen kann, wählen Sie **Einstellungen** aus. Im Menü „Einstellungen“ wird in den Plug & Play-Konfigurationen möglicherweise bereits **On the connected device** (Auf dem verbundenen Gerät) angezeigt. Wenn dies nicht der Fall ist, wählen Sie **+ Add module definition source** (Moduldefinitionsquelle hinzufügen) und dann **On the connected device** (Auf dem verbundenen Gerät) aus, um es hinzuzufügen.

1. Suchen Sie auf der Übersichtsseite **Geräte** nach der Geräteidentität, die Sie zuvor erstellt haben. Wenn die Geräteanwendung weiterhin an der Eingabeaufforderung ausgeführt wird, überprüfen Sie, ob im Azure IoT-Explorer für den **Verbindungsstatus** des Geräts _Verbunden_ angezeigt wird (wählen Sie andernfalls **Aktualisieren** aus, bis dies der Fall ist). Wählen Sie ein Gerät aus, um weitere Details anzuzeigen.

1. Erweitern Sie die Schnittstelle mit der ID **urn:csharp_sdk_sample:EnvironmentalSensor:1**, um die Schnittstelle und primitive IoT Plug & Play-Typen anzuzeigen: Eigenschaften, Befehle und Telemetriedaten.

[!INCLUDE [iot-pnp-iot-explorer-2.md](../../includes/iot-pnp-iot-explorer-2.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einem IoT-Hub verbinden. Weitere Informationen zum Erstellen einer Lösung, die mit Ihren IoT Plug & Play-Geräten interagiert, finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Gewusst wie: Herstellen einer Verbindung und Interagieren mit einem IoT Plug & Play Preview-Gerät](howto-develop-solution.md)
