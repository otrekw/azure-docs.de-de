---
title: Konsistenzebenen in Azure Cosmos DB
description: Azure Cosmos DB bietet fünf Konsistenzebenen, um für vorhersehbare Kompromisse zwischen Konsistenz, Verfügbarkeit und Latenz sorgen zu können.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.openlocfilehash: b5d9df7a0afa9b4270f0eff643e083e5bccfceb8
ms.sourcegitcommit: e6bce4b30486cb19a6b415e8b8442dd688ad4f92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2020
ms.locfileid: "78933679"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Konsistenzebenen in Azure Cosmos DB

Verteilte Datenbanken, die auf Replikation angewiesen sind, um Hochverfügbarkeit, niedrige Latenzzeiten oder beides sicherzustellen, bilden den grundlegenden Kompromiss zwischen Lesekonsistenz und Verfügbarkeit, Latenz sowie Durchsatz. Die meisten kommerziell verfügbaren verteilten Datenbanken verlangen von Entwicklern, dass sie zwischen den beiden extremen Konsistenzmodellen wählen: *starke* Konsistenz und *letztliche* Konsistenz. Die Linearisierbarkeit oder das Modell für starke Konsistenz ist der Goldstandard in Sachen Datenprogrammierbarkeit. Es wird jedoch teuer durch hohe Wartezeit (im stabilen Zustand) bzw. durch geringere Verfügbarkeit (im Fall eines Ausfalls) erkauft. Auf der anderen Seite bietet die letztliche Konsistenz eine höhere Verfügbarkeit und bessere Leistung, erschwert jedoch die Programmierung von Anwendungen erheblich. 

Azure Cosmos DB bietet für die Datenkonsistenz nicht nur diese beiden Extreme, sondern ein ganzes Spektrum von Auswahlmöglichkeiten. Die starke und die letztliche Konsistenz bilden die beiden Enden des Spektrums, aber dazwischen gibt es viele weitere Konsistenzoptionen. Entwickler können mit diesen Optionen eine genaue Auswahl treffen und differenzierte Kompromisse in Bezug auf Hochverfügbarkeit und Leistung eingehen. 

Mit Azure Cosmos DB können Entwickler aus fünf klar definierten Konsistenzmodellen für das Konsistenzspektrum auswählen. Dabei handelt es sich um die folgenden Modelle (von der stärksten bis zur schwächsten Option): *starke Konsistenz*, *begrenzte Veraltung*, *Sitzungskonsistenz*, *Präfixkonsistenz* und *letztliche Konsistenz*. Die Modelle sind klar definiert und intuitiv und können für spezifische reale Szenarien verwendet werden. Jedes Modell ermöglicht [Verfügbarkeits- und Leistungskompromisse](consistency-levels-tradeoffs.md) und wird durch SLAs abgesichert. In der folgenden Abbildung sind die verschiedenen Konsistenzebenen als Spektrum dargestellt.

![Konsistenz als Spektrum](./media/consistency-levels/five-consistency-levels.png)

Die Konsistenzebenen sind regionsunabhängig und für alle Vorgänge garantiert, unabhängig von der Region, in der die Lese- und Schreibvorgänge verarbeitet werden, der Anzahl der mit Ihrem Azure Cosmos-Konto verbundenen Regionen und davon, ob Ihr Konto mit einer oder mehreren Schreibregionen konfiguriert ist.

## <a name="scope-of-the-read-consistency"></a>Geltungsbereich der Lesekonsistenz

Die Lesekonsistenz gilt für einen einzelnen Lesevorgang innerhalb eines Partitionsschlüsselbereichs bzw. einer logischen Partition. Ein Lesevorgang kann durch einen Remoteclient oder eine gespeicherte Prozedur ausgelöst werden.

## <a name="configure-the-default-consistency-level"></a>Konfigurieren der Standardkonsistenzebene

