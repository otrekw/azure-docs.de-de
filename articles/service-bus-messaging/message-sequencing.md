---
title: Nachrichtensequenzierung und -zeitstempel mit Azure Service Bus | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie die Sequenz und Reihenfolge (mit Zeitstempeln) von Azure Service Bus-Nachrichten beibehalten.
ms.topic: article
ms.date: 04/14/2021
ms.openlocfilehash: 3d5300568232afae1238445113d60eda8cdb2f1b
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107497096"
---
# <a name="message-sequencing-and-timestamps"></a>Nachrichtensequenzierung und -zeitstempel

Die Sequenzier- und Zeitstempelfunktionen sind in allen Service Bus-Entitäten und -Oberflächen über die Eigenschaften `SequenceNumber` und `EnqueuedTimeUtc` von empfangenen oder durchsuchten Nachrichten immer aktiviert.

In Fällen, in denen die absolute Reihenfolge der Nachrichten von Bedeutung ist und/oder ein Kunde einen vertrauenswürdigen eindeutigen Bezeichner für Nachrichten benötigt, versieht der Broker Nachrichten mit einem Stempel mit einer lückenlosen, aufsteigenden Sequenznummer relativ zur Warteschlange oder zum Thema. Bei partitionierten Entitäten wird die Sequenznummer relativ zur Partition ausgegeben.

Der Wert **SequenceNumber** ist eine eindeutige ganze 64-Bit-Zahl, die einer Nachricht zugeordnet wird, sobald sie vom Broker akzeptiert und gespeichert wird, und fungiert als ihr interner Bezeichner. Bei partitionierte Entitäten stellen die obersten 16 Bits den Partitionsbezeichner dar. Für Sequenznummern wird ein Rollover auf Null ausgeführt, wenn der 48-/64-Bit-Bereich erschöpft ist.

Die Sequenznummer kann als eindeutiger Bezeichner vertrauenswürdig sein, da sie nicht von Clients, sondern von einer zentralen und einer neutralen Autorität vergeben wird. Sie stellt auch die wahre Ankunftsreihenfolge dar und ist als Sortierkriterium genauer als ein Zeitstempel, da Zeitstempel bei extremen Nachrichtenraten möglicherweise keine ausreichend hohe Auflösung aufweisen und in Situationen, in denen das Brokereigentum zwischen Knoten übergeht, einer (allerdings minimalen) Uhrabweichung unterliegen kann.

Die absolute Ankunftsreihenfolge ist beispielsweise in Geschäftsszenarien von Bedeutung, in denen eine begrenzte Anzahl von angebotenen Waren auf Grundlage des First-Come-First-Serve-Prinzips verarbeitet wird, solange der Vorrat reicht (z.B. beim Konzertkartenverkauf).

Die Zeitstempelfunktion dient als neutrale und vertrauenswürdige Instanz, die den UTC-Zeitpunkt eines Nachrichteneingangs über die Eigenschaft **EnqueuedTimeUtc** präzise erfasst. Der Wert ist für terminabhängige Geschäftsszenarien nützlich, z.B. bei der Frage, ob ein Arbeitselement vor Mitternacht an einem bestimmten Datum übermittelt wurde, die Verarbeitung jedoch weit hinter dem Warteschlangenbacklog zurückliegt.

## <a name="scheduled-messages"></a>Geplante Nachrichten

Sie können Nachrichten zur verzögerten Verarbeitung an eine Warteschlange oder ein Thema übergeben, um z.B. einen Auftrag zu planen, der an einem bestimmten Zeitpunkt für die Verarbeitung von einem System zur Verfügung gestellt werden soll. Mit dieser Funktion wird ein zuverlässiger, verteilter zeitbasierter Scheduler bereitgestellt.

Geplante Nachrichten materialisieren in der Warteschlange erst nach dem definierten Zeitpunkt der Einreihung in die Warteschlange. Davor können geplante Nachrichten abgebrochen werden. Durch einen Abbruch wird die Nachricht gelöscht.

Sie können Nachrichten mit einem unserer Clients auf zwei Arten planen:
- Verwenden Sie die reguläre Sende-API, aber legen Sie die `ScheduledEnqueueTimeUtc`-Eigenschaft für die Nachricht vor dem Senden fest.
- Verwenden Sie die API zum Planen von Nachrichten, und übergeben Sie sowohl die normale Nachricht als auch die geplante Zeit. Letztere gibt die **SequenceNumber** der geplanten Nachricht zurück, mit der Sie die geplante Nachricht bei Bedarf später löschen können. 

Geplante Nachrichten und deren Sequenznummern können auch durch das [Durchsuchen von Nachrichten](message-browsing.md) ermittelt werden.

Die Eigenschaft **SequenceNumber** für eine geplante Nachricht ist nur gültig, wenn sich die Nachricht in diesem Zustand befindet. Wenn die Nachricht in den aktiven Zustand übergeht, wird sie an die Warteschlange angefügt, als ob sie zum aktuellen Zeitpunkt in die Warteschlange gereiht worden wäre, was auch die Zuweisung einer neuen **SequenceNumber**-Eigenschaft einschließt.

Da das Feature auf einzelne Nachrichten verankert ist und Nachrichten nur einmal abgefragt werden können, unterstützt Service Bus keine wiederkehrenden Zeitpläne für Nachrichten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Service Bus-Messaging finden Sie in folgenden Themen:

* [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Verwenden von Service Bus-Themen und -Abonnements](service-bus-dotnet-how-to-use-topics-subscriptions.md)
