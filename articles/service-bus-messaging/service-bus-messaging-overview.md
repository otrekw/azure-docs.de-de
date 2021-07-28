---
title: Übersicht über Azure Service Bus-Messaging | Microsoft-Dokumentation
description: Dieser Artikel enthält eine allgemeine Übersicht über Azure Service Bus (ein vollständig verwalteter Nachrichtenbroker für die Unternehmensintegration).
ms.topic: overview
ms.date: 06/11/2021
ms.openlocfilehash: 932fde55bdd315993de5375706f7d5d3621cef6d
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112030711"
---
# <a name="what-is-azure-service-bus"></a>Was ist Azure Service Bus?
Bei Microsoft Azure Service Bus handelt es sich um einen vollständig verwalteten Nachrichtenbroker für Unternehmen mit Nachrichtenwarteschlangen und Veröffentlichen-Abonnieren-Themen. Service Bus wird verwendet, um Anwendungen und Dienste voneinander zu entkoppeln und so die folgenden Vorteile zu erzielen:

- Übergreifender Lastenausgleich für konkurrierende Worker
- Sicherheit beim Routing und der Übertragung von Daten sowie Kontrolle über Dienst- und Anwendungsgrenzen hinweg
- Koordinierung von Transaktionsausgaben mit hohen Anforderungen an die Zuverlässigkeit 

## <a name="overview"></a>Übersicht
Die Datenübertragung zwischen verschiedenen Anwendungen und Diensten erfolgt mithilfe von **Nachrichten**. Eine Nachricht ist ein Container, der über Metadaten verfügt und Daten enthält. Bei den Daten kann es sich um eine beliebige Art von Informationen handeln, z. B. strukturierte Daten in den folgenden gängigen Formaten: JSON, XML, Apache Avro, Nur-Text.

Im Anschluss finden Sie einige gängige Messagingszenarien:

* **Messaging**: Übertragung von Geschäftsdaten (beispielsweise Verkäufe/Bestellungen, Journale oder Bestandsbewegungen)
* **Entkoppelung von Anwendungen**: Höhere Zuverlässigkeit und bessere Skalierbarkeit von Anwendungen und Diensten. Producer und Consumer müssen nicht gleichzeitig online bzw. immer verfügbar sein. Die [Last wird verteilt](/azure/architecture/patterns/queue-based-load-leveling), damit ein Dienst nicht aufgrund von Datenverkehrsspitzen überlastet wird. 
* **Lastenausgleich**: Ermöglicht es, dass mehrere [konkurrierende Consumer](/azure/architecture/patterns/competing-consumers) gleichzeitig Daten aus einer Warteschlange auslesen und die Consumer dann jeweils auf sichere Weise die exklusive Eigentümerschaft für bestimmte Nachrichten erhalten. 
* **Themen und Abonnements**: Ermöglichen 1:*n*-Beziehungen zwischen [Herausgebern und Abonnenten](/azure/architecture/patterns/publisher-subscriber), damit Abonnenten bestimmte Nachrichten aus einem Datenstrom mit veröffentlichten Nachrichten auswählen können.
* **Transaktionen**: Ermöglichen Ihnen die Durchführung mehrerer Vorgänge im Rahmen einer atomaren Transaktion. Beispielsweise können die folgenden Vorgänge im Rahmen einer Transaktion durchgeführt werden.  

    1. Abrufen einer Nachricht aus einer Warteschlange
    2. Posten von Ergebnissen in einer oder mehreren anderen Warteschlangen
    3. Verschieben der Eingabenachricht aus der ursprünglichen Warteschlange 
    
    Die Ergebnisse werden für nachgeschaltete Consumer erst im Erfolgsfall sichtbar, z. B. der erfolgreiche Abgleich der Eingabenachricht. Dies ermöglicht die Verwendung einer Semantik mit einmaliger Verarbeitung. Dieses Transaktionsmodell ist eine stabile Grundlage für das Muster für [Kompensierende Transaktionen](/azure/architecture/patterns/compensating-transaction) im weiteren Lösungskontext. 
* **Nachrichtensitzungen**: Hiermit wird eine umfassende Koordinierung von Workflows und Multiplex-Übertragungen implementiert, für die eine strikte Nachrichtensortierung oder -verzögerung erforderlich ist.

Wenn Sie bereits mit anderen Nachrichtenbrokern wie Apache ActiveMQ vertraut sind, werden Ihnen die Service Bus-Konzepte bekannt vorkommen, weil sie ähnlich sind. Da es sich bei Service Bus um ein PaaS-Angebot (Platform-as-a-Service) handelt, besteht ein wichtiger Unterschied darin, dass Sie sich nicht um die folgenden Aktionen kümmern müssen. Azure übernimmt die Durchführung dieser Aufgaben für Sie. 

