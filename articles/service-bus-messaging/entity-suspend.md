---
title: 'Azure Service Bus: Anhalten von Messagingentitäten'
description: In diesem Artikel wird erläutert, wie Sie Azure Service Bus-Messagingentitäten (Warteschlangen, Themen und Abonnements) vorübergehend anhalten und reaktivieren.
ms.topic: article
ms.date: 09/29/2020
ms.openlocfilehash: f89e17e494cc777691b7f7ca47538cd29114d2dc
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2020
ms.locfileid: "91575237"
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>Anhalten und Reaktivieren von Messagingentitäten (deaktiviert)

Warteschlangen, Themen und Abonnements können vorübergehend angehalten werden. Das Anhalten versetzt die Entität in einen deaktivierten Zustand, in dem alle Nachrichten im Speicher beibehalten werden. Es können jedoch keine Nachrichten entfernt oder hinzugefügt werden, und die entsprechenden Protokollvorgänge führen zu Fehlern.

Das Anhalten einer Entität wird üblicherweise aus dringenden administrativen Gründen durchgeführt. Ein Szenario ist, dass ein fehlerhafter Empfänger bereitgestellt wurde, der Nachrichten aus der Warteschlange nimmt, diese fehlerhaft verarbeitet, falsch vervollständigt und schließlich entfernt. Wenn dieses Verhalten diagnostiziert wird, kann die Warteschlange für den Empfang deaktiviert werden, bis der korrigierte Code bereitgestellt ist, sodass weiterer Datenverlust durch den fehlerhaften Code verhindert werden kann.

Das Anhalten oder Reaktivieren kann entweder vom Benutzer oder vom System durchgeführt werden. Das System hält Entitäten nur aufgrund von schwerwiegenden administrativen Gründen an, z.B. das Erreichen des Ausgabenlimits des Abonnements. Vom System deaktivierte Entitäten können nicht vom Benutzer reaktiviert werden, sondern werden wiederhergestellt, wenn die Ursache des Anhaltens behandelt wurde.

## <a name="queue-status"></a>Warteschlangenstatus 
Folgende Zustände können für eine Warteschlange festgelegt werden:

-   **Aktiv:** Die Warteschlange ist aktiv.
-   **Disabled**: Die Warteschlange wurde angehalten. Dies entspricht dem Festlegen von **SendDisabled** und **ReceiveDisabled**. 
-   **SendDisabled**: Die Warteschlange ist teilweise angehalten, der Empfang ist zulässig.
-   **ReceiveDisabled**: Die Warteschlange ist teilweise angehalten, das Senden ist zulässig.

### <a name="change-the-queue-status-in-the-azure-portal"></a>Ändern des Warteschlangenstatus im Azure-Portal: 

1. Navigieren Sie im Azure-Portal zu Ihrem Service Bus-Namespace. 
1. Wählen Sie die Warteschlange aus, deren Status Sie ändern möchten. Die Warteschlangen werden im unteren Bereich in der Mitte angezeigt. 
1. Auf der Seite **Service Bus-Warteschlange** wird der aktuelle Status der Warteschlange als Hyperlink angezeigt. Wenn die **Übersicht** nicht im linken Menü ausgewählt ist, wählen Sie diese Option aus, um den Status der Warteschlange anzuzeigen. Wählen Sie den aktuellen Status der Warteschlange aus, um ihn zu ändern. 

    :::image type="content" source="./media/entity-suspend/select-state.png" alt-text="Auswählen des Warteschlangenstatus":::
4. Wählen Sie den neuen Status für die Warteschlange und dann **OK** aus. 

    :::image type="content" source="./media/entity-suspend/entity-state-change.png" alt-text="Auswählen des Warteschlangenstatus":::
    
Das Portal ermöglicht nur das vollständige Deaktivieren von Warteschlangen. Sie können die Vorgänge „Senden“ und „Empfangen“ auch separat deaktivieren, indem Sie die [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager)-APIs von Service Bus im .NET Framework SDK oder mit einer Azure Resource Manager-Vorlage über die Azure CLI oder Azure PowerShell verwenden.

### <a name="change-the-queue-status-using-azure-powershell"></a>Ändern des Warteschlangenstatus mithilfe von Azure PowerShell
Der PowerShell-Befehl zum Deaktivieren einer Warteschlange wird im folgenden Beispiel gezeigt. Der Befehl für die Reaktivierung ist identisch und legt `Status` auf **Active** fest.

```powershell
$q = Get-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="topic-status"></a>Themenstatus
Das Ändern des Themenstatus im Azure-Portal ähnelt dem Ändern des Status einer Warteschlange. Wenn Sie den aktuellen Status eines Themas auswählen, wird die folgende Seite angezeigt, auf der Sie den Status ändern können. 

:::image type="content" source="./media/entity-suspend/topic-state-change.png" alt-text="Auswählen des Warteschlangenstatus":::

Folgende Zustände können für ein Thema festgelegt werden:
- **Aktiv:** Das Thema ist aktiv.
- **Disabled**: Das Thema wurde angehalten.
- **SendDisabled**: Dieselbe Auswirkung wie **Deaktiviert**.

## <a name="subscription-status"></a>Abonnementstatus
Das Ändern des Abonnementstatus im Azure-Portal ähnelt dem Ändern des Status eines Themas oder einer Warteschlange. Wenn Sie den aktuellen Status eines Abonnements auswählen, wird die folgende Seite angezeigt, auf der Sie den Status ändern können. 

:::image type="content" source="./media/entity-suspend/subscription-state-change.png" alt-text="Auswählen des Warteschlangenstatus":::

Folgende Zustände können für ein Thema festgelegt werden:
- **Aktiv:** Das Thema ist aktiv.
- **Disabled**: Das Thema wurde angehalten.
- **ReceiveDisabled**: Dieselbe Auswirkung wie **Deaktiviert**.

## <a name="other-statuses"></a>Andere Status
Die Enumeration [EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) definiert ebenfalls eine Reihe von Übergangszuständen, die nur vom System festgelegt werden können. 


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Service Bus-Messaging finden Sie in folgenden Themen:

* [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Verwenden von Service Bus-Themen und -Abonnements](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/entity-state-change.png

