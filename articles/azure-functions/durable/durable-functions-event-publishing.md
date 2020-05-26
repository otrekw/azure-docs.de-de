---
title: Veröffentlichungen von Durable Functions in Azure Event Grid (Vorschau)
description: Erfahren Sie, wie Sie eine automatische Veröffentlichung in Azure Event Grid für Durable Functions konfigurieren.
ms.topic: conceptual
ms.date: 04/25/2020
ms.openlocfilehash: c0106f3754e0cdcbf1f295fbe3f1b5def8dc3ca1
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83124251"
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

Erstellen Sie ein Event Grid-Thema zum Senden von Ereignissen aus Durable Functions. In den folgenden Anweisungen wird gezeigt, wie Sie ein Thema mithilfe der Azure-Befehlszeilenschnittstelle (CLI) erstellen. Sie können dieses Thema auch erstellen, indem Sie [PowerShell verwenden](../../event-grid/custom-event-quickstart-powershell.md) oder [mithilfe des Azure-Portals](../../event-grid/custom-event-quickstart-portal.md).

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

### <a name="durable-functions-1x"></a>Durable Functions 1.x

Fügen Sie `eventGridTopicEndpoint` und `eventGridKeySettingName` einer `durableTask`-Eigenschaft hinzu.

```json
{
  "durableTask": {
    "eventGridTopicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
    "eventGridKeySettingName": "EventGridKey"
  }
}
```

### <a name="durable-functions-2x"></a>Durable Functions 2.x

Fügen Sie den Abschnitt `notifications` zur Eigenschaft `durableTask` der Datei hinzu, und ersetzen Sie `<topic_name>` durch den von Ihnen gewählten Namen. Wenn die Eigenschaften `durableTask` oder `extensions` nicht vorhanden sind, erstellen Sie sie wie in diesem Beispiel:

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "notifications": {
        "eventGrid": {
          "topicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
          "keySettingName": "EventGridKey"
        }
      }
    }
  }
}
```

Die möglichen Konfigurationseigenschaften von Azure Event Grid finden Sie in der [host.json-Dokumentation](../functions-host-json.md#durabletask). Nach dem Konfigurieren der Datei `host.json` sendet die Funktions-App Lebenszyklusereignisse an das Event Grid-Thema. Diese Aktion startet, wenn Sie die Funktions-App sowohl lokal als auch in Azure ausführen.

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

1. Wählen Sie in Ihrer Funktions-App **Funktionen** und dann **+ Hinzufügen** aus. 

   :::image type="content" source="./media/durable-functions-event-publishing/function-add-function.png" alt-text="Hinzufügen einer Funktion im Azure-Portal." border="true":::

1. Suchen Sie nach **Event Grid**, und wählen Sie dann die **Azure Event Grid-Trigger**vorlage aus. 

    :::image type="content" source="./media/durable-functions-event-publishing/function-select-event-grid-trigger.png" alt-text="Auswählen der Event Grid-Triggervorlage im Azure-Portal." border="true":::

1. Benennen Sie den neuen Trigger, und wählen Sie dann **Funktion erstellen** aus.

    :::image type="content" source="./media/durable-functions-event-publishing/function-name-event-grid-trigger.png" alt-text="Benennen des Event Grid-Triggers im Azure-Portal." border="true":::


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

### <a name="add-an-event-grid-subscription"></a>Hinzufügen eines Event Grid-Abonnements

Sie können jetzt dem gerade erstellten Event Grid-Thema ein Event Grid-Abonnement hinzufügen. Weitere Informationen finden Sie unter [Begriffe in Azure Event Grid](https://docs.microsoft.com/azure/event-grid/concepts).

1. Wählen Sie in Ihrer neuen Funktion **Integration** und dann **Event Grid-Trigger (eventGridEvent)** aus. 

    :::image type="content" source="./media/durable-functions-event-publishing/eventgrid-trigger-link.png" alt-text="Auswählen des Event Grid-Triggerlinks." border="true":::

1. Wählen Sie **Event Grid-Beschreibung erstellen** aus.

    :::image type="content" source="./media/durable-functions-event-publishing/create-event-grid-subscription.png" alt-text="Erstellen des Event Grid-Abonnements." border="true":::

1. Benennen Sie Ihr Ereignisabonnement, und wählen Sie den Thementyp **Event Grid-Themen** aus. 

1. Wählen Sie das Abonnement aus. Wählen Sie dann die Ressourcengruppe und Ressource aus, die Sie für das Event Grid-Thema erstellt haben. 

1. Klicken Sie auf **Erstellen**.

    :::image type="content" source="./media/durable-functions-event-publishing/event-grid-subscription-details.png" alt-text="Erstellen eines Event Grid-Abonnements." border="true":::

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
