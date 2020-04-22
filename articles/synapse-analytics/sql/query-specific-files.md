---
title: Verwenden von Dateimetadaten in Abfragen
description: Die OPENROWSET-Funktion stellt Datei- und Pfadinformationen zu jeder Datei bereit, die in der Abfrage zum Filtern oder Analysieren von Daten auf der Grundlage des Dateinamens und/oder des Ordnerpfads verwendet wird.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 40a8e2c153ec3d8e7b4007340b9433a38f9ccc89
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427546"
---
# <a name="using-file-metadata-in-queries"></a>Verwenden von Dateimetadaten in Abfragen

Der SQL On-Demand-Abfragedienst kann mehrere Dateien und Ordner behandeln, wie im Artikel [Abfrageordner und mehrere Dateien](query-folders-multiple-csv-files.md) beschrieben. In diesem Artikel erfahren Sie, wie Sie Metadateninformationen zu Datei- und Ordnernamen in den Abfragen verwenden können.

Gelegentlich müssen Sie möglicherweise wissen, welche Datei- oder Ordnerquelle mit einer bestimmten Zeile im Resultset korreliert.

Sie können die Funktionen `filepath` und `filename` verwenden, um Dateinamen und/oder den Pfad im Resultset zurückzugeben. Oder Sie können sie verwenden, um Daten anhand des Dateinamens und/oder des Ordnerpfades zu filtern. Diese Funktionen werden in den Syntaxabschnitten zur [Dateinamenfunktion](develop-storage-files-overview.md#filename-function) und zur [Dateipfadfunktion](develop-storage-files-overview.md#filepath-function) beschrieben. Nachfolgend finden Sie kurze Beschreibungen zusammen mit Beispielen.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie den Rest dieses Artikels lesen, sollten Sie die folgenden Voraussetzungen überprüfen:

- [Erstmalige Einrichtung](query-data-storage.md#first-time-setup)
- [Voraussetzungen](query-data-storage.md#prerequisites)

## <a name="functions"></a>Functions

### <a name="filename"></a>Dateiname

Diese Funktion gibt den Namen der Datei zurück, aus der die Zeile stammt.

Das folgende Beispiel liest die Datendateien von „NYC Yellow Taxi“ für die letzten drei Monate des Jahres 2017 und gibt die Anzahl der Fahrten pro Datei zurück. Der OPENROWSET-Teil der Abfrage gibt an, welche Dateien gelesen werden sollen.

```sql
SELECT
    r.filename() AS [filename]
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet',
        FORMAT='PARQUET') AS [r]
GROUP BY
    r.filename()
ORDER BY
    [filename];
```

Das folgende Beispiel zeigt, wie *filename()* in der WHERE-Klausel verwendet werden kann, um die zu lesenden Dateien zu filtern. Es greift auf den gesamten Ordner im OPENROWSET-Teil der Abfrage zu und filtert Dateien in der WHERE-Klausel.

Ihre Ergebnisse werden dieselben sein wie im vorherigen Beispiel.

```sql
SELECT
    r.filename() AS [filename]
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
    BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet',
    FORMAT='PARQUET') AS [r]
WHERE
    r.filename() IN ('yellow_tripdata_2017-10.parquet', 'yellow_tripdata_2017-11.parquet', 'yellow_tripdata_2017-12.parquet')
GROUP BY
    r.filename()
ORDER BY
    [filename];
```

### <a name="filepath"></a>Filepath

Die Dateipfadfunktion (filepath) gibt einen vollständigen oder teilweisen Pfad zurück:

- Beim Aufruf ohne Parameter wird der vollständige Pfad einer Datei zurückgegeben, aus der die Zeile stammt.
- Beim Aufruf mit Parameter wird ein Teil eines Pfads zurückgegeben, der dem Platzhalterzeichen an der im Parameter angegebenen Position entspricht. Der Parameterwert 1 würde z. B. den Teil eines Pfads zurückgeben, der dem ersten Platzhalterzeichen entspricht.

Das folgende Beispiel liest die Datendateien von „NYC Yellow Taxi“ für die letzten drei Monate des Jahres 2017. Es gibt die Anzahl der Fahrten pro Dateipfad zurück. Der OPENROWSET-Teil der Abfrage gibt an, welche Dateien gelesen werden sollen.

```sql
SELECT
    r.filepath() AS filepath
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/taxi/yellow_tripdata_2017-1*.csv',
        FORMAT = 'CSV',
        FIRSTROW = 2
    )
    WITH (
        vendor_id INT,
        pickup_datetime DATETIME2,
        dropoff_datetime DATETIME2,
        passenger_count SMALLINT,
        trip_distance FLOAT,
        rate_code SMALLINT,
        store_and_fwd_flag SMALLINT,
        pickup_location_id INT,
        dropoff_location_id INT,
        payment_type SMALLINT,
        fare_amount FLOAT,
        extra FLOAT,
        mta_tax FLOAT,
        tip_amount FLOAT,
        tolls_amount FLOAT,
        improvement_surcharge FLOAT,
        total_amount FLOAT
    ) AS [r]
GROUP BY
    r.filepath()
ORDER BY
    filepath;
```

Das folgende Beispiel zeigt, wie *filepath()* in der WHERE-Klausel verwendet werden kann, um die zu lesenden Dateien zu filtern.

Sie können die Platzhalterzeichen im OPENROWSET-Teil der Abfrage verwenden und die Dateien in der WHERE-Klausel filtern. Ihre Ergebnisse werden dieselben sein wie im vorherigen Beispiel.

```sql
SELECT
    r.filepath() AS filepath
    ,r.filepath(1) AS [year]
    ,r.filepath(2) AS [month]
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/taxi/yellow_tripdata_*-*.csv',
        FORMAT = 'CSV',
        FIRSTROW = 2
    )
WITH (
    vendor_id INT,
    pickup_datetime DATETIME2,
    dropoff_datetime DATETIME2,
    passenger_count SMALLINT,
    trip_distance FLOAT,
    rate_code SMALLINT,
    store_and_fwd_flag SMALLINT,
    pickup_location_id INT,
    dropoff_location_id INT,
    payment_type SMALLINT,
    fare_amount FLOAT,
    extra FLOAT,
    mta_tax FLOAT,
    tip_amount FLOAT,
    tolls_amount FLOAT,
    improvement_surcharge FLOAT,
    total_amount FLOAT
) AS [r]
WHERE
    r.filepath(1) IN ('2017')
    AND r.filepath(2) IN ('10', '11', '12')
GROUP BY
    r.filepath()
    ,r.filepath(1)
    ,r.filepath(2)
ORDER BY
    filepath;
```

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie [Parquet-Dateien abfragen](query-parquet-files.md) können.