- Achten auf Hardwarefehler 
- Sicherstellen der Patches für die Betriebssysteme oder die Produkte
- Anordnen von Protokollen und Verwalten des Speicherplatzes
- Verarbeiten von Sicherungsvorgängen
- Ausführen von Failovern auf einen Reservecomputer

## <a name="compliance-with-standards-and-protocols"></a>Sicherstellen der Konformität mit Standards und Protokollen

Das primäre Verbindungsprotokoll für Service Bus ist [Advanced Messaging Queueing Protocol (AMQP) 1.0](service-bus-amqp-overview.md) (offener ISO/IEC-Standard). Es ermöglicht Kunden das Schreiben von Anwendungen für die Verwendung mit Service Bus und lokalen Brokern wie ActiveMQ oder RabbitMQ. Der [Protokollleitfaden zu AMQP](service-bus-amqp-protocol-guide.md) enthält ausführliche Informationen, die Ihnen weiterhelfen, falls Sie eine Abstraktion dieser Art erstellen möchten.

[Service Bus Premium](service-bus-premium-messaging.md) ist vollständig mit der [Java Message Service (JMS) 2.0](how-to-use-java-message-service-20.md)-API für Java/Jakarta EE konform. Darüber hinaus wird von Service Bus Standard auch die JMS 1.1-Unterversion für Warteschlangen unterstützt. JMS ist eine gängige Abstraktion für Nachrichtenbroker und kann mit vielen Anwendungen und Frameworks integriert werden, z. B. das häufig verwendete Spring-Framework. Für den Wechsel von anderen Brokern zu Azure Service Bus müssen Sie lediglich die Topologie mit den Warteschlangen und Themen neu erstellen und die Abhängigkeiten und die Konfiguration für den Clientanbieter ändern. Ein Beispiel finden Sie im [Migrationsleitfaden für ActiveMQ](migrate-jms-activemq-to-servicebus.md).

## <a name="concepts-and-terminology"></a>Konzepte und Terminologie 
In diesem Abschnitt werden die Konzepte und die Terminologie von Service Bus beschrieben.

### <a name="queues"></a>Warteschlangen
Nachrichten werden an **Warteschlangen** gesendet und daraus empfangen. Warteschlangen dienen zum Speichern von Nachrichten, bis die empfangende Anwendung empfangs- und verarbeitungsbereit ist.

![Warteschlange](./media/service-bus-messaging-overview/about-service-bus-queue.png)

Nachrichten in Warteschlangen werden bei ihrem Eingang sortiert und mit einem Zeitstempel versehen. Nachdem die Nachricht vom Broker akzeptiert wurde, wird sie immer dauerhaft im dreifach redundanten Speicher vorgehalten – verteilt auf die Verfügbarkeitszonen, falls die Nutzung in Zonen für den Namespace aktiviert ist. Von Service Bus werden Nachrichten niemals im Arbeitsspeicher oder im flüchtigen Speicher belassen, nachdem sie dem Client als akzeptiert gemeldet wurden.

Nachrichten werden im **Pull-Modus** (also nur auf Anforderung) zugestellt. Im Gegensatz zum Busy-Polling-Modell einiger anderer Cloudwarteschlangen ist der Pullvorgang langlebig und wird erst abgeschlossen, nachdem eine Nachricht verfügbar ist. 

### <a name="topics"></a>Themen

Nachrichten können auch unter Verwendung von **Themen** gesendet und empfangen werden. Themen sind in Veröffentlichungs-/Abonnementszenarien hilfreich. Bei der Punkt-zu-Punkt-Kommunikation werden dagegen häufig Warteschlangen verwendet.

![Thema](./media/service-bus-messaging-overview/about-service-bus-topic.png)

Themen können über mehrere, unabhängige Abonnements verfügen, die an das Thema angefügt sind und ansonsten genauso wie Warteschlangen auf der Empfängerseite funktionieren. Ein Abonnent eines Themas kann eine Kopie jeder Nachricht erhalten, die an das Thema gesendet wird. Bei Abonnements handelt es sich um benannte Entitäten. Abonnements sind standardmäßig dauerhafter Natur, aber sie können auch so konfiguriert werden, dass sie ablaufen und dann automatisch gelöscht werden. Über die JMS-API ermöglicht Service Bus Premium Ihnen auch die Erstellung von flüchtigen Abonnements, die nur für die Dauer der Verbindung bestehen.

Sie können Regeln für ein Abonnement definieren. Eine Abonnementregel verfügt über einen **Filter**, um für die Nachricht eine Bedingung für das Kopieren in das Abonnement zu definieren, und über eine optionale **Aktion**, mit der die Metadaten der Nachricht geändert werden können. Weitere Informationen finden Sie unter [Themenfilter und -aktionen](topic-filters.md). Dieses Feature ist in den folgenden Szenarien nützlich:

- Sie möchten nicht, dass ein Abonnement alle Nachrichten empfängt, die an ein Thema gesendet werden.
- Sie möchten Nachrichten mit zusätzlichen Metadaten kennzeichnen, wenn sie ein Abonnement durchlaufen.

