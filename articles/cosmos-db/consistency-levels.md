---
title: Konsistenzebenen in Azure Cosmos DB
description: Azure Cosmos DB bietet fünf Konsistenzebenen, um für vorhersehbare Kompromisse zwischen Konsistenz, Verfügbarkeit und Latenz sorgen zu können.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 5ba3fc70a2ccfbe342e222dbb475658629ec60a4
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85851701"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Konsistenzebenen in Azure Cosmos DB

Verteilte Datenbanken, die auf Replikation angewiesen sind, um Hochverfügbarkeit, niedrige Latenzzeiten oder beides sicherzustellen, bilden den grundlegenden Kompromiss zwischen Lesekonsistenz und Verfügbarkeit, Latenz sowie Durchsatz. Die meisten kommerziell verfügbaren verteilten Datenbanken verlangen von Entwicklern, dass sie zwischen den beiden extremen Konsistenzmodellen wählen: *starke* Konsistenz und *letztliche* Konsistenz. Die Linearisierbarkeit oder das Modell für starke Konsistenz ist der Goldstandard bei der Datenprogrammierbarkeit. Es wird jedoch teuer durch hohe Schreiblatenz (im stabilen Zustand) bzw. durch geringere Verfügbarkeit (im Fall eines Ausfalls) erkauft. Auf der anderen Seite bietet die letztliche Konsistenz eine höhere Verfügbarkeit und bessere Leistung, erschwert jedoch die Programmierung von Anwendungen erheblich.

Azure Cosmos DB bietet für die Datenkonsistenz nicht nur diese beiden Extreme, sondern ein ganzes Spektrum von Auswahlmöglichkeiten. Entwickler können mit diesen Optionen eine genaue Auswahl treffen und differenzierte Kompromisse in Bezug auf Hochverfügbarkeit und Leistung eingehen.

Mit Azure Cosmos DB können Entwickler aus fünf klar definierten Konsistenzebenen für das Konsistenzspektrum auswählen. Zu den Konsistenzebenen gehören *Sicher*, *Begrenzte Veraltung*, *Sitzung*, *Präfixkonsistenz* und *Letztlich*. Die Ebenen sind klar definiert und intuitiv und können für spezifische reale Szenarien verwendet werden. Jede Ebene bietet [Verfügbarkeits- und Leistungskompromisse](consistency-levels-tradeoffs.md) und wird durch SLAs abgesichert. In der folgenden Abbildung sind die verschiedenen Konsistenzebenen als Spektrum dargestellt.

:::image type="content" source="./media/consistency-levels/five-consistency-levels.png" alt-text="Konsistenz als Spektrum" border="false" :::

Die Konsistenzebenen sind regionsunabhängig und für alle Vorgänge garantiert, unabhängig von der Region, in der die Lese- und Schreibvorgänge verarbeitet werden, der Anzahl der mit Ihrem Azure Cosmos-Konto verbundenen Regionen und davon, ob Ihr Konto mit einer oder mehreren Schreibregionen konfiguriert ist.

## <a name="scope-of-the-read-consistency"></a>Geltungsbereich der Lesekonsistenz

Die Lesekonsistenz gilt für einen einzelnen Lesevorgang innerhalb einer logischen Partition. Ein Lesevorgang kann durch einen Remoteclient oder eine gespeicherte Prozedur ausgelöst werden.

## <a name="configure-the-default-consistency-level"></a>Konfigurieren der Standardkonsistenzebene

