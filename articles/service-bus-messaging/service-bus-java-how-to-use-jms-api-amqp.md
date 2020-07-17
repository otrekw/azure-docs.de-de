---
title: Verwenden von AMQP mit der Java Message Service-API und Azure Service Bus
description: Erfahren Sie, wie Sie den Java Message Service (JMS) mit Azure Service Bus und Advanced Message Queuing Protocol (AMQP) 1.0 verwenden.
ms.topic: article
ms.date: 06/23/2020
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 04d2595951640b7fe878decfeb862863f06c17a2
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119156"
---
# <a name="use-the-java-message-service-jms-with-azure-service-bus-and-amqp-10"></a>Verwenden von Java Message Service (JMS) mit Azure Service Bus und AMQP 1.0

Unterstützung für das Protokoll **AMQP 1.0 (Advance Message Queueing Protocol)** in Azure Service Bus bedeutet, dass Sie Brokermessagingfunktionen für Warteschlangen und Veröffentlichen/Abonnieren mithilfe eines effizienten binären Protokolls auf unterschiedlichen Plattformen nutzen können. Zudem können Sie Anwendungen erstellen, deren Komponenten mit einer Mischung aus Sprachen, Frameworks und Betriebssystemen erstellt wurden.

In diesem Artikel wird beschrieben, wie die Messagingfunktionen von Azure Service Bus (Warteschlange und Veröffentlichen/Abonnieren von Themen) aus Java-Anwendungen mit dem beliebten API-Standard **Java Message Service (JMS)** über das AMQP-Protokoll verwendet werden.

