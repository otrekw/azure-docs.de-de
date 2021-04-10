---
title: Entwicklerleitfaden für Azure Service Bus JMS 2.0
description: Erfahren Sie, wie Sie die JMS 2.0-API (Java Message Service) zur Kommunikation mit Azure Service Bus verwenden.
ms.topic: article
ms.date: 01/17/2021
ms.openlocfilehash: 492da46a5ce4f5955a72972d91a8c56d54c385bb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101726953"
---
# <a name="azure-service-bus-jms-20-developer-guide"></a>Entwicklerleitfaden für Azure Service Bus JMS 2.0

Dieser Leitfaden bietet detaillierte Informationen zu einer erfolgreichen Kommunikation mit Azure Service Bus über die JMS 2.0-API (Java Message Service).

Wenn Sie Java-Entwickler, aber mit Azure Service Bus noch nicht vertraut sind, sollten Sie die folgenden Artikel lesen.

| Erste Schritte | Konzepte |
|----------------|-------|
| <ul> <li> [Was ist Azure Service Bus?](service-bus-messaging-overview.md) </li> <li> [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md) </li> </ul> | <ul> <li> [Service Bus Premium- und Standard-Preisstufe für Messaging](service-bus-premium-messaging.md) </li> </ul> |

## <a name="java-message-service-jms-programming-model"></a>JMS-Programmiermodell (Java Message Service)

Hier finden Sie eine Kurzdarstellung des Programmiermodells mit Java Message Service-API. 

> [!NOTE]
>
>Der **Azure Service Bus-Premium-Tarif** unterstützt JMS 1.1 und JMS 2.0.
> <br> <br>
> Der **Azure Service Bus-Standard-Tarif** unterstützt eine eingeschränkte JMS 1.1-Funktionalität. Weitere Informationen finden Sie in dieser [Dokumentation](service-bus-java-how-to-use-jms-api-amqp.md).
>

# <a name="jms-20-programming-model"></a>[JMS 2.0-Programmiermodell](#tab/JMS-20)

:::image type="content" source="./media/jms-developer-guide/java-message-service-20-programming-model.png "alt-text="Diagramm des JMS 2.0-Programmiermodells." border="false":::

# <a name="jms-11-programming-model"></a>[JMS 1.1-Programmiermodell](#tab/JMS-11)

:::image type="content" source="./media/jms-developer-guide/java-message-service-11-programming-model.png "alt-text="Diagramm des JMS 1.1-Programmiermodells." border="false":::

---

## <a name="jms---building-blocks"></a>JMS: Bausteine

Die folgenden Bausteine stehen für die Kommunikation mit der JMS-Anwendung zur Verfügung.

