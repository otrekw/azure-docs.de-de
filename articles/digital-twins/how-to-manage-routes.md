---
title: Verwalten von Endpunkten und Routen
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie Endpunkte und Ereignisrouten für Azure Digital Twins-Daten einrichten und verwalten.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 6/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 923ae652872246916b2a4c5e8be95871983dbe95
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85559837"
---
# <a name="manage-endpoints-and-routes-in-azure-digital-twins"></a>Verwalten von Endpunkten und Routen in Azure Digital Twins

In Azure Digital Twins können Sie [Ereignisbenachrichtigungen](how-to-interpret-event-data.md) an Downstreamdienste weiterleiten oder eine Verbindung zu Computeressourcen herstellen. Dazu werden zunächst **Endpunkte** eingerichtet, die die Ereignisse empfangen können, gefolgt von den [**Ereignisrouten**](concepts-route-events.md), die angeben, welche von Azure Digital Twins erzeugten Ereignisse an welche Endpunkte übermittelt werden.

Zu den unterstützten Endpunkttypen gehören:
* [Event Hub](../event-hubs/event-hubs-about.md)
* [Event Grid](../event-grid/overview.md)
* [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)

Weitere Informationen zu den verschiedenen Endpunkten finden Sie unter [Wählen zwischen Azure-Messagingdiensten](https://docs.microsoft.com/azure/event-grid/compare-messaging-services).

Endpunkte und Routen werden mit den [**EventRoutes-APIs**](how-to-use-apis-sdks.md), dem [.NET (C#) SDK](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core) oder der [Azure Digital Twins CLI](how-to-use-cli.md) verwaltet. Sie können auch über das [Azure-Portal](https://portal.azure.com) verwaltet werden.

> [!NOTE]
> Das Verwalten von Ereignisrouten durch das Azure-Portal ist derzeit nur für Azure-Benutzer mit Unternehmensdomänenkonten verfügbar. 
>
>Wenn Sie ein persönliches [Microsoft-Konto (MSA)](https://account.microsoft.com/account/Account) verwenden, z. B. ein @outlook.com-Konto, nutzen Sie die Azure Digital Twins-APIs oder die Befehlszeilenschnittstelle, um Ereignisrouten wie in diesem Artikel beschrieben zu verwalten.

## <a name="create-an-endpoint-for-azure-digital-twins"></a>Erstellen eines Endpunkts für Azure Digital Twins

Um einen Endpunkt mit Azure Digital Twins zu verknüpfen, muss der Event Hub, das Event Grid-Thema oder der Service Bus, den Sie für den Endpunkt verwenden, bereits vorhanden sein. 

Das folgende Beispiel zeigt, wie ein Event Grid-Thema mit der Azure-Befehlszeilenschnittstelle (CLI) erstellt wird:

```azurecli
az eventgrid topic create -g <your-resource-group-name> --name <your-topic-name> -l <region>
```

> [!TIP]
> Führen Sie den folgenden Befehl aus, um eine Liste mit Namen von Azure-Regionen auszugeben, die in der Azure CLI an Befehle übergeben werden können:
> ```azurecli
> az account list-locations -o table
> ```

Nachdem Sie das Thema erstellt haben, können Sie es mit dem folgenden Befehl mit Azure Digital Twins verknüpfen:

```azurecli
az dt endpoint create eventgrid --endpoint-name <Event-Grid-endpoint-name> --eventgrid-resource-group <Event-Grid-resource-group-name> --eventgrid-topic <your-Event-Grid-topic-name> -n <your-Azure-Digital-Twins-instance-name>
```

Dadurch wird das Event Grid-Thema innerhalb von Azure Digital Twins unter dem mit dem Argument `--endpoint-name` angegebenen Namen verfügbar. In der Regel werden Sie diesen Namen als Ziel einer **Ereignisroute** verwenden, die Sie im nächsten Abschnitt mithilfe der Azure Digital Twins-Dienst-API erstellen werden.

Entsprechende Befehle gibt es für Event Hub- und Service Bus-Endpunkte:

* Hinzufügen eines Service Bus-Themaendpunkts (erfordert eine vorab erstellte Service Bus-Ressource)
```azurecli 
az dt endpoint create servicebus --endpoint-name <Service-Bus-endpoint-name> --servicebus-resource-group <Service-Bus-resource-group-name> --servicebus-namespace <Service-Bus-namespace> --servicebus-topic <Service-Bus-topic-name> --servicebus-policy <Service-Bus-topic-policy> -n <your-Azure-Digital-Twins-instance-name>
```

* Hinzufügen eines Event Hub-Endpunkts (erfordert eine vorab erstellte Event Hub-Ressource)
```azurecli
az dt endpoint create eventhub --endpoint-name <Event-Hub-endpoint-name> --eventhub-resource-group <Event-Hub-resource-group> --eventhub-namespace <Event-Hub-namespace> --eventhub <Event-Hub-name> --eventhub-policy <Event-Hub-policy> -n <your-Azure-Digital-Twins-instance-name>
```

## <a name="manage-event-routes-with-apis"></a>Verwalten von Ereignisrouten mit APIs

Wenn Sie Daten von Azure Digital Twins tatsächlich an einen Endpunkt senden möchten, müssen Sie eine Ereignisroute definieren. Die **EventRoutes-APIs** von Azure Digital Twins ermöglichen es Entwicklern, den Ereignisfluss im gesamten System und zu Downstreamdiensten einzurichten. Weitere Informationen zu Ereignisrouten finden Sie unter [Konzepte: Weiterleiten von Azure Digital Twins-Ereignissen](concepts-route-events.md).

Die Beispiele in diesem Artikel verwenden das C#-SDK.

Ereignisrouten werden mithilfe von Datenebenen-APIs definiert. Eine Routendefinition kann folgende Elemente enthalten:
* Die Routen-ID, die Sie verwenden möchten.
* Den Namen des Endpunkts, den Sie verwenden möchten.
* Ein Filter, der definiert, welche Ereignisse an den Endpunkt gesendet werden. 

Wenn es keine Routen-ID gibt, werden keine Nachrichten außerhalb von Azure Digital Twins weitergeleitet. Wenn es eine Routen-ID gibt und der Filter `true` ergibt, werden alle Nachrichten an den Endpunkt weitergeleitet. Wenn es eine Routen-ID gibt und ein anderer Filter hinzugefügt wird, werden Nachrichten auf der Grundlage des Filters gefiltert.

Eine Route sollte es ermöglichen, mehrere Benachrichtigungen und Ereignistypen auszuwählen. 

`CreateEventRoute` ist der SDK-Aufruf, der zum Hinzufügen einer Ereignisroute verwendet wird. Hier ist ein Beispiel für seine Verwendung:

```csharp
EventRoute er = new EventRoute("endpointName");
er.Filter("true"); //Filter allows all messages
await client.CreateEventRoute("routeName", er);
```

> [!TIP]
> Alle SDK-Funktionen sind in synchronen und asynchronen Versionen enthalten.

### <a name="event-route-sample-code"></a>Beispielcode für Ereignisroute

Das folgende Codebeispiel zeigt, wie Sie eine Ereignisroute erstellen, auflisten und löschen können:
```csharp
try
{
    Console.WriteLine("Create a route: testRoute1");
    EventRoute er = new EventRoute("< your - endpoint - name >");
    // Make a filter that passes everything
    er.Filter = "true";
    client.CreateEventRoute("< your - route - name >", er);
    Console.WriteLine("Create route succeeded. Now listing routes:");
    Pageable <EventRoute> result = client.GetEventRoutes();
    foreach (EventRoute r in result)
    {
        Console.WriteLine($"Route {r.Id} to endpoint {r.EndpointId} with filter {r.Filter} ");
    }
    Console.WriteLine("Deleting routes:");
    foreach (EventRoute r in result)
    {
        Console.WriteLine($"Deleting route {r.Id}:");
        client.DeleteEventRoute(r.Id);
    }
}
catch (RequestFailedException e)
{
    Console.WriteLine($"*** Error in event route processing ({e.ErrorCode}):\n${e.Message}");
}
```

### <a name="filter-events"></a>Filtern von Ereignissen

Ohne Filterung erhalten die Endpunkte eine Vielzahl von Ereignissen von Azure Digital Twins:
* Telemetrie, die von [digitalen Zwillingen](concepts-twins-graph.md) unter Verwendung der Azure Digital Twins-Dienst-API ausgelöst wird.
* Änderungsbenachrichtigungen für Zwillingseigenschaften, ausgelöst bei Eigenschaftsänderungen für einen Zwilling in der Azure Digital Twins-Instanz.
* Lebenszyklusereignisse, die ausgelöst werden, wenn Zwillinge oder Beziehungen erstellt oder gelöscht werden.
* Modelländerungsereignisse, die ausgelöst werden, wenn [Modelle](concepts-models.md), die in einer Instanz von Azure Digital Twins konfiguriert sind, hinzugefügt oder gelöscht werden.

Sie können die zu sendenden Ereignisse durch Hinzufügen eines Filters zu einem Endpunkt einschränken.

Um einen Filter hinzuzufügen, können Sie eine PUT-Anforderung für *https://{YourHost}/EventRoutes/myNewRoute?api-version=2020-05-31-preview* mit folgendem Hauptteil verwenden:

```json  
{
    "endpointName": "<endpoint-name>",
    "filter": "<filter-text>"
}
``` 

Dies sind die unterstützten Routenfilter.

| Filtername | BESCHREIBUNG | Filterschema | Unterstützte Werte | 
| --- | --- | --- | --- |
| type | Der [Ereignistyp](./concepts-route-events.md#types-of-event-messages), der durch Ihre digitale Zwillingsinstanz fließt. | `"filter" : "type = '<eventType>'"` | `Microsoft.DigitalTwins.Twin.Create` <br> `Microsoft.DigitalTwins.Twin.Delete` <br> `Microsoft.DigitalTwins.Twin.Update`<br>`Microsoft.DigitalTwins.Relationship.Create`<br>`Microsoft.DigitalTwins.Relationship.Update`<br> `Microsoft.DigitalTwins.Relationship.Delete` <br> `microsoft.iot.telemetry`  |
| `Source` | Der Name der Azure Digital Twins-Instanz. | `"filter" : "source = '<hostname>'"`|  **Für Benachrichtigungen**: `<yourDigitalTwinInstance>.<yourRegion>.azuredigitaltwins.net` <br> **Für Telemetrie**: `<yourDigitalTwinInstance>.<yourRegion>.azuredigitaltwins.net/digitaltwins/<twinId>`|
| Subject | Eine Beschreibung des Ereignisses im Zusammenhang mit der oben genannten Ereignisquelle. | `"filter": " subject = '<subject>'"` | **Für Benachrichtigungen**: Der Betreff ist `<twinid>` <br> oder ein URI-Format für Betreffelemente, die durch mehrere Teile oder IDs eindeutig identifiziert werden:<br>`<twinid>/relationships/<relationshipid>`<br> **Für Telemetrie**: Der Betreff ist der Komponentenpfad (wenn die Telemetrie von einer Zwillingskomponente ausgegeben wird). Beispiel: `comp1.comp2`. Wenn die Telemetrie nicht von einer Komponente ausgegeben wird, dann ist ihr Betrefffeld leer. |
| Datenschema | DTDL-Modell-ID | `"filter": "dataschema = 'dtmi:example:com:floor4;2'"` | **Für Telemetrie**: Das Datenschema ist die Modell-ID des Zwillings oder der Komponente, die die Telemetrie ausgibt. <br>**Für Benachrichtigungen**: Das Datenschema wird nicht unterstützt.|
| Inhaltstyp | Der Inhaltstyp des Datenwerts. | `"filter": "datacontenttype = '<contentType>'"` | `application/json` |
| Spezifikationsversion | Die Version des von Ihnen verwendeten Ereignisschemas. | `"filter": "specversion = '<version>'"` | Muss `1.0`lauten. Dies gibt das CloudEvents-Schema, Version 1.0, an. |
| True/False | Ermöglicht das Erstellen einer Route ohne Filterung oder das Deaktivieren einer Route. | `"filter" : "<true/false>"` | `true` = Route ist ohne Filterung aktiviert. <br> `false` = Route ist deaktiviert. |
<!--
| ID | *Not implemented for public preview* | "filter": "id = '…'" | |
| Schema | *Not implemented for public preview*  | "filter": "dataschema = '…'" | |
-->

Es ist auch möglich, Filter mit den folgenden Vorgängen zu kombinieren:

| Filtername | Filterschema | Beispiel | 
| --- | --- | --- |
| AND/OR | `"filter": "<filter1> AND <filter2>"` | `"filter": "type != 'microsoft.iot.telemetry' AND datacontenttype = 'application/json'"` |
| AND/OR | `"filter": "<filter1> OR <filter2>"` | `"filter": "type != 'microsoft.iot.telemetry' OR datacontenttype = 'application/json'"` |
| Geschachtelte Vorgänge | `"filter": "(<Comparison1>) AND (<Comparison2>)"` | `"filter": "(type != 'microsoft.iot.telemetry' OR datacontenttype = 'application/json') OR (specversion != '1.0')"` |

> [!NOTE]
> Während der Vorschau wird nur die Zeichenfolgengleichheit unterstützt (=, !=).

Wenn Sie einen Filter implementieren oder aktualisieren, kann es einige Minuten dauern, bis sich die Änderung in der Datenpipeline widerspiegelt.

## <a name="manage-endpoints-and-routes-with-cli"></a>Verwalten von Endpunkten und Routen mit der Befehlszeilenschnittstelle (CLI)

Endpunkte und Routen können auch über die Azure Digital Twins-Befehlszeilenschnittstelle (CLI) verwaltet werden. Die Befehle finden Sie in [Gewusst wie: Verwenden der Azure Digital Twins-Befehlszeilenschnittstelle](how-to-use-cli.md).

## <a name="monitor-event-routes"></a>Überwachen von Ereignisrouten

Routingmetriken wie Anzahl, Wartezeit und Ausfallrate können im [Azure-Portal](https://portal.azure.com/) angezeigt werden. 

Suchen Sie auf der Startseite des Portals nach Ihrer Azure Digital Twins-Instanz, um die Details abzurufen. Wählen Sie die Option **Metrics** aus dem Menü der Azure Digital Twins-Instanz aus, um die Seite *Metriken* aufzurufen.

:::image type="content" source="media/how-to-manage-routes/metrics.png" alt-text="Seite „Metriken“ einer Azure Digital Twins-Instanz im Azure-Portal":::

Von hier aus können Sie die Metriken für Ihre Instanz anzeigen und benutzerdefinierte Ansichten erstellen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über die verschiedenen Arten von Ereignismeldungen, die Sie erhalten können:
* [Gewusst wie: Interpretieren von Ereignisdaten](how-to-interpret-event-data.md)
