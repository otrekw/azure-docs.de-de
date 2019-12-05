---
title: Verbinden von IoT Plug & Play-Beispielgerätecode (Vorschauversion) mit IoT Hub (Windows) | Microsoft-Dokumentation
description: Erstellen Sie unter Windows IoT Plug & Play-Beispielgerätecode (Vorschauversion), der eine Verbindung mit einem IoT-Hub herstellt, und führen Sie ihn aus. Verwenden Sie das Tool Azure IoT-Explorer, um die vom Gerät an den Hub gesendeten Informationen anzuzeigen.
author: ChrisGMsft
ms.author: chrisgre
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 0d89be9da55c97a5b49157251896d3a513c2c6db
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74152067"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-running-on-windows-to-iot-hub-c-windows"></a>Schnellstart: Verbinden einer unter Windows ausgeführten IoT Plug & Play-Beispielgeräteanwendung (Vorschauversion) mit IoT Hub (C unter Windows)

In diesem Schnellstart erfahren Sie, wie Sie eine IoT Plug & Play-Beispielgeräteanwendung erstellen, mit Ihrem IoT-Hub verbinden und mit dem Azure IoT-Explorer-Tool die an den Hub gesendeten Informationen anzeigen. Die Beispielanwendung wurde in C geschrieben und ist im Azure IoT Hub-Geräte-SDK für C enthalten. Ein Lösungsentwickler kann das Azure IoT-Explorer-Tool verwenden, um mehr über die Funktionen eines IoT Plug & Play-Geräts zu erfahren, ohne Gerätecode anzeigen zu müssen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

Zum Abschließen dieses Schnellstarts müssen Sie auf Ihrem lokalen Computer die folgende Software installieren:

