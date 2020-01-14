---
title: Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit ihrer Azure IoT-Lösung verbunden ist | Microsoft-Dokumentation
description: Verwenden Sie C# (.NET) zum Herstellen einer Verbindung und zum Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit Ihrer Azure IoT-Lösung verbunden ist.
author: baanders
ms.author: baanders
ms.date: 12/30/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 9e8bc6c4ad7ed852ddaae2e193b91887fcd92e47
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75550773"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-c"></a>Schnellstart: Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit Ihrer Lösung verbunden ist (C#)

[!INCLUDE [iot-pnp-quickstarts-3-selector.md](../../includes/iot-pnp-quickstarts-3-selector.md)]

IoT Plug & Play Preview vereinfacht das IoT, indem es Ihnen die Interaktion mit den Funktionen eines Geräts ohne Kenntnisse der vorliegenden Geräteimplementierung ermöglicht. In dieser Schnellstartanleitung erfahren Sie, wie Sie mit C# (mit .NET) eine Verbindung mit einem IoT Plug & Play-Gerät herstellen, das mit Ihrer Lösung verbunden ist, und wie Sie dieses Gerät steuern.

## <a name="prerequisites"></a>Voraussetzungen

Um diese Schnellstartanleitung abzuschließen, müssen Sie .NET Core (2.x.x oder 3.x.x) auf Ihrem Entwicklungscomputer installieren. Sie können Ihre bevorzugte Version des .NET Core SDK für verschiedene Plattformen von [Download .NET Core](https://dotnet.microsoft.com/download/dotnet-core/) herunterladen.

Überprüfen Sie die auf Ihrem Entwicklungscomputer installierte .NET-Version, indem Sie in einem lokalen Terminalfenster den folgenden Befehl ausführen: 

```cmd/sh
dotnet --version
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Führen Sie den folgenden Befehl aus, um die _IoT-Hub-Verbindungszeichenfolge_ für Ihren Hub abzurufen (notieren Sie sie für die spätere Verwendung):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="run-the-sample-device"></a>Ausführen des Beispielgeräts

In dieser Schnellstartanleitung verwenden Sie einen in C# geschriebenen Beispielumgebungssensor als IoT Plug & Play-Gerät. In den folgenden Anweisungen erfahren Sie, wie Sie das Gerät installieren und ausführen:

1. Öffnen Sie ein Terminalfenster in einem Verzeichnis Ihrer Wahl. Führen Sie den folgenden Befehl aus, um das GitHub-Repository [Azure IoT Samples for C# (.NET)](https://github.com/Azure-Samples/azure-iot-samples-csharp) an diesem Speicherort zu klonen:

    ```cmd/sh
    git clone https://github.com/Azure-Samples/azure-iot-samples-csharp
    ```

1. Dieses Terminalfenster wird jetzt als _Geräteterminal_ verwendet. Wechseln Sie zum Ordner Ihres geklonten Repositorys, und navigieren Sie zum Ordner **/azure-iot-samples-csharp/digitaltwin/Samples/device/EnvironmentalSensorSample**.

1. Konfigurieren Sie die _Geräte-Verbindungszeichenfolge_:

    ```cmd/sh
    set DIGITAL_TWIN_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Führen Sie den folgenden Befehl aus, um die erforderlichen Pakete zu erstellen und das Beispiel auszuführen:

    ```cmd\sh
        dotnet run
    ```

1. Sie werden in Meldungen darüber informiert, dass das Gerät erfolgreich registriert wurde und auf Updates aus der Cloud wartet. Das bedeutet, dass das Gerät jetzt zum Empfangen von Befehlen und Eigenschaftenaktualisierungen bereit ist und mit dem Senden von Telemetriedaten an den Hub begonnen hat. Schließen Sie dieses Terminal nicht. Sie benötigen es später, um zu prüfen, ob die Dienstbeispiele funktionieren.

## <a name="run-the-sample-solution"></a>Ausführen der Beispiellösung

In dieser Schnellstartanleitung verwenden Sie eine IoT-Beispiellösung in C#, um mit dem Beispielgerät zu interagieren.

1. Öffnen Sie ein weiteres Terminalfenster (Ihr _Dienstterminal_). Wechseln Sie zum Ordner Ihres geklonten Repositorys, und navigieren Sie zum Ordner **/azure-iot-samples-csharp/digitaltwin/Samples/service**.

1. Konfigurieren Sie die _IoT-Hub-Verbindungszeichenfolge_ und die _Geräte-ID_, damit der Dienst eine Verbindung damit herstellen kann:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIoTHubConnectionString>
    set DEVICE_ID=<YourDeviceID>
    ```

### <a name="read-a-property"></a>Lesen einer Eigenschaft

1. Wenn Sie das _Gerät_ in seinem Terminal verbunden haben, wurde die folgende Meldung angezeigt, die es als „online“ kennzeichnet:

    ```cmd/sh
    Waiting to receive updates from cloud...
    ```

1. Wechseln Sie zum _Dienstterminal_, und verwenden Sie die folgenden Befehle, um das Beispiel für das Lesen von Geräteinformationen auszuführen:

    ```cmd/sh
    cd GetDigitalTwin
    dotnet run
    ```

1. Scrollen Sie in der Ausgabe des _Dienstterminals_ zur Komponente `environmentalSensor`. Wie Sie sehen, wurde die Eigenschaft `state`, mit der angegeben wird, ob das Gerät online ist, als _TRUE_ gemeldet:

    ```JSON
    "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "state": {
          "reported": {
            "value": true
          }
        }
      }
    }
    ```

### <a name="update-a-writable-property"></a>Aktualisieren einer schreibbaren Eigenschaft

1. Wechseln Sie zum _Dienstterminal_, und legen Sie die folgenden Variablen fest, um zu definieren, welche Eigenschaft aktualisiert werden soll:
    ```cmd/sh
    set INTERFACE_INSTANCE_NAME=environmentalSensor
    set PROPERTY_NAME=brightness
    set PROPERTY_VALUE=42
    ```

1. Verwenden Sie die folgenden Befehle, um das Beispiel für das Aktualisieren der Eigenschaft auszuführen:

    ```cmd/sh
    cd ..\UpdateProperty
    dotnet run
    ```

1. In der Ausgabe des _Dienstterminals_ werden die aktualisierten Geräteinformationen angezeigt. Scrollen Sie zur Komponente `environmentalSensor`, um den neuen Helligkeitswert 42 anzuzeigen.

    ```json
        "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "brightness": {
          "desired": {
            "value": "42"
          }
        },
        "state": {
          "reported": {
            "value": true
          }
        }
      }
    }
    ```

1. In Ihrem _Geräteterminal_ sehen Sie, dass das Gerät die Aktualisierung erhalten hat:

    ```cmd/sh
    Received updates for property 'brightness'
    Desired brightness = '"42"'.
    Reported brightness = ''.
    Version is '2'.
    Sent pending status for brightness property.
    Sent completed status for brightness property.
    ```
2. Kehren Sie zu Ihrem _Dienstterminal_ zurück, und führen Sie die folgenden Befehle zum Abrufen von Geräteinformationen erneut aus, um zu überprüfen, ob die Eigenschaft aktualisiert wurde.
    
    ```cmd/sh
    cd ..\GetDigitalTwin
    dotnet run
    ```
3. In der Ausgabe des _Dienstterminals_ wird unter der Komponente `environmentalSensor` der aktualisierte Helligkeitswert gemeldet. Hinweis: Es kann eine Weile dauern, bis das Gerät die Aktualisierung abgeschlossen hat. Sie können diesen Schritt wiederholen, bis das Gerät die Aktualisierung der Eigenschaft tatsächlich verarbeitet hat.
    
    ```json
    "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "brightness": {
          "desired": {
            "value": "42"
          },
          "reported": {
            "value": "42",
            "desiredState": {
              "code": 200,
              "description": "Request completed",
              "version": 2
            }
          }
        },
        "state": {
          "reported": {
            "value": true
          }
        }
      }
    },
    ```

### <a name="invoke-a-command"></a>Aufrufen eines Befehls

1. Wechseln Sie zum _Dienstterminal_, und legen Sie die folgenden Variablen fest, um zu definieren, welcher Befehl aufgerufen werden soll:
    ```cmd/sh
    set INTERFACE_INSTANCE_NAME=environmentalSensor
    set COMMAND_NAME=blink
    ```

1. Verwenden Sie die folgenden Befehle, um das Beispiel für das Aufrufen des Befehls auszuführen:

    ```cmd/sh
    cd ..\InvokeCommand
    dotnet run
    ```

1. Die Ausgabe im _Dienstterminal_ sollte die folgende Bestätigung enthalten:

    ```cmd/sh
    Invoking blink on device <YourDeviceID> with interface instance name environmentalSensor
    Command blink invoked on the device successfully, the returned status was 200 and the request id was <some ID value>
    The returned payload was
    {"description": "abc"}
    Enter any key to finish
    ```

1. Im _Geräteterminal_ sehen Sie, dass der Befehl bestätigt wurde:

    ```cmd/sh
    Command - blink was invoked from the service
    Data - null
    Request Id - <some ID value>.
    ```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einer IoT-Lösung verbinden. Weitere Informationen zum Erstellen einer Lösung, die mit Ihren IoT Plug & Play-Geräten interagiert, finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Gewusst wie: Herstellen einer Verbindung und Interagieren mit einem Gerät](howto-develop-solution.md)
