---
title: 'Synapse SQL: Architektur'
description: In diesem Artikel erfahren Sie, wie Azure Synapse SQL die Verarbeitungsfunktionen für verteilte Abfragen mit Azure Storage kombiniert, um hohe Leistung und Skalierbarkeit zu erzielen.
services: synapse-analytics
author: mlee3gsd
manager: rothja
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: ae3b54ca72c92722dffa370b0b8be1ca2c490f97
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2020
ms.locfileid: "92476007"
---
# <a name="azure-synapse-sql-architecture"></a>Azure Synapse SQL-Architektur 

In diesem Artikel werden die Architekturkomponenten von Synapse SQL beschrieben.

[!INCLUDE [preview](../includes/note-preview.md)]

## <a name="synapse-sql-architecture-components"></a>Architekturkomponenten von Synapse SQL

Synapse SQL nutzt eine aufskalierte Architektur zur Verteilung der Berechnungsverarbeitung von Daten auf mehrere Knoten. Compute- und Speicherressourcen sind getrennt, sodass Sie Compute unabhängig von den Daten in Ihrem System skalieren können. 

Für den SQL-Pool ist Skalierungseinheit eine Abstraktion der Computeleistung, die als [Data Warehouse-Einheit](resource-consumption-models.md) bezeichnet wird. 

Beim serverlosen SQL On-Demand erfolgt die Skalierung automatisch, um den Anforderungen an die Abfrageressourcen zu entsprechen. Wenn sich die Topologie im Laufe der Zeit durch Hinzufügen, Entfernen von Knoten oder Failover ändert, passt sie sich den Änderungen an und stellt sicher, dass Ihre Abfrage über genügend Ressourcen verfügt und erfolgreich abgeschlossen wird. Die Abbildung unten zeigt zum Beispiel SQL On-Demand unter Verwendung von vier Computeknoten zum Ausführen einer Abfrage.

