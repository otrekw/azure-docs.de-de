---
title: Hinzufügen einer verwalteten Identität zu einer Rolle am Azure Event Grid-Ziel
description: In diesem Artikel wird beschrieben, wie Sie Azure-Rollen verwaltete Identitäten an Zielen wie Azure Service Bus und Azure Event Hubs hinzufügen.
ms.topic: how-to
ms.date: 03/25/2021
ms.openlocfilehash: 1578e4c24201614ce89351b3c3cee52a09cadc30
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106280478"
---
# <a name="grant-managed-identity-the-access-to-event-grid-destination"></a>Gewähren des Zugriffs auf ein Event Grid-Ziel für die verwaltete Identität
In diesem Abschnitt wird beschrieben, wie Sie die Identität für das Systemthema, das benutzerdefinierte Thema oder die benutzerdefinierte Domäne einer Azure-Rolle hinzufügen. 

## <a name="prerequisites"></a>Voraussetzungen
Weisen Sie anhand der Anweisungen in den folgenden Artikeln eine systemseitig zugewiesene verwaltete Identität zu:

- [Benutzerdefinierte Themen oder Domänen](enable-identity-custom-topics-domains.md)
- [Systemthemen](enable-identity-system-topics.md)

## <a name="supported-destinations-and-azure-roles"></a>Unterstützte Ziele und Azure-Rollen
Nachdem Sie die Identität für das benutzerdefinierte Event Grid-Thema bzw. die benutzerdefinierte Event Grid-Domäne aktiviert haben, erstellt Azure automatisch eine Identität in Azure Active Directory. Fügen Sie diese Identität den entsprechenden Azure-Rollen hinzu, damit das benutzerdefinierte Thema bzw. die benutzerdefinierte Domäne Ereignisse an unterstützte Ziele weiterleiten kann. Fügen Sie die Identität z. B. der Rolle **Azure Event Hubs-Datensender** für einen Azure Event Hubs-Namespace hinzu, damit das benutzerdefinierte Event Grid-Thema Ereignisse an Event Hubs in diesem Namespace weiterleiten kann. 

Derzeit unterstützt Azure Event Grid benutzerdefinierte Themen oder Domänen, die mit der vom System zugewiesenen verwalteten Identität konfiguriert wurden, um Ereignisse an die folgenden Ziele weiterzuleiten. Diese Tabelle enthält auch die Rollen, die die Identität aufweisen sollte, damit das benutzerdefinierte Thema die Ereignisse weiterleiten kann.

