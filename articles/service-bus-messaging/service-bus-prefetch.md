---
title: Vorabruf von Azure Service Bus-Nachrichten | Microsoft-Dokumentation
description: Verbessern Sie die Leistung durch den Vorabruf von Azure Service Bus-Nachrichten. Nachrichten sind direkt für den lokalen Abruf verfügbar, bevor die Anwendung sie anfordert.
ms.topic: article
ms.date: 04/23/2021
ms.openlocfilehash: 992fa9228d3cf8bafb3d2ea9110b515ff4b56498
ms.sourcegitcommit: aba63ab15a1a10f6456c16cd382952df4fd7c3ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107989088"
---
# <a name="prefetch-azure-service-bus-messages"></a>Vorabruf von Azure Service Bus-Nachrichten
Wenn Sie das *Vorabruffeature* für einen der offiziellen Service Bus-Clients aktivieren, erhält der Empfänger bis zur angegebenen Anzahl von Vorabrufen mehr Nachrichten als von der Anwendung ursprünglich gefordert. Beachten Sie, dass der JavaScript- und TypeScript-Client dieses Feature noch nicht unterstützt.

Wenn Nachrichten an die Anwendung zurückgegeben werden, ruft der Client weitere Nachrichten im Hintergrund ab, um den Vorabrufpuffer aufzufüllen.

## <a name="enabling-prefetch"></a>Aktivieren des Vorabrufs
Legen Sie zum Aktivieren des Vorabruffeatures die Vorabrufanzahl des Warteschlangen- oder Abonnementclients auf eine Zahl größer als null fest. Durch Festlegen des Werts auf 0 (null) wird der Vorabruf deaktiviert. 