## <a name="get-started-with-service-bus"></a>Erste Schritte mit Service Bus
In diesem Leitfaden wird davon ausgegangen, dass Sie bereits einen Service Bus-Namespace haben, der eine Warteschlange mit dem Namen `basicqueue` enthält. Falls nicht, können Sie [den Namespace und die Warteschlange](service-bus-create-namespace-portal.md) im [Azure-Portal](https://portal.azure.com) erstellen. Weitere Informationen zum Erstellen von Namespaces und Warteschlangen für Service Bus finden Sie unter [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md).

> [!NOTE]
> Partitionierte Warteschlangen und Themen unterstützen zudem AMQP. Weitere Informationen finden Sie unter [Partitionierte Messagingentitäten](service-bus-partitioning.md) und [AMQP 1.0-Unterstützung für partitionierte Warteschlangen und Themen von Service Bus](service-bus-partitioned-queues-and-topics-amqp-overview.md).
> 
>

## <a name="what-jms-features-are-supported"></a>Welche JMS-Funktionen werden unterstützt?

Im Folgenden finden Sie die JMS-Funktionen, die in Azure Service Bus unterstützt werden.

| Features | Tarif „Standard“ von Azure Service Bus: JMS 1.1 | Tarif „Premium“ von Azure Service Bus: JMS 2.0 (Vorschau) |
|---|---|---|
| Automatische Erstellung von Entitäten über AMQP | Nicht unterstützt | **Unterstützt** |
| Warteschlangen | **Unterstützt** | **Unterstützt** |
| Themen | **Unterstützt** | **Unterstützt** |
| Temporäre Warteschlangen | Nicht unterstützt <br/> (Erstellen Sie stattdessen eine reguläre Warteschlange mit festgelegter Angabe *AutoDeleteOnIdle*) | **Unterstützt** |
| Temporäre Themen | Nicht unterstützt | **Unterstützt** |
| Nachrichtenselektoren | Nicht unterstützt | **Unterstützt** |
| Warteschlangenbrowser | Nicht unterstützt <br/> (Verwenden Sie die *Peek*-Funktion der Service Bus-API) | **Unterstützt** |
| Freigegebene dauerhafte Abonnements | **Unterstützt** | **Unterstützt**|
| Nicht freigegebene dauerhafte Abonnements | Nicht unterstützt | **Unterstützt** |
| Freigegebene nicht dauerhafte Abonnements | Nicht unterstützt | **Unterstützt** |
| Nicht freigegebene nicht dauerhafte Abonnements | Nicht unterstützt | **Unterstützt** |
| Abonnement für dauerhafte Abonnements kündigen | Nicht unterstützt | **Unterstützt** |
| ReceiveNoWait | Nicht unterstützt | **Unterstützt** |
| Verteilte Transaktionen | Nicht unterstützt | Nicht unterstützt |
| Dauerhafter Terminus | Nicht unterstützt | Nicht unterstützt |

### <a name="additional-caveats-for-service-bus-standard-tier"></a>Zusätzliche Einschränkungen für Tarif „Standard“von Service Bus
Pro **Sitzung** ist nur ein **MessageConsumer** oder **MessageProducer** erlaubt. Falls Sie mehrere **MessageProducer** oder **MessageConsumer** in einer Anwendung benötigen, müssen Sie für diese jeweils eine eigene **Sitzung** erstellen.

## <a name="downloading-the-java-message-service-jms-client-library"></a>Herunterladen der JMS-Clientbibliothek (Java Message Service)

Zum Herstellen einer Verbindung mit Azure Service Bus und zum Nutzen der JMS-API (Java Message Service) über AMQP müssen die folgenden Bibliotheken verwendet werden. Diese müssen dem Buildpfad hinzugefügt werden, indem Sie das bevorzugte Tool für Abhängigkeitsverwaltung für Ihr Projekt verwenden.

Welche Clientbibliothek erforderlich ist, hängt davon ab, welcher Tarif verwendet wird.

### <a name="premium-tier---jms-20-over-amqp-preview"></a>Tarif „Premium“: JMS 2.0 über AMQP (Vorschau)

Um alle Vorschaufeatures zu nutzen, die für den Tarif „Premium“ von Azure Service Bus verfügbar sind, verwenden Sie die [Azure-servicebus-jms](https://search.maven.org/artifact/com.microsoft.azure/azure-servicebus-jms)-Bibliothek.

### <a name="standard-tier---jms-11-over-amqp"></a>Tarif „Standard“: JMS 1.1 über AMQP

Verwenden Sie die folgenden Bibliotheken, um die vom Tarif „Standard“ von Service Bus unterstützten JMS-Funktionen zu nutzen (siehe [welche JMS-Features werden unterstützt?](service-bus-java-how-to-use-jms-api-amqp.md#what-jms-features-are-supported)):

* [Geronimo JMS 1.1-Spezifikation](https://search.maven.org/artifact/org.apache.geronimo.specs/geronimo-jms_1.1_spec)
* [Qpid JMS-Client](https://search.maven.org/artifact/org.apache.qpid/qpid-jms-client)

> [!NOTE]
> JMS-JAR-Namen und -Versionen haben sich möglicherweise geändert. Ausführliche Informationen finden Sie unter [Qpid JMS (AMQP 1.0)](https://qpid.apache.org/maven.html#qpid-jms-amqp-10).
>

## <a name="coding-java-applications"></a>Programmieren von Java-Anwendungen

Nachdem die Abhängigkeiten importiert wurden, können die Java-Anwendungen in einer JMS-anbieteragnostischen Weise geschrieben werden.

Da sich die Tarife „Standard“ und „Premium“ von Azure Service Bus in den Abhängigkeiten und der Anzahl der von Ihnen unterstützten JMS-Features unterscheiden, unterscheidet sich das Programmiermodell für beide Tarife leicht.

> [!IMPORTANT]
> Die folgenden Anleitungen zeigen, wie Sie in einer einfachen Anwendung eine Verbindung mit Azure Service Bus herstellen.
>
> Da die meisten Unternehmensanwendungsarchitekturen über eine benutzerdefinierte Methode zum Verwalten von Abhängigkeiten und Konfigurationen verfügen, verwenden Sie die Informationen unten als Leitfaden, um zu verstehen, was erforderlich ist, und passen Sie diese Angaben dann entsprechend an Ihre Anwendung an.
>

### <a name="connecting-to-azure-service-bus-using-jms"></a>Herstellen einer Verbindung mit Azure Service Bus mithilfe von JMS

Zum Herstellen einer Verbindung mit Azure Service Bus mithilfe von JMS-Clients benötigen Sie die Angabe **ConnectionString**, die im [Azure-Portal](https://portal.azure.com) in den „SAS-Richtlinien“ unter **Primäre Verbindungszeichenfolge** verfügbar ist.


#### <a name="connecting-to-azure-service-bus-premium-over-jms-20-preview"></a>Herstellen einer Verbindung mit Azure Service Bus Premium über JMS 2.0 (Vorschau)

1. Instanziieren der `ServiceBusJmsConnectionFactorySettings`
    ```java
    ServiceBusJmsConnectionFactorySettings connFactorySettings = new ServiceBusJmsConnectionFactorySettings();

    connFactorySettings.setConnectionIdleTimeoutMS(20000);
    ```
2. Instanziieren Sie die `ServiceBusJmsConnectionFactory` mit dem entsprechenden `ServiceBusConnectionString`-Element.
    ```java
    String ServiceBusConnectionString = "<SERVICE_BUS_CONNECTION_STRING_WITH_MANAGE_PERMISSIONS>";
    ConnectionFactory factory = new ServiceBusJmsConnectionFactory(ServiceBusConnectionString, connFactorySettings);
    ```

3. Verwenden Sie die `ConnectionFactory`, um eine `Connection` und dann eine `Session` 
    ```java
    Connection connection = factory.createConnection();
    Session session = connection.createSession();
    ```
    oder einen `JMSContext` (für JMS 2.0-Clients) zu erstellen.

    ```java
    JMSContext jmsContext = factory.createContext();
    ```

#### <a name="connecting-to-azure-service-bus-standard-over-jms-11"></a>Herstellen einer Verbindung mit Azure Service Bus Standard über JMS 1.1

1. Fügen Sie die Azure Service Bus-Konfiguration in die JNDI-Eigenschaftendatei mit dem Namen **servicebus.properties** ein.
    ```properties
    # servicebus.properties - sample JNDI configuration
    
    # Register a ConnectionFactory in JNDI using the form:
    # connectionfactory.[jndi_name] = [ConnectionURL]
    connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
    ```

2. Einrichten des JNDI-Kontexts und Konfigurieren der ConnectionFactory
    ```java
    ConnectionStringBuilder csb = new ConnectionStringBuilder(connectionString);

    // set up JNDI context
    Hashtable<String, String> hashtable = new Hashtable<>();
    hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + "?amqp.idleTimeout=120000&amqp.traceFrames=true");
    hashtable.put("queue.QUEUE", "BasicQueue");
    hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
    Context context = new InitialContext(hashtable);
    
    ConnectionFactory factory = (ConnectionFactory) context.lookup("SBCF");
    ```
3. Verwenden Sie die `ConnectionFactory`, um eine `Connection` und dann eine `Session` zu erstellen.
    ```java
    Connection connection - factory.createConnection(csb.getSasKeyName(), csb.getSasKey());
    Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
    ```

### <a name="write-the-jms-application"></a>Schreiben der JMS-Anwendung

Nachdem die `Session` oder der `JMSContext` instanziiert wurde, kann die Anwendung die vertrauten JMS-APIs nutzen, um sowohl Verwaltungs- als auch Datenvorgänge auszuführen.

Welche APIs in den einzelnen Tarifen unterstützt werden, entnehmen Sie der Liste der [unterstützten JMS-Funktionen](service-bus-java-how-to-use-jms-api-amqp.md#what-jms-features-are-supported) für die Tarife „Standard“ und „Premium“.

## <a name="summary"></a>Zusammenfassung
In diesem Leitfaden wurde gezeigt, wie Java-Clientanwendungen, die Java Message Service (JMS) über AMQP 1.0 verwenden, mit Azure Service Bus interagieren können.

Sie können Service Bus AMQP 1.0 auch mit anderen Sprachen verwenden, unter anderem .NET, C, Python und PHP. Komponenten, die mit diesen verschiedenen Sprachen geschrieben wurden, können mit der AMQP 1.0-Unterstützung in Service Bus Nachrichten zuverlässig und bei voller Vertraulichkeit austauschen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Service Bus und Details zu JMS-Entitäten (Java Message Service) finden Sie unter den folgenden Links: 
* [Service Bus: Warteschlangen, Themen und Abonnements](service-bus-queues-topics-subscriptions.md)
* [Service Bus: Java Message Service-Entitäten](service-bus-queues-topics-subscriptions.md#java-message-service-jms-20-entities-preview)
* [AMQP 1.0-Unterstützung in Azure Service Bus](service-bus-amqp-overview.md)
* [Entwicklerhandbuch für Service Bus AMQP 1.0](service-bus-amqp-dotnet.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)

