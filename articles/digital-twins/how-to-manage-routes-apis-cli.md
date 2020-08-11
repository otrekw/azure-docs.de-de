---
title: Verwalten von Endpunkten und Routen (APIs und CLI)
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie Endpunkte und Ereignisrouten für Azure Digital Twins-Daten einrichten und verwalten.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 6/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 9274e147bcaec4b3e63a6720e369946d64e94628
ms.sourcegitcommit: 85eb6e79599a78573db2082fe6f3beee497ad316
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2020
ms.locfileid: "87809883"
---
# <a name="manage-endpoints-and-routes-in-azure-digital-twins-apis-and-cli"></a>Verwalten von Endpunkten und Routen in Azure Digital Twins (APIs und CLI)

[!INCLUDE [digital-twins-route-selector.md](../../includes/digital-twins-route-selector.md)]

In Azure Digital Twins können Sie [Ereignisbenachrichtigungen](how-to-interpret-event-data.md) an Downstreamdienste oder verbundene Computeressourcen weiterleiten. Dies erfolgt, indem zunächst **Endpunkte** eingerichtet werden, die die Ereignisse empfangen können. Anschließend können Sie [**Ereignisrouten**](concepts-route-events.md) erstellen, die angeben, welche von Azure Digital Twins generierten Ereignisse an welche Endpunkte übermittelt werden.

