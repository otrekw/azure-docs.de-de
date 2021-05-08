---
title: Aktivieren der automatischen Weiterleitung für Azure Service Bus-Warteschlangen und -Abonnements
description: In diesem Artikel wird erläutert, wie Sie die automatische Weiterleitung für Warteschlangen und Abonnements mithilfe von Azure-Portal, PowerShell, Befehlszeilenschnittstelle und Programmiersprachen (C#, Java, Python und JavaScript) aktivieren.
ms.topic: how-to
ms.date: 04/19/2021
ms.openlocfilehash: ef22ae08485dc896c94858db4e422cf89a00ec1f
ms.sourcegitcommit: 6686a3d8d8b7c8a582d6c40b60232a33798067be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107755130"
---
# <a name="enable-auto-forwarding-for-azure-service-bus-queues-and-subscriptions"></a>Aktivieren der automatischen Weiterleitung für Azure Service Bus-Warteschlangen und -Abonnements
Mit dem Service Bus-Feature für die automatische Weiterleitung können Sie eine Warteschlange oder ein Abonnement mit einer weiteren Warteschlange oder einem Thema aus demselben Namespace verketten. Wenn die automatische Weiterleitung aktiviert ist, entfernt Service Bus die Nachrichten automatisch, die in der ersten Warteschlange oder dem Abonnement (Quelle) platziert wurden, und fügt sie in die zweite Warteschlange oder das Thema (Ziel) ein. Es ist weiterhin möglich, eine Nachricht direkt an die Zielentität zu senden. Weitere Informationen finden Sie unter [Verketten von Service Bus-Entitäten mit automatischer Weiterleitung](service-bus-auto-forwarding.md). In diesem Artikel werden verschiedene Möglichkeiten zum Aktivieren der automatischen Weiterleitung für Service Bus-Warteschlangen und -Abonnements beschrieben. 

> [!IMPORTANT]
> Im Basic-Tarif von Service Bus wird die automatische Weiterleitung nicht unterstützt. Die Standard- und Premium-Tarife unterstützen das Feature. Informationen zu den Unterschieden zwischen diesen Tarifen finden Sie unter [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="using-azure-portal"></a>Verwenden des Azure-Portals
Wählen Sie beim Erstellen einer **Warteschlange** oder eines **Abonnements** für ein Thema im Azure-Portal die Option **Nachrichten an Warteschlange/Thema weiterleiten** aus, wie in den folgenden Beispielen gezeigt. Geben Sie dann an, ob Nachrichten an eine Warteschlange oder ein Thema weitergeleitet werden sollen. In diesem Beispiel wird die Option **Warteschlange** und anschließend eine Warteschlange (**myqueue**) aus demselben Namespace ausgewählt.

### <a name="create-a-queue-with-auto-forwarding-enabled"></a>Erstellen einer Warteschlange mit aktivierter automatischer Weiterleitung
:::image type="content" source="./media/enable-auto-forward/create-queue.png" alt-text="Aktivieren der automatischen Weiterleitung zum Zeitpunkt der Erstellung der Warteschlange":::

### <a name="create-a-subscription-for-a-topic-with-auto-forwarding-enabled"></a>Erstellen eines Abonnements für ein Thema mit aktivierter automatischer Weiterleitung
:::image type="content" source="./media/enable-auto-forward/create-subscription.png" alt-text="Aktivieren der automatischen Weiterleitung zum Zeitpunkt der Erstellung des Abonnements":::

## <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Um **eine Warteschlange mit aktivierter automatischer Weiterleitung** zu erstellen, verwenden Sie den Befehl [`az servicebus queue create`](/cli/azure/servicebus/queue#az_servicebus_queue_create). Legen Sie dabei `--forward-to` auf den Namen der Warteschlange oder des Themas fest, an die bzw. das die Nachrichten weitergeleitet werden sollen. 

```azurecli-interactive
az servicebus queue create \
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

## <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell
Um **eine Warteschlange mit aktivierter automatischer Weiterleitung** zu erstellen, verwenden Sie den Befehl [`New-AzServiceBusQueue`](/powershell/module/az.servicebus/new-azservicebusqueue). Legen Sie dabei `-ForwardTo` auf den Namen der Warteschlange oder des Themas fest, an die bzw. das die Nachrichten weitergeleitet werden sollen. 

```azurepowershell-interactive
New-AzServiceBusQueue -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -QueueName myqueue `
    -ForwardTo myqueue2
```

Um **ein Abonnement für ein Thema mit aktivierter automatischer Weiterleitung** zu erstellen, verwenden Sie den Befehl [`New-AzServiceBusSubscription`](/powershell/module/az.servicebus/new-azservicebussubscription). Legen Sie dabei `-ForwardTo` auf den Namen der Warteschlange oder des Themas fest, an die bzw. das die Nachrichten weitergeleitet werden sollen.

```azurepowershell-interactive
New-AzServiceBusSubscription -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -TopicName mytopic `
    -SubscriptionName mysubscription `
    -ForwardTo myqueue2
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


## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die Beispiele in der Sprache Ihrer Wahl an, um Azure Service Bus-Features zu untersuchen. 

- [Azure Service Bus-Clientbibliothekbeispiele für Java](/samples/azure/azure-sdk-for-java/servicebus-samples/)
- [Azure Service Bus-Clientbibliothekbeispiele für Python](/samples/azure/azure-sdk-for-python/servicebus-samples/)
- [Azure Service Bus-Clientbibliothekbeispiele für JavaScript](/samples/azure/azure-sdk-for-js/service-bus-javascript/)
- [Azure Service Bus-Clientbibliothekbeispiele für TypeScript](/samples/azure/azure-sdk-for-js/service-bus-typescript/)
- [Azure.Messaging.ServiceBus-Beispiele für .NET](/samples/azure/azure-sdk-for-net/azuremessagingservicebus-samples/)

Hier finden Sie Beispiele für die älteren .NET- und Java-Clientbibliotheken:
- [Microsoft.Azure.ServiceBus-Beispiele für .NET](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/)
- [azure-servicebus-Beispiele für Java](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/azure-servicebus/MessageBrowse)