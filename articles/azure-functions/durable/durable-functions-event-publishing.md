---
title: Veröffentlichungen von Durable Functions in Azure Event Grid (Vorschau)
description: Erfahren Sie, wie Sie eine automatische Veröffentlichung in Azure Event Grid für Durable Functions konfigurieren.
ms.topic: conceptual
ms.date: 03/14/2019
ms.openlocfilehash: 52ffcd4eb81936ffcfa61580288c60bd59ffb744
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78249761"
---
# <a name="durable-functions-publishing-to-azure-event-grid-preview"></a>Veröffentlichungen von Durable Functions in Azure Event Grid (Vorschau)

In diesem Artikel wird gezeigt, wie Sie Durable Functions so einrichten, dass Lebenszyklusereignisse einer Orchestrierung (z.B. Erstellen, Abschließen und Fehlschlagen) in einem benutzerdefinierten [Azure Event Grid-Thema](https://docs.microsoft.com/azure/event-grid/overview) veröffentlicht werden.

Es folgen einige Szenarien, in denen diese Funktion hilfreich ist:

* **DevOps-Szenarien wie Blau/Grün-Bereitstellungen**: Vor der Implementierung einer [parallelen Bereitstellungsstrategie](durable-functions-versioning.md#side-by-side-deployments) möchten Sie möglicherweise wissen, ob irgendeine Aufgabe ausgeführt wird.

* **Erweiterte Unterstützung für Überwachung und Diagnose:** Sie können die Orchestrierungsstatusinformationen in einem externen, für Abfragen optimierten Speicher wie Azure SQL-Datenbank oder Azure Cosmos DB verfolgen.

* **Lang andauernde Hintergrundaktivität:** Bei Verwendung von Durable Functions für eine lang andauernde Hintergrundaktivität erleichtert diese Funktion Ihnen die Ermittlung des aktuellen Status.

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) in Ihrem Durable Functions-Projekt.
* Installieren Sie [Azure Storage Emulator](../../storage/common/storage-use-emulator.md) (nur Windows), oder verwenden Sie ein vorhandenes Azure Storage-Konto.
* Installieren Sie [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), oder verwenden Sie die [Azure Cloud Shell](../../cloud-shell/overview.md).

## <a name="create-a-custom-event-grid-topic"></a>Erstellen eines benutzerdefinierten Event Grid-Themas

Erstellen Sie ein Event Grid-Thema zum Senden von Ereignissen aus Durable Functions. In den folgenden Anweisungen wird gezeigt, wie Sie ein Thema mithilfe der Azure-Befehlszeilenschnittstelle (CLI) erstellen. Dies kann [mithilfe von PowerShell](../../event-grid/custom-event-quickstart-powershell.md) oder [über das Azure-Portal](../../event-grid/custom-event-quickstart-portal.md) erfolgen.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl `az group create` eine Ressourcengruppe. Derzeit unterstützt Azure Event Grid nicht alle Regionen. Informationen zu den unterstützten Regionen finden Sie unter [Azure Event Grid – Übersicht](../../event-grid/overview.md).

```azurecli
az group create --name eventResourceGroup --location westus2
```

### <a name="create-a-custom-topic"></a>Erstellen eines benutzerdefinierten Themas

Ein Event Grid-Thema stellt einen benutzerdefinierten Endpunkt für die Veröffentlichung Ihrer Ereignisse bereit. Ersetzen Sie `<topic_name>` durch einen eindeutigen Namen für Ihr Thema. Der Name des Themas muss eindeutig sein, da er als DNS-Eintrag verwendet wird.

```azurecli
az eventgrid topic create --name <topic_name> -l westus2 -g eventResourceGroup
```

## <a name="get-the-endpoint-and-key"></a>Abrufen von Endpunkt und Schlüssel

Rufen Sie den Endpunkt des Themas ab. Ersetzen Sie `<topic_name>` durch den von Ihnen ausgewählten Namen.

```azurecli
az eventgrid topic show --name <topic_name> -g eventResourceGroup --query "endpoint" --output tsv
```

Rufen Sie den Schlüssel für das Thema ab. Ersetzen Sie `<topic_name>` durch den von Ihnen ausgewählten Namen.

```azurecli
az eventgrid topic key list --name <topic_name> -g eventResourceGroup --query "key1" --output tsv
```

Jetzt können Sie Ereignisse an das Thema senden.

## <a name="configure-event-grid-publishing"></a>Konfigurieren der Veröffentlichung in Event Grid

Suchen Sie in Ihrem Durable Functions-Projekt nach der Datei `host.json`.

Fügen Sie `eventGridTopicEndpoint` und `eventGridKeySettingName` einer `durableTask`-Eigenschaft hinzu.

```json
{
    "durableTask": {
        "eventGridTopicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
        "eventGridKeySettingName": "EventGridKey"
    }
}
```

Die möglichen Konfigurationseigenschaften von Azure Event Grid finden Sie in der [host.json-Dokumentation](../functions-host-json.md#durabletask). Nach dem Konfigurieren der Datei `host.json` sendet die Funktions-App Lebenszyklusereignisse an das Event Grid-Thema. Dies funktioniert, wenn Sie die Funktions-App lokal und in Azure ausführen.

Legen Sie die App-Einstellung für den Schlüssel des Themas in der Funktions-App und in der Datei `local.settings.json` fest. Die folgende JSON-Datei dient als Beispiel für die Datei `local.settings.json` für lokales Debuggen. Ersetzen Sie `<topic_key>` durch den Schlüssel für das Thema.  

```json
{
    "IsEncrypted": false,
    "Values": {
        "AzureWebJobsStorage": "UseDevelopmentStorage=true",
        "AzureWebJobsDashboard": "UseDevelopmentStorage=true",
        "EventGridKey": "<topic_key>"
    }
}
```

Wenn Sie den [Speicheremulator](../../storage/common/storage-use-emulator.md) (nur Windows) verwenden, vergewissern Sie sich, dass er funktioniert. Es ist ratsam, vor der Ausführung den Befehl `AzureStorageEmulator.exe clear all` auszuführen.

Wenn Sie ein vorhandenes Azure Storage-Konto verwenden, ersetzen Sie `UseDevelopmentStorage=true` in `local.settings.json` durch die zugehörige Verbindungszeichenfolge.

## <a name="create-functions-that-listen-for-events"></a>Erstellen von Funktionen, die auf Ereignisse warten

Erstellen Sie mithilfe des Azure-Portals eine weitere Funktions-App zum Lauschen auf Ereignisse, die von Ihrer Durable Functions-App veröffentlicht werden. Es wird empfohlen, sie in derselben Region wie das Event Grid-Thema zu platzieren.

### <a name="create-an-event-grid-trigger-function"></a>Erstellen einer Event Grid-Triggerfunktion

Erstellen Sie eine Funktion zum Empfangen der Lebenszyklusereignisse. Wählen Sie **Benutzerdefinierte Funktion** aus.

![Wählen Sie „Erstellen einer benutzerdefinierten Funktion“ aus.](./media/durable-functions-event-publishing/functions-portal.png)

Wählen Sie „Event Grid-Trigger“ und dann eine Sprache aus.

![Wählen Sie den Event Grid-Trigger aus.](./media/durable-functions-event-publishing/eventgrid-trigger.png)

Geben Sie den Namen der Funktion ein, und wählen Sie dann `Create` aus.

![Erstellen Sie den Event Grid-Trigger.](./media/durable-functions-event-publishing/eventgrid-trigger-creation.png)

Es wird eine Funktion mit folgendem Code erstellt:

# <a name="c-script"></a>[C#-Skript](#tab/csharp-script)

```csharp
#r "Newtonsoft.Json"
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Microsoft.Extensions.Logging;

public static void Run(JObject eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.ToString(Formatting.Indented));
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
module.exports = async function(context, eventGridEvent) {
    context.log(typeof eventGridEvent);
    context.log(eventGridEvent);
}
```

---

Wählen Sie `Add Event Grid Subscription`aus. Durch diesen Vorgang wird dem erstellten Event Grid-Thema ein Event Grid-Abonnement hinzugefügt. Weitere Informationen finden Sie unter [Begriffe in Azure Event Grid](https://docs.microsoft.com/azure/event-grid/concepts).

![Wählen Sie den Link zum Event Grid-Trigger aus.](./media/durable-functions-event-publishing/eventgrid-trigger-link.png)

Wählen Sie `Event Grid Topics` als **Thementyp** aus. Wählen Sie die Ressourcengruppe aus, die Sie für das Event Grid-Thema erstellt haben. Wählen Sie dann die Instanz des Event Grid-Themas aus. Drücken Sie `Create`.

![Erstellen Sie ein Event Grid-Abonnement.](./media/durable-functions-event-publishing/eventsubscription.png)

Sie können nun Lebenszyklusereignisse empfangen.

## <a name="run-durable-functions-app-to-send-the-events"></a>Ausführen der Durable Functions-App zum Senden der Ereignisse

Starten Sie in dem zuvor konfigurierten Durable Functions-Projekt das Debuggen auf Ihrem lokalen Computer, und starten Sie eine Orchestrierung. Die App veröffentlicht Durable Functions-Lebenszyklusereignisse in Event Grid. Stellen Sie sicher, dass Event Grid die von Ihnen erstellte Listenerfunktion auslöst, indem Sie ihre Protokolle im Azure-Portal überprüfen.

```
2019-04-20T09:28:21.041 [Info] Function started (Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d)
2019-04-20T09:28:21.104 [Info] {
    "id": "054fe385-c017-4ce3-b38a-052ac970c39d",
    "subject": "durable/orchestrator/Running",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Running"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2019-04-20T09:28:19.6492068Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}

2019-04-20T09:28:21.104 [Info] Function completed (Success, Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d, Duration=65ms)
2019-04-20T09:28:37.098 [Info] Function started (Id=36fadea5-198b-4345-bb8e-2837febb89a2)
2019-04-20T09:28:37.098 [Info] {
    "id": "8cf17246-fa9c-4dad-b32a-5a868104f17b",
    "subject": "durable/orchestrator/Completed",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Completed"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2019-04-20T09:28:36.5061317Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}
2019-04-20T09:28:37.098 [Info] Function completed (Success, Id=36fadea5-198b-4345-bb8e-2837febb89a2, Duration=0ms)
```

## <a name="event-schema"></a>Ereignisschema

In der folgenden Liste wird das Schema für Lebenszyklusereignisse erläutert:

* **`id`** : Eindeutiger Bezeichner für das Event Grid-Ereignis.
* **`subject`** : Pfad zum Ereignisbetreff. `durable/orchestrator/{orchestrationRuntimeStatus}` `{orchestrationRuntimeStatus}` kann `Running`, `Completed`, `Failed` und `Terminated` sein.  
* **`data`** : Spezielle Durable Functions-Parameter.
  * **`hubName`** : Name des [TaskHub](durable-functions-task-hubs.md).
  * **`functionName`** : Name der Orchestratorfunktion.
  * **`instanceId`** : ID der Durable Functions-Instanz.
  * **`reason`** : Zusätzliche Daten zum Nachverfolgungsereignis. Weitere Informationen finden Sie unter [Diagnose in Durable Functions (Azure Functions)](durable-functions-diagnostics.md).
  * **`runtimeStatus`** : Laufzeitstatus der Orchestrierung. „Running“, „Completed“, „Failed“, „Canceled“.
* **`eventType`** : "orchestratorEvent"
* **`eventTime`** : Ereigniszeit (UTC).
* **`dataVersion`** : Version des Lebenszyklus-Ereignisschemas.
* **`metadataVersion`** :  Version der Metadaten.
* **`topic`** : Ressource des Event Grid-Themas.

## <a name="how-to-test-locally"></a>Lokales Testen

Informationen zum lokalen Testen finden Sie unter [Lokales Debuggen von Azure-Funktionen mit Event Grid-Trigger](../functions-debug-event-grid-trigger-local.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zur Instanzverwaltung in Durable Functions](durable-functions-instance-management.md)

> [!div class="nextstepaction"]
> [Informationen zur Versionsverwaltung in Durable Functions](durable-functions-versioning.md)