# <a name="net"></a>[.NET](#tab/dotnet)
Wenn Sie die neueste Azure.Messaging.ServiceBus-Bibliothek verwenden, können Sie die Eigenschaft für die Anzahl der Vorabrufe für die [ServiceBusReceiver](/dotnet/api/azure.messaging.servicebus.servicebusreceiver.prefetchcount#Azure_Messaging_ServiceBus_ServiceBusReceiver_PrefetchCount)- und [ServiceBusProcessor](/dotnet/api/azure.messaging.servicebus.servicebusprocessor.prefetchcount#Azure_Messaging_ServiceBus_ServiceBusProcessor_PrefetchCount)-Objekte festlegen. 

Wenn Sie die ältere .NET-Clientbibliothek für Service Bus (Microsoft.Azure.ServiceBus) verwenden, können Sie die Eigenschaft für die Anzahl der Vorabrufe für [MessageReceiver,](/dotnet/api/microsoft.servicebus.messaging.messagereceiver.prefetchcount) [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount#Microsoft_Azure_ServiceBus_QueueClient_PrefetchCount) oder [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.prefetchcount) festlegen.
 
# <a name="java"></a>[Java](#tab/java)
Wenn Sie die neueste azure-messaging-servicebus-Bibliothek verwenden, können Sie die Eigenschaft für die Anzahl der Vorabrufe für die [ServiceBusReceiver](/dotnet/api/azure.messaging.servicebus.servicebusreceiver.prefetchcount#Azure_Messaging_ServiceBus_ServiceBusReceiver_PrefetchCount)- und [ServiceBusProcessor](/dotnet/api/azure.messaging.servicebus.servicebusprocessor.prefetchcount#Azure_Messaging_ServiceBus_ServiceBusProcessor_PrefetchCount)-Objekte festlegen. 

Wenn Sie die ältere Java-Clientbibliothek für Service Bus (azure-servicebus) verwenden, können Sie die Eigenschaft für die Anzahl der Vorabrufe für [MessageReceiver,](/java/api/com.microsoft.azure.servicebus.imessagereceiver.setprefetchcount#com_microsoft_azure_servicebus_IMessageReceiver_setPrefetchCount_int_) [QueueClient](/java/api/com.microsoft.azure.servicebus.queueclient.setprefetchcount#com_microsoft_azure_servicebus_QueueClient_setPrefetchCount_int_) oder [SubscriptionClient](/java/api/com.microsoft.azure.servicebus.subscriptionclient.setprefetchcount#com_microsoft_azure_servicebus_SubscriptionClient_setPrefetchCount_int_) festlegen.
 
# <a name="python"></a>[Python](#tab/python)

Sie können die **prefetch_count** auf [azure.servicebus.ServiceBusReceiver](/python/api/azure-servicebus/azure.servicebus.servicebusreceiver) oder [azure.servicebus.aio.ServiceBusReceiver](/python/api/azure-servicebus/azure.servicebus.aio.servicebusreceiver) festlegen.

---

> [!NOTE]
> Das Java Script SDK unterstützt das **Vorabruffeature** nicht. 

Solange Nachrichten im Vorabrufpuffer vorhanden sind, werden alle nachfolgenden Empfangsaufrufe sofort aus dem Puffer erfüllt. Der Puffer wird im Hintergrund aufgefüllt, wenn Speicherplatz verfügbar wird. Wenn keine zu übermittelnden Nachrichten vorhanden sind, leert der Empfangsvorgang den Puffer und wartet oder sperrt dann wie erwartet.

## <a name="why-is-prefetch-not-the-default-option"></a>Warum ist Vorabruf nicht die Standardoption?
Vorabruf beschleunigt den Nachrichtenfluss, indem eine Nachricht zum lokalen Abruf bereitgehalten wird, bevor die Anwendung sie anfragt. Diese Durchsatzbeschleunigung ist das Ergebnis eines Kompromisses, die der Anwendungsautor explizit treffen muss:

Mit dem Modus [Empfangen und Löschen](message-transfers-locks-settlement.md#receiveanddelete) sind alle Nachrichten, die in den Vorabrufpuffer abgerufen werden, nicht mehr in der Warteschlange verfügbar. Die Nachrichten verbleiben nur im Arbeitsspeicher-Vorabrufpuffer, bis sie in der Anwendung empfangen werden. Wenn die Anwendung vor dem Eingang der Nachrichten in der Anwendung beendet wird, gehen diese Nachrichten unwiederbringlich verloren.

Im Empfangsmodus [PeekLock](message-transfers-locks-settlement.md#peeklock) werden Nachrichten, die in den Vorabrufpuffer abgerufen wurden, im gesperrten Zustand im Puffer erfasst. Für sie wird das Zeitlimit für die Sperre aktiviert. Wenn der Vorabrufpuffer groß ist und die Verarbeitung so lange dauert, dass Sperren für Nachrichten ablaufen, während sie sich im Vorabrufpuffer befinden oder sogar während die Anwendung die Nachricht verarbeitet, kann es einige verwirrende Ereignisse geben, die von der Anwendung behandelt werden müssen.

Die Anwendung kann eine Nachricht mit einer abgelaufenen oder in Kürze ablaufenden Sperre empfangen. Wenn dies der Fall ist, verarbeitet die Anwendung möglicherweise die Nachricht, stellt dann aber fest, dass sie die Nachricht aufgrund einer abgelaufenen Sperre nicht abschließen kann. Das Programm kann die `LockedUntilUtc`-Eigenschaft überprüfen (die der Taktverschiebung zwischen dem Broker und der Uhr des lokalen Computers unterliegt). Wenn die Nachrichtensperre abgelaufen ist, muss die Anwendung die Nachricht ignorieren und sollte keinen API-Aufruf für die Nachricht ausführen. Wenn die Nachricht nicht abgelaufen ist, der Ablauf aber bevorsteht, kann die Sperre erneuert und um eine weitere Standardsperrdauer verlängert werden.

Wenn die Sperre im Vorabrufpuffer automatisch abläuft, wird die Nachricht als abgebrochen behandelt und wieder für den Abruf aus der Warteschlange zur Verfügung gestellt. Dies kann dazu führen, dass die Nachricht in den Vorabrufpuffer abgerufen und am Ende platziert wird. Wenn der Vorabrufpuffer während des Nachrichtenablaufs in der Regel nicht abgearbeitet werden kann, werden Nachrichten zwar wiederholt vorabgerufen, aber nie effektiv in einen nutzbaren (gültig gesperrten) Zustand versetzt. Sie werden schließlich in die Warteschlange für unzustellbare Nachrichten verschoben, sobald die maximale Anzahl der Zustellungen überschritten ist.

Wenn Sie für die Nachrichtenverarbeitung ein hohes Maß an Zuverlässigkeit benötigen und die Verarbeitung viel Arbeit und Zeit in Anspruch nimmt, empfiehlt es sich, die Vorabruffunktion dosiert oder gar nicht zu verwenden.

Wenn Sie hohe Durchsatzraten benötigen und die Nachrichtenverarbeitung im Allgemeinen günstig ist, bietet der Vorabruf erhebliche Durchsatzvorteile.

Die maximale Anzahl der Vorabrufvorgänge und die für die Warteschlange oder das Abonnement konfigurierte Sperrdauer müssen so abgestimmt werden, dass das Zeitlimit für die Sperre mindestens die kumulative erwartete Verarbeitungszeit der erwarteten Nachrichten für die maximale Größe des Vorabrufpuffers plus eine Nachricht überschreitet. Gleichzeitig sollte das Sperrentimeout nicht so lang sein, dass Nachrichten ihre maximale Lebensdauer überschreiten können, wenn sie versehentlich gelöscht werden und somit ihre Sperre ablaufen muss, bevor sie erneut zugestellt werden.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die Beispiele in der Sprache Ihrer Wahl an, um Azure Service Bus-Features zu untersuchen. 

- [Azure Service Bus-Clientbibliothekbeispiele für .NET (neueste Version)](/samples/azure/azure-sdk-for-net/azuremessagingservicebus-samples/) 
- [Azure Service Bus-Clientbibliothekbeispiele für Java (neueste Version)](/samples/azure/azure-sdk-for-java/servicebus-samples/)
- [Azure Service Bus-Clientbibliothekbeispiele für Python](/samples/azure/azure-sdk-for-python/servicebus-samples/)
- [Azure Service Bus-Clientbibliothekbeispiele für JavaScript](/samples/azure/azure-sdk-for-js/service-bus-javascript/)
- [Azure Service Bus-Clientbibliothekbeispiele für TypeScript](/samples/azure/azure-sdk-for-js/service-bus-typescript/)

Hier finden Sie Beispiele für die älteren .NET- und Java-Clientbibliotheken:
- [Azure Service Bus-Clientbibliothekbeispiele für .NET (Legacy)](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/): siehe **Beispiel zum Vorabruf**. 
- [Azure Service Bus-Clientbibliothekbeispiele für Java (Legacy)](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/azure-servicebus): siehe **Beispiel zum Vorabruf**. 
