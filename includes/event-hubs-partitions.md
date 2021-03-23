---
title: include file
description: include file
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 03/15/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 2cb203a00bb00767126f95e1fdc2f5aff8990f01
ms.sourcegitcommit: 27cd3e515fee7821807c03e64ce8ac2dd2dd82d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103601275"
---
In Event Hubs werden Sequenzen von an einen Event Hub gesendeten Ereignissen in einer oder mehreren Partitionen organisiert. Neu eingehende Ereignisse werden am Ende dieser Sequenz hinzugefügt. 

![Event Hubs](./media/event-hubs-partitions/multiple-partitions.png)

Eine Partition kann als „Commitprotokoll“ betrachtet werden. Partitionen enthalten Ereignisdaten mit dem Text des Ereignisses und einer benutzerdefinierten Eigenschaftensammlung mit einer Beschreibung des Ereignisses. Darüber hinaus enthalten sie Metadaten, z. B. den Offset in der Partition, die Nummer in der Datenstromsequenz und den dienstseitigen Zeitstempel des Akzeptierungsvorgangs.

![Diagramm: Sequenz der Ereignisse (von alt bis neu)](./media/event-hubs-partitions/partition.png)

### <a name="advantages-of-using-partitions"></a>Vorteile der Verwendung von Partitionen
Event Hubs wurde zur Unterstützung der Verarbeitung von großen Ereignismengen konzipiert, und die Partitionierung ist hierbei aus zwei Gründen hilfreich:

- Bei Event Hubs handelt es sich zwar um einen PaaS-Dienst, aber es gibt auch eine physische Komponente. Zum Führen eines Protokolls, in dem die Reihenfolge der Ereignisse festgehalten wird, müssen diese Ereignisse zusammen im zugrunde liegenden Speicher und in den zugehörigen Replikaten gespeichert werden. Dies führt dazu, dass für ein Protokoll dieser Art ein Grenzwert für den Durchsatz gilt. Die Partitionierung ermöglicht es, dass mehrere parallele Protokolle für denselben Event Hub verwendet werden. Auf diese Weise wird die verfügbare Rohkapazität für den E/A-Durchsatz multipliziert.
- Ihre eigenen Anwendungen müssen die Verarbeitung der Ereignismenge, die an einen Event Hub gesendet wird, bewältigen können. Dies kann ziemlich komplex sein und erfordert eine hohe Kapazität für die Parallelverarbeitung mit Skalierungsoptionen. Die Kapazität eines einzelnen Prozesses zum Verarbeiten von Ereignissen ist begrenzt, sodass Sie mehrere Prozesse benötigen. Partitionen werden von Ihrer Lösung verwendet, um diese Prozesse mit Daten zu versorgen, während gleichzeitig sichergestellt wird, dass jedes Ereignis über einen eindeutigen Verarbeitungsbesitzer verfügt. 

### <a name="number-of-partitions"></a>Anzahl von Partitionen
Die Anzahl von Partitionen wird bei der Erstellung angegeben und muss für Event Hubs Standard zwischen 1 und 32 liegen. Die Partitionsanzahl kann für Event Hubs Dedicated bei bis zu 2.000 Partitionen pro Kapazitätseinheit liegen. 

Wir empfehlen Ihnen, mindestens so viele Partitionen auszuwählen, wie Sie voraussichtlich in dauerhaften [Durchsatzeinheiten](../articles/event-hubs/event-hubs-faq.md#what-are-event-hubs-throughput-units) während der Spitzenlast Ihrer Anwendung für den entsprechenden Event Hub benötigen. Sie sollten für die Berechnung davon ausgehen, dass eine Partition über eine Durchsatzkapazität von 1 Durchsatzeinheit verfügt (1 MB eingehend, 2 MB ausgehend). Sie können die Durchsatzeinheiten in Ihrem Namespace oder die Kapazitätseinheiten Ihres Clusters unabhängig von der Partitionsanzahl skalieren. Für einen Event Hub mit 32 Partitionen oder einen Event Hub mit nur einer Partition fallen die gleichen Kosten an, wenn der Namespace auf eine Kapazität von einer Durchsatzeinheit festgelegt ist. 

Die Partitionsanzahl für einen Event Hub in einem [dedizierten Event Hubs-Cluster](../articles/event-hubs/event-hubs-dedicated-overview.md) kann nach seiner Erstellung [erhöht](../articles/event-hubs/dynamically-add-partitions.md) werden, aber dann ändert sich die Verteilung von Datenströmen auf die Partitionen, da sich die Zuordnung von Partitionsschlüsseln zu den Partitionen ändert. Daher sollten Sie Änderungen dieser Art unbedingt vermeiden, falls die relative Reihenfolge der Ereignisse in Ihrer Anwendung wichtig ist.

Es ist verlockend, die Anzahl von Partitionen auf den zulässigen Höchstwert festzulegen. Hierbei sollten Sie aber stets beachten, dass Ihre Ereignisdatenströme so strukturiert sein müssen, dass Sie wirklich mehrere Partitionen nutzen können. Falls bei Ihnen die Reihenfolge für alle Ereignisse oder auch nur eine Handvoll untergeordneter Datenströme unbedingt beibehalten werden muss, kommen Sie ggf. nicht in den Genuss der Vorteile, die sich aus der Verwendung von mehreren Partitionen ergeben. Wenn mehrere Partitionen genutzt werden, erhöht sich zudem die Komplexität der Verarbeitungsseite. 


### <a name="mapping-of-events-to-partitions"></a>Zuordnen der Ereignisse zu Partitionen
Sie können einen Partitionsschlüssel zum Zuordnen der in spezifischen Partitionen eingehenden Daten für die Datenorganisation verwenden. Der Partitionsschlüssel ist ein vom Absender bereitgestellter Wert, der an einen Event Hub übergeben wird. Er wird über eine statische Hashfunktion verarbeitet, die die Partitionszuweisung erstellt. Wenn Sie beim Veröffentlichen eines Ereignisses keinen Partitionsschlüssel angeben, wird eine Roundrobinzuordnung verwendet.

Dem Ereignisherausgeber ist nur der Partitionsschlüssel bekannt, nicht die Partition, auf der die Ereignisse veröffentlicht werden. Dieses Entkoppeln von Schlüssel und Partition entbindet den Absender davon, zu viel über die Downstreamverarbeitung wissen zu müssen. Eine gerätebezogene oder für einen Benutzer eindeutige Identität stellt einen guten Partitionsschlüssel dar, es können aber auch andere Attribute wie z. B. Geografie zum Gruppieren von verwandten Ereignissen in einer einzelnen Partition verwendet werden.

Das Angeben eines Partitionsschlüssels ermöglicht es, verwandte Ereignisse zusammen auf derselben Partition in der exakten Sendereihenfolge aufzubewahren. Der Partitionsschlüssel ist eine Zeichenfolge, die von Ihrem Anwendungskontext abgeleitet wird, und dient zum Identifizieren der Beziehung zwischen den Ereignissen. Eine von einem Partitionsschlüssel identifizierte Ereignissequenz ist ein *Datenstrom*. Eine Partition ist ein Multiplex-Protokollspeicher für viele Datenströme dieser Art. 

> [!NOTE]
> Sie können Ereignisse zwar direkt an Partitionen senden, dies wird jedoch insbesondere dann nicht empfohlen, wenn Sie Wert auf Hochverfügbarkeit legen. Dadurch wird die Verfügbarkeit eines Event Hub auf Partitionsebene herabgestuft. Weitere Informationen finden Sie unter [Verfügbarkeit und Konsistenz](../articles/event-hubs/event-hubs-availability-and-consistency.md).