Endpunkte und Routen werden mit den [EventRoutes-APIs](how-to-use-apis-sdks.md), dem [.NET (C#) SDK](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core) oder der [Azure Digital Twins CLI](how-to-use-cli.md) verwaltet. Dieser Artikel führt Sie durch den Vorgang zum Erstellen von Endpunkten und Routen über diese Mechanismen.

Sie können auch über das [Azure-Portal](https://portal.azure.com) verwaltet werden. Eine Version dieses Artikels, die stattdessen das Portal verwendet, finden Sie unter [*Vorgehensweise: Verwalten von Endpunkten und Routen (Portal)* ](how-to-manage-routes-portal.md).

## <a name="prerequisites"></a>Voraussetzungen

* Sie benötigen ein **Azure-Konto** (Sie können [hier](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) ein kostenloses Konto einrichten).
* Sie benötigen eine **Azure Digital Twins-Instanz** in Ihrem Azure-Abonnement. Falls Sie noch keine Instanz besitzen, können Sie zum Erstellen die Schritte unter [*Vorgehensweise: Einrichten einer Instanz und von Authentifizierung*](how-to-set-up-instance-scripted.md) verwenden. Notieren Sie sich die folgenden Werte aus dem Setup, um sie später in diesem Artikel zu verwenden:
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

Das folgende Beispiel zeigt, wie ein Endpunkt vom Event Grid-Typ mit der Azure CLI erstellt wird. Sie können [Azure Cloud Shell](https://shell.azure.com) verwenden oder die [CLI lokal installieren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

Erstellen Sie zunächst ein Event Grid-Thema. Sie können den folgenden Befehl verwenden oder die Schritte ausführlicher anzeigen, indem Sie [den Abschnitt *Erstellen eines benutzerdefinierten Themas*](../event-grid/custom-event-quickstart-portal.md#create-a-custom-topic) des Event Grid-Schnellstarts *Benutzerdefinierte Ereignisse* verwenden.

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

Nun ist das Event Grid-Thema als Endpunkt innerhalb von Azure Digital Twins unter dem mit dem Argument `--endpoint-name` angegebenen Namen verfügbar. In der Regel werden Sie diesen Namen als Ziel einer **Ereignisroute** verwenden, die Sie [später in diesem Artikel](#event-routes-with-apis-and-the-c-sdk) mithilfe der Azure Digital Twins-Dienst-API erstellen.

### <a name="create-an-event-hubs-or-service-bus-endpoint"></a>Erstellen eines Event Hubs- oder Service Bus-Endpunkts

Der Vorgang zum Erstellen von Event Hubs- oder Service Bus-Endpunkten ähnelt dem oben gezeigten Event Grid-Vorgang.

Erstellen Sie zunächst die Ressourcen, die Sie als Endpunkt verwenden möchten. Folgendes ist erforderlich:
* Service Bus: _Service Bus-Namespace_, _Service Bus-Thema_, _Autorisierungsregel_
* Event Hubs: _Event Hubs-Namespace_, _Event Hub_, _Autorisierungsregel_

Verwenden Sie dann die folgenden Befehle, um die Endpunkte in Azure Digital Twins zu erstellen: 

* Hinzufügen eines Service Bus-Themaendpunkts (erfordert eine vorab erstellte Service Bus-Ressource)
```azurecli 
az dt endpoint create servicebus --endpoint-name <Service-Bus-endpoint-name> --servicebus-resource-group <Service-Bus-resource-group-name> --servicebus-namespace <Service-Bus-namespace> --servicebus-topic <Service-Bus-topic-name> --servicebus-policy <Service-Bus-topic-policy> -n <your-Azure-Digital-Twins-instance-name>
```

* Hinzufügen eines Event Hubs-Endpunkts (erfordert eine vorab erstellte Event Hubs-Ressource)
```azurecli
az dt endpoint create eventhub --endpoint-name <Event-Hub-endpoint-name> --eventhub-resource-group <Event-Hub-resource-group> --eventhub-namespace <Event-Hub-namespace> --eventhub <Event-Hub-name> --eventhub-policy <Event-Hub-policy> -n <your-Azure-Digital-Twins-instance-name>
```

## <a name="event-routes-with-apis-and-the-c-sdk"></a>Ereignisrouten (mit APIs und dem C# SDK)

Wenn Sie tatsächlich Daten von Azure Digital Twins an einen Endpunkt senden möchten, müssen Sie eine **Ereignisroute** definieren. Die **EventRoutes-APIs** von Azure Digital Twins ermöglichen es Entwicklern, den Ereignisfluss im gesamten System und zu Downstreamdiensten einzurichten. Weitere Informationen zu Ereignisrouten finden Sie unter [*Konzepte: Weiterleiten von Azure Digital Twins-Ereignissen*](concepts-route-events.md).

Die Beispiele in diesem Abschnitt verwenden das C# SDK.

**Voraussetzung**: Sie müssen wie weiter oben in diesem Artikel beschrieben Endpunkte erstellen, bevor Sie mit dem Erstellen einer Route fortfahren können. Nachdem die Einrichtung Ihrer Endpunkte abgeschlossen ist, können Sie mit dem Erstellen einer Ereignisroute fortfahren.

>[!NOTE]
>Wenn Sie Ihre Endpunkte gerade erst bereitgestellt haben, vergewissern Sie sich, dass die Bereitstellung abgeschlossen ist, **bevor** Sie versuchen, die Endpunkte für eine neue Ereignisroute zu verwenden. Wenn die Routenbereitstellung fehlschlägt, weil die Endpunkte noch nicht bereit sind, warten Sie einige Minuten, und versuchen Sie es dann erneut.
>
> Wenn Sie eine Skripterstellung für diesen Flow durchführen, sollten Sie dies berücksichtigen, indem Sie eine Wartezeit von zwei bis drei Minuten vorsehen, damit der Endpunktdienst die Bereitstellung abschließen kann, bevor mit der Routeneinrichtung fortgefahren wird.

### <a name="create-an-event-route"></a>Erstellen einer Ereignisroute

Ereignisrouten werden mithilfe von Datenebenen-APIs definiert. 

Eine Routendefinition kann folgende Elemente enthalten:
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

Sie können die gesendeten Ereignisse einschränken, indem Sie der Ereignisroute einen **Filter** für einen Endpunkt hinzufügen.

Um einen Filter hinzuzufügen, können Sie eine PUT-Anforderung für *https://{YourHost}/EventRoutes/myNewRoute?api-version=2020-05-31-preview* mit folgendem Hauptteil verwenden:

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
