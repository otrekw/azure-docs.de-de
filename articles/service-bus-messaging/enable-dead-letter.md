---
title: Aktivieren Sie Dead Lettering für Azure Service Bus-Warteschlangen und -Abonnements
description: In diesem Artikel wird erklärt, wie Sie Dead Lettering für Warteschlangen und Abonnements mithilfe von Azure-Portal, PowerShell, CLI und Programmiersprachen (C#, Java, Python und JavaScript) aktivieren
ms.topic: how-to
ms.date: 04/20/2021
ms.openlocfilehash: 789f9221c224a70225849d05b736276b95ebfc11
ms.sourcegitcommit: aba63ab15a1a10f6456c16cd382952df4fd7c3ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107989268"
---
# <a name="enable-dead-lettering-on-message-expiration-for-azure-service-bus-queues-and-subscriptions"></a>Aktivieren Sie Dead Lettering beim Ablauf von Nachrichten für Azure Service Bus-Warteschlangen und -Abonnements
Die Azure Service Bus-Warteschlangen und Themenabonnements bieten eine sekundäre Sub-Warteschlange, die sogenannte Dead-Letter-Queue (DLQ). Die Warteschlange für unzustellbare Nachrichten muss nicht explizit erstellt werden, und kann nicht gelöscht oder unabhängig von der Hauptentität verwaltet werden. Die Warteschlange für unzustellbare Nachrichten dient zum Speichern von Nachrichten, die an keinen Empfänger übermittelt oder nicht verarbeitet werden können. Weitere Informationen finden Sie unter [Übersicht über Service Bus-Warteschlangen für unzustellbare Nachrichten](service-bus-dead-letter-queues.md). In diesem Artikel werden verschiedene Möglichkeiten zum Aktivieren von Dead Lettering für Service Bus-Warteschlangen und -Abonnements beschrieben. 

## <a name="using-azure-portal"></a>Verwenden des Azure-Portals
Beim Erstellen einer **Warteschlange** oder eines **Themenabonnements** im Azure-Portal wählen Sie die Option **Ablauf von Nachrichten aktivieren**, wie in den folgenden Beispielen gezeigt. 

### <a name="create-a-queue-with-dead-lettering-enabled"></a>Erstellen Sie eine Warteschlange mit aktiviertem Dead Lettering
:::image type="content" source="./media/enable-dead-letter/create-queue.png" alt-text="Aktivieren Sie Dead Lettering zum Zeitpunkt der Erstellung der Warteschlange":::

### <a name="create-a-subscription-with-dead-lettering-enabled"></a>Erstellen Sie ein Abonnement mit aktiviertem Dead Lettering
:::image type="content" source="./media/enable-dead-letter/create-subscription.png" alt-text="Aktivieren Sie Dead Lettering zum Zeitpunkt der Abonnementerstellung":::

### <a name="update-the-dead-lettering-on-message-expiration-setting-for-an-existing-queue"></a>Aktualisieren Sie die Einstellung für Dead Lettering bei Ablauf der Nachricht für eine vorhandene Warteschlange
Auf der Seite **Übersicht** für Ihre Service-Bus-Warteschlange wählen Sie den aktuellen Wert für die Einstellung **Dead lettering** bei Ablauf der Nachricht. Im folgenden Beispiel ist der aktuelle Wert **Deaktiviert**. Im Popup-Fenster können Sie Dead Lettering beim Ablauf der Nachricht aktivieren oder deaktivieren. 

:::image type="content" source="./media/enable-dead-letter/queue-configuration.png" alt-text="Deaktivieren Sie Dead-Lettering bei Nachrichtenablauf für eine vorhandene Warteschlange":::

### <a name="update-the-dead-lettering-on-message-expiration-setting-for-an-existing-subscription"></a>Aktualisieren Sie die Einstellung für Dead Lettering bei Ablauf der Nachricht für eine vorhandene Abonnement
Auf der Seite **Übersicht** für Ihr Service-Bus-Abonnement wählen Sie den aktuellen Wert für die Einstellung **Dead lettering** bei Ablauf der Nachricht. Im folgenden Beispiel ist der aktuelle Wert **Deaktiviert**. Im Popup-Fenster können Sie Dead Lettering beim Ablauf der Nachricht aktivieren oder deaktivieren. 

:::image type="content" source="./media/enable-dead-letter/subscription-configuration.png" alt-text="Aktivieren des Dead-Lettering bei Nachrichtenablauf für ein bestehendes Abonnement":::

## <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Um **eine Warteschlange mit aktivierter Dead Lettering bei Ablauf der Nachricht zu erstellen**, verwenden Sie den Befehl [`az servicebus queue create`](/cli/azure/servicebus/queue#az_servicebus_queue_create) mit der Einstellung `--enable-dead-lettering-on-message-expiration` auf `true`. 

```azurecli-interactive
az servicebus queue create \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --name myqueue \
    --enable-dead-lettering-on-message-expiration true
```

Um **die Einstellung Dead Lettering bei Ablauf der Nachricht für eine vorhandene Warteschlange** zu aktivieren, verwenden Sie den Befehl [`az servicebus queue update`](/cli/azure/servicebus/queue#az_servicebus_queue_update) mit `--enable-dead-lettering-on-message-expiration` auf `true`. 

```azurecli-interactive
az servicebus queue update \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --name myqueue \
    --enable-dead-lettering-on-message-expiration true
```


Um **eine Themenabonnement mit aktivierter Dead Lettering bei Ablauf der Nachricht zu erstellen**, verwenden Sie den Befehl [`az servicebus topic subscription create`](/cli/azure/servicebus/topic/subscription#az_servicebus_topic_subscription_create) mit der Einstellung `--enable-dead-lettering-on-message-expiration` auf `true`.

```azurecli-interactive
az servicebus topic subscription create \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --topic-name mytopic \
    --name mysubscription \
    --enable-dead-lettering-on-message-expiration true
```

Um **die Einstellung Dead Lettering bei Ablauf der Nachricht für ein Themenabonnement** zu aktivieren, verwenden Sie den Befehl [`az servicebus topic subscription update`](/cli/azure/servicebus/topic/subscription#az_servicebus_topic_subscription_update) mit `--enable-dead-lettering-on-message-expiration` auf `true`.

```azurecli-interactive
az servicebus topic subscription create \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --topic-name mytopic \
    --name mysubscription \
    --enable-dead-lettering-on-message-expiration true
```

## <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell
Um **eine Warteschlange mit aktivierter Dead Lettering bei Ablauf der Nachricht zu erstellen**, verwenden Sie den Befehl [`New-AzServiceBusQueue`](/powershell/module/az.servicebus/new-azservicebusqueue) mit der Einstellung `-DeadLetteringOnMessageExpiration` auf `$True`. 

```azurepowershell-interactive
New-AzServiceBusQueue -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -QueueName myqueue `
    -DeadLetteringOnMessageExpiration $True
```

Um **die Einstellung Dead Lettering bei Ablauf der Nachricht für eine vorhandene Warteschlange** zu aktivieren, verwenden Sie den Befehl [`Set-AzServiceBusQueue`](/powershell/module/az.servicebus/set-azservicebusqueue) wie im folgenden Beispiel gezeigt.

```azurepowershell-interactive
$queue=Get-AzServiceBusQueue -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -QueueName myqueue 

$queue.DeadLetteringOnMessageExpiration=$True

Set-AzServiceBusQueue -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -QueueName myqueue `
    -QueueObj $queue
``` 

Um **ein Abonnement für ein Thema mit aktivierter Dead Lettering bei Ablauf der Nachricht zu erstellen**, verwenden Sie den Befehl [`New-AzServiceBusSubscription`](/powershell/module/az.servicebus/new-azservicebussubscription) mit der Einstellung `-DeadLetteringOnMessageExpiration` auf `$True`. 

```azurepowershell-interactive
New-AzServiceBusSubscription -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -TopicName mytopic `
    -SubscriptionName mysubscription `
    -DeadLetteringOnMessageExpiration $True
```

Um **die Einstellung Dead Lettering bei Ablauf der Nachricht für ein bestehendes Abonnement zu aktivieren**, beachten Sie das folgende Beispiel.

```azurepowershell-interactive
$subscription=Get-AzServiceBusSubscription -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -TopicName mytopic `
    -SubscriptionName mysub

$subscription.DeadLetteringOnMessageExpiration=$True

Set-AzServiceBusSubscription -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -Name mytopic `
    -SubscriptionName mysub `
    -SubscriptionObj $subscription 
```

## <a name="using-azure-resource-manager-template"></a>Verwenden von Azure Resource Manager-Vorlagen
Um **eine Warteschlange zu erstellen, bei der das Schreiben von Dead Lettering bei Ablauf der Nachricht aktiviert ist**, setzen Sie `deadLetteringOnMessageExpiration` im Abschnitt Warteschlangeneigenschaften auf `true`. Weitere Informationen finden Sie unter [Microsoft. ServiceBus Namespaces/Warteschlangen Vorlagenreferenz](/azure/templates/microsoft.servicebus/namespaces/queues?tabs=json). 

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
            "deadLetteringOnMessageExpiration": true
          }
        }
      ]
    }
  ]
}

```

Um **ein Themenabonnement zu erstellen, bei dem das Schreiben von Dead Lettering bei Ablauf der Nachricht aktiviert ist**, setzen Sie `deadLetteringOnMessageExpiration` im Abschnitt Warteschlangeneigenschaften auf `true`. Weitere Informationen finden Sie unter [Vorlagenreferenz für Microsoft.ServiceBus-Namespaces/-Themen/-Abonnements Vorlagenreferenz](/azure/templates/microsoft.servicebus/namespaces/topics/subscriptions?tabs=json). 

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
                "deadLetteringOnMessageExpiration": true
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