| Destination | Azure-Rolle | 
| ----------- | --------- | 
| Service Bus-Warteschlangen und -Themen | [Azure Service Bus-Datensender](../service-bus-messaging/authenticate-application.md#azure-built-in-roles-for-azure-service-bus) |
| Azure Event Hubs | [Azure Event Hubs-Datensender](../event-hubs/authorize-access-azure-active-directory.md#azure-built-in-roles-for-azure-event-hubs) | 
| Azure Blob Storage | [Mitwirkender an Speicherblobdaten](../storage/common/storage-auth-aad-rbac-portal.md#azure-roles-for-blobs-and-queues) |
| Azure Queue Storage |[Absender der Speicherwarteschlangen-Datennachricht](../storage/common/storage-auth-aad-rbac-portal.md#azure-roles-for-blobs-and-queues) | 


## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals
Sie können das Azure-Portal verwenden, um die Identität des benutzerdefinierten Themas bzw. der benutzerdefinierten Domäne einer entsprechenden Rolle zuzuweisen, damit das Thema/die Domäne Ereignisse an das Ziel weiterleiten kann. 

Im folgenden Beispiel wird eine verwaltete Identität für ein benutzerdefiniertes Event Grid-Thema mit dem Namen **msitesttopic** der Rolle **Azure Service Bus-Datensender** für einen Service Bus-Namespace hinzugefügt, der eine Warteschlangen- oder Themenressource enthält. Wenn Sie das benutzerdefinierte Event Grid-Thema der Rolle auf Namespaceebene hinzufügen, kann es Ereignisse an alle Entitäten im Namespace weiterleiten. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem **Service Bus-Namespace**. 
1. Klicken Sie im linken Bereich auf **Zugriffssteuerung**. 
1. Klicken Sie im Bereich **Rollenzuweisung hinzufügen** auf **Hinzufügen**. 
1. Führen Sie die folgenden Schritte auf der Seite **Rollenzuweisung hinzufügen** durch:
    1. Wählen Sie die Rolle aus. In diesem Fall wird die Rolle **Azure Service Bus-Datensender** verwendet. 
    1. Wählen Sie die **Identität** für das benutzerdefinierte Event Grid-Thema bzw. die benutzerdefinierte Event Grid-Domäne aus. 
    1. Wählen Sie zum Speichern der Konfiguration **Speichern** aus.

Die Schritte entsprechen dem Hinzufügen einer Identität zu anderen Rollen, die in der Tabelle aufgeführt sind. 

## <a name="use-the-azure-cli"></a>Verwenden der Azure-CLI
Im Beispiel in diesem Abschnitt wird gezeigt, wie Sie mit der Azure-Befehlszeilenschnittstelle eine Identität einer Azure-Rolle hinzufügen. Die Beispielbefehle beziehen sich auf benutzerdefinierte Event Grid-Themen. Die Befehle für Event Grid-Domänen sind ähnlich. 

### <a name="get-the-principal-id-for-the-custom-topics-system-identity"></a>Abrufen der Prinzipal-ID für die Systemidentität des benutzerdefinierten Themas 
Rufen Sie zunächst die Prinzipal-ID der vom System verwalteten Identität des benutzerdefinierten Themas ab, und weisen Sie die Identität den entsprechenden Rollen zu.

```azurecli-interactive
topic_pid=$(az ad sp list --display-name "$<TOPIC NAME>" --query [].objectId -o tsv)
```

### <a name="create-a-role-assignment-for-event-hubs-at-various-scopes"></a>Erstellen einer Rollenzuweisung für Event Hubs in verschiedenen Bereichen 
Im folgenden Beispiel für die Befehlszeilenschnittstelle wird veranschaulicht, wie die Identität eines benutzerdefinierten Themas der Rolle **Azure Event Hubs-Datensender** auf Namespaceebene oder auf Event Hub-Ebene hinzugefügt wird. Wenn Sie die Rollenzuweisung auf Namespaceebene erstellen, kann das benutzerdefinierte Thema Ereignisse an alle Event Hubs in diesem Namespace weiterleiten. Wenn Sie das Thema auf Event Hub-Ebene erstellen, kann das benutzerdefinierte Thema Ereignisse nur an diesen Event Hub weiterleiten. 


```azurecli-interactive
role="Azure Event Hubs Data Sender" 
namespaceresourceid=$(az eventhubs namespace show -n $<EVENT HUBS NAMESPACE NAME> -g <RESOURCE GROUP of EVENT HUB> --query "{I:id}" -o tsv) 
eventhubresourceid=$(az eventhubs eventhub show -n <EVENT HUB NAME> --namespace-name <EVENT HUBS NAMESPACE NAME> -g <RESOURCE GROUP of EVENT HUB> --query "{I:id}" -o tsv) 

# create role assignment for the whole namespace 
az role assignment create --role "$role" --assignee "$topic_pid" --scope "$namespaceresourceid" 

# create role assignment scoped to just one event hub inside the namespace 
az role assignment create --role "$role" --assignee "$topic_pid" --scope "$eventhubresourceid" 
```

### <a name="create-a-role-assignment-for-a-service-bus-topic-at-various-scopes"></a>Erstellen einer Rollenzuweisung für ein Service Bus-Thema in verschiedenen Bereichen 
Im folgenden Beispiel für die Befehlszeilenschnittstelle wird gezeigt, wie die Identität eines benutzerdefinierten Event Grid-Themas zur Rolle **Azure Service Bus-Datensender** auf Namespaceebene oder auf Ebene des Service Bus-Themas hinzugefügt wird. Wenn Sie die Rollenzuweisung auf Namespaceebene erstellen, kann das Event Grid-Thema Ereignisse an alle Entitäten (Service Bus-Warteschlangen oder -Themen) innerhalb dieses Namespace weiterleiten. Beim Erstellen einer Rollenzuweisung auf Service Bus-Warteschlangen- oder -Themenebene kann das benutzerdefinierte Event Grid-Thema Ereignisse nur an die betreffende Service Bus-Warteschlange oder das Service Bus-Thema weiterleiten. 

```azurecli-interactive
role="Azure Service Bus Data Sender" 
namespaceresourceid=$(az servicebus namespace show -n $RG\SB -g "$RG" --query "{I:id}" -o tsv 
sbustopicresourceid=$(az servicebus topic show -n topic1 --namespace-name $RG\SB -g "$RG" --query "{I:id}" -o tsv) 

# create role assignment for the whole namespace 
az role assignment create --role "$role" --assignee "$topic_pid" --scope "$namespaceresourceid" 

# create role assignment scoped to just one hub inside the namespace 
az role assignment create --role "$role" --assignee "$topic_pid" --scope "$sbustopicresourceid" 
```

## <a name="next-steps"></a>Nächste Schritte
Sie haben dem Systemthema, dem benutzerdefinierten Thema oder der benutzerdefinierten Domäne eine systemseitig zugewiesene Identität zugewiesen und die Identität den entsprechenden Rollen an Zielen hinzugefügt. Lesen Sie nun die Informationen unter [Ereignisübermittlung mit einer verwalteten Identität](managed-service-identity.md), um zu erfahren, wie Sie mithilfe der Identität Ereignisse an Ziele übermitteln.