Sie können die Standardkonsistenzebene für Ihr Azure Cosmos-Konto jederzeit konfigurieren. Die für Ihr Konto konfigurierte Standardkonsistenzebene gilt für alle Azure Cosmos-Datenbanken und -Container in diesem Konto. Bei allen in einem Container oder einer Datenbank ausgeführten Lesevorgängen und Abfragen wird standardmäßig die angegebene Konsistenzebene verwendet. Weitere Informationen finden Sie unter [Konfigurieren der Standardkonsistenzebene](how-to-manage-consistency.md#configure-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Garantien in Zusammenhang mit Konsistenzebenen

Die von Azure Cosmos DB bereitgestellten umfassenden SLAs garantieren, dass 100 Prozent aller Leseanforderungen die Konsistenzgarantie für jede von Ihnen ausgewählte Konsistenzebene erfüllen. Eine Leseanforderung erfüllt die Konsistenz-SLA, wenn alle Konsistenzgarantien der Konsistenzebene erfüllt werden. Die genauen Definitionen der fünf Konsistenzebenen in Azure Cosmos DB – unter Verwendung der TLA+-Spezifikationssprache – finden Sie im GitHub-Repository [azure-cosmos-tla](https://github.com/Azure/azure-cosmos-tla).

Im Folgenden wird die Semantik der fünf Konsistenzebenen beschrieben:

- **Starke Konsistenz**: Starke Konsistenz bietet garantierte Linearisierbarkeit. Linearisierbarkeit bedeutet die gleichzeitige Verarbeitung von Anforderungen. Die Lesevorgänge geben garantiert die neueste Version eines Elements zurück, für die ein Commit ausgeführt wurde. Einem Client wird nie ein partieller Schreibvorgang bzw. ein Schreibvorgang, für den kein Commit ausgeführt wurde, angezeigt. Benutzer haben immer die Garantie, dass sie den neuesten Schreibvorgang lesen, für den ein Commit ausgeführt wurde.

  In der folgenden Grafik wird die starke Konsistenz anhand von Noten veranschaulicht. Nachdem die Daten in die Region „USA, Osten“ geschrieben wurden, erhalten Sie beim Lesen der Daten in anderen Regionen den neuesten Wert:

  ![video](media/consistency-levels/strong-consistency.gif)

- **Begrenzte Veraltung (Bounded staleness)** : Die Lesevorgänge berücksichtigen immer die Garantie der Präfixkonsistenz. Lesevorgänge bleiben höchstens um *„K“* Versionen (Updates) eines Elements oder um ein durch *„T“* definiertes Zeitintervall hinter Schreibvorgängen zurück. Wenn Sie also die begrenzte Veraltung auswählen, kann die Veraltung auf zwei Arten konfiguriert werden: 

  * Anhand der Anzahl von Versionen (*K*) des Elements
  * Anhand des Zeitintervalls (*T*) zwischen Lese- und Schreibvorgängen 

  Begrenzte Veraltung bietet eine vollständige globale Reihenfolge außer innerhalb des „Veraltungsfensters“. Die monotonen Lesegarantien bestehen innerhalb einer Region sowohl innerhalb als auch außerhalb des Veraltungszeitfensters. Die starke Konsistenz weist die gleiche Semantik auf wie die begrenzte Veraltung. Das Veraltungszeitfenster ist gleich Null (0). Die begrenzte Veraltung wird auch als „Linearisierbarkeit mit Zeitverzögerung“ bezeichnet. Wenn ein Client Lesevorgänge in einer Region ausführt, die Schreibvorgänge akzeptiert, bietet die begrenzte Veraltung die gleichen Garantien wie die starke Konsistenz.

  Die begrenzte Veraltung wird häufig von global verteilten Anwendungen genutzt, die niedrige Schreibwartezeiten erwarten, aber eine garantierte vollständige globale Reihenfolge benötigen. Begrenzte Veraltung eignet sich hervorragend für Anwendungen mit Gruppenzusammenarbeit und -freigabe, Börsenticker, Veröffentlichen-Abonnieren-Muster/Queueing und Ähnlichem. In der folgenden Grafik wird die begrenzte Veraltung anhand von Noten veranschaulicht. Nachdem die Daten in die Region „USA, Osten“ geschrieben wurden, wird der geschriebene Wert in den Regionen „USA, Westen“ und „Australien, Osten“ basierend auf der konfigurierten maximalen Verzögerung oder den maximalen Vorgängen gelesen:

  ![video](media/consistency-levels/bounded-staleness-consistency.gif)

- **Sitzung (Session)** :  Die Lesevorgänge in einer einzelnen Clientsitzung berücksichtigen immer folgende Garantien: Präfixkonsistenz (sofern es sich um eine einzelne Schreibsitzung handelt), monotone Lesevorgänge, monotone Schreibvorgänge, Lesen der eigenen Schreibvorgänge, Schreibvorgänge folgen Lesevorgängen. Clients außerhalb der Sitzung, die Schreibvorgänge ausführen, erreichen auch irgendwann Konsistenz.

  Die Sitzungskonsistenz ist die gängige Konsistenzebene für Anwendungen in einer einzelnen Region sowie für weltweit verteilte Anwendungen. Sie bietet Schreibwartezeiten, Verfügbarkeit und Lesedurchsatz, die mit der letztlichen Konsistenz vergleichbar sind. Darüber hinaus stellt sie die Konsistenzgarantien für Anwendungen bereit, die für den Betrieb im Benutzerkontext geschrieben wurden. In der folgenden Grafik wird die Sitzungskonsistenz anhand von Noten veranschaulicht. Die Region „USA, Westen“ und die Region „USA, Osten“ verwenden die gleiche Sitzung (Sitzung A) und lesen die Daten somit zur gleichen Zeit. Die Region „Australien, Osten“ verwendet dagegen „Sitzung B“ und empfängt Daten somit später, aber weiterhin in der Reihenfolge der Schreibvorgänge.

  ![video](media/consistency-levels/session-consistency.gif)

- **Präfixkonsistenz**: Die zurückgegebenen Updates enthalten ein bestimmtes Präfix aller Updates ohne Lücken. Die Konsistenzebene „Präfixkonsistenz“ garantiert, dass für Lesevorgänge niemals Schreibvorgänge in falscher Reihenfolge angezeigt werden.

  Wenn Schreibvorgänge in der Reihenfolge `A, B, C` erfolgen, wird einem Client entweder `A`, `A,B` oder `A,B,C`, aber niemals eine falsche Reihenfolge wie `A,C` oder `B,A,C` angezeigt. Die Präfixkonsistenz bietet Schreibwartezeiten, Verfügbarkeit und Lesedurchsatz, die mit der letztlichen Konsistenz vergleichbar sind. Darüber hinaus stellt sie Reihenfolgengarantien für Szenarien bereit, in denen die Reihenfolge relevant ist. In der folgenden Grafik wird die Präfixkonsistenz anhand von Noten veranschaulicht. In allen Regionen werden bei Lesevorgängen niemals Schreibvorgänge in der falschen Reihenfolge angezeigt:

  ![video](media/consistency-levels/consistent-prefix.gif)

- **Letztlich (Eventual)** : Es gibt keine Reihenfolgengarantie für Lesevorgänge. Wenn keine weiteren Schreibvorgänge vorhanden sind, konvergieren die Replikate schließlich.  
Die letztliche Konsistenz stellt die schwächste Form der Konsistenz dar, da ein Client unter Umständen Werte liest, die älter als die zuvor gelesenen Werte sind. Letztliche Konsistenz ist ideal, wenn die Anwendung keinerlei Reihenfolgengarantien erfordert. Ein Beispiel wäre etwa das Zählen von Retweets, „Gefällt mir“-Markierungen oder Kommentaren ohne Thread. In der folgenden Grafik wird die letztliche Konsistenz anhand von Noten veranschaulicht:

  ![video](media/consistency-levels/eventual-consistency.gif)

## <a name="additional-reading"></a>Zusätzliche Lektüre

In folgenden Ressourcen erfahren Sie mehr über Konsistenzkonzepte:

- [High-level TLA+ specifications for the five consistency levels offered by Azure Cosmos DB](https://github.com/Azure/azure-cosmos-tla) (Allgemeine TLA+-Spezifikationen für die fünf von Azure Cosmos DB angebotenen Konsistenzebenen)
- [Erläuterung der Konsistenz replizierter Daten anhand von Baseball (Video) von Doug Terry](https://www.youtube.com/watch?v=gluIh8zd26I)
- [Erläuterung der Konsistenz replizierter Daten anhand von Baseball (Whitepaper) von Doug Terry](https://www.microsoft.com/en-us/research/publication/replicated-data-consistency-explained-through-baseball/?from=http%3A%2F%2Fresearch.microsoft.com%2Fpubs%2F157411%2Fconsistencyandbaseballreport.pdf)
- [Sitzungsgarantien für schwach konsistente replizierte Daten](https://dl.acm.org/citation.cfm?id=383631)
- [Konsistenzkompromisse im Design moderner verteilter Datenbanksysteme: CAP ist nur ein Teil der Wahrheit](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Probabilistic Bounded Staleness (PBS) for Practical Partial Quorums](https://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
- [Eventually Consistent – Revisited](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html) (Letztliche Konsistenz – Neuauflage)

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um mehr über die Konsistenzebenen in Azure Cosmos DB zu erfahren:

* [Auswählen der richtigen Konsistenzebene für Ihre Anwendung](consistency-levels-choosing.md)
* [Konsistenzebenen und Azure Cosmos DB-APIs](consistency-levels-across-apis.md)
* [Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen](consistency-levels-tradeoffs.md)
* [Konfigurieren der Standardkonsistenzebene](how-to-manage-consistency.md#configure-the-default-consistency-level)
* [Außerkraftsetzen der Standardkonsistenzebene](how-to-manage-consistency.md#override-the-default-consistency-level)

