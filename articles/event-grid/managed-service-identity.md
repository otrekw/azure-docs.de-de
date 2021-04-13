---
title: Ereignisbereitstellung, verwaltete Dienstidentität und private Verbindung
description: In diesem Artikel wird beschrieben, wie Sie die verwaltete Dienstidentität für ein Azure Event Grid-Thema aktivieren. So können Sie Ereignisse an unterstützte Ziele weiterleiten.
ms.topic: how-to
ms.date: 03/25/2021
ms.openlocfilehash: 76f10b4627dc9578b1e616a868eab03431b59b69
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105625274"
---
# <a name="event-delivery-with-a-managed-identity"></a>Ereignisübermittlung mit einer verwalteten Identität
In diesem Artikel wird beschrieben, wie Sie eine [verwaltete Dienstidentität](../active-directory/managed-identities-azure-resources/overview.md) für ein Systemthema oder ein benutzerdefiniertes Thema von Azure Event Grid oder für eine Azure Event Grid-Domäne verwenden. Verwenden Sie diese Methode, um Ereignisse an unterstützte Ziele wie Service Bus-Warteschlangen und -Themen, Event Hubs und Speicherkonten weiterzuleiten.



## <a name="prerequisites"></a>Voraussetzungen
1. Zuweisen einer systemseitig zugewiesenen Identität zu einem Systemthema, einem benutzerdefinierten Thema oder einer Domäne. 
    - Informationen zu benutzerdefinierten Themen und Domänen finden Sie unter [Aktivieren einer verwalteten Identität für benutzerdefinierte Themen und Domänen](enable-identity-custom-topics-domains.md). 
    - Informationen zu Systemthemen finden Sie unter [Aktivieren einer verwalteten Identität für Systemthemen](enable-identity-system-topics.md).
