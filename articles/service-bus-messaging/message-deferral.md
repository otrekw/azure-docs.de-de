---
title: 'Azure Service Bus: Nachrichtenverzögerung'
description: In diesem Artikel wird erläutert, wie die Übermittlung von Azure Service Bus-Nachrichten verzögert wird. Die Nachricht verbleibt in der Warteschlange oder im Abonnement, wird jedoch zurückgestellt.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: f4fe231c56a1bcdea4f15de90cb0e9406f0284a3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85341230"
---
# <a name="message-deferral"></a>Nachrichtenverzögerung

Wenn eine Warteschlange oder ein Abonnementclient eine Nachricht erhält, die verarbeitet werden soll, aber die Verarbeitung aufgrund von besonderen Umständen innerhalb der Anwendung zu diesem Zeitpunkt nicht möglich ist, besteht die Möglichkeit, das Abrufen der Nachricht auf einen späteren Zeitpunkt zu „verzögern“. Die Nachricht verbleibt in der Warteschlange oder im Abonnement, wird jedoch zurückgestellt.

Das Verzögerungsfeature wurde speziell für Szenarios für die Workflowverarbeitung erstellt. Workflowframeworks erfordern möglicherweise bestimmte Vorgänge, um in einer bestimmten Reihenfolge verarbeitet zu werden. Das Verarbeiten mancher empfangenen Nachrichten muss möglicherweise verschoben werden, bis die vorgeschriebenen vorherigen Arbeiten, die auf anderen Nachrichten basieren, abgeschlossen wurden.

Ein einfaches und anschauliches Beispiel ist die Verarbeitungssequenz für eine Bestellung, bei der eine Zahlungsaufforderung von einem externen Zahlungsdienstleister in einem System angezeigt wird, bevor die entsprechende Bestellung vom Geschäft an das Abwicklungssystem weitergegeben wurde. In diesem Fall kann das Abwicklungssystem das Verarbeiten der Zahlungsaufforderung verzögern, bis eine Bestellung vorhanden ist, der diese zugeordnet werden kann. In Rendezvousszenarios, bei denen Nachrichten von verschiedenen Quellen einen Workflow vorantreiben, kann die Echtzeitausführung einer Bestellung korrekt sein, aber die Nachrichten, die die Ergebnisse darstellen, können in der falschen Reihenfolge eingehen.

Die Verzögerung hilft letztendlich beim Neuordnen der Nachrichten von der Eingangsreihenfolge in eine Reihenfolge, in der sie verarbeitet werden können. Dabei werden die Nachrichten, deren Verarbeitung verschoben werden muss, sicher im Nachrichtenspeicher belassen.

## <a name="message-deferral-apis"></a>APIs für die Nachrichtenverzögerung

Die API ist [BrokeredMessage.Defer](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.defer?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_Defer) oder [BrokeredMessage.DeferAsync](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deferasync?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeferAsync) im .NET Framework-Client, [MessageReceiver.DeferAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) im .NET Standard-Client und [IMessageReceiver.defer](/java/api/com.microsoft.azure.servicebus.imessagereceiver.defer?view=azure-java-stable) oder [IMessageReceiver.deferAsync](/java/api/com.microsoft.azure.servicebus.imessagereceiver.deferasync?view=azure-java-stable) im Java-Client. 

Verzögerte Nachrichten verbleiben im Gegensatz zu unzustellbaren Nachrichten, die sich in einer Unterwarteschlange befinden, mit allen anderen aktiven Nachrichten in der Hauptwarteschlange, können jedoch nicht mehr mithilfe der regulären Funktionen „Receive“ bzw. „ReceiveAsync“ empfangen werden. Verzögerte Nachrichten können über das [Durchsuchen von Nachrichten](message-browsing.md) ermittelt werden, wenn eine Anwendung diese nicht mehr findet.

Zum Abrufen einer verzögerten Nachricht muss sich deren Besitzer die [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) beim Verzögern merken. Jeder Empfänger, der die Sequenznummer einer verzögerten Nachricht kennt, kann die Nachricht später explizit durch `Receive(sequenceNumber)` empfangen.

Wenn eine Nachricht nicht verarbeitet werden kann, weil eine bestimmte Ressource für die Verarbeitung dieser Nachricht vorübergehend nicht verfügbar ist, aber die Verarbeitung der Nachricht nicht sofort angehalten werden soll, stellt das Merken der **SequenceNumber** in einer [geplanten Nachricht](message-sequencing.md), die in ein paar Minuten gesendet werden soll und das erneute Abrufen der verzögerten Nachricht, wenn die geplante Nachricht eingeht, eine Methode dar, um diese Nachricht für ein paar Minuten zurückzustellen. Wenn alle Vorgänge eines Nachrichtenhandlers von einer Datenbank abhängen und diese Datenbank vorübergehend nicht verfügbar ist, sollten Sie nicht die Verzögerung nutzen, sondern das Empfangen von Nachrichten vollständig anhalten, bis die Datenbank wieder verfügbar ist.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Service Bus-Messaging finden Sie in folgenden Themen:

* [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Verwenden von Service Bus-Themen und -Abonnements](service-bus-dotnet-how-to-use-topics-subscriptions.md)
