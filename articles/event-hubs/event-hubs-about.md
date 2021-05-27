---
title: Was ist Azure Event Hubs? – ein Big Data-Erfassungsdienst | Microsoft-Dokumentation
description: Erfahren Sie mehr über Azure Event Hubs, einem Big Data-Streamingdienst, der Millionen von Ereignissen pro Sekunde erfasst.
ms.topic: overview
ms.date: 05/25/2021
ms.openlocfilehash: 54a37a61a8bb845d60da999e091fdf6657b961a2
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110373543"
---
# <a name="azure-event-hubs--a-big-data-streaming-platform-and-event-ingestion-service"></a>Azure Event Hubs: Big Data-Streamingplattform und Ereigniserfassungsdienst
Bei Azure Event Hubs handelt es sich um eine Big Data-Streamingplattform und einen Ereigniserfassungsdienst. Mit diesem Dienst können Millionen von Ereignissen pro Sekunde empfangen und verarbeitet werden. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden.

Event Hubs kann beispielsweise in den folgenden Szenarien verwendet werden:

- Anomalieerkennung (Betrug/Ausreißer)
- Anwendungsprotokollierung
- Analyse-Pipelines, z.B. Clickstreams
- Dashboards in Echtzeit
- Archivierung von Daten
- Transaktionsverarbeitung
- Verarbeitung der Benutzertelemetrie
- Streaming der Gerätetelemetrie

<iframe width="560" height="315" src="https://www.youtube.com/embed/45wgY-VSk9I" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## <a name="why-use-event-hubs"></a>Vorteile von Event Hubs
Daten sind nur wertvoll, wenn es eine einfache Möglichkeit für die Verarbeitung und das Abrufen frühzeitiger Erkenntnisse aus Datenquellen gibt. Event Hubs stellt eine verteilte Verarbeitungsplattform für Datenströme mit kurzer Wartezeit und nahtloser Integration sowie Daten und Analysediensten innerhalb und außerhalb von Azure bereit, um eine vollständige Big Data-Pipeline zu erstellen.

Event Hubs stellt die „Vordertür“ für eine Ereignispipeline dar. Dies wird in Lösungsarchitekturen oft als *Ereignisingestor* bezeichnet. Ein Ereigniserfasser ist eine Komponente oder ein Dienst zwischen Ereignisherausgeber und Ereignisverarbeitung zum Entkoppeln der Erzeugung eines Ereignisdatenstroms von der Verarbeitung dieser Ereignisse. Event Hubs bietet eine einheitliche Streamingplattform mit einem Aufbewahrungspuffer, durch den die Ereignisersteller von den Ereignisconsumern entkoppelt werden.


In den folgenden Abschnitten werden die wichtigsten Features des Azure Event Hubs-Diensts beschrieben:

## <a name="fully-managed-paas"></a>Vollständig verwaltete PaaS
Event Hubs ist eine vollständig verwaltete PaaS (Platform-as-a-Service) mit geringfügigem Konfigurations- oder Verwaltungsaufwand, sodass Sie sich auf Ihre Unternehmenslösungen konzentrieren können. [Event Hubs für Apache Kafka-Ökosysteme](event-hubs-for-kafka-ecosystem-overview.md) bietet Ihnen die Funktionen von PaaS-Kafka, ohne dass Sie Cluster verwalten, konfigurieren oder ausführen müssen.

