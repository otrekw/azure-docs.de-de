---
title: Abfragen von CSV-Dateien mit SQL On-Demand (Vorschauversion)
description: In diesem Artikel erfahren Sie, wie Sie mit SQL On-Demand (Vorschauversion) einzelne CSV-Dateien mit verschiedenen Dateiformaten abfragen können.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 3d09692c06bcdffbb070f545950092592e417838
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427558"
---
# <a name="query-csv-files"></a>Abfragen von CSV-Dateien

In diesem Artikel erfahren Sie, wie Sie eine einzelne CSV-Datei mit SQL On-Demand (Vorschauversion) in Azure Synapse Analytics abfragen. CSV-Dateien können unterschiedliche Formate aufweisen: 

- Mit und ohne Kopfzeile
- Werte mit Kommas und Tabstopps als Trennzeichen
- Zeilenenden im Windows- und Unix-Stil
- Werte mit und ohne Anführungszeichen sowie Zeichen mit Escapezeichen

Alle oben aufgeführten Varianten werden im Folgenden behandelt.

## <a name="prerequisites"></a>Voraussetzungen

Lesen Sie die folgenden Artikel, bevor Sie den Rest dieses Artikels lesen:

- [Erstmalige Einrichtung](query-data-storage.md#first-time-setup)
- [Voraussetzungen](query-data-storage.md#prerequisites)

## <a name="windows-style-new-line"></a>Neue-Zeile-Zeichen im Windows-Stil

Die folgende Abfrage zeigt, wie eine CSV-Datei ohne Kopfzeile gelesen wird, die ein Neue-Zeile-Zeichen im Windows-Stil und Spalten mit Kommas als Trennzeichen enthält.

Dateivorschau:

![Die ersten zehn Zeilen der CSV-Datei (ohne Kopfzeile und mit Neue-Zeile-Zeichen im Windows-Stil).](./media/query-single-csv-file/population.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/population.csv',
         FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '\n'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="unix-style-new-line"></a>Neue-Zeile-Zeichen im Unix-Stil

Die folgende Abfrage zeigt, wie eine Datei ohne Kopfzeile gelesen wird, die ein Neue-Zeile-Zeichen im Unix-Stil und Spalten mit Kommas als Trennzeichen enthält. Beachten Sie den abweichenden Speicherort der Datei im Vergleich zu den anderen Beispielen.

Dateivorschau:

![Die ersten zehn Zeilen der CSV-Datei ohne Kopfzeile und mit Neue-Zeile-Zeichen im Unix-Stil.](./media/query-single-csv-file/population-unix.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="header-row"></a>Kopfzeile

Die folgende Abfrage zeigt, wie eine Datei mit einer Kopfzeile, mit einem Neue-Zeile-Zeichen im Unix-Stil und Spalten mit Kommas als Trennzeichen gelesen wird. Beachten Sie den abweichenden Speicherort der Datei im Vergleich zu den anderen Beispielen.

Dateivorschau:

![Die ersten zehn Zeilen der CSV-Datei mit Kopfzeile und mit Neue-Zeile-Zeichen im Unix-Stil.](./media/query-single-csv-file/population-unix-hdr.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        FIRSTROW = 2
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="custom-quote-character"></a>Benutzerdefiniertes Anführungszeichen

Die folgende Abfrage zeigt, wie eine Datei mit Kopfzeile gelesen wird, die ein Neue-Zeile-Zeichen im Unix-Stil, Spalten mit Kommas als Trennzeichen und Werte in Anführungszeichen enthält. Beachten Sie den abweichenden Speicherort der Datei im Vergleich zu den anderen Beispielen.

Dateivorschau:

![Die ersten zehn Zeilen der CSV-Datei mit Kopfzeile und mit Neue-Zeile-Zeichen im Unix-Stil sowie mit Werten in Anführungszeichen.](./media/query-single-csv-file/population-unix-hdr-quoted.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr-quoted/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2,
        FIELDQUOTE = '"'
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

> [!NOTE]
> Diese Abfrage würde dieselben Ergebnisse liefern, wenn Sie den FIELDQUOTE-Parameter weglassen, da der Standardwert für FIELDQUOTE ein doppeltes Anführungszeichen ist.

## <a name="escaping-characters"></a>Zeichen mit Escapezeichen

Die folgende Abfrage zeigt, wie eine Datei mit einer Kopfzeile, mit einem Neue-Zeile-Zeichen im Unix-Stil, Spalten mit Kommas als Trennzeichen und einem Zeichen mit Escapezeichen für das Feldtrennzeichen (Komma) innerhalb von Werten gelesen werden kann. Beachten Sie den abweichenden Speicherort der Datei im Vergleich zu den anderen Beispielen.

Dateivorschau:

![Die ersten zehn Zeilen der CSV-Datei mit Kopfzeile und mit Neue-Zeile-Zeichen im Unix-Stil und Zeichen mit Escapezeichen als Feldtrennzeichen.](./media/query-single-csv-file/population-unix-hdr-escape.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr-escape/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2,
        ESCAPECHAR = '\\'
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Slov,enia';
```

> [!NOTE]
> Diese Abfrage würde nicht erfolgreich ausgeführt werden, wenn ESCAPECHAR nicht angegeben wird, da das Komma in „Slov,enia“ als Feldtrennzeichen und nicht als Teil des Ländernamens behandelt würde. „Slov,enia“ würde als zwei Spalten behandelt. Daher hätte die bestimmte Zeile eine Spalte mehr als die anderen Zeilen und eine Spalte mehr als Sie in der WITH-Klausel definiert haben.

## <a name="tab-delimited-files"></a>Dateien mit Tabstopps als Trennzeichen

Die folgende Abfrage zeigt, wie eine Datei mit Kopfzeile gelesen wird, die ein Neue-Zeile-Zeichen im Unix-Stil und Spalten mit Tabstopps als Trennzeichen enthält. Beachten Sie den abweichenden Speicherort der Datei im Vergleich zu den anderen Beispielen.

Dateivorschau:

![Die ersten zehn Zeilen der CSV-Datei mit Kopfzeile und mit Neue-Zeile-Zeichen im Unix-Stil sowie Tabstopps als Trennzeichen.](./media/query-single-csv-file/population-unix-hdr-tsv.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr-tsv/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR ='\t',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017
```

## <a name="returning-subset-of-columns"></a>Zurückgeben einer Teilmenge von Spalten

Bisher haben Sie das CSV-Dateischema mit WITH angegeben und alle Spalten aufgelistet. Sie können nur Spalten angeben, die Sie in Ihrer Abfrage tatsächlich benötigen, indem Sie für jede erforderliche Spalte eine Ordnungszahl verwenden. Sie werden auch Spalten auslassen, die Sie nicht interessieren.

Die folgende Abfrage gibt die Anzahl der verschiedenen Ländernamen in einer Datei zurück, wobei nur die erforderlichen Spalten angegeben werden:

> [!NOTE]
> Werfen Sie einen Blick auf die WITH-Klausel in der Abfrage unten und beachten Sie, dass am Ende der Zeile, in der Sie die Spalte *[country_name]* definieren, „2“ (ohne Anführungszeichen) steht. Dies bedeutet, dass die Spalte *[country_name]* die zweite Spalte in der Datei ist. Die Abfrage ignoriert alle Spalten in der Datei außer der zweiten.

```sql
SELECT
    COUNT(DISTINCT country_name) AS countries
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/population.csv',
         FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '\n'
    )
WITH (
    --[country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2 2
    --[year] smallint,
    --[population] bigint
) AS [r]
```

## <a name="next-steps"></a>Nächste Schritte

In den nächsten Artikeln erfahren Sie Folgendes:

- [Abfragen von Parquet-Dateien](query-parquet-files.md)
- [Abfragen von Ordnern und mehreren Dateien](query-folders-multiple-csv-files.md)
