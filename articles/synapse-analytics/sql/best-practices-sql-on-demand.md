---
title: Best Practices für SQL On-Demand (Vorschauversion)
description: In diesem Artikel finden Sie Empfehlungen und Best Practices, die Sie bei der Arbeit mit SQL On-Demand (Vorschauversion) kennen sollten.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 05/01/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 79318ab67ec58ed10520365a366785ea0de41666
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836327"
---
# <a name="best-practices-for-sql-on-demand-preview-in-azure-synapse-analytics"></a>Bewährte Methoden für SQL On-Demand (Vorschauversion) in Azure Synapse Analytics

In diesem Artikel finden Sie eine Sammlung von bewährten Methoden für die Verwendung von SQL On-Demand (Vorschauversion). SQL On-Demand ist eine Ressource in Azure Synapse Analytics.

## <a name="general-considerations"></a>Allgemeine Hinweise

SQL On-Demand ermöglicht es Ihnen, Dateien in Ihren Azure-Speicherkonten abzufragen. Es verfügt nicht über lokale Speicher- oder Erfassungsfunktionen. Somit sind alle Dateien, auf die die Abfrage ausgerichtet ist, extern zu SQL On-Demand. Alles, was mit dem Lesen von Dateien aus dem Speicher zusammenhängt, kann sich auf die Abfrageleistung auswirken.

## <a name="colocate-your-azure-storage-account-and-sql-on-demand"></a>Zusammenstellen Ihres Azure-Speicherkontos und SQL On-Demand

Stellen Sie Ihr Azure-Speicherkonto und Ihren SQL-On-Demand-Endpunkt zusammen, um die Wartezeit zu minimieren. Speicherkonten und Endpunkte, die während der Einrichtung des Arbeitsbereichs bereitgestellt werden, befinden sich in derselben Region.

Wenn Sie mit SQL On-Demand auf andere Speicherkonten zugreifen, stellen Sie für eine optimale Leistung sicher, dass sich diese in derselben Region befinden. Wenn sie sich nicht in derselben Region befinden, erhöht sich die Wartezeit für die Netzwerkübertragung der Daten zwischen der Remote- und der Endpunktregion.

## <a name="azure-storage-throttling"></a>Azure Storage-Drosselung

Mehrere Anwendungen und Dienste können auf Ihr Speicherkonto zugreifen. Die Speicherdrosselung tritt auf, wenn der von Anwendungen, Diensten und SQL On-Demand-Workloads generierte kombinierte IOPS oder Durchsatz die Grenzen des Speicherkontos überschreitet. Als Folge davon werden Sie eine erhebliche negative Auswirkung auf die Abfrageleistung erleben.

Sobald eine Drosselung erkannt wird, verfügt SQL On-Demand über eine integrierte Behandlung dieses Szenarios. Bei SQL On-Demand werden Anforderungen an den Speicher langsamer ausgeführt, bis die Drosselung behoben ist.

> [!TIP]
> Für eine optimale Abfrageausführung sollten Sie das Speicherkonto während der Abfrageausführung nicht mit anderen Workloads belasten.

## <a name="prepare-files-for-querying"></a>Vorbereiten von Dateien für Abfragen

Wenn möglich, können Sie Dateien für eine bessere Leistung vorbereiten:

- Konvertieren Sie das CSV- und das JSON-Format in Parquet. Bei Parquet handelt es sich um ein Spaltenformat. Da es komprimiert ist, sind die Dateien kleiner als CSV- oder JSON-Dateien mit denselben Daten. SQL On-Demand benötigt weniger Zeit und Speicheranforderungen, um es zu lesen.
- Wenn eine Abfrage auf eine einzelne große Datei ausgerichtet ist, lohnt es sich, diese in mehrere kleinere Dateien aufzuteilen.
- Versuchen Sie, die Größe Ihrer CSV-Datei unter 10 GB zu halten.
- Es ist besser, gleich große Dateien für einen einzelnen OPENROWSET-Pfad oder einen Speicherort für die Tabelle zu verwenden.
- Partitionieren Sie Ihre Daten, indem Sie Partitionen in verschiedenen Ordnern oder unter unterschiedlichen Dateinamen speichern. Unter [Verwenden von Dateiname und Dateipfadfunktionen zum Ausrichten auf bestimmte Partitionen](#use-filename-and-filepath-functions-to-target-specific-partitions) finden Sie weitere Informationen.

## <a name="push-wildcards-to-lower-levels-in-the-path"></a>Pushen von Platzhaltern auf niedrigere Ebenen im Pfad

Sie können Platzhalter in Ihrem Pfad verwenden, um [mehrere Dateien und Ordner](develop-storage-files-overview.md#query-multiple-files-or-folders) abzufragen. SQL On-Demand listet Dateien in Ihrem Speicherkonto auf und beginnt dabei beim ersten *. Dabei kommt die Storage-API zum Einsatz. Dateien, die nicht mit dem angegebenen Pfad übereinstimmen, werden dabei ausgeschlossen. Das Verringern der ursprünglichen Liste mit Dateien kann die Leistung verbessern, wenn viele Dateien vorhanden sind, die mit dem angegebenen Pfad bis zum ersten Platzhalter übereinstimmen.

## <a name="use-appropriate-data-types"></a>Verwenden geeigneter Datentypen

Die Datentypen, die Sie in Ihrer Abfrage verwenden, wirken sich auf die Leistung aus. Sie können mit folgenden Maßnahmen eine bessere Leistung erzielen: 

- Verwenden der kleinsten Datengröße, die den größtmöglichen Wert aufnehmen kann.
  - Wenn die maximale Zeichenlänge des Werts 30 Zeichen beträgt, verwenden Sie den Zeichendatentyp der Länge 30.
  - Wenn Zeichenspaltenwerte eine festgelegte Größe haben, verwenden Sie **char** oder **nchar**. Verwenden Sie andernfalls **varchar** oder **nvarchar**.
  - Wenn der Höchstwert für eine ganzzahlige (integer) Spalte 500 ist, verwenden Sie **smallint**, da dies der kleinste Datentyp ist, der diesen Wert aufnehmen kann. Ganzzahlige Datentypen (integer) finden Sie [hier](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql?view=sql-server-ver15).
- Verwenden Sie nach Möglichkeit **varchar** und **char** anstelle von **nvarchar** und **nchar**.
- Verwenden Sie nach Möglichkeit ganzzahlige Datentypen (integer). Die Vorgänge SORT, JOIN und GROUP BY erfolgen schneller mit ganzzahligen Daten als mit Zeichendaten.
- Wenn Sie Schemarückschlüsse verwenden, [überprüfen Sie den abgeleiteten Datentyp](#check-inferred-data-types).

## <a name="check-inferred-data-types"></a>Abgeleitete Datentypen überprüfen

Mithilfe von [Schemarückschlüssen](query-parquet-files.md#automatic-schema-inference) können Sie schnell Abfragen schreiben und Daten erkunden, ohne das Dateischema zu kennen. Diese Vorteile haben allerdings auch einen Nachteil, der darin besteht, dass abgeleitete Datentypen größer als die tatsächlichen Datentypen sind. Hierzu kommt es, wenn nicht genügend Informationen in den Quelldateien vorhanden sind, um sicherzustellen, dass geeignete Datentypen verwendet werden. Beispielsweise enthalten Parquet-Dateien keine Metadaten zur maximalen Länge von Zeichenspalten. SQL On-Demand leitet diese als „varchar(8000)“ ab.

Sie können die resultierenden Datentypen Ihrer Abfrage mithilfe von [sp_describe_first_results_set](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql?view=sql-server-ver15) überprüfen.

Im folgenden Beispiel wird gezeigt, wie Sie abgeleitete Datentypen optimieren können. Die Prozedur wird zum Anzeigen von abgeleiteten Datentypen verwendet: 
```sql  
EXEC sp_describe_first_result_set N'
    SELECT
        vendor_id, pickup_datetime, passenger_count
    FROM 
        OPENROWSET(
            BULK ''https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/*/*/*'',
            FORMAT=''PARQUET''
        ) AS nyc';
```

Das Ergebnis lautet wie folgt:

|is_hidden|column_ordinal|name|system_type_name|max_length|
|----------------|---------------------|----------|--------------------|-------------------||
|0|1|vendor_id|varchar(8000)|8\.000|
|0|2|pickup_datetime|datetime2(7)|8|
|0|3|passenger_count|INT|4|

Sobald Sie die abgeleiteten Datentypen für die Abfrage kennen, können Sie geeignete Datentypen angeben:

```sql  
SELECT
    vendor_id, pickup_datetime, passenger_count
FROM 
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/*/*/*',
        FORMAT='PARQUET'
    ) 
    WITH (
        vendor_id varchar(4), -- we used length of 4 instead of the inferred 8000
        pickup_datetime datetime2,
        passenger_count int
    ) AS nyc;
```

## <a name="use-filename-and-filepath-functions-to-target-specific-partitions"></a>Verwenden von Dateiname und Dateipfadfunktionen zum Ausrichten auf bestimmte Partitionen

Daten sind oft in Partitionen organisiert. Sie können SQL On-Demand anweisen, bestimmte Ordner und Dateien abzufragen. Dies reduziert die Anzahl der Dateien und die Datenmenge, die die Abfrage zum Lesen und Verarbeiten benötigt. Ein weiterer Vorteil ist, dass Sie bessere Leistungen erzielen.

Im Artikel [Verwenden von Dateimetadaten in Abfragen](query-specific-files.md) finden Sie Beispiele für das Abfragen bestimmter Dateien sowie weitere Informationen zu [Dateiname](develop-storage-files-overview.md#filename-function) und [Dateipfad](develop-storage-files-overview.md#filepath-function).

> [!TIP]
> Wandeln Sie das Ergebnis der Dateipfad- und Dateinamefunktionen immer in einen geeigneten Datentyp um. Achten Sie, wenn Sie Zeichendatentypen verwenden, unbedingt auf die Verwendung der richtigen Länge.

> [!NOTE]
> Funktionen zur Partitionsentfernung (Dateipfad und Dateiname) werden derzeit nicht für externe Tabellen unterstützt, außer denen, die automatisch für jede in Apache Spark für Azure Synapse Analytics erstellte Tabelle erzeugt werden.

Wenn Ihre gespeicherten Daten nicht partitioniert sind, können Sie sie partitionieren. So können Sie diese Funktionen verwenden, um auf diese Dateien ausgerichtete Abfragen zu optimieren. Wenn Sie [partitionierte Apache Spark für Azure Synapse-Tabellen](develop-storage-files-spark-tables.md) in SQL On-Demand abfragen, verwendet die Abfrage automatisch die erforderlichen Dateien als Ziel.

## <a name="use-parser_version-20-to-query-csv-files"></a>Verwenden von PARSER_VERSION 2.0 zum Abfragen von CSV-Dateien

Beim Abfragen von CSV-Dateien können Sie einen leistungsoptimierten Parser verwenden. Weitere Informationen finden Sie unter [PARSER_VERSION](develop-openrowset.md).

## <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Verwenden von CETAS zum Verbessern von Abfrageleistung und Verknüpfungen

[CETAS](develop-tables-cetas.md) ist eine der wichtigsten Funktionen, die in SQL On-Demand verfügbar sind. CETAS ist ein paralleler Vorgang, der externe Tabellenmetadaten erstellt und die Ergebnisse der SELECT-Abfrage in eine Reihe von Dateien in Ihrem Speicherkonto exportiert.

Sie können CETAS verwenden, um häufig verwendete Teile von Abfragen, z. B. verknüpfte Verweistabellen, zu einem neuen Satz von Dateien zu speichern. Als Nächstes können Sie eine Verknüpfung zu dieser einzelnen externen Tabelle herstellen, anstatt gemeinsame Verknüpfungen in mehreren Abfragen zu wiederholen.

Da CETAS Parquet-Dateien generiert, werden Statistiken automatisch erstellt, wenn die erste Abfrage auf diese externe Tabelle ausgerichtet ist, was zu einer verbesserten Leistung führt.

## <a name="azure-ad-pass-through-performance"></a>Leistung von Azure AD-Pass-Through

SQL On-Demand ermöglicht Ihnen den Zugriff auf Dateien im Speicher über Azure Active Directory-Pass-Through (Azure AD) oder SAS-Anmeldeinformationen. Die Leistung von Azure AD-Pass-Through kann im Vergleich zu SAS geringer sein.

Wenn Sie eine bessere Leistung benötigen, verwenden Sie SAS-Anmeldeinformationen für den Speicherzugriff, bis die Leistung von Azure AD-Pass-Through verbessert wurde.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu allgemeinen Problemen und entsprechende Lösungen finden Sie im Artikel [Problembehandlung bei SQL Analytics in Azure Synapse](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json). Wenn Sie eher mit SQL-Pool als mit SQL On-Demand arbeiten, finden Sie spezifische Anleitungen im Artikel [Bewährte Methoden für SQL-Pools in Azure Synapse Analytics](best-practices-sql-pool.md).
