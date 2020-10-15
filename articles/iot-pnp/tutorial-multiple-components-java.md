---
title: Verbinden von IoT Plug & Play-Java-Beispielcode für Gerätekomponenten mit IoT Hub | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie IoT Plug & Play-Beispielgerätecode (Java) erstellen und ausführen, bei dem mehrere Komponenten verwendet werden und eine Verbindung mit einem IoT-Hub hergestellt wird. Verwenden Sie das Tool Azure IoT-Explorer, um die vom Gerät an den Hub gesendeten Informationen anzuzeigen.
author: ericmitt
ms.author: ericmitt
ms.date: 07/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: fce6477313cb7fb917c0fb81d03a73eec4714915
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92046399"
---
# <a name="tutorial-connect-a-sample-iot-plug-and-play-multiple-component-device-application-to-iot-hub-java"></a>Tutorial: Herstellen einer Verbindung zwischen einer exemplarischen IoT Plug & Play-Geräteanwendung mit mehreren Komponenten und IoT Hub (Java)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

In diesem Tutorial erfahren Sie, wie Sie eine exemplarische IoT Plug & Play-Geräteanwendung mit mehreren Komponenten erstellen, mit Ihrem IoT-Hub verbinden und die gesendeten Telemetriedaten mithilfe der Azure CLI anzeigen. Die Beispielanwendung wurde in Java geschrieben und ist im Azure IoT-Geräte-SDK für Java enthalten. Ein Lösungsentwickler kann mithilfe der Azure CLI mehr über die Funktionen eines IoT Plug & Play-Geräts erfahren, ohne Gerätecode anzeigen zu müssen.

In diesem Tutorial erfahren Sie, wie Sie eine IoT Plug & Play-Beispielgeräteanwendung mit Komponenten erstellen, mit Ihrem IoT-Hub verbinden und die an den Hub gesendeten Informationen mithilfe des Tools Azure IoT-Explorer anzeigen. Die Beispielanwendung wurde in Java geschrieben und ist im Azure IoT-Geräte-SDK für Java enthalten. Ein Lösungsentwickler kann mithilfe von Azure IoT-Explorer mehr über die Funktionen eines IoT Plug & Play-Geräts erfahren, ohne Gerätecode anzeigen zu müssen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Installieren Sie die folgende Software in Ihrer lokalen Windows-Umgebung, um dieses Tutorial unter Windows ausführen zu können:

* Java SE Development Kit 8. Wählen Sie unter [Langfristiger Java-Support für Azure und Azure Stack](/java/azure/jdk/?preserve-view=true&view=azure-java-stable) unter **Langfristiger Support** die Option **Java 8** aus.
* [Apache Maven 3](https://maven.apache.org/download.cgi)

## <a name="download-the-code"></a>Laden Sie den Code herunter.

Wenn Sie [Schnellstart: Verbinden einer unter Windows ausgeführten IoT Plug & Play-Beispielgeräteanwendung mit IoT Hub (Java)](quickstart-connect-device-java.md) abgeschlossen haben, wurde das Repository bereits geklont.

Öffnen Sie eine Eingabeaufforderung in einem Verzeichnis Ihrer Wahl. Führen Sie den folgenden Befehl aus, um das GitHub-Repository für [Azure IoT-Java-SDKs und die zugehörigen Bibliotheken](https://github.com/Azure/azure-iot-sdk-java) an diesem Speicherort zu klonen:

```cmd
git clone https://github.com/Azure/azure-iot-sdk-java.git
```

Dieser Vorgang kann mehrere Minuten dauern.

## <a name="build-the-code"></a>Erstellen des Codes

Navigieren Sie unter Windows zum Stammordner des geklonten Java SDK-Repositorys. Führen Sie den folgenden Befehl zum Erstellen der Abhängigkeiten aus:

```cmd/sh
mvn install -T 2C -DskipTests
```

## <a name="run-the-device-sample"></a>Ausführen des Gerätebeispiels

[!INCLUDE [iot-pnp-environment](../../includes/iot-pnp-environment.md)]

Navigieren Sie zum Ausführen der Beispielanwendung zum Ordner *\device\iot-device-samples\pnp-device-sample\temperature-controller-device-sample*, und führen Sie den folgenden Befehl aus:

```cmd/sh
mvn exec:java -Dexec.mainClass="samples.com.microsoft.azure.sdk.iot.device.TemperatureController"
```

Das Gerät ist jetzt zum Empfangen von Befehlen und Eigenschaftsaktualisierungen bereit und hat damit begonnen, Telemetriedaten an den Hub zu senden. Behalten Sie die Ausführung des Beispiels während der nächsten Schritte bei.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Überprüfen des Codes mithilfe von Azure IoT-Explorer

Überprüfen Sie nach dem Start des Geräteclientbeispiels mithilfe von Azure IoT-Explorer, ob das Beispiel funktioniert.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

## <a name="review-the-code"></a>Überprüfen des Codes

In diesem Beispiel wird ein IoT Plug & Play-Temperaturregler implementiert. Das in diesem Beispiel implementierte Modell umfasst [mehrere Komponenten](concepts-components.md). In der [DTDL-Modelldatei (Digital Twins Definition Language) für das Temperaturgerät](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) werden die vom Gerät implementierten Telemetriedaten, Eigenschaften und Befehle definiert.

Vom Gerätecode wird die Standardklasse `DeviceClient` verwendet, um eine Verbindung mit Ihrem IoT-Hub herzustellen. Das Gerät sendet die Modell-ID des in der Verbindungsanforderung implementierten DTDL-Modells. Ein Gerät, das eine Modell-ID sendet, ist ein IoT Plug & Play-Gerät:

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

Mit dem Tool Azure IoT-Explorer können Sie auch Befehle in einer der beiden Thermostatkomponenten oder in der Standardkomponente aufrufen.

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie für ein IoT Plug & Play-Gerät mit Komponenten eine Verbindung mit einem IoT-Hub herstellen. Weitere Informationen zu IoT Plug & Play-Gerätemodellen finden Sie hier:

> [!div class="nextstepaction"]
> [Anleitung für Entwickler: Modellierung mit IoT Plug & Play](concepts-developer-guide-device-csharp.md)