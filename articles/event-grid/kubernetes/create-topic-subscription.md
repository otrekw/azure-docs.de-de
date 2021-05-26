---
title: Azure Event Grid auf Kubernetes – Webhook als Ereignishandler
description: Dieser Artikel beschreibt, wie Sie ein Event Grid-Thema auf einem Kubernetes-Cluster erstellen, das mit Azure Arc verbunden ist, und dann ein Abonnement für das Thema erstellen.
author: jfggdl
ms.subservice: kubernetes
ms.author: jafernan
ms.date: 05/25/2021
ms.topic: quickstart
ms.openlocfilehash: c0e2a4422cea681a3bccee0739b8c26350803eb8
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385657"
---
# <a name="route-cloud-events-to-webhooks-with-azure-event-grid-on-kubernetes"></a>Weiterleiten von Cloudereignissen an Webhooks mit Azure Event Grid auf Kubernetes
In diesem Schnellstart erstellen Sie ein Thema im Event Grid auf Kubernetes, ein Abonnement für das Thema und senden dann ein Beispielereignis an das Thema, um das Szenario zu testen. 

[!INCLUDE [event-grid-preview-feature-note.md](../../../includes/event-grid-preview-feature-note.md)]


## <a name="prerequisites"></a>Voraussetzungen

1. [Verbinden Sie Ihren Kubernetes-Cluster mit Azure Arc](../../azure-arc/kubernetes/quickstart-connect-cluster.md).
1. [Installieren der Azure Event-Grid-Erweiterung auf dem Kubernetes-Cluster](install-k8s-extension.md). Diese Erweiterung stellt Event Grid in einem Kubernetes-Cluster bereit. 
1. [Erstellen eines benutzerdefinierten Speicherorts](../../azure-arc/kubernetes/custom-locations.md). Ein benutzerdefinierter Speicherort stellt einen Namespace im Cluster dar und ist die Stelle, an der Themen und Ereignisabonnements bereitgestellt werden.

## <a name="create-a-topic"></a>Erstellen eines Themas

### <a name="azure-cli"></a>Azure CLI
Führen Sie den folgenden Azure CLI-Befehl aus, um ein Thema zu erstellen:

```azurecli-interactive
az eventgrid topic create --name <EVENT GRID TOPIC NAME> \
                        --resource-group <RESOURCE GROUP NAME> \
                        --location <REGION> \
                        --kind azurearc \
                        --extended-location-name /subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.ExtendedLocation/customLocations/<CUSTOM LOCATION NAME> \
                        --extended-location-type customlocation \
                        --input-schema CloudEventSchemaV1_0
```
Geben Sie Werte für die Platzhalter an, bevor Sie den Befehl ausführen:
- Name der Azure-Ressourcengruppe, in der das Event Grid-Thema erstellt werden soll. 
- Namen des Themas. 
- Region des Themas.
- Geben Sie in der Ressourcen-ID des benutzerdefinierten Speicherorts die folgenden Werte an:
    - ID des Azure-Abonnements, in dem der benutzerdefinierte Speicherort vorhanden ist.
    - Der Name der Ressourcengruppe, die den benutzerdefinierten Speicherort enthält.
    - Namen des benutzerdefinierten Standorts

