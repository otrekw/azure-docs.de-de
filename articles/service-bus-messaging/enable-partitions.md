---
title: Aktivieren der Partitionierung in Azure Service Bus-Warteschlangen und -Themen
description: In diesem Artikel wird erläutert, wie Sie die Partitionierung in Azure Service Bus-Warteschlangen und -Themen mithilfe von Azure-Portal, PowerShell, CLI und Programmiersprachen (C#, Java, Python und JavaScript) aktivieren
ms.topic: how-to
ms.date: 04/19/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 786a78cfa86c67ba32675fcee2b03c4e3817fbf6
ms.sourcegitcommit: df574710c692ba21b0467e3efeff9415d336a7e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110671390"
---
# <a name="enable-partitioning-for-an-azure-service-bus-queue-or-a-topic"></a>Aktivieren der Partitionierung für eine Azure Service Bus-Warteschlange oder ein -Thema
Service Bus-Partitionen ermöglichen das Partitionieren von Warteschlangen und Themen oder Nachrichtenentitäten über mehrere Nachrichtenbroker und -speicher. Eine Partitionierung bedeutet, dass der Gesamtdurchsatz einer partitionierten Entität nicht mehr durch die Leistung eines einzelnen Nachrichtenbrokers oder Nachrichtenspeichers beschränkt wird. Außerdem führt ein vorübergehender Ausfall eines Nachrichtenspeichers nicht dazu, dass eine partitionierte Warteschlange oder ein Thema nicht verfügbar ist. Partitionierte Warteschlangen und Themen können alle erweiterten Service Bus-Features enthalten, z. B. die Unterstützung von Transaktionen und Sitzungen. Weitere Informationen finden Sie unter [partitionierte Warteschlangen und Themen](service-bus-partitioning.md). In diesem Artikel werden verschiedene Möglichkeiten zum Aktivieren der Erkennung doppelter Nachrichten für eine Service Bus-Warteschlange oder ein Thema beschrieben. 

> [!IMPORTANT]
> - Die Partitionierung ist bei der Erstellung der Entität für alle Warteschlangen und Themen in den SKUs „Basic“ oder „Standard“ verfügbar. Für die Premium-Messaging-SKU ist sie nicht verfügbar, aber alle bereits vorhandenen partitionierten Entitäten in Premium-Namespaces funktionieren weiterhin wie erwartet.
> - Es ist nicht möglich, die Partitionierungsoption für eine vorhandene Warteschlange oder ein Thema zu ändern. Sie können die Option nur festlegen, wenn Sie eine Warteschlange oder ein Thema erstellen. 
> - In einem Namespace des **Standard**-Tarifs können Sie Service Bus-Warteschlangen und -Themen in Größen von 1, 2, 3, 4 oder 5 GB erstellen (die Standardgröße ist 1 GB). Bei aktivierter Partitionierung erstellt Service Bus 16 Kopien (16 Partitionen) der Entität mit der jeweils angegebenen Größe. Wenn Sie also eine Warteschlange mit einer Größe von 5 GB erstellen, beträgt die maximale Warteschlangengröße bei 16 Partitionen 5 \* 16 = 80 GB. 
> - In einem Namespace des **Premium**-Tarifs werden Partitionierungsentitäten nicht unterstützt. Sie können jedoch weiterhin Service Bus-Warteschlangen und -Themen in Größen von 1, 2, 3, 4, 5, 10, 20, 40 oder 80 GB erstellen (die Standardgröße ist 1 GB). Die maximale Größe der partitionierten Warteschlange oder des Themas wird auf der Seite [Übersicht](https://portal.azure.com) im **Azure-Portal** angezeigt.


## <a name="using-azure-portal"></a>Verwenden des Azure-Portals
Wenn Sie eine **Warteschlange** im Azure-Portal erstellen, wählen Sie **Partitionierung aktivieren** aus, wie in der folgenden Abbildung dargestellt. 

:::image type="content" source="./media/enable-partitions/create-queue.png" alt-text="Aktivieren der Partitionierung zum Zeitpunkt der Erstellung der Warteschlange":::

Wenn Sie ein Thema im Azure-Portal erstellen, wählen Sie **Partitionierung aktivieren** aus, wie in der folgenden Abbildung dargestellt. 

:::image type="content" source="./media/enable-partitions/create-topic.png" alt-text="Aktivieren der Partitionierung zum Zeitpunkt der Erstellung des Themas":::

## <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Um **eine Warteschlange mit aktivierter Partitionierung zu erstellen,** verwenden Sie den [`az servicebus queue create`](/cli/azure/servicebus/queue#az_servicebus_queue_create)-Befehl , bei dem `--enable-partitioning` auf `true` festgelegt ist.

```azurecli-interactive
az servicebus queue create \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --name myqueue \
    --enable-partitioning true
```

Um **ein Thema mit aktivierter Partitionierung zu erstellen,** verwenden Sie den [`az servicebus topic create`](/cli/azure/servicebus/topic#az_servicebus_topic_create)-Befehl , bei dem `--enable-partitioning` auf `true` festgelegt ist.

```azurecli-interactive
az servicebus topic create \
    --resource-group myresourcegroup \
    --namespace-name mynamespace \
    --name mytopic \
    --enable-partitioning true
```

## <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell
Um **eine Warteschlange mit aktivierter Partitionierung zu erstellen,** verwenden Sie den [`New-AzServiceBusQueue`](/powershell/module/az.servicebus/new-azservicebusqueue)-Befehl , bei dem `-EnablePartitioning` auf `$True` festgelegt ist. 

```azurepowershell-interactive
New-AzServiceBusQueue -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -QueueName myqueue `
    -EnablePartitioning $True
```

Um **ein Thema mit aktivierter Partitionierung zu erstellen,** verwenden Sie den [`New-AzServiceBusTopic`](/powershell/module/az.servicebus/new-azservicebustopic)-Befehl, bei dem `-EnablePartitioning` auf `true` festgelegt ist. 

```azurepowershell-interactive
New-AzServiceBusTopic -ResourceGroup myresourcegroup `
    -NamespaceName mynamespace `
    -Name mytopic `
    -EnablePartitioning $True
```

## <a name="using-azure-resource-manager-template"></a>Verwenden von Azure Resource Manager-Vorlagen
Um **eine Warteschlange mit aktivierter Partitionierung zu erstellen**, stellen Sie im Abschnitt Warteschlangeneigenschaften `enablePartitioning` auf `true`. Weitere Informationen finden Sie unter [Microsoft.ServiceBus namespaces/queues template reference](/azure/templates/microsoft.servicebus/namespaces/queues?tabs=json). 

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
            "enablePartitioning": true
          }
        }
      ]
    }
  ]
}

```

Um **ein Thema mit aktivierter Duplikaterkennung zu erstellen**, stellen Sie im Abschnitt Themeneigenschaften `enablePartitioning` auf `true`. Weitere Informationen finden Sie unter [Microsoft.ServiceBus namespaces/topics template reference](/azure/templates/microsoft.servicebus/namespaces/topics?tabs=json). 

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
            "enablePartitioning": true
          }
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
