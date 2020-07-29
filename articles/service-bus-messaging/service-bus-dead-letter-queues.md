---
title: Service Bus-Warteschlangen für unzustellbare Nachrichten | Microsoft Docs
description: Beschreibung von Warteschlangen für unzustellbare Nachrichten in Azure Service Bus Service Bus-Warteschlangen und -Themenabonnements verfügen über eine sekundäre Unterwarteschlange, die als „Warteschlange für unzustellbare Nachrichten“ bezeichnet wird.
ms.topic: article
ms.date: 06/23/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: 7078a7889947c4121713e9374d1487f408fed871
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86511210"
---
# <a name="overview-of-service-bus-dead-letter-queues"></a>Übersicht über Service Bus-Warteschlangen für unzustellbare Nachrichten

Azure Service Bus-Warteschlangen und -Themenabonnements verfügen über eine sekundäre Unterwarteschlange, die als *Warteschlange für unzustellbare Nachrichten* (Dead Letter Queue, DLQ) bezeichnet wird. Die Warteschlange für unzustellbare Nachrichten muss nicht explizit erstellt und kann nicht gelöscht oder anderweitig unabhängig von der Hauptentität verwaltet werden.

Dieser Artikel erläutert Warteschlangen für unzustellbare Nachrichten in Service Bus. Ein Großteil der Informationen wird durch das Beispiel [Dead-Letter Queues](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/DeadletterQueue) auf GitHub veranschaulicht.
 
## <a name="the-dead-letter-queue"></a>Die Warteschlange für unzustellbare Nachrichten

Die Warteschlange für unzustellbare Nachrichten dient zum Speichern von Nachrichten, die an keinen Empfänger übermittelt oder nicht verarbeitet werden können. Nachrichten können dann aus dieser Warteschlange entfernt und überprüft werden. Eine Anwendung kann mithilfe eines Operators Probleme beheben und die Nachricht erneut senden, den Fehler protokollieren und korrigierende Maßnahmen ergreifen. 

Im Hinblick auf API und Protokoll ähnelt die Warteschlange für unzustellbare Nachrichten weitgehend anderen Warteschlangen, mit der Ausnahme, dass Nachrichten nur durch die übergeordnete Entität über den Vorgang für unzustellbare Nachrichten gesendet werden können. Darüber hinaus ist die Gültigkeitsdauer nicht relevant, und eine Nachricht kann nicht von einer Warteschlange für unzustellbare Nachrichten aus als unzustellbar gekennzeichnet werden. Die Warteschlange für unzustellbare Nachrichten bietet vollständige Unterstützung für die Peek/Lock-Übermittlung und Transaktionsvorgänge.

Es wird keine automatische Bereinigung der Warteschlange für unzustellbare Nachrichten durchgeführt. Nachrichten verbleiben in dieser Warteschlange, bis Sie sie explizit daraus abrufen und für die unzustellbare Nachricht [Complete()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) aufrufen.

## <a name="dlq-message-count"></a>Nachrichtenanzahl der Warteschlange für unzustellbare Nachrichten
Es ist nicht möglich, auf Themenebene die Anzahl von Nachrichten in der Warteschlange für unzustellbare Nachrichten abzurufen. Der Grund ist, dass sich die Nachrichten nicht auf Themenebene befinden, sofern von Service Bus kein interner Fehler ausgelöst wird. Wenn ein Absender eine Nachricht an ein Thema sendet, wird diese stattdessen innerhalb von Millisekunden an Abonnements für das Thema weitergeleitet und ist daher nicht mehr auf der Themenebene vorhanden. Sie können Nachrichten in der Warteschlange für unzustellbare Nachrichten anzeigen, die dem Abonnement für das Thema zugeordnet ist. Im folgenden Beispiel wird im **Service Bus Explorer** angegeben, dass sich in der Warteschlange für unzustellbare Nachrichten für das Abonnement „test1“ derzeit 62 Nachrichten befinden. 

![Nachrichtenanzahl der Warteschlange für unzustellbare Nachrichten](./media/service-bus-dead-letter-queues/dead-letter-queue-message-count.png)

