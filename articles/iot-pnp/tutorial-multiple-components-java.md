---
title: Verbinden von exemplarischem IoT Plug & Play Preview-Komponentengerätecode (Java) mit IoT Hub | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie IoT Plug & Play Preview-Beispielgerätecode (Java) erstellen und ausführen, von dem mehrere Komponenten verwendet werden und eine Verbindung mit einem IoT-Hub hergestellt wird. Verwenden Sie das Tool Azure IoT-Explorer, um die vom Gerät an den Hub gesendeten Informationen anzuzeigen.
author: ericmitt
ms.author: ericmitt
ms.date: 07/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 1d16d8c54939c4f659b6a1530e2d360b957a09ad
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87351884"
---
# <a name="tutorial-connect-a-sample-iot-plug-and-play-preview-multiple-component-device-application-to-iot-hub-java"></a>Tutorial: Herstellen einer Verbindung zwischen einer exemplarischen IoT Plug & Play Preview-Geräteanwendung mit mehreren Komponenten und IoT Hub (Java)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

In diesem Tutorial erfahren Sie, wie Sie eine exemplarische IoT Plug & Play-Geräteanwendung mit mehreren Komponenten erstellen, mit Ihrem IoT-Hub verbinden und die gesendeten Telemetriedaten mithilfe der Azure CLI anzeigen. Die Beispielanwendung wurde in Java geschrieben und ist im Azure IoT-Geräte-SDK für Java enthalten. Ein Lösungsentwickler kann mithilfe der Azure CLI mehr über die Funktionen eines IoT Plug & Play-Geräts erfahren, ohne Gerätecode anzeigen zu müssen.

In diesem Tutorial erfahren Sie, wie Sie eine exemplarische IoT Plug & Play-Geräteanwendung mit Komponenten und Stammschnittstelle erstellen, mit Ihrem IoT-Hub verbinden und mithilfe von Azure IoT-Explorer die an den Hub gesendeten Informationen anzeigen. Die Beispielanwendung wurde in Java geschrieben und ist im Azure IoT-Geräte-SDK für Java enthalten. Ein Lösungsentwickler kann mithilfe von Azure IoT-Explorer mehr über die Funktionen eines IoT Plug & Play-Geräts erfahren, ohne Gerätecode anzeigen zu müssen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

Installieren Sie die folgende Software in Ihrer lokalen Windows-Umgebung, um dieses Tutorial unter Windows ausführen zu können:

* Java SE Development Kit 8. Wählen Sie unter [Langfristiger Java-Support für Azure und Azure Stack](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable) unter **Langfristiger Support** die Option **Java 8** aus.
* [Apache Maven 3](https://maven.apache.org/download.cgi)

### <a name="azure-iot-explorer"></a>Azure IoT-Explorer

Wenn Sie im zweiten Teil dieser Schnellstartanleitung mit dem Beispielgerät interagieren möchten, verwenden Sie das Tool **Azure IoT-Explorer**. Dazu müssen Sie [die neueste Version von Azure IoT-Explorer für Ihr Betriebssystem herunterladen und installieren](./howto-use-iot-explorer.md).

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Führen Sie den folgenden Befehl aus, um die _IoT-Hub-Verbindungszeichenfolge_ für Ihren Hub abzurufen. Notieren Sie sich diese Verbindungszeichenfolge zur späteren Verwendung in dieser Schnellstartanleitung:

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

> [!TIP]
> Die IoT-Hub-Verbindungszeichenfolge kann auch per Azure IoT-Explorer ermittelt werden.

Führen Sie den folgenden Befehl aus, um die _Geräteverbindungszeichenfolge_ für das dem Hub hinzugefügte Gerät abzurufen. Notieren Sie sich diese Verbindungszeichenfolge zur späteren Verwendung in dieser Schnellstartanleitung:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```

[!INCLUDE [iot-pnp-download-models.md](../../includes/iot-pnp-download-models.md)]

## <a name="download-the-code"></a>Laden Sie den Code herunter.

In diesem Tutorial wird eine Entwicklungsumgebung vorbereitet, die Sie zum Klonen und Erstellen des Azure IoT Hub-Geräte-SDKs für Java verwenden können.

Öffnen Sie eine Eingabeaufforderung in einem Verzeichnis Ihrer Wahl. Führen Sie den folgenden Befehl aus, um das GitHub-Repository für [Azure IoT-Java-SDKs und die zugehörigen Bibliotheken](https://github.com/Azure/azure-iot-sdk-java) an diesem Speicherort zu klonen:

```cmd
git clone https://github.com/Azure/azure-iot-sdk-java.git
```

Dieser Vorgang kann mehrere Minuten dauern.

## <a name="build-the-code"></a>Erstellen des Codes

Navigieren Sie unter Windows zum Stammordner des geklonten Java SDK-Repositorys. Navigieren Sie dann zum Ordner *\device\iot-device-samples\pnp-device-sample\temerature-controller-device-sample*.

Führen Sie den folgenden Befehl aus, um die Beispielanwendung zu erstellen:

```java
mvn clean package
```

## <a name="run-the-device-sample"></a>Ausführen des Gerätebeispiels

Erstellen Sie eine Umgebungsvariable namens **IOTHUB_DEVICE_CONNECTION_STRING**, um die zuvor notierte Geräteverbindungszeichenfolge zu speichern.

Führen Sie den folgenden Befehl aus, um die Beispielanwendung auszuführen:

```java
mvn exec:java -Dexec.mainClass="samples.com.microsoft.azure.sdk.iot.device.TemperatureController"
```

Das Gerät ist jetzt zum Empfangen von Befehlen und Eigenschaftsaktualisierungen bereit und hat damit begonnen, Telemetriedaten an den Hub zu senden. Behalten Sie die Ausführung des Beispiels während der nächsten Schritte bei.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Überprüfen des Codes mithilfe von Azure IoT-Explorer

Überprüfen Sie nach dem Start des Geräteclientbeispiels mithilfe von Azure IoT-Explorer, ob das Beispiel funktioniert.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

## <a name="review-the-code"></a>Überprüfen des Codes

In diesem Beispiel wird ein IoT Plug & Play-Temperaturregler implementiert. Das in diesem Beispiel implementierte Modell umfasst [mehrere Komponenten](concepts-components.md). In der [DTDL-Modelldatei (Digital Twins Definition Language) für das Temperaturgerät](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) werden die vom Gerät implementierten Telemetriedaten, Eigenschaften und Befehle definiert.

Vom Gerätecode wird die Standardklasse `DeviceClient` verwendet, um eine Verbindung mit Ihrem IoT-Hub herzustellen. Das Gerät sendet die Modell-ID des in der Verbindungsanforderung implementierten DTDL-Modells. Ein Gerät, das eine Modell-ID sendet, ist ein IoT Plug & Play-Gerät:

```java
private static void initializeDeviceClient() throws URISyntaxException, IOException {
    ClientOptions options = new ClientOptions();
    options.setModelId(MODEL_ID);
    deviceClient = new DeviceClient(deviceConnectionString, protocol, options);

    deviceClient.registerConnectionStatusChangeCallback((status, statusChangeReason, throwable, callbackContext) -> {
        log.debug("Connection status change registered: status={}, reason={}", status, statusChangeReason);

        if (throwable != null) {
            log.debug("The connection status change was caused by the following Throwable: {}", throwable.getMessage());
            throwable.printStackTrace();
        }
    }, deviceClient);

    deviceClient.open();
}
```

Die Modell-ID wird im Code gespeichert, wie im folgenden Codeausschnitt zu sehen:

```java
private static final String MODEL_ID = "dtmi:com:example:Thermostat;1";
```

Nachdem das Gerät eine Verbindung mit Ihrem IoT-Hub hergestellt hat, registriert der Code die Befehlshandler.

```java
deviceClient.subscribeToDeviceMethod(new MethodCallback(), null, new MethodIotHubEventCallback(), null);
```

Es gibt separate Handler für die Aktualisierung gewünschter Eigenschaften der beiden Thermostatkomponenten:

```java
deviceClient.startDeviceTwin(new TwinIotHubEventCallback(), null, new GenericPropertyUpdateCallback(), null);
Map<Property, Pair<TwinPropertyCallBack, Object>> desiredPropertyUpdateCallback = Stream.of(
        new AbstractMap.SimpleEntry<Property, Pair<TwinPropertyCallBack, Object>>(
                new Property(THERMOSTAT_1, null),
                new Pair<>(new TargetTemperatureUpdateCallback(), THERMOSTAT_1)),
        new AbstractMap.SimpleEntry<Property, Pair<TwinPropertyCallBack, Object>>(
                new Property(THERMOSTAT_2, null),
                new Pair<>(new TargetTemperatureUpdateCallback(), THERMOSTAT_2))
).collect(Collectors.toMap(AbstractMap.SimpleEntry::getKey, AbstractMap.SimpleEntry::getValue));

deviceClient.subscribeToTwinDesiredProperties(desiredPropertyUpdateCallback);
```

Durch den Beispielcode werden Telemetriedaten von beiden Thermostatkomponenten gesendet:

```java
sendTemperatureReading(THERMOSTAT_1);
sendTemperatureReading(THERMOSTAT_2);
```

Von der Methode `sendTemperatureReading` wird die Klasse `PnpHhelper` verwendet, um Nachrichten für die jeweilige Komponente zu erstellen:

```java
Message message = PnpHelper.createIotHubMessageUtf8(telemetryName, currentTemperature, componentName);
```

Die Klasse `PnpHelper` enthält weitere Beispielmethoden, die Sie mit einem Modell mit mehreren Komponenten verwenden können.

Verwenden Sie Azure IoT-Explorer, um die Telemetriedaten und Eigenschaften der beiden Thermostatkomponenten anzuzeigen:

:::image type="content" source="media/tutorial-multiple-components-java/multiple-component.png" alt-text="Gerät mit mehreren Komponenten in Azure IoT-Explorer":::

Mit Azure IoT-Explorer können Sie auch Befehle in den beiden Thermostatkomponenten oder in der Stammschnittstelle aufrufen.

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie für ein IoT Plug & Play-Gerät mit Komponenten eine Verbindung mit einem IoT-Hub herstellen. Weitere Informationen zu IoT Plug & Play-Gerätemodellen finden Sie hier:

> [!div class="nextstepaction"]
> [Anleitung für Entwickler: Modellierung mit IoT Plug & Play (Vorschau)](concepts-developer-guide.md)
