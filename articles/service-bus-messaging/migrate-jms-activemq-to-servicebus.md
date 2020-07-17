---
title: Migrieren von JMS-Anwendungen (Java Message Service) von ActiveMQ zu Azure Service Bus | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie vorhandene JMS-Anwendungen migrieren, die mit Active MQ interagieren, um mit Azure Service Bus zu interagieren.
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2020
ms.author: aschhab
ms.openlocfilehash: 3da4f693f4cfec47c5456a0c5998f58f5fe02949
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86122199"
---
# <a name="migrate-existing-java-message-service-jms-20-applications-from-active-mq-to-azure-service-bus"></a>Migrieren vorhandener JMS 2.0-Anwendungen (Java Message Service) von Active MQ zu Azure Service Bus

Azure Service Bus unterstützt Java-/J2EE- und Spring-Workloads, die die JMS 2.0-API (Java Message Service) über AMQP (Advanced Message Queueing Protocol) nutzen.

Dieser Leitfaden beschreibt, was Sie beachten sollten, wenn Sie eine vorhandene JJMS 2.0-Anwendung (Java Message Service) ändern möchten, die mit einem JMS-Broker (insbesondere Apache ActiveMQ oder Amazon MQ) interagiert, um mit Azure Service Bus zu interagieren.

## <a name="before-you-start"></a>Vorbereitung

### <a name="differences-between-azure-service-bus-and-apache-activemq"></a>Unterschiede zwischen Azure Service Bus und Apache ActiveMQ

Sowohl Azure Service Bus als auch Apache ActiveMQ sind Nachrichtenbroker, die als JMS-Anbieter fungieren, damit Clientanwendungen Nachrichten senden und empfangen können. Beide ermöglichen Punkt-zu-Punkt-Semantik mit **Warteschlangen** und Veröffentlichen/Abonnieren-Semantik mit **Themen** und **Abonnements**. 

Trotzdem sind einige Unterschiede zwischen beiden Brokern zu beachten.

| Category | Active MQ | Azure Service Bus |
| --- | --- | --- |
| Anwendungstiering | Gruppiert monolithisch | Zweistufig <br> (Gateway und Back-End) |
| Protokollunterstützung | <ul> <li>AMQP</li> <li> STOMP </li> <li> OpenWire </li> </ul> | AMQP |
| Bereitstellungsmodus | <ul> <li> IaaS (lokal) </li> <li> Amazon MQ (verwaltete PaaS) </li> | Verwaltete PaaS |
| Nachrichtengröße | Vom Benutzer konfigurierbar | 1 MB (Premium-Tarif) |
| Hochverfügbarkeit | Vom Kunden verwaltet | Von der Plattform verwaltet |
| Notfallwiederherstellung | Vom Kunden verwaltet | Von der Plattform verwaltet | 

### <a name="current-supported-and-unsupported-features"></a>Zurzeit unterstützte und nicht unterstützte Funktionen

[!INCLUDE [service-bus-jms-features-list](../../includes/service-bus-jms-feature-list.md)]

### <a name="caveats"></a>Vorbehalte

Die zweistufige Natur von Azure Service Bus bietet verschiedene Funktionen für Geschäftskontinuität (Hochverfügbarkeit und Notfallwiederherstellung). Bei der Verwendung von JMS-Features gibt es jedoch einige Überlegungen zu beachten.

#### <a name="service-upgrades"></a>Dienstupgrades

Bei Service Bus-Upgrades und -Neustarts werden temporäre Warteschlangen oder Themen gelöscht.

Wenn die Anwendung empfindlich auf Datenverlust in temporären Warteschlangen oder Themen reagiert, wird empfohlen, **keine** temporären Warteschlangen oder Themen zu verwenden und stattdessen dauerhafte Warteschlangen, Themen und Abonnements zu nutzen.

#### <a name="data-migration"></a>Datenmigration

Im Rahmen der Migration/Änderung von Clientanwendungen für die Interaktion mit Azure Service Bus werden die in ActiveMQ gespeicherten Daten nicht zu Service Bus migriert.

Eine benutzerdefinierte Anwendung muss möglicherweise ActiveMQ-Warteschlangen, -Themen und -Abonnements entfernen und die Nachrichten in Warteschlangen, Themen und Abonnements von Service Bus erneut wiedergeben.

#### <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung

Rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC), die von Azure ActiveDirectory unterstützt wird, ist der bevorzugte Authentifizierungsmechanismus für Azure Service Bus.

RBAC wird jedoch aufgrund fehlender anspruchsbasierter Authentifizierung durch Apache QPID JMS derzeit nicht unterstützt.

Zurzeit wird nur Authentifizierung mit SAS-Schlüsseln unterstützt.

## <a name="pre-migration"></a>Vor der Migration

### <a name="version-check"></a>Versionsüberprüfung

Im Folgenden finden Sie die Komponenten, die beim Schreiben der JMS-Anwendungen und der unterstützten spezifischen Versionen verwendet werden. 

| Komponenten | Version |
|---|---|
| JMS-API (Java Message Service) | 1.1 oder höher |
| AMQP-Protokoll | 1.0 |

### <a name="ensure-that-amqp-ports-are-open"></a>Stellen Sie sicher, dass AMQP-Ports geöffnet sind.

Azure Service Bus unterstützt die Kommunikation über das AMQP-Protokoll. Zu diesem Zweck muss die Kommunikation über die Ports 5671 (AMQP) und 443 (TCP) aktiviert werden. Abhängig davon, wo die Clientanwendungen gehostet werden, benötigen Sie möglicherweise ein Supportticket, um die Kommunikation über diese Ports zuzulassen.

> [!IMPORTANT]
> Azure Service Bus unterstützt **nur** das AMQP 1.0-Protokoll.

### <a name="set-up-enterprise-configurations-vnet-firewall-private-endpoint-etc"></a>Einrichten von Unternehmenskonfigurationen (VNET, Firewall, privater Endpunkt usw.)

Azure Service Bus bietet verschiedene Features für Unternehmenssicherheit und Hochverfügbarkeit. Weitere Informationen hierzu finden Sie unter den folgenden Dokumentationslinks.

  * [VNET-Dienstendpunkte](service-bus-service-endpoints.md)
  * [Firewall](service-bus-ip-filtering.md)
  * [Dienstseitige Verschlüsselung mit kundenseitig verwaltetem Schlüssel (BYOK)](configure-customer-managed-key.md)
  * [Private Endpunkte](private-link-service.md)
  * [Authentifizierung und Autorisierung](service-bus-authentication-and-authorization.md)

### <a name="monitoring-alerts-and-tracing"></a>Überwachung, Warnungen und Ablaufverfolgung

Metriken werden für jeden Service Bus-Namespace in Azure Monitor veröffentlicht und können für Warnungen und dynamische Skalierung von Ressourcen genutzt werden, die dem Namespace zugeordnet sind.

Weitere Informationen zu den verschiedenen Metriken und zum Einrichten von Warnungen für diese finden Sie unter [Service Bus-Metriken in Azure Monitor](service-bus-metrics-azure-monitor.md).

Auch zu [clientseitiger Ablaufverfolgung für Datenvorgänge](service-bus-end-to-end-tracing.md) und [Betriebs-/Diagnoseprotokollierung für Verwaltungsvorgänge](service-bus-diagnostic-logs.md) finden Sie weitere Informationen.

### <a name="metrics---newrelic"></a>Metriken: NewRelic