Sie können die Anzahl von Nachrichten in der Warteschlange für unzustellbare Nachrichten auch abrufen, indem Sie den folgenden Azure CLI-Befehl verwenden: [`az servicebus topic subscription show`](/cli/azure/servicebus/topic/subscription?view=azure-cli-latest#az-servicebus-topic-subscription-show). 

## <a name="moving-messages-to-the-dlq"></a>Verschieben von Nachrichten in die Warteschlange für unzustellbare Nachrichten

Es gibt verschiedene Aktivitäten in Service Bus, durch welche Nachrichten aus dem Messaging-Engine selbst per Push in die Warteschlange für unzustellbare Nachrichten übertragen werden. Eine Anwendung kann Nachrichten auch explizit in die Warteschlange für unzustellbare Nachrichten verschieben. 

Während die Nachricht vom Broker verschoben wird, werden ihr zwei Eigenschaften hinzugefügt, wenn der Broker die interne Version der [DeadLetter](/dotnet/api/microsoft.azure.servicebus.queueclient.deadletterasync)-Methode für die Nachricht aufruft: `DeadLetterReason` und `DeadLetterErrorDescription`.

Anwendungen können ihre eigenen Codes für die `DeadLetterReason`-Eigenschaft definieren, aber das System legt die folgenden Werte fest.

| DeadLetterReason | DeadLetterErrorDescription |
| --- | --- |
|HeaderSizeExceeded |Das Größenkontingent für diesen Datenstrom wurde überschritten. |
|TTLExpiredException |Die Nachricht ist abgelaufen und wurde in die Warteschlange für unzustellbare Nachrichten verschoben. Weitere Informationen finden Sie im Abschnitt [Überschreiten von TimeToLive](#exceeding-timetolive). |
|Sitzungs-ID ist NULL. |Die für die Sitzung aktivierte Entität erlaubt keine Nachricht, deren Sitzungsbezeichner null ist. |
|MaxTransferHopCountExceeded | Die maximale Anzahl zulässiger Hops beim Weiterleiten zwischen Warteschlangen. Der Wert ist auf 4 festgelegt. |
| MaxDeliveryCountExceededExceptionMessage | Die Nachricht konnte nach der maximalen Anzahl der Übermittlungsversuche nicht verarbeitet werden. Weitere Informationen finden Sie im Abschnitt [Überschreiten von MaxDeliveryCount](#exceeding-maxdeliverycount). |

## <a name="exceeding-maxdeliverycount"></a>Überschreiten von MaxDeliveryCount

Warteschlangen und Abonnements haben die Eigenschaft [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) bzw. [SubscriptionDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.maxdeliverycount). Der jeweilige Standardwert ist 10. Jedes Mal, wenn eine Nachricht unter einer Sperre übermittelt wurde ([ReceiveMode.PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode)), aber entweder explizit abgebrochen wurde oder die Sperre abgelaufen ist, wird der Wert [BrokeredMessage.DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) für die Nachricht erhöht. Wenn [DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) den Wert [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) überschreitet, wird die Nachricht in die Warteschlange für unzustellbare Nachrichten verschoben, und der Ursachencode `MaxDeliveryCountExceeded` wird angegeben.

Dieses Verhalten kann nicht deaktiviert werden, aber Sie können [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) auf eine hohe Zahl festlegen.

## <a name="exceeding-timetolive"></a>Überschreiten von TimeToLive

Wenn die Eigenschaft [QueueDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription) bzw. [SubscriptionDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) auf **TRUE** festgelegt ist (die Standardeinstellung lautet **FALSE**), werden alle ablaufenden Nachrichten in die Warteschlange für unzustellbare Nachrichten verschoben, und der Ursachencode `TTLExpiredException` wird angegeben.

Abgelaufene Nachrichten werden nur bereinigt und in die Warteschlange für unzustellbare Nachrichten verschoben, wenn mindestens ein aktiver Empfänger per Pull aus der Hauptwarteschlange oder dem Abonnement abruft. [Verzögerte Nachrichten](./message-deferral.md) werden nach ihrem Ablauf ebenfalls nicht bereinigt und in die Warteschlange für unzustellbare Nachrichten verschoben. Dieses Verhalten ist beabsichtigt.

## <a name="errors-while-processing-subscription-rules"></a>Fehler beim Verarbeiten von Regeln für Abonnements

Wenn die [SubscriptionDescription.EnableDeadLetteringOnFilterEvaluationExceptions](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription)-Eigenschaft für ein Abonnement aktiviert ist, werden Fehler, die während der Ausführung der SQL-Filterregel eines Abonnements auftreten, zusammen mit der betreffenden Nachricht in der Warteschlange für unzustellbare Nachrichten erfasst.

## <a name="application-level-dead-lettering"></a>Unzustellbare Nachrichten auf Anwendungsebene

Zusätzlich zu den vom System bereitgestellten Features für unzustellbare Nachrichten können Anwendungen die Warteschlange für unzustellbare Nachrichten verwenden, um nicht annehmbare Nachrichten explizit abzulehnen. Hierzu zählen beispielsweise Nachrichten, die aufgrund eines bestimmten Systemproblems nicht richtig verarbeitet werden können, Nachrichten mit fehlerhaften Nutzlasten oder Nachrichten, bei denen Authentifizierungsfehler auftreten, wenn ein Sicherheitsschema auf Nachrichtenebene verwendet wird.

## <a name="dead-lettering-in-forwardto-or-sendvia-scenarios"></a>Unzustellbare Nachrichten bei ForwardTo- oder SendVia-Szenarien

Nachrichten werden in den folgenden Situationen an die Warteschlange für unzustellbare Nachrichten übermittelt:

- Eine Meldung durchläuft mehr als vier Warteschlangen oder Themen, die [miteinander verkettet](service-bus-auto-forwarding.md) sind.
- Die Zielwarteschlange oder das Zielthema wurde deaktiviert oder gelöscht.
- Die Zielwarteschlange oder das Zielthema überschreitet die maximale Entitätsgröße.

Zum Abrufen dieser unzustellbaren Nachrichten können Sie mithilfe der [FormatTransferDeadletterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formattransferdeadletterpath)-Hilfsmethode einen Empfänger erstellen.

## <a name="example"></a>Beispiel

Der folgende Codeausschnitt erstellt einen Nachrichtenempfänger. In der Nachrichtenempfangsschleife für die Hauptwarteschlange ruft der Code die Nachricht mit [Receive(TimeSpan.Zero)](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) ab. Dadurch wird der Broker aufgefordert, sofort alle bereitstehenden Nachrichten bzw. kein Ergebnis zurückzugeben. Wenn der Code eine Nachricht empfängt, wird diese sofort abgebrochen und `DeliveryCount` wird erhöht. Sobald das System die Nachricht in die Warteschlange für unzustellbare Nachrichten verschiebt, ist die Hauptwarteschlange leer und die Schleife wird beendet, da [ReceiveAsync](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) den Wert **null** zurückgibt.

```csharp
var receiver = await receiverFactory.CreateMessageReceiverAsync(queueName, ReceiveMode.PeekLock);
while(true)
{
    var msg = await receiver.ReceiveAsync(TimeSpan.Zero);
    if (msg != null)
    {
        Console.WriteLine("Picked up message; DeliveryCount {0}", msg.DeliveryCount);
        await msg.AbandonAsync();
    }
    else
    {
        break;
    }
}
```

## <a name="path-to-the-dead-letter-queue"></a>Pfad zur Warteschlange für unzustellbare Nachrichten
Sie können auf die Warteschlange für unzustellbare Nachrichten zugreifen, indem Sie die folgende Syntax verwenden:

```
<queue path>/$deadletterqueue
<topic path>/Subscriptions/<subscription path>/$deadletterqueue
```

Wenn Sie das .NET SDK verwenden, erhalten Sie den Pfad zur Warteschlange für unzustellbare Nachrichten, indem Sie die „SubscriptionClient.FormatDeadLetterPath()“-Methode verwenden. Diese Methode akzeptiert den Namen des Themas/Abonnements und fügt diesem das Suffix **/$DeadLetterQueue** an.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Service Bus-Warteschlangen finden Sie in den folgenden Artikeln:

* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Vergleich von Azure-Warteschlangen und Service Bus-Warteschlangen](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

