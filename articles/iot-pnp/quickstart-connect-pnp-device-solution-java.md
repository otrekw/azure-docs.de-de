---
title: Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit ihrer Azure IoT-Lösung verbunden ist | Microsoft-Dokumentation
description: Verwenden Sie Java zum Herstellen einer Verbindung und zum Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit Ihrer Azure IoT-Lösung verbunden ist.
author: baanders
ms.author: baanders
ms.date: 12/27/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 217dfe125dcacae5d50645275b20421a23169cb9
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75550872"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-java"></a>Schnellstart: Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit Ihrer Lösung verbunden ist (Java)

[!INCLUDE [iot-pnp-quickstarts-3-selector.md](../../includes/iot-pnp-quickstarts-3-selector.md)]

IoT Plug & Play Preview vereinfacht das IoT, indem es Ihnen die Interaktion mit den Funktionen eines Geräts ohne Kenntnisse der vorliegenden Geräteimplementierung ermöglicht. In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Java eine Verbindung mit einem IoT Plug & Play-Gerät herstellen, das mit Ihrer Lösung verbunden ist, und wie Sie dieses Gerät steuern.

## <a name="prerequisites"></a>Voraussetzungen

Um diese Schnellstartanleitung abzuschließen, benötigen Sie Java SE 8 auf Ihrem Entwicklungscomputer. Sie müssen auch Maven 3 installieren.

