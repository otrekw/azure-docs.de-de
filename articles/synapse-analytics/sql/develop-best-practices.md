---
title: Bewährte Methoden für die Entwicklung für Synapse SQL
description: Dieser Artikel enthält Empfehlungen und bewährte Methoden, die Sie kennen sollten, wenn Sie für Synapse SQL entwickeln.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 9a291971ce0edead9ca28a47f7ad0689b0f65547
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83834950"
---
# <a name="development-best-practices-for-synapse-sql"></a>Bewährte Methoden für die Entwicklung für Synapse SQL
In diesem Artikel werden Anleitungen und bewährte Methoden für die Entwicklung Ihrer Data Warehouse-Lösung beschrieben. 

## <a name="sql-pool-development-best-practices"></a>Bewährte Methoden für die SQL-Poolentwicklung

### <a name="reduce-cost-with-pause-and-scale"></a>Reduzieren von Kosten durch das Pausieren und Skalieren

Weitere Informationen zur Kostensenkung durch Anhalten und Skalieren finden Sie im [Artikel zur Computeverwaltung](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="maintain-statistics"></a>Verwalten von Statistiken

Stellen Sie sicher, dass Sie Ihre Statistiken täglich oder nach jedem Ladevorgang aktualisieren.  Es gibt immer Vor- und Nachteile, was die Leistung einerseits und die Kosten für die Erstellung und Aktualisierung von Statistiken andererseits betrifft. Falls die Verwaltung aller Statistiken zu lange dauert, können Sie selektiver auszuwählen, welche Spalten über Statistiken verfügen sollen oder häufig aktualisiert werden müssen.  

Beispielsweise könnte es sein, dass Sie Datumsspalten aktualisieren möchten, zu denen auf täglicher Basis neue Werte hinzugefügt werden sollen. 

> [!NOTE]
> Die größten Vorteile ergeben sich, wenn Sie Statistiken zu Spalten in Verknüpfungen, in der WHERE-Klausel verwendete Spalten und Spalten in GROUP BY nutzen.

Siehe auch: [Verwalten von Tabellenstatistiken](develop-tables-statistics.md), [CREATE STATISTICS](/sql/t-sql/statements/create-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [UPDATE STATISTICS](/sql/t-sql/statements/update-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

### <a name="hash-distribute-large-tables"></a>Verwenden der Hashverteilung für große Tabellen

Für Tabellen wird standardmäßig die Roundrobin-Verteilung eingesetzt.  Dies erleichtert es Benutzern, mit dem Erstellen von Tabellen zu beginnen, ohne entscheiden zu müssen, wie die Tabellen verteilt werden sollen.  Roundrobin-Tabellen können für einige Workloads ausreichend leistungsfähig sein. In den meisten Fällen führt die Auswahl einer Verteilungsspalte jedoch zu einer besseren Leistung.  

Das häufigste Beispiel für den Fall, in dem eine von einer Spalte verteilte Tabelle eine deutlich höhere Leistung als eine Roundrobin-Tabelle aufweist, ist die Verknüpfung von zwei großen Faktentabellen.  

Wenn Sie z. B. eine Tabelle mit Bestellungen verwenden, die nach „order_id“ verteilt wird, und eine Tabelle mit Transaktionen, die ebenfalls nach „order_id“ verteilt wird, gilt Folgendes: Wenn Sie die Tabelle mit den Bestellungen per „order_id“ mit der Tabelle mit den Transaktionen verknüpfen, wird diese Abfrage zu einer Pass-Through-Abfrage. 

Das bedeutet, dass Datenverschiebungen beseitigt werden.  Weniger Schritte führen zu einer schnelleren Abfrage.  Außerdem führen weniger Datenverschiebungen zu schnelleren Abfragen.

> [!TIP]
> Achten Sie beim Laden einer verteilten Tabelle darauf, dass Ihre eingehenden Daten nicht nach dem Verteilungsschlüssel sortiert sind, da dies mit einer Verlangsamung Ihrer Ladevorgänge verbunden ist.  

Unter den folgenden Links finden Sie weitere Details dazu, wie die Auswahl einer Verteilungsspalte zu einer Leistungsbesserung führen kann, und wie Sie eine verteilte Tabelle in der WITH-Klausel Ihrer CREATE TABLES-Anweisung definieren.

Siehe auch [Übersicht über Tabellen](develop-tables-overview.md), [Tabellenverteilung](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [Auswählen der Tabellenverteilung](https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/), [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

### <a name="do-not-over-partition"></a>Vermeiden von übermäßiger Partitionierung
Das Partitionieren von Daten kann für die Verwaltung Ihrer Daten durch Partitionswechsel oder die Optimierung von Scans durch eine Partitionsbeseitigung zwar effektiv sein, aber eine zu hohe Zahl von Partitionen kann Ihre Abfragen verlangsamen.  Oft funktioniert eine Partitionierungsstrategie mit zu hoher Granularität, die für SQL Server gut geeignet ist, bei einem SQL-Pool nicht gut.  

> [!NOTE]
> Oft funktioniert eine Partitionierungsstrategie mit zu hoher Granularität, die für SQL Server gut geeignet ist, bei einem SQL-Pool nicht gut.  

Die Verwendung von zu vielen Partitionen kann auch die Effektivität von gruppierten Columnstore-Indizes reduzieren, wenn jede Partition weniger als 1 Million Zeilen enthält. Der SQL-Pool partitioniert Ihre Daten für Sie in 60 Datenbanken. 

Wenn Sie also eine Tabelle mit 100 Partitionen erstellen, wird das Ergebnis 6000 Partitionen sein.  Jede Workload ist anders, sodass es ratsam ist, mit der Partitionierung zu experimentieren. So können Sie ermitteln, was für Ihre Workload am besten funktioniert.  

Eine zu erwägende Option ist die Verwendung einer Granularität, die niedriger ist als die, die für Sie in SQL Server funktioniert haben könnte.  Versuchen Sie es beispielsweise anstelle von täglichen Partitionen mit wöchentlichen oder monatlichen Partitionen.

Siehe auch: [Tabellenpartitionierung](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="minimize-transaction-sizes"></a>Verringern von Transaktionsgrößen

INSERT-, UPDATE- und DELETE-Anweisungen werden in einer Transaktion ausgeführt, und wenn hierbei Fehler auftreten, muss ein Rollback durchgeführt werden.  Es ist ratsam, die Transaktionsgrößen nach Möglichkeit zu verringern, um das Potenzial für einen langen Rollbackvorgang zu reduzieren.  Hierfür können Sie INSERT-, UPDATE- und DELETE-Anweisungen in Teile unterteilen.  

Wenn Sie z. B. über eine INSERT-Anweisung verfügen, die voraussichtlich eine Stunde dauert, können Sie die INSERT-Anweisung in vier Teile unterteilen und so jede Ausführung auf 15 Minuten verkürzen.

> [!TIP]
> Nutzen Sie spezielle Fälle mit minimaler Protokollierung, z.B. CTAS, TRUNCATE, DROP TABLE oder INSERT zum Leeren von Tabellen, um das Rollback-Risiko zu verringern.  

Eine weitere Möglichkeit zum Beseitigen von Rollbacks ist die Verwendung von Vorgängen vom Typ „Nur Metadaten“, z.B. ein Partitionswechsel für die Datenverwaltung.  

Anstatt z.B. eine DELETE-Anweisung zum Löschen aller Zeilen in einer Tabelle auszuführen, für die das Bestelldatum (order_date) im Oktober 2001 liegt, können Sie Ihre Daten monatlich partitionieren und die Partition dann gegen Daten für eine leere Partition aus einer anderen Tabelle auswechseln (siehe Beispiele zu ALTER TABLE).  

Erwägen Sie für nicht partitionierte Tabellen die Verwendung eines CTAS-Vorgangs, um beizubehaltende Daten in eine Tabelle zu schreiben, anstatt DELETE zu verwenden.  Wenn für einen CTAS-Vorgang die gleiche Zeit benötigt wird, ist dies der sicherere Vorgang, da er mit minimaler Transaktionsprotokollierung verbunden ist und bei Bedarf schnell abgebrochen werden kann.

Weitere Informationen finden Sie unter [Verwenden von Transaktionen in SQL Data Warehouse](develop-transactions.md), [Optimieren von Transaktionen in SQL Analytics](../sql-data-warehouse/sql-data-warehouse-develop-best-practices-transactions.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [Tabellenpartitionierung](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [TRUNCATE TABLE](/sql/t-sql/statements/truncate-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) und [CREATE TABLE AS SELECT (CTAS)](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="use-the-smallest-possible-column-size"></a>Verwenden der kleinstmöglichen Spaltengröße

Beim Ihrer DDL verbessert sich die Abfrageleistung, wenn Sie den kleinsten Datentyp verwenden, der für Ihre Daten unterstützt wird.  Dies ist besonders für CHAR- und VARCHAR-Spalten wichtig.  

Wenn der längste Wert in einer Spalte 25 Zeichen lang ist, sollten Sie die Spalte VARCHAR(25) definieren.  Vermeiden Sie es, für alle Zeichenspalten eine hohe Standardlänge zu definieren.  Definieren Sie darüber hinaus Spalten als VARCHAR, sofern dies ausreicht, anstatt NVARCHAR zu verwenden.

Siehe auch: [Übersicht über Tabellen](develop-tables-overview.md), [Tabellendatentypen](develop-tables-data-types.md) und [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

### <a name="optimize-clustered-columnstore-tables"></a>Optimieren von gruppierten Columnstore-Tabellen

Gruppierte Columnstore-Indizes sind eine der effizientesten Möglichkeiten zum Speichern Ihrer Daten in einem SQL-Pool.  Tabellen werden in SQL-Pools standardmäßig als gruppierter Columnstore erstellt.  

Die Verwendung einer guten Segmentqualität ist wichtig, um für Abfragen in Columnstore-Tabellen die beste Leistung zu erzielen.  Wenn Zeilen bei hohem Arbeitsspeicherdruck in Columnstore-Tabellen geschrieben werden, kann die Qualität von Columnstore-Segmenten leiden.  

Die Segmentqualität kann anhand der Anzahl an Zeilen in einer komprimierten Zeilengruppe gemessen werden.  Eine ausführliche Anleitung zur Erkennung und Verbesserung der Segmentqualität für gruppierte Columnstore-Tabellen finden Sie in den Artikeln [Ursachen für eine schlechte Qualität des Columnstore-Index](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) unter [Tabellenindizes](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#causes-of-poor-columnstore-index-quality).  

Da qualitativ hochwertige Columnstore-Segmente wichtig sind, ist es eine gute Idee, Benutzer-IDs, die zur mittleren oder großen Ressourcenklasse gehören, zum Laden von Daten zu verwenden. Wenn Sie [Data Warehouse-Einheiten](resource-consumption-models.md) auf niedrigerer Ebene verwenden, sollten Sie dem ladenden Benutzer eine größere Ressourcenklasse zuweisen.

Da Columnstore-Tabellen Daten generell erst dann in ein komprimiertes Columnstore-Segment übertragen, wenn eine Tabelle mehr als eine Million Zeilen hat und jede SQL-Pool-Tabelle in 60 Tabellen partitioniert ist, empfiehlt sich Folgendes: Columnstore-Tabellen stellen für eine Abfrage nur dann einen Vorteil dar, wenn die Tabelle mehr als 60 Millionen Zeilen hat.  

> [!TIP]
> Bei Tabellen mit weniger als 60 Millionen Zeilen ist ein Columnstore-Index möglicherweise nicht die optimale Lösung.  

Wenn Sie Ihre Daten partitionieren, können Sie auch darauf achten, dass jede Partition 1 Million Zeilen aufweisen muss, um von einem gruppierten Columnstore-Index zu profitieren.  Falls eine Tabelle 100 Partitionen hat, muss sie mindestens 6 Milliarden Zeilen enthalten, um von einem gruppierten Columnstore zu profitieren (60 Verteilungen *100 Partitionen* 1 Million Zeilen).  

Falls Ihre Tabelle nicht 6 Milliarden Zeilen enthält, sollten Sie entweder die Anzahl der Partitionen reduzieren oder stattdessen eine Heaptabelle verwenden.  Außerdem kann sich folgendes Experiment lohnen: Ermitteln Sie, ob eine bessere Leistung erzielt werden kann, indem eine Heaptabelle mit sekundären Indizes anstelle einer Columnstore-Tabelle verwendet wird.

Beim Abfragen einer Columnstore-Tabelle werden die Abfragen schneller ausgeführt, wenn Sie nur die benötigten Spalten auswählen.  

Siehe auch: [Tabellenindizes](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [Beschreibung von Columnstore-Indizes](/sql/relational-databases/indexes/columnstore-indexes-overview?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [Neuerstellen von Columnstore-Indizes](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#rebuilding-indexes-to-improve-segment-quality).

## <a name="sql-on-demand-development-best-practices"></a>Bewährte Methoden für die SQL On-Demand-Entwicklung

### <a name="general-considerations"></a>Allgemeine Hinweise

SQL On-Demand ermöglicht es Ihnen, Dateien in Ihren Azure-Speicherkonten abzufragen. Es verfügt nicht über lokale Speicher- oder Erfassungsfunktionen, was bedeutet, dass alle Dateien, auf die die Abfrageziele ausgerichtet sind, extern zu SQL On-Demand sind. Daher kann sich alles, was mit dem Lesen von Dateien aus dem Speicher zusammenhängt, auf die Abfrageleistung auswirken.

### <a name="colocate-azure-storage-account-and-sql-on-demand"></a>Zusammenstellen von Azure-Speicherkonto und SQL On-Demand

Stellen Sie Ihr Azure-Speicherkonto und Ihren SQL-On-Demand-Endpunkt zusammen, um die Wartezeit zu minimieren. Speicherkonten und Endpunkte, die während der Einrichtung des Arbeitsbereichs bereitgestellt werden, befinden sich in derselben Region.

Wenn Sie mit SQL On-Demand auf andere Speicherkonten zugreifen, stellen Sie für eine optimale Leistung sicher, dass sich diese in derselben Region befinden. Andernfalls erhöht sich die Wartezeit für die Netzwerkübertragung der Daten von der Remoteregion zur Region des Endpunkts.

### <a name="azure-storage-throttling"></a>Azure Storage-Drosselung

Mehrere Anwendungen und Dienste können auf Ihr Speicherkonto zugreifen. Wenn der von Anwendungen, Diensten und SQL On-Demand-Workloads generierte kombinierte IOPS oder Durchsatz die Grenzen des Speicherkontos überschreitet, tritt die Speicherdrosselung auf. Wenn die Speicherdrosselung auftritt, wirkt sich dies erheblich negativ auf die Abfrageleistung aus.

Sobald eine Drosselung erkannt wird, verfügt SQL On-Demand über eine integrierte Behandlung dieses Szenarios. Bei SQL On-Demand werden Anforderungen an den Speicher langsamer ausgeführt, bis die Drosselung behoben ist. 

Für eine optimale Abfrageausführung wird jedoch empfohlen, das Speicherkonto während der Abfrageausführung nicht mit anderen Workloads zu belasten.

### <a name="prepare-files-for-querying"></a>Vorbereiten von Dateien für Abfragen

Wenn möglich, können Sie Dateien für eine bessere Leistung vorbereiten:

- Konvertieren Sie das CSV- in das Parquet-Format – Parquet ist ein Spaltenformat. Da es komprimiert ist, weist es kleinere Dateigrößen als CSV-Dateien mit denselben Daten auf, und SQL On-Demand benötigt weniger Zeit und Speicheranforderungen, um es zu lesen.
- Wenn eine Abfrage auf eine einzelne große Datei ausgerichtet ist, lohnt es sich, diese in mehrere kleinere Dateien aufzuteilen.
- Versuchen Sie, die Größe Ihrer CSV-Datei unter 10 GB zu halten.
- Es wird bevorzugt, gleich große Dateien für einen einzelnen OPENROWSET-Pfad oder einen Speicherort für die Tabelle zu verwenden.
- Partitionieren Sie Ihre Daten, indem Sie Partitionen in verschiedenen Ordnern oder Dateinamen speichern. Lesen Sie [Verwenden von Dateinamen- und Dateipfadfunktionen zum Ausrichten auf bestimmte Partitionen](#use-fileinfo-and-filepath-functions-to-target-specific-partitions).

### <a name="use-fileinfo-and-filepath-functions-to-target-specific-partitions"></a>Verwenden von Dateiinfo- und Dateipfadfunktionen zum Ausrichten auf bestimmte Partitionen

Daten sind oft in Partitionen organisiert. Sie können SQL On-Demand anweisen, bestimmte Ordner und Dateien abzufragen. Dies reduziert die Anzahl der Dateien und die Datenmenge, die die Abfrage zum Lesen und Verarbeiten benötigt. 

Folglich erzielen Sie eine bessere Leistung. Weitere Informationen finden Sie unter [Dateinamen](develop-storage-files-overview.md#filename-function)- und [Dateipfad](develop-storage-files-overview.md#filepath-function)-Funktionen und in den Beispielen für das [Abfragen bestimmter Dateien](query-specific-files.md).

Wenn Ihre gespeicherten Daten nicht partitioniert sind, sollten Sie eine Partitionierung in Erwägung ziehen, damit Sie diese Funktionen verwenden können, um Abfragen zu optimieren, die auf diese Dateien ausgerichtet sind.

Bei der [Abfrage partitionierter externer Apache Spark für Azure Synapse-Tabellen](develop-storage-files-spark-tables.md) in SQL On-Demand ist die Abfrage automatisch nur auf die erforderlichen Dateien ausgerichtet.

### <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Verwenden von CETAS zum Verbessern von Abfrageleistung und Verknüpfungen

[CETAS](develop-tables-cetas.md) ist eine der wichtigsten Funktionen, die in SQL On-Demand verfügbar sind. CETAS ist ein paralleler Vorgang, der externe Tabellenmetadaten erstellt und die Ergebnisse der SELECT-Abfrage in eine Reihe von Dateien in Ihrem Speicherkonto exportiert.

Sie können CETAS verwenden, um häufig verwendete Teile von Abfragen, z. B. verknüpfte Verweistabellen, zu einem neuen Satz von Dateien zu speichern. Später können Sie eine Verknüpfung zu dieser einzelnen externen Tabelle herstellen, anstatt gemeinsame Verknüpfungen in mehreren Abfragen zu wiederholen. 

Da CETAS Parquet-Dateien generiert, werden Statistiken automatisch erstellt, wenn die erste Abfrage auf diese externe Tabelle ausgerichtet ist, was zu einer verbesserten Leistung führt.

### <a name="next-steps"></a>Nächste Schritte

Wenn Sie Informationen benötigen, die in diesem Artikel nicht enthalten sind, verwenden Sie die „Nach Dokumenten suchen“ auf der linken Seite dieser Seite, um alle SQL-Pooldokumente zu durchsuchen.  Auf der [Frageseite von Microsoft Q&A (Fragen und Antworten) für SQL-Pool ](https://docs.microsoft.com/answers/topics/azure-synapse-analytics.html) können Sie anderen Benutzern und der Produktgruppe „SQL-Pool“ Fragen stellen.  

Wir überwachen dieses Forum aktiv, um sicherzustellen, dass Ihre Frage entweder von einem anderen Benutzer oder einem Mitarbeiter beantwortet wird.  Falls Sie Ihre Fragen lieber über Stack Overflow stellen möchten, können Sie dazu auch das [Stack Overflow-Forum für Azure SQL-Pool](https://stackoverflow.com/questions/tagged/azure-sqldw) nutzen.
 