![Synapse SQL: Architektur](./media//overview-architecture/sql-architecture.png)

Synapse SQL verwendet eine knotenbasierte Architektur. Anwendungen stellen eine Verbindung her und geben T-SQL-Befehle an einen Steuerknoten aus, der der einzige Einstiegspunkt für Synapse SQL ist. 

Der Azure Synapse SQL-Steuerknoten nutzt eine Engine für verteilte Abfragen, um die Abfragen für die Parallelverarbeitung zu optimieren und dann Vorgänge an Computeknoten zu übergeben, sodass die Vorgänge parallel ausgeführt werden. 

Der SQL On-Demand-Steuerknoten verwendet eine DQP-Engine (Distributed Query Processing), um die verteilte Ausführung von Benutzerabfragen zu optimieren und zu orchestrieren, indem er sie in kleinere Abfragen aufteilt, die auf den Computeknoten ausgeführt werden. Jede kleine Abfrage wird als Aufgabe bezeichnet und stellt eine verteilte Ausführungseinheit dar. Sie liest Dateien aus dem Speicher, verknüpft Ergebnisse aus anderen Aufgaben, gruppiert oder ordnet Daten an, die aus anderen Aufgaben abgerufen wurden. 

Auf den Serverknoten werden alle Benutzerdaten in Azure Storage gespeichert und die parallelen Abfragen ausgeführt. Der Datenverschiebungsdienst (Data Movement Service, DMS) ist ein interner Dienst auf Systemebene, der Daten nach Bedarf zwischen den Knoten verschiebt, sodass Abfragen parallel ausgeführt und genaue Ergebnisse zurückgegeben werden. 

Durch die Entkopplung von Speicherung und Compute profitieren Sie beim Einsatz von Synapse SQL von einer unabhängigen Skalierung der Computeleistung, die unabhängig von Ihrem Speicherbedarf ist. Bei SQL On-Demand erfolgt die Skalierung automatisch, während beim SQL-Pool Folgendes möglich ist:

* Vergrößern oder Verkleinern der Computeleistung innerhalb eines SQL-Pools (Data Warehouse), ohne Daten verschieben zu müssen.
* Anhalten der Computekapazität ohne Beeinträchtigung der Daten (und nur Bezahlung für den Speicher)
* Fortsetzen der Computekapazität während der Betriebszeiten

## <a name="azure-storage"></a>Azure Storage

Synapse SQL nutzt Azure Storage, um Benutzerdaten zu schützen. Da Ihre Daten von Azure Storage gespeichert und verwaltet werden, werden die Kosten für Ihre Speichernutzung getrennt berechnet. 

Mit SQL On-Demand können Sie Dateien in Ihrem Data Lake schreibgeschützt abfragen, während Sie mit dem SQL-Pool auch Daten erfassen können. Wenn Daten in den SQL-Pool aufgenommen werden, werden sie zur Optimierung der Systemleistung in **Verteilungen** aufgeteilt. Beim Definieren der Tabelle können Sie das Shardingmuster zum Verteilen der Daten auswählen. Folgende Shardingmuster werden unterstützt:

* Hash
* Roundrobin
* Replikat

## <a name="control-node"></a>Steuerknoten

Der Steuerknoten ist der zentrale Knoten der Architektur. Dies ist das Front-End, das mit allen Anwendungen und Verbindungen interagiert. 

Die Engine für verteilte Abfragen wird in Synapse SQL auf dem Steuerknoten ausgeführt, um parallele Abfragen zu optimieren und zu koordinieren. Wenn Sie eine T-SQL-Abfrage an den SQL-Pool übermitteln, wird sie vom Steuerknoten in Abfragen transformiert, die für die einzelnen Verteilungen parallel ausgeführt werden.

In SQL On-Demand wird die DQP-Engine auf dem Steuerknoten ausgeführt, um die verteilte Ausführung der Benutzerabfrage zu optimieren und zu koordinieren, indem sie in kleinere Abfragen aufgeteilt wird, die auf den Computeknoten ausgeführt werden. Sie weist auch Sätze von Dateien zu, die von den einzelnen Knoten verarbeitet werden sollen.

## <a name="compute-nodes"></a>Serverknoten

Die Serverknoten liefern die Rechnerleistung. 

Bei SQL-Pool werden Verteilungen zur Verarbeitung zu Serverknoten zugeordnet. Wenn Sie für weitere Computeressourcen bezahlen, ordnet der Pool die Verteilungen den verfügbaren Serverknoten neu zu. Die Anzahl der Serverknoten liegt zwischen 1 und 60 und wird durch die Dienstebene für den SQL-Pool bestimmt. Jedem Serverknoten ist eine Knoten-ID zugewiesen, die in Systemsichten sichtbar ist. Sie können die Serverknoten-ID anzeigen, indem Sie in den Systemsichten, deren Namen mit „sys.pdw_nodes“ beginnen, nach der Spalte „node_id“ suchen. Eine Liste dieser Systemsichten finden Sie unter [Synapse SQL-Systemsichten](/sql/relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views?view=azure-sqldw-latest).

Bei SQL On-Demand wird jedem Computeknoten eine Aufgabe und ein Satz von Dateien zugewiesen, für die die Aufgabe ausgeführt werden soll. „Aufgabe“ ist eine verteilte Abfrageausführungseinheit, die eigentlich Teil der vom Benutzer eingereichten Abfrage ist. Es erfolgt eine automatische Skalierung, um sicherzustellen, dass genügend Computeknoten zur Ausführung der Benutzerabfrage verwendet werden.

## <a name="data-movement-service"></a>Datenverschiebungsdienst

Der Datenverschiebungsdienst (Data Movement Service, DMS) ist die Datentransporttechnologie im SQL-Pool, mit der die Datenverschiebung zwischen den Serverknoten koordiniert wird. Einige Abfragen erfordern eine Datenverschiebung, damit sichergestellt ist, dass die parallelen Abfragen genaue Ergebnisse zurückgeben. Wenn eine Datenverschiebung erforderlich ist, wird durch den Datenverschiebungsdienst sichergestellt, dass die richtigen Daten an die richtige Position verschoben werden.

> [!VIDEO https://www.youtube.com/embed/PlyQ8yOb8kc]

## <a name="distributions"></a>Verteilungen

Eine Verteilung ist die Basiseinheit zur Speicherung und Verarbeitung von parallelen Abfragen, die für verteilte Daten im SQL-Pool ausgeführt werden. Wenn der SQL-Pool eine Abfrage ausführt, wird der Vorgang in 60 kleinere Abfragen unterteilt, die parallel ausgeführt werden. 

Die einzelnen 60 kleineren Abfragen werden jeweils auf einer der Datenverteilungen ausgeführt. Auf jedem Serverknoten werden eine oder mehrere der 60 Verteilungen verwaltet. Bei einem SQL-Pool mit maximalen Computeressourcen befindet sich eine Verteilung auf jeweils einem Serverknoten. Bei einem SQL-Pool mit minimalen Computeressourcen befinden sich alle Verteilungen auf einem einzigen Serverknoten. 

## <a name="hash-distributed-tables"></a>Tabellen mit Hashverteilung
Eine Tabelle mit Hashverteilung kann die höchste Abfrageleistung für Verknüpfungen und Aggregationen in großen Tabellen bieten. 

Zum horizontalen Partitionieren von Daten in eine Tabelle mit Hashverteilung verwendet der SQL-Pool eine Hashfunktion, um jede Zeile einer einzigen Verteilung deterministisch zuzuweisen. In der Tabellendefinition wird eine der Spalten als Verteilungsspalte festgelegt. Die Hashfunktion verwendet die Werte in der Verteilungsspalte, um jede Zeile einer Verteilung zuzuweisen.

Im folgenden Diagramm ist dargestellt, wie eine vollständige (nicht verteilte) Tabelle als Tabelle mit Hashverteilung gespeichert wird. 

![Verteilte Tabelle](media//overview-architecture/hash-distributed-table.png "Verteilte Tabelle") 

* Jede Zeile gehört zu einer Verteilung. 
* Mit einem deterministischen Hashalgorithmus wird jede Zeile einer Verteilung zugewiesen. 
* Die Anzahl von Tabellenzeilen pro Verteilung variiert, wie an den unterschiedlichen Größen von Tabellen zu sehen ist.

Bei der Auswahl einer Verteilungsspalte sollten einige Aspekte in Bezug auf die Leistung beachtet werden, z.B. Eindeutigkeit, eventuelle Datenschiefe und die Arten von Abfragen, die im System ausgeführt werden.

## <a name="round-robin-distributed-tables"></a>Tabellen mit Roundrobin-Verteilung

Eine Roundrobin-Tabelle lässt sich am einfachsten erstellen und bietet schnelle Leistung, wenn sie als Stagingtabelle für Ladevorgänge verwendet wird.

In einer Tabelle mit Roundrobin-Verteilung werden Daten gleichmäßig auf die Tabelle aufgeteilt, dies erfolgt jedoch ohne weitere Optimierung. Eine Verteilung wird zunächst nach dem Zufallsprinzip ausgewählt. Dann werden Zeilenpuffer sequenziell Verteilungen zugewiesen. Die Daten lassen sich schnell in eine Roundrobin-Tabelle laden, die Abfrageleistung ist jedoch häufig bei Tabellen mit Hashverteilung besser. Verknüpfungen für Roundrobin-Tabellen machen eine neue Verteilung der Daten erforderlich. Dies erfordert zusätzlich Zeit.

## <a name="replicated-tables"></a>Replizierte Tabellen
Eine replizierte Tabelle bietet die schnellste Abfrageleistung für kleine Tabellen.

In einer Tabelle, die repliziert wird, wird eine vollständige Kopie der Tabelle auf jedem Serverknoten zwischengespeichert. Daher müssen beim Replizieren einer Tabelle Daten vor einer Verknüpfung oder Aggregation nicht mehr auf Serverknoten übertragen werden. Replizierte Tabellen werden am besten mit kleinen Tabellen verwendet. Zusätzlicher Speicherplatz ist erforderlich, und beim Schreiben von Daten entsteht zusätzlicher Mehraufwand, sodass dies für große Tabellen nicht praktikabel ist. 

Das nachstehende Diagramm zeigt eine replizierte Tabelle, die bei der ersten Verteilung auf den einzelnen Serverknoten zwischengespeichert wird. 

![Replizierte Tabelle](media/overview-architecture/replicated-table.png "Replizierte Tabelle") 

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie jetzt ein wenig über Synapse SQL wissen, erfahren Sie, wie Sie schnell [einen SQL-Pool erstellen](../quickstart-create-sql-pool-portal.md) und [Beispieldaten laden](../sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) können (./sql-data-warehouse-load-sample-databases.md). Oder Sie starten [mithilfe von SQL On-Demand](../quickstart-sql-on-demand.md). Falls Sie mit Azure noch nicht vertraut sind und auf neue Terminologie stoßen, ist das [Azure-Glossar](../../azure-glossary-cloud-terminology.md) sehr nützlich. 
