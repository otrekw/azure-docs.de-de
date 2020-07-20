---
title: Verwenden von Azure Digital Twins zum Aktualisieren eines Azure Maps-Gebäudeplans
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie eine Azure-Funktion erstellen, die den Zwillingsgraphen und die Azure Digital Twins-Benachrichtigungen verwenden kann, um die in Azure Maps angezeigten Informationen zu aktualisieren.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 6/3/2020
ms.topic: how-to
ms.service: digital-twins
ms.reviewer: baanders
ms.openlocfilehash: 8f3e670a4f2a49bcce48be1ba0452a36cbf96df1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85392317"
---
# <a name="use-azure-digital-twins-to-update-an-azure-maps-indoor-map"></a>Verwenden von Azure Digital Twins zum Aktualisieren eines Azure Maps-Gebäudeplans

In diesem Artikel werden die Schritte erläutert, die erforderlich sind, um mithilfe von Azure Digital Twins-Daten Informationen zu aktualisieren, die in einem *Gebäudeplan* unter Verwendung von [Azure Maps](../azure-maps/about-azure-maps.md) angezeigt werden. Azure Digital Twins speichert einen Graphen der Beziehungen Ihres IoT-Geräts und leitet die Telemetrie an verschiedene Endpunkte weiter, was es zum perfekten Dienst für die Aktualisierung von Informationsüberlagerungen auf Karten macht.

Diese Anleitung umfasst Folgendes:

1. Konfigurieren Ihrer Azure Digital Twins-Instanz, um Aktualisierungsereignisse des Zwillings an eine Funktion in [Azure Functions](../azure-functions/functions-overview.md) zu senden.
2. Erstellen einer Azure-Funktion zur Aktualisierung eines Featurezustandssets für Azure Maps-Gebäudepläne.
3. Speichern Ihrer Karten-ID und Featurezustandsset-ID im Azure Digital Twins-Graphen.

### <a name="prerequisites"></a>Voraussetzungen

* Befolgen Sie das Azure Digital Twins-[Tutorial: Erstellen einer End-to-End-Lösung](./tutorial-end-to-end.md).
    * Sie werden diesen Zwilling um einen zusätzlichen Endpunkt und eine zusätzliche Route erweitern. Sie werden auch eine weitere Funktion aus diesem Tutorial zu Ihrer Funktions-App hinzufügen. 
