---
title: Kompromisse in Bezug auf Konsistenz, Verfügbarkeit und Leistung in Azure Cosmos DB
description: Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen in Azure Cosmos DB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/23/2020
ms.reviewer: sngun
ms.openlocfilehash: 4e2cb2b93010478338cd40236403da4ca0ca99fc
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91825267"
---
# <a name="latency-availability-and-performance-tradeoffs-with-different-azure-cosmos-db-consistency-levels"></a>Kompromisse in Bezug auf Latenz, Verfügbarkeit und Leistung bei verschiedenen Azure Cosmos DB-Konsistenzebenen

Bei verteilten Datenbanken, die mithilfe der Replikation Hochverfügbarkeit, niedrige Latenzzeiten oder beides sicherstellen, müssen Kompromisse eingegangen werden. Diese Kompromisse betreffen die Lesekonsistenz im Vergleich zu Verfügbarkeit, Latenz und Durchsatz.

Azure Cosmos DB bietet in Bezug auf die Datenkonsistenz vielfältige Auswahlmöglichkeiten. Dieser Ansatz beinhaltet mehr Optionen als die beiden Extreme der starken und der letztlichen Konsistenz. Sie können im Hinblick auf die Konsistenz aus fünf gut definierten Ebenen auswählen. Diese sind (von der stärksten bis zur schwächsten Ebene):

- *Starke Konsistenz*
- *Begrenzte Veraltung*
- *Sitzungskonsistenz*
- *Präfixkonsistenz*
- *Letztliche Konsistenz*

Jede Ebene ermöglicht Verfügbarkeits- und Leistungskompromisse und wird durch umfassende SLAs abgesichert.

## <a name="consistency-levels-and-latency"></a>Konsistenzebenen und Latenz

Für alle Konsistenzebenen wird eine Leselatenz garantiert, die jederzeit unter 10 Millisekunden im 99. Perzentil liegt. Diese Leselatenz wird durch die SLA abgesichert. Die durchschnittliche Leselatenz (im 50. Perzentil) beträgt typischerweise 4 Millisekunden oder weniger.

Für alle Konsistenzebenen wird eine Schreiblatenz garantiert, die jederzeit unter 10 Millisekunden im 99. Perzentil liegt. Außerdem wird sie durch die SLA unterstützt. Die durchschnittliche Schreiblatenz (im 50. Perzentil) beträgt üblicherweise 5 Millisekunden oder weniger. Eine Ausnahme bei dieser Garantie bilden Azure Cosmos-Konten, die sich über mehrere Regionen erstrecken und mit starker Konsistenz konfiguriert sind.

### <a name="write-latency-and-strong-consistency"></a>Schreiblatenz und starke Konsistenz

Für Azure Cosmos-Konten, die mit starker Konsistenz mit mehreren Regionen konfiguriert sind, beträgt die Schreiblatenz der doppelten Roundtripzeit (Round-Trip Time, RTT) zwischen den beiden am weitesten entfernten Regionen plus 10 Millisekunden im 99. Perzentil. Eine hohe Netzwerk-RTT zwischen den Regionen führt zu einer höheren Latenz für Cosmos DB-Anforderungen, da ein Vorgang bei starker Konsistenz erst abgeschlossen wird, nachdem sichergestellt wurde, dass er in allen Regionen innerhalb eines Kontos committet wurde.

Die exakte RTT-Latenz richtet sich nach der physischen Entfernung und der Azure-Netzwerktopologie. Azure-Netzwerke bieten keine Latenz-SLAs für die RTT zwischen zwei Azure-Regionen. Die Replikationslatenzzeiten für Ihr Azure Cosmos-Konto werden im Azure-Portal angezeigt. Sie können das Azure-Portal (wählen Sie auf dem Blatt „Metriken“ die Registerkarte „Konsistenz“ aus) nutzen, um die Replikationslatenz zwischen verschiedenen Regionen zu überwachen, die Ihrem Azure Cosmos-Konto zugeordnet sind.

> [!IMPORTANT]
> Starke Konsistenz wird für Konten mit Regionen, die mehr als 5.000 Meilen (8.000 Kilometer) umfassen, standardmäßig aufgrund der hohen Schreiblatenz blockiert. Wenden Sie sich an den Support, um diese Funktion zu aktivieren.

## <a name="consistency-levels-and-throughput"></a>Konsistenzebenen und Durchsatz

- Für starke und begrenzte Veraltung werden Lesevorgänge für zwei Replikate in einer Gruppe mit vier Replikaten (Minderheitsquorum) durchgeführt, um die Konsistenz zu garantieren. Die Lesevorgänge werden in der Reihenfolge „Sitzung“, „Präfixkonsistenz“ und „Letztlich“ durchgeführt. Im Ergebnis ist der Lesedurchsatz für die gleiche Anzahl von Anforderungseinheiten für starke und begrenzte Veraltung halb so hoch wie bei anderen Konsistenzebenen.

