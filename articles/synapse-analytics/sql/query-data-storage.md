---
title: 'Übersicht: Abfragen von Daten im Speicher mithilfe von SQL On-Demand (Vorschauversion)'
description: Dieser Abschnitt enthält Beispielabfragen, mit denen Sie die SQL On-Demand-Ressource (Vorschauversion) in Azure Synapse Analytics ausprobieren können.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: cdad95b1a910a45629e85bcc716218b272afd9de
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81421294"
---
# <a name="overview-query-data-in-storage"></a>Übersicht: Abfragen von Daten im Speicher

Dieser Abschnitt enthält Beispielabfragen, mit denen Sie die SQL On-Demand-Ressource (Vorschauversion) in Azure Synapse Analytics ausprobieren können.
Aktuell werden folgende Dateien unterstützt: 
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

| Parameter                                 | Beschreibung                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| Dienstendpunktadresse von SQL On-Demand    | Wird als Servername verwendet.                                   |
| Dienstendpunktregion von SQL On-Demand     | Wird verwendet, um den in den Beispielen verwendeten Speicher zu bestimmen. |
| Benutzername und Kennwort für den Endpunktzugriff | Wird für den Zugriff auf den Endpunkt verwendet.                               |
| Datenbank zum Erstellen von Sichten     | Diese Datenbank wird als Ausgangspunkt für die Beispiele verwendet.       |

## <a name="first-time-setup"></a>Erstmalige Einrichtung

Führen Sie zunächst die beiden folgenden Schritte aus, um die Beispiele weiter unten in diesem Artikel verwenden zu können:

- Erstellen Sie eine Datenbank für Ihre Sichten (sofern Sie Sichten verwenden möchten).
- Erstellen Sie Anmeldeinformationen, die von SQL On-Demand für den Zugriff auf die Dateien im Speicher verwendet werden können.

### <a name="create-database"></a>Erstellen einer Datenbank

Zum Erstellen von Sichten wird eine Datenbank benötigt. Diese Datenbank wird für einige der Beispielabfragen in dieser Dokumentation verwendet.

> [!NOTE]
> Datenbanken werden nur zum Anzeigen von Metadaten (nicht für tatsächliche Daten) verwendet.  Notieren Sie sich den von Ihnen verwendeten Datenbanknamen. Sie benötigen ihn später.

```sql
CREATE DATABASE mydbname;
```

### <a name="create-credentials"></a>Erstellen von Anmeldeinformationen

Sie müssen Anmeldeinformationen erstellen, damit Sie Abfragen ausführen können. Diese Anmeldeinformationen werden von SQL On-Demand für den Zugriff auf die Dateien im Speicher verwendet.

> [!NOTE]
> Zur erfolgreichen Ausführung der Anleitungen in diesem Abschnitt muss ein SAS-Token verwendet werden.
>
> Für die Verwendung von SAS-Token muss die Benutzeridentität gelöscht werden, wie in [diesem Artikel](develop-storage-files-storage-access-control.md#disable-forcing-azure-ad-pass-through) erläutert.
>
> Von SQL On-Demand wird standardmäßig immer AAD-Pass-Through verwendet.

Weitere Informationen zum Verwalten der Speicherzugriffssteuerung finden Sie unter [diesem Link](develop-storage-files-storage-access-control.md).

> [!WARNING]
> Sie müssen Anmeldeinformationen für ein Speicherkonto erstellen, das sich in der Region Ihres Endpunkts befindet. SQL On-Demand kann zwar auf Speicher aus unterschiedlichen Regionen zugreifen, die Leistung ist jedoch besser, wenn sich Speicher und Endpunkt in derselben Region befinden.

Führen Sie den folgenden Code aus, um Anmeldeinformationen für CSV-, JSON- und Parquet-Container zu erstellen:

```sql
-- create credentials for CSV container in our demo storage account
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://sqlondemandstorage.blob.core.windows.net/csv')
DROP CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/csv];
GO

CREATE CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/csv]
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D';
GO

-- create credentials for JSON container in our demo storage account
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://sqlondemandstorage.blob.core.windows.net/json')
DROP CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/json];
GO

CREATE CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/json]
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D';
GO

-- create credentials for PARQUET container in our demo storage account
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://sqlondemandstorage.blob.core.windows.net/parquet')
DROP CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/parquet];
GO

CREATE CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net/parquet]
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D';
GO
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

| Ordnerpfad                                                  | Beschreibung                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| /csv/                                                        | Übergeordneter Ordner für Daten im CSV-Format                         |
| /csv/population/<br />/csv/population-unix/<br />/csv/population-unix-hdr/<br />/csv/population-unix-hdr-escape<br />/csv/population-unix-hdr-quoted | Ordner mit Bevölkerungsdatendateien in verschiedenen CSV-Formaten |
| /csv/taxi/                                                   | Ordner mit öffentlichen NYC-Datendateien im CSV-Format              |
| /parquet/                                                    | Übergeordneter Ordner für Daten im Parquet-Format                     |
| /parquet/taxi                                                | Öffentliche NYC-Datendateien im Parquet-Format, partitioniert nach Jahr und Monat (mithilfe des Hive-/Hadoop-Partitionierungsschemas) |
| /parquet/nested/                                             | Parquet-Beispieldateien mit geschachtelten Spalten                     |
| /json/                                                       | Übergeordneter Ordner für Daten im JSON-Format                        |
| /json/books/                                                 | JSON-Dateien mit Bücherdaten                                   |

## <a name="validation"></a>Überprüfen

Führen Sie die drei folgenden Abfragen aus, und überprüfen Sie, ob die Anmeldeinformationen korrekt erstellt werden.

> [!NOTE]
> Für alle URIs in den Beispielabfragen wird ein Speicherkonto in der Azure-Region „Europa, Norden“ verwendet. Vergewissern Sie sich, dass Sie entsprechende Anmeldeinformationen erstellt haben. Führen Sie die folgende Abfrage aus, und vergewissern Sie sich, dass das Speicherkonto aufgeführt wird.

```sql
SELECT name
FROM sys.credentials
WHERE
     name IN ( 'https://sqlondemandstorage.blob.core.windows.net/csv',
     'https://sqlondemandstorage.blob.core.windows.net/parquet',
     'https://sqlondemandstorage.blob.core.windows.net/json');
```

Sollten Sie die entsprechenden Anmeldeinformationen nicht finden, lesen Sie [Erstmalige Einrichtung](#first-time-setup).

### <a name="sample-query"></a>Beispielabfrage

Als letzter Überprüfungsschritt muss die folgende Abfrage ausgeführt werden:

```sql
SELECT
    COUNT_BIG(*)
FROM  
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet',
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