Informationen zur Einrichtung finden Sie im Microsoft Azure IoT-Geräte-SDK für Java unter [Vorbereiten Ihrer Entwicklungsumgebung](https://github.com/Azure/azure-iot-sdk-java/blob/preview/doc/java-devbox-setup.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Führen Sie den folgenden Befehl aus, um die _IoT-Hub-Verbindungszeichenfolge_ für Ihren Hub abzurufen (notieren Sie sie für die spätere Verwendung):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="run-the-sample-device"></a>Ausführen des Beispielgeräts

In dieser Schnellstartanleitung verwenden Sie einen in Java geschriebenen Beispielumgebungssensor als IoT Plug & Play-Gerät. In den folgenden Anweisungen erfahren Sie, wie Sie das Gerät installieren und ausführen:

1. Öffnen Sie ein Terminalfenster in einem Verzeichnis Ihrer Wahl. Führen Sie den folgenden Befehl aus, um das GitHub-Repository [Azure IoT Samples for Java](https://github.com/Azure-Samples/azure-iot-samples-java) an diesem Speicherort zu klonen:

    ```cmd/sh
    git clone https://github.com/Azure-Samples/azure-iot-samples-java
    ```

1. Dieses Terminalfenster wird jetzt als _Geräteterminal_ verwendet. Wechseln Sie zum Ordner Ihres geklonten Repositorys, und navigieren Sie zum Ordner **/azure-iot-samples-java/digital-twin/Samples/device/JdkSample**. Führen Sie dann den folgenden Befehl aus, um die erforderlichen Bibliotheken zu installieren und die simulierte Geräteanwendung zu erstellen:

    ```cmd/sh
    mvn clean install -DskipTests
    ```

1. Konfigurieren Sie die _Geräte-Verbindungszeichenfolge_:

    ```cmd/sh
    set DIGITAL_TWIN_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Führen Sie den folgenden Befehl aus, um das Beispiel aus dem Geräteordner auszuführen.

    ```cmd/sh
    java -jar environmental-sensor-sample\target\environmental-sensor-sample-with-deps.jar
    ```

1. Sie werden in Meldungen darüber informiert, dass das Gerät verbunden ist, verschiedene Einrichtungsschritte ausgeführt werden und auf Dienstupdates gewartet wird. Anschließend werden Telemetrieprotokolle angezeigt. Das bedeutet, dass das Gerät jetzt zum Empfangen von Befehlen und Eigenschaftenaktualisierungen bereit ist und mit dem Senden von Telemetriedaten an den Hub begonnen hat. Behalten Sie die Ausführung des Beispiels während der nächsten Schritte bei. Schließen Sie dieses Terminal nicht. Sie benötigen es später, um zu prüfen, ob die Dienstbeispiele funktionieren.

## <a name="run-the-sample-solution"></a>Ausführen der Beispiellösung

In dieser Schnellstartanleitung verwenden Sie eine IoT-Beispiellösung in Java, um mit dem Beispielgerät zu interagieren.

1. Öffnen Sie ein weiteres Terminalfenster (Ihr _Dienstterminal_). Wechseln Sie zum Ordner Ihres geklonten Repositorys, und navigieren Sie zum Ordner **/azure-iot-samples-java\digital-twin\Samples\service\JdkSample**.

1. Führen Sie den folgenden Befehl aus, um die erforderlichen Bibliotheken zu installieren und das Dienstbeispiel zu erstellen:

    ```cmd/sh
    mvn clean install -DskipTests
    ```

1. Konfigurieren Sie die _IoT-Hub-Verbindungszeichenfolge_ und die _Geräte-ID_, damit der Dienst eine Verbindung damit herstellen kann:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIoTHubConnectionString>
    set DEVICE_ID=<YourDeviceID>
    ```

### <a name="read-a-property"></a>Lesen einer Eigenschaft

1. Als Sie über das Terminal eine Verbindung mit dem _Gerät_ hergestellt haben, wurde die folgende Meldung zum Onlinestatus des Geräts angezeigt. Die Eigenschaft `state`, mit der angegeben wird, ob das Gerät online ist, ist _TRUE_:

    ```cmd/sh
    State of environmental sensor was set to true
    ```

1. Wechseln Sie zum _Dienstterminal_, und verwenden Sie den folgenden Befehl, um das Dienstbeispiel für das Lesen von Geräteinformationen auszuführen:

    ```cmd/sh
    java -jar get-digital-twin/target/get-digital-twin-with-deps.jar
    ```

1. Scrollen Sie in der Ausgabe des _Dienstterminals_ zur Komponente `environmentalSensor`. Wie Sie sehen, wurde die `state`-Eigenschaft als _TRUE_ gemeldet:
    ```JSON
    "environmentalSensor" : {
      "name" : "environmentalSensor",
      "properties" : {
        "state" : {
          "reported" : {
            "value" : true,
            "desiredState" : null
          },
          "desired" : null
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

1. Verwenden Sie den folgenden Befehl, um das Dienstbeispiel für das Aktualisieren der Eigenschaft auszuführen:

    ```cmd/sh
    java -jar update-digital-twin/target/update-digital-twin-with-deps.jar
    ```

1. In der Ausgabe des _Dienstterminals_ werden die aktualisierten Geräteinformationen angezeigt. Scrollen Sie zur Komponente `environmentalSensor`, um den neuen Helligkeitswert 42 anzuzeigen.

    ```json
    "environmentalSensor": {
        "name": "environmentalSensor",
        "properties": {
            "brightness": {
                "reported": null,
                "desired": {
                    "value": "42"
                }
            },
    ```

1. In Ihrem _Geräteterminal_ sehen Sie, dass das Gerät die Aktualisierung erhalten hat:

    ```cmd/sh
    OnPropertyUpdate called: propertyName=brightness, reportedValue=null, desiredVersion=2, desiredValue={"value":"42"}
    Report property: propertyName=brightness, reportedValue={"value":"42"}, desiredVersion=2 was DIGITALTWIN_CLIENT_OK
    ```
2. Kehren Sie zu Ihrem _Dienstterminal_ zurück, und führen Sie den folgenden Befehl zum Abrufen von Geräteinformationen erneut aus, um zu überprüfen, ob die Eigenschaft aktualisiert wurde.
    
    ```cmd/sh
    java -jar get-digital-twin/target/get-digital-twin-with-deps.jar
    ```
3. In der Ausgabe des _Dienstterminals_ wird unter der Komponente `environmentalSensor` der aktualisierte Helligkeitswert gemeldet. Hinweis: Es kann eine Weile dauern, bis das Gerät die Aktualisierung abgeschlossen hat. Sie können diesen Schritt wiederholen, bis das Gerät die Aktualisierung der Eigenschaft tatsächlich verarbeitet hat.
    
    ```json
    "environmentalSensor" : {
      "name" : "environmentalSensor",
      "properties" : {
        "brightness" : {
          "reported" : {
            "value" : {
              "value" : "42"
            },
            "desiredState" : {
              "code" : 200,
              "version" : 2,
              "description" : "OK"
            }
          },
          "desired" : {
            "value" : "42"
          }
        },
        "state" : {
          "reported" : {
            "value" : true,
            "desiredState" : null
          },
          "desired" : null
        }
      }
    },       
    ```

### <a name="invoke-a-command"></a>Aufrufen eines Befehls

1. Wechseln Sie zum _Dienstterminal_, und legen Sie die folgenden Variablen fest, um zu definieren, welcher Befehl aufgerufen werden soll:
    ```cmd/sh
    set INTERFACE_INSTANCE_NAME=environmentalSensor
    set COMMAND_NAME=blink
    set PAYLOAD=10
    ```

1. Verwenden Sie den folgenden Befehl, um das Dienstbeispiel für das Aufrufen des Befehls auszuführen:

    ```cmd/sh
    java -jar invoke-digital-twin-command/target/invoke-digital-twin-command-with-deps.jar
    ```

1. Die Ausgabe im _Dienstterminal_ sollte die folgende Bestätigung enthalten:

    ```cmd/sh
    Invoking blink on device <YourDeviceID> with interface instance name environmentalSensor
    Command invoked on the device successfully, the returned status was 200 and the request id was <some ID value>
    The returned PAYLOAD was
    ```

1. Im _Geräteterminal_ sehen Sie, dass der Befehl bestätigt wurde:

    ```cmd/sh
    OnCommandReceived called: commandName=blink, requestId=<some ID value>, commandPayload="10"
    EnvironmentalSensor is blinking every 10 seconds.
    ```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einer IoT-Lösung verbinden. Weitere Informationen zum Erstellen einer Lösung, die mit Ihren IoT Plug & Play-Geräten interagiert, finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Gewusst wie: Herstellen einer Verbindung und Interagieren mit einem Gerät](howto-develop-solution.md)