- Für bestimmte Schreibvorgänge (z.B. Einfügen, Ersetzen, Upsert, Löschen) ist der Schreibdurchsatz für RUs für alle Konsistenzebenen identisch.

|**Konsistenzebene**|**Quorumlesevorgänge**|**Quorumschreibvorgänge**|
|--|--|--|
|**Starke Konsistenz**|Lokale Minderheit|Globale Mehrheit|
|**Begrenzte Veraltung**|Lokale Minderheit|Lokale Mehrheit|
|**Sitzungskonsistenz**|Einzelnes Replikat (mit Sitzungstoken)|Lokale Mehrheit|
|**Präfixkonsistenz**|Einzelnes Replikat|Lokale Mehrheit|
|**Letztliche Konsistenz**|Einzelnes Replikat|Lokale Mehrheit|

## <a name="consistency-levels-and-data-durability"></a><a id="rto"></a>Konsistenzebenen und Datendauerhaftigkeit

Bei einer global verteilten Datenbankumgebung besteht eine direkte Beziehung zwischen der Konsistenzebene und der Datendauerhaftigkeit bei einem Ausfall in der gesamten Region. Wenn Sie Ihren Plan für die Geschäftskontinuität entwickeln, müssen Sie wissen, wie viel Zeit maximal vergehen darf, bis die Anwendung nach einer Störung vollständig wiederhergestellt ist. Die Zeit, die für die vollständige Wiederherstellung einer Anwendung erforderlich ist, wird als **RTO** (**Recovery Time Objective**) bezeichnet. Sie müssen auch wissen, über welchen Zeitraum kürzlich durchgeführte Datenupdates maximal verloren gehen dürfen, wenn die Anwendung nach einer Störung wiederhergestellt wird. Der Zeitraum der Updates, der verloren gehen darf, wird als **RPO** (**Recovery Point Objective**) bezeichnet.

In der folgenden Tabelle wird die Beziehung zwischen dem Konsistenzmodell und der Datendauerhaftigkeit bei einem regionsweiten Ausfall definiert. Beachten Sie unbedingt, dass es in einem verteilten System auch bei starker Konsistenz unmöglich ist, eine verteilte Datenbank mit einem RPO- und RTO-Wert von 0 (null) zu erzielen. Grund ist das CAP-Theorem. Weitere Informationen finden Sie unter [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md).

|**Region(en)**|**Replikationsmodus**|**Konsistenzebene**|**RPO**|**RTO**|
|---------|---------|---------|---------|---------|
|1|Eine oder mehrere Schreibregionen|Jede Konsistenzebene|< 240 Minuten|< 1 Woche|
|> 1|Eine Schreibregion|Sitzung, Präfixkonsistenz, Letztlich|< 15 Minuten|< 15 Minuten|
|> 1|Eine Schreibregion|Begrenzte Veraltung (Bounded staleness)|*K* & *T*|< 15 Minuten|
|> 1|Eine Schreibregion|STARK (Strong)|0|< 15 Minuten|
|> 1|Mehrere Schreibregionen|Sitzung, Präfixkonsistenz, Letztlich|< 15 Minuten|0|
|> 1|Mehrere Schreibregionen|Begrenzte Veraltung (Bounded staleness)|*K* & *T*|0|

*K* = Anzahl von *„K“* -Versionen (d. h. Updates) eines Elements.

*T* = Zeitintervall *„T“* seit dem letzten Update.

## <a name="strong-consistency-and-multiple-write-regions"></a>Hohe Konsistenz und mehrere Schreibregionen

Mit mehreren Schreibregionen konfigurierte Cosmos-Konten können nicht für hohe Konsistenz konfiguriert werden, da es für ein verteiltes System nicht möglich ist, einen RPO-Wert von 0 (null) und einen RTO-Wert von 0 zu erreichen. Außerdem gibt es keine Vorteile hinsichtlich der Schreiblatenz bei Verwendung von hoher Konsistenz mit mehreren Schreibregionen, da die Schreibanforderung für jede Region repliziert und in allen konfigurierten Regionen innerhalb des Kontos festgeschrieben werden muss. Dies führt zur selben Schreiblatenz wie bei einem Konto mit nur einer Schreibregion.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel bieten weitere Informationen zur globalen Verteilung und zu den allgemeinen Konsistenzkompromissen in verteilten Systemen. Weitere Informationen finden Sie in folgenden Artikeln:

- [Konsistenzkompromisse im Design moderner verteilter Datenbanksysteme](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Hochverfügbarkeit](high-availability.md)
- [Azure Cosmos DB-SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_3/)
