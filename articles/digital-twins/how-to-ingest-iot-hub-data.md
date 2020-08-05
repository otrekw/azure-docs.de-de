---
title: Erfassen von Telemetriedaten aus IoT Hub
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie Gerätetelemetrienachrichten aus IoT Hub erfassen.
author: cschormann
ms.author: cschorm
ms.date: 3/17/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 7c73f007f85a963a09de4e05222082fd52f784c0
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87131564"
---
# <a name="ingest-iot-hub-telemetry-into-azure-digital-twins"></a>Erfassen von IoT Hub Telemetriedaten in Azure Digital Twins

Azure Digital Twins wird mit Daten von IoT-Geräten und anderen Quellen gesteuert. Eine gängige Quelle für Gerätedaten zur Verwendung in Azure Digital Twins ist [IoT Hub](../iot-hub/about-iot-hub.md).

Während der Vorschauphase besteht die Vorgehensweise zum Erfassen von Daten in Azure Digital Twins im Einrichten einer externen Computeressource wie einer [Azure-Funktion](../azure-functions/functions-overview.md), die die Daten empfängt und mithilfe der [Digital Twins-APIs](how-to-use-apis-sdks.md) nach Bedarf Eigenschaften festlegt oder Telemetrieereignisse zu [digitalen Zwillingen](concepts-twins-graph.md) auslöst. 

In dieser Schrittanleitung wird erläutert, wie Sie eine Azure-Funktion schreiben, die Telemetrie von IoT Hub erfassen kann.

## <a name="example-telemetry-scenario"></a>Beispieltelemetrieszenario

In dieser Schrittanleitung wird erläutert, wie Sie Nachrichten von IoT Hub mithilfe einer Azure-Funktion an Azure Digital Twins senden. Dazu gibt es viele mögliche Konfigurationen und Abgleichstrategien, das Beispiel für diesen Artikel enthält jedoch die folgenden Teile:
* Ein Thermometergerät in IoT Hub mit einer bekannten Geräte-ID.
* Ein digitaler Zwilling als Darstellung des Geräts mit einer übereinstimmenden ID
* Ein digitaler Zwilling als Darstellung eines Raums

> [!NOTE]
> In diesem Beispiel wird eine einfache ID-Übereinstimmung zwischen der Geräte-ID und der ID eines zugehörigen digitalen Zwillings verwendet. Es können jedoch auch anspruchsvollere Zuordnungen vom Gerät zu dessen Zwilling bereitgestellt werden, z. B. eine Zuordnungstabelle.

Wenn ein Temperaturtelemetrieereignis vom Thermometergerät gesendet wird, sollte die Eigenschaft *temperature* des *Raum*-Zwillings aktualisiert werden. Dazu erstellen Sie eine Zuordnung von einem Telemetrieereignis auf einem Gerät zu einem Eigenschaftensetter für den digitalen Zwilling. Sie verwenden die Topologieinformationen aus dem [Zwillingsgraphen](concepts-twins-graph.md), um den *Raum*-Zwilling zu finden, und können dann die Eigenschaft des Zwillings festlegen. In anderen Szenarien möchten Benutzer möglicherweise eine Eigenschaft für den passenden Zwilling festlegen (in diesem Beispiel den Zwilling mit der ID *123*). Azure Digital Twins bietet Ihnen große Flexibilität bei der Entscheidung, wie Telemetriedaten Zwillingen zugeordnet werden. 

Dieses Szenario ist nachstehend in einem Diagramm dargestellt:

