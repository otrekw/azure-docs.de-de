---
title: Verwalten von Endpunkten und Routen (APIs und CLI)
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie Endpunkte und Ereignisrouten für Azure Digital Twins-Daten einrichten und verwalten.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 10/12/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 0b8bd9006482daf7c9218f0f3dbb16d2e08359bf
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2020
ms.locfileid: "94533751"
---
# <a name="manage-endpoints-and-routes-in-azure-digital-twins-apis-and-cli"></a>Verwalten von Endpunkten und Routen in Azure Digital Twins (APIs und CLI)

[!INCLUDE [digital-twins-route-selector.md](../../includes/digital-twins-route-selector.md)]

In Azure Digital Twins können Sie [Ereignisbenachrichtigungen](how-to-interpret-event-data.md) an Downstreamdienste oder verbundene Computeressourcen weiterleiten. Dies erfolgt, indem zunächst **Endpunkte** eingerichtet werden, die die Ereignisse empfangen können. Anschließend können Sie [**Ereignisrouten**](concepts-route-events.md) erstellen, die angeben, welche von Azure Digital Twins generierten Ereignisse an welche Endpunkte übermittelt werden.

Dieser Artikel führt Sie schrittweise durch das Erstellen von Endpunkten und Routen mit der [Ereignisrouten-APIs](/rest/api/digital-twins/dataplane/eventroutes), dem [.NET (C#) SDK](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true) und der [CLI von Azure Digital Twins](how-to-use-cli.md).

Alternativ können Sie auch Endpunkte und Routen mit dem [Azure-Portal](https://portal.azure.com) verwalten. Eine Version dieses Artikels, die stattdessen das Portal verwendet, finden Sie unter [*Vorgehensweise: Verwalten von Endpunkten und Routen (Portal)*](how-to-manage-routes-portal.md).

## <a name="prerequisites"></a>Voraussetzungen

* Sie benötigen ein **Azure-Konto** (Sie können [hier](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) ein kostenloses Konto einrichten).
* Sie benötigen eine **Azure Digital Twins-Instanz** in Ihrem Azure-Abonnement. Falls Sie noch keine Instanz besitzen, können Sie zum Erstellen die Schritte unter [*Vorgehensweise: Einrichten einer Instanz und von Authentifizierung*](how-to-set-up-instance-cli.md) verwenden. Notieren Sie sich die folgenden Werte aus dem Setup, um sie später in diesem Artikel zu verwenden:
    - Instanzname
    - Ressourcengruppe
    
## <a name="create-an-endpoint-for-azure-digital-twins"></a>Erstellen eines Endpunkts für Azure Digital Twins

Dies sind die unterstützten Typen von Endpunkten, die Sie für Ihre Instanz erstellen können:
* [Event Grid](../event-grid/overview.md)
* [Event Hubs](../event-hubs/event-hubs-about.md)
* [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)

Weitere Informationen zu den verschiedenen Endpunkttypen finden Sie unter [*Wählen zwischen Azure-Messagingdiensten*](../event-grid/compare-messaging-services.md).

Um einen Endpunkt mit Azure Digital Twins zu verknüpfen, muss das für den Endpunkt verwendete Event Grid-Thema, der Event Hub oder Service Bus bereits vorhanden sein. 

### <a name="create-an-event-grid-endpoint"></a>Erstellen eines Event Grid-Endpunkts

Das folgende Beispiel zeigt, wie ein Endpunkt vom Event Grid-Typ mit der Azure CLI erstellt wird. Sie können [Azure Cloud Shell](https://shell.azure.com) verwenden oder die [CLI lokal installieren](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest).

Erstellen Sie zunächst ein Event Grid-Thema. Sie können den folgenden Befehl verwenden oder die Schritte ausführlicher anzeigen, indem Sie [den Abschnitt *Erstellen eines benutzerdefinierten Themas*](../event-grid/custom-event-quickstart-portal.md#create-a-custom-topic) des Event Grid-Schnellstarts *Benutzerdefinierte Ereignisse* verwenden.

```azurecli-interactive
az eventgrid topic create -g <your-resource-group-name> --name <your-topic-name> -l <region>
```

> [!TIP]
> Führen Sie den folgenden Befehl aus, um eine Liste mit Namen von Azure-Regionen auszugeben, die in der Azure CLI an Befehle übergeben werden können:
> ```azurecli-interactive
> az account list-locations -o table
> ```

Nachdem Sie das Thema erstellt haben, können Sie es mit dem folgenden [Azure Digital Twins-CLI-Befehl](how-to-use-cli.md) mit Azure Digital Twins verknüpfen:

```azurecli-interactive
az dt endpoint create eventgrid --endpoint-name <Event-Grid-endpoint-name> --eventgrid-resource-group <Event-Grid-resource-group-name> --eventgrid-topic <your-Event-Grid-topic-name> -n <your-Azure-Digital-Twins-instance-name>
```

Nun ist das Event Grid-Thema als Endpunkt innerhalb von Azure Digital Twins unter dem mit dem Argument `--endpoint-name` angegebenen Namen verfügbar. In der Regel werden Sie diesen Namen als Ziel einer **Ereignisroute** verwenden, die Sie [später in diesem Artikel](#create-an-event-route) mithilfe der Azure Digital Twins-Dienst-API erstellen.

### <a name="create-an-event-hubs-or-service-bus-endpoint"></a>Erstellen eines Event Hubs- oder Service Bus-Endpunkts

Der Vorgang zum Erstellen von Event Hubs- oder Service Bus-Endpunkten ähnelt dem oben gezeigten Event Grid-Vorgang.

Erstellen Sie zunächst die Ressourcen, die Sie als Endpunkt verwenden möchten. Folgendes ist erforderlich:
* Service Bus: _Service Bus-Namespace_, _Service Bus-Thema_, _Autorisierungsregel_
* Event Hubs: _Event Hubs-Namespace_, _Event Hub_, _Autorisierungsregel_

Verwenden Sie dann die folgenden Befehle, um die Endpunkte in Azure Digital Twins zu erstellen: 

* Hinzufügen eines Service Bus-Themaendpunkts (erfordert eine vorab erstellte Service Bus-Ressource)
```azurecli-interactive 
az dt endpoint create servicebus --endpoint-name <Service-Bus-endpoint-name> --servicebus-resource-group <Service-Bus-resource-group-name> --servicebus-namespace <Service-Bus-namespace> --servicebus-topic <Service-Bus-topic-name> --servicebus-policy <Service-Bus-topic-policy> -n <your-Azure-Digital-Twins-instance-name>
```

* Hinzufügen eines Event Hubs-Endpunkts (erfordert eine vorab erstellte Event Hubs-Ressource)
```azurecli-interactive
az dt endpoint create eventhub --endpoint-name <Event-Hub-endpoint-name> --eventhub-resource-group <Event-Hub-resource-group> --eventhub-namespace <Event-Hub-namespace> --eventhub <Event-Hub-name> --eventhub-policy <Event-Hub-policy> -n <your-Azure-Digital-Twins-instance-name>
```

### <a name="create-an-endpoint-with-dead-lettering"></a>Erstellen eines Endpunkts mit unzustellbaren Nachrichten

Wenn ein Endpunkt innerhalb eines bestimmten Zeitraums oder nach einer bestimmten Anzahl von Übermittlungsversuchen nicht übermittelt werden kann, kann Event Grid das nicht übermittelte Ereignis an ein Speicherkonto senden. Dieser Prozess wird als Speicherung **unzustellbarer Nachrichten** bezeichnet.

Zum Erstellen eines Endpunkts mit aktivierten unzustellbaren Nachrichten müssen Sie die [ARM-APIs](/rest/api/digital-twins/controlplane/endpoints/digitaltwinsendpoint_createorupdate) verwenden, um den Endpunkt zu erstellen. 

Wenn Sie den Speicherort für unzustellbare Nachrichten festlegen möchten, benötigen Sie ein Speicherkonto mit einem Container. Sie geben die URL für diesen Container an, wenn Sie den Endpunkt erstellen. Die unzustellbaren Nachrichten werden als Container-URL mit einem SAS-Token bereitgestellt. Dieses Token benötigt nur die `write`-Berechtigung für den Zielcontainer innerhalb des Speicherkontos. Die vollständig formatierte URL weist das folgende Format auf: `https://<storageAccountname>.blob.core.windows.net/<containerName>?<SASToken>`

Weitere Informationen zu SAS-Token finden Sie hier: [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](/azure/storage/common/storage-sas-overview)

Weitere Informationen zu unzustellbaren Nachrichten finden Sie unter [*Konzepte: Ereignisrouten*](concepts-route-events.md#dead-letter-events).

#### <a name="configuring-the-endpoint"></a>Konfigurieren des Endpunkts

Wenn Sie einen Endpunkt erstellen, fügen Sie dem `properties`-Objekt im Text der Anforderungs ein `deadLetterSecret` hinzu, das eine Container-URL und ein SAS-Token für Ihr Speicherkonto enthält.

```json
{
  "properties": {
    "endpointType": "EventGrid",
    "TopicEndpoint": "https://contosoGrid.westus2-1.eventgrid.azure.net/api/events",
    "accessKey1": "xxxxxxxxxxx",
    "accessKey2": "xxxxxxxxxxx",
    "deadLetterSecret":"https://<storageAccountname>.blob.core.windows.net/<containerName>?<SASToken>"
  }
}
```

Weitere Informationen finden Sie in der Dokumentation zur Azure Digital Twins-REST-API: [Endpunkte: DigitalTwinsEndpoint und CreateOrUpdate](/rest/api/digital-twins/controlplane/endpoints/digitaltwinsendpoint_createorupdate).

### <a name="message-storage-schema"></a>Nachrichtenspeicherschema

Unzustellbare Nachrichten werden im folgenden Format in Ihrem Speicherkonto gespeichert:

`{container}/{endpointName}/{year}/{month}/{day}/{hour}/{eventId}.json`

Unzustellbare Nachrichten entsprechen dem Schema des ursprünglichen Ereignisses, das an den ursprünglichen Endpunkt übermittelt werden sollte.

Im Folgenden finden Sie ein Beispiel für eine unzustellbare Nachricht für eine [Benachrichtigung zur Erstellung eines Zwillings](how-to-interpret-event-data.md#digital-twin-life-cycle-notifications):

```json
{
  "specversion": "1.0",
  "id": "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.DigitalTwins.Twin.Create",
  "source": "<yourInstance>.api.<yourregion>.da.azuredigitaltwins-test.net",
  "data": {
    "$dtId": "<yourInstance>xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "$etag": "W/\"xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
    "TwinData": "some sample",
    "$metadata": {
      "$model": "dtmi:test:deadlettermodel;1",
      "room": {
        "lastUpdateTime": "2020-10-14T01:11:49.3576659Z"
      }
    }
  },
  "subject": "<yourInstance>xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "time": "2020-10-14T01:11:49.3667224Z",
  "datacontenttype": "application/json",
  "traceparent": "00-889a9094ba22b9419dd9d8b3bfe1a301-f6564945cb20e94a-01"
}
```

## <a name="create-an-event-route"></a>Erstellen einer Ereignisroute

Wenn Sie tatsächlich Daten von Azure Digital Twins an einen Endpunkt senden möchten, müssen Sie eine **Ereignisroute** definieren. Die **EventRoutes-APIs** von Azure Digital Twins ermöglichen es Entwicklern, den Ereignisfluss im gesamten System und zu Downstreamdiensten einzurichten. Weitere Informationen zu Ereignisrouten finden Sie unter [*Konzepte: Weiterleiten von Azure Digital Twins-Ereignissen*](concepts-route-events.md).

Die Beispiele in diesem Abschnitt verwenden das [.NET (C#) SDK](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true).

**Voraussetzung**: Sie müssen wie weiter oben in diesem Artikel beschrieben Endpunkte erstellen, bevor Sie mit dem Erstellen einer Route fortfahren können. Nachdem die Einrichtung Ihrer Endpunkte abgeschlossen ist, können Sie mit dem Erstellen einer Ereignisroute fortfahren.

>[!NOTE]
>Wenn Sie Ihre Endpunkte gerade erst bereitgestellt haben, vergewissern Sie sich, dass die Bereitstellung abgeschlossen ist, **bevor** Sie versuchen, die Endpunkte für eine neue Ereignisroute zu verwenden. Wenn die Routenbereitstellung fehlschlägt, weil die Endpunkte noch nicht bereit sind, warten Sie einige Minuten, und versuchen Sie es dann erneut.
>
> Wenn Sie eine Skripterstellung für diesen Flow durchführen, sollten Sie dies berücksichtigen, indem Sie eine Wartezeit von zwei bis drei Minuten vorsehen, damit der Endpunktdienst die Bereitstellung abschließen kann, bevor mit der Routeneinrichtung fortgefahren wird.

### <a name="creation-code-with-apis-and-the-c-sdk"></a>Erstellungscode mit APIs und dem C#-SDK

Ereignisrouten werden mithilfe von [Datenebenen-APIs](how-to-use-apis-sdks.md#overview-data-plane-apis) definiert. 

Eine Routendefinition kann folgende Elemente enthalten:
* Den Routennamen, den Sie verwenden möchten.
* Den Namen des Endpunkts, den Sie verwenden möchten.
* Ein Filter, der definiert, welche Ereignisse an den Endpunkt gesendet werden. 

Wenn es keinen Routennamen gibt, werden keine Nachrichten außerhalb von Azure Digital Twins weitergeleitet. Wenn es einen Routennamen gibt und der Filter `true` ergibt, werden alle Nachrichten an den Endpunkt weitergeleitet. Wenn es einen Routennamen gibt und ein anderer Filter hinzugefügt wird, werden Nachrichten auf der Grundlage des Filters gefiltert.

Eine Route sollte es ermöglichen, mehrere Benachrichtigungen und Ereignistypen auszuwählen. 

`CreateOrReplaceEventRouteAsync` ist der SDK-Aufruf, der zum Hinzufügen einer Ereignisroute verwendet wird. Hier ist ein Beispiel für seine Verwendung:

```csharp
string eventFilter = "$eventType = 'DigitalTwinTelemetryMessages' or $eventType = 'DigitalTwinLifecycleNotification'";
var er = new DigitalTwinsEventRoute("<your-endpointName>", eventFilter);
await CreateOrReplaceEventRouteAsync(client, "routeName", er);
```
    
> [!TIP]
> Alle SDK-Funktionen sind in synchronen und asynchronen Versionen enthalten.

### <a name="event-route-sample-code"></a>Beispielcode für Ereignisroute

Die folgende Beispielmethode zeigt, wie Sie eine Ereignisroute erstellen, auflisten und löschen können:
```csharp
private async static Task CreateEventRoute(DigitalTwinsClient client, String routeName, DigitalTwinsEventRoute er)
{
  try
  {
    Console.WriteLine("Create a route: testRoute1");
            
    // Make a filter that passes everything
    er.Filter = "true";
    await client.CreateOrReplaceEventRouteAsync(routeName, er);
    Console.WriteLine("Create route succeeded. Now listing routes:");
    Pageable<DigitalTwinsEventRoute> result = client.GetEventRoutes();
    foreach (DigitalTwinsEventRoute r in result)
    {
        Console.WriteLine($"Route {r.Id} to endpoint {r.EndpointName} with filter {r.Filter} ");
    }
    Console.WriteLine("Deleting routes:");
    foreach (DigitalTwinsEventRoute r in result)
    {
        Console.WriteLine($"Deleting route {r.Id}:");
        client.DeleteEventRoute(r.Id);
    }
  }
    catch (RequestFailedException e)
    {
        Console.WriteLine($"*** Error in event route processing ({e.ErrorCode}):\n${e.Message}");
    }
  }
```

## <a name="filter-events"></a>Filtern von Ereignissen

Ohne Filterung erhalten die Endpunkte eine Vielzahl von Ereignissen von Azure Digital Twins:
* Telemetrie, die von [digitalen Zwillingen](concepts-twins-graph.md) unter Verwendung der Azure Digital Twins-Dienst-API ausgelöst wird.
* Änderungsbenachrichtigungen für Zwillingseigenschaften, ausgelöst bei Eigenschaftsänderungen für einen Zwilling in der Azure Digital Twins-Instanz.
* Lebenszyklusereignisse, die ausgelöst werden, wenn Zwillinge oder Beziehungen erstellt oder gelöscht werden.

Sie können die gesendeten Ereignisse einschränken, indem Sie der Ereignisroute einen **Filter** für einen Endpunkt hinzufügen.

Um einen Filter hinzuzufügen, können Sie eine PUT-Anforderung für *https://{IhrHost}/EventRoutes/myNewRoute?api-version=2020-10-31* mit folgendem Hauptteil verwenden:

```json  
{
    "endpointName": "<endpoint-name>",
    "filter": "<filter-text>"
}
``` 

Dies sind die unterstützten Routenfilter. Verwenden Sie die Details in der Spalte *Filtertextschema*, um den Platzhalter `<filter-text>` im Anforderungstext oben zu ersetzen.

[!INCLUDE [digital-twins-route-filters](../../includes/digital-twins-route-filters.md)]

## <a name="manage-endpoints-and-routes-with-cli"></a>Verwalten von Endpunkten und Routen mit der Befehlszeilenschnittstelle (CLI)

Endpunkte und Routen können auch über die Azure Digital Twins-Befehlszeilenschnittstelle (CLI) verwaltet werden. Weitere Informationen zur Verwendung der CLI und zu den verfügbaren Befehlen finden Sie unter [*Vorgehensweise: Verwenden der Azure Digital Twins-Befehlszeilenschnittstelle*](how-to-use-cli.md).

[!INCLUDE [digital-twins-route-metrics](../../includes/digital-twins-route-metrics.md)]

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über die verschiedenen Arten von Ereignismeldungen, die Sie erhalten können:
* [*Gewusst wie: Interpretieren von Ereignisdaten*](how-to-interpret-event-data.md)