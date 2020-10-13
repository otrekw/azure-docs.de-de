---
title: Interagieren mit einem IoT Plug & Play-Gerät, das mit Ihrer Azure IoT-Lösung verbunden ist (C#) | Microsoft-Dokumentation
description: Mithilfe von C# können Sie eine Verbindung mit einem IoT Plug & Play-Gerät herstellen, das mit ihrer Azure IoT-Lösung verbunden ist, und mit ihm interagieren.
author: ericmitt
ms.author: ericmitt
ms.date: 09/21/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: b35268cd8d36901f750225713407c5392e5c429e
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91759168"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-device-thats-connected-to-your-solution-c"></a>Schnellstart: Interagieren mit einem IoT Plug & Play-Gerät, das mit Ihrer Lösung verbunden ist (C#)

[!INCLUDE [iot-pnp-quickstarts-service-selector.md](../../includes/iot-pnp-quickstarts-service-selector.md)]

IoT Plug & Play vereinfacht IoT, indem es Ihnen die Interaktion mit den Funktionen eines Geräts – ohne Kenntnisse der zugrunde liegenden Geräteimplementierung – ermöglicht. In dieser Schnellstartanleitung erfahren Sie, wie Sie mit C# eine Verbindung mit einem IoT Plug & Play-Gerät herstellen, das mit Ihrer Lösung verbunden ist, und wie Sie dieses Gerät steuern.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Zur Ausführung dieser Schnellstartanleitung unter Windows müssen Sie auf Ihrem Entwicklungscomputer die folgende Software installiert haben:

* [Visual Studio (Community, Professional oder Enterprise)](https://visualstudio.microsoft.com/downloads/).
* [Git](https://git-scm.com/download/).

### <a name="clone-the-sdk-repository-with-the-sample-code"></a>Klonen des SDK-Repositorys mit dem Beispielcode

Wenn Sie [Schnellstart: Verbinden einer unter Windows ausgeführten IoT Plug & Play-Beispielgeräteanwendung mit IoT Hub (C#)](quickstart-connect-device-csharp.md) abgeschlossen haben, wurde das Repository bereits geklont.

Klonen Sie die Beispiele aus dem GitHub-Repository für Microsoft Azure IoT SDK für .NET. Öffnen Sie eine Eingabeaufforderung in einem Ordner Ihrer Wahl. Führen Sie den folgenden Befehl zum Klonen des GitHub-Repositorys [Microsoft Azure IoT Samples für .NET](https://github.com/Azure-Samples/azure-iot-samples-csharp) aus:

```cmd
git clone https://github.com/Azure-Samples/azure-iot-samples-csharp.git
```

## <a name="run-the-sample-device"></a>Ausführen des Beispielgeräts

In dieser Schnellstartanleitung verwenden Sie ein in C# geschriebenes Beispielthermostat als IoT Plug & Play-Gerät. So führen Sie das Beispielgerät aus:

1. Öffnen Sie in Visual Studio 2019 die Projektdatei *azure-iot-samples-csharp\iot-hub\Samples\device\PnpDeviceSamples\Thermostat\Thermostat.csproj*.

1. Navigieren Sie in Visual Studio zu **Projekt > Thermostateigenschaften > Debuggen**. Fügen Sie dem Projekt dann die folgenden Umgebungsvariablen hinzu:

    | Name | Wert |
    | ---- | ----- |
    | IOTHUB_DEVICE_SECURITY_TYPE | DPS |
    | IOTHUB_DEVICE_DPS_ENDPOINT | global.azure-devices-provisioning.net |
    | IOTHUB_DEVICE_DPS_ID_SCOPE | Der Wert, den Sie sich beim Abschließen von [Einrichten Ihrer Umgebung](set-up-environment.md) notiert haben |
    | IOTHUB_DEVICE_DPS_DEVICE_ID | my-pnp-device |
    | IOTHUB_DEVICE_DPS_DEVICE_KEY | Der Wert, den Sie sich beim Abschließen von [Einrichten Ihrer Umgebung](set-up-environment.md) notiert haben |


1. Jetzt können Sie das Beispiel in Visual Studio erstellen und im Debugmodus ausführen.

1. Es werden Nachrichten darüber angezeigt, dass das Gerät Informationen gesendet und sich selbst als „online“ gemeldet hat. Diese Nachrichten weisen darauf hin, dass das Gerät damit begonnen hat, Telemetriedaten an den Hub zu senden und jetzt bereit zum Empfangen von Befehlen und Eigenschaftsaktualisierungen ist. Schließen Sie diese Instanz von Visual Studio nicht. Sie benötigen sie zur Bestätigung, dass das Dienstbeispiel funktioniert.

## <a name="run-the-sample-solution"></a>Ausführen der Beispiellösung

In [Einrichten Ihrer Umgebung für die IoT Plug & Play-Schnellstarts und -Tutorials](set-up-environment.md) haben Sie zwei Umgebungsvariablen erstellt, um das Beispiel so zu konfigurieren, dass eine Verbindung mit Ihrem IoT-Hub und -Gerät hergestellt wird:

* **IOTHUB_CONNECTION_STRING**: die IoT-Hub-Verbindungszeichenfolge, die Sie sich zuvor notiert haben.
* **IOTHUB_DEVICE_ID**: `"my-pnp-device"`.

In dieser Schnellstartanleitung verwenden Sie eine IoT-Beispiellösung in C# für die Interaktion mit dem gerade eingerichteten Beispielgerät.

1. Öffnen Sie in einer anderen Instanz von Visual Studio das Projekt *azure-iot-samples-csharp\iot-hub\Samples\service\PnpServiceSamples\Thermostat\Thermostat.csproj*.

1. Navigieren Sie in Visual Studio zu **Projekt > Thermostateigenschaften > Debuggen**. Fügen Sie dem Projekt dann die folgenden Umgebungsvariablen hinzu:

    | Name | Wert |
    | ---- | ----- |
    | IOTHUB_DEVICE_ID | my-pnp-device |
    | IOTHUB_CONNECTION_STRING | Der Wert, den Sie sich beim Abschließen von [Einrichten Ihrer Umgebung](set-up-environment.md) notiert haben |

1. Jetzt können Sie das Beispiel in Visual Studio erstellen und im Debugmodus ausführen.

### <a name="get-digital-twin"></a>Abrufen des digitalen Zwillings

Der folgende Codeausschnitt zeigt, wie die-Dienstanwendung den digitalen Zwilling abruft:

```C#
// Get a Twin and retrieves model Id set by Device client
Twin twin = await s_registryManager.GetTwinAsync(s_deviceId);
s_logger.LogDebug($"Model Id of this Twin is: {twin.ModelId}");
```

> [!NOTE]
> In diesem Beispiel wird der Namespace **Microsoft.Azure.Devices.Client;** aus dem **IoT Hub-Dienstclient** verwendet. Weitere Informationen zum Abrufen der Modell-ID finden Sie im [Entwicklerhandbuch](concepts-developer-guide-service.md).

Dieser Code generiert die folgende Ausgabe:

```cmd
[09/21/2020 11:26:04]dbug: Thermostat.Program[0]
      Model Id of this Twin is: dtmi:com:example:Thermostat;1
```

Der folgende Codeausschnitt zeigt, wie Sie Eigenschaften über den digitalen Zwilling mithilfe eines *Patches* aktualisieren:

```C#
// Update the twin
var twinPatch = new Twin();
twinPatch.Properties.Desired[PropertyName] = PropertyValue;
await s_registryManager.UpdateTwinAsync(s_deviceId, twinPatch, twin.ETag);
```

Mit diesem Code wird die folgende Ausgabe des Geräts generiert, wenn der Dienst die Eigenschaft `targetTemperature` aktualisiert:

```cmd
[09/21/2020 11:26:05]dbug: Thermostat.ThermostatSample[0]
      Property: Received - { "targetTemperature": 60°C }.
[09/21/2020 11:26:05]dbug: Thermostat.ThermostatSample[0]
      Property: Update - {"targetTemperature": 60°C } is InProgress.

...

[09/21/2020 11:26:17]dbug: Thermostat.ThermostatSample[0]
      Property: Update - {"targetTemperature": 60°C } is Completed.
```

### <a name="invoke-a-command"></a>Aufrufen eines Befehls

Der folgende Codeausschnitt zeigt das Aufrufen eines Befehls:

```csharp
private static async Task InvokeCommandAsync()
{
    var commandInvocation = new CloudToDeviceMethod(CommandName) { ResponseTimeout = TimeSpan.FromSeconds(30) };

    // Set command payload
    string componentCommandPayload = JsonConvert.SerializeObject(s_dateTime);
    commandInvocation.SetPayloadJson(componentCommandPayload);

    CloudToDeviceMethodResult result = await s_serviceClient.InvokeDeviceMethodAsync(s_deviceId, commandInvocation);

    if (result == null)
    {
        throw new Exception($"Command {CommandName} invocation returned null");
    }

    s_logger.LogDebug($"Command {CommandName} invocation result status is: {result.Status}");
}
```

Mit diesem Code wird die folgende Ausgabe des Geräts generiert, wenn der Dienst den `getMaxMinReport` Befehl aufruft:

```cmd
[09/21/2020 11:26:05]dbug: Thermostat.ThermostatSample[0]
      Command: Received - Generating max, min and avg temperature report since 21/09/2020 11:25:58.
[09/21/2020 11:26:05]dbug: Thermostat.ThermostatSample[0]
      Command: MaxMinReport since 21/09/2020 11:25:58: maxTemp=32, minTemp=32, avgTemp=32, startTime=21/09/2020 11:25:59, endTime=21/09/2020 11:26:04
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einer IoT-Lösung verbinden. Weitere Informationen zu IoT Plug & Play-Gerätemodellen finden Sie unter:

> [!div class="nextstepaction"]
> [Anleitung für Entwickler: Modellierung mit IoT Plug & Play](concepts-developer-guide-device-csharp.md)
