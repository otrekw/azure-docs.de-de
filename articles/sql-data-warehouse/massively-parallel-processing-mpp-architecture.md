---
title: Azure Synapse Analytics (ehemals SQL DW)-Architektur
description: In diesem Artikel erfahren Sie, wie in Azure Synapse Analytics (ehemals SQL DW) durch Kombination von Massively Parallel Processing (MPP) und Azure Storage hohe Leistung und Skalierbarkeit erzielt werden.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 1d808210861d971b2915206e7be0fe9b955616c5
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720315"
---
# <a name="azure-synapse-analytics-formerly-sql-dw-architecture"></a>Azure Synapse Analytics (ehemals SQL DW)-Architektur 

Azure Synapse ist ein unbegrenzter Analysedienst, der Data Warehousing für Unternehmen mit Big Data-Analysen vereint. Er ermöglicht flexible Datenabfragen nach Ihren Vorstellungen, indem serverlose On-Demand-Ressourcen oder bereitgestellten Ressourcen im gewünschten Umfang verwendet werden. Azure Synapse kombiniert diese beiden Welten in einer vereinheitlichten Oberfläche zur Erfassung, Vorbereitung, Verwaltung und Verarbeitung von Daten für sofortige BI- und Machine Learning-Anforderungen.

 Azure Synapse besteht aus vier Komponenten:
- SQL-Analyse: Vollständige T-SQL-basierte Analyse 
    - SQL-Pool (Bezahlung pro bereitgestellter DWU) – Allgemein verfügbar
    - SQL On-Demand (Bezahlung pro verarbeitetem TB) – Vorschau
- Spark: Fest integriertes Apache Spark (Vorschau) 
- Datenintegration: Hybrid-Datenintegration (Vorschau)
- Studio: einheitliche Benutzeroberfläche.  (Vorschau)

> [!VIDEO https://www.youtube.com/embed/PlyQ8yOb8kc]

## <a name="sql-analytics-mpp-architecture-components"></a>Komponenten der MPP-Architektur für SQL-Analyse

[SQL-Analyse](sql-data-warehouse-overview-what-is.md#sql-analytics-and-sql-pool-in-azure-synapse) nutzt eine horizontal skalierte Architektur zur Verteilung der Berechnungsverarbeitung von Daten auf mehrere Knoten. Die Skalierungseinheit ist eine Abstraktion der Computeleistung, die als [Data Warehouse-Einheit](what-is-a-data-warehouse-unit-dwu-cdwu.md) bezeichnet wird. Compute- und Speicherressourcen sind getrennt, sodass Sie Compute unabhängig von den Daten in Ihrem System skalieren können.

![Architektur von SQL-Analyse](media/massively-parallel-processing-mpp-architecture/massively-parallel-processing-mpp-architecture.png)

SQL-Analyse verwendet eine knotenbasierte Architektur. Anwendungen stellen eine Verbindung her und geben T-SQL-Befehle an einen Steuerknoten aus, der der einzige Einstiegspunkt für SQL-Analyse ist. Der Steuerknoten führt die MPP-Engine aus, die Abfragen für die Parallelverarbeitung optimiert und dann Vorgänge an Serverknoten übergibt, sodass die Vorgänge parallel ausgeführt werden. 

Auf den Serverknoten werden alle Benutzerdaten in Azure Storage gespeichert und die parallelen Abfragen ausgeführt. Der Datenverschiebungsdienst (Data Movement Service, DMS) ist ein interner Dienst auf Systemebene, der Daten nach Bedarf zwischen den Knoten verschiebt, sodass Abfragen parallel ausgeführt und genaue Ergebnisse zurückgegeben werden. 

Mit entkoppelten Speicher- und Computeressourcen können Sie bei der Verwendung von SQL-Analyse folgende Aktionen ausführen:

* Anpassen der Größe der Computeleistung unabhängig von den Speicheranforderungen
* Vergrößern oder Verkleinern der Computeleistung innerhalb eines SQL-Pools (Data Warehouse), ohne Daten verschieben zu müssen.
* Anhalten der Computekapazität ohne Beeinträchtigung der Daten (und nur Bezahlung für den Speicher)
* Fortsetzen der Computekapazität während der Betriebszeiten

### <a name="azure-storage"></a>Azure Storage

SQL-Analyse nutzt Azure Storage, um Ihre Benutzerdaten zu schützen.  Da Ihre Daten von Azure Storage gespeichert und verwaltet werden, werden die Kosten für Ihre Speichernutzung getrennt berechnet. Die Daten werden zur Optimierung der Systemleistung in **Verteilungen** horizontal partitioniert. Beim Definieren der Tabelle können Sie das Shardingmuster zum Verteilen der Daten auswählen. Folgende Shardingmuster werden unterstützt:

* Hash
* Roundrobin
* Replikat

### <a name="control-node"></a>Steuerknoten

Der Steuerknoten ist der zentrale Knoten der Architektur. Dies ist das Front-End, das mit allen Anwendungen und Verbindungen interagiert. Die MPP-Engine wird auf dem Steuerknoten ausgeführt, um parallele Abfragen zu optimieren und zu koordinieren. Wenn Sie eine T-SQL-Abfrage an SQL-Analyse übermitteln, wird sie vom Steuerknoten in Abfragen transformiert, die für die einzelnen Verteilungen parallel ausgeführt werden.

### <a name="compute-nodes"></a>Serverknoten

Die Serverknoten liefern die Rechnerleistung. Verteilungen werden Serverknoten zur Verarbeitung zugeordnet. Wenn Sie für weitere Computeressourcen bezahlen, ordnet SQL-Analyse die Verteilungen den verfügbaren Serverknoten neu zu. Die Anzahl der Serverknoten liegt zwischen 1 und 60 und wird durch die Dienstebene für SQL-Analyse bestimmt.

Jedem Serverknoten ist eine Knoten-ID zugewiesen, die in Systemsichten sichtbar ist. Sie können die Serverknoten-ID anzeigen, indem Sie in den Systemsichten, deren Namen mit „sys.pdw_nodes“ beginnen, nach der Spalte „node_id“ suchen. Eine Liste dieser Systemsichten finden Sie unter [MPP-Systemsichten](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views?view=aps-pdw-2016-au7).

### <a name="data-movement-service"></a>Datenverschiebungsdienst
Der Datenverschiebungsdienst (Data Movement Service, DMS) ist die Datentransporttechnologie, mit der die Datenverschiebung zwischen den Serverknoten koordiniert wird. Einige Abfragen erfordern eine Datenverschiebung, damit sichergestellt ist, dass die parallelen Abfragen genaue Ergebnisse zurückgeben. Wenn eine Datenverschiebung erforderlich ist, wird durch den Datenverschiebungsdienst sichergestellt, dass die richtigen Daten an die richtige Position verschoben werden. 

## <a name="distributions"></a>Verteilungen

Eine Verteilung ist die Basiseinheit zur Speicherung und Verarbeitung von parallelen Abfragen, die für verteilte Daten ausgeführt werden. Wenn SQL-Analyse eine Abfrage ausführt, wird der Vorgang in 60 kleinere Abfragen unterteilt, die parallel ausgeführt werden. 

Die einzelnen 60 kleineren Abfragen werden jeweils auf einer der Datenverteilungen ausgeführt. Auf jedem Serverknoten werden eine oder mehrere der 60 Verteilungen verwaltet. Bei einem SQL-Pool mit maximalen Computeressourcen befindet sich eine Verteilung auf jeweils einem Serverknoten. Bei einem SQL-Pool mit minimalen Computeressourcen befinden sich alle Verteilungen auf einem einzigen Serverknoten.  

## <a name="hash-distributed-tables"></a>Tabellen mit Hashverteilung
Eine Tabelle mit Hashverteilung kann die höchste Abfrageleistung für Verknüpfungen und Aggregationen in großen Tabellen bieten. 

Zum horizontalen Partitionieren von Daten in eine Tabelle mit Hashverteilung verwendet SQL-Analyse eine Hashfunktion, um jede Zeile einer einzigen Verteilung deterministisch zuzuweisen. In der Tabellendefinition wird eine der Spalten als Verteilungsspalte festgelegt. Die Hashfunktion verwendet die Werte in der Verteilungsspalte, um jede Zeile einer Verteilung zuzuweisen.

Im folgenden Diagramm ist dargestellt, wie eine vollständige (nicht verteilte) Tabelle als Tabelle mit Hashverteilung gespeichert wird. 

![Verteilte Tabelle](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "Verteilte Tabelle")  

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

![Replizierte Tabelle](media/sql-data-warehouse-distributed-data/replicated-table.png "Replizierte Tabelle") 

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit den Grundlagen von Azure Synapse vertraut gemacht haben, informieren Sie sich nun darüber, wie Sie innerhalb kurzer Zeit [einen SQL-Pool erstellen](./sql-data-warehouse-get-started-provision.md) und [Beispieldaten laden](./sql-data-warehouse-load-sample-databases.md) können. Falls Sie mit Azure noch nicht vertraut sind und auf neue Terminologie stoßen, ist das [Azure-Glossar](../azure-glossary-cloud-terminology.md) sehr nützlich. Oder sehen Sie sich einige der folgenden weiteren Azure Synapse-Ressourcen an:  

* [Kundenerfolgsgeschichten](https://azure.microsoft.com/case-studies/?service=sql-data-warehouse)
* [Blogs](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
* [Funktionsanfragen](https://feedback.azure.com/forums/307516-sql-data-warehouse)
* [Videos](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
* [Erstellen eines Supporttickets](./sql-data-warehouse-get-started-create-support-ticket.md)
* [MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureSQLDataWarehouse)
* [Stack Overflow-Forum](https://stackoverflow.com/questions/tagged/azure-sqldw)
* [Twitter](https://twitter.com/hashtag/SQLDW)

