---
title: Aktivierung von Azure Service Bus-Nachrichtensitzungen | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie Nachrichtensitzungen mit Azure-Portal, PowerShell, CLI und Programmiersprachen (C#, Java, Python und JavaScript) aktivieren
ms.topic: how-to
ms.date: 04/19/2021
ms.openlocfilehash: 53791bb7dc1dec9bdf6c2c4aa8dda14728c2c465
ms.sourcegitcommit: aba63ab15a1a10f6456c16cd382952df4fd7c3ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107989142"
---
# <a name="enable-message-sessions-for-an-azure-service-bus-queue-or-a-subscription"></a>Aktivieren von Nachrichtensitzungen für eine Azure Service Bus-Warteschlange oder ein Abonnement
Azure Service Bus-Sitzungen ermöglichen die gemeinsame und geordnete Verarbeitung unbegrenzter Sequenzen verwandter Nachrichten. Sitzungen können mit **FIFO**-Mustern (First in, First Out) und **Anforderung/Antwort**-Mustern verwendet werden. Weitere Informationen finden Sie unter [Nachrichtensitzungen](message-sessions.md). In diesem Artikel werden verschiedene Möglichkeiten zum Aktivieren von Sitzungen für eine Service Bus-Warteschlange oder ein Abonnement beschrieben. 

> [!IMPORTANT]
> - Der Basic-Tarif von Service Bus unterstützt keine Sitzungen. Die Standard- und Premium-Tarife unterstützen Sitzungen. Informationen zu den Unterschieden zwischen diesen Tarifen finden Sie unter [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/).
> - Sie können Nachrichtensitzungen nicht aktivieren oder deaktivieren, nachdem die Warteschlange oder das Abonnement erstellt wurde. Dies ist nur zum Zeitpunkt der Erstellung der Warteschlange oder des Abonnements möglich. 

## <a name="using-azure-portal"></a>Verwenden des Azure-Portals
Wenn Sie eine **Warteschlange** im Azure-Portal erstellen, wählen Sie **Sitzungen aktivieren** aus, wie in der folgenden Abbildung dargestellt. 

:::image type="content" source="./media/message-sessions/queue-sessions.png" alt-text="Aktivieren von Sitzungen zum Zeitpunkt der Warteschlangenerstellung":::

Wenn Sie ein Abonnement im Azure-Portal erstellen, wählen Sie **Sitzungen aktivieren** aus, wie in der folgenden Abbildung dargestellt. 

:::image type="content" source="./media/message-sessions/subscription-sessions.png" alt-text="Aktivieren von Sitzungen zum Zeitpunkt der Abonnementerstellung":::

## <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Um **eine Warteschlange mit aktivierten Nachrichtensitzungen zu erstellen**, verwenden Sie den [`az servicebus queue create`](/cli/azure/servicebus/queue#az_servicebus_queue_create)-Befehl, bei dem `--enable-session` auf `true` festgelegt ist.

```azurecli-interactive
az servicebus queue create \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --name myqueue \
    --enable-session true
```

Um **ein Abonnement für ein Thema mit aktivierten Nachrichtensitzungen zu erstellen**, verwenden Sie den [`az servicebus topic subscription create`](/cli/azure/servicebus/topic/subscription#az_servicebus_topic_subscription_create)-Befehl, bei dem `--enable-session` auf festgelegt `true` ist.

```azurecli-interactive
az servicebus topic subscription create \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --topic-name mytopic \
    --name mysubscription \
    --enable-session true
```

## <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell
Um **eine Warteschlange mit aktivierten Nachrichtensitzungen zu erstellen**, verwenden Sie den [`New-AzServiceBusQueue`](/powershell/module/az.servicebus/new-azservicebusqueue)-Befehl, bei dem `-RequiresSession` auf `$True` festgelegt ist. 

```azurepowershell-interactive
New-AzServiceBusQueue -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -QueueName myqueue `
    -RequiresSession $True
```

Um **ein Abonnement für ein Thema mit aktivierten Nachrichtensitzungen zu erstellen**, verwenden Sie den [`New-AzServiceBusSubscription`](/powershell/module/az.servicebus/new-azservicebussubscription)-Befehl, bei dem `-RequiresSession` auf festgelegt `true` ist. 

```azurepowershell-interactive
New-AzServiceBusSubscription -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -TopicName mytopic `
    -SubscriptionName mysubscription `
    -RequiresSession $True
```

## <a name="using-azure-resource-manager-template"></a>Verwenden von Azure Resource Manager-Vorlagen
Um **eine Warteschlange mit aktivierten Nachrichtensitzungen zu erstellen**, stellen Sie im Abschnitt Warteschlangeneigenschaften `requiresSession` auf `true`. Weitere Informationen finden Sie unter [Microsoft.ServiceBus namespaces/queues template reference](/azure/templates/microsoft.servicebus/namespaces/queues?tabs=json). 

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
            "requiresSession": true
          }
        }
      ]
    }
  ]
}

```

Um **eine Warteschlange mit aktivierten Nachrichtensitzungen zu erstellen**, stellen Sie im Abschnitt Warteschlangeneigenschaften `requiresSession` auf `true`. Weitere Informationen finden Sie unter [Microsoft.ServiceBus namespaces/topics/subscriptions template reference](/azure/templates/microsoft.servicebus/namespaces/topics/subscriptions?tabs=json). 

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
                "requiresSession": true
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

- [Azure Service Bus-Clientbibliothekbeispiele für .NET (neueste Version)](/samples/azure/azure-sdk-for-net/azuremessagingservicebus-samples/) 
- [Azure Service Bus-Clientbibliothekbeispiele für Java (neueste Version)](/samples/azure/azure-sdk-for-java/servicebus-samples/)
- [Azure Service Bus-Clientbibliothekbeispiele für Python](/samples/azure/azure-sdk-for-python/servicebus-samples/)
- [Azure Service Bus-Clientbibliothekbeispiele für JavaScript](/samples/azure/azure-sdk-for-js/service-bus-javascript/)
- [Azure Service Bus-Clientbibliothekbeispiele für TypeScript](/samples/azure/azure-sdk-for-js/service-bus-typescript/)

Hier finden Sie Beispiele für die älteren .NET- und Java-Clientbibliotheken:
- [Azure Service Bus-Clientbibliothekbeispiele für .NET (Legacy)](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/)
- [Azure Service Bus-Clientbibliothekbeispiele für Java (Legacy)](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/azure-servicebus)