Im folgenden finden Sie eine praktische Anleitung dazu, welche Metriken von ActiveMQ welchen Metriken in Azure Service Bus zugeordnet sind. Auf die folgenden Metriken wird von NewRelicverwiesen.

  * [ActiveMQ/Amazon MQ NewRelic-Metriken](https://docs.newrelic.com/docs/integrations/amazon-integrations/aws-integrations-list/aws-mq-integration)
  * [Azure Service Bus NewRelic-Metriken](https://docs.newrelic.com/docs/integrations/microsoft-azure-integrations/azure-integrations-list/azure-service-bus-monitoring-integration)

> [!NOTE]
> Derzeit verfügt NewRelic nicht über direkte nahtlose Integration in ActiveMQ, aber es sind Metriken für Amazon MQ verfügbar.
> Da Amazon MQ von ActiveMQ abgeleitet ist, ordnet der nachfolgende Leitfaden die NewRelic-Metriken von AmazonMQ Azure Service Bus zu.
>

|Metrikgruppierung| AmazonMQ/Active MQ-Metrik | Azure Service Bus-Metrik |
|------------|---------------------------|--------------------------|
|Broker|`CpuUtilization`|`CPUXNS`|
|Broker|`MemoryUsage`|`WSXNS`|
|Broker|`CurrentConnectionsCount`|`activeConnections`|
|Broker|`EstablishedConnectionsCount`|`activeConnections` + `connectionsClosed`|
|Broker|`InactiveDurableTopicSubscribersCount`|Nutzen von Ereignisabonnementmetriken|
|Broker|`TotalMessageCount`|Nutzen der Warteschlangen-/Thema-/Abonnementebene `activeMessages`|
|Warteschlange/Thema|`EnqueueCount`|`incomingMessages`|
|Warteschlange/Thema|`DequeueCount`|`outgoingMessages`|
|Warteschlange|`QueueSize`|`sizeBytes`|



## <a name="migration"></a>Migration

Die folgenden Schritte müssen ausgeführt werden, um Ihre vorhandene JMS 2.0-Anwendung für die Interaktion mit Azure Service Bus zu migrieren.

### <a name="export-topology-from-activemq-and-create-the-entities-in-azure-service-bus-optional"></a>Exportieren der Topologie aus ActiveMQ und Erstellen der Entitäten in Azure Service Bus (optional)

Um sicherzustellen, dass die Clientanwendungen nahtlos eine Verbindung mit Azure Service Bus herstellen können, muss die Topologie (die Warteschlangen, Themen und Abonnements enthält) von **Apache ActiveMQ** zu **Azure Service Bus** migriert werden.

> [!NOTE]
> Für JMS-Anwendungen (Java Message Service) ist das Erstellen von Warteschlangen, Themen und Abonnements ein Laufzeitvorgang. Die meisten JMS-Anbieter (Nachrichtenbroker) bieten eine Funktion zum Erstellen von *Warteschlangen*, *Themen* und *Abonnements* zur Laufzeit.
>
> Daher ist der oben genannte Schritt optional.
>
> Um sicherzustellen, dass Ihre Anwendung über die Berechtigungen zum Erstellen der Topologie zur Laufzeit verfügt, stellen Sie sicher, dass die Verbindungszeichenfolge mit den ***SAS-Berechtigungen „Verwalten“*** verwendet wird.

Gehen Sie dazu folgendermaßen vor: 
  * Verwenden Sie die [ActiveMQ-Befehlszeilentools](https://activemq.apache.org/activemq-command-line-tools-reference), um die Topologie zu exportieren.
  * Erstellen Sie die gleiche Topologie mit einer [Azure Resource Manager-Vorlage](../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md) erneut.
  * Führen Sie die Azure Resource Manager-Vorlage aus.


### <a name="import-the-maven-dependency-for-service-bus-jms-implementation"></a>Importieren der Maven-Abhängigkeit für die Service Bus JMS-Implementierung

Um eine nahtlose Verbindung mit Azure Service Bus sicherzustellen, muss das ***azure-servicebus-jms***-Paket wie unten beschrieben der Maven-Datei `pom.xml` als Abhängigkeit hinzugefügt werden.

```xml
<dependencies>
...
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus-jms</artifactId>
    </dependency>
...
</dependencies>
```

### <a name="application-server-configuration-changes"></a>Änderungen der Anwendungsserverkonfiguration

Dieser Teil ist für den Anwendungsserver benutzerdefiniert, auf dem Ihre Clientanwendungen gehostet werden, die eine Verbindung mit Active MQ herstellen.

#### <a name="tomcat"></a>Tomcat

Hier beginnen wir mit der Konfiguration, die für Active MQ spezifisch ist, wie in der Datei `/META-INF/context.xml` gezeigt.

```XML
<Context antiJARLocking="true">
    <Resource
        name="jms/ConnectionFactory"
        auth="Container"
        type="org.apache.activemq.ActiveMQConnectionFactory"
        description="JMS Connection Factory"
        factory="org.apache.activemq.jndi.JNDIReferenceFactory"
        brokerURL="tcp://localhost:61616"
        brokerName="LocalActiveMQBroker"
        useEmbeddedBroker="false"/>

    <Resource name="jms/topic/MyTopic"
        auth="Container"
        type="org.apache.activemq.command.ActiveMQTopic"
        factory="org.apache.activemq.jndi.JNDIReferenceFactory"
        physicalName="MY.TEST.FOO"/>
    <Resource name="jms/queue/MyQueue"
        auth="Container"
        type="org.apache.activemq.command.ActiveMQQueue"
        factory="org.apache.activemq.jndi.JNDIReferenceFactory"
        physicalName="MY.TEST.FOO.QUEUE"/>
</Context>
```

die wie unten gezeigt angepasst werden kann, um auf Azure Service Bus zu verweisen.

```xml
<Context antiJARLocking="true">
    <Resource
        name="jms/ConnectionFactory"
        auth="Container"
        type="com.microsoft.azure.servicebus.jms.ServiceBusJmsConnectionFactory"
        description="JMS Connection Factory"
        factory="org.apache.qpid.jms.jndi.JNDIReferenceFactory"
        connectionString="<INSERT YOUR SERVICE BUS CONNECTION STRING HERE>"/>

    <Resource name="jms/topic/MyTopic"
        auth="Container"
        type="org.apache.qpid.jms.JmsTopic"
        factory="org.apache.qpid.jms.jndi.JNDIReferenceFactory"
        physicalName="MY.TEST.FOO"/>
    <Resource name="jms/queue/MyQueue"
        auth="Container"
        type="org.apache.qpid.jms.JmsQueue"
        factory="org.apache.qpid.jms.jndi.JNDIReferenceFactory"
        physicalName="MY.TEST.FOO.QUEUE"/>
</Context>
```

#### <a name="spring-applications"></a>Spring-Anwendungen

##### <a name="update-applicationproperties-file"></a>Aktualisieren der Datei `application.properties`

Wenn eine Spring Boot-Anwendung verwendet wird, um eine Verbindung mit ActiveMQ herzustellen.

Ziel ist es, die ActiveMQ-spezifischen Eigenschaften aus der Datei `application.properties` zu ***entfernen***.

```properties
spring.activemq.broker-url=<ACTIVEMQ BROKER URL>
spring.activemq.user=<ACTIVEMQ USERNAME>
spring.activemq.password=<ACTIVEMQ PASSWORD>
```

Anschließend müssen Sie der Datei `application.properties` dann die für Service Bus spezifischen Eigenschaften ***hinzufügen***.

```properties
azure.servicebus.connection-string=Endpoint=myEndpoint;SharedAccessKeyName=mySharedAccessKeyName;SharedAccessKey=mySharedAccessKey
```

##### <a name="replace-the-activemqconnectionfactory-with-servicebusjmsconnectionfactory"></a>Ersetzen von ActiveMQConnectionFactory durch ServiceBusJmsConnectionFactory

Der nächste Schritt besteht darin, die Instanz von ActiveMQConnectionFactory durch ServiceBusJmsConnectionFactory zu ersetzen.

> [!NOTE] 
> Die tatsächlichen Codeänderungen sind spezifisch für die Anwendung und die Art der Verwaltung von Abhängigkeiten, aber das folgende Beispiel enthält eine Anleitung dazu, ***was*** geändert werden sollte.
>

Zuvor instanziieren Sie möglicherweise ein Objekt der ActiveMQConnectionFactory wie unten beschrieben.

```java

String BROKER_URL = "<URL of the hosted ActiveMQ broker>";
ConnectionFactory factory = new ActiveMQConnectionFactory(BROKER_URL);

Connection connection = factory.createConnection();
connection.start();

```

Dies würde zum Instanziieren eines Objekts von ServiceBusJmsConnectionFactory geändert werden.

```java

ServiceBusJmsConnectionFactorySettings settings = new ServiceBusJmsConnectionFactorySettings();
String SERVICE_BUS_CONNECTION_STRING = "<Service Bus Connection string>";

ConnectionFactory factory = new ServiceBusJmsConnectionFactory(SERVICE_BUS_CONNECTION_STRING, settings);

Connection connection = factory.createConnection();
connection.start();

```

## <a name="post-migration"></a>Nach der Migration

Da Sie die Anwendung nun so geändert haben, dass sie mit dem Senden von Nachrichten an und Empfangen von Nachrichten von Azure Service Bus beginnt, sollten Sie überprüfen, ob sie wie erwartet funktioniert. Sobald dies erledigt ist, können Sie den Anwendungsstapel weiter verfeinern und modernisieren.

## <a name="next-steps"></a>Nächste Schritte

Nutzen Sie [Spring Boot-Starter für Azure Service Bus JMS](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-boot-starter-java-app-with-azure-service-bus) für die nahtlose Integration in Service Bus.

Weitere Informationen zum Service Bus-Messaging sowie zu JMS (Java Messaging Service) finden Sie in folgenden Themen:

* [Service Bus JMS](service-bus-java-how-to-use-jms-api-amqp.md)
* [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