* Befolgen Sie das Azure Maps-[Tutorial: Erstellen von Gebäudeplänen mit Azure Maps Creator](../azure-maps/tutorial-creator-indoor-maps.md), um einen Azure Maps-Gebäudeplan mit einem *Featurezustandsset* zu erstellen.
    * [Featurezustandssets](../azure-maps/creator-indoor-maps.md#feature-statesets) sind Sammlungen dynamischer Eigenschaften (Zustände), die Datasetfeatures wie z. B. Räumen oder Ausstattung zugewiesen sind. Im obigen Azure Maps-Tutorial speichert das Featurezustandsset den Raumstatus, den Sie auf einer Karte anzeigen werden.
    * Sie benötigen Ihre *Featurezustandsset-ID* und die Azure Maps-*Abonnement-ID*.

### <a name="topology"></a>Topologie

Die Abbildung unten zeigt, wo die Integrationselemente der Gebäudepläne in diesem Tutorial in ein größeres Azure Digital Twins-End-to-End-Szenario passen.

:::image type="content" source="media/how-to-integrate-maps/maps-integration-topology.png" alt-text="Eine Ansicht der Azure-Dienste in einem End-to-End-Szenario, wobei die Komponente der Gebäudeplanintegration hervorgehoben wird" lightbox="media/how-to-integrate-maps/maps-integration-topology.png":::

## <a name="create-a-function-to-update-a-map-when-twins-update"></a>Erstellen einer Funktion zum Aktualisieren einer Karte beim Aktualisieren von Zwillingen

Zunächst erstellen Sie eine Route in Azure Digital Twins, um alle Ereignisse zur Aktualisierung der Zwillinge an ein Event Grid-Thema weiterzuleiten. Dann verwenden Sie eine Azure-Funktion, um diese Aktualisierungsnachrichten zu lesen und ein Featurezustandsset in Azure Maps zu aktualisieren. 

## <a name="create-a-route-and-filter-to-twin-update-notifications"></a>Erstellen einer Route und eines Filters für Benachrichtigungen beim Aktualisieren von Zwillingen

Azure Digital Twins-Instanzen können Ereignisse zur Aktualisierung von Zwillingen ausgeben, wenn der Zustand eines Zwillings aktualisiert wird. Das [Azure Digital Twins-Tutorial: Verbinden einer End-to-End-Lösung](./tutorial-end-to-end.md), das oben verknüpft ist, führt Sie durch ein Szenario, in dem ein Temperaturattribut, das an den Zwilling eines Raums angefügt ist, mit einem Thermometer aktualisiert wird. Sie werden diese Lösung erweitern, indem Sie Aktualisierungsbenachrichtigungen für Zwillinge abonnieren und diese Informationen zum Aktualisieren unserer Karten verwenden.

Dieses Muster liest direkt aus dem Raumzwilling und nicht vom IoT-Gerät, was uns die Flexibilität bietet, die zugrunde liegende Datenquelle für die Temperatur zu ändern, ohne dass wir unsere Zuordnungslogik aktualisieren müssen. Sie können z. B. mehrere Thermometer hinzufügen oder diesen Raum so einrichten, dass ein Thermometer mit einem anderen Raum gemeinsam genutzt wird, ohne dass Sie unsere Kartenlogik aktualisieren müssen.

1. Erstellen Sie ein Event Grid-Thema, das Ereignisse von unserer Azure Digital Twins-Instanz empfangen wird.
    ```azurecli
    az eventgrid topic create -g <your-resource-group-name> --name <your-topic-name> -l <region>
    ```

2. Erstellen Sie einen Endpunkt, um Ihr Event Grid-Thema mit Azure Digital Twins zu verknüpfen.
    ```azurecli
    az dt endpoint create eventgrid --endpoint-name <Event-Grid-endpoint-name> --eventgrid-resource-group <Event-Grid-resource-group-name> --eventgrid-topic <your-Event-Grid-topic-name> -n <your-Azure-Digital-Twins-instance-name>
    ```

3. Erstellen Sie eine Route in Azure Digital Twins, um Ereignisse zur Aktualisierung von Zwillingen an Ihren Endpunkt zu senden.
    ```azurecli
    az dt route create -n <your-Azure-Digital-Twins-instance-name> --endpoint-name <Event-Grid-endpoint-name> --route-name <my_route> --filter "{ "endpointId": "<endpoint-ID>","filter": "type = 'Microsoft.DigitalTwins.Twin.Update'"}"
    ```

## <a name="create-an-azure-function-to-update-maps"></a>Erstellen einer Azure-Funktion zum Aktualisieren von Karten

Sie werden eine durch ein Event Grid ausgelöste Funktion innerhalb unserer Funktions-App aus dem [End-to-End-Tutorial](./tutorial-end-to-end.md) erstellen. Diese Funktion entpackt diese Benachrichtigungen und sendet Aktualisierungen an ein Azure Maps-Featurezustandsset, um die Temperatur in einem Raum zu aktualisieren. 

Referenzinformationen finden Sie im folgenden Dokument: [Azure Event Grid-Trigger für Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-bindings-event-grid-trigger).

Ersetzen Sie den Funktionscode durch folgenden Code. Er wird nur Aktualisierungen für Raumzwillinge herausfiltern, die aktualisierte Temperatur lesen und diese Informationen an Azure Maps senden.

```C#
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using System;
using System.Threading.Tasks;
using System.Net.Http;

namespace SampleFunctionsApp
{
    public static class ProcessDTUpdatetoMaps
    {   //Read maps credentials from application settings on function startup
        private static string statesetID = Environment.GetEnvironmentVariable("statesetID");
        private static string subscriptionKey = Environment.GetEnvironmentVariable("subscription-key");
        private static HttpClient httpClient = new HttpClient();

        [FunctionName("ProcessDTUpdatetoMaps")]
        public static async Task Run([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
        {
            JObject message = (JObject)JsonConvert.DeserializeObject(eventGridEvent.Data.ToString());
            log.LogInformation("Reading event from twinID:" + eventGridEvent.Subject.ToString() + ": " +
                eventGridEvent.EventType.ToString() + ": " + message["data"]);

            //Parse updates to "space" twins
            if (message["data"]["modelId"].ToString() == "dtmi:contosocom:DigitalTwins:Space;1")
            {   //Set the ID of the room to be updated in our map. 
                //Replace this line with your logic for retrieving featureID. 
                string featureID = "UNIT103";

                //Iterate through the properties that have changed
                foreach (var operation in message["data"]["patch"])
                {
                    if (operation["op"].ToString() == "replace" && operation["path"].ToString() == "/Temperature")
                    {   //Update the maps feature stateset
                        var postcontent = new JObject(new JProperty("States", new JArray(
                            new JObject(new JProperty("keyName", "temperature"),
                                 new JProperty("value", operation["value"].ToString()),
                                 new JProperty("eventTimestamp", DateTime.Now.ToString("s"))))));

                        var response = await httpClient.PostAsync(
                            $"https://atlas.microsoft.com/featureState/state?api-version=1.0&statesetID={statesetID}&featureID={featureID}&subscription-key={subscriptionKey}",
                            new StringContent(postcontent.ToString()));

                        log.LogInformation(await response.Content.ReadAsStringAsync());
                    }
                }
            }
        }
    }
}
```

Sie müssen zwei Umgebungsvariablen in Ihrer Funktions-App festlegen. Eine Variable ist Ihr [primärer Azure Maps-Abonnementschlüssel](../azure-maps/quick-demo-map-app.md#get-the-primary-key-for-your-account) und die andere ist Ihre [Azure Maps-Zustandsset-ID](../azure-maps/tutorial-creator-indoor-maps.md#create-a-feature-stateset).

```azurecli-interactive
az functionapp config appsettings set --settings "subscription-key=<your-Azure-Maps-primary-subscription-key> -g <your-resource-group> -n <your-App-Service-(function-app)-name>"
az functionapp config appsettings set --settings "statesetID=<your-Azure-Maps-stateset-ID> -g <your-resource-group> -n <your-App-Service-(function-app)-name>
```

### <a name="view-live-updates-on-your-map"></a>Anzeigen von Liveupdates auf Ihrer Karte

Führen Sie die folgenden Schritte aus, um die Liveupdate-Temperatur anzuzeigen:

1. Beginnen Sie mit dem Senden simulierter IoT-Daten, indem Sie das **DeviceSimulator**-Projekt aus folgendem Azure Digital Twins-Tutorial ausführen: [Tutorial: Erstellen einer End-to-End-Lösung](tutorial-end-to-end.md). Die Anweisungen hierzu finden Sie im Abschnitt [*Konfigurieren und Ausführen der Simulation*](././tutorial-end-to-end.md#configure-and-run-the-simulation).
2. Verwenden Sie [das Modul **Azure Maps Indoor**](../azure-maps/how-to-use-indoor-module.md), um Ihre mit Azure Maps Creator erstellten Karten für Gebäudepläne zu rendern.
    1. Kopieren Sie den HTML-Code aus dem Abschnitt [*Beispiel: Verwenden des Moduls „Gebäudepläne“* ](../azure-maps/how-to-use-indoor-module.md#example-use-the-indoor-maps-module) des Gebäudeplan-[Tutorials: Verwenden des Moduls „Gebäudepläne“ von Azure Maps](../azure-maps/how-to-use-indoor-module.md) in eine lokale Datei.
    1. Ersetzen Sie die *tilesetId* und *statesetID* in der lokalen HTML-Datei durch Ihre Werte.
    1. Öffnen Sie diese Datei in Ihrem Browser.

Beide Beispiele senden die Temperatur in einem kompatiblen Bereich, sodass die Farbe von Raum 121 etwa alle 30 Sekunden auf der Karte aktualisiert werden sollte.

:::image type="content" source="media/how-to-integrate-maps/maps-temperature-update.png" alt-text="Ein Büroplan, der Raum 121 in Orange anzeigt":::

## <a name="store-your-maps-information-in-azure-digital-twins"></a>Speichern Ihrer Karteninformationen in Azure Digital Twins

Nachdem Sie jetzt über eine hartcodierte Lösung zur Aktualisierung Ihrer Karteninformationen verfügen, können Sie den Azure Digital Twins-Graphen verwenden, um alle Informationen zu speichern, die für die Aktualisierung Ihres Gebäudeplans erforderlich sind. Dies würde die Zustandsset-ID, die Kartenabonnement-ID und die Feature-ID jeder Karte bzw. jedes Ortes umfassen. 

Eine Lösung für dieses spezielle Beispiel würde die Aktualisierung jedes Raums der obersten Ebene mit einer Zustandsset-ID und einem Kartenabonnement-ID-Attribut sowie die Aktualisierung jedes Raums mit einer Feature-ID umfassen. Sie müssten diese Werte einmal bei der Initialisierung des Zwillingsgraphen festlegen und diese Werte dann für jedes Aktualisierungsereignis des Zwillings abfragen.

Abhängig von der Konfiguration Ihrer Topologie können Sie diese drei Attribute auf verschiedenen Ebenen speichern, die mit der Granularität Ihrer Karte korrelieren.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwalten, Aktualisieren und Abrufen von Informationen aus dem Zwillingsgraphen finden Sie in den folgenden Referenzen:

* [Gewusst wie: Verwalten digitaler Zwillinge](./how-to-manage-twin.md)
* [Gewusst wie: Abfragen des Zwillingsgraphen](./how-to-query-graph.md)