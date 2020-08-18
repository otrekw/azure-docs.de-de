---
title: Erfassen von Telemetriedaten aus IoT Hub
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie Gerätetelemetrienachrichten aus IoT Hub erfassen.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 8/11/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 5209ffb0328e90fb2ca9b91773cbf18dd4ed2916
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88163611"
---
# <a name="ingest-iot-hub-telemetry-into-azure-digital-twins"></a>Erfassen von IoT Hub Telemetriedaten in Azure Digital Twins

Azure Digital Twins wird mit Daten von IoT-Geräten und anderen Quellen gesteuert. Eine gängige Quelle für Gerätedaten zur Verwendung in Azure Digital Twins ist [IoT Hub](../iot-hub/about-iot-hub.md).

Der Prozess zum Erfassen von Daten in Azure Digital Twins besteht im Einrichten einer externen Computeressource wie einer [Azure-Funktion](../azure-functions/functions-overview.md), die die Daten empfängt und mithilfe der [Digital Twins-APIs](how-to-use-apis-sdks.md) Eigenschaften entsprechend festlegt oder Telemetrieereignisse zu [digitalen Zwillingen](concepts-twins-graph.md) auslöst. 

In dieser Schrittanleitung wird erläutert, wie Sie eine Azure-Funktion schreiben, die Telemetrie von IoT Hub erfassen kann.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Beispiel fortfahren, müssen Sie die folgenden Voraussetzungen erfüllen.
* **Einen IoT-Hub**. Anweisungen finden Sie im Abschnitt *Erstellen eines IoT-Hubs* in [diesem IoT Hub-Schnellstart](../iot-hub/quickstart-send-telemetry-cli.md).
* **Eine Azure-Funktion** mit den richtigen Berechtigungen zum Aufrufen Ihrer Digital Twins-Instanz. Weitere Informationen finden Sie unter [*Vorgehensweise: Einrichten einer Azure-Funktion zum Verarbeiten von Daten*](how-to-create-azure-function.md). 
* **Eine Digital Twins-Instanz** zum Empfangen Ihrer Gerätetelemetrie. Weitere Informationen finden Sie unter [*Vorgehensweise: Einrichten einer Azure Digital Twins-Instanz und der Authentifizierung*](./how-to-set-up-instance-portal.md) 

### <a name="example-telemetry-scenario"></a>Beispieltelemetrieszenario

In dieser Schrittanleitung wird erläutert, wie Sie Nachrichten von IoT Hub mithilfe einer Azure-Funktion an Azure Digital Twins senden. Dazu gibt es viele mögliche Konfigurationen und Abgleichstrategien, das Beispiel für diesen Artikel enthält jedoch die folgenden Teile:
* Ein Thermometergerät in IoT Hub mit einer bekannten Geräte-ID.
* Ein digitaler Zwilling als Darstellung des Geräts mit einer übereinstimmenden ID

> [!NOTE]
> In diesem Beispiel wird eine einfache ID-Übereinstimmung zwischen der Geräte-ID und der ID eines zugehörigen digitalen Zwillings verwendet. Es können jedoch auch anspruchsvollere Zuordnungen vom Gerät zu dessen Zwilling bereitgestellt werden, z. B. eine Zuordnungstabelle.

Immer wenn ein Temperaturtelemetrieereignis vom Thermometergerät gesendet wird, sollte die Eigenschaft *temperature* des digitalen Zwillings aktualisiert werden. Dieses Szenario ist nachstehend in einem Diagramm dargestellt:

:::image type="content" source="media/how-to-ingest-iot-hub-data/events.png" alt-text="Abbildung eines Flussdiagramms. Im Diagramm sendet ein IoT Hub-Gerät Temperaturtelemetrie über IoT Hub an eine Azure-Funktion, die eine Temperatureigenschaft in einem Zwilling in Azure Digital Twins aktualisiert." border="false":::

## <a name="add-a-model-and-twin"></a>Hinzufügen eines Modells und eines Zwillings

Sie müssen einen Zwilling mit IoT Hub-Informationen aktualisieren.

Das Modell sieht so aus:
```JSON
{
  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",
  "@type": "Interface",
  "@context": "dtmi:dtdl:context;2",
  "contents": [
    {
      "@type": "Property",
      "name": "Temperature",
      "schema": "double"
    }
  ]
}
```

Zum **Hochladen dieses Modells zu Ihrer Digital Twins-Instanz** öffnen Sie die Azure CLI, und führen Sie den folgenden Befehl aus:
```azurecli-interactive
az dt model create --models '{  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",  "@type": "Interface",  "@context": "dtmi:dtdl:context;2",  "contents": [    {      "@type": "Property",      "name": "Temperature",      "schema": "double"    }  ]}' -n {digital_twins_instance_name}
```

