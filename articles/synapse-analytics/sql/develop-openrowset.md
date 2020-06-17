---
title: Verwenden von OPENROWSET in SQL On-Demand (Vorschauversion)
description: In diesem Artikel wird die Syntax von OPENROWSET in SQL On-Demand (Vorschauversion) beschrieben und die Verwendung von Argumenten erläutert.
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 05/07/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 9c2a2d7059e24b37b0f47d0b568a3929f296d8c6
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84560872"
---
# <a name="how-to-use-openrowset-with-sql-on-demand-preview"></a>Verwenden von OPENROWSET mit SQL On-Demand (Vorschauversion)

Die `OPENROWSET(BULK...)`-Funktion ermöglicht den Zugriff auf Dateien in Azure Storage. Die `OPENROWSET`-Funktion liest den Inhalt einer Remotedatenquelle (z. B. einer Datei) und gibt den Inhalt als eine Reihe von Zeilen zurück. Innerhalb der SQL On-Demand-Ressource (Vorschauversion) wird die OPENROWSET-Funktion aufgerufen und die BULK-Option angegeben, um auf den OPENROWSET-Massenrowsetanbieter zuzugreifen.  

Auf die `OPENROWSET`-Funktion kann in der `FROM`-Klausel einer Abfrage so verwiesen werden, als handele es sich um einen Tabellennamen vom Typ `OPENROWSET`. Sie unterstützt Massenvorgänge über einen integrierten BULK-Anbieter, mit dem Daten aus einer Datei gelesen und als Rowset zurückgegeben werden können.

## <a name="data-source"></a>Datenquelle

Die OPENROWSET-Funktion in Synapse SQL liest den Inhalt der Datei(en) aus einer Datenquelle. Die Datenquelle ist ein Azure-Speicherkonto, auf das in der `OPENROWSET`-Funktion explizit verwiesen werden oder das dynamisch aus der URL der zu lesenden Dateien abgeleitet werden kann.
Die `OPENROWSET`-Funktion kann optional einen `DATA_SOURCE`-Parameter enthalten, um die Datenquelle anzugeben, die Dateien enthält.
- Mit `OPENROWSET` ohne `DATA_SOURCE` kann der Inhalt der Dateien direkt aus dem URL-Speicherort gelesen werden, der als `BULK`-Option angegeben ist:

    ```sql
    SELECT *
    FROM OPENROWSET(BULK 'http://storage..../container/folder/*.parquet',
                    TYPE = 'PARQUET') AS file
    ```

Dies ist eine schnelle und einfache Möglichkeit, den Inhalt der Dateien ohne Vorkonfiguration zu lesen. Mit dieser Option können Sie die Standardauthentifizierungsoption zum Zugreifen auf den Speicher verwenden (Azure AD-Pass-Through für Azure AD-Anmeldungen und SAS-Token für SQL-Anmeldungen). 

