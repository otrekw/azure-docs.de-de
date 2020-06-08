---
title: Änderungsfeedprozessor in Azure Cosmos DB
description: Erfahren Sie, wie Sie mithilfe des Änderungsfeedprozessors von Azure Cosmos DB den Änderungsfeed lesen. Außerdem finden Sie hier Informationen zu den Komponenten des Änderungsfeedprozessors.
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/13/2020
ms.reviewer: sngun
ms.openlocfilehash: 584fc48aad6a64f8df54088e6dbfd990e8e112e8
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83655304"
---
# <a name="change-feed-processor-in-azure-cosmos-db"></a>Änderungsfeedprozessor in Azure Cosmos DB

Der Änderungsfeedprozessor ist im [Azure Cosmos DB SDK V3](https://github.com/Azure/azure-cosmos-dotnet-v3) enthalten. Er vereinfacht das Lesen des Änderungsfeeds sowie die effektive Verteilung der Ereignisverarbeitung auf mehrere Consumer.

Der Hauptvorteil des Änderungsfeedprozessors ist sein fehlertolerantes Verhalten, das eine At-Least-Once-Übermittlung aller Ereignisse im Änderungsfeed gewährleistet.

## <a name="components-of-the-change-feed-processor"></a>Komponenten des Änderungsfeedprozessors

Die Implementierung des Änderungsfeedprozessors umfasst vier Hauptkomponenten:

1. **Überwachter Container:** Der überwachte Container enthält die Daten, aus denen der Änderungsfeed generiert wird. Alle Einfügungen und Aktualisierungen für den überwachten Container werden im Änderungsfeed des Containers berücksichtigt.

1. **Leasecontainer:** Der Leasecontainer fungiert als Zustandsspeicher und koordiniert die Verarbeitung des Änderungsfeeds über mehrere Worker hinweg. Er kann im gleichen Konto wie der überwachte Container oder in einem separaten Konto gespeichert werden.

1. **Host:** Ein Host ist eine Anwendungsinstanz, die den Änderungsfeedprozessor verwendet, um auf Änderungen zu lauschen. Mehrere Instanzen mit der gleichen Leasekonfiguration können parallel ausgeführt werden, die einzelnen Instanzen müssen allerdings jeweils einen anderen **Instanznamen** besitzen.

1. **Delegat:** Der Delegat ist der Code, der definiert, wie Sie (der Entwickler) mit dem jeweiligen Batch von Änderungen verfahren möchten, der vom Änderungsfeedprozessor gelesen wurde. 

Um besser zu verstehen, wie diese vier Elemente von Change Feed Processor zusammenarbeiten, sehen wir uns ein Beispiel in der folgenden Abbildung an. Der überwachte Container speichert Dokumente und verwendet „City“ als Partitionsschlüssel. Wie Sie sehen, sind die Partitionsschlüsselwerte auf Bereiche verteilt, die Elemente enthalten. Es gibt zwei Hostinstanzen, und der Änderungsfeedprozessor weist jeder Instanz unterschiedliche Bereiche von Partitionsschlüsselwerten zu, um die Computeverteilung zu maximieren. Die einzelnen Bereiche werden parallel gelesen, und der Fortschritt wird getrennt von anderen Bereichen im Leasecontainer verwaltet.

![Beispiel für den Änderungsfeedprozessor](./media/change-feed-processor/changefeedprocessor.png)

## <a name="implementing-the-change-feed-processor"></a>Implementieren des Änderungsfeedprozessors

Der Einstiegspunkt ist immer der überwachte Container. Von einer `Container`-Instanz aus wird `GetChangeFeedProcessorBuilder` aufgerufen:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-change-feed-processor/src/Program.cs?name=DefineProcessor)]

Der erste Parameter ist hierbei ein eindeutiger Name, der das Ziel dieses Prozessors beschreibt. Der zweite Name ist die Delegatimplementierung zur Behandlung von Änderungen. 

Ein Beispiel für einen Delegaten wäre etwa:


[!code-csharp[Main](~/samples-cosmosdb-dotnet-change-feed-processor/src/Program.cs?name=Delegate)]

Abschließend wird mit `WithInstanceName` ein Name für diese Prozessorinstanz und mit `WithLeaseContainer` der Container für die Verwaltung des Leasezustands definiert.

Durch Aufrufen von `Build` wird die Prozessorinstanz erstellt, die dann durch Aufrufen von `StartAsync` gestartet werden kann.

## <a name="processing-life-cycle"></a>Verarbeitungslebenszyklus

Der normale Lebenszyklus einer Hostinstanz sieht wie folgt aus:

1. Der Änderungsfeed wird gelesen.
1. Sollten keine Änderungen vorhanden sein, wird nach einer vordefinierten Pause (die im Generator mithilfe von `WithPollInterval` angepasst werden kann) erneut Schritt 1 ausgeführt.
1. Sind Änderungen vorhanden, werden sie an den **Delegaten** gesendet.
1. Nach **erfolgreicher** Verarbeitung der Änderungen durch den Delegaten wird der Leasespeicher mit dem neuesten verarbeiteten Zeitpunkt aktualisiert, und es wird wieder Schritt 1 ausgeführt.

## <a name="error-handling"></a>Fehlerbehandlung