Anschließend müssen Sie **einen einzigen Zwilling mit diesem Modell erstellen**. Verwenden Sie den folgenden Befehl zum Erstellen eines Zwillings und zum Festlegen von „0,0“ als anfänglichen Temperaturwert.
```azurecli-interactive
az dt twin create --dtmi "dtmi:contosocom:DigitalTwins:Thermostat;1" --twin-id thermostat67 --properties '{"Temperature": 0.0,}' --dt-name {digital_twins_instance_name}
```

Die Ausgabe eines erfolgreichen Befehls zum Erstellen des Zwillings sollte so aussehen:
```json
{
  "$dtId": "thermostat67",
  "$etag": "W/\"0000000-9735-4f41-98d5-90d68e673e15\"",
  "$metadata": {
    "$model": "dtmi:contosocom:DigitalTwins:Thermostat;1",
    "Temperature": {
      "ackCode": 200,
      "ackDescription": "Auto-Sync",
      "ackVersion": 1,
      "desiredValue": 0.0,
      "desiredVersion": 1
    }
  },
  "Temperature": 0.0
}
```

## <a name="create-an-azure-function"></a>Erstellen einer Azure-Funktion

In diesem Abschnitt werden die gleichen Startschritte in Visual Studio und das Azure-Funktionsgerüst verwendet wie in [*Vorgehensweise: von Azure Functions-Apps für die Verarbeitung von Daten*](how-to-create-azure-function.md). Das Gerüst übernimmt die Authentifizierung und erstellt einen Dienstclient, der zum Verarbeiten von Daten und zum Aufrufen von Azure Digital Twins-APIs als Antwort bereit ist. 

In den folgenden Schritten fügen Sie bestimmten Code hinzu, um IoT-Telemetrieereignisse von IoT Hub zu verarbeiten.  

### <a name="add-telemetry-processing"></a>Hinzufügen einer Telemetrieverarbeitung
    
Telemetrieereignisse treffen in Form von Nachrichten vom Gerät ein. Beim Hinzufügen von Telemetrieverarbeitungscode besteht der erste Schritt im Extrahieren des relevanten Teils dieser Gerätenachricht aus dem Event Grid-Ereignis. 

Unterschiedliche Geräte können ihre Nachrichten unterschiedlich strukturieren, sodass der Code für **diesen Schritt abhängig ist vom verbundenen Gerät**. 

Der folgende Code zeigt ein Beispiel für ein einfaches Gerät, das Telemetrie als JSON sendet. Dieses Beispiel wird vollständig untersucht in [*Tutorial: Erstellen einer End-to-End-Lösung*](./tutorial-end-to-end.md). Der folgende Code ermittelt die Geräte-ID des Geräts, das die Nachricht gesendet hat, und den Temperaturwert.

```csharp
JObject deviceMessage = (JObject)JsonConvert.DeserializeObject(eventGridEvent.Data.ToString());
string deviceId = (string)deviceMessage["systemProperties"]["iothub-connection-device-id"];
var temperature = deviceMessage["body"]["Temperature"];
```

Im nächsten Codebeispiel wird die ID und der Temperaturwert verwendet und dieser Zwilling damit „gepatcht“ (aktualisiert).

```csharp
//Update twin using device temperature
var uou = new UpdateOperationsUtility();
uou.AppendReplaceOp("/Temperature", temperature.Value<double>());
await client.UpdateDigitalTwinAsync(deviceId, uou.Serialize());
...
```

### <a name="update-your-azure-function-code"></a>Aktualisieren Ihres Azure-Funktionscodes

Sie verstehen jetzt den Code aus den früheren Beispielen. Öffnen Sie nun Visual Studio, und ersetzen Sie darin den Code Ihrer Azure-Funktion durch diesen Beispielcode.