:::image type="content" source="media/how-to-ingest-iot-hub-data/events.png" alt-text="Ein IoT Hub Gerät sendet Temperaturtelemetrie über IoT Hub-, Event Grid- oder Systemthemen an eine Azure-Funktion, die eine Temperatureigenschaft in Zwillingen in Azure Digital Twins aktualisiert." border="false":::

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Beispiel fortfahren, müssen Sie die folgenden Voraussetzungen erfüllen.
1. Erstellen Sie einen IoT Hub. Anweisungen finden Sie im Abschnitt *Erstellen eines IoT-Hubs* in [diesem IoT Hub-Schnellstart](../iot-hub/quickstart-send-telemetry-cli.md).
2. Erstellen Sie mindestens eine Azure-Funktion, um Ereignisse aus IoT Hub zu verarbeiten. Weitere Informationen finden Sie unter [*Vorgehensweise: Einrichten einer Azure-Funktion zum Verarbeiten von Daten*](how-to-create-azure-function.md). Dort wird das Erstellen einer grundlegenden Azure-Funktion erläutert, die eine Verbindung mit Azure Digital Twins herstellen und API-Funktionen von Azure Digital Twins aufrufen kann. Der Rest dieser Schrittanleitung baut auf dieser Funktion auf.
3. Richten Sie ein Ereignisziel für Hubdaten ein. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrer IoT Hub-Instanz. Erstellen Sie unter *Ereignisse* ein Abonnement für Ihre Azure-Funktion. 

    :::image type="content" source="media/how-to-ingest-iot-hub-data/add-event-subscription.png" alt-text="Azure-Portal: Hinzufügen eines Ereignisabonnements":::

4. Füllen Sie auf der Seite *Ereignisabonnement erstellen* die Felder wie folgt aus:
   * Geben Sie dem Abonnement unter *DETAILS ZUM EREIGNISABONNEMENT* einen Namen Ihrer Wahl.
   * Wählen Sie unter *EREIGNISTYPEN* als Ereignistyp, nach dem gefiltert werden soll, *Gerätetelemetrie* aus.
      - Nach Bedarf können Sie auch anderen Ereignistypen Filter hinzufügen.
   * Wählen Sie unter *ENDPUNKTDETAILS* Ihre Azure-Funktion als Endpunkt aus.

## <a name="create-an-azure-function-in-visual-studio"></a>Erstellen einer Azure-Funktion in Visual Studio

In diesem Abschnitt werden die gleichen Startschritte in Visual Studio und das Azure-Funktionsgerüst verwendet wie in [*Vorgehensweise: von Azure Functions-Apps für die Verarbeitung von Daten*](how-to-create-azure-function.md). Das Gerüst übernimmt die Authentifizierung und erstellt einen Dienstclient, der zum Verarbeiten von Daten und zum Aufrufen von Azure Digital Twins-APIs als Antwort bereit ist. 

Das Kernstück des Funktionsgerüsts ist:

```csharp
namespace FunctionSample
{
    public static class FooFunction
    {
        const string adtAppId = "https://digitaltwins.azure.net";
        private static string adtInstanceUrl = Environment.GetEnvironmentVariable("ADT_SERVICE_URL");
        private static HttpClient httpClient = new HttpClient();

        [FunctionName("Foo")]
        public static async Task Run([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
        {
            DigitalTwinsClient client = null;
            try
            {
                ManagedIdentityCredential cred = new ManagedIdentityCredential(adtAppId);
                DigitalTwinsClientOptions opts = 
                    new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
                client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, opts);
                                                
                log.LogInformation($"ADT service client connection created.");
            }
            catch (Exception e)
            {
                log.LogError($"ADT service client connection failed. " + e.ToString());
                return;
            }
            log.LogInformation(eventGridEvent.Data.ToString());
        }
    }
}
```

In den folgenden Schritten fügen Sie bestimmten Code hinzu, um IoT-Telemetrieereignisse von IoT Hub zu verarbeiten.  

## <a name="add-telemetry-processing"></a>Hinzufügen einer Telemetrieverarbeitung

Telemetrieereignisse treffen in Form von Nachrichten vom Gerät ein. Beim Hinzufügen von Telemetrieverarbeitungscode besteht der erste Schritt im Extrahieren des relevanten Teils dieser Gerätenachricht aus dem Event Grid-Ereignis. 

Unterschiedliche Geräte können ihre Nachrichten unterschiedlich strukturieren, sodass der Code für diesen Schritt vom verbundenen Gerät abhängig ist. 

