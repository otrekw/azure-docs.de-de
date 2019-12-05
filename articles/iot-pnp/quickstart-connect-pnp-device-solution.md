---
title: Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit ihrer Azure IoT-Lösung verbunden ist | Microsoft-Dokumentation
description: Verwenden Sie Node.js zum Herstellen einer Verbindung und zum Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit ihrer Azure IoT-Lösung verbunden ist.
author: miagdp
ms.author: miag
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 3275c01059a61e4eb8591948695656f4e4b722ed
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74152128"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-nodejs"></a>Schnellstart: Interagieren mit einem IoT Plug & Play-Gerät (Vorschauversion), das mit Ihrer Lösung (Node.js) verbunden ist

IoT Plug & Play Preview vereinfacht das IoT, indem es Ihnen die Interaktion mit den Funktionen eines Geräts ohne Kenntnisse der vorliegenden Geräteimplementierung ermöglicht. In diesem Schnellstart erfahren Sie, wie Sie mit Node.js eine Verbindung mit einem IoT Plug & Play-Gerät, das mit Ihrer Lösung verbunden ist, herstellen und dieses steuern.

## <a name="prerequisites"></a>Voraussetzungen

Um diesen Schnellstart abzuschließen, benötigen Sie Node.js auf Ihrem Entwicklungscomputer. Sie können die neueste empfohlene Version für mehrere Plattformen von [nodejs.org](https://nodejs.org) herunterladen.

Mit dem folgenden Befehl können Sie die aktuelle Node.js-Version auf Ihrem Entwicklungscomputer überprüfen:

```cmd/sh
node --version
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-pnp-prepare-iot-hub-windows.md](../../includes/iot-pnp-prepare-iot-hub-windows.md)]

## <a name="connect-your-device"></a>Verbinden Ihres Geräts

In diesem Schnellstart verwenden Sie einen Beispielumgebungssensor, der in Node.js als IoT Plug & Play-Gerät geschrieben wird. In den folgenden Anweisungen erfahren Sie, wie Sie das Gerät installieren und ausführen:

1. Öffnen Sie ein Terminalfenster in einem Verzeichnis Ihrer Wahl. Führen Sie den folgenden Befehl zum Klonen des GitHub-Repositorys [Azure IoT Samples for Node.js](https://github.com/azure-samples/azure-iot-samples-node) an diesem Speicherort aus:

    ```cmd/sh
    git clone https://github.com/azure-samples/azure-iot-samples-node
    ```

1. Dieses Terminalfenster wird jetzt als _Geräteterminal_ verwendet. Wechseln Sie zu Ihrem geklonten Repository, und navigieren Sie zum Ordner **/azure-iot-samples-node/digital-twins/Quickstarts/Device**. Installieren Sie alle Abhängigkeiten, indem Sie den folgenden Befehl ausführen:

    ```cmd/sh
    npm install
    ```

1. Konfigurieren Sie die _Geräte-Verbindungszeichenfolge_:

    ```cmd/sh
    set DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Führen Sie das Beispiel mit dem folgenden Befehl aus:

    ```cmd/sh
    node sample_device.js
    ```

1. Es werden Nachrichten darüber angezeigt, dass das Gerät Informationen gesendet und sich selbst als „online“ gemeldet hat. Dies deutet darauf hin, dass das Gerät damit begonnen hat, Telemetriedaten an den Hub zu senden und jetzt bereit zum Empfangen von Befehlen und Eigenschaftenaktualisierungen ist. Schließen Sie dieses Terminal nicht. Sie benötigen es später, um zu prüfen, ob die Dienstbeispiele funktionieren.

## <a name="build-the-solution"></a>Erstellen Sie die Lösung.

In diesem Schnellstart verwenden Sie eine Beispiel-IoT-Lösung in Node.js, um mit dem Beispielgerät zu interagieren.

1. Öffnen Sie ein weiteres Terminalfenster (Ihr _Dienstterminal_). Wechseln Sie zum Ordner Ihres geklonten Repositorys, und navigieren Sie zum Ordner **/azure-iot-samples-node/digital-twins/Quickstarts/Service**. Installieren Sie alle Abhängigkeiten, indem Sie den folgenden Befehl ausführen:

    ```cmd/sh
    npm install
    ```

1. Konfigurieren Sie die _IoT-Hub-Verbindungszeichenfolge_, damit der Dienst eine Verbindung damit herstellen kann:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIoTHubConnectionString>
    ```

### <a name="read-a-property"></a>Lesen einer Eigenschaft

1. Wenn Sie das _Gerät_ in seinem Terminal verbunden haben, wurde die folgende Meldung angezeigt, die es als „online“ kennzeichnet:

    ```cmd/sh
    reported state property as online
    ```

1. Öffnen Sie im Ordner **/azure-iot-samples-node/digital-twins/Quickstarts/Service** die Datei **get_digital_twin.js**. Ersetzen Sie den Platzhalter `<DEVICE_ID_GOES_HERE>` durch Ihre Geräte-ID, und speichern Sie die Datei.

1. Wechseln Sie zum _Dienstterminal_, und führen Sie den folgenden Befehl aus, um das Beispiel für das Lesen von Geräteinformationen auszuführen:

    ```cmd/sh
    node get_digital_twin.js
    ```

1. Scrollen Sie in der Ausgabe des _Dienstterminals_ zur Komponente `environmentalSensor`. Sie sehen, dass die `state`-Eigenschaft als _Online_ gemeldet wurde:

    ```JSON
    "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "state": {
          "reported": {
            "value": "online"
          }
        }
      }
    }
    ```

### <a name="update-a-writable-property"></a>Aktualisieren einer schreibbaren Eigenschaft

1. Öffnen Sie die Datei **update_digital_twin_property.js**.

1. Am Anfang der Datei finden Sie eine Reihe von Konstanten, die mit Platzhaltern in Großbuchstaben definiert sind. Ersetzen Sie den Platzhalter `<DEVICE_ID_GOES_HERE>` durch Ihre tatsächliche Geräte-ID, aktualisieren Sie die übrigen Konstanten mit den folgenden Werten, und speichern Sie die Datei:

    ```javascript
    const interfaceInstanceName = 'environmentalSensor';
    const propertyName = 'brightness';
    const propertyValue = 42;
    ```

1. Wechseln Sie zum _Dienstterminal_, und führen Sie den folgenden Befehl aus, um das Beispiel für das Aktualisieren der Eigenschaft auszuführen:

    ```cmd/sh
    node update_digital_twin_property.js
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
            "value": "online"
          }
        }
      }
    }
    ```

1. In Ihrem _Geräteterminal_ sehen Sie, dass das Gerät die Aktualisierung erhalten hat:

    ```cmd/sh
    Received an update for brightness: 42
    updated the property
    ```
2. Kehren Sie zu Ihrem _Dienstterminal_ zurück, und führen Sie den folgenden Befehl zum Abrufen von Geräteinformationen erneut aus, um zu überprüfen, ob die Eigenschaft aktualisiert wurde.
    
    ```cmd/sh
    node get_digital_twin.js
    ```

3. In der Ausgabe des _Dienstterminals_ wird unter der Komponente `environmentalSensor` der aktualisierte Helligkeitswert gemeldet. Hinweis: Es kann eine Weile dauern, bis das Gerät die Aktualisierung abgeschlossen hat. Sie können diesen Schritt wiederholen, bis das Gerät die Aktualisierung der Eigenschaft tatsächlich verarbeitet hat.
    
    ```json
    "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "brightness": {
          "reported": {
            "value": "42",
            "desiredState": {
              "code": 200,
              "version": 2,
              "description": "helpful descriptive text"
            }
          },
          "desired": {
            "value": "42"
          }
        },
        "state": {
          "reported": {
            "value": "online"
          }
        }
      }
    }
    ```

### <a name="invoke-a-command"></a>Aufrufen eines Befehls

1. Öffnen Sie die Datei **invoke_command.js**.

1. Ersetzen Sie am Anfang der Datei den Platzhalter `<DEVICE_ID_GOES_HERE>` durch Ihre tatsächliche Geräte-ID. Aktualisieren Sie die übrigen Konstanten mit den folgenden Werten, und speichern Sie dann die Datei:

    ```javascript
    const interfaceInstanceName = 'environmentalSensor';
    const commandName = 'blink';
    const commandArgument = '<For the environmental sensor, this value does not matter. Any string will do.>'; 
    ```

1. Wechseln Sie zum _Dienstterminal_. Verwenden Sie den folgenden Befehl, um das Beispiel für das Aufrufen des Befehls auszuführen:

    ```cmd/sh
    node invoke_command.js
    ```

1. Die Ausgabe im _Dienstterminal_ sollte die folgende Bestätigung enthalten:

    ```cmd/sh
    invoking command blink on interface instanceenvironmentalSensor for device <device ID>...
    {
      "result": "helpful response text",
      "statusCode": 200,
      "requestId": "<some ID value>",
      "_response": "helpful response text"
    }
    ```

1. Im _Geräteterminal_ sehen Sie, dass der Befehl bestätigt wurde:

    ```cmd/sh
    received command: blink for interfaceInstance: environmentalSensor
    acknowledgement succeeded.
    ```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einer IoT-Lösung verbinden. Weitere Informationen zum Erstellen einer Lösung, die mit Ihren IoT Plug & Play-Geräten interagiert, finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Gewusst wie: Herstellen einer Verbindung und Interagieren mit einem Gerät](howto-develop-solution.md)