Der Änderungsfeedprozessor bietet Resilienz bei Benutzercodefehlern. Das bedeutet: Wenn Ihre Delegatimplementierung eine nicht behandelte Ausnahme (Schritt 4) aufweist, wird der Thread, der diesen speziellen Batch von Änderungen verarbeitet, beendet, und ein neuer Thread wird erstellt. Der neue Thread prüft den letzten Zeitpunkt, der im Leasespeicher für diesen Bereich von Partitionsschlüsselwerten vorliegt, und setzt den Vorgang an dieser Stelle fort, sodass letztendlich der gleiche Batch von Änderungen an den Delegaten gesendet wird. Dieses Verhalten wird so lange fortgesetzt, bis der Delegat die Änderungen ordnungsgemäß verarbeitet. Dies ist auch der Grund dafür, dass der Änderungsfeedprozessor über eine At-Least-Once-Garantie verfügt, denn sollte der Delegatcode eine Ausnahme auslösen, wird der entsprechende Batch wiederholt.

Um zu verhindern, dass der Änderungsfeedprozessor fortlaufend denselben Batch von Änderungen wiederholt, sollten Sie im Delegatcode Logik hinzufügen, um bei einer Ausnahme Dokumente in eine Warteschlange für unzustellbare Nachrichten zu schreiben. Dieser Entwurf stellt sicher, dass Sie nicht verarbeitete Änderungen nachverfolgen und gleichzeitig zukünftige Änderungen verarbeiten können. Die Warteschlange für unzustellbare Nachrichten kann einfach ein anderer Cosmos-Container sein. Der genaue Datenspeicher spielt keine Rolle, da in ihm lediglich die nicht verarbeiteten Änderungen gespeichert werden sollen.

Darüber hinaus können Sie den Fortschritt Ihrer Änderungsfeedprozessor-Instanzen beim Lesen des Änderungsfeeds mithilfe des [Änderungsfeed-Estimators](how-to-use-change-feed-estimator.md) überwachen. Zusätzlich zur Überwachung, ob der Änderungsfeedprozessor immer denselben Batch von Änderungen wiederholt, können Sie auch ermitteln, ob der Änderungsfeedprozessor durch die Verfügbarkeit von Ressourcen wie CPU, Arbeitsspeicher und Netzwerkbandbreite Verzögerungen unterliegt.

## <a name="deployment-unit"></a>Bereitstellungseinheit

Eine Bereitstellungseinheit für den Änderungsfeedprozessor besteht aus einer oder mehreren Instanzen mit dem gleichen `processorName`-Wert und der gleichen Konfiguration für Leasecontainer. Sie können mehrere Bereitstellungseinheiten besitzen, deren Geschäftsabläufe für Änderungen sich unterschieden und die jeweils aus einer oder mehreren Instanzen bestehen. 

Beispielsweise könnte eine Ihrer Bereitstellungseinheiten immer dann eine externe API auslösen, wenn eine Änderung am Container erfolgt. Eine andere Bereitstellungseinheit könnte immer dann Daten in Echtzeit verschieben, wenn eine Änderung erfolgt. Wenn in Ihrem überwachten Container eine Änderung auftritt, werden alle Bereitstellungseinheiten benachrichtigt.

## <a name="dynamic-scaling"></a>Dynamische Skalierung

Wie bereits erwähnt, kann eine Bereitstellungseinheit aus einer oder mehreren Instanzen bestehen. Sie müssen nur die folgenden wichtigen Voraussetzungen erfüllen, um die Computeverteilung innerhalb der Bereitstellungseinheit zu nutzen:

1. Alle Instanzen müssen über die gleiche Leasecontainerkonfiguration verfügen.
1. Alle Instanzen müssen den gleichen `processorName`-Wert besitzen.
1. Die einzelnen Instanzen müssen jeweils einen anderen Instanznamen (`WithInstanceName`) besitzen.

Sind diese drei Bedingungen erfüllt, verteilt der Änderungsfeedprozessor unter Verwendung eines Algorithmus für eine gleichmäßige Verteilung alle Leases im Leasecontainer auf alle aktiven Instanzen dieser Bereitstellungseinheit und parallelisiert das Computing. Da sich eine Lease immer nur im Besitz einer einzelnen Instanz befinden kann, ist die maximale Anzahl von Instanzen gleich der Leaseanzahl.

Die Anzahl der Instanzen kann sich erhöhen und verringern. Der Änderungsfeedprozessor passt die Last durch entsprechende Verteilung dynamisch an.

Außerdem lässt sich der Änderungsfeedprozessor dynamisch an Containerskalierungen aufgrund von Durchsatz- oder Speichererhöhungen anpassen. Bei zunehmender Größe des Containers verarbeitet der Änderungsfeedprozessor diese Szenarien transparent, indem die Leases dynamisch erhöht und die neuen Leases auf vorhandene Instanzen verteilt werden.

## <a name="change-feed-and-provisioned-throughput"></a>Änderungsfeed und bereitgestellter Durchsatz

Ihnen werden die genutzten Anforderungseinheiten (Request Units, RUs) in Rechnung gestellt, da bei der Datenverschiebung in und aus Cosmos-Containern immer RUs verbraucht werden. Ihnen werden die vom Leasecontainer genutzten Anforderungseinheiten in Rechnung gestellt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)
* [Verwendungsbeispiele auf GitHub](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [Zusätzliche Beispiele auf GitHub](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln erfahren Sie mehr über den Änderungsfeedprozessor:

* [Übersicht über Änderungsfeeds](change-feed.md)
* [Pullmodell für den Änderungsfeed](change-feed-pull-model.md)
* [Migrieren von der Änderungsfeed-Verarbeitungsbibliothek](how-to-migrate-from-change-feed-library.md)
* [Use the change feed estimator](how-to-use-change-feed-estimator.md) (Verwenden des Änderungsfeed-Estimators)
* [Konfigurieren der Startzeit des Änderungsfeedprozessors](how-to-configure-change-feed-start-time.md)
