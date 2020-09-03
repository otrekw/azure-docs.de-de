---
title: Anleitung für Entwickler – IoT Plug & Play (Vorschau) | Microsoft-Dokumentation
description: Beschreibung von IoT Plug & Play für Entwickler
author: rido-min
ms.author: rmpablos
ms.date: 07/16/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: f1860f9645a50789803d509a6a1ea98bc0ea1a9e
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88950135"
---
# <a name="iot-plug-and-play-preview-developer-guide"></a>Anleitung für Entwickler: IoT Plug & Play (Vorschau)

Mit IoT Plug & Play (Vorschau) können Sie intelligente Geräte erstellen, deren Funktionen für Azure IoT-Anwendungen verfügbar gemacht werden. Bei IoT Plug & Play-Geräten ist keine manuelle Konfiguration erforderlich, wenn ein Kunde sie mit IoT Plug & Play-fähigen Anwendungen verbindet.

In dieser Anleitung werden die grundlegenden Schritte beschrieben, die zum Erstellen eines Geräts erforderlich sind, das den [IoT Plug & Play-Konventionen](concepts-convention.md) entspricht. Außerdem werden die verfügbaren REST-APIs erläutert, mit denen Sie mit dem Gerät interagieren können.

Führen Sie die folgenden Schritte aus, um ein IoT Plug & Play-Gerät zu erstellen:

1. Stellen Sie sicher, dass Ihr Gerät entweder das MQTT- oder das MQTT über WebSockets-Protokoll verwendet, um eine Verbindung mit Azure IoT Hub herzustellen.
1. Erstellen Sie ein [Digital Twins Definition Language (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl)-Modell zum Beschreiben Ihres Geräts. Weitere Informationen finden Sie unter [Verstehen der Komponenten in IoT Plug & Play-Modellen](concepts-components.md).
1. Aktualisieren Sie Ihr Gerät, um `model-id` als Teil der Geräteverbindung anzukündigen.
1. Implementieren Sie Telemetrie, Eigenschaften und Befehle anhand der [IoT Plug & Play-Konventionen](concepts-convention.md).

Sobald die Geräteimplementierung abgeschlossen ist, verwenden Sie den [Azure IoT-Explorer](howto-use-iot-explorer.md), um zu überprüfen, ob das Gerät den IoT Plug & Play-Konventionen entspricht.

> [!Tip]
> In allen Codefragmenten in diesem Artikel wird C# verwendet, doch gelten die Konzepte auch für alle verfügbaren SDKs für C, Python, Node und Java.

## <a name="model-id-announcement"></a>Modell-ID-Ankündigung

Zum Ankündigen der Modell-ID muss das Gerät diese in den Verbindungsinformationen enthalten:

```csharp
DeviceClient.CreateFromConnectionString(
  connectionString,
  TransportType.Mqtt,
  new ClientOptions() { ModelId = modelId })
```

Die neue `ClientOptions`-Überladung ist in allen `DeviceClient`-Methoden verfügbar, die zum Initialisieren einer Verbindung verwendet werden.

Die Modell-ID-Ankündigung wurde den nächsten Versionen der SDKs hinzugefügt.

|SDK|Version|
|---|-------|
|C-SDK|1.3.9|
|.NET|1.27.0|
|Java|1.14.0|
|Node|1.17.0|
|Python|2.1.4|

## <a name="dps-payload"></a>DPS-Nutzlast

Geräte, die [Device Provisioning Service (DPS)](../iot-dps/about-iot-dps.md) verwenden, können das `modelId`-Element enthalten, das während des Bereitstellungsprozesses mit der folgenden JSON-Nutzlast verwendet werden soll:

```json
{
    "modelId" : "dtmi:com:example:Thermostat;1"
}
```

## <a name="implement-telemetry-properties-and-commands"></a>Implementieren von Telemetrie, Eigenschaften und Befehlen

Wie unter [Verstehen der Komponenten in IoT Plug & Play-Modellen](concepts-components.md) beschrieben, müssen Geräteentwickler entscheiden, ob sie Komponenten zum Beschreiben ihrer Geräte verwenden möchten. Bei Verwendung von Komponenten müssen Geräte den in diesem Abschnitt beschriebenen Regeln entsprechen.

### <a name="telemetry"></a>Telemetrie

Für Modelle ohne Komponenten ist keine spezielle Eigenschaft erforderlich.

Bei Verwendung von Komponenten müssen Geräte eine Nachrichteneigenschaft mit dem Komponentennamen festlegen:

```c#
public async Task SendComponentTelemetryValueAsync(string componentName, string serializedTelemetry)
{
  var message = new Message(Encoding.UTF8.GetBytes(serializedTelemetry));
  message.Properties.Add("$.sub", componentName);
  message.ContentType = "application/json";
  message.ContentEncoding = "utf-8";
  await deviceClient.SendEventAsync(message);
}
```

### <a name="read-only-properties"></a>Schreibgeschützte Eigenschaften

Für Modelle ohne Komponenten ist kein spezielles Konstrukt erforderlich:

```csharp
TwinCollection reportedProperties = new TwinCollection();
reportedProperties["maxTemperature"] = 38.7;
await client.UpdateReportedPropertiesAsync(reportedProperties);
```

Der Gerätezwilling wird mit der nächsten gemeldeten Eigenschaft aktualisiert:

```json
{
  "reported": {
      "maxTemperature" : 38.7
  }
}
```

Bei Verwendung von Komponenten müssen Eigenschaften innerhalb des Komponentennamens erstellt werden:

```csharp
TwinCollection reportedProperties = new TwinCollection();
TwinCollection component = new TwinCollection();
component["maxTemperature"] = 38.7;
component["__t"] = "c"; // marker to identify a component
reportedProperties["thermostat1"] = component;
await client.UpdateReportedPropertiesAsync(reportedProperties);
```

Der Gerätezwilling wird mit der nächsten gemeldeten Eigenschaft aktualisiert:

```json
{
  "reported": {
    "thermostat1" : {  
      "__t" : "c",  
      "maxTemperature" : 38.7
     } 
  }
}
```

### <a name="writable-properties"></a>Schreibbare Eigenschaften

Diese Eigenschaften können vom Gerät festgelegt oder von der Lösung aktualisiert werden. Wenn die Lösung eine Eigenschaft aktualisiert, empfängt der Client eine Benachrichtigung als Rückruf in `DeviceClient`. Um den IoT Plug & Play-Konventionen zu entsprechen, muss das Gerät den Dienst informieren, dass die Eigenschaft erfolgreich empfangen wurde.

#### <a name="report-a-writable-property"></a>Melden einer schreibbaren Eigenschaft

Wenn ein Gerät eine schreibbare Eigenschaft meldet, müssen die `ack`-Werte enthalten sein, die in den Konventionen definiert sind.

So wird eine schreibbare Eigenschaft ohne Komponenten gemeldet:

```csharp
TwinCollection reportedProperties = new TwinCollection();
TwinCollection ackProps = new TwinCollection();
ackProps["value"] = 23.2;
ackProps["ac"] = 200; // using HTTP status codes
ackProps["av"] = 0; // not readed from a desired property
ackProps["ad"] = "reported default value";
reportedProperties["targetTemperature"] = ackProps;
await client.UpdateReportedPropertiesAsync(reportedProperties);
```

Der Gerätezwilling wird mit der nächsten gemeldeten Eigenschaft aktualisiert:

```json
{
  "reported": {
      "targetTemperature": {
          "value": 23.2,
          "ac": 200,
          "av": 3,
          "ad": "complete"
      }
  }
}
```

Zum Melden einer schreibbaren Eigenschaft aus einer Komponente muss der Zwilling einen Marker enthalten:

```csharp
TwinCollection reportedProperties = new TwinCollection();
TwinCollection component = new TwinCollection();
TwinCollection ackProps = new TwinCollection();
component["__t"] = "c"; // marker to identify a component
ackProps["value"] = 23.2;
ackProps["ac"] = 200; // using HTTP status codes
ackProps["av"] = 0; // not read from a desired property
ackProps["ad"] = "reported default value";
component["targetTemperature"] = ackProps;
reportedProperties["thermostat1"] = component;
await client.UpdateReportedPropertiesAsync(reportedProperties);
```

Der Gerätezwilling wird mit der nächsten gemeldeten Eigenschaft aktualisiert:

```json
{
  "reported": {
    "thermostat1": {
      "__t" : "c",
      "targetTemperature": {
          "value": 23.2,
          "ac": 200,
          "av": 3,
          "ad": "complete"
      }
    }
  }
}
```

#### <a name="subscribe-to-desired-property-updates"></a>Abonnieren von Aktualisierungen der gewünschten Eigenschaften

Dienste können gewünschte Eigenschaften aktualisieren, die eine Benachrichtigung auf den verbundenen Geräten auslösen. Diese Benachrichtigung enthält die aktualisierten gewünschten Eigenschaften, einschließlich der Versionsnummer zum Identifizieren der Aktualisierung. Geräte müssen mit der gleichen `ack`-Nachricht wie gemeldete Eigenschaften antworten.

Modelle ohne Komponenten sehen die einzelne Eigenschaft und erstellen die gemeldete `ack` mit der empfangenen Version:

```csharp
await client.SetDesiredPropertyUpdateCallbackAsync(async (desired, ctx) => 
{
  JValue targetTempJson = desired["targetTemperature"];
  double targetTemperature = targetTempJson.Value<double>();

  TwinCollection reportedProperties = new TwinCollection();
  TwinCollection ackProps = new TwinCollection();
  ackProps["value"] = targetTemperature;
  ackProps["ac"] = 200;
  ackProps["av"] = desired.Version; 
  ackProps["ad"] = "desired property received";
  reportedProperties["targetTemperature"] = ackProps;

  await client.UpdateReportedPropertiesAsync(reportedProperties);
}, null);
```

Der Gerätezwilling zeigt die Eigenschaft in den Abschnitten für gewünschte und gemeldete Eigenschaften:

```json
{
  "desired" : {
    "targetTemperature": 23.2,
    "$version" : 3
  },
  "reported": {
      "targetTemperature": {
          "value": 23.2,
          "ac": 200,
          "av": 3,
          "ad": "complete"
      }
  }
}
```

Modelle mit Komponenten empfangen die gewünschten Eigenschaften eingeschlossenen im Komponentennamen und sollten die mit `ack` gemeldete Eigenschaft zurückmelden:

```csharp
await client.SetDesiredPropertyUpdateCallbackAsync(async (desired, ctx) =>
{
  JObject thermostatComponent = desired["thermostat1"];
  JToken targetTempProp = thermostatComponent["targetTemperature"];
  double targetTemperature = targetTempProp.Value<double>();

  TwinCollection reportedProperties = new TwinCollection();
  TwinCollection component = new TwinCollection();
  TwinCollection ackProps = new TwinCollection();
  component["__t"] = "c"; // marker to identify a component
  ackProps["value"] = targetTemperature;
  ackProps["ac"] = 200; // using HTTP status codes
  ackProps["av"] = desired.Version; // not readed from a desired property
  ackProps["ad"] = "desired property received";
  component["targetTemperature"] = ackProps;
  reportedProperties["thermostat1"] = component;

  await client.UpdateReportedPropertiesAsync(reportedProperties);
}, null);
```

Der Gerätezwilling für Komponenten zeigt die Abschnitte für gewünschte und gemeldete Eigenschaften wie folgt an:

```json
{
  "desired" : {
    "thermostat1" : {
        "__t" : "c",
        "targetTemperature": 23.2,
    }
    "$version" : 3
  },
  "reported": {
    "thermostat1" : {
        "__t" : "c",
      "targetTemperature": {
          "value": 23.2,
          "ac": 200,
          "av": 3,
          "ad": "complete"
      }
    }
  }
}
```

### <a name="commands"></a>Befehle

Modelle ohne Komponenten empfangen den Befehlsnamen, wie er vom Dienst aufgerufen wurde.

Modelle mit Komponenten empfangen den Befehlsnamen mit vorangestellter Komponente und dem Trennzeichen `*`.

```csharp
await client.SetMethodHandlerAsync("themostat*reboot", (MethodRequest req, object ctx) =>
{
  Console.WriteLine("REBOOT");
  return Task.FromResult(new MethodResponse(200));
},
null);
```

#### <a name="request-and-response-payloads"></a>Anforderungs- und Antwortnutzlasten

Befehle verwenden Typen zum Definieren ihrer Anforderungs- und Antwortnutzlasten. Ein Gerät muss den eingehenden Eingabeparameter deserialisieren und die Antwort serialisieren. Im folgenden Beispiel wird gezeigt, wie ein Befehl mit komplexen Typen implementiert wird, die in den Nutzlasten definiert sind:

```json
{
  "@type": "Command",
  "name": "start",
  "request": {
    "name": "startRequest",
    "schema": {
      "@type": "Object",
      "fields": [
        {
          "name": "startPriority",
          "schema": "integer"
        },
        {
          "name": "startMessage",
          "schema" : "string"
        }
      ]
    }
  },
  "response": {
    "name": "startReponse",
    "schema": {
      "@type": "Object",
      "fields": [
        {
            "name": "startupTime",
            "schema": "integer" 
        },
        {
          "name": "startupMessage",
          "schema": "string"
        }
      ]
    }
  }
}
```

Die folgenden Codeausschnitte zeigen, wie ein Gerät diese Befehlsdefinition implementiert, einschließlich der Typen, die zum Aktivieren der Serialisierung und Deserialisierung verwendet werden:

```csharp
class startRequest
{
  public int startPriority { get; set; }
  public string startMessage { get; set; }
}

class startResponse
{
  public int startupTime { get; set; }
  public string startupMessage { get; set; }
}

// ... 

await client.SetMethodHandlerAsync("start", (MethodRequest req, object ctx) =>
{
  var startRequest = JsonConvert.DeserializeObject<startRequest>(req.DataAsJson);
  Console.WriteLine($"Received start command with priority ${startRequest.startPriority} and ${startRequest.startMessage}");

  var startResponse = new startResponse
  {
    startupTime = 123,
    startupMessage = "device started with message " + startRequest.startMessage
  };

  string responsePayload = JsonConvert.SerializeObject(startResponse);
  MethodResponse response = new MethodResponse(Encoding.UTF8.GetBytes(responsePayload), 200);
  return Task.FromResult(response);
},null);
```

> [!Tip]
> Die Anforderungs- und Antwortnamen sind in den über die Verbindung übertragenen serialisierten Nutzlasten nicht vorhanden.

## <a name="interact-with-the-device"></a>Interagieren mit dem Gerät 

Mithilfe von IoT Plug & Play können Sie Geräte verwenden, deren Model-ID bei Ihrem IoT-Hub angekündigt wurde. Beispielsweise können Sie direkt auf die Eigenschaften und Befehle eines Geräts zugreifen.

Zur Verwendung eines mit dem IoT-Hub verbundenen IoT Plug & Play-Geräts können Sie entweder die IoT Hub-REST-API oder eines der IoT-Sprach-SDKs nutzen. In den folgenden Beispielen wird die IoT Hub-REST-API verwendet. Die aktuelle Version der API lautet `2020-05-31-preview`. Fügen Sie `?api-version=2020-05-31` an die REST-API-Aufrufe an.

Wenn das Thermostatgerät den Namen `t-123` trägt, können Sie die Eigenschaften über alle mit dem Gerät implementierten Schnittstellen mit einem GET-Aufruf der REST-API abrufen:

```REST
GET /digitalTwins/t-123
```

Dieser Aufruf enthält die JSON-Eigenschaft `$metadata.$model` mit der vom Gerät angekündigten Modell-ID.

Über die REST-API-Vorlage `GET /DigitalTwin/{device-id}` wird auf alle Eigenschaften auf allen Schnittstellen zugegriffen, wobei `{device-id}` der Bezeichner für das Gerät ist:

```REST
GET /digitalTwins/{device-id}
```

Sie können Befehle des IoT Plug & Play-Geräts direkt aufrufen. Wenn die `Thermostat`-Komponente im Gerät `t-123` den Befehl `restart` enthält, können Sie diesen mit einem POST-Aufruf der REST-API aufrufen:

```REST
POST /digitalTwins/t-123/components/Thermostat/commands/restart
```

Generell können Befehle über folgende REST-API-Vorlage aufgerufen werden:

- `device-id`: der Bezeichner für das Gerät
- `component-name`: der Name der Schnittstelle aus dem Abschnitt „implements“ im Gerätefunktionsmodell
- `command-name`: der Name des Befehls

```REST
/digitalTwins/{device-id}/components/{component-name}/commands/{command-name}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit der Gerätemodellierung vertraut gemacht haben, finden Sie weitere Informationen in folgenden Artikeln:

- [Digital Twins Definition Language (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl)
- [C-Geräte-SDK](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
- [IoT-REST-API](https://docs.microsoft.com/rest/api/iothub/device)
- [Modellkomponenten](./concepts-components.md)
