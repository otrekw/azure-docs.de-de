---
title: Entwerfen von ELT anstelle von ETL
description: Implementieren von flexiblen Datenladestrategien für Synapse SQL-Pools in Azure Synapse Analytics
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/19/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: e99fd898956e11a4827d023691111a47e5a790c0
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80744959"
---
# <a name="data-loading-strategies-for-synapse-sql-pool"></a>Datenladestrategien für Synapse SQL-Pools

In herkömmlichen SMP-SQL-Pools wird zum Laden von Daten ein ETL-Prozess (Extrahieren, Transformieren und Laden) verwendet. Synapse SQL-Pools in Azure Synapse Analytics haben eine Architektur mit massiver Parallelverarbeitung (Massive Parallel Processing, MPP), die die Vorteile der Skalierbarkeit und Flexibilität von Compute- und Speicherressourcen nutzt.

Ein ELT-Prozess (Extrahieren, Laden und Transformieren) nutzt die Vorteile von MPP und verringert die Ressourcen, die vor dem Laden zum Transformieren der Daten erforderlich sind.

SQL-Pools unterstützen zwar viele Lademethoden (unter anderem beliebte SQL Server-Optionen wie [bcp](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) und die [SqlBulkCopy-API](/dotnet/api/system.data.sqlclient.sqlbulkcopy?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)), doch die schnellste und am besten skalierbare Möglichkeit zum Laden von Daten stellen externe PolyBase-Tabellen und die [COPY-Anweisung](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) (Vorschauversion) dar.

Mit PolyBase und der COPY-Anweisung können Sie über die T-SQL-Sprache auf externe Daten zugreifen, die in Azure Blob Storage oder in Azure Data Lake Storage gespeichert sind. Wir empfehlen die Verwendung der COPY-Anweisung, um beim Laden von Daten so flexibel wie möglich zu sein.

> [!NOTE]  
> Die COPY-Anweisung befindet sich zurzeit in der Public Preview. Wenn Sie Feedback abgeben möchten, senden Sie eine E-Mail an die folgende Verteilerliste: sqldwcopypreview@service.microsoft.com.

