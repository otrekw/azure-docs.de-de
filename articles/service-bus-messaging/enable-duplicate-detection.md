---
title: Aktivieren der Erkennung doppelter Nachrichten – Azure Service Bus
description: In diesem Artikel wird erläutert, wie Sie die Erkennung doppelter Nachrichten mit Azure-Portal, PowerShell, CLI und Programmiersprachen (C#, Java, Python und JavaScript) aktivieren
ms.topic: how-to
ms.date: 04/19/2021
ms.openlocfilehash: 708009fcf2479660316b38ac0b7d545d450de28c
ms.sourcegitcommit: 6686a3d8d8b7c8a582d6c40b60232a33798067be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107755058"
---
# <a name="enable-duplicate-message-detection-for-an-azure-service-bus-queue-or-a-topic"></a>Aktivieren der Erkennung doppelter Nachrichten für eine Azure Service Bus-Warteschlange oder ein Thema
Wenn Sie die Duplikaterkennung für eine Warteschlange oder ein Thema aktivieren, speichert Azure Service Bus einen Verlauf aller Nachrichten, die an die Warteschlange oder das Thema gesendet werden, für einen konfigurierten Zeitraum. In diesem Intervall werden in der Warteschlange oder dem Thema keine doppelten Nachrichten gespeichert. Wenn Sie diese Eigenschaft aktivieren, wird die Übermittlung genau einmal über einen benutzerdefinierten Zeitraum garantiert. Weitere Informationen finden Sie unter [Duplikaterkennung](duplicate-detection.md). In diesem Artikel werden verschiedene Möglichkeiten zum Aktivieren der Erkennung doppelter Nachrichten für eine Service Bus-Warteschlange oder ein Thema beschrieben. 


> [!NOTE]
> - Der Basic-Tarif von Service Bus unterstützt keine Duplikaterkennung. Die Standard- und Premium-Tarife unterstützen Duplikaterkennung. Informationen zu den Unterschieden zwischen diesen Tarifen finden Sie unter [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/).
> - Sie können die Duplikaterkennung nach der Erstellung der Warteschlange oder des Themas nicht aktivieren oder deaktivieren. Dies ist nur zum Zeitpunkt der Erstellung der Warteschlange oder des Themas möglich. 

## <a name="using-azure-portal"></a>Verwenden des Azure-Portals
Wenn Sie eine **Warteschlange** im Azure-Portal erstellen, wählen Sie **Duplikaterkennung aktivieren** aus, wie in der folgenden Abbildung dargestellt. Sie können die Größe des Duplikaterkennungsfensters beim Erstellen einer Warteschlange oder eines Themas konfigurieren. 

:::image type="content" source="./media/enable-duplicate-detection/create-queue.png" alt-text="Aktivieren der Duplikaterkennung zum Zeitpunkt der Erstellung der Warteschlange":::

Wenn Sie ein Thema im Azure-Portal erstellen, wählen Sie **Duplikaterkennung aktivieren** aus, wie in der folgenden Abbildung dargestellt. 

:::image type="content" source="./media/enable-duplicate-detection/create-topic.png" alt-text="Aktivieren der Duplikaterkennung zum Zeitpunkt der Erstellung des Themas":::

Sie können diese Einstellung auch für eine vorhandene Warteschlange oder ein vorhandenes Thema konfigurieren, wenn Sie zum Zeitpunkt der Erstellung die Duplikaterkennung aktiviert haben. 

### <a name="update-duplicate-detection-window-size-for-an-existing-queue-or-a-topic"></a>Aktualisieren der Fenstergröße für die Duplikaterkennung für eine vorhandene Warteschlange oder ein Thema
Um die Größe des Duplikaterkennungsfensters für eine vorhandene Warteschlange oder ein Thema zu ändern, wählen Sie auf der **Übersichtsseite** die Option **Ändern** für **Duplikaterkennungsfenster** aus.  

#### <a name="queue"></a>Warteschlange
:::image type="content" source="./media/enable-duplicate-detection/window-size.png" alt-text="Festlegen der Größe des Duplikaterkennungsfensters für eine Warteschlange":::

#### <a name="topic"></a>Thema
:::image type="content" source="./media/enable-duplicate-detection/window-size-topic.png" alt-text="Festlegen der Größe des Duplikaterkennungsfensters für ein Thema":::


## <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Um **eine Warteschlange mit aktivierter Duplikaterkennung** zu erstellen, verwenden Sie den [`az servicebus queue create`](/cli/azure/servicebus/queue#az_servicebus_queue_create)-Befehl , bei dem `--enable-duplicate-detection` auf `true` festgelegt ist. 

```azurecli-interactive
az servicebus queue create \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --name myqueue \
    --enable-duplicate-detection true \
    --duplicate-detection-history-time-window P1D
```

Um **ein Thema mit aktivierter Duplikaterkennung** zu erstellen, verwenden Sie den [`az servicebus topic create`](/cli/azure/servicebus/topic#az_servicebus_topic_create)-Befehl , bei dem `--enable-duplicate-detection` auf `true` festgelegt ist.

```azurecli-interactive
az servicebus topic create \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --name mytopic \
    --enable-duplicate-detection true \
    --duplicate-detection-history-time-window P1D
```

In den obigen Beispielen wird auch die Größe des Duplikaterkennungsfensters mithilfe des `--duplicate-detection-history-time-window`-Parameters festgelegt. Die Fenstergröße ist auf einen Tag festgelegt. Der Standardwert beträgt 10 Minuten, und der maximal zulässige Wert beträgt sieben Tage.

Um **eine Warteschlange mit einer neuen Erkennungsfenstergröße** z u aktualisieren, verwenden Sie den [`az servicebus queue update`](/cli/azure/servicebus/queue#az_servicebus_queue_update)-Befehl mit dem `--duplicate-detection-history-time-window`-Parameter. In diesem Beispiel wird die Fenstergröße auf sieben Tage aktualisiert. 

```azurecli-interactive
az servicebus queue update \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --name myqueue \
    --duplicate-detection-history-time-window P7D
```

Ähnlich wie beim Vorgang **eine Warteschlange mit einer neuen Erkennungsfenstergröße aktualisieren**, verwenden Sie den [`az servicebus topic update`](/cli/azure/servicebus/topic#az_servicebus_topic_update)-Befehl mit dem `--duplicate-detection-history-time-window`-Parameter. In diesem Beispiel wird die Fenstergröße auf sieben Tage aktualisiert.

```azurecli-interactive
az servicebus topic update \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --name myqueue \
    --duplicate-detection-history-time-window P7D
```
 
## <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell
Um **eine Warteschlange mit aktivierter Duplikaterkennung** zu erstellen, verwenden Sie den [`New-AzServiceBusQueue`](/powershell/module/az.servicebus/new-azservicebusqueue)-Befehl , bei dem `-RequiresDuplicateDetection` auf `$True` festgelegt ist. 

```azurepowershell-interactive
New-AzServiceBusQueue -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -QueueName myqueue `
    -RequiresDuplicateDetection $True `
    -DuplicateDetectionHistoryTimeWindow P1D
```

Um **ein Thema mit aktivierter Duplikaterkennung** zu erstellen, verwenden Sie den [`New-AzServiceBusTopic`](/powershell/module/az.servicebus/new-azservicebustopic)-Befehl , bei dem `-RequiresDuplicateDetection` auf `true` festgelegt ist. 

```azurepowershell-interactive
New-AzServiceBusTopic -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -Name mytopic `
    -RequiresDuplicateDetection $True
    -DuplicateDetectionHistoryTimeWindow P1D
```

In den obigen Beispielen wird auch die Größe des Duplikaterkennungsfensters mithilfe des `-DuplicateDetectionHistoryTimeWindow`-Parameters festgelegt. Die Fenstergröße ist auf einen Tag festgelegt. Der Standardwert beträgt 10 Minuten, und der maximal zulässige Wert beträgt sieben Tage.

Informationen zum **eine Warteschlange mit einer neuen Erkennungsfenstergröße aktualisieren** finden Sie im folgenden Beispiel.  In diesem Beispiel wird die Fenstergröße auf sieben Tage aktualisiert.

```azurepowershell-interactive
$queue=Get-AzServiceBusQueue -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -QueueName myqueue 

$queue.DuplicateDetectionHistoryTimeWindow='P7D'

Set-AzServiceBusQueue -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -QueueName myqueue `
    -QueueObj $queue
```

Informationen zum **ein Thema mit einer neuen Erkennungsfenstergröße aktualisieren** finden Sie im folgenden Beispiel. In diesem Beispiel wird die Fenstergröße auf sieben Tage aktualisiert.

```azurepowershell-interactive
$topic=Get-AzServiceBusTopic -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -Name mytopic

$topic.DuplicateDetectionHistoryTimeWindow='P7D'

Set-AzServiceBusTopic -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -Name mytopic `
    -TopicObj $topic
```

## <a name="using-azure-resource-manager-template"></a>Verwenden von Azure Resource Manager-Vorlagen
Um **eine Warteschlange mit aktivierter Duplikaterkennung zu erstellen**, stellen Sie im Abschnitt Warteschlangeneigenschaften `requiresDuplicateDetection` auf `true`. Weitere Informationen finden Sie unter [Microsoft.ServiceBus namespaces/queues template reference](/azure/templates/microsoft.servicebus/namespaces/queues?tabs=json). Geben Sie einen Wert für die `duplicateDetectionHistoryTimeWindow`-Eigenschaft an, um die Größe des Duplikaterkennungsfensters festzulegen. Im folgenden Beispiel ist sie auf einen Tag festgelegt.  

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
            "requiresDuplicateDetection": true,
            "duplicateDetectionHistoryTimeWindow": "P1D"
          }
        }
      ]
    }
  ]
}

```

Um **ein Thema mit aktivierter Duplikaterkennung zu erstellen**, stellen Sie im Abschnitt Themeneigenschaften `requiresDuplicateDetection` auf `true`. Weitere Informationen finden Sie unter [Microsoft.ServiceBus namespaces/topics template reference](/azure/templates/microsoft.servicebus/namespaces/topics?tabs=json). 

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
            "requiresDuplicateDetection": true,
            "duplicateDetectionHistoryTimeWindow": "P1D"
          }
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
- [Azure-ServiceBus-Beispiele für Java](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/azure-servicebus/MessageBrowse)