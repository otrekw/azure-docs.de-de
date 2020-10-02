---
title: Bewährte Methoden für die Entwicklung
description: Dieser Artikel enthält Empfehlungen und bewährte Methoden, die Sie kennen sollten, wenn Sie Lösungen für Synapse-SQL-Pools entwickeln.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 09/04/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: f8db19b209ddefebd4a297f18fb9b178fcf40f21
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2020
ms.locfileid: "89457905"
---
# <a name="development-best-practices-for-synapse-sql-pool"></a>Bewährte Methoden für die Entwicklung von Synapse-SQL-Pools

In diesem Artikel werden Anleitungen und bewährte Methoden für die Entwicklung Ihrer SQL-Pool-Lösung beschrieben.

## <a name="tune-query-performance-with-new-product-enhancements"></a>Optimieren der Abfrageleistung mit neuen Produkterweiterungen

- [Leistungsoptimierung mit materialisierten Sichten](performance-tuning-materialized-views.md)
- [Leistungsoptimierung mit einem sortierten gruppierten Columnstore-Index](performance-tuning-ordered-cci.md)
- [Leistungsoptimierung mit Zwischenspeichern von Resultsets](performance-tuning-result-set-caching.md)

## <a name="reduce-cost-with-pause-and-scale"></a>Reduzieren von Kosten durch das Pausieren und Skalieren

Weitere Informationen zur Kostensenkung durch Anhalten und Skalieren finden Sie im Artikel [Verwalten von Computeressourcen](sql-data-warehouse-manage-compute-overview.md).

## <a name="maintain-statistics"></a>Verwalten von Statistiken

SQL-Pools können so konfiguriert werden, dass Statistiken für Spalten automatisch erkannt und erstellt werden.  Die vom Optimierer erstellten Abfragepläne sind nur so gut wie die verfügbaren Statistiken.  

Wir empfehlen, dass Sie AUTO_CREATE_STATISTICS für Ihre Datenbanken aktivieren und die Statistiken täglich oder nach jedem Ladevorgang aktualisieren, um sicherzustellen, dass Statistiken für in Ihren Abfragen verwendete Spalten immer auf dem neuesten Stand sind.

Falls die Aktualisierung aller Ihrer Statistiken zu lange dauert, können Sie versuchen, selektiver auszuwählen, in welchen Spalten Statistiken häufig aktualisiert werden müssen. Beispielsweise können Sie Datumsspalten aktualisieren, wenn täglich neue Werte hinzugefügt werden.

> [!TIP]
> Die größten Vorteile ergeben sich, wenn Sie aktualisierte Statistiken für Spalten in Verknüpfungen, in der WHERE-Klausel verwendete Spalten sowie Spalten in GROUP BY nutzen.