* [Visual Studio (Community, Professional oder Enterprise):](https://visualstudio.microsoft.com/downloads/) Stellen Sie sicher, dass Sie die Komponente **NuGet-Paket-Manager** und die Workload **Desktopentwicklung mit C++** aktivieren, wenn Sie Visual Studio installieren.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).

### <a name="install-the-azure-iot-explorer"></a>Installieren von Azure IoT-Explorer

Laden Sie das neueste Release von **Azure IoT Explorer** von der [Repositoryseite](https://github.com/Azure/azure-iot-explorer/releases) des Tools herunter, und installieren Sie es, indem Sie die MSI-Datei unter „Assets“ für das neueste Update auswählen.

[!INCLUDE [iot-pnp-prepare-iot-hub-windows.md](../../includes/iot-pnp-prepare-iot-hub-windows.md)]

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung

In diesem Schnellstart bereiten Sie eine Entwicklungsumgebung vor, die Sie zum Klonen und Erstellen des Azure IoT Hub-Geräte-SDK für C verwenden können.

Öffnen Sie eine Eingabeaufforderung in einem Verzeichnis Ihrer Wahl. Führen Sie den folgenden Befehl zum Klonen des GitHub-Repositorys für das [Azure IoT-C-SDK und die zugehörigen Bibliotheken](https://github.com/Azure/azure-iot-sdk-c) an diesem Speicherort aus:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
```

Sie sollten damit rechnen, dass die Ausführung dieses Vorgangs mehrere Minuten in Anspruch nimmt.

## <a name="build-the-code"></a>Erstellen des Codes

Sie verwenden das Geräte-SDK, um den enthaltenen Beispielcode zu erstellen. Die von Ihnen erstellte Anwendung simuliert ein Gerät, mit dem eine Verbindung mit einem IoT-Hub hergestellt wird. Die Anwendung sendet Telemetriedaten und Eigenschaften und empfängt Befehle.

1. Erstellen Sie im Stammordner des Geräte-SDK den Unterordner `cmake`, und navigieren Sie zu diesem Ordner:

    ```cmd\sh
    cd <root folder>\azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Führen Sie die folgenden Befehle aus, um das Geräte-SDK und den generierten Code-Stub zu erstellen:

    ```cmd\sh
    cmake ..
    cmake --build . -- /m /p:Configuration=Release
    ```

    > [!NOTE]
    > Falls CMake Ihren C++-Compiler nicht finden kann, treten beim Ausführen des obigen Befehls Buildfehler auf. Führen Sie den Befehl in diesem Fall an der [Visual Studio-Eingabeaufforderung](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) aus.

## <a name="run-the-device-sample"></a>Ausführen des Gerätebeispiels

Führen Sie eine Beispielanwendung im SDK aus, um ein IoT Plug & Play-Gerät zu simulieren, das Telemetriedaten an Ihren IoT-Hub sendet. Verwenden Sie diese Befehle, um die Beispielanwendung auszuführen, und übergeben Sie die _Geräteverbindungszeichenfolge_ als Parameter.

```cmd\sh
cd digitaltwin_client\samples\digitaltwin_sample_device\Release
copy ..\EnvironmentalSensor.interface.json .
digitaltwin_sample_device.exe "<YourDeviceConnectionString>"
```

Das Gerät ist jetzt bereit zum Empfangen von Befehlen und Eigenschaftenaktualisierungen, und es hat damit begonnen, Telemetriedaten an den Hub zu senden. Behalten Sie die Ausführung des Beispiels während der nächsten Schritte bei.

## <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Verwenden von Azure IoT-Explorer zum Überprüfen des Codes

1. Öffnen Sie den Azure-IoT-Explorer. Die Seite **App-Konfigurationen** wird angezeigt.

1. Geben Sie Ihre _IoT-Hub-Verbindungszeichenfolge_ ein, und klicken Sie auf **Verbinden**.

1. Nachdem Sie eine Verbindung hergestellt haben, wird die Übersichtsseite **Geräte** angezeigt.

1. Um sicherzustellen, dass das Tool die Modelldefinitionen der Schnittstelle von Ihrem Gerät lesen kann, wählen Sie **Einstellungen** aus. Im Menü „Einstellungen“ wird in den Plug & Play-Konfigurationen möglicherweise bereits **On the connected device** (Auf dem verbundenen Gerät) angezeigt. Wenn dies nicht der Fall ist, wählen Sie **+ Add module definition source** (Moduldefinitionsquelle hinzufügen) und dann **On the connected device** (Auf dem verbundenen Gerät) aus, um es hinzuzufügen.

1. Suchen Sie auf der Übersichtsseite **Geräte** nach der Geräteidentität, die Sie zuvor erstellt haben. Wenn die Geräteanwendung weiterhin an der Eingabeaufforderung ausgeführt wird, überprüfen Sie, ob im Azure IoT-Explorer für den **Verbindungsstatus** des Geräts _Verbunden_ angezeigt wird (wählen Sie andernfalls **Aktualisieren** aus, bis dies der Fall ist). Wählen Sie ein Gerät aus, um weitere Details anzuzeigen.

1. Erweitern Sie die Schnittstelle mit der ID **urn:IHR_UNTERNEHMENSNAME:EnvironmentalSensor:1**, um die Schnittstelle und primitive IoT Plug & Play-Typen anzuzeigen: Eigenschaften, Befehle und Telemetriedaten.

1. Wählen Sie die Seite **Telemetrie** aus, und klicken Sie auf _Start_, um die vom Gerät gesendeten Telemetriedaten anzuzeigen.

1. Wählen Sie die Seite **Properties (non-writable)** (Eigenschaften (nicht schreibbar)) aus, um die vom Gerät gemeldeten, nicht schreibbaren Eigenschaften anzuzeigen.

1. Wählen Sie die Seite **Properties (writable)** (Eigenschaften (schreibbar)) aus, um die schreibbaren Eigenschaften anzuzeigen, die Sie aktualisieren können.

1. Erweitern Sie die **name**-Eigenschaft, aktualisieren Sie sie mit einem neuen Namen, und wählen Sie **Update writable property** (Schreibbare Eigenschaft aktualisieren) aus. 

1. Wählen Sie im oberen Seitenbereich die Schaltfläche **Aktualisieren** aus, damit der neue Name in der Spalte **Gemeldete Eigenschaft** angezeigt wird.

1. Wählen Sie die Seite **Befehle** aus, um alle vom Gerät unterstützten Befehle anzuzeigen.

1. Erweitern Sie den Befehl **blink** (Blinken), und legen Sie ein neues Intervall für die Blinkzeit fest. Wählen Sie **Befehl senden** aus, um den Befehl auf dem Gerät aufzurufen.

1. Navigieren Sie zur Eingabeaufforderung des simulierten Geräts, und lesen Sie die ausgegebenen Bestätigungsmeldungen, um sich zu vergewissern, dass die Befehle erwartungsgemäß ausgeführt wurden.

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einem IoT-Hub verbinden. Weitere Informationen zum Erstellen einer Lösung, die mit Ihren IoT Plug & Play-Geräten interagiert, finden Sie unter:

> [!div class="nextstepaction"]
> [Gewusst wie: Herstellen einer Verbindung und Interagieren mit einem IoT Plug & Play Preview-Gerät](howto-develop-solution.md)