Sie können die Standardkonsistenzebene für Ihr Azure Cosmos-Konto jederzeit konfigurieren. Die für Ihr Konto konfigurierte Standardkonsistenzebene gilt für alle Azure Cosmos-Datenbanken und -Container in diesem Konto. Bei allen in einem Container oder einer Datenbank ausgeführten Lesevorgängen und Abfragen wird standardmäßig die angegebene Konsistenzebene verwendet. Weitere Informationen finden Sie unter [Konfigurieren der Standardkonsistenzebene](how-to-manage-consistency.md#configure-the-default-consistency-level). Sie können auch die Standardkonsistenzebene für eine bestimmte Anforderung außer Kraft setzen. Weitere Informationen hierzu finden Sie im Artikel [Außerkraftsetzen der Standardkonsistenzebene](how-to-manage-consistency.md?#override-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Garantien in Zusammenhang mit Konsistenzebenen

Die von Azure Cosmos DB bereitgestellten umfassenden SLAs garantieren, dass 100 Prozent aller Leseanforderungen die Konsistenzgarantie für jede von Ihnen ausgewählte Konsistenzebene erfüllen. Eine Leseanforderung erfüllt die Konsistenz-SLA, wenn alle Konsistenzgarantien der Konsistenzebene erfüllt werden. Die genauen Definitionen der fünf Konsistenzebenen in Azure Cosmos DB – unter Verwendung der TLA+-Spezifikationssprache – finden Sie im GitHub-Repository [azure-cosmos-tla](https://github.com/Azure/azure-cosmos-tla).

Im Folgenden wird die Semantik der fünf Konsistenzebenen beschrieben:

- **Starke Konsistenz**: Starke Konsistenz bietet garantierte Linearisierbarkeit. Linearisierbarkeit bedeutet die gleichzeitige Verarbeitung von Anforderungen. Die Lesevorgänge geben garantiert die neueste Version eines Elements zurück, für die ein Commit ausgeführt wurde. Einem Client wird nie ein partieller Schreibvorgang bzw. ein Schreibvorgang, für den kein Commit ausgeführt wurde, angezeigt. Benutzer haben immer die Garantie, dass sie den neuesten Schreibvorgang lesen, für den ein Commit ausgeführt wurde.

  In der folgenden Grafik wird die starke Konsistenz anhand von Noten veranschaulicht. Nachdem die Daten in die Region „USA, Westen 2“ geschrieben wurden, erhalten Sie beim Lesen der Daten aus anderen Regionen den neuesten Wert:

  :::image type="content" source="media/consistency-levels/strong-consistency.gif" alt-text="Video":::

- **Begrenzte Veraltung (Bounded staleness)** : Die Lesevorgänge berücksichtigen immer die Garantie der Präfixkonsistenz. Lesevorgänge bleiben höchstens um *„K“* Versionen (Updates) eines Elements oder um ein durch *„T“* definiertes Zeitintervall hinter Schreibvorgängen zurück, je nachdem, was zuerst erreicht wird. Wenn Sie also die begrenzte Veraltung auswählen, kann die Veraltung auf zwei Arten konfiguriert werden:

- Anhand der Anzahl von Versionen (*K*) des Elements
- Anhand des Zeitintervalls (*T*) zwischen Lese- und Schreibvorgängen

Begrenzte Veraltung bietet eine vollständige globale Reihenfolge außerhalb des „Veraltungsfensters“. Wenn ein Client Lesevorgänge in einer Region ausführt, die Schreibvorgänge akzeptiert, bietet die begrenzte Veraltung die gleichen Garantien wie die starke Konsistenz.

Im Veraltungsfenster bietet die begrenzte Veraltung die folgenden Konsistenzgarantien:

- Konsistenz für Clients in derselben Region für ein Einzelmasterkonto = Sicher
- Konsistenz für Clients in unterschiedlichen Regionen für ein Einzelmasterkonto = Präfixkonsistenz
- Konsistenz für Clients, die in einer einzelnen Region für ein Multimasterkonto schreiben = Präfixkonsistenz
- Konsistenz für Clients, die in verschiedene Regionen für ein Multimasterkonto schreiben = Letztlich

  Die begrenzte Veraltung wird häufig von global verteilten Anwendungen genutzt, die niedrige Schreibwartezeiten erwarten, aber eine garantierte vollständige globale Reihenfolge benötigen. Begrenzte Veraltung eignet sich hervorragend für Anwendungen mit Gruppenzusammenarbeit und -freigabe, Börsenticker, Veröffentlichen-Abonnieren-Muster/Queueing und Ähnlichem. In der folgenden Grafik wird die begrenzte Veraltung anhand von Noten veranschaulicht. Nachdem die Daten in die Region „USA, Westen 2“ geschrieben wurden, wird der geschriebene Wert in den Regionen „USA, Osten 2“ und „Australien, Osten“ basierend auf der konfigurierten maximalen Verzögerung oder den maximalen Vorgängen gelesen:

  :::image type="content" source="media/consistency-levels/bounded-staleness-consistency.gif" alt-text="Video":::

- **Sitzung (Session)** :  Innerhalb einer einzelnen Clientsitzung berücksichtigen Lesevorgänge immer die folgenden Garantien: Präfixkonsistenz, monotone Lesevorgänge, monotone Schreibvorgänge, Lesen der eigenen Schreibvorgänge, Schreibvorgänge folgen Lesevorgängen. Dabei wird davon ausgegangen, dass eine einzelne Schreibsitzung oder das Sitzungstoken für mehrere Writer gemeinsam genutzt wird.

Für Clients außerhalb der Sitzung, die Schreibvorgänge ausführen, gelten folgende Garantien:

- Konsistenz für Clients in derselben Region für ein Einzelmasterkonto = Präfixkonsistenz
- Konsistenz für Clients in unterschiedlichen Regionen für ein Einzelmasterkonto = Präfixkonsistenz
- Konsistenz für Clients, die in einer einzelnen Region für ein Multimasterkonto schreiben = Präfixkonsistenz
- Konsistenz für Clients, die in mehrere Regionen für ein Multimasterkonto schreiben = Letztlich

  Die Sitzungskonsistenz ist die gängigste Konsistenzebene für Anwendungen in einer einzelnen Region sowie für weltweit verteilte Anwendungen. Sie bietet Schreibwartezeiten, Verfügbarkeit und Lesedurchsatz, die mit der letztlichen Konsistenz vergleichbar sind. Darüber hinaus stellt sie die Konsistenzgarantien für Anwendungen bereit, die für den Betrieb im Benutzerkontext geschrieben wurden. In der folgenden Grafik wird die Sitzungskonsistenz anhand von Noten veranschaulicht. Für „USA, Westen 2“ (Schreiben) und „USA, Westen 2“ (Lesen) wird dieselbe Sitzung (Sitzung A) verwendet, sodass beide dieselben Daten zur gleichen Zeit lesen. Die Region „Australien, Osten“ dagegen verwendet „Sitzung B“ und empfängt Daten somit später, aber weiterhin in der Reihenfolge der Schreibvorgänge.

  :::image type="content" source="media/consistency-levels/session-consistency.gif" alt-text="Video":::

- **Präfixkonsistenz**: Die zurückgegebenen Updates enthalten ein bestimmtes Präfix aller Updates ohne Lücken. Die konsistente Präfixkonsistenzebene garantiert, dass für Lesevorgänge niemals Schreibvorgänge in falscher Reihenfolge angezeigt werden.

Wenn Schreibvorgänge in der Reihenfolge `A, B, C` erfolgen, wird einem Client entweder `A`, `A,B` oder `A,B,C`, aber niemals falsche Permutationen wie `A,C` oder `B,A,C` angezeigt. Die Präfixkonsistenz bietet Schreibwartezeiten, Verfügbarkeit und Lesedurchsatz, die mit der letztlichen Konsistenz vergleichbar sind. Darüber hinaus stellt sie Reihenfolgengarantien für Szenarien bereit, in denen die Reihenfolge relevant ist. 

Im Folgenden finden Sie die Konsistenzgarantien für Präfixkonsistenz:

- Konsistenz für Clients in derselben Region für ein Einzelmasterkonto = Präfixkonsistenz
- Konsistenz für Clients in unterschiedlichen Regionen für ein Einzelmasterkonto = Präfixkonsistenz
- Konsistenz für Clients, die in einer einzelnen Region für ein Multimasterkonto schreiben = Präfixkonsistenz
- Konsistenz für Clients, die in mehrere Regionen für ein Multimasterkonto schreiben = Letztlich

In der folgenden Grafik wird die Präfixkonsistenz anhand von Noten veranschaulicht. In allen Regionen werden bei Lesevorgängen niemals Schreibvorgänge in der falschen Reihenfolge angezeigt:

  :::image type="content" source="media/consistency-levels/consistent-prefix.gif" alt-text="Video":::

- **Letztlich (Eventual)** : Es gibt keine Reihenfolgengarantie für Lesevorgänge. Wenn keine weiteren Schreibvorgänge vorhanden sind, konvergieren die Replikate schließlich.  
Die letztliche Konsistenz stellt die schwächste Form der Konsistenz dar, da ein Client unter Umständen Werte liest, die älter als die zuvor gelesenen Werte sind. Letztliche Konsistenz ist ideal, wenn die Anwendung keinerlei Reihenfolgengarantien erfordert. Ein Beispiel wäre etwa das Zählen von Retweets, „Gefällt mir“-Markierungen oder Kommentaren ohne Thread. In der folgenden Grafik wird die letztliche Konsistenz anhand von Noten veranschaulicht:

  :::image type="content" source="media/consistency-levels/eventual-consistency.gif" alt-text="Video":::

## <a name="additional-reading"></a>Zusätzliche Lektüre

In folgenden Ressourcen erfahren Sie mehr über Konsistenzkonzepte:

- [High-level TLA+ specifications for the five consistency levels offered by Azure Cosmos DB](https://github.com/Azure/azure-cosmos-tla) (Allgemeine TLA+-Spezifikationen für die fünf von Azure Cosmos DB angebotenen Konsistenzebenen)
- [Erläuterung der Konsistenz replizierter Daten anhand von Baseball (Video) von Doug Terry](https://www.youtube.com/watch?v=gluIh8zd26I)
- [Erläuterung der Konsistenz replizierter Daten anhand von Baseball (Whitepaper) von Doug Terry](https://www.microsoft.com/research/publication/replicated-data-consistency-explained-through-baseball/)
- [Sitzungsgarantien für schwach konsistente replizierte Daten](https://dl.acm.org/citation.cfm?id=383631)
- [Konsistenzkompromisse im Design moderner verteilter Datenbanksysteme: CAP ist nur ein Teil der Wahrheit](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Probabilistic Bounded Staleness (PBS) for Practical Partial Quorums](https://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
- [Eventually Consistent – Revisited](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html) (Letztliche Konsistenz – Neuauflage)

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um mehr über die Konsistenzebenen in Azure Cosmos DB zu erfahren:

- [Auswählen der richtigen Konsistenzebene für Ihre Anwendung](consistency-levels-choosing.md)
- [Konsistenzebenen und Azure Cosmos DB-APIs](consistency-levels-across-apis.md)
- [Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen](consistency-levels-tradeoffs.md)
- [Konfigurieren der Standardkonsistenzebene](how-to-manage-consistency.md#configure-the-default-consistency-level)
- [Außerkraftsetzen der Standardkonsistenzebene](how-to-manage-consistency.md#override-the-default-consistency-level)