> [!VIDEO https://www.youtube.com/embed/l9-wP7OdhDk]

## <a name="what-is-elt"></a>Was ist ELT?

ELT (Extrahieren, Laden und Transformieren) ist ein Prozess, bei dem Daten aus einem Quellsystem extrahiert, in einen SQL-Pool geladen und anschließend transformiert werden.

Dies sind die grundlegenden Schritte für die Implementierung von ELT:

1. Extrahieren Sie die Quelldaten in Textdateien.
2. Legen Sie die Daten in Azure Blob Storage oder Azure Data Lake Store ab.
3. Bereiten Sie die Daten für das Laden vor.
4. Laden Sie die Daten mithilfe von PolyBase oder des COPY-Befehls in Stagingtabellen.
5. Transformieren Sie die Daten.
6. Fügen Sie die Daten in Produktionstabellen ein.

Ein Tutorial zum PolyBase-Ladevorgang finden Sie unter [Verwenden von PolyBase zum Laden von Daten aus Azure Blob Storage](load-data-from-azure-blob-storage-using-polybase.md).

Weitere Informationen finden Sie im Blog [Lademuster](https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/).

## <a name="1-extract-the-source-data-into-text-files"></a>1. Extrahieren der Quelldaten in Textdateien

Das Abrufen von Daten aus dem Quellsystem hängt vom Speicherort ab.  Das Ziel besteht darin, die Daten in durch Trennzeichen getrennte Text- oder CSV-Dateien zu verschieben, die von PolyBase und der COPY-Anweisung unterstützt werden.

### <a name="polybase-and-copy-external-file-formats"></a>Externe PolyBase- und COPY-Dateiformate

Mit PolyBase und der COPY-Anweisung können Sie Daten aus UTF-8- und UTF-16-codierten, durch Trennzeichen getrennten Text- oder CSV-Dateien laden. Neben durch Trennzeichen getrennten Text- oder CSV-Dateien können auch Daten auch aus Hadoop-Dateiformaten wie ORC oder Parquet geladen werden. Mit PolyBase und der COPY-Anweisung können auch Daten aus Dateien geladen werden, die mit Gzip und Snappy komprimiert wurden.

Erweitertes ASCII, Formate mit fester Breite und geschachtelte Formate wie WinZip oder XML werden nicht unterstützt. Beim Exportieren aus SQL Server können Sie die Daten mit dem [Befehlszeilentool bcp](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) in durch Trennzeichen getrennte Textdateien exportieren.

## <a name="2-land-the-data-into-azure-blob-storage-or-azure-data-lake-store"></a>2. Laden der Daten in Azure Blob Storage oder Azure Data Lake Storage

Wenn Sie Daten in Azure Storage platzieren möchten, können Sie sie in [Azure Blob Storage](../../storage/blobs/storage-blobs-introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) oder in [Azure Data Lake Store Gen2](../../data-lake-store/data-lake-store-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) verschieben. In beiden Fällen sollten die Daten in Textdateien gespeichert werden. PolyBase und die COPY-Anweisung können Daten von beiden Orten laden.

Tools und Dienste, mit denen Sie Daten in Azure Storage verschieben können:

- Der [Azure ExpressRoute](../../expressroute/expressroute-introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)-Dienst verbessert Netzwerkdurchsatz, Leistung und Vorhersagbarkeit. ExpressRoute ist ein Dienst, der Ihre Daten über eine dedizierte private Verbindung zu Azure weiterleitet. Bei ExpressRoute-Verbindungen werden Daten nicht über das öffentliche Internet weitergeleitet. Die Verbindungen bieten mehr Zuverlässigkeit, eine höhere Geschwindigkeit, niedrigere Latenzzeiten und mehr Sicherheit als herkömmliche Verbindungen über das öffentliche Internet.
- Das Hilfsprogramm [AZCopy](../../storage/common/storage-choose-data-transfer-solution.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) verschiebt Daten über das öffentliche Internet in Azure Storage. Dies funktioniert, wenn Ihre Datenmengen weniger als 10 TB umfassen. Wenn Sie Ladevorgänge in regelmäßigen Abständen mit AZCopy ausführen möchten, testen Sie die Netzwerkgeschwindigkeit, um festzustellen, ob sie geeignet ist.
- [Azure Data Factory (ADF)](../../data-factory/introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) verfügt über ein Gateway, das Sie auf dem lokalen Server installieren können. Anschließend können Sie eine Pipeline erstellen, um Daten vom lokalen Server in Azure Storage zu verschieben. Informationen zur Verwendung von Data Factory bei SQL Analytics finden Sie unter [Laden von Daten für SQL Analytics](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

## <a name="3-prepare-the-data-for-loading"></a>3. Vorbereiten der Daten für das Laden

Möglicherweise müssen Sie die Daten in Ihrem Speicherkonto vorbereiten und bereinigen, bevor Sie den Ladevorgang durchführen. Die Datenvorbereitung kann durchgeführt werden, während sich Ihre Daten in der Quelle befinden, während Sie die Daten in Textdateien exportieren oder nachdem sich die Daten in Azure Storage befinden.  Am einfachsten ist es, so früh wie möglich im Prozess mit den Daten zu arbeiten.  

### <a name="define-external-tables"></a>Definieren externer Tabellen

Bei Verwendung von PolyBase müssen Sie vor dem Laden externe Tabellen in Ihrem SQL-Pool definieren. Bei Verwendung der COPY-Anweisung sind keine externen Tabellen erforderlich. PolyBase verwendet externe Tabellen, um Daten in Azure Storage zu definieren und auf diese zuzugreifen.

Eine externe Tabelle ähnelt einer Datenbanksicht. Die externe Tabelle enthält das Tabellenschema und verweist auf Daten, die außerhalb des SQL-Pools gespeichert sind.

Die Definition externer Tabellen umfasst die Angabe der Datenquelle, des Formats der Textdateien und der Tabellendefinitionen. Die folgenden Referenzartikel zur T-SQL-Syntax benötigen Sie:

- [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [CREATE EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

Beim Laden von Parquet ist dies die SQL-Datentypzuordnung:

| **Parquet-Datentyp** | **SQL-Datentyp** |
| :-------------------: | :---------------: |
|        TINYINT        |      TINYINT      |
|       SMALLINT        |     SMALLINT      |
|          INT          |        INT        |
|        BIGINT         |      BIGINT       |
|        boolean        |        bit        |
|        double         |       float       |
|         float         |       real        |
|        double         |       money       |
|        double         |    SMALLMONEY     |
|        Zeichenfolge         |       NCHAR       |
|        Zeichenfolge         |     NVARCHAR      |
|        Zeichenfolge         |       char        |
|        Zeichenfolge         |      varchar      |
|        BINARY         |      BINARY       |
|        BINARY         |     varbinary     |
|       timestamp       |       date        |
|       timestamp       |   smalldatetime   |
|       timestamp       |     datetime2     |
|       timestamp       |     datetime      |
|       timestamp       |       time        |
|         date          |       date        |
|        Decimal        |      Decimal      |

Ein Beispiel für die Erstellung externer Objekte finden Sie im Schritt [Erstellen externer Tabellen](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) im Tutorial zu Ladevorgängen.

### <a name="format-text-files"></a>Formatieren von Textdateien

Bei Verwendung von PolyBase müssen die definierten externen Objekte die Zeilen der Textdateien mit der externen Tabelle und der Definition des Dateiformats abgleichen. Die Daten in den einzelnen Zeilen der Textdatei müssen mit der Tabellendefinition übereinstimmen.
So formatieren Sie die Textdateien

- Wenn Ihre Daten aus einer nicht relationalen Quelle stammen, müssen Sie sie in Zeilen und Spalten transformieren. Unabhängig davon, ob die Daten aus einer relationalen oder nicht relationalen Quelle stammen, müssen die Daten so transformiert werden, dass sie mit den Spaltendefinitionen der Tabelle übereinstimmen, in die die Daten geladen werden sollen.
- Formatieren Sie die Daten in der Textdatei so, dass sie mit den Spalten und Datentypen in der Zieltabelle übereinstimmen. Eine Nichtübereinstimmung zwischen den Datentypen in den externen Textdateien und der SQL-Pooltabelle führt dazu, dass Zeilen beim Laden zurückgewiesen werden.
- Trennen Sie Felder in der Textdatei mit einem Abschlusszeichen.  Stellen Sie sicher, dass Sie ein Zeichen oder eine Zeichenfolge verwenden, die nicht in Ihren Quelldaten enthalten ist. Verwenden Sie das durch [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) angegebene Abschlusszeichen.

## <a name="4-load-the-data-using-polybase-or-the-copy-statement"></a>4. Laden der Daten mithilfe von PolyBase oder der COPY-Anweisung

Es hat sich bewährt, die Daten in eine Stagingtabelle zu laden. Stagingtabellen ermöglichen das Behandeln von Fehlern, ohne die Produktionstabellen zu beeinträchtigen. Eine Stagingtabelle bietet Ihnen außerdem die Möglichkeit, Datentransformationen mit SQL Pool-MPP durchzuführen, bevor die Daten in Produktionstabellen eingefügt werden.

Die Tabelle muss vorab erstellt werden, wenn Daten mithilfe von COPY in eine Stagingtabelle geladen werden.

### <a name="options-for-loading-with-polybase-and-copy-statement"></a>Optionen für das Laden per PolyBase und COPY-Anweisung

Um Daten mit PolyBase zu laden, können Sie eine der folgenden Ladeoptionen nutzen:

- [PolyBase mit T-SQL](load-data-from-azure-blob-storage-using-polybase.md) funktioniert gut, wenn sich Ihre Daten in Azure Blob Storage oder Azure Data Lake Store befinden. Es bietet Ihnen die größtmögliche Kontrolle über den Ladevorgang, erfordert aber auch die Definition externer Datenobjekte. Die anderen Methoden definieren diese Objekte im Hintergrund, wenn Sie Quelltabellen zu Zieltabellen zuordnen.  Zum Orchestrieren von T-SQL-Ladevorgängen können Sie Azure Data Factory-, SSIS- oder Azure-Funktionen verwenden.
- [PolyBase mit SSIS](/sql/integration-services/load-data-to-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) funktioniert gut, wenn sich die Quelldaten in SQL Server befinden (entweder lokal in SQL Server oder in der Cloud). SSIS definiert die Zuordnung von Quell- zu Zieltabellen und orchestriert zudem die Workload. Wenn Sie bereits über SSIS-Pakete verfügen, können Sie die Pakete so ändern, dass sie mit dem neuen Data Warehouse-Ziel funktionieren.
- [PolyBase und COPY-Anweisung mit Azure Data Factory (ADF)](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) ist ein weiteres Orchestrierungstool.  Es definiert eine Pipeline und plant Aufträge.
- [PolyBase mit Azure Databricks](../../azure-databricks/databricks-extract-load-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) überträgt Daten aus einer Tabelle in einen Databricks-Datenrahmen und/oder schreibt Daten aus einem Databricks-Datenrahmen in eine SQL Data Warehouse-Tabelle, die PolyBase verwendet.

### <a name="other-loading-options"></a>Weitere Ladeoptionen

Neben PolyBase und der COPY-Anweisung können Sie auch [bcp](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) oder die [SqlBulkCopy-API](/dotnet/api/system.data.sqlclient.sqlbulkcopy?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) verwenden. Mit „bcp“ werden Daten direkt in die Datenbank geladen, ohne Azure Blob Storage zu durchlaufen. Daher ist es nur für kleine Workloads konzipiert.

> [!NOTE]
> Die Ladeleistung dieser Optionen ist geringer als bei PolyBase und der COPY-Anweisung.

## <a name="5-transform-the-data"></a>5. Transformieren der Daten

Führen Sie während der Bereitstellung der Daten in der Stagingtabelle entsprechende Transformationen durch, die für Ihre Workload erforderlich sind. Anschließend verschieben Sie die Daten in eine Produktionstabelle.

## <a name="6-insert-the-data-into-production-tables"></a>6. Einfügen der Daten in Produktionstabellen

Die SELECT-Anweisung „INSERT INTO...“ verschiebt die Daten aus der Stagingtabelle in die permanente Tabelle.

Versuchen Sie beim Entwerfen eines ETL-Prozesses, den Prozess für ein kleines Testbeispiel auszuführen. Versuchen Sie, 1000 Zeilen aus der Tabelle in eine Datei zu extrahieren, sie nach Azure zu verschieben und sie dann in eine Stagingtabelle zu laden.

## <a name="partner-loading-solutions"></a>Ladelösungen von Partnern

Viele unserer Partner stellen Ladelösungen bereit. Weitere Informationen finden Sie in der Liste mit unseren [Lösungspartnern](sql-data-warehouse-partner-business-intelligence.md).

## <a name="next-steps"></a>Nächste Schritte

Eine Anleitung zum Laden finden Sie unter [Guidance for loading data into Azure SQL Data Warehouse](guidance-for-loading-data.md) (Anleitung zum Laden von Daten in Azure SQL Data Warehouse).