- Mit `OPENROWSET` mit `DATA_SOURCE` kann auf Dateien im angegebenen Speicherkonto zugegriffen werden:

    ```sql
    SELECT *
    FROM OPENROWSET(BULK '/folder/*.parquet',
                    DATA_SOURCE='storage', --> Root URL is in LOCATION of DATA SOURCE
                    TYPE = 'PARQUET') AS file
    ```


    Mit dieser Option können Sie den Speicherort des Speicherkontos in der Datenquelle konfigurieren und die Authentifizierungsmethode angeben, die für den Speicherzugriff verwendet werden soll. 
    
    > [!IMPORTANT]
    > `OPENROWSET` ohne `DATA_SOURCE` bietet eine schnelle und einfache Möglichkeit, auf die Speicherdateien zuzugreifen, weist jedoch eingeschränkte Authentifizierungsoptionen auf. Beispielsweise kann der Azure AD-Prinzipal nur auf Dateien über deren [Azure AD-Identität](develop-storage-files-storage-access-control.md?tabs=user-identity#force-azure-ad-pass-through) und nicht auf öffentlich verfügbare Dateien zugreifen. Wenn Sie leistungsfähigere Authentifizierungsoptionen benötigen, verwenden Sie die `DATA_SOURCE`-Option, und definieren Sie die Anmeldeinformation, die Sie für den Speicherzugriff verwenden möchten.


## <a name="security"></a>Sicherheit

Ein Datenbankbenutzer muss über die Berechtigung `ADMINISTER BULK OPERATIONS` verfügen, um die `OPENROWSET`-Funktion zu verwenden.

Der Speicheradministrator muss einem Benutzer außerdem den Zugriff auf die Dateien ermöglichen, indem er ein gültiges SAS-Token bereitstellt oder dem Azure AD-Prinzipal den Zugriff auf Speicherdateien ermöglicht. Weitere Informationen zur Speicherzugriffssteuerung finden Sie in [diesem Artikel](develop-storage-files-storage-access-control.md).

`OPENROWSET` verwendet die folgenden Regeln, um zu bestimmen, wie die Authentifizierung beim Speicher erfolgt:
- Bei `OPENROWSET` ohne `DATA_SOURCE` ist der Authentifizierungsmechanismus abhängig vom Aufrufertyp.
  - Azure AD-Anmeldungen können über die eigene [Azure AD-Identität](develop-storage-files-storage-access-control.md?tabs=user-identity#supported-storage-authorization-types) nur dann auf Dateien zugreifen, wenn der Azure-Speicher dem Azure AD-Benutzer den Zugriff auf zugrunde liegende Dateien erlaubt (z. B. wenn der Aufrufer über die Berechtigung als Leser für den Speicher verfügt) und wenn Sie die [Azure AD-Pass-Through-Authentifizierung](develop-storage-files-storage-access-control.md#force-azure-ad-pass-through) für den Synapse SQL-Dienst aktivieren.
  - SQL-Anmeldungen können `OPENROWSET` auch ohne `DATA_SOURCE` verwenden, um auf öffentlich verfügbare Dateien, Dateien, die mit einem SAS-Token geschützt sind, oder eine verwaltete Identität des Synapse-Arbeitsbereichs zuzugreifen. Sie müssen eine [serverbezogene Anmeldeinformation erstellen](develop-storage-files-storage-access-control.md#examples), um den Zugriff auf Speicherdateien zuzulassen. 
- Bei `OPENROWSET` mit `DATA_SOURCE` ist der Authentifizierungsmechanismus in der datenbankbezogenen Anmeldeinformation definiert, die der referenzierten Datenquelle zugewiesen wurde. Mit dieser Option können Sie auf öffentlich verfügbaren Speicher zugreifen oder über ein SAS-Token, die verwaltete Identität des Arbeitsbereichs oder die [Azure AD-Identität des Aufrufers](develop-storage-files-storage-access-control.md?tabs=user-identity#supported-storage-authorization-types) (wenn der Aufrufer der Azure AD-Prinzipal ist) auf den Speicher zugreifen. Wenn `DATA_SOURCE` auf nicht öffentlichen Azure-Speicher verweist, müssen Sie eine [datenbankbezogene Anmeldeinformation erstellen](develop-storage-files-storage-access-control.md#examples) und in `DATA SOURCE` darauf verweisen, um den Zugriff auf Speicherdateien zu erlauben.

Der Aufrufer muss über die Berechtigung `REFERENCES` für die Anmeldeinformation verfügen, um sie für die Authentifizierung beim Speicher zu verwenden.

## <a name="syntax"></a>Syntax

```syntaxsql
--OPENROWSET syntax for reading Parquet files
OPENROWSET  
( { BULK 'unstructured_data_path' , [DATA_SOURCE = <data source name>, ]
    FORMAT='PARQUET' }  
)  
[WITH ( {'column_name' 'column_type' }) ]
[AS] table_alias(column_alias,...n)

--OPENROWSET syntax for reading delimited text files
OPENROWSET  
( { BULK 'unstructured_data_path' , [DATA_SOURCE = <data source name>, ] 
    FORMAT = 'CSV'
    [ <bulk_options> ] }  
)  
WITH ( {'column_name' 'column_type' [ 'column_ordinal'] })  
[AS] table_alias(column_alias,...n)
 
<bulk_options> ::=  
[ , FIELDTERMINATOR = 'char' ]    
[ , ROWTERMINATOR = 'char' ] 
[ , ESCAPE_CHAR = 'char' ] 
[ , FIRSTROW = 'first_row' ]     
[ , FIELDQUOTE = 'quote_characters' ]
[ , DATA_COMPRESSION = 'data_compression_method' ]
[ , PARSER_VERSION = 'parser_version' ]
```

## <a name="arguments"></a>Argumente

Bei den Eingabedateien mit den Zieldaten für die Abfrage stehen zwei Optionen zur Auswahl. Gültige Werte sind:

- 'CSV': Eine beliebige, durch Trennzeichen getrennte Textdatei mit Zeilen-/Spaltentrennzeichen. Als Feldtrennzeichen kann ein beliebiges Zeichen verwendet werden. Für durch Tabstopp getrennte Werte wäre dies beispielsweise: FIELDTERMINATOR = tab.

- 'PARQUET': Binärdatei im Parquet-Format. 

**'unstructured_data_path'**

Bei dem Pfad für unstrukturierte Daten kann es sich um einen absoluten oder relativen Pfad handeln:
- Der absolute Pfad im Format \<prefix>://\<storage_account_path>/\<storage_path> ermöglicht einem Benutzer, die Dateien direkt zu lesen.
- Der relative Pfad im Format '<Speicherpfad>' muss mit dem Parameter `DATA_SOURCE` verwendet werden und beschreibt das Dateimuster innerhalb des Speicherorts <Speicherkontopfad>, der in `EXTERNAL DATA SOURCE` definiert ist. 

 Nachfolgend sind die relevanten <storage account path>-Werte für die Verknüpfung mit Ihrer speziellen externen Datenquelle angegeben. 

| Externe Datenquelle       | Präfix | Speicherkontopfad                                 |
| -------------------------- | ------ | ---------------------------------------------------- |
| Azure Blob Storage         | https  | \<storage_account>.blob.core.windows.net             |
| Azure Data Lake Store Gen1 | https  | \<storage_account>.azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Store Gen2 | https  | \<storage_account>.dfs.core.windows.net              |
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
    >
    
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

Dient zum Angeben des zu verwendenden Zeilenabschlusszeichens. Wenn kein Zeilenabschlusszeichen angegeben wird, wird eines der Standardabschlusszeichen verwendet. Standardabschlusszeichen für „PARSER_VERSION = '1.0'“ sind „\r\n“, „\n“ und „\r“. Standardabschlusszeichen für „PARSER_VERSION = '2.0'“ sind „\r\n“ und „\n“.

ESCAPE_CHAR = 'char'

Dient zum Angeben des Zeichens in der Datei, das als Escapezeichen für sich selbst und für alle Trennzeichenwerte in der Datei fungiert. Wenn auf das Escapezeichen ein Wert folgt, bei dem es sich nicht um das Escapezeichen selbst oder um einen der Trennzeichenwerte handelt, wird das Escapezeichen beim Lesen des Werts gelöscht. 

Der ESCAPE_CHAR-Parameter wird unabhängig davon angewendet, ob FIELDQUOTE aktiviert ist. Er fungiert nicht als Escapezeichen für das Zitierzeichen. Das Anführungszeichen wird in Übereinstimmung mit dem CSV-Verhalten von Excel mit doppelten Anführungszeichen als Escapezeichen versehen.

FIRSTROW = 'first_row' 

Gibt die Nummer der ersten zu ladenden Zeile an. Der Standardwert ist 1. Damit wird die erste Zeile in der festgelegten Datendatei angegeben. Die Zeilennummern werden durch Zählen der Zeilenabschlusszeichen bestimmt. FIRSTROW ist einsbasiert.

FIELDQUOTE = 'field_quote' 

Gibt ein Zeichen an, das als Anführungszeichen in der CSV-Datei verwendet wird. Ohne diese Angabe wird das Anführungszeichen (") verwendet. 

DATA_COMPRESSION = 'data_compression_method'

Gibt die Komprimierungsmethode an. Die folgende Komprimierungsmethode wird unterstützt:

- org.apache.hadoop.io.compress.GzipCodec

PARSER_VERSION = 'parser_version'

Gibt die beim Lesen von Dateien zu verwendende Parserversion an. Zurzeit werden die CSV-Parserversionen 1.0 und 2.0 unterstützt:

- PARSER_VERSION = '1.0'
- PARSER_VERSION = '2.0'

Die CSV-Parserversion 1.0 ist die funktionsreiche Standardversion, während 2.0 im Hinblick auf Leistung erstellt wurde und nicht alle Optionen und Codierungen unterstützt. 

Einzelheiten zu CSV-Parserversion 2.0:

- Nicht alle Datentypen werden unterstützt.
- Die maximale Zeilengröße beträgt 8 MB.
- Die folgenden Optionen werden nicht unterstützt: DATA_COMPRESSION.
- Eine leere Zeichenfolge in Anführungszeichen ("") wird als leere Zeichenfolge interpretiert.

## <a name="examples"></a>Beispiele

Im folgenden Beispiel werden nur zwei Spalten mit den Ordinalzahlen 1 und 4 aus den Dateien „population*.csv“ zurückgegeben. Da in den Dateien keine Kopfzeile vorhanden ist, beginnt der Lesevorgang in der ersten Zeile:

```sql
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
SELECT 
    TOP 1 *
FROM  
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet',
        FORMAT='PARQUET'
    ) AS [r]
```

Wenn Ihnen eine Fehlermeldung mitteilt, dass die Dateien nicht aufgelistet werden können, müssen Sie den Zugriff auf öffentlichen Speicher in Synapse SQL On-Demand aktivieren:
- Wenn Sie eine SQL-Anmeldung verwenden, müssen Sie eine [serverbezogene Anmeldeinformation erstellen, die den Zugriff auf öffentlichen Speicher erlaubt](develop-storage-files-storage-access-control.md#examples).
- Wenn Sie einen Azure AD-Prinzipal für den Zugriff auf öffentlichen Speicher verwenden, müssen Sie eine [serverbezogene Anmeldeinformation erstellen, die den Zugriff auf öffentlichen Speicher erlaubt](develop-storage-files-storage-access-control.md#examples), und die [Azure AD-Pass-Through-Authentifizierung](develop-storage-files-storage-access-control.md#disable-forcing-azure-ad-pass-through) deaktivieren.

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele finden Sie im [Schnellstart zum Abfragen von Daten im Speicher](query-data-storage.md). Dort erfahren Sie, wie Sie OPENROWSET zum Lesen von [CSV](query-single-csv-file.md)-, [PARQUET](query-parquet-files.md)- und [JSON](query-json-files.md)-Dateiformaten verwenden. Sie erfahren außerdem, wie Sie die Ergebnisse Ihrer Abfrage mithilfe von [CETAS](develop-tables-cetas.md) in Azure Storage speichern.
