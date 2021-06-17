---
title: Aktivieren der automatischen Weiterleitung für Azure Service Bus-Warteschlangen und -Abonnements
description: In diesem Artikel wird erläutert, wie Sie die automatische Weiterleitung für Warteschlangen und Abonnements mithilfe von Azure-Portal, PowerShell, Befehlszeilenschnittstelle und Programmiersprachen (C#, Java, Python und JavaScript) aktivieren.
ms.topic: how-to
ms.date: 04/19/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: da5af50941263c54a08de27df43fc85b5bd75b4a
ms.sourcegitcommit: df574710c692ba21b0467e3efeff9415d336a7e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110671493"
---
# <a name="enable-auto-forwarding-for-azure-service-bus-queues-and-subscriptions"></a>Aktivieren der automatischen Weiterleitung für Azure Service Bus-Warteschlangen und -Abonnements
Mit dem Service Bus-Feature für die automatische Weiterleitung können Sie eine Warteschlange oder ein Abonnement mit einer weiteren Warteschlange oder einem Thema aus demselben Namespace verketten. Wenn die automatische Weiterleitung aktiviert ist, entfernt Service Bus die Nachrichten automatisch, die in der ersten Warteschlange oder dem Abonnement (Quelle) platziert wurden, und fügt sie in die zweite Warteschlange oder das Thema (Ziel) ein. Es ist weiterhin möglich, eine Nachricht direkt an die Zielentität zu senden. Weitere Informationen finden Sie unter [Verketten von Service Bus-Entitäten mit automatischer Weiterleitung](service-bus-auto-forwarding.md). In diesem Artikel werden verschiedene Möglichkeiten zum Aktivieren der automatischen Weiterleitung für Service Bus-Warteschlangen und -Abonnements beschrieben. 

> [!IMPORTANT]
> Im Basic-Tarif von Service Bus wird die automatische Weiterleitung nicht unterstützt. Die Standard- und Premium-Tarife unterstützen das Feature. Informationen zu den Unterschieden zwischen diesen Tarifen finden Sie unter [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="using-azure-portal"></a>Verwenden des Azure-Portals
Wählen Sie beim Erstellen einer **Warteschlange** oder eines **Abonnements** für ein Thema im Azure-Portal die Option **Nachrichten an Warteschlange/Thema weiterleiten** aus, wie in den folgenden Beispielen gezeigt. Geben Sie dann an, ob Nachrichten an eine Warteschlange oder ein Thema weitergeleitet werden sollen. In diesem Beispiel wird die Option **Queue** (Warteschlange) und anschließend eine Warteschlange aus demselben Namespace ausgewählt.

### <a name="create-a-queue-with-auto-forwarding-enabled"></a>Erstellen einer Warteschlange mit aktivierter automatischer Weiterleitung
:::image type="content" source="./media/enable-auto-forward/create-queue.png" alt-text="Aktivieren der automatischen Weiterleitung zum Zeitpunkt der Erstellung der Warteschlange":::

### <a name="create-a-subscription-for-a-topic-with-auto-forwarding-enabled"></a>Erstellen eines Abonnements für ein Thema mit aktivierter automatischer Weiterleitung
:::image type="content" source="./media/enable-auto-forward/create-subscription.png" alt-text="Aktivieren der automatischen Weiterleitung zum Zeitpunkt der Erstellung des Abonnements":::

### <a name="update-the-auto-forward-setting-for-an-existing-queue"></a>Aktualisieren der Einstellung für die automatische Weiterleitung für eine vorhandene Warteschlange
Wählen Sie unter **Übersicht** für Ihre Service Bus-Warteschlange den aktuellen Wert für die Einstellung **Forward messages to** (Weiterleitung von Nachrichten an) aus. Im folgenden Beispiel lautet der aktuelle Wert **Disabled** (Deaktiviert). Im Fenster **Nachrichten an Warteschlange/Thema weiterleiten** können Sie die Warteschlange oder das Thema auswählen, an die bzw. das die Nachrichten weitergeleitet werden sollen. 

:::image type="content" source="./media/enable-auto-forward/queue-auto-forward.png" alt-text="Aktivieren der automatischen Weiterleitung für eine vorhandene Warteschlange":::

### <a name="update-the-auto-forward-setting-for-an-existing-subscription"></a>Aktualisieren der Einstellung für die automatische Weiterleitung für ein vorhandenes Abonnement
Wählen Sie unter **Übersicht** für Ihr Service Bus-Abonnement den aktuellen Wert für die Einstellung **Forward messages to** (Weiterleitung von Nachrichten an) aus. Im folgenden Beispiel lautet der aktuelle Wert **Disabled** (Deaktiviert). Im Fenster **Nachrichten an Warteschlange/Thema weiterleiten** können Sie die Warteschlange oder das Thema auswählen, an die bzw. das die Nachrichten weitergeleitet werden sollen. 

:::image type="content" source="./media/enable-auto-forward/subscription-auto-forward.png" alt-text="Aktivieren der automatischen Weiterleitung für ein vorhandenes Abonnement":::

## <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Um **eine Warteschlange mit aktivierter automatischer Weiterleitung** zu erstellen, verwenden Sie den Befehl [`az servicebus queue create`](/cli/azure/servicebus/queue#az_servicebus_queue_create). Legen Sie dabei `--forward-to` auf den Namen der Warteschlange oder des Themas fest, an die bzw. das die Nachrichten weitergeleitet werden sollen. 

```azurecli-interactive
az servicebus queue create \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --name myqueue \
    --forward-to myqueue2
```

Um **die Einstellung für die automatische Weiterleitung für eine vorhandene Warteschlange zu aktualisieren**, verwenden Sie den Befehl [`az servicebus queue update`](/cli/azure/servicebus/queue#az_servicebus_queue_update), bei dem `--forward-to` auf den Namen der Warteschlange oder des Themas festgelegt ist, an die bzw. das die Nachrichten weitergeleitet werden sollen. 

```azurecli-interactive
az servicebus queue update \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --name myqueue \
    --forward-to myqueue2
```


Um **ein Abonnement für ein Thema mit aktivierter automatischer Weiterleitung** zu erstellen, verwenden Sie den Befehl [`az servicebus topic subscription create`](/cli/azure/servicebus/topic/subscription#az_servicebus_topic_subscription_create). Legen Sie dabei `--forward-to` auf den Namen der Warteschlange oder des Themas fest, an die bzw. das die Nachrichten weitergeleitet werden sollen.

```azurecli-interactive
az servicebus topic subscription create \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --topic-name mytopic \
    --name mysubscription \
    --forward-to myqueue2
```

Um **die Einstellung für die automatische Weiterleitung für ein Abonnement zu aktualisieren**, verwenden Sie den Befehl [`az servicebus topic subscription update`](/cli/azure/servicebus/topic/subscription#az_servicebus_topic_subscription_update), bei dem `--forward-to` auf den Namen der Warteschlange oder des Themas festgelegt ist, an die bzw. das die Nachrichten weitergeleitet werden sollen.

```azurecli-interactive
az servicebus topic subscription create \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --topic-name mytopic \
    --name mysubscription \
    --forward-to myqueue2
```

## <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell
Um **eine Warteschlange mit aktivierter automatischer Weiterleitung** zu erstellen, verwenden Sie den Befehl [`New-AzServiceBusQueue`](/powershell/module/az.servicebus/new-azservicebusqueue). Legen Sie dabei `-ForwardTo` auf den Namen der Warteschlange oder des Themas fest, an die bzw. das die Nachrichten weitergeleitet werden sollen. 

```azurepowershell-interactive
New-AzServiceBusQueue -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -QueueName myqueue `
    -ForwardTo myqueue2
```

Verwenden Sie den Befehl [`Set-AzServiceBusQueue`](/powershell/module/az.servicebus/set-azservicebusqueue), wie im folgenden Beispiel gezeigt, um die **Einstellung für die automatische Weiterleitung für eine vorhandene Warteschlange zu aktualisieren**.

```azurepowershell-interactive
$queue=Get-AzServiceBusQueue -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -QueueName myqueue 

$queue.ForwardTo='myqueue2'

Set-AzServiceBusQueue -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -QueueName myqueue `
    -QueueObj $queue
``` 

Um **ein Abonnement für ein Thema mit aktivierter automatischer Weiterleitung** zu erstellen, verwenden Sie den Befehl [`New-AzServiceBusSubscription`](/powershell/module/az.servicebus/new-azservicebussubscription). Legen Sie dabei `-ForwardTo` auf den Namen der Warteschlange oder des Themas fest, an die bzw. das die Nachrichten weitergeleitet werden sollen.

```azurepowershell-interactive
New-AzServiceBusSubscription -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -TopicName mytopic `
    -SubscriptionName mysubscription `
    -ForwardTo myqueue2
```

Informationen **zum Aktualisieren der Einstellung für die automatische Weiterleitung für ein vorhandenes Abonnement** finden Sie im folgenden Beispiel.

```azurepowershell-interactive
$subscription=Get-AzServiceBusSubscription -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -TopicName mytopic `
    -SubscriptionName mysub

$subscription.ForwardTo='mytopic2'

Set-AzServiceBusSubscription -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -Name mytopic `
    -SubscriptionName mysub `
    -SubscriptionObj $subscription 
```

## <a name="using-azure-resource-manager-template"></a>Verwenden von Azure Resource Manager-Vorlagen
Um **eine Warteschlange mit aktivierter automatischer Weiterleitung** zu erstellen, legen Sie `forwardTo` im Abschnitt „Eigenschaften“ der Warteschlange auf den Namen der Warteschlange oder des Themas fest, an die bzw. das die Nachrichten weitergeleitet werden sollen. Weitere Informationen finden Sie in der [Vorlagenreferenz für Microsoft.ServiceBus-Namespaces/-Warteschlangen](/azure/templates/microsoft.servicebus/namespaces/queues?tabs=json). 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "serviceBusNamespaceName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Service Bus namespace"
      }
    },
    "serviceBusQueueName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Queue"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.ServiceBus/namespaces",
      "apiVersion": "2018-01-01-preview",
      "name": "[parameters('serviceBusNamespaceName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard"
      },
      "properties": {},
      "resources": [
        {
          "type": "Queues",
          "apiVersion": "2017-04-01",
          "name": "[parameters('serviceBusQueueName')]",
          "dependsOn": [
            "[resourceId('Microsoft.ServiceBus/namespaces', parameters('serviceBusNamespaceName'))]"
          ],
          "properties": {
            "forwardTo": "myqueue2"
          }
        }
      ]
    }
  ]
}

```

Um ein **Abonnement für ein Thema mit aktivierter automatischer Weiterleitung** zu erstellen, legen Sie `forwardTo` im Abschnitt „Eigenschaften“ des Themas auf den Namen der Warteschlange oder des Themas fest, an die bzw. das die Nachrichten weitergeleitet werden sollen. Weitere Informationen finden Sie in der [Vorlagenreferenz für Microsoft.ServiceBus-Namespaces/-Themen/-Abonnements](/azure/templates/microsoft.servicebus/namespaces/topics/subscriptions?tabs=json). 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "service_BusNamespace_Name": {
      "type": "string",
      "metadata": {
        "description": "Name of the Service Bus namespace"
      }
    },
    "serviceBusTopicName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Topic"
      }
    },
    "serviceBusSubscriptionName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Subscription"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "resources": [
    {
      "apiVersion": "2018-01-01-preview",
      "name": "[parameters('service_BusNamespace_Name')]",
      "type": "Microsoft.ServiceBus/namespaces",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard"
      },
      "properties": {},
      "resources": [
        {
          "apiVersion": "2017-04-01",
          "name": "[parameters('serviceBusTopicName')]",
          "type": "topics",
          "dependsOn": [
            "[resourceId('Microsoft.ServiceBus/namespaces/', parameters('service_BusNamespace_Name'))]"
          ],
          "properties": {
            "maxSizeInMegabytes": 1024
          },
          "resources": [
            {
              "apiVersion": "2017-04-01",
              "name": "[parameters('serviceBusSubscriptionName')]",
              "type": "Subscriptions",
              "dependsOn": [
                "[parameters('serviceBusTopicName')]"
              ],
              "properties": {
                "forwardTo": "myqueue2"
              }
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="net"></a>.NET 

### <a name="azuremessagingservicebus-latest"></a>Azure.Messaging.ServiceBus (aktuelle Version)
Sie können die Funktion für die automatische Weiterleitung aktivieren, indem Sie [CreateQueueOptions.ForwardTo](/dotnet/api/azure.messaging.servicebus.administration.createqueueoptions.forwardto) oder [CreateSubscriptionOptions.ForwardTo](/dotnet/api/azure.messaging.servicebus.administration.createsubscriptionoptions.forwardto) festlegen und dann die Methoden [CreateQueueAsync](/dotnet/api/azure.messaging.servicebus.administration.servicebusadministrationclient.createqueueasync#Azure_Messaging_ServiceBus_Administration_ServiceBusAdministrationClient_CreateQueueAsync_Azure_Messaging_ServiceBus_Administration_CreateQueueOptions_System_Threading_CancellationToken_) oder [CreateSubscriptionAsync](/dotnet/api/azure.messaging.servicebus.administration.servicebusadministrationclient.createsubscriptionasync#Azure_Messaging_ServiceBus_Administration_ServiceBusAdministrationClient_CreateSubscriptionAsync_Azure_Messaging_ServiceBus_Administration_CreateSubscriptionOptions_System_Threading_CancellationToken_) verwenden, die die Parameter `CreateQueueOptions` oder `CreateSubscriptionOptions` annehmen. 

### <a name="microsoftazureservicebus-legacy"></a>Microsoft.Azure.ServiceBus (Legacyversion)
Sie können die automatische Weiterleitung durch Festlegen der Eigenschaften [QueueDescription.ForwardTo](/dotnet/api/microsoft.servicebus.messaging.queuedescription) oder [SubscriptionDescription.ForwardTo](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) für die Quelle aktivieren, wie im folgenden Beispiel veranschaulicht:

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

## <a name="java"></a>Java

### <a name="azure-messaging-servicebus-latest"></a>azure-messaging-servicebus (aktuelle Version)
Sie können die Funktion für die automatische Weiterleitung aktivieren, indem Sie die Methode [CreateQueueOptions.setForwardTo(String forwardTo)](/java/api/com.azure.messaging.servicebus.administration.models.createqueueoptions.setforwardto) oder [CreateSubscriptionOptions.setForwardTo(String forwardTo)](/java/api/com.azure.messaging.servicebus.administration.models.createsubscriptionoptions.setforwardto) verwenden und dann die [createQueue](/java/api/com.azure.messaging.servicebus.administration.servicebusadministrationclient.createqueue#com_azure_messaging_servicebus_administration_ServiceBusAdministrationClient_createQueue_java_lang_String_com_azure_messaging_servicebus_administration_models_CreateQueueOptions_)-Methode oder die [createSubscription](/java/api/com.azure.messaging.servicebus.administration.servicebusadministrationclient.createsubscription#com_azure_messaging_servicebus_administration_ServiceBusAdministrationClient_createSubscription_java_lang_String_java_lang_String_com_azure_messaging_servicebus_administration_models_CreateSubscriptionOptions_)-Methode verwenden, die die Parameter `CreateQueueOptions` oder `CreateSubscriptionOptions` annehmen können. 

### <a name="azure-servicebus-legacy"></a>azure-servicebus (Legacyversion)
Sie können die automatische Weiterleitung aktivieren, indem Sie [QueueDescription.setForwardTo(String forwardTo)](/java/api/com.microsoft.azure.servicebus.management.queuedescription.setforwardto#com_microsoft_azure_servicebus_management_QueueDescription_setForwardTo_java_lang_String_) oder [SubscriptionDescription.setForwardTo(String forwardTo)](/java/api/com.microsoft.azure.servicebus.management.subscriptiondescription.setforwardto) für die Quelle verwenden. 


## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die Beispiele in der Sprache Ihrer Wahl an, um Azure Service Bus-Features zu untersuchen. 

- [Azure Service Bus-Clientbibliothekbeispiele für .NET (neueste Version)](/samples/azure/azure-sdk-for-net/azuremessagingservicebus-samples/) 
- [Azure Service Bus-Clientbibliothekbeispiele für Java (neueste Version)](/samples/azure/azure-sdk-for-java/servicebus-samples/)
- [Azure Service Bus-Clientbibliothekbeispiele für Python](/samples/azure/azure-sdk-for-python/servicebus-samples/)
- [Azure Service Bus-Clientbibliothekbeispiele für JavaScript](/samples/azure/azure-sdk-for-js/service-bus-javascript/)
- [Azure Service Bus-Clientbibliothekbeispiele für TypeScript](/samples/azure/azure-sdk-for-js/service-bus-typescript/)

Hier finden Sie Beispiele für die älteren .NET- und Java-Clientbibliotheken:
- [Azure Service Bus-Clientbibliothekbeispiele für .NET (Legacy)](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/)
- [Azure Service Bus-Clientbibliothekbeispiele für Java (Legacy)](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/azure-servicebus)