Weitere Informationen zum CL-Befehl finden Sie unter [`az eventgrid topic create`](/cli/azure/eventgrid/topic#az_eventgrid_topic_create).

## <a name="create-a-message-endpoint"></a>Erstellen eines Nachrichtenendpunkts
Erstellen Sie zunächst einen Endpunkt für die Ereignisnachricht, bevor Sie ein Abonnement für das benutzerdefinierte Thema erstellen. Der Endpunkt führt in der Regel Aktionen auf der Grundlage der Ereignisdaten aus. Der Einfachheit halber stellen Sie in dieser Schnellstartanleitung eine [vorgefertigte Web-App](https://github.com/Azure-Samples/azure-event-grid-viewer) bereit, die die Ereignisnachrichten anzeigt. Die bereitgestellte Lösung umfasst einen App Service-Plan, eine App Service-Web-App und Quellcode von GitHub.

1. Wählen Sie auf der Artikelseite **Deploy to Azure** (In Azure bereitstellen) aus, um die Lösung für Ihr Abonnement bereitzustellen. Geben Sie im Azure-Portal Werte für die Parameter an.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"  alt="Button to Deploy to Aquent." /></a>
1. Die Bereitstellung kann einige Minuten dauern. Nach erfolgreichem Abschluss der Bereitstellung können Sie Ihre Web-App anzeigen und sich vergewissern, dass sie ausgeführt wird. Navigieren Sie hierzu in einem Webbrowser zu `https://<your-site-name>.azurewebsites.net`.

    Wenn die Bereitstellung fehlschlägt, überprüfen Sie die Fehlermeldung. Möglicherweise ist der Name der Website bereits vergeben. Stellen Sie die Vorlage noch mal bereit, und wählen Sie einen anderen Namen für die Site aus. 
1. Die Website wird angezeigt, aber es wurden noch keine Ereignisse bereitgestellt.

   ![Anzeigen der neuen Website](../media/custom-event-quickstart-portal/view-site.png)

## <a name="create-a-subscription"></a>Erstellen eines Abonnements
Abonnenten können sich für Ereignisse registrieren, die in einem Thema veröffentlicht werden. Zum Empfang von Ereignissen müssen Sie ein Event Grid-Abonnement für das gewünschte Thema erstellen. Ein Event-Abonnement definiert das Ziel, an das diese Ereignisse gesendet werden. Informationen zu allen unterstützten Zielen oder Handlern finden Sie unter [Ereignishandler](event-handlers.md).


### <a name="azure-cli"></a>Azure CLI
Um ein Ereignisabonnement mit einem WebHook (HTTPS-Endpunkt) zu erstellen, führen Sie den folgenden Azure CLI-Befehl aus:

```azurecli-interactive
az eventgrid event-subscription create --name <EVENT SUBSCRIPTION NAME> \
                                    --source-resource-id /subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<TOPIC'S RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<TOPIC NAme> \
                                    --endpoint https://<SITE NAME>.azurewebsites.net/api/updates
```

Geben Sie Werte für die Platzhalter an, bevor Sie den Befehl ausführen:
- Name des Ereignisabonnements. 

- Geben Sie in der **Ressourcen-ID des Themas** die folgenden Werte an:
    - ID des Azure-Abonnements, in dem das Abonnement erstellt werden soll. 
    - Name der Ressourcengruppe, die das Thema enthält.
    - Name des Themas. 
- Geben Sie für den Endpunkt den Namen der Event Grid Viewer-Website an.
    
Weitere Informationen zum CL-Befehl finden Sie unter [`az eventgrid event-subscription create`](/cli/azure/eventgrid/event-subscription#az_eventgrid_event_subscription_create).


## <a name="send-events-to-the-topic"></a>Ereignisse an das Thema senden
1. Führen Sie den folgenden Befehl aus, um den **Endpunkt** für das Thema abzurufen: Aktualisieren Sie nach dem Kopieren und Einfügen des Befehls den Namen des Themas (**topic name**) und den Namen der Ressourcengruppe (**resource group name**), bevor Sie den Befehl ausführen. Sie veröffentlichen Beispielereignisse in diesem Themenendpunkt. 

    ```azurecli
    az eventgrid topic show --name <topic name> -g <resource group name> --query "endpoint" --output tsv
    ```
2. Führen Sie den folgenden Befehl aus, um den **Schlüssel** für das benutzerdefinierte Thema abzurufen: Aktualisieren Sie nach dem Kopieren und Einfügen des Befehls den Namen des Themas (**topic name**) und den Namen der Ressourcengruppe (**resource group name**), bevor Sie den Befehl ausführen. Das ist der Primärschlüssel des Themas. Um diesen Schlüssel aus dem Azure-Portal abzurufen, wechseln Sie zur Registerkarte **Zugriffsschlüssel** der Seite **Event Grid-Thema**. Um ein Ereignis in einem benutzerdefinierten Thema bereitstellen zu können, benötigen Sie den Zugriffsschlüssel. 

    ```azurecli
    az eventgrid topic key list --name <topic name> -g <resource group name> --query "key1" --output tsv
    ```
3. Erstellen Sie eine Datei namens **evt.json** mit dem folgendem Inhalt: 

    ```json
    [{
          "specVersion": "1.0",
          "type" : "orderCreated",
          "source": "myCompanyName/us/webCommerceChannel/myOnlineCommerceSiteBrandName",
          "id" : "eventId-n",
          "time" : "2020-12-25T20:54:07+00:00",
          "subject" : "account/acct-123224/order/o-123456",
          "dataSchema" : "1.0",
          "data" : {
             "orderId" : "123",
             "orderType" : "PO",
             "reference" : "https://www.myCompanyName.com/orders/123"
          }
    }]
    ```
4. Führen Sie den folgenden **Curl**-Befehl aus, um das Ereignis zu posten. Geben Sie die Endpunkt-URL und den Schlüssel aus Schritt 1 und 2 an, bevor Sie den Befehl ausführen. 

    ```
    curl -k -X POST -H "Content-Type: application/cloudevents-batch+json" -H "aeg-sas-key: <KEY FROM STEP 2>" -g -d @evt.json <ENDPOINT URL from STEP 1>
    ```

    > [!NOTE]
    > Um zu erfahren, wie Sie Cloudereignisse mithilfe von Programmiersprachen senden können, sehen Sie sich die folgenden Beispiele an: 
    > - [C#](/samples/azure/azure-sdk-for-net/azure-event-grid-sdk-samples/).
    > - [Java](/samples/azure/azure-sdk-for-java/eventgrid-samples/)
    > - [JavaScript](/samples/azure/azure-sdk-for-js/eventgrid-javascript/) und [TypeScript](/samples/azure/azure-sdk-for-js/eventgrid-typescript/)
    > - [Python](/samples/azure/azure-sdk-for-python/eventgrid-samples/)

### <a name="verify-in-the-event-grid-viewer"></a>Überprüfen im Event Grid-Viewer
Sie haben das Ereignis ausgelöst, und Event Grid hat die Nachricht an den Endpunkt gesendet, den Sie beim Abonnieren konfiguriert haben. Zeigen Sie Ihre Web-App an, um das soeben gesendete Ereignis anzuzeigen.

:::image type="content" source="./media/create-topic-subscription/viewer-received-event.png" alt-text="Empfangenes Ereignis im Event Grid Viewer anzeigen":::

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln: 
- [Ereignishandler und -Ziele](event-handlers.md): stellt Informationen über alle Ereignishandler und Ziele bereit, die Event Grid auf Kubernetes unterstützt. 
- [Ereignisfilterung](filter-events.md): stellt Informationen bezüglich des Filterns von Ereignissen in Ereignisabonnements bereit. 