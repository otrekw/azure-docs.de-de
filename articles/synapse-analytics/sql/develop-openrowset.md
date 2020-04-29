---
title: Verwenden von OPENROWSET in SQL On-Demand (Vorschauversion)
description: In diesem Artikel wird die Syntax von OPENROWSET in SQL On-Demand (Vorschauversion) beschrieben und die Verwendung von Argumenten erläutert.
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 6325d5555b01373b148dce69731ec64896d6e1fd
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680491"
---
# <a name="how-to-use-openrowset-with-sql-on-demand-preview"></a>Verwenden von OPENROWSET mit SQL On-Demand (Vorschauversion)

Die OPENROWSET(BULK...)-Funktion ermöglicht den Zugriff auf Dateien in Azure Storage. Innerhalb der SQL On-Demand-Ressource (Vorschauversion) wird die OPENROWSET-Funktion aufgerufen und die BULK-Option angegeben, um auf den OPENROWSET-Massenrowsetanbieter zuzugreifen.  

Auf die OPENROWSET-Funktion kann in der FROM-Klausel einer Abfrage wie auf eine Tabelle namens OPENROWSET verwiesen werden. Sie unterstützt Massenvorgänge über einen integrierten BULK-Anbieter, mit dem Daten aus einer Datei gelesen und als Rowset zurückgegeben werden können.

OPENROWSET wird derzeit in SQL-Pools nicht unterstützt.

## <a name="syntax"></a>Syntax

```syntaxsql
--OPENROWSET syntax for reading Parquet files
OPENROWSET  
( { BULK 'unstructured_data_path' , 
    FORMAT='PARQUET' }  
)  
[WITH ( {'column_name' 'column_type' }) ]
[AS] table_alias(column_alias,...n)

--OPENROWSET syntax for reading delimited text files
OPENROWSET  
( { BULK 'unstructured_data_path' , 
    FORMAT = 'CSV'
    [ <bulk_options> ] }  
)  
WITH ( {'column_name' 'column_type' [ 'column_ordinal'] })  
[AS] table_alias(column_alias,...n)
 
<bulk_options> ::=  
[ , FIELDTERMINATOR = 'char' ]    
[ , ROWTERMINATOR = 'char' ] 
[ , ESCAPE_CHAR = 'char' ] 
[ , FIRSTROW = 'first_row'  ]     
[ , FIELDQUOTE = 'quote_characters']
```

## <a name="arguments"></a>Argumente

Bei den Eingabedateien mit den Zieldaten für die Abfrage stehen zwei Optionen zur Auswahl. Gültige Werte sind:

- 'CSV': Eine beliebige, durch Trennzeichen getrennte Textdatei mit Zeilen-/Spaltentrennzeichen. Als Feldtrennzeichen kann ein beliebiges Zeichen verwendet werden. Für durch Tabstopp getrennte Werte wäre dies beispielsweise: FIELDTERMINATOR = tab.

- 'PARQUET': Binärdatei im Parquet-Format. 

**'unstructured_data_path'**

Der Pfad für unstrukturierte Daten hat das folgende Format:  
\<Präfix>://\<Speicherkontopfad>/\<Speicherpfad>.
 
 
 Im Anschluss finden Sie die relevanten Speicherkontopfade für die Verknüpfung mit Ihrer speziellen externen Datenquelle: 

| Externe Datenquelle       | Präfix | Speicherkontopfad                                 |
| -------------------------- | ------ | ---------------------------------------------------- |
| Azure Blob Storage         | https  | \<Speicherkonto>.blob.core.windows.net             |
| Azure Data Lake Store Gen1 | https  | \<Speicherkonto>.azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Store Gen2 | https  | \<Speicherkonto>.dfs.core.windows.net              |
||||

'\<storage_path>'

 Dient zum Angeben eines Pfads innerhalb Ihres Speichers, der auf den zu lesenden Ordner oder auf die zu lesende Datei verweist. Verweist der Pfad auf einen Container oder Ordner, werden alle Dateien aus diesem Container oder Ordner gelesen. Dateien in Unterordnern werden nicht einbezogen. 
 
 Sie können Platzhalter angeben, um mehrere Dateien oder Ordner zu verwenden. Dabei können mehrere nicht aufeinander folgende Platzhalter verwendet werden.
Im folgenden Beispiel werden alle mit *population* beginnenden *CSV-Dateien* aus allen mit */csv/population* beginnenden Ordnern gelesen:  
`https://sqlondemandstorage.blob.core.windows.net/csv/population*/population*.csv`

Wenn Sie den Pfad für unstrukturierte Daten (unstructured_data_path) als Ordner angeben, werden bei einer SQL On-Demand-Abfrage Dateien aus diesem Ordner abgerufen. 

> [!NOTE]
> Im Gegensatz zu Hadoop und PolyBase werden von SQL On-Demand keine Unterordner zurückgegeben. Außerdem werden von SQL On-Demand anders als bei Hadoop und PolyBase keine Dateien zurückgegeben, deren Dateiname mit einem Unterstrich (_) oder einem Punkt (.) beginnt.

Im folgenden Beispiel wird „unstructured_data_path=`https://mystorageaccount.dfs.core.windows.net/webdata/`“ verwendet, wodurch bei einer SQL On-Demand-Abfrage Zeilen aus „mydata.txt“ und „_hidden.txt“ zurückgegeben werden. „mydata2.txt“ und „mydata3.txt“ werden nicht zurückgegeben, da sie sich in einem Unterordner befinden.

![Rekursive Daten für externe Tabellen](./media/develop-openrowset/folder-traversal.png)

`[WITH ( {'column_name' 'column_type' [ 'column_ordinal'] }) ]`

Mit der WITH-Klausel können Sie Spalten angeben, die aus Dateien gelesen werden sollen.

- Geben Sie bei CSV-Datendateien Spaltennamen und den jeweiligen Datentyp an, um alle Spalten zu lesen. Wenn Sie sich nur für eine Teilmenge der Spalten interessieren, verwenden Sie Ordinalzahlen, um die Spalten aus den Ursprungsdatendateien anhand der Ordinalzahl auszuwählen. Spalten werden auf der Grundlage der Ordinalangabe gebunden. 

> [!IMPORTANT]
> Die WITH-Klausel ist für CSV-Dateien obligatorisch.
- Geben Sie bei Parquet-Datendateien Spaltennamen an, die den Spaltennamen in den Ursprungsdatendateien entsprechen. Spalten werden auf der Grundlage des Namens gebunden. Ohne Angabe der WITH-Klausel werden alle Spalten aus Parquet-Dateien zurückgegeben.

column_name: Der Name für die Ausgabespalte. Bei Angabe dieser Option wird der Spaltenname in der Quelldatei überschrieben.

column_type: Der Datentyp für die Ausgabespalte. Hier findet die implizite Datentypkonvertierung statt.

column_ordinal: Die Ordinalzahl der Spalte in den Quelldateien. Dieses Argument wird bei Parquet-Dateien ignoriert, da die Bindung auf dem Namen basiert. Im folgenden Beispiel wird nur bei Verwendung einer CSV-Datei eine zweite Spalte zurückgegeben:

```sql
WITH (
    --[country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2 2
    --[year] smallint,
    --[population] bigint
)
```

**\<bulk_options>**

FIELDTERMINATOR ='field_terminator'

Dient zum Angeben des zu verwendenden Feldabschlusszeichens. Das Standard-Feldabschlusszeichen ist ein Komma ( **,** ).

ROWTERMINATOR ='row_terminator'`

Dient zum Angeben des zu verwendenden Zeilenabschlusszeichens. Standardmäßig wird ein Neue-Zeile-Zeichen als Zeilenabschlusszeichen verwendet (beispielsweise „\r\n“).

ESCAPE_CHAR = 'char'

Dient zum Angeben des Zeichens in der Datei, das als Escapezeichen für sich selbst und für alle Trennzeichenwerte in der Datei fungiert. Wenn auf das Escapezeichen ein Wert folgt, bei dem es sich nicht um das Escapezeichen selbst oder um einen der Trennzeichenwerte handelt, wird das Escapezeichen beim Lesen des Werts gelöscht. 

Der ESCAPE_CHAR-Parameter wird unabhängig davon angewendet, ob FIELDQUOTE aktiviert ist. Er fungiert nicht als Escapezeichen für das Zitierzeichen. Das Anführungszeichen wird in Übereinstimmung mit dem CSV-Verhalten von Excel mit doppelten Anführungszeichen als Escapezeichen versehen.

FIRSTROW = 'first_row' 

Gibt die Nummer der ersten zu ladenden Zeile an. Der Standardwert ist 1. Damit wird die erste Zeile in der festgelegten Datendatei angegeben. Die Zeilennummern werden durch Zählen der Zeilenabschlusszeichen bestimmt. FIRSTROW ist einsbasiert.

FIELDQUOTE = 'field_quote' 

Gibt ein Zeichen an, das als Anführungszeichen in der CSV-Datei verwendet wird. Ohne diese Angabe wird das Anführungszeichen (") verwendet. 

## <a name="examples"></a>Beispiele

Im folgenden Beispiel werden nur zwei Spalten mit den Ordinalzahlen 1 und 4 aus den Dateien „population*.csv“ zurückgegeben. Da in den Dateien keine Kopfzeile vorhanden ist, beginnt der Lesevorgang in der ersten Zeile:

```sql
/* make sure you have credentials for storage account access created
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer')
DROP CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]
GO

CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = ''
GO
*/

SELECT * 
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/population*.csv',
        FORMAT = 'CSV',
        FIRSTROW = 1
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2 1,
    [population] bigint 4
) AS [r]
```



Im folgenden Beispiel werden alle Spalten der ersten Zeile aus dem Zensus-Dataset im Parquet-Format ohne Angabe von Spaltennamen und Datentypen zurückgegeben: 

```sql
/* make sure you have credentials for storage account access created
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer')
DROP CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]
GO

CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = ''
GO
*/

SELECT 
    TOP 1 *
FROM  
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet',
        FORMAT='PARQUET'
    ) AS [r]
```



## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele finden Sie in den [Schnellstartanleitungen](query-data-storage.md). Alternativ können Sie die Ergebnisse Ihrer Abfrage mithilfe von [CETAS](develop-tables-cetas.md) in Azure Storage speichern.
