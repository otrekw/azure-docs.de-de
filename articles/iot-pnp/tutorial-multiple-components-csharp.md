---
title: Verbinden von exemplarischem IoT Plug & Play Preview-Komponentengerätecode (C#) mit IoT Hub | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie IoT Plug & Play Preview-Beispielgerätecode (C#) erstellen und ausführen, von dem mehrere Komponenten verwendet werden und eine Verbindung mit einem IoT-Hub hergestellt wird. Verwenden Sie das Tool Azure IoT-Explorer, um die vom Gerät an den Hub gesendeten Informationen anzuzeigen.
author: ericmitt
ms.author: ericmitt
ms.date: 07/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 67b71399332fb29a277381a8c2806dbe7fb31d85
ms.sourcegitcommit: 1b2d1755b2bf85f97b27e8fbec2ffc2fcd345120
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/04/2020
ms.locfileid: "87552117"
---
# <a name="tutorial-connect-an-iot-plug-and-play-multiple-component-device-application-running-on-windows-to-iot-hub-c"></a>Tutorial: Herstellen einer Verbindung zwischen einer unter Windows ausgeführten IoT Plug & Play-Geräteanwendung mit mehreren Komponenten und IoT Hub (C#)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

In diesem Tutorial erfahren Sie, wie Sie eine exemplarische IoT Plug & Play-Geräteanwendung mit Komponenten und Stammschnittstelle erstellen, mit Ihrem IoT-Hub verbinden und mithilfe von Azure IoT-Explorer die an den Hub gesendeten Informationen anzeigen. Die Beispielanwendung wurde in C# geschrieben und ist im Azure IoT-Geräte-SDK für C# enthalten. Ein Lösungsentwickler kann mithilfe von Azure IoT-Explorer mehr über die Funktionen eines IoT Plug & Play-Geräts erfahren, ohne Gerätecode anzeigen zu müssen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

Installieren Sie die folgende Software in Ihrer lokalen Windows-Umgebung, um dieses Tutorial unter Windows ausführen zu können:

* [Visual Studio (Community, Professional oder Enterprise)](https://visualstudio.microsoft.com/downloads/).
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).

### <a name="azure-iot-explorer"></a>Azure IoT-Explorer

Wenn Sie im zweiten Teil dieses Tutorials mit dem Beispielgerät interagieren möchten, verwenden Sie das Tool **Azure IoT-Explorer**. Dazu müssen Sie [die neueste Version von Azure IoT-Explorer für Ihr Betriebssystem herunterladen und installieren](./howto-use-iot-explorer.md).

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Führen Sie den folgenden Befehl aus, um die _IoT-Hub-Verbindungszeichenfolge_ für Ihren Hub abzurufen. Notieren Sie sich diese Verbindungszeichenfolge zur späteren Verwendung in diesem Tutorial:

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

> [!TIP]
> Die IoT-Hub-Verbindungszeichenfolge kann auch per Azure IoT-Explorer ermittelt werden.

Führen Sie den folgenden Befehl aus, um die _Geräteverbindungszeichenfolge_ für das dem Hub hinzugefügte Gerät abzurufen. Notieren Sie sich diese Verbindungszeichenfolge zur späteren Verwendung in diesem Tutorial:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```

[!INCLUDE [iot-pnp-download-models.md](../../includes/iot-pnp-download-models.md)]

## <a name="download-the-code"></a>Laden Sie den Code herunter.

In diesem Tutorial wird eine Entwicklungsumgebung vorbereitet, die Sie zum Klonen und Erstellen des Azure IoT Hub-Geräte-SDKs für C# verwenden können.

Öffnen Sie eine Eingabeaufforderung in einem Verzeichnis Ihrer Wahl. Führen Sie den folgenden Befehl aus, um das GitHub-Repository für [Azure IoT-C#-SDKs und die zugehörigen Bibliotheken](https://github.com/Azure/azure-iot-sdk-csharp) an diesem Speicherort zu klonen:

```cmd
git clone https://github.com/Azure/azure-iot-sdk-csharp.git
```

## <a name="build-the-code"></a>Erstellen des Codes

Öffnen Sie die Projektmappendatei **azureiot.sln** in Visual Studio 2019, und legen Sie das Projekt **TemperatureController** als Startprojekt fest. Im **Projektmappen-Explorer** finden Sie diese Projektdatei unter **iothub > device > samples**.

Nun können Sie das Beispiel in Visual Studio erstellen und im Debugmodus ausführen.

## <a name="run-the-device-sample"></a>Ausführen des Gerätebeispiels

Erstellen Sie eine Umgebungsvariable namens **IOTHUB_DEVICE_CONNECTION_STRING**, um die zuvor notierte Geräteverbindungszeichenfolge zu speichern.

Wenn Sie die Codeausführung in Visual Studio unter Windows nachverfolgen möchten, fügen Sie der Funktion `main` in der Datei „program.cs“ einen Breakpoint hinzu.

Das Gerät ist jetzt zum Empfangen von Befehlen und Eigenschaftsaktualisierungen bereit und hat damit begonnen, Telemetriedaten an den Hub zu senden. Behalten Sie die Ausführung des Beispiels während der nächsten Schritte bei.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Überprüfen des Codes mithilfe von Azure IoT-Explorer

Überprüfen Sie nach dem Start des Geräteclientbeispiels mithilfe von Azure IoT-Explorer, ob das Beispiel funktioniert.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

## <a name="review-the-code"></a>Überprüfen des Codes

In diesem Beispiel wird ein IoT Plug & Play-Temperaturregler implementiert. Das in diesem Beispiel implementierte Modell umfasst [mehrere Komponenten](concepts-components.md). In der [DTDL-Modelldatei (Digital Twins Definition Language) für das Temperaturgerät](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) werden die vom Gerät implementierten Telemetriedaten, Eigenschaften und Befehle definiert.

Der Gerätecode stellt mithilfe der Standardmethode `CreateFromConnectionString` eine Verbindung mit Ihrem IoT-Hub her. Das Gerät sendet die Modell-ID des in der Verbindungsanforderung implementierten DTDL-Modells. Ein Gerät, das eine Modell-ID sendet, ist ein IoT Plug & Play-Gerät:

```csharp
private static void InitializeDeviceClientAsync()
{
  var options = new ClientOptions
  {
      ModelId = ModelId,
  };
  s_deviceClient = DeviceClient.CreateFromConnectionString(s_deviceConnectionString, TransportType.Mqtt, options);
  s_deviceClient.SetConnectionStatusChangesHandler((status, reason) =>
  {
      s_logger.LogDebug($"Connection status change registered - status={status}, reason={reason}.");
  });
}
```

Die Modell-ID wird im Code gespeichert, wie im folgenden Codeausschnitt zu sehen:

```csharp
private const string ModelId = "dtmi:com:example:Thermostat;1";
```

Nachdem das Gerät eine Verbindung mit Ihrem IoT-Hub hergestellt hat, registriert der Code die Befehlshandler. Der Befehl `reboot` wird in der Stammschnittstelle definiert. Der Befehl `getMaxMinReport` wird in den beiden Thermostatkomponenten definiert:

```csharp
await s_deviceClient.SetMethodHandlerAsync("reboot", HandleRebootCommandAsync, s_deviceClient);
await s_deviceClient.SetMethodHandlerAsync("thermostat1*getMaxMinReport", HandleMaxMinReportCommandAsync, Thermostat1);
await s_deviceClient.SetMethodHandlerAsync("thermostat2*getMaxMinReport", HandleMaxMinReportCommandAsync, Thermostat2);
```

Es gibt separate Handler für die Aktualisierung gewünschter Eigenschaften der beiden Thermostatkomponenten:

```csharp
s_desiredPropertyUpdateCallbacks.Add(Thermostat1, TargetTemperatureUpdateCallbackAsync);
s_desiredPropertyUpdateCallbacks.Add(Thermostat2, TargetTemperatureUpdateCallbackAsync);
```

Durch den Beispielcode werden Telemetriedaten von beiden Thermostatkomponenten gesendet:

```csharp
await SendTemperatureAsync(Thermostat1);
await SendTemperatureAsync(Thermostat2);
```

Von der Methode `SendTemperature` wird die Klasse `PnpHhelper` verwendet, um Nachrichten für die jeweilige Komponente zu erstellen:

```csharp
Message msg = PnpHelper.CreateIothubMessageUtf8(telemetryName, JsonConvert.SerializeObject(currentTemperature), componentName);
```

Die Klasse `PnpHelper` enthält weitere Beispielmethoden, die Sie mit einem Modell mit mehreren Komponenten verwenden können.

Verwenden Sie Azure IoT-Explorer, um die Telemetriedaten und Eigenschaften der beiden Thermostatkomponenten anzuzeigen:

:::image type="content" source="media/tutorial-multiple-components-csharp/multiple-component.png" alt-text="Gerät mit mehreren Komponenten in Azure IoT-Explorer":::

Mit Azure IoT-Explorer können Sie auch Befehle in den beiden Thermostatkomponenten oder in der Stammschnittstelle aufrufen.

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie für ein IoT Plug & Play-Gerät mit Komponenten eine Verbindung mit einem IoT-Hub herstellen. Weitere Informationen zu IoT Plug & Play-Gerätemodellen finden Sie hier:

> [!div class="nextstepaction"]
> [Anleitung für Entwickler: Modellierung mit IoT Plug & Play (Vorschau)](concepts-developer-guide.md)