> [!NOTE]
> Der folgende Leitfaden wurde aus dem [Oracle Java EE 6-Tutorial für Java Message Service (JMS)](https://docs.oracle.com/javaee/6/tutorial/doc/bnceh.html) übernommen und angepasst.
>
> Zum besseren Verständnis des Java Message Service (JMS) wird die Lektüre dieses Tutorials empfohlen.
>

### <a name="connection-factory"></a>Verbindungsfactory
Das Verbindungsfactoryobjekt wird vom Client zur Herstellung der Verbindung mit dem JMS-Anbieter verwendet. Die Verbindungsfactory kapselt eine Reihe von Konfigurationsparametern für die Verbindung, die vom Administrator definiert werden.

Jede Verbindungsfactory ist eine Instanz der Schnittstellen `ConnectionFactory`, `QueueConnectionFactory` oder `TopicConnectionFactory`.

Um die Verbindungsherstellung mit Azure Service Bus zu vereinfachen, sind diese Schnittstellen durch `ServiceBusJmsConnectionFactory`, `ServiceBusJmsQueueConnectionFactory` bzw. `ServiceBusJmsTopicConnectionFactory` implementiert. Die Verbindungsfactory kann mit den folgenden Parametern instanziiert werden: 
   * Verbindungszeichenfolge: die Verbindungszeichenfolge für den Namespace des Azure Service Bus-Premium-Tarif
   * ServiceBusJmsConnectionFactorySettings-Eigenschaftenbehälter, der Folgendes enthält:
      * connectionIdleTimeoutMS: Timeout für Leerlaufverbindungen in Millisekunden.
      * traceFrames: Boolesches Flag zum Erfassen von AMQP-Ablaufverfolgungsframes für das Debugging.
      * *andere Konfigurationsparameter*

Die Factory kann wie folgt erstellt werden: Die Verbindungszeichenfolge ist ein erforderlicher Parameter, die weiteren Eigenschaften sind optional.

```java
ConnectionFactory factory = new ServiceBusJmsConnectionFactory(SERVICE_BUS_CONNECTION_STRING, null);
```

> [!IMPORTANT]
> Java-Anwendungen, die die JMS 2.0-API nutzen, dürfen zum Herstellen der Verbindung mit Azure Service Bus nur die Verbindungszeichenfolge verwenden. Derzeit wird die Authentifizierung für JMS-Clients nur über die Verbindungszeichenfolge unterstützt.
>
> Die Back-End-Authentifizierung von Azure Active Directory (AAD) wird derzeit nicht unterstützt.
>

### <a name="jms-destination"></a>JMS-Ziel

Ein Ziel (destination) ist ein Objekt, das von einem Client verwendet wird, um das Ziel der erzeugten Nachricht und die Quelle der genutzten Nachrichten anzugeben.

Ziele sind Entitäten in Azure Service Bus zugeordnet: Warteschlangen (in Punkt-zu-Punkt-Szenarien) und Themen (in Szenarien mit dem Muster „Veröffentlichen-Abonnieren“).

### <a name="connections"></a>Verbindungen

Eine Verbindung kapselt eine virtuelle Verbindung mit einem JMS-Anbieter. Bei Azure Service Bus stellt dies eine zustandsbehaftete Verbindung zwischen der Anwendung und Azure Service Bus über AMQP dar.

Eine Verbindung wird wie unten gezeigt von der Verbindungsfactory erstellt.

```java
Connection connection = factory.createConnection();
```

### <a name="sessions"></a>Sitzungen

Eine Sitzung ist ein Einzelthreadkontext zum Erstellen und Nutzen von Nachrichten. Sie kann zum Erstellen von Nachrichten sowie Nachrichtenproducern und -consumern verwendet werden, bietet aber auch einen Transaktionskontext, um Sende- und Empfangsvorgänge in eine atomische Arbeitseinheit zu gruppieren.

Eine Sitzung kann wie unten gezeigt aus dem Verbindungsobjekt erstellt werden.

```java
Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
```

#### <a name="session-modes"></a>Sitzungsmodi

Eine Sitzung kann in einem beliebigen der folgenden Modi erstellt werden.

| Sitzungsmodi | Verhalten |
| ----------------- | -------- |
|**Session.AUTO_ACKNOWLEDGE** | Die Sitzung bestätigt automatisch den Empfang einer Nachricht durch den Client, wenn die Sitzung erfolgreich von einem Empfangsaufruf zurückgegeben wird oder wenn der von der Sitzung zum Verarbeiten der Nachricht aufgerufene Nachrichtenlistener erfolgreich zurückgegeben wird.|
|**Session.CLIENT_ACKNOWLEDGE** |Der Client bestätigt eine genutzte Nachricht durch Aufrufen der Bestätigungsmethode der Nachricht.|
|**Session.DUPS_OK_ACKNOWLEDGE**|Dieser Bestätigungsmodus weist die Sitzung an, die Übermittlung von Nachrichten verzögert zu bestätigen.| 
|**Session.SESSION_TRANSACTED**|Dieser Wert kann als Argument an die Methode „createSession(int sessionMode)“ im Verbindungsobjekt übergeben werden, um anzugeben, dass die Sitzung eine lokale Transaktion verwenden soll.| 

Wenn kein Sitzungsmodus angegeben ist, wird standardmäßig **Session.AUTO_ACKNOWLEDGE** ausgewählt.

### <a name="jmscontext"></a>JMSContext

> [!NOTE]
> JMSContext ist im Rahmen der JMS 2.0-Spezifikation definiert.
>

JMSContext kombiniert die Funktionen von Verbindungs- und Sitzungsobjekt. Dieser Kontext kann aus dem Verbindungsfactoryobjekt erstellt werden.

```java
JMSContext context = connectionFactory.createContext();
```

#### <a name="jmscontext-modes"></a>JMSContext-Modi

Ebenso wie das Objekt für die **Sitzung** kann JMSContext mit denselben Bestätigungsmodi erstellt werden, wie unter [Sitzungsmodi](#session-modes) beschrieben.

```java
JMSContext context = connectionFactory.createContext(JMSContext.AUTO_ACKNOWLEDGE);
```

Wenn kein Modus angegeben ist, wird standardmäßig **JMSContext.AUTO_ACKNOWLEDGE** ausgewählt.

### <a name="jms-message-producers"></a>JMS-Nachrichtenproducer

Ein Nachrichtenproducer ist ein Objekt, das mithilfe eines JMSContexts oder einer Sitzung erstellt und zum Senden von Nachrichten an ein Ziel verwendet wird.

Er kann wie folgt als eigenständiges Objekt erstellt werden: 

```java
JMSProducer producer = context.createProducer();
```

Alternativ kann der Producer auch zur Laufzeit erstellt werden, wenn eine Nachricht gesendet werden muss:

```java
context.createProducer().send(destination, message);
```

### <a name="jms-message-consumers"></a>JMS-Nachrichtenconsumer

Ein Nachrichtenconsumer ist ein Objekt, das durch einen JMSContext oder eine Sitzung erstellt und zum Empfangen von an ein Ziel gesendeten Nachrichten verwendet wird. Er kann wie folgt erstellt werden:

```java
JMSConsumer consumer = context.createConsumer(dest);
```

#### <a name="synchronous-receives-via-receive-method"></a>Synchrone Empfangsvorgänge per receive()-Methode

Der Nachrichtenconsumer bietet eine synchrone Möglichkeit zum Empfangen von Nachrichten vom Ziel über die `receive()`-Methode.

Wenn keine Argumente und kein Timeout festgelegt wurden oder der Timeoutwert 0 angegeben wurde, blockiert der Consumer so lange, bis die Nachricht eintrifft oder die Verbindung getrennt wird (je nachdem, welches Ereignis früher eintritt).

```java
Message m = consumer.receive();
Message m = consumer.receive(0);
```

Wenn ein positives Argument ungleich 0 angegeben wird, blockiert der Consumer so lange, bis dieser Timer abgelaufen ist.

```java
Message m = consumer.receive(1000); // time out after one second.
```

#### <a name="asynchronous-receives-with-jms-message-listeners"></a>Asynchrone Empfangsvorgänge mit JMS-Nachrichtenlistenern

Ein Nachrichtenlistener ist ein Objekt, das zur asynchronen Verarbeitung von Nachrichten an einem Ziel verwendet wird. Er implementiert die `MessageListener`-Schnittstelle, die die `onMessage`-Methode enthält, in der sich die spezifische Geschäftslogik befinden muss.

Ein Nachrichtenlistenerobjekt muss mithilfe der `setMessageListener`-Methode für einen bestimmten Nachrichtenconsumer instanziiert und registriert werden.

```java
Listener myListener = new Listener();
consumer.setMessageListener(myListener);
```

### <a name="consuming-from-topics"></a>Nutzen von Themen

[JMS-Nachrichtenconsumer](#jms-message-consumers) werden für ein [Ziel](#jms-destination) erstellt, bei dem es sich um eine Warteschlange oder ein Thema handeln kann.

Consumer für Warteschlangen sind einfach clientseitige Objekte, die im Kontext der Sitzung (und der Verbindung) zwischen Clientanwendung und Azure Service Bus existieren.

Consumer für Themen bestehen jedoch aus zwei Teilen: 
   * Ein **clientseitiges Objekt**, das im Kontext der Sitzung (oder von JMSContext) existiert
   * Ein **Abonnement**, das eine Entität in Azure Service Bus ist

Die Abonnements sind [hier](java-message-service-20-entities.md#java-message-service-jms-subscriptions) dokumentiert und können in einer der folgenden Formen vorliegen: 
   * Freigegebene dauerhafte Abonnements
   * Freigegebene nicht dauerhafte Abonnements
   * Nicht freigegebene dauerhafte Abonnements
   * Nicht freigegebene nicht dauerhafte Abonnements

### <a name="jms-queue-browsers"></a>JMS-Warteschlangenbrowser

Die JMS-API stellt ein `QueueBrowser`-Objekt bereit, das es der Anwendung ermöglicht, die Nachrichten in der Warteschlange zu durchsuchen und die Headerwerte jeder Nachricht anzuzeigen.

Ein Warteschlangenbrowser kann wie folgt mit dem JMSContext erstellt werden:

```java
QueueBrowser browser = context.createBrowser(queue);
```

> [!NOTE]
> Die JMS-API stellt keine API zum Durchsuchen eines Themas bereit.
>
> Das liegt daran, dass das Thema selbst die Nachrichten nicht speichert. Sobald eine Nachricht an ein Thema gesendet wird, wird sie an die jeweiligen Abonnements weitergeleitet.
>

### <a name="jms-message-selectors"></a>JMS-Nachrichtenselektoren

Nachrichtenselektoren können von empfangenden Anwendungen zum Filtern der empfangenen Nachrichten verwendet werden. Mit Nachrichtenselektoren lagert die empfangende Anwendung das Filtern von Nachrichten an den JMS-Anbieter (in diesem Fall Azure Service Bus) aus, statt die Filterung selbst durchzuführen.

Selektoren können beim Erstellen der folgenden Consumer verwendet werden: 
   * Freigegebene dauerhafte Abonnements
   * Nicht freigegebene dauerhafte Abonnements
   * Freigegebene nicht dauerhafte Abonnements
   * Nicht freigegebene nicht dauerhafte Abonnements
   * Warteschlangenbrowser


## <a name="summary"></a>Zusammenfassung

In diesem Entwicklerleitfaden wurde veranschaulicht, wie Java-Clientanwendungen, die Java Message Service (JMS) verwenden, eine Verbindung mit Azure Service Bus herstellen können.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Service Bus und Details zu JMS-Entitäten (Java Message Service) finden Sie unter den folgenden Links: 
* [Service Bus: Warteschlangen, Themen und Abonnements](service-bus-queues-topics-subscriptions.md)
* [Service Bus: Java Message Service-Entitäten](service-bus-queues-topics-subscriptions.md#java-message-service-jms-20-entities)
* [AMQP 1.0-Unterstützung in Azure Service Bus](service-bus-amqp-overview.md)
* [Entwicklerhandbuch für Service Bus AMQP 1.0](service-bus-amqp-dotnet.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Java Message Service-API(externes Oracle-Dokument)](https://docs.oracle.com/javaee/7/api/javax/jms/package-summary.html)
* [Informationen zum Migrieren von ActiveMQ zu Azure Service Bus](migrate-jms-activemq-to-servicebus.md)