## <a name="support-for-real-time-and-batch-processing"></a>Unterstützung für Echtzeit- und Batchverarbeitung
Erfassen, puffern, speichern und verarbeiten Sie Ihren Datenstrom in Echtzeit, um verwertbare Einblicke zu erhalten. Event Hubs verwendet ein [partitioniertes Consumermodell](event-hubs-scalability.md#partitions), sodass mehrere Anwendungen den Datenstrom gleichzeitig verarbeiten und Sie die Verarbeitungsgeschwindigkeit steuern können. Azure Event Hubs lässt sich für eine serverlose Architektur auch mit [Azure Functions](../azure-functions/index.yml) integrieren.

## <a name="capture-event-data"></a>Erfassen von Ereignisdaten
Erfassen Sie Ihre Daten mit [Capture](event-hubs-capture-overview.md) nahezu in Echtzeit in [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) oder [Azure Data Lake Storage](https://azure.microsoft.com/services/data-lake-store/) für die Langzeitaufbewahrung oder die Microbatch-Verarbeitung. Sie können dieses Verhalten für den gleichen Datenstrom erreichen, den Sie zum Ableiten von Echtzeitanalysen verwenden. Das Einrichten der Erfassung von Ereignisdaten geht schnell. Für das Ausführen fallen keine Verwaltungskosten an, und die Skalierung erfolgt automatisch mit  [Durchsatzeinheiten](event-hubs-scalability.md#throughput-units) oder [Verarbeitungseinheiten](event-hubs-scalability.md#processing-units) von Event Hubs. Mit Event Hubs können Sie sich anstelle auf die Datenerfassung auf die Datenverarbeitung konzentrieren.

## <a name="scalable"></a>Skalierbar
Mit Event Hubs können Sie mit Datenströmen in Megabytes anfangen und zu Gigabytes oder Terabytes erweitern. Das Feature für die [automatische Vergrößerung](event-hubs-auto-inflate.md) ist eine von vielen verfügbaren Optionen zum Skalieren der Anzahl von Durchsatzeinheiten oder Verarbeitungseinheiten, um Ihren Nutzungsanforderungen gerecht zu werden.

## <a name="rich-ecosystem"></a>Umfangreiches Ökosystem

Mit einem umfassenden Ökosystem, das auf dem branchenüblichen Standardprotokoll AMQP 1.0 basiert und in verschiedenen Sprachen verfügbar ist ([.NET](https://github.com/Azure/azure-sdk-for-net/), [Java](https://github.com/Azure/azure-sdk-for-java/), [Python](https://github.com/Azure/azure-sdk-for-python/), [JavaScript](https://github.com/Azure/azure-sdk-for-js/)), können Sie problemlos mit der Verarbeitung Ihrer Datenströme über Event Hubs beginnen. Mit allen unterstützten Clientsprachen wird tiefgreifende Integration gewährleistet. Das Ökosystem ermöglicht darüber hinaus die nahtlose Integration in Azure-Dienste wie Azure Stream Analytics und Azure Functions und damit die Erstellung serverloser Architekturen.

## <a name="event-hubs-for-apache-kafka"></a>Event Hubs für Apache Kafka
Mit [Event Hubs für Apache Kafka-Ökosysteme](event-hubs-for-kafka-ecosystem-overview.md) können darüber hinaus [Apache Kafka-Clients und -Anwendungen (1.0 und höher)](https://kafka.apache.org/) mit Event Hubs kommunizieren. Für Sie ist es nicht erforderlich, Ihre eigenen Kafka- und Zookeeper-Cluster einzurichten, zu konfigurieren und zu verwalten oder ein Kafka-as-a-Service-Angebot zu nutzen, das nicht ursprünglich von Azure stammt.

## <a name="event-hubs-premium-and-dedicated"></a>Event Hubs Premium und Dedicated 
Event Hubs **Premium** erfüllt High-End-Streaminganforderungen, die eine bessere Leistung, bessere Isolation mit vorhersagbarer Latenz und minimalen Störungen in einer verwalteten mehrinstanzenfähigen PaaS-Umgebung erfordern. Neben allen Features des Standard-Angebots bietet der Premium-Tarif mehrere zusätzliche Features, z. B. das Hochskalieren dynamischer Partitionen, verlängerte Aufbewahrung und kundenseitig verwaltete Schlüssel. Weitere Informationen finden Sie unter [Event Hubs Premium](event-hubs-premium-overview.md).

Der Event Hubs-Tarif **Dedicated** bietet Bereitstellungen mit einem einzelnen Mandanten für Kunden mit äußerst anspruchsvollen Streaminganforderungen. Dieses Einzelmandantangebot verfügt über ein garantiertes 99,99 %-SLA und ist nur in unserem Dedicated-Tarif verfügbar. Ein Event Hubs-Cluster kann Millionen Ereignisse pro Sekunde mit garantierter Kapazität und Latenz im Sekundenbruchteilbereich erfassen. Namespaces und Event Hubs, die innerhalb des dedizierten Clusters erstellt werden, bieten neben allen Features des Premium-Angebots noch weitere Funktionen. Weitere Informationen finden Sie unter [Event Hubs Dedicated](event-hubs-dedicated-overview.md).

Weitere Informationen finden Sie im [Vergleich der Event Hubs-Tarife](event-hubs-quotas.md).

## <a name="event-hubs-on-azure-stack-hub"></a>Event Hubs in Azure Stack Hub
Mit Event Hubs in Azure Stack Hub können Sie Hybrid Cloud-Szenarien umsetzen. Streaming- und ereignisbasierte Lösungen werden sowohl für lokale als auch für Azure-Cloudverarbeitung unterstützt. Unabhängig davon, ob Ihr Szenario ein hybrides (verbundenes) oder getrenntes Szenario ist, kann Ihre Lösung die Verarbeitung von großvolumigen Ereignissen/Streams unterstützen. Ihr Szenario ist nur an die Größe des Event Hubs-Clusters gebunden, die Sie entsprechend Ihren Anforderungen bereitstellen können. 

Die Event Hubs-Editionen (in Azure Stack Hub und in Azure) bieten ein hohes Maß an Featureparität (Funktionsparität). Diese Parität bedeutet, dass SDKs, Beispiele, PowerShell, CLI und Portale eine ähnliche Funktionalität bieten, mit wenigen Unterschieden. 

Event Hubs in Stack ist während der öffentlichen Vorschauphase kostenlos. Weitere Informationen finden Sie unter [Event Hubs in Azure Stack Hub – Übersicht](/azure-stack/user/event-hubs-overview).

## <a name="key-architecture-components"></a>Wichtige Komponenten der Architektur
Event Hubs enthält die folgenden [Schlüsselkomponenten](event-hubs-features.md):

- **Ereignisersteller:** Jede Entität, die Daten an einen Event Hub sendet. Ereignisherausgeber können Ereignisse über HTTPS, AMQP 1.0 oder Apache Kafka (1.0 und höher) veröffentlichen.
- **Partitionen:** Jeder Consumer liest nur eine bestimmte Teilmenge oder Partition des Nachrichtendatenstroms.
- **Consumergruppen:** Eine Ansicht (Zustand, Position oder Offset) eines gesamten Event Hubs. Mithilfe von Consumergruppen können verarbeitende Anwendungen jeweils über eine separate Ansicht des Ereignisdatenstroms verfügen. Sie können den Datenstrom unabhängig voneinander im eigenen Tempo und mit eigenen Offsets lesen.
- [Durchsatzeinheiten](event-hubs-scalability.md#throughput-units) oder [Verarbeitungseinheiten](event-hubs-scalability.md#processing-units): Vorab erworbene Einheiten von Kapazitäten, die die Durchsatzkapazität von Event Hubs steuern.
- **Ereignisempfänger:** Jede Entität, die Ereignisdaten aus einem Event Hub liest. Alle Event Hubs-Consumer stellen eine Verbindung über die AMQP 1.0-Sitzung her. Der Event Hubs-Dienst stellt Ereignisse über eine Sitzung bereit, sobald diese verfügbar werden. Alle Kafka-Consumer verwenden zur Verbindungsherstellung mindestens das Kafka-Protokoll 1.0.

Die folgende Abbildung zeigt die Datenstromverarbeitungsarchitektur von Event Hubs:

![Event Hubs](./media/event-hubs-about/event_hubs_architecture.svg)


## <a name="next-steps"></a>Nächste Schritte

Lesen Sie zum Einstieg in die Verwendung von Event Hubs die Tutorials zum **Senden und Empfangen von Ereignissen**:

- [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](event-hubs-python-get-started-send.md)
- [JavaScript](event-hubs-node-get-started-send.md)
- [Go](event-hubs-go-get-started-send.md)
- [C (nur senden)](event-hubs-c-getstarted-send.md)
- [Apache Storm (nur empfangen)](event-hubs-storm-getstarted-receive.md)


Weitere Informationen zu Event Hubs finden Sie in den folgenden Artikeln:

- [Event Hubs-Features im Überblick](event-hubs-features.md)
- [Häufig gestellte Fragen zu Event Hubs](event-hubs-faq.yml)