1. Fügen Sie die Identität einer geeigneten Rolle (z. B. Azure Service Bus-Datensender) für das Ziel (z. B. Service Bus-Warteschlange) zu. Ausführliche Schritte finden Sie unter [Hinzufügen einer Identität zu Azure-Rollen für Ziele](add-identity-roles.md).

    > [!NOTE]
    > Derzeit ist es nicht möglich, Ereignisse über [private Endpunkte](../private-link/private-endpoint-overview.md) zu übermitteln. Weitere Informationen finden Sie im Abschnitt [Private Endpunkte](#private-endpoints) am Ende dieses Artikels. 

## <a name="create-event-subscriptions-that-use-an-identity"></a>Erstellen von Ereignisabonnements, die eine Identität verwenden
Sobald Sie über ein benutzerdefiniertes Event Grid-Thema, ein Event Grid-Systemthema oder eine Domäne mit einer systemseitig verwalteten Identität verfügen und die Identität der entsprechenden Rolle auf dem Ziel hinzugefügt haben, können Sie Abonnements erstellen, die die Identität verwenden. 

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals
Wenn Sie ein Ereignisabonnement erstellen, wird im Abschnitt **ENDPUNKTDETAILS** eine Option angezeigt, mit der die Verwendung der vom System zugewiesenen Identität für einen Endpunkt aktiviert werden kann. 

![Aktivieren der Identität beim Erstellen eines Ereignisabonnements für die Service Bus-Warteschlange](./media/managed-service-identity/service-bus-queue-subscription-identity.png)

Auf der Registerkarte **Zusätzliche Features** können Sie auch die Verwendung der vom System zugewiesenen Identität für unzustellbare Nachrichten aktivieren. 

![Aktivieren der vom System zugewiesenen Identität zur Verarbeitung unzustellbarer Nachrichten](./media/managed-service-identity/enable-deadletter-identity.png)

### <a name="use-the-azure-cli---service-bus-queue"></a>Verwenden der Azure-Befehlszeilenschnittstelle: Service Bus-Warteschlange 
In diesem Abschnitt erfahren Sie, wie Sie die vom System zugewiesene Identität zum Übermitteln von Ereignissen an eine Service Bus-Warteschlange mit der Azure-Befehlszeilenschnittstelle aktivieren. Die Identität muss Mitglied der Rolle **Azure Service Bus-Datensender** sein. Sie muss auch Mitglied der Rolle **Mitwirkender an Storage-Blobdaten** für das Speicherkonto sein, das für unzustellbare Nachrichten verwendet wird. 

#### <a name="define-variables"></a>Definieren von Variablen
Geben Sie zunächst Werte für die folgenden Variablen an, die im Befehl für die Befehlszeilenschnittstelle verwendet werden sollen. 

```azurecli-interactive
subid="<AZURE SUBSCRIPTION ID>"
rg = "<RESOURCE GROUP of EVENT GRID CUSTOM TOPIC>"
topicname = "<EVENT GRID TOPIC NAME>"

# get the service bus queue resource id
queueid=$(az servicebus queue show --namespace-name <SERVICE BUS NAMESPACE NAME> --name <QUEUE NAME> --resource-group <RESOURCE GROUP NAME> --query id --output tsv)
sb_esname = "<Specify a name for the event subscription>" 
```

#### <a name="create-an-event-subscription-by-using-a-managed-identity-for-delivery"></a>Erstellen eines Ereignisabonnements mithilfe der verwalteten Identität für die Übermittlung 
Mit diesem Beispielbefehl wird ein Ereignisabonnement für ein benutzerdefiniertes Event Grid-Thema erstellt, wobei der Endpunkttyp auf **Service Bus-Warteschlange** festgelegt ist. 

```azurecli-interactive
az eventgrid event-subscription create  
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname
    --delivery-identity-endpoint-type servicebusqueue  
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $queueid
    -n $sb_esname 
```

#### <a name="create-an-event-subscription-by-using-a-managed-identity-for-delivery-and-dead-lettering"></a>Erstellen eines Ereignisabonnements mithilfe der verwalteten Identität für die Übermittlung und unzustellbare Nachrichten
Mit diesem Beispielbefehl wird ein Ereignisabonnement für ein benutzerdefiniertes Event Grid-Thema erstellt, wobei der Endpunkttyp auf **Service Bus-Warteschlange** festgelegt ist. Außerdem wird angegeben, dass die vom System verwaltete Identität für unzustellbare Nachrichten verwendet werden soll. 

```azurecli-interactive
storageid=$(az storage account show --name demoStorage --resource-group gridResourceGroup --query id --output tsv)
deadletterendpoint="$storageid/blobServices/default/containers/<BLOB CONTAINER NAME>"

az eventgrid event-subscription create  
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname 
    --delivery-identity-endpoint-type servicebusqueue
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $queueid
    --deadletter-identity-endpoint $deadletterendpoint 
    --deadletter-identity systemassigned 
    -n $sb_esnameq 
```

### <a name="use-the-azure-cli---event-hubs"></a>Verwenden der Azure-Befehlszeilenschnittstelle: Event Hubs 
In diesem Abschnitt erfahren Sie, wie Sie die vom System zugewiesene Identität zum Übermitteln von Ereignissen an einen Event Hub mit der Azure-Befehlszeilenschnittstelle aktivieren. Die Identität muss Mitglied der Rolle **Azure Event Hubs-Datensender** sein. Sie muss auch Mitglied der Rolle **Mitwirkender an Storage-Blobdaten** für das Speicherkonto sein, das für unzustellbare Nachrichten verwendet wird. 

#### <a name="define-variables"></a>Definieren von Variablen
```azurecli-interactive
subid="<AZURE SUBSCRIPTION ID>"
rg = "<RESOURCE GROUP of EVENT GRID CUSTOM TOPIC>"
topicname = "<EVENT GRID CUSTOM TOPIC NAME>"

hubid=$(az eventhubs eventhub show --name <EVENT HUB NAME> --namespace-name <NAMESPACE NAME> --resource-group <RESOURCE GROUP NAME> --query id --output tsv)
eh_esname = "<SPECIFY EVENT SUBSCRIPTION NAME>" 
```

#### <a name="create-an-event-subscription-by-using-a-managed-identity-for-delivery"></a>Erstellen eines Ereignisabonnements mithilfe der verwalteten Identität für die Übermittlung 
Mit diesem Beispielbefehl wird ein Ereignisabonnement für ein benutzerdefiniertes Event Grid-Thema erstellt, wobei der Endpunkttyp auf **Event Hubs** festgelegt ist. 

```azurecli-interactive
az eventgrid event-subscription create  
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname 
    --delivery-identity-endpoint-type eventhub 
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $hubid
    -n $sbq_esname 
```

#### <a name="create-an-event-subscription-by-using-a-managed-identity-for-delivery--deadletter"></a>Erstellen eines Ereignisabonnements mithilfe der verwalteten Identität für die Übermittlung und unzustellbare Nachrichten 
Mit diesem Beispielbefehl wird ein Ereignisabonnement für ein benutzerdefiniertes Event Grid-Thema erstellt, wobei der Endpunkttyp auf **Event Hubs** festgelegt ist. Außerdem wird angegeben, dass die vom System verwaltete Identität für unzustellbare Nachrichten verwendet werden soll. 

```azurecli-interactive
storageid=$(az storage account show --name demoStorage --resource-group gridResourceGroup --query id --output tsv)
deadletterendpoint="$storageid/blobServices/default/containers/<BLOB CONTAINER NAME>"

az eventgrid event-subscription create
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname 
    --delivery-identity-endpoint-type servicebusqueue  
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $hubid
    --deadletter-identity-endpoint $eh_deadletterendpoint
    --deadletter-identity systemassigned 
    -n $eh_esname 
```

### <a name="use-the-azure-cli---azure-storage-queue"></a>Verwenden der Azure-Befehlszeilenschnittstelle: Azure Storage-Warteschlange 
In diesem Abschnitt erfahren Sie, wie Sie die vom System zugewiesene Identität zum Übermitteln von Ereignissen an eine Azure Storage-Warteschlange mit der Azure-Befehlszeilenschnittstelle aktivieren. Die Identität muss Mitglied der Rolle **Absender der Speicherwarteschlangen-Datennachricht** im Speicherkonto sein. Sie muss auch Mitglied der Rolle **Mitwirkender an Storage-Blobdaten** für das Speicherkonto sein, das für unzustellbare Nachrichten verwendet wird.

#### <a name="define-variables"></a>Definieren von Variablen  

```azurecli-interactive
subid="<AZURE SUBSCRIPTION ID>"
rg = "<RESOURCE GROUP of EVENT GRID CUSTOM TOPIC>"
topicname = "<EVENT GRID CUSTOM TOPIC NAME>"

# get the storage account resource id
storageid=$(az storage account show --name <STORAGE ACCOUNT NAME> --resource-group <RESOURCE GROUP NAME> --query id --output tsv)

# build the resource id for the queue
queueid="$storageid/queueservices/default/queues/<QUEUE NAME>" 

sa_esname = "<SPECIFY EVENT SUBSCRIPTION NAME>" 
```

#### <a name="create-an-event-subscription-by-using-a-managed-identity-for-delivery"></a>Erstellen eines Ereignisabonnements mithilfe der verwalteten Identität für die Übermittlung 

```azurecli-interactive
az eventgrid event-subscription create 
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname 
    --delivery-identity-endpoint-type storagequeue  
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $queueid
    -n $sa_esname 
```

#### <a name="create-an-event-subscription-by-using-a-managed-identity-for-delivery--deadletter"></a>Erstellen eines Ereignisabonnements mithilfe der verwalteten Identität für die Übermittlung und unzustellbare Nachrichten 

```azurecli-interactive
storageid=$(az storage account show --name demoStorage --resource-group gridResourceGroup --query id --output tsv)
deadletterendpoint="$storageid/blobServices/default/containers/<BLOB CONTAINER NAME>"

az eventgrid event-subscription create  
    --source-resource-id /subscriptions/$subid/resourceGroups/$rg/providers/Microsoft.EventGrid/topics/$topicname 
    --delivery-identity-endpoint-type storagequeue  
    --delivery-identity systemassigned 
    --delivery-identity-endpoint $queueid
    --deadletter-identity-endpoint $deadletterendpoint 
    --deadletter-identity systemassigned 
    -n $sa_esname 
```

## <a name="private-endpoints"></a>Private Endpunkte
Derzeit ist es nicht möglich, Ereignisse über [private Endpunkte](../private-link/private-endpoint-overview.md) zu übermitteln. Das heißt, es gibt keine Unterstützung, wenn Sie strikte Anforderungen an die Netzwerkisolation haben, bei denen der Datenverkehr der übermittelten Ereignisse den privaten IP-Adressraum nicht verlassen darf. 

Wenn Ihre Anforderungen jedoch eine sichere Methode zum Senden von Ereignissen über einen verschlüsselten Kanal und eine bekannte Identität des Absenders (in diesem Fall Event Grid) unter Verwendung eines öffentlichen IP-Adressraums erfordern, könnten Sie Ereignisse an Event Hubs, Service Bus oder Azure Storage Service unter Verwendung eines benutzerdefinierten Azure Event Grid-Themas oder einer Domäne mit vom System verwalteter Identität bereitstellen, die wie in diesem Artikel gezeigt konfiguriert ist. Anschließend können Sie eine private Verbindung verwenden, die in Azure Functions konfiguriert ist, oder Ihren Webhook, der in Ihrem virtuellen Netzwerk zum Pullen von Ereignissen bereitgestellt wurde. Weitere Informationen finden Sie im Beispiel: [Herstellen einer Verbindung mit privaten Endpunkten mit Azure Functions](/samples/azure-samples/azure-functions-private-endpoints/connect-to-private-endpoints-with-azure-functions/).

Bei dieser Konfiguration wird der Datenverkehr über die öffentliche IP-Adresse bzw. das Internet aus Event Grid an Event Hubs, Service Bus oder Azure Storage übermittelt. Der Kanal kann jedoch verschlüsselt werden, und eine verwaltete Identität von Event Grid wird verwendet. Wenn Sie Ihre Azure Functions-Funktion oder Ihren Webhook, der in Ihrem virtuellen Netzwerk bereitgestellt wurde, für die Verwendung von Event Hubs, Service Bus oder Azure Storage über eine private Verbindung konfigurieren, verbleibt dieser Teil des Datenverkehrs offensichtlich in Azure.


## <a name="next-steps"></a>Nächste Schritte
Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md).