Siehe auch [Verwalten von Tabellenstatistiken](sql-data-warehouse-tables-statistics.md), [CREATE STATISTICS](sql-data-warehouse-tables-statistics.md) und [UPDATE STATISTICS](sql-data-warehouse-tables-statistics.md#update-statistics).

## <a name="hash-distribute-large-tables"></a>Verwenden der Hashverteilung für große Tabellen

Für Tabellen wird standardmäßig die Roundrobin-Verteilung eingesetzt.  Dieser Entwurf erleichtert Benutzern die ersten Schritte beim Erstellen von Tabellen, ohne entscheiden zu müssen, wie die Tabellen verteilt werden sollen.  

Roundrobin-Tabellen ermöglichen für einige Workloads eine ausreichende Leistung, aber in den meisten Fällen führt die Auswahl einer Verteilungsspalte zu einer besseren Leistung.  Das häufigste Beispiel für den Fall, in dem eine von einer Spalte verteilte Tabelle eine deutlich höhere Leistung als eine Roundrobin-Tabelle aufweist, ist die Verknüpfung von zwei großen Faktentabellen.  

Wenn Sie beispielsweise eine Tabelle mit Bestellungen verwenden, die nach „order_id“ verteilt wird, und eine Tabelle mit Transaktionen, die ebenfalls nach „order_id“ verteilt wird, gilt Folgendes: Wenn Sie die Tabelle mit den Bestellungen per „order_id“ mit der Tabelle mit den Transaktionen verknüpfen, wird diese Abfrage zu einer Pass-Through-Abfrage. Dies bedeutet, dass wir Datenverschiebungsvorgänge beseitigen.  Weniger Schritte führen zu einer schnelleren Abfrage.  Außerdem führen weniger Datenverschiebungen zu schnelleren Abfragen.  

Achten Sie beim Laden einer verteilten Tabelle darauf, dass Ihre eingehenden Daten nicht nach dem Verteilungsschlüssel sortiert sind, da dies mit einer Verlangsamung Ihrer Ladevorgänge verbunden ist.  In den folgenden Artikeln finden Sie weitere Einzelheiten zum Verbessern der Leistung durch Auswählen einer Verteilungsspalte; außerdem wird beschrieben, wie Sie eine verteilte Tabelle in der WITH-Klausel Ihrer CREATE TABLES-Anweisung definieren.

Siehe auch [Übersicht über Tabellen](sql-data-warehouse-tables-overview.md), [Tabellenverteilung](sql-data-warehouse-tables-distribute.md), [Auswählen der Tabellenverteilung](https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/), [CREATE TABLE](sql-data-warehouse-tables-overview.md) und [CREATE TABLE AS SELECT](sql-data-warehouse-develop-ctas.md).

## <a name="do-not-over-partition"></a>Vermeiden von übermäßiger Partitionierung

Das Partitionieren von Daten kann für die Verwaltung Ihrer Daten durch Partitionswechsel oder die Optimierung von Scans durch eine Partitionsbeseitigung zwar effektiv sein, aber eine zu hohe Zahl von Partitionen kann Ihre Abfragen verlangsamen.  

Häufig funktioniert eine Partitionierungsstrategie mit zu hoher Granularität, die für SQL Server gut geeignet ist, bei einem SQL-Pool nicht gut.  Die Verwendung von zu vielen Partitionen kann auch die Effektivität von gruppierten Columnstore-Indizes reduzieren, wenn jede Partition weniger als 1 Million Zeilen enthält.  

Beachten Sie, dass ein SQL-Pool Ihre Daten im Hintergrund in 60 Datenbanken partitioniert. Wenn Sie also eine Tabelle mit 100 Partitionen erstellen, ergibt dies tatsächlich insgesamt 6.000 Partitionen.  Jede Workload ist anders, sodass es ratsam ist, mit der Partitionierung zu experimentieren. So können Sie ermitteln, was für Ihre Workload am besten funktioniert.  

> [!TIP]
> Erwägen Sie die Verwendung einer geringeren Granularität als für SQL Server.  Versuchen Sie es beispielsweise anstelle von täglichen Partitionen mit wöchentlichen oder monatlichen Partitionen.

Siehe auch: [Tabellenpartitionierung](sql-data-warehouse-tables-partition.md).

## <a name="minimize-transaction-sizes"></a>Verringern von Transaktionsgrößen

INSERT-, UPDATE- und DELETE-Anweisungen werden in einer Transaktion ausgeführt, und wenn hierbei Fehler auftreten, muss ein Rollback durchgeführt werden.  Es ist ratsam, die Transaktionsgrößen nach Möglichkeit zu verringern, um das Potenzial für einen langen Rollbackvorgang zu reduzieren.  Hierfür können Sie INSERT-, UPDATE- und DELETE-Anweisungen in Teile unterteilen.  

Wenn Sie beispielsweise einen INSERT-Vorgang verwenden, der voraussichtlich eine Stunde dauert, können Sie ihn – falls möglich – in vier Teile unterteilen, die jeweils 15 Minuten dauern.  Nutzen Sie spezielle Fälle mit minimaler Protokollierung, z. B. CTAS, TRUNCATE, DROP TABLE oder INSERT zum Leeren von Tabellen, um das Rollbackrisiko zu verringern.  

Eine weitere Möglichkeit zum Beseitigen von Rollbacks ist die Verwendung von Vorgängen vom Typ „Nur Metadaten“, z.B. ein Partitionswechsel für die Datenverwaltung.  Anstatt z.B. eine DELETE-Anweisung zum Löschen aller Zeilen in einer Tabelle auszuführen, für die das Bestelldatum (order_date) im Oktober 2001 liegt, können Sie Ihre Daten monatlich partitionieren und die Partition dann gegen Daten für eine leere Partition aus einer anderen Tabelle auswechseln (siehe Beispiele zu ALTER TABLE).  

Erwägen Sie bei nicht partitionierten Tabellen die Verwendung eines CTAS-Vorgangs, um die beizubehaltenden Daten in eine Tabelle zu schreiben, statt DELETE zu verwenden.  Wenn für einen CTAS-Vorgang die gleiche Zeit benötigt wird, ist dies ein viel sichererer Vorgang, da er mit minimaler Transaktionsprotokollierung verbunden ist und bei Bedarf schnell abgebrochen werden kann.

Weitere Informationen finden Sie unter [Verwenden von Transaktionen in SQL Data Warehouse](sql-data-warehouse-develop-transactions.md), [Optimieren von Transaktionen in SQL Analytics](sql-data-warehouse-develop-best-practices-transactions.md), [Tabellenpartitionierung](sql-data-warehouse-tables-partition.md), [TRUNCATE TABLE](/sql/t-sql/statements/truncate-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) und [CREATE TABLE AS SELECT (CTAS)](sql-data-warehouse-develop-ctas.md).

## <a name="use-the-smallest-possible-column-size"></a>Verwenden der kleinstmöglichen Spaltengröße

Beim Definieren Ihrer DDL verbessert sich die Abfrageleistung, wenn Sie den kleinsten Datentyp verwenden, der für Ihre Daten unterstützt wird.  Diese Vorgehensweise ist besonders für CHAR- und VARCHAR-Spalten wichtig.  

Wenn der längste Wert in einer Spalte 25 Zeichen lang ist, sollten Sie die Spalte VARCHAR(25) definieren.  Vermeiden Sie es, für alle Zeichenspalten eine hohe Standardlänge zu definieren.  Definieren Sie darüber hinaus Spalten als VARCHAR, sofern dies ausreicht, anstatt NVARCHAR zu verwenden.

Siehe auch: [Übersicht über Tabellen](sql-data-warehouse-tables-overview.md), [Tabellendatentypen](sql-data-warehouse-tables-data-types.md) und [CREATE TABLE](sql-data-warehouse-tables-overview.md).

## <a name="optimize-clustered-columnstore-tables"></a>Optimieren von gruppierten Columnstore-Tabellen

Gruppierte Columnstore-Indizes sind eine der effizientesten Möglichkeiten zum Speichern Ihrer Daten in einem SQL-Pool.  Tabellen werden in SQL-Pools standardmäßig als gruppierter Columnstore erstellt.  

> [!NOTE]
> Die Verwendung einer guten Segmentqualität ist wichtig, um für Abfragen in Columnstore-Tabellen die optimale Leistung zu erzielen.  

Wenn Zeilen bei hohem Arbeitsspeicherdruck in Columnstore-Tabellen geschrieben werden, kann die Qualität von Columnstore-Segmenten leiden.  Die Segmentqualität kann anhand der Anzahl an Zeilen in einer komprimierten Zeilengruppe gemessen werden.  

Eine ausführliche Anleitung zur Erkennung und Verbesserung der Segmentqualität für gruppierte Columnstore-Tabellen finden Sie im Artikel [Tabellenindizes](sql-data-warehouse-tables-index.md) unter [Ursachen für eine schlechte Qualität des Columnstore-Index](sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality).  

Weil qualitativ hochwertige Columnstore-Segmente wichtig sind, ist es eine gute Idee, Benutzer-IDs, die zur mittleren oder großen Ressourcenklasse gehören, zum Laden von Daten zu verwenden. Wenn Sie [Data Warehouse-Einheiten](what-is-a-data-warehouse-unit-dwu-cdwu.md) auf niedrigerer Ebene verwenden, sollten Sie dem ladenden Benutzer eine größere Ressourcenklasse zuweisen.

Da Columnstore-Tabellen Daten generell erst dann in ein komprimiertes Columnstore-Segment übertragen, wenn eine Tabelle mehr als eine Million Zeilen hat und jede SQL-Pool-Tabelle in 60 Tabellen partitioniert ist, empfiehlt sich im Allgemeinen Folgendes: Columnstore-Tabellen stellen für eine Abfrage nur dann einen Vorteil dar, wenn die Tabelle mehr als 60 Millionen Zeilen hat.  

Für Tabellen mit weniger als 60 Millionen Zeilen ist es unter Umständen nicht sinnvoll, einen Columnstore-Index zu nutzen.  Es muss aber nicht unbedingt einen Nachteil bedeuten.  

Wenn Sie Ihre Daten partitionieren, können Sie auch darauf achten, dass jede Partition 1 Million Zeilen aufweisen muss, um von einem gruppierten Columnstore-Index zu profitieren.  Falls eine Tabelle 100 Partitionen hat, muss sie mindestens 6 Milliarden Zeilen enthalten, um von einem gruppierten Columnstore zu profitieren (60 Verteilungen *100 Partitionen* 1 Million Zeilen).  

Falls Ihre Tabelle für dieses Beispiel nicht 6 Milliarden Zeilen enthält, sollten Sie entweder die Anzahl der Partitionen reduzieren oder stattdessen eine Heaptabelle verwenden.  Außerdem kann sich folgendes Experiment lohnen: Ermitteln Sie, ob eine bessere Leistung erzielt werden kann, indem eine Heaptabelle mit sekundären Indizes anstelle einer Columnstore-Tabelle verwendet wird.

> [!TIP]
> Beim Abfragen einer Columnstore-Tabelle werden die Abfragen schneller ausgeführt, wenn Sie nur die benötigten Spalten auswählen.  

Siehe auch [Tabellenindizes](sql-data-warehouse-tables-index.md), [Beschreibung von Columnstore-Indizes](/sql/relational-databases/indexes/columnstore-indexes-overview?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) und [Neuerstellen von Columnstore-Indizes](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality).

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die benötigten Informationen in diesem Artikel nicht gefunden haben, können Sie links auf dieser Seite über die Option „Nach Dokumenten suchen“ alle Dokumente zu Azure Synapse durchsuchen.  

Auf der [Frageseite von Microsoft Q&A zu Azure Synapse](https://docs.microsoft.com/answers/topics/azure-synapse-analytics.html) können Sie anderen Benutzern und der Azure Synapse-Produktgruppe Fragen stellen.  Wir überwachen dieses Forum aktiv, um sicherzustellen, dass Ihre Frage entweder von einem anderen Benutzer oder einem Mitarbeiter beantwortet wird.  

Falls Sie Ihre Fragen lieber über Stack Overflow stellen möchten, können Sie dazu auch das [Stack Overflow-Forum für Azure Synapse Analytics](https://stackoverflow.com/questions/tagged/azure-sqldw) nutzen.

Verwenden Sie die Seite [Azure Synapse-Feedback](https://feedback.azure.com/forums/307516-sql-data-warehouse), um Features anzufordern.  Indem Sie Ihre Anfragen hinzufügen oder für andere Anfragen stimmen, leisten Sie wertvolle Unterstützung beim Priorisieren von Funktionen.
