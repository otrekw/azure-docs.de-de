---
title: Abfragen von Speicherdateien mithilfe von SQL On-Demand (Vorschau) innerhalb von Synapse SQL
description: In diesem Artikel wird beschrieben, wie Sie Speicherdateien mithilfe von SQL On-Demand (Vorschau) innerhalb von Synapse SQL abfragen.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/19/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 2126996620d6f891dde4e7530c057d2c7f31a996
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81676673"
---
# <a name="query-storage-files-using-sql-on-demand-preview-resources-within-synapse-sql"></a>Abfragen von Speicherdateien mithilfe von SQL On-Demand-Ressourcen (Vorschau) innerhalb von Synapse SQL

SQL On-Demand (Vorschau) ermöglicht Ihnen das Abfragen von Daten in Ihrem Data Lake. Dieses Feature bietet eine T-SQL-Abfrageoberfläche, die semistrukturierte und unstrukturierte Datenabfragen verarbeiten kann.

Für Abfragen werden die folgenden T-SQL-Aspekte unterstützt:

- Vollständige [SELECT](/sql/t-sql/queries/select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)-Oberfläche, einschließlich der meisten SQL-Funktionen, -Operatoren usw.
- CREATE EXTERNAL TABLE AS SELECT ([CETAS](develop-tables-cetas.md)) erstellt eine [externe Tabelle](develop-tables-external-tables.md) und exportiert parallel die Ergebnisse einer Transact-SQL-SELECT-Anweisung in Azure Storage.

Weitere Informationen zu unterstützten und nicht unterstützten Aspekten finden Sie im Artikel [Übersicht über SQL On-Demand](on-demand-workspace-overview.md).

Wenn Azure AD-Benutzer Abfragen ausführen, erfolgt der Zugriff auf Speicherkonten standardmäßig über das Azure AD-Pass-Through-Authentifizierungsprotokoll. Daher werden Identitätswechsel und Berechtigungsprüfung für Benutzer auf Speicherebene ausgeführt. Sie können die [Steuerung des Speicherzugriffs](develop-storage-files-storage-access-control.md) Ihren Anforderungen entsprechend einrichten.

## <a name="extensions"></a>Erweiterungen

Um reibungslose Vorgänge beim Abfragen von Daten in Azure Storage-Dateien zu gewährleisten, verwendet SQL On-Demand die Funktion [OPENROWSET](develop-openrowset.md) mit zusätzlichen Funktionen:

