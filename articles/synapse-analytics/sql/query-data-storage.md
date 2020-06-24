---
title: 'Übersicht: Abfragen von Daten im Speicher mithilfe von SQL On-Demand (Vorschauversion)'
description: Dieser Abschnitt enthält Beispielabfragen, mit denen Sie die SQL On-Demand-Ressource (Vorschauversion) in Azure Synapse Analytics ausprobieren können.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: d42dac0899c5ab9c4f817df40095dd08891fb8e7
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2020
ms.locfileid: "85207701"
---
# <a name="overview-query-data-in-storage"></a>Übersicht: Abfragen von Daten im Speicher

Dieser Abschnitt enthält Beispielabfragen, mit denen Sie die SQL On-Demand-Ressource (Vorschauversion) in Azure Synapse Analytics ausprobieren können.
Die folgenden Dateiformate werden derzeit unterstützt:  
- CSV
- Parquet
- JSON

## <a name="prerequisites"></a>Voraussetzungen

Für Abfragen erforderliche Tools:

- SQL-Client Ihrer Wahl:
    - Azure Synapse Studio (Vorschauversion)
    - Azure Data Studio
    - SQL Server Management Studio

Zusätzlich folgende Parameter:

| Parameter                                 | BESCHREIBUNG                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| Dienstendpunktadresse von SQL On-Demand    | Wird als Servername verwendet.                                   |
| Dienstendpunktregion von SQL On-Demand     | Wird verwendet, um den in den Beispielen verwendeten Speicher zu bestimmen. |
| Benutzername und Kennwort für den Endpunktzugriff | Wird für den Zugriff auf den Endpunkt verwendet.                               |
| Datenbank zum Erstellen von Sichten     | Diese Datenbank wird als Ausgangspunkt für die Beispiele verwendet.       |

## <a name="first-time-setup"></a>Erstmalige Einrichtung

Im ersten Schritt **erstellen Sie eine Datenbank**, in der Sie die Abfragen ausführen. Initialisieren Sie dann die Objekte, indem Sie das [Setupskript](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) für diese Datenbank ausführen. Mit diesem Setupskript werden die Datenquellen, die für die gesamte Datenbank gültigen Anmeldeinformationen und externe Dateiformate erstellt, die in diesen Beispielen zum Lesen von Daten verwendet werden.

> [!NOTE]
> Datenbanken werden nur zum Anzeigen von Metadaten (nicht für tatsächliche Daten) verwendet.  Notieren Sie sich den von Ihnen verwendeten Datenbanknamen. Sie benötigen ihn später.

```sql
CREATE DATABASE mydbname;
```

## <a name="provided-demo-data"></a>Bereitgestellte Demodaten

Die Demodaten enthalten die folgenden Datasets:

- Fahrtenaufzeichnungen für „NYC Taxi – Yellow Taxi“ (Teil des öffentlichen NYC-Datasets)
  - CSV-Format
  - Parquet-Format
- Dataset mit Bevölkerungsdaten
  - CSV-Format
- Parquet-Beispieldateien mit geschachtelten Spalten
  - Parquet-Format
- Bücher (JSON)
  - JSON-Format

| Ordnerpfad                                                  | BESCHREIBUNG                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| /csv/                                                        | Übergeordneter Ordner für Daten im CSV-Format                         |
| /csv/population/<br />/csv/population-unix/<br />/csv/population-unix-hdr/<br />/csv/population-unix-hdr-escape<br />/csv/population-unix-hdr-quoted | Ordner mit Bevölkerungsdatendateien in verschiedenen CSV-Formaten |
| /csv/taxi/                                                   | Ordner mit öffentlichen NYC-Datendateien im CSV-Format              |
| /parquet/                                                    | Übergeordneter Ordner für Daten im Parquet-Format                     |
| /parquet/taxi                                                | Öffentliche NYC-Datendateien im Parquet-Format, partitioniert nach Jahr und Monat (mithilfe des Hive-/Hadoop-Partitionierungsschemas) |
| /parquet/nested/                                             | Parquet-Beispieldateien mit geschachtelten Spalten                     |
| /json/                                                       | Übergeordneter Ordner für Daten im JSON-Format                        |
| /json/books/                                                 | JSON-Dateien mit Bücherdaten                                   |

### <a name="sample-query"></a>Beispielabfrage

Als letzter Überprüfungsschritt muss die folgende Abfrage ausgeführt werden:

```sql
SELECT
    COUNT_BIG(*)
FROM  
    OPENROWSET(
        BULK 'parquet/taxi/year=2017/month=9/*.parquet',
        DATA_SOURCE = 'sqlondemanddemo',
        FORMAT='PARQUET'
    ) AS nyc;
```

Von der obigen Abfrage sollte die folgende Zahl zurückgegeben werden: **8945574**.

## <a name="next-steps"></a>Nächste Schritte

Nun können Sie mit den folgenden Anleitungsartikeln fortfahren:

- [Abfragen einer einzelnen CSV-Datei](query-single-csv-file.md)

- [Abfragen von Ordnern und mehreren CSV-Dateien](query-folders-multiple-csv-files.md)

- [Abfragen bestimmter Dateien](query-specific-files.md)

- [Abfragen von Parquet-Dateien](query-parquet-files.md)

- [Abfragen von geschachtelten Parquet-Typen](query-parquet-nested-types.md)

- [Abfragen von JSON-Dateien](query-json-files.md)

- [Erstellen und Verwenden von Sichten](create-use-views.md)