```csharp
using System;
using System.Net.Http;
using Azure.Core.Pipeline;
using Azure.DigitalTwins.Core;
using Azure.DigitalTwins.Core.Serialization;
using Azure.Identity;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

namespace IotHubtoTwins
{
    public class IoTHubtoTwins
    {
        private static readonly string adtInstanceUrl = Environment.GetEnvironmentVariable("ADT_SERVICE_URL");
        private static readonly HttpClient httpClient = new HttpClient();

        [FunctionName("IoTHubtoTwins")]
        public async void Run([EventGridTrigger] EventGridEvent eventGridEvent, ILogger log)
        {
            if (adtInstanceUrl == null) log.LogError("Application setting \"ADT_SERVICE_URL\" not set");

            try
            {
                //Authenticate with Digital Twins
                ManagedIdentityCredential cred = new ManagedIdentityCredential("https://digitaltwins.azure.net");
                DigitalTwinsClient client = new DigitalTwinsClient(
                    new Uri(adtInstanceUrl), cred, new DigitalTwinsClientOptions 
                    { Transport = new HttpClientTransport(httpClient) });
                log.LogInformation($"ADT service client connection created.");
            
                if (eventGridEvent != null && eventGridEvent.Data != null)
                {
                    log.LogInformation(eventGridEvent.Data.ToString());

                    // Reading deviceId and temperature for IoT Hub JSON
                    JObject deviceMessage = (JObject)JsonConvert.DeserializeObject(eventGridEvent.Data.ToString());
                    string deviceId = (string)deviceMessage["systemProperties"]["iothub-connection-device-id"];
                    var temperature = deviceMessage["body"]["Temperature"];
                    
                    log.LogInformation($"Device:{deviceId} Temperature is:{temperature}");

                    //Update twin using device temperature
                    var uou = new UpdateOperationsUtility();
                    uou.AppendReplaceOp("/Temperature", temperature.Value<double>());
                    await client.UpdateDigitalTwinAsync(deviceId, uou.Serialize());
                }
            }
            catch (Exception e)
            {
                log.LogError($"Error in ingest function: {e.Message}");
            }
        }
    }
}
```

## <a name="connect-your-function-to-iot-hub"></a>Verbinden Ihrer Funktion mit IoT Hub

1. Richten Sie ein Ereignisziel für Hubdaten ein. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrer IoT Hub-Instanz. Erstellen Sie unter **Ereignisse** ein Abonnement für Ihre Azure-Funktion. 

    :::image type="content" source="media/how-to-ingest-iot-hub-data/add-event-subscription.png" alt-text="Screenshot des Azure-Portals mit „Hinzufügen eines Ereignisabonnements“":::

2. Füllen Sie auf der Seite **Ereignisabonnement erstellen** die Felder wie folgt aus:
    1. Geben Sie unter **Name** Ihren gewünschten Abonnementnamen ein.
    2. Wählen Sie unter **Ereignisschema** die Option **Event Grid-Schema** aus.
    3. Wählen Sie unter **Name des Systemthemas** einen eindeutigen Namen aus.
    4. Wählen Sie unter **Ereignistypen** als Ereignistyp, nach dem gefiltert werden soll, **Gerätetelemetrie** aus.
    5. Wählen Sie unter **Endpunktdetails** Ihre Azure-Funktion als Endpunkt aus.

    :::image type="content" source="media/how-to-ingest-iot-hub-data/event-subscription-2.png" alt-text="Screenshot des Azure-Portals mit Details zum Ereignisabonnement":::

## <a name="send-simulated-iot-data"></a>Senden von simulierten IoT-Daten

Verwenden Sie zum Testen Ihrer neuen Eingangsfunktion den Gerätesimulator aus [*Tutorial: Erstellen einer End-to-End-Lösung*](./tutorial-end-to-end.md). Grundlage dieses Tutorials ist ein in C# geschriebenes Beispielprojekt. Den Beispielcode finden Sie hier: [Azure Digital Twins (Beispiele)](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples). Sie verwenden in diesem Repository das Projekt **DeviceSimulator**.

Führen Sie im End-to-End-Tutorial die folgenden Schritte aus:
1. [*Registrieren des simulierten Geräts für IoT Hub*](./tutorial-end-to-end.md#register-the-simulated-device-with-iot-hub)
2. [*Konfigurieren und Ausführen der Simulation*](./tutorial-end-to-end.md#configure-and-run-the-simulation)

## <a name="validate-your-results"></a>Überprüfen Ihrer Ergebnisse

Während der Ausführung des vorstehenden Gerätesimulators ändert sich der Temperaturwert Ihres digitalen Zwillings. Führen Sie in der Azure CLI den folgenden Befehl zum Anzeigen des Temperaturwerts aus.

```azurecli-interactive
az dt twin query -q "select * from digitaltwins" -n {digital_twins_instance_name}
```

Die Ausgabe sollte einen Temperaturwert wie diesen enthalten:

```json
{
  "result": [
    {
      "$dtId": "thermostat67",
      "$etag": "W/\"0000000-1e83-4f7f-b448-524371f64691\"",
      "$metadata": {
        "$model": "dtmi:contosocom:DigitalTwins:Thermostat;1",
        "Temperature": {
          "ackCode": 200,
          "ackDescription": "Auto-Sync",
          "ackVersion": 1,
          "desiredValue": 69.75806974934324,
          "desiredVersion": 1
        }
      },
      "Temperature": 69.75806974934324
    }
  ]
}
```

Führen Sie zum Anzeigen der Wertänderung den vorstehenden Abfragebefehl wiederholt aus.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über eingehende und ausgehende Daten mit Azure Digital Twins:
* [*Konzepte: Integration in andere Dienste*](concepts-integration.md)
