---
title: 'Azure Service Bus: Nachrichtenverzögerung'
description: In diesem Artikel wird erläutert, wie die Übermittlung von Azure Service Bus-Nachrichten verzögert wird. Die Nachricht bleibt in der Warteschlange oder im Abonnement, wird jedoch zurückgestellt.
ms.topic: conceptual
ms.date: 04/21/2021
ms.openlocfilehash: 71446f7989a235e79ca3ab208579edd0043dfeb2
ms.sourcegitcommit: aba63ab15a1a10f6456c16cd382952df4fd7c3ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107987252"
---
# <a name="message-deferral"></a>Nachrichtenverzögerung
Wenn ein Warteschlangen- oder Abonnementclient eine Nachricht empfängt, die er verarbeiten möchte, aber die Verarbeitung aufgrund besonderer Umstände derzeit nicht möglich ist, hat er die Möglichkeit, den Abruf der Nachricht auf einen späteren Zeitpunkt zu „verzögern“. Die Nachricht bleibt in der Warteschlange oder im Abonnement, wird jedoch zurückgestellt.

> [!NOTE]
> Verzögerte Nachrichten werden [nach ihrem Ablauf](./service-bus-dead-letter-queues.md#time-to-live) nicht automatisch in die Warteschlange für unzustellbare Nachrichten verschoben. Dieses Verhalten ist beabsichtigt:

## <a name="sample-scenarios"></a>Beispielszenarios
Das Verzögerungsfeature wurde speziell für Szenarien für die Workflowverarbeitung erstellt. Workflowframeworks erfordern möglicherweise, dass bestimmte Vorgänge in einer bestimmten Reihenfolge verarbeitet werden. Sie müssen möglicherweise die Verarbeitung einiger empfangener Nachrichten verschieben, bis vorgeschriebene Vorarbeiten, die durch andere Nachrichten bedingt sind, abgeschlossen wurden.

Ein einfaches und anschauliches Beispiel ist die Verarbeitungssequenz für eine Bestellung, bei der eine Zahlungsaufforderung von einem externen Zahlungsdienstleister in einem System angezeigt wird, bevor die entsprechende Bestellung vom Geschäft an das Abwicklungssystem weitergegeben wurde. In diesem Fall kann das Abwicklungssystem das Verarbeiten der Zahlungsaufforderung verzögern, bis eine Bestellung vorhanden ist, der diese zugeordnet werden kann. In Rendezvousszenarios, bei denen Nachrichten von verschiedenen Quellen einen Workflow vorantreiben, kann die Echtzeitausführung einer Bestellung korrekt sein, aber die Nachrichten, die die Ergebnisse darstellen, können in der falschen Reihenfolge eingehen.

Die Verzögerung hilft letztendlich beim Neuordnen der Nachrichten von der Eingangsreihenfolge in eine Reihenfolge, in der sie verarbeitet werden können. Dabei werden die Nachrichten, deren Verarbeitung verschoben werden muss, sicher im Nachrichtenspeicher belassen.

Wenn eine Nachricht nicht verarbeitet werden kann, weil eine bestimmte Ressource für die Verarbeitung dieser Nachricht vorübergehend nicht verfügbar ist, aber die Verarbeitung der Nachricht nicht sofort angehalten werden soll, stellt das Merken der Sequenznummer in einer [geplanten Nachricht](message-sequencing.md), die in einigen Minuten gesendet werden soll, und das erneute Abrufen der verzögerten Nachricht, wenn die geplante Nachricht eingeht, eine Methode dar, um diese Nachricht für einige Minuten zurückzustellen. Wenn alle Vorgänge eines Nachrichtenhandlers von einer Datenbank abhängen und diese Datenbank vorübergehend nicht verfügbar ist, sollten Sie nicht die Verzögerung nutzen, sondern das Empfangen von Nachrichten vollständig anhalten, bis die Datenbank wieder verfügbar ist. 

## <a name="retrieving-deferred-messages"></a>Abrufen von verzögerten Nachrichten
Verzögerte Nachrichten verbleiben im Gegensatz zu unzustellbaren Nachrichten, die sich in einer Unterwarteschlange befinden, mit allen anderen aktiven Nachrichten in der Hauptwarteschlange, können jedoch nicht mehr mithilfe der regulären Empfangsfunktionen empfangen werden. Verzögerte Nachrichten können über das [Durchsuchen von Nachrichten](message-browsing.md) ermittelt werden, wenn eine Anwendung diese nicht mehr findet.

Zum Abrufen einer verzögerten Nachricht muss sich deren Besitzer die Sequenznummer beim Verzögern merken. Jeder Empfänger, der die Sequenznummer einer verzögerten Nachricht kennt, kann die Nachricht später mithilfe von Empfangsmethoden empfangen, die die Sequenznummer als Parameter verwenden. Weitere Informationen zu Sequenznummern finden Sie unter [Nachrichtensequenzierung und Zeitstempel](message-sequencing.md).

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die Beispiele in der Sprache Ihrer Wahl an, um Azure Service Bus-Features zu untersuchen. 

- [Azure Service Bus Clientbibliothekbeispiele für .NET (neueste Version)](/samples/azure/azure-sdk-for-net/azuremessagingservicebus-samples/): siehe Beispiel **Abwickeln von Nachrichten**. 
- [Azure Service Bus-Clientbibliothekbeispiele für Java (neueste Version)](/samples/azure/azure-sdk-for-java/servicebus-samples/)
- [Azure Service Bus-Clientbibliothekbeispiele für Python](/samples/azure/azure-sdk-for-python/servicebus-samples/): siehe Beispiel **receive_deferred_message_queue.py**. 
- [Azure Service Bus Clientbibliothekbeispiele für JavaScript](/samples/azure/azure-sdk-for-js/service-bus-javascript/): siehe Beispiel **advances/deferral.js**. 
- [Azure Service Bus Clientbibliothekbeispiele für TypeScript](/samples/azure/azure-sdk-for-js/service-bus-typescript/): siehe Beispiel **advances/deferral.ts**. 

Hier finden Sie Beispiele für die älteren .NET- und Java-Clientbibliotheken:
- [Azure Service Bus-Clientbibliothekbeispiele für .NET (Legacy)](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/): siehe Beispiel zur **Verzögerung**. 
- [Azure Service Bus-Clientbibliothekbeispiele für Java (Legacy)](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/azure-servicebus/MessageBrowse)