### <a name="namespaces"></a>Namespaces
Ein Namespace ist ein Container für alle Messagingkomponenten (Warteschlangen und Themen). Ein einzelner Namespace kann mehrere Warteschlangen und Themen enthalten, und Namespaces fungieren häufig als Anwendungscontainer. 

Ein Namespace ist damit vergleichbar, was in der Terminologie anderer Broker als Server bezeichnet wird, aber die Konzepte sind nicht völlig äquivalent. Ein Service Bus-Namespace ist Ihr eigenes Kapazitätssegment eines großen Clusters, der aus Dutzenden von aktiven virtuellen Computern besteht. Optional kann er über drei [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md) reichen. So profitieren Sie von allen Vorteilen in Bezug auf die Verfügbarkeit und Stabilität, die sich aus der umfassenden Ausführung des Nachrichtenbrokers ergeben. Zudem müssen Sie sich nicht mit den zugrunde liegenden komplexen Zusammenhängen beschäftigen. Service Bus steht für serverloses Messaging.

## <a name="advanced-concepts"></a>Erweiterte Konzepte
Service Bus enthält erweiterte Features wie Nachrichtensitzungen, geplante Übermittlungen und Transaktionen für komplexere Messagingszenarien. Weitere Informationen finden Sie unter [Azure Service Bus: Erweiterte Features](advanced-features-overview.md).

## <a name="client-libraries"></a>Clientbibliotheken

Vollständig unterstützte Service Bus-Clientbibliotheken sind über das Azure SDK verfügbar.

- [Azure Service Bus für .NET](/dotnet/api/overview/azure/service-bus?preserve-view=true)
- [Azure Service Bus-Bibliotheken für Java](/java/api/overview/azure/servicebus?preserve-view=true)
- [Azure Service Bus-Anbieter für Java JMS 2.0](how-to-use-java-message-service-20.md)
- [Azure Service Bus-Module für JavaScript und TypeScript](/javascript/api/overview/azure/service-bus?preserve-view=true)
- [Azure Service Bus-Bibliotheken für Python](/python/api/overview/azure/servicebus?preserve-view=true)

Das [primäre Protokoll von Azure Service Bus ist AMQP 1.0](service-bus-amqp-overview.md). Es kann über jeden AMQP 1.0-konformen Protokollclient genutzt werden. Einige Open-Source-AMQP-Clients verfügen über Beispiele, mit denen die Service Bus-Interoperabilität explizit demonstriert wird. Im [AMQP 1.0-Protokollleitfaden](service-bus-amqp-protocol-guide.md) erfahren Sie, wie Sie die Features von Service Bus direkt mit AMQP 1.0-Clients verwenden.

[!INCLUDE [messaging-oss-amqp-stacks.md](../../includes/messaging-oss-amqp-stacks.md)]

## <a name="integration"></a>Integration

Service Bus kann vollständig mit vielen Microsoft- und Azure-Diensten integriert werden, z. B.:

* [Event Grid](service-bus-to-event-grid-integration-example.md)
* [Logik-Apps](../connectors/connectors-create-api-servicebus.md)
* [Azure-Funktionen](../azure-functions/functions-bindings-service-bus.md)
* [Power Platform](../connectors/connectors-create-api-servicebus.md)
* [Dynamics 365](/dynamics365/fin-ops-core/dev-itpro/business-events/how-to/how-to-servicebus)
* [Azure Stream Analytics](../stream-analytics/stream-analytics-define-outputs.md)

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten mit Service Bus-Messaging finden Sie in folgenden Artikeln:

* Eine Gegenüberstellung von Azure-Messagingdiensten finden Sie unter [Auswahl von Azure-Messagingdiensten: Event Grid, Event Hubs und Service Bus](../event-grid/compare-messaging-services.md?toc=%2fazure%2fservice-bus-messaging%2ftoc.json&bc=%2fazure%2fservice-bus-messaging%2fbreadcrumb%2ftoc.json).
* Sehen Sie sich die Schnellstartanleitung für [.NET](service-bus-dotnet-get-started-with-queues.md), [Java](service-bus-java-how-to-use-queues.md) oder [JMS](service-bus-java-how-to-use-jms-api-amqp.md) an.
* Informationen zur Verwaltung von Service Bus-Ressourcen finden Sie unter [Service Bus-Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/releases).
* Informationen zu Standard- und Premium-Tarifen und den jeweiligen Preisen finden Sie unter [Service Bus – Preise](https://azure.microsoft.com/pricing/details/service-bus/).
* Informationen zu Leistung und Wartezeit für den Premium-Tarif finden Sie unter [Premium-Messaging: Wie schnell ist es?](https://techcommunity.microsoft.com/t5/Service-Bus-blog/Premium-Messaging-How-fast-is-it/ba-p/370722).