- [Abfragen mehrerer Dateien oder Ordner](#query-multiple-files-or-folders)
- [Parquet-Dateiformat](#parquet-file-format)
- [Zusätzliche Optionen für die Verwendung von durch Trennzeichen getrenntem Text (Feldabschlusszeichen, Zeilenabschlusszeichen, Escapezeichen)](#additional-options-for-working-with-delimited-text)
- [Lesen einer ausgewählten Teilmenge von Spalten](#read-a-chosen-subset-of-columns)
- [Schemarückschluss](#schema-inference)
- [filename-Funktion](#filename-function)
- [filepath-Funktion](#filepath-function)
- [Verwenden von komplexen Typen und geschachtelten oder wiederholten Datenstrukturen](#work-with-complex-types-and-nested-or-repeated-data-structures)

### <a name="query-multiple-files-or-folders"></a>Abfragen mehrerer Dateien oder Ordner

Um eine T-SQL-Abfrage für eine Gruppe von Dateien innerhalb eines Ordners oder für eine Ordnergruppe auszuführen und diese dabei als einzelne Entität oder einzelnes Rowset zu behandeln, geben Sie einen Pfad oder ein Muster für eine Datei- oder Ordnergruppe an. Verwenden Sie dabei Platzhalterzeichen.

Es gelten die folgenden Regeln:

- Muster können in einem Teil eines Verzeichnispfads oder in einem Dateinamen verwendet werden.
- Im gleichen Verzeichnisschritt oder Dateinamen können mehrere Muster verwendet werden.
- Wenn mehrere Platzhalterzeichen vorhanden sind, werden Dateien in allen übereinstimmenden Pfaden in den resultierenden Dateisatz eingeschlossen.

```
N'https://myaccount.blob.core.windows.net/myroot/*/mysubfolder/*.csv'
```

Beispiele zur Verwendung finden Sie unter [Abfragen von Ordnern und mehreren Dateien](query-folders-multiple-csv-files.md).

### <a name="parquet-file-format"></a>Parquet-Dateiformat

Um Parquet-Quelldaten abzufragen, verwenden Sie FORMAT = 'PARQUET'

```syntaxsql
OPENROWSET
(
    { BULK 'data_file' ,
    { FORMATFILE = 'format_file_path' [ <bulk_options>] | SINGLE_BLOB | SINGLE_CLOB | SINGLE_NCLOB } }
)
AS table_alias(column_alias,...n)
<bulk_options> ::=
...
[ , FORMAT = {'CSV' | 'PARQUET'} ]
```

Beispiele zur Verwendung finden Sie im Artikel [Abfragen von Parquet-Dateien](query-parquet-files.md).

### <a name="additional-options-for-working-with-delimited-text"></a>Weitere Optionen für durch Trennzeichen getrennten Text

Die folgenden zusätzlichen Parameter wurden für die Arbeit mit CSV-Dateien (durch Trennzeichen getrennter Text) eingeführt:

```syntaxsql
<bulk_options> ::=
...
[ , FIELDTERMINATOR = 'char' ]
[ , ROWTERMINATOR = 'char' ]
[ , ESCAPE_CHAR = 'char' ]
...
```

- ESCAPE_CHAR = 'char': Dient zum Angeben des Zeichens in der Datei, das als Escapezeichen für sich selbst und für alle Trennzeichenwerte in der Datei fungiert. Wenn auf das Escapezeichen ein Wert folgt, bei dem es sich nicht um das Escapezeichen selbst oder um einen der Trennzeichenwerte handelt, wird das Escapezeichen beim Lesen des Werts verworfen.
Der ESCAPE_CHAR-Parameter wird unabhängig davon angewendet, ob FIELDQUOTE aktiviert ist. Er fungiert nicht als Escapezeichen für das Anführungszeichen. Das Anführungszeichen wird in Übereinstimmung mit dem CSV-Verhalten von Excel mit doppelten Anführungszeichen als Escapezeichen versehen.
- FIELDTERMINATOR ='field_terminator': Dient zum Angeben des zu verwendenden Feldabschlusszeichens. Das Standard-Feldabschlusszeichen ist das Komma ( **,** ).
- ROWTERMINATOR ='row_terminator': Dient zum Angeben des zu verwendenden Zeilenabschlusszeichens. Das Standard-Zeilenabschlusszeichen ist das Zeilenvorschubzeichen:  **\r\n**.

### <a name="read-a-chosen-subset-of-columns"></a>Lesen einer ausgewählten Teilmenge von Spalten

Um zu lesende Spalten anzugeben, können Sie eine optionale WITH-Klausel in Ihre OPENROWSET-Anweisung einfügen.

- Geben Sie bei CSV-Datendateien Spaltennamen und den jeweiligen Datentyp an, um alle Spalten zu lesen. Wenn Sie nur eine Teilmenge von Spalten abrufen möchten, verwenden Sie Ordnungszahlen, um die Spalten anhand ihrer Ordnungszahl aus den Ursprungsdatendateien auszuwählen. Spalten werden anhand der Ordnungszahl gebunden.
- Wenn es sich um Parquet-Datendateien handelt, geben Sie Spaltennamen an, die den Spaltennamen in den Ursprungsdatendateien entsprechen. Spalten werden anhand des Namens gebunden.

```syntaxsql
OPENROWSET
...
| BULK 'data_file',
{ FORMATFILE = 'format_file_path' [ <bulk_options>] | SINGLE_BLOB | SINGLE_CLOB | SINGLE_NCLOB } }
) AS table_alias(column_alias,...n) | WITH ( {'column_name' 'column_type' [ 'column_ordinal'] })
```

Beispiele finden Sie unter [Lesen von CSV-Dateien ohne Angabe aller Spalten](query-single-csv-file.md#returning-subset-of-columns).

### <a name="schema-inference"></a>Schemarückschluss

Indem Sie die WITH-Klausel in der OPENROWSET-Anweisung weglassen, können Sie den Dienst anweisen, das Schema anhand der zugrunde liegenden Dateien per Rückschluss automatisch zu erkennen.

> [!NOTE]
> Dies funktioniert derzeit nur für das Parquet-Dateiformat.

```sql
OPENROWSET(
BULK N'path_to_file(s)', FORMAT='PARQUET');
```

### <a name="filename-function"></a>filename-Funktion

Diese Funktion gibt den Namen der Datei zurück, aus der die Zeile stammt.

Informationen zum Abfragen bestimmter Dateien finden Sie in Abschnitt zu Dateinamen im Artikel [Abfragen bestimmter Dateien](query-specific-files.md#filename).

### <a name="filepath-function"></a>filepath-Funktion

Diese Funktion gibt eine vollständigen Pfad oder eine Teil eines Pfads zurück:

- Beim Aufruf ohne Parameter wird der vollständige Pfad einer Datei zurückgegeben, aus der eine Zeile stammt.
- Beim Aufruf mit Parameter wird ein Teil eines Pfads zurückgegeben, der dem Platzhalterzeichen an der im Parameter angegebenen Position entspricht. Der Parameterwert 1 beispielsweise würde den Teil eines Pfads zurückgeben, der dem ersten Platzhalterzeichen entspricht.

Weitere Informationen finden Sie im Abschnitt zu Dateipfaden im Artikel [Abfragen bestimmter Dateien](query-specific-files.md#filepath).

### <a name="work-with-complex-types-and-nested-or-repeated-data-structures"></a>Verwenden von komplexen Typen und geschachtelten oder wiederholten Datenstrukturen

Um problemlos mit Daten arbeiten zu können, die in geschachtelten oder wiederholten Datentypen (beispielsweise in [Parquet](https://github.com/apache/parquet-format/blob/master/LogicalTypes.md#nested-types)-Dateien) gespeichert sind, wurden von SQL On-Demand folgende Erweiterungen hinzugefügt.

#### <a name="project-nested-or-repeated-data"></a>Projizieren von geschachtelten oder wiederholten Daten

Zum Projizieren von Daten führen Sie eine SELECT-Anweisung in der Parquet-Datei aus, die Spalten mit geschachtelten Datentypen enthält. In der Ausgabe werden die geschalteten Werte in das JSON-Format serialisiert und als varchar(8000)-SQL-Datentyp zurückgegeben.

```sql
    SELECT * FROM
    OPENROWSET
    (   BULK 'unstructured_data_path' ,
        FORMAT = 'PARQUET' )
    [AS alias]
```

Ausführlichere Informationen finden Sie im Abschnitt zum Projizieren von geschachtelten oder wiederholten Daten im Artikel [Abfragen von geschachtelten Parquet-Typen](query-parquet-nested-types.md#project-nested-or-repeated-data).

#### <a name="access-elements-from-nested-columns"></a>Zugreifen auf Elemente aus geschachtelten Spalten

Um auf geschachtelte Elemente aus einer geschachtelten Spalte zuzugreifen – z. B. eine Struktur –, verwenden Sie die Punktnotation, um Feldnamen im Pfad zu verketten. Geben Sie den Pfad als „column_name“ in der WITH-Klausel der OPENROWSET-Funktion an.

So sieht das Syntaxfragment aus:

```syntaxsql
    OPENROWSET
    (   BULK 'unstructured_data_path' ,
        FORMAT = 'PARQUET' )
    WITH ({'column_name' 'column_type',})
    [AS alias]
    'column_name' ::= '[field_name.] field_name'
```

Standardmäßig gleicht die OPENROWSET-Funktion den Namen und Pfad des Quellfelds mit dem Spaltennamen ab, der in der WITH-Klausel angegeben ist. Auf Elemente, die sich auf verschiedenen Schachtelungsebenen innerhalb derselben Parquet-Quelldatei befinden, kann über die WITH-Klausel zugegriffen werden.

**Rückgabewerte**

- Diese Funktion gibt für alle Parquet-Typen, die sich nicht in der Gruppe „Geschachtelter Typ“ befinden, einen Skalarwert wie z. B. „int“, „decimal“ oder „varchar“ aus dem angegebenen Element im angegebenen Pfad zurück.
- Wenn der Pfad auf ein Element eines geschachtelten Typs zeigt, gibt die Funktion ein JSON-Fragment zurück, das im obersten Element im angegebenen Pfad beginnt. Das JSON-Fragment weist den Typ „varchar(8000)“ auf.
- Wenn die Eigenschaft im angegebenen column_name-Element nicht gefunden wird, gibt die Funktion einen Fehler zurück.
- Wenn die Eigenschaft im angegebenen column_path-Element nicht gefunden wird, gibt die Funktion je nach [Pfadmodus](/sql/relational-databases/json/json-path-expressions-sql-server?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#PATHMODE) im Strict-Modus einen Fehler oder im Lax-Modus NULL zurück.

Beispiele für Abfragen finden Sie im Abschnitt „Zugreifen auf Elemente aus geschachtelten Spalten“ im Artikel [Abfragen von geschachtelten Parquet-Typen](query-parquet-nested-types.md#access-elements-from-nested-columns).

#### <a name="access-elements-from-repeated-columns"></a>Zugreifen auf Elemente aus wiederholten Spalten

Um auf Elemente aus einer wiederholten Spalte zuzugreifen – z. B. ein Element eines Arrays oder einer Zuordnung –, verwenden Sie die [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)-Funktion für jedes skalare Element, das Sie projizieren und bereitstellen müssen:

- Geschachtelte oder wiederholte Spalte als erster Parameter
- Ein [JSON-Pfad](/sql/relational-databases/json/json-path-expressions-sql-server?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), der das Element oder die Eigenschaft angibt, auf das bzw. die zugegriffen werden soll, als zweiter Parameter

Um auf nicht skalare Elemente aus einer wiederholten Spalte zuzugreifen, verwenden Sie die [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)-Funktion für jedes nicht skalare Element, das Sie projizieren und bereitstellen müssen:

- Geschachtelte oder wiederholte Spalte als erster Parameter
- Ein [JSON-Pfad](/sql/relational-databases/json/json-path-expressions-sql-server?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), der das Element oder die Eigenschaft angibt, auf das bzw. die zugegriffen werden soll, als zweiter Parameter

Hier finden Sie das entsprechende Syntaxfragment:

```syntaxsql
    SELECT
       { JSON_VALUE (column_name, path_to_sub_element), }
       { JSON_QUERY (column_name [ , path_to_sub_element ]), )
    FROM
    OPENROWSET
    (   BULK 'unstructured_data_path' ,
        FORMAT = 'PARQUET' )
    [AS alias]
```

Abfragebeispiele für den Zugriff auf Elemente aus wiederholten Spalten finden Sie im Artikel [Abfragen von geschachtelten Parquet-Typen](query-parquet-nested-types.md#access-elements-from-repeated-columns).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Abfragen verschiedener Dateitypen und zum Erstellen und Verwenden von Sichten finden Sie in den folgenden Artikeln:

- [Abfragen einer einzelnen CSV-Datei](query-single-csv-file.md)
- [Abfragen von Parquet-Dateien](query-parquet-files.md)
- [Abfragen von JSON-Dateien](query-json-files.md)
- [Abfragen von geschachtelten Parquet-Typen](query-parquet-nested-types.md)
- [Abfragen von Ordnern und mehreren CSV-Dateien](query-folders-multiple-csv-files.md)
- [Verwenden von Dateimetadaten in Abfragen](query-specific-files.md)
- [Erstellen und Verwenden von Sichten](create-use-views.md)
