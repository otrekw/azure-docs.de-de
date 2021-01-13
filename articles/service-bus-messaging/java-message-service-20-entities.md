---
title: 'Azure Service Bus Messaging: Java-Messagingdienstentitäten (Vorschau)'
description: Dieser Artikel bietet eine Übersicht über Azure Service Bus-Messagingentitäten, auf die über die Java-Messagingdienst-API zugegriffen werden kann.
ms.topic: article
ms.date: 07/20/2020
ms.openlocfilehash: 1a7fe3d6355146ccf0fce50266a6f3b8da5231b3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87801307"
---
# <a name="java-message-service-jms-20-entities-preview"></a>Java Message Service 2.0-Entitäten (JMS) (Vorschau)

Clientanwendungen, die eine Verbindung mit Azure Service Bus Premium herstellen und die [JMS-Bibliothek von Azure Service Bus](https://search.maven.org/artifact/com.microsoft.azure/azure-servicebus-jms) verwenden, können die unten aufgeführten Entitäten nutzen.

## <a name="queues"></a>Warteschlangen

Warteschlangen in JMS sind semantisch vergleichbar mit den herkömmlichen [Service Bus-Warteschlangen](service-bus-queues-topics-subscriptions.md#queues).

Verwenden Sie die folgenden Methoden in der `JMSContext`-Klasse, um eine Warteschlange zu erstellen:

```java
Queue createQueue(String queueName)
```

## <a name="topics"></a>Themen

Themen in JMS sind semantisch vergleichbar mit den herkömmlichen [Service Bus-Themen](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions).

Verwenden Sie die folgenden Methoden in der `JMSContext`-Klasse, um ein Thema zu erstellen:

```java
Topic createTopic(String topicName)
```

## <a name="temporary-queues"></a>Temporäre Warteschlangen

Wenn eine Clientanwendung eine temporäre Entität erfordert, die für die Lebensdauer der Anwendung vorhanden ist, kann sie temporäre Warteschlangen verwenden. Diese werden mit dem Muster [Anforderung/Antwort](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html) genutzt.

Verwenden Sie die folgenden Methoden in der `JMSContext`-Klasse, um eine temporäre Warteschlange zu erstellen:

```java
TemporaryQueue createTemporaryQueue()
```

## <a name="temporary-topics"></a>Temporäre Themen

Ebenso wie temporäre Warteschlangen gibt es auch temporäre Themen, um das Veröffentlichen/Abonnieren über eine temporäre Entität zu ermöglichen, die für die gesamte Lebensdauer der Anwendung vorhanden ist.

Verwenden Sie die folgenden Methoden in der `JMSContext`-Klasse, um ein temporäres Thema zu erstellen:

```java
TemporaryTopic createTemporaryTopic()
```

## <a name="java-message-service-jms-subscriptions"></a>Java Message Service-Abonnements (JMS)

Auch wenn diese semantisch [Abonnements](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) ähnlich sind (sie also für ein Thema vorhanden sind und Veröffentlichungs-/ Abonniersemantik ermöglichen), führt die Java Message Service-Spezifikation die Konzepte der Attribute **Shared** (freigegeben), **Unshared** (nicht freigegeben), **Durable** (dauerhaft) und **Non-durable** (nicht dauerhaft) für das jeweilige Abonnement ein.

> [!NOTE]
> Die unten genannten Abonnements sind im Premium-Tarif von Azure Service Bus als Vorschau für Clientanwendungen verfügbar, die über die [JMS-Bibliothekvon Azure Service Bus](https://search.maven.org/artifact/com.microsoft.azure/azure-servicebus-jms) eine Verbindung mit Azure Service Bus herstellen.
>
> Für die öffentliche Vorschau können diese Abonnements nicht mithilfe des Azure-Portals erstellt werden.
>

### <a name="shared-durable-subscriptions"></a>Freigegebene dauerhafte Abonnements

Ein freigegebenes dauerhaftes Abonnement wird verwendet, wenn alle zu einem Thema veröffentlichten Nachrichten von einer Anwendung empfangen und verarbeitet werden sollen, und zwar unabhängig davon, ob die Anwendung das Abonnement jederzeit aktiv nutzt.

Da es sich um ein freigegebenes Abonnement handelt, kann jede Anwendung, die für den Empfang von Service Bus authentifiziert ist, von diesem Abonnement Nachrichten empfangen.

Verwenden Sie zum Erstellen eines freigegebenen dauerhaften Abonnements die folgenden Methoden für die `JMSContext`-Klasse:

```java
JMSConsumer createSharedDurableConsumer(Topic topic, String name)

JMSConsumer createSharedDurableConsumer(Topic topic, String name, String messageSelector)
```

Das freigegebene dauerhafte Abonnement ist weiterhin vorhanden, es sei denn, es wird mithilfe der `unsubscribe`-Methode für die `JMSContext` Klasse gelöscht.

```java
void unsubscribe(String name)
```

### <a name="unshared-durable-subscriptions"></a>Nicht freigegebene dauerhafte Abonnements

Genau wie freigegebenes dauerhaftes Abonnement wird ein nicht freigegebenes dauerhaftes Abonnement verwendet, wenn alle zu einem Thema veröffentlichten Nachrichten von einer Anwendung empfangen und verarbeitet werden sollen, und zwar unabhängig davon, ob die Anwendung das Abonnement jederzeit aktiv nutzt.

Da es sich hierbei jedoch um ein nicht freigegebenes Abonnement handelt, kann nur die Anwendung, die das Abonnement erstellt hat, daraus Nachrichten empfangen.

Verwenden Sie zum Erstellen eines nicht freigegebenen dauerhaften Abonnements die folgenden Methoden aus der `JMSContext`-Klasse: 

```java
JMSConsumer createDurableConsumer(Topic topic, String name)

JMSConsumer createDurableConsumer(Topic topic, String name, String messageSelector, boolean noLocal)
```

> [!NOTE]
> Das `noLocal`-Feature wird zurzeit nicht unterstützt und somit ignoriert.
>

Das nicht freigegebene dauerhafte Abonnement ist weiterhin vorhanden, es sei denn, es wird mithilfe der `unsubscribe`-Methode für die `JMSContext` Klasse gelöscht.

```java
void unsubscribe(String name)
```

### <a name="shared-non-durable-subscriptions"></a>Freigegebene nicht dauerhafte Abonnements

Ein freigegebenes nicht dauerhaftes Abonnement wird verwendet, wenn mehrere Clientanwendungen Nachrichten von einem einzigen Abonnement empfangen und verarbeiten müssen, und zwar nur so lange, wie sie aktiv Nachrichten von diesem Abonnement nutzen/empfangen.

Da das Abonnement nicht dauerhaft ist, wird es nicht persistent gespeichert. Nachrichten werden von diesem Abonnement nicht empfangen, wenn keine aktiven Consumer für das Abonnement vorhanden sind.

Um ein freigegebenes nicht dauerhaftes Abonnement zu erstellen, erstellen Sie ein `JmsConsumer`-Element, wie in den unten genannten Methoden aus der `JMSContext`-Klasse gezeigt:

```java
JMSConsumer createSharedConsumer(Topic topic, String sharedSubscriptionName)

JMSConsumer createSharedConsumer(Topic topic, String sharedSubscriptionName, String messageSelector)
```

Das freigegebene nicht dauerhafte Abonnement ist so lange vorhanden, wie aktive Consumer Nachrichten von ihm empfangen.

### <a name="unshared-non-durable-subscriptions"></a>Nicht freigegebene nicht dauerhafte Abonnements

Ein nicht freigegebenes nicht dauerhaftes Abonnement wird verwendet, wenn die Clientanwendung Nachrichten von einem einzigen Abonnement empfangen und verarbeiten muss, und zwar nur so lange, wie sie aktiv Nachrichten von diesem Abonnement nutzt. Für dieses Abonnement darf nur ein Consumer vorhanden sein, d. h. der Client, der das Abonnement erstellt hat.

Da das Abonnement nicht dauerhaft ist, wird es nicht persistent gespeichert. Nachrichten werden von diesem Abonnement nicht empfangen, wenn kein aktiver Consumer für das Abonnement vorhanden ist.

Um ein nicht freigegebenes und nicht dauerhaftes Abonnement zu erstellen, erstellen Sie ein `JMSConsumer`-Element, wie in den unten genannten Methoden aus der `JMSContext`-Klasse gezeigt:

```java
JMSConsumer createConsumer(Destination destination)

JMSConsumer createConsumer(Destination destination, String messageSelector)

JMSConsumer createConsumer(Destination destination, String messageSelector, boolean noLocal)
```

> [!NOTE]
> Das `noLocal`-Feature wird zurzeit nicht unterstützt und somit ignoriert.
>

Das nicht freigegebene nicht dauerhafte Abonnement ist so lange vorhanden, wie ein aktiver Consumer Nachrichten von ihm empfängt.

### <a name="message-selectors"></a>Nachrichtenselektoren

Ebenso wie **Filter und Aktionen** für reguläre Service Bus-Abonnements vorhanden sind, sind **Nachrichtenselektoren** für JMS-Abonnements vorhanden.

Nachrichtenselektoren können für jedes der JMS-Abonnements eingerichtet werden und sind als Filterbedingung für die Nachrichtenheadereigenschaften vorhanden. Es werden nur Nachrichten zugestellt, deren Headereigenschaften mit dem Nachrichtenselektorausdruck übereinstimmen. Ein Wert von NULL oder eine leere Zeichenfolge gibt an, dass kein Nachrichtenselektor für das JMS-Abonnement bzw. den -Consumer vorhanden ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen und Beispiele für die Verwendung von Service Bus-Messaging finden Sie in den folgenden erweiterten Themen:

* [Übersicht über Service Bus-Messaging](service-bus-messaging-overview.md)
* [Verwenden der Java Message Service 2.0-API mit Azure Service Bus Premium (Vorschau)](how-to-use-java-message-service-20.md)



