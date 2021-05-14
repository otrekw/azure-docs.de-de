---
title: 'Azure Service Bus: Durchsuchen von Nachrichten'
description: Durch das Durchsuchen und Einsehen von Service Bus-Nachrichten kann ein Azure Service Bus-Client alle Nachrichten in einer Warteschlange oder in einem Abonnement aufzählen.
ms.topic: article
ms.date: 03/29/2021
ms.openlocfilehash: 23ec2187ae58b1b4f83addb80573bdb130ff99bd
ms.sourcegitcommit: aba63ab15a1a10f6456c16cd382952df4fd7c3ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107989394"
---
# <a name="message-browsing"></a>Einsehen von Nachrichten
Das Einsehen von Nachrichten ermöglicht einem Service Bus-Client, alle Nachrichten in einer Warteschlange oder einem Abonnement zu Diagnose- und Debugzwecken aufzuzählen.

Der Peek-Vorgang für eine Warteschlange oder ein Abonnement gibt höchstens die angeforderte Anzahl von Nachrichten zurück. Die folgende Tabelle zeigt die Typen von Nachrichten, die vom Peek-Vorgang zurückgegeben werden. 

| Nachrichtentyp | Enthalten? | 
| ---------------- | ----- | 
| Aktive Nachrichten | Ja |
| Unzustellbare Nachrichten | Nein | 
| Gesperrte Nachrichten | Ja |
| Abgelaufene Nachrichten |  Möglicherweise (bevor sie unzustellbar sind) |
| Geplante Nachrichten | Ja für Warteschlangen. Nein für Abonnements |

## <a name="dead-lettered-messages"></a>Unzustellbare Nachrichten
Um einen Einblick in **unzustellbare** Nachrichten einer Warteschlange oder eines Abonnements zu erhalten, sollte der Peek-Vorgang in der Warteschlange für unzustellbare Nachrichten ausgeführt werden, die der Warteschlange oder dem Abonnement zugeordnet ist. Weitere Informationen finden Sie unter [Zugreifen auf Warteschlangen für unzustellbare Nachrichten](service-bus-dead-letter-queues.md#path-to-the-dead-letter-queue).

## <a name="expired-messages"></a>Abgelaufene Nachrichten
Abgelaufene Nachrichten können in den Ergebnissen enthalten sein, die vom Peek-Vorgang zurückgegeben werden. Verbrauchte und abgelaufene Nachrichten werden durch eine asynchrone Ausführung von „Garbage Collection“ bereinigt. Dieser Schritt tritt möglicherweise nicht unmittelbar nach dem Ablauf von Nachrichten auf. Deshalb kann ein Peek-Vorgang Nachrichten zurückgeben, die bereits abgelaufen sind. Diese Nachrichten werden entfernt oder in die Warteschlange für unzustellbare Nachrichten verschoben, wenn das nächste Mal ein Empfangsvorgang für die Warteschlange oder das Abonnement aufgerufen wird. Beachten Sie dieses Verhalten, wenn Sie versuchen, verzögerte Nachrichten aus der Warteschlange wiederherzustellen. 

Eine abgelaufene kann nicht mehr auf andere Weise regelmäßig abgerufen werden, selbst wenn sie von Peek zurückgegeben wird. Die Rückgabe dieser Nachrichten ist beabsichtigt, da Peek ein Diagnosetool ist, das den aktuellen Status des Protokolls widerspiegelt.

## <a name="locked-messages"></a>Gesperrte Nachrichten
Peek gibt auch Nachrichten zurück, die **gesperrt** wurden und aktuell von anderen Empfängern verarbeitet werden. Da Peek jedoch eine getrennte Momentaufnahme zurückgibt, kann der Sperrzustand einer Nachricht für eingesehene Nachrichten nicht beobachtet werden.

## <a name="peek-apis"></a>Peek-APIs
Peek funktioniert für Warteschlangen, Abonnements und ihre Warteschlangen für unzustellbare Nachrichten. 

Bei wiederholtem Aufruf listet der Peek-Vorgang alle Nachrichten in der Warteschlange oder dem Abonnement auf, und zwar in der Reihenfolge der Sequenznummern von der niedrigsten verfügbaren bis zur höchsten. Dies ist die Reihenfolge, in der Nachrichten in die Warteschlange gestellt wurden, und nicht die Reihenfolge, in der Nachrichten schließlich abgerufen werden.

Sie können auch „SequenceNumber“ an einen Peek-Vorgang übergeben. Sie wird verwendet, um zu bestimmen, wo Sie mit dem Peek-Vorgang beginnen sollen. Sie können nachfolgende Aufrufe des Peek-Vorgangs vornehmen, ohne den Parameter anzugeben, der weiter enumeriert.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die Beispiele in der Sprache Ihrer Wahl an, um Azure Service Bus-Features zu untersuchen. 

- [Azure Service Bus Clientbibliotheksbeispiele für .NET (neueste Version)](/samples/azure/azure-sdk-for-net/azuremessagingservicebus-samples/): siehe Beispiel **Abwickeln von Nachrichten**.
- [Azure Service Bus-Clientbibliotheksbeispiele für Java (neueste Version)](/samples/azure/azure-sdk-for-java/servicebus-samples/): siehe Beispiel  - **Einsehen einer Nachricht**
- [Azure Service Bus-Clientbibliotheksbeispiele für Python](/samples/azure/azure-sdk-for-python/servicebus-samples/)  - **Beispiel: „receive_peek.py“**
- [Azure Service Bus-Clientbibliotheksbeispiele für JavaScript](/samples/azure/azure-sdk-for-js/service-bus-javascript/)  - **browseMessages.js**-Beispiel
- [Azure Service Bus-Clientbibliotheksbeispiele für TypeScript](/samples/azure/azure-sdk-for-js/service-bus-typescript/) - **browseMessages.js**-Beispiel

Hier finden Sie Beispiele für die älteren .NET- und Java-Clientbibliotheken:
- [Azure Service Bus-Clientbibliotheksbeispiele für .NET (Vorgängerversion)](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/)Beispiel für das  - **Durchsuchen von Nachrichten (Peek)**
- [Azure Service Bus-Clientbibliothekbeispiele für Java (Vorgängerversion)](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/azure-servicebus): siehe Beispiel für das  - **Durchsuchen von Nachrichten**. 

