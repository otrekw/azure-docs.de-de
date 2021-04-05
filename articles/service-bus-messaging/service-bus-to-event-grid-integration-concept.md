---
title: Übersicht über die Integration von Azure Service Bus in Event Grid | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Beschreibung der Integration von Azure Service Bus-Messaging in Azure Event Grid.
documentationcenter: .net
author: spelluru
ms.topic: conceptual
ms.date: 02/11/2021
ms.author: spelluru
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 658107bb74396891c8e6e05a9e8074a9416a5f6f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100369661"
---
# <a name="azure-service-bus-to-event-grid-integration-overview"></a>Übersicht über die Integration von Azure Service Bus in Event Grid
Service Bus kann nun Ereignisse an Event Grid ausgeben, wenn Nachrichten in einer Warteschlange oder in einem Abonnement vorliegen und keine Empfänger vorhanden sind. Sie können Event Grid-Abonnements für Ihre Service Bus-Namespaces erstellen, auf diese Ereignisse lauschen und als Reaktion auf die Ereignisse einen Empfänger starten. Mit diesem Feature können Sie Service Bus in reaktiven Programmiermodellen verwenden. Die zentrale Bedeutung dieses Features besteht in einem Szenario, in dem Service Bus-Warteschlangen oder Abonnements mit einer geringen Anzahl von Nachrichten keinen Empfänger benötigen, der kontinuierlich Nachrichten abfragt. 

Um das Feature aktivieren zu können, benötigen Sie Folgendes:

* Einen Service Bus Premium-Namespace mit mindestens einer Service Bus-Warteschlange oder ein Service Bus-Thema mit mindestens einem Abonnement
* Zugriff für Mitwirkende auf den Service Bus-Namespace Navigieren Sie zu Ihrem Service Bus-Namespace im Azure-Portal, und wählen Sie **Zugriffssteuerung (IAM)** und dann die Registerkarte **Rollenzuweisungen** aus. Vergewissern Sie sich, dass Sie über Zugriff für Mitwirkende auf den Namespace verfügen. 
* Außerdem benötigen Sie ein Event Grid-Abonnement für den Service Bus-Namespace. Dieses Abonnement erhält eine Benachrichtigung von Event Grid, wenn abzurufende Nachrichten verfügbar sind. Zu den typischen Abonnenten zählen beispielsweise das Logic Apps-Feature von Azure App Service, Azure Functions oder ein Webhook, der eine Web-App kontaktiert. Die Nachrichten werden dann vom Abonnenten verarbeitet. 

![19][]

[!INCLUDE [event-grid-service-bus.md](../../includes/event-grid-service-bus.md)]

## <a name="event-grid-subscriptions-for-service-bus-namespaces"></a>Event Grid-Abonnements für Service Bus-Namespaces
Event Grid-Abonnements für Service Bus-Namespaces können auf drei Arten erstellt werden:

- Azure-Portal. In den folgenden Tutorials erfahren Sie, wie Sie im Azure-Portal Event Grid-Abonnements für Service Bus-Ereignisse mit Azure Logic-Apps und Azure Functions als Handler erstellen. 
    - [Azure Logic Apps](service-bus-to-event-grid-integration-example.md#receive-messages-by-using-logic-apps)
    - [Azure-Funktionen](service-bus-to-event-grid-integration-function.md#connect-the-function-and-namespace-via-event-grid)
* Azure-Befehlszeilenschnittstelle. Im folgenden CLI-Beispiel wird gezeigt, wie ein Azure Functions-Abonnement für ein [Systemthema](../event-grid/system-topics.md) erstellt wird, das von einem Service Bus-Namespace erstellt wird.

     ```azurecli-interactive
    namespaceid=$(az resource show --namespace Microsoft.ServiceBus --resource-type namespaces --name "<service bus namespace>" --resource-group "<resource group that contains the service bus namespace>" --query id --output tsv
    
    az eventgrid event-subscription create --resource-id $namespaceid --name "<YOUR EVENT GRID SUBSCRIPTION NAME>" --endpoint "<your_endpoint_url>" --subject-ends-with "<YOUR SERVICE BUS SUBSCRIPTION NAME>"
    ```
- PowerShell. Hier sehen Sie ein Beispiel:
    ```powershell-interactive
    $namespaceID = (Get-AzServiceBusNamespace -ResourceGroupName "<YOUR RESOURCE GROUP NAME>" -NamespaceName "<YOUR NAMESPACE NAME>").Id
    
    New-AzEVentGridSubscription -EventSubscriptionName "<YOUR EVENT GRID SUBSCRIPTION NAME>" -ResourceId $namespaceID -Endpoint "<YOUR ENDPOINT URL>” -SubjectEndsWith "<YOUR SERVICE BUS SUBSCRIPTION NAME>"
    ```
### <a name="how-many-events-are-emitted-and-how-often"></a>Wie viele Ereignisse werden ausgegeben, und wie oft?

Wenn der Namespace mehrere Warteschlangen und Themen oder Abonnements enthält, erhalten Sie mindestens ein Ereignis pro Warteschlange und ein Ereignis pro Abonnement. Die Ereignisse werden sofort ausgegeben, wenn in der Service Bus-Entität keine Nachrichten vorhanden sind und eine neue Nachricht eingeht. Andernfalls werden Ereignisse alle zwei Minuten ausgegeben – es sei denn, Service Bus erkennt einen aktiven Empfänger. Durch das Einsehen von Nachrichten werden die Ereignisse nicht unterbrochen.

Standardmäßig gibt Service Bus Ereignisse für alle Entitäten im Namespace aus. Wenn Sie nur Ereignisse für bestimmte Entitäten abrufen möchten, lesen Sie den folgenden Abschnitt.

### <a name="use-filters-to-limit-where-you-get-events-from"></a>Verwenden von Filtern, um einzuschränken, von wo Ereignisse abgerufen werden

Wenn Sie beispielsweise nur Ereignisse aus einer bestimmten Warteschlange oder aus einem bestimmten Abonnement innerhalb Ihres Namespace abrufen möchten, können Sie den Event Grid-Filter *Beginnt mit* oder *Endet mit* verwenden. Bei einigen Schnittstellen werden die Filter als *Präfix* und *Suffix* bezeichnet. Wenn Sie Ereignisse für mehrere (aber nicht für alle) Warteschlangen und Abonnements abrufen möchten, können Sie mehrere Event Grid-Abonnements erstellen und diese jeweils mit einem Filter versehen.

## <a name="next-steps"></a>Nächste Schritte
Arbeiten Sie die folgenden Tutorials durch: 
- [Empfangen von Nachrichten mithilfe von Logik-Apps](service-bus-to-event-grid-integration-example.md#receive-messages-by-using-logic-apps)
- [Verbinden der Funktion und des Namespace per Event Grid](service-bus-to-event-grid-integration-function.md#connect-the-function-and-namespace-via-event-grid)

[1]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgrid1.png
[19]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgriddiagram.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png