Der folgende Code zeigt ein Beispiel für ein einfaches Gerät, das Telemetrie als JSON sendet. Im Beispiel werden die Geräte-ID des Geräts, das die Nachricht gesendet hat, und der Temperaturwert extrahiert.

```csharp
JObject job = eventGridEvent.Data as JObject;
string devid = (string)job["systemProperties"].ToObject<JObject>().Property("IoT-hub-connection-device-ID").Value;
double temp = (double)job["body"].ToObject<JObject>().Property("temperature").Value;
```

Beachten Sie, dass der Zweck dieser Übung darin besteht, die Temperatur eines *Raums* im Zwillingsgraphen zu aktualisieren. Dies bedeutet, dass das Ziel für die Nachricht nicht der diesem Gerät zugeordnete digitale Zwilling ist, sondern der übergeordnete *Raum*-Zwilling. Sie können den übergeordneten Zwilling mithilfe des Geräte-ID-Werts ermitteln, den Sie mithilfe des obigen Codes aus der Telemetrienachricht extrahiert haben.

Verwenden Sie hierzu die Azure Digital Twins-APIs, um auf die eingehenden Beziehungen zu dem Zwilling zuzugreifen, der das Gerät darstellt (in diesem Fall ist dies die gleiche ID wie das Gerät). Aus der eingehenden Beziehung können Sie mit dem nachstehenden Codeausschnitt die ID des übergeordneten Elements ermitteln.

Der nachstehende Codeausschnitt zeigt, wie Sie eingehende Beziehungen eines Zwillings abrufen:

```csharp
AsyncPageable<IncomingRelationship> res = client.GetIncomingRelationshipsAsync(twin_id);
await foreach (IncomingRelationship irel in res)
{
    Log.Ok($"Relationship: {irel.RelationshipName} from {irel.SourceId} | {irel.RelationshipId}");
}
```

Das übergeordnete Element Ihres Zwillings befindet sich in der *SourceId*-Eigenschaft der Beziehung.

Ein Modell eines Zwillings, der ein Gerät darstellt, weist häufig nur eine einzelne eingehende Beziehung auf. In diesem Fall können Sie die erste (und einzige) zurückgegebene Beziehung auswählen. Wenn in Ihren Modellen mehrere Typen von Beziehungen zu diesem Zwilling zulässig sind, müssen Sie möglicherweise genauere Angaben machen, um zwischen den eingehenden Beziehungen auszuwählen. Eine gängige Methode hierfür ist, die Beziehung nach `RelationshipName` auszuwählen. 

Sobald Sie über die ID des übergeordneten Zwillings verfügen, der den *Raum* darstellt, können Sie diesen Zwilling „patchen“ (aktualisieren). Verwenden Sie dazu den folgenden Code:

```csharp
UpdateOperationsUtility uou = new UpdateOperationsUtility();
uou.AppendAddOp("/Temperature", temp);
try
{
    await client.UpdateDigitalTwinAsync(twin_id, uou.Serialize());
    Log.Ok($"Twin '{twin_id}' updated successfully!");
}
...
```

### <a name="full-azure-function-code"></a>Vollständiger Code der Azure-Funktion

Wenn Sie den Code aus den vorherigen Beispielen verwenden, finden Sie hier die gesamte Azure-Funktion im Kontext:

```csharp
[FunctionName("ProcessHubToDTEvents")]
public async void Run([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
{
    // After this is deployed, in order for this function to be authorized on Azure Digital Twins APIs,
    // you'll need to turn the Managed Identity Status to "On", 
    // grab the Object ID of the function, and assign the "Azure Digital Twins Owner (Preview)" role to this function identity.

    DigitalTwinsClient client = null;
    //log.LogInformation(eventGridEvent.Data.ToString());
    // Authenticate on Azure Digital Twins APIs
    try
    {
        
        ManagedIdentityCredential cred = new ManagedIdentityCredential(adtAppId);
        client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
        log.LogInformation($"ADT service client connection created.");
    }
    catch (Exception e)
    {
        log.LogError($"ADT service client connection failed. " + e.ToString());
        return;
    }

    if (client != null)
    {
        try
        {
            if (eventGridEvent != null && eventGridEvent.Data != null)
            {
                #region Open this region for message format information
                // Telemetry message format
                //{
                //  "properties": { },
                //  "systemProperties": 
                // {
                //    "iothub-connection-device-id": "thermostat1",
                //    "iothub-connection-auth-method": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
                //    "iothub-connection-auth-generation-id": "637199981642612179",
                //    "iothub-enqueuedtime": "2020-03-18T18:35:08.269Z",
                //    "iothub-message-source": "Telemetry"
                //  },
                //  "body": "eyJUZW1wZXJhdHVyZSI6NzAuOTI3MjM0MDg3MTA1NDg5fQ=="
                //}
                #endregion

                // Reading deviceId from message headers
                log.LogInformation(eventGridEvent.Data.ToString());
                JObject job = (JObject)JsonConvert.DeserializeObject(eventGridEvent.Data.ToString());
                string deviceId = (string)job["systemProperties"]["iothub-connection-device-id"];
                log.LogInformation($"Found device: {deviceId}");

                // Extracting temperature from device telemetry
                byte[] body = System.Convert.FromBase64String(job["body"].ToString());
                var value = System.Text.ASCIIEncoding.ASCII.GetString(body);
                var bodyProperty = (JObject)JsonConvert.DeserializeObject(value);
                var temperature = bodyProperty["Temperature"];
                log.LogInformation($"Device Temperature is:{temperature}");

                // Update device Temperature property
                await AdtUtilities.UpdateTwinProperty(client, deviceId, "/Temperature", temperature, log);

                // Find parent using incoming relationships
                string parentId = await AdtUtilities.FindParent(client, deviceId, "contains", log);
                if (parentId != null)
                {
                    await AdtUtilities.UpdateTwinProperty(client, parentId, "/Temperature", temperature, log);
                }

            }
        }
        catch (Exception e)
        {
            log.LogError($"Error in ingest function: {e.Message}");
        }
    }
}
```

Die Hilfsfunktion zum Suchen eingehender Beziehungen:
```csharp
public static async Task<string> FindParent(DigitalTwinsClient client, string child, string relname, ILogger log)
{
    // Find parent using incoming relationships
    try
    {
        AsyncPageable<IncomingRelationship> rels = client.GetIncomingRelationshipsAsync(child);

        await foreach (IncomingRelationship ie in rels)
        {
            if (ie.RelationshipName == relname)
                return (ie.SourceId);
        }
    }
    catch (RequestFailedException exc)
    {
        log.LogInformation($"*** Error in retrieving parent:{exc.Status}:{exc.Message}");
    }
    return null;
}
```

Und die Hilfsfunktion zum Patchen des Zwillings:
```csharp
public static async Task UpdateTwinProperty(DigitalTwinsClient client, string twinId, string propertyPath, object value, ILogger log)
{
    // If the twin does not exist, this will log an error
    try
    {
        // Update twin property
        UpdateOperationsUtility uou = new UpdateOperationsUtility();
        uou.AppendAddOp(propertyPath, value);
        await client.UpdateDigitalTwinAsync(twinId, uou.Serialize());
    }
    catch (RequestFailedException exc)
    {
        log.LogInformation($"*** Error:{exc.Status}/{exc.Message}");
    }
}
```

Nun verfügen Sie über eine Azure-Funktion, die zum Lesen und Interpretieren der aus IoT Hub eingehenden Daten in diesem Szenario geeignet ist.

## <a name="debug-azure-function-apps-locally"></a>Lokales Debuggen von Azure-Funktions-Apps

Es ist möglich, Azure-Funktionen lokal mit einem Event Grid-Trigger zu debuggen. Weitere Informationen hierzu finden Sie unter [*Lokales Debuggen des Event Grid-Triggers*](../azure-functions/functions-debug-event-grid-trigger-local.md).

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über eingehende und ausgehende Daten mit Azure Digital Twins:
* [*Konzepte: Integration in andere Dienste*](concepts-integration.md)
