---
title: Abfragen von Ordnern und mehreren CSV-Dateien mit SQL On-Demand (Vorschauversion)
description: SQL On-Demand (Vorschauversion) unterstützt das Lesen mehrerer Dateien/Ordner durch die Verwendung von Platzhalterzeichen, die den in Windows-Betriebssystemen verwendeten Platzhalterzeichen ähnlich sind.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 8f8af7fab7113e38b91c3f5f1bcc41b4e4fba2c1
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457364"
---
# <a name="query-folders-and-multiple-csv-files"></a>Abfragen von Ordnern und mehreren CSV-Dateien  

In diesem Artikel erfahren Sie, wie Sie eine Abfrage mit SQL On-Demand (Vorschauversion) in Azure Synapse Analytics schreiben können.

SQL On-Demand unterstützt das Lesen mehrerer Dateien/Ordner durch die Verwendung von Platzhalterzeichen, die den in Windows-Betriebssystemen verwendeten Platzhalterzeichen ähnlich sind. Allerdings ist die Flexibilität größer, da mehrere Platzhalterzeichen erlaubt sind.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie den Rest dieses Artikels lesen, überprüfen Sie die unten aufgeführten Artikel:

- [Erstmalige Einrichtung](query-data-storage.md#first-time-setup)
- [Voraussetzungen](query-data-storage.md#prerequisites)

## <a name="read-multiple-files-in-folder"></a>Lesen mehrerer Dateien im Ordner

Sie verwenden den Ordner *csv/taxi*, um die Beispielabfragen nachzuverfolgen. Er enthält Daten der Fahrtenaufzeichnungen für „NYC Taxi – Yellow Taxi“ von Juli 2016 bis Juni 2018.

Dateien in *csv/taxi* sind nach Jahr und Monat benannt:

- yellow_tripdata_2016-07.csv
- yellow_tripdata_2016-08.csv
- yellow_tripdata_2016-09.csv
- ...
- yellow_tripdata_2018-04.csv
- yellow_tripdata_2018-05.csv
- yellow_tripdata_2018-06.csv

Die einzelnen Dateien haben die folgende Struktur:
        
    [First 10 rows of the CSV file](./media/querying-folders-and-multiple-csv-files/nyc-taxi.png)

## <a name="read-all-files-in-folder"></a>Lesen aller Dateien im Ordner
    
Das folgende Beispiel liest alle Datendateien von „NYC Yellow Taxi“ aus dem Ordner *csv/taxi* und gibt die Gesamtzahl der Fahrgäste und Fahrten pro Jahr zurück. Außerdem wird die Verwendung von Aggregatfunktionen veranschaulicht.

```sql
SELECT 
    YEAR(pickup_datetime) as [year],
    SUM(passenger_count) AS passengers_total,
    COUNT(*) AS [rides_total]
FROM OPENROWSET(
    BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/taxi/*.*',
        FORMAT = 'CSV', 
        FIRSTROW = 2
    )
    WITH (
        vendor_id VARCHAR(100) COLLATE Latin1_General_BIN2, 
        pickup_datetime DATETIME2, 
        dropoff_datetime DATETIME2,
        passenger_count INT,
           trip_distance FLOAT,
        rate_code INT,
        store_and_fwd_flag VARCHAR(100) COLLATE Latin1_General_BIN2,
        pickup_location_id INT,
        dropoff_location_id INT,
           payment_type INT,
        fare_amount FLOAT,
        extra FLOAT,
        mta_tax FLOAT,
        tip_amount FLOAT,
        tolls_amount FLOAT,
        improvement_surcharge FLOAT,
        total_amount FLOAT
    ) AS nyc
GROUP BY
    YEAR(pickup_datetime)
ORDER BY
    YEAR(pickup_datetime);
```

> [!NOTE]
> Alle Dateien, auf die mit dem einzelnen OPENROWSET zugegriffen wird, müssen dieselbe Struktur aufweisen (d. h. Anzahl der Spalten und Typ der Daten).

### <a name="read-subset-of-files-in-folder"></a>Lesen einer Teilmenge der Dateien im Ordner

Das folgende Beispiel liest die Datendateien von „NYC Yellow Taxi“ für 2017 aus dem Ordner *csv/taxi* unter Verwendung eines Platzhalterzeichens und gibt den Gesamtfahrpreis pro Zahlungsart zurück.

```sql
SELECT 
    payment_type,  
    SUM(fare_amount) AS fare_total
FROM OPENROWSET(
    BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/taxi/yellow_tripdata_2017-*.csv',
        FORMAT = 'CSV', 
        FIRSTROW = 2
    )
    WITH (
        vendor_id VARCHAR(100) COLLATE Latin1_General_BIN2, 
        pickup_datetime DATETIME2, 
        dropoff_datetime DATETIME2,
        passenger_count INT,
        trip_distance FLOAT,
        rate_code INT,
        store_and_fwd_flag VARCHAR(100) COLLATE Latin1_General_BIN2,
        pickup_location_id INT,
        dropoff_location_id INT,
        payment_type INT,
        fare_amount FLOAT,
        extra FLOAT,
        mta_tax FLOAT,
        tip_amount FLOAT,
        tolls_amount FLOAT,
        improvement_surcharge FLOAT,
        total_amount FLOAT
    ) AS nyc
GROUP BY payment_type
ORDER BY payment_type;
```

> [!NOTE]
> Alle Dateien, auf die mit dem einzelnen OPENROWSET zugegriffen wird, müssen dieselbe Struktur aufweisen (d. h. Anzahl der Spalten und Typ der Daten).

## <a name="read-folders"></a>Lesen von Ordnern

Der Pfad, den Sie für OPENROWSET bereitstellen, kann auch ein Pfad zu einem Ordner sein. Diese Abfragetypen sind in den folgenden Abschnitten enthalten.

### <a name="read-all-files-from-specific-folder"></a>Lesen aller Dateien aus einem bestimmten Ordner

Sie können alle Dateien in einem Ordner lesen, indem Sie das Platzhalterzeichen auf Dateiebene verwenden, wie in [Lesen aller Dateien im Ordner](#read-all-files-in-folder) gezeigt. Es gibt jedoch eine Möglichkeit, einen Ordner abzufragen und alle Dateien in diesem Ordner zu verwenden.

Wenn der in OPENROWSET angegebene Pfad auf einen Ordner verweist, werden alle Dateien in diesem Ordner als Quelle für die Abfrage verwendet. Mit der folgenden Abfrage werden alle Dateien im Ordner *csv/taxi* gelesen.

> [!NOTE]
> Beachten Sie die Angabe von „/“ am Pfadende in der folgenden Abfrage. Es bezeichnet einen Ordner. Wenn „/“ ausgelassen wird, wird die Abfrage stattdessen auf eine Datei mit dem Namen *taxi* ausgerichtet.

```sql
SELECT
    YEAR(pickup_datetime) as [year],
    SUM(passenger_count) AS passengers_total,
    COUNT(*) AS [rides_total]
FROM OPENROWSET(
    BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/taxi/',
        FORMAT = 'CSV', 
        FIRSTROW = 2
    )
    WITH (
        vendor_id VARCHAR(100) COLLATE Latin1_General_BIN2, 
        pickup_datetime DATETIME2, 
        dropoff_datetime DATETIME2,
        passenger_count INT,
        trip_distance FLOAT,
        rate_code INT,
        store_and_fwd_flag VARCHAR(100) COLLATE Latin1_General_BIN2,
        pickup_location_id INT,
        dropoff_location_id INT,
        payment_type INT,
        fare_amount FLOAT,
        extra FLOAT,
        mta_tax FLOAT,
        tip_amount FLOAT,
        tolls_amount FLOAT,
        improvement_surcharge FLOAT,
        total_amount FLOAT
    ) AS nyc
GROUP BY
    YEAR(pickup_datetime)
ORDER BY
    YEAR(pickup_datetime);
```

> [!NOTE]
> Alle Dateien, auf die mit dem einzelnen OPENROWSET zugegriffen wird, müssen dieselbe Struktur aufweisen (d. h. Anzahl der Spalten und Typ der Daten).

### <a name="read-all-files-from-multiple-folders"></a>Lesen aller Dateien aus mehreren Ordnern

Es ist möglich, Dateien aus mehreren Ordnern zu lesen, indem Sie ein Platzhalterzeichen verwenden. Die folgende Abfrage liest alle Dateien aus allen Ordnern, die sich im Ordner *csv* befinden und deren Namen mit *t* beginnen und mit *i* enden.

> [!NOTE]
> Beachten Sie die Angabe von „/“ am Pfadende in der folgenden Abfrage. Es bezeichnet einen Ordner. Wenn „/“ ausgelassen wird, wird die Abfrage stattdessen auf Dateien mit dem Namen *t&ast;i* ausgerichtet.

```sql
SELECT
    YEAR(pickup_datetime) as [year],
    SUM(passenger_count) AS passengers_total,
    COUNT(*) AS [rides_total]
FROM OPENROWSET(
    BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/t*i/', 
        FORMAT = 'CSV', 
        FIRSTROW = 2
    )
    WITH (
        vendor_id VARCHAR(100) COLLATE Latin1_General_BIN2, 
        pickup_datetime DATETIME2, 
        dropoff_datetime DATETIME2,
        passenger_count INT,
        trip_distance FLOAT,
        rate_code INT,
        store_and_fwd_flag VARCHAR(100) COLLATE Latin1_General_BIN2,
        pickup_location_id INT,
        dropoff_location_id INT,
        payment_type INT,
        fare_amount FLOAT,
        extra FLOAT,
        mta_tax FLOAT,
        tip_amount FLOAT,
        tolls_amount FLOAT,
        improvement_surcharge FLOAT,
        total_amount FLOAT
    ) AS nyc
GROUP BY
    YEAR(pickup_datetime)
ORDER BY
    YEAR(pickup_datetime);
```

> [!NOTE]
> Alle Dateien, auf die mit dem einzelnen OPENROWSET zugegriffen wird, müssen dieselbe Struktur aufweisen (d. h. Anzahl der Spalten und Typ der Daten).

Da Sie nur über einen Ordner verfügen, der den Kriterien entspricht, ist das Abfrageergebnis dasselbe wie unter [Lesen aller Dateien im Ordner](#read-all-files-in-folder).

## <a name="multiple-wildcards"></a>Mehrere Platzhalterzeichen

Sie können mehrere Platzhalterzeichen auf verschiedenen Pfadebenen verwenden. Sie können z. B. vorherige Abfragen ergänzen, um Dateien nur mit Daten von 2017 zu lesen, und zwar aus allen Ordnern, deren Namen mit *t* beginnen und mit *i* enden.

> [!NOTE]
> Beachten Sie die Angabe von „/“ am Pfadende in der folgenden Abfrage. Es bezeichnet einen Ordner. Wenn „/“ ausgelassen wird, wird die Abfrage stattdessen auf Dateien mit dem Namen *t&ast;i* ausgerichtet.
> Es gibt eine Obergrenze von zehn Platzhalterzeichen pro Abfrage.

```sql
SELECT
    YEAR(pickup_datetime) as [year],
    SUM(passenger_count) AS passengers_total,
    COUNT(*) AS [rides_total]
FROM OPENROWSET(
    BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/t*i/yellow_tripdata_2017-*.csv',
        FORMAT = 'CSV', 
        FIRSTROW = 2
    )
    WITH (
        vendor_id VARCHAR(100) COLLATE Latin1_General_BIN2, 
        pickup_datetime DATETIME2, 
        dropoff_datetime DATETIME2,
        passenger_count INT,
        trip_distance FLOAT,
        rate_code INT,
        store_and_fwd_flag VARCHAR(100) COLLATE Latin1_General_BIN2,
        pickup_location_id INT,
        dropoff_location_id INT,
        payment_type INT,
        fare_amount FLOAT,
        extra FLOAT,
        mta_tax FLOAT,
        tip_amount FLOAT,
        tolls_amount FLOAT,
        improvement_surcharge FLOAT,
        total_amount FLOAT
    ) AS nyc
GROUP BY
    YEAR(pickup_datetime)
ORDER BY
    YEAR(pickup_datetime);
```

> [!NOTE]
> Alle Dateien, auf die mit dem einzelnen OPENROWSET zugegriffen wird, müssen dieselbe Struktur aufweisen (d. h. Anzahl der Spalten und Typ der Daten).

Da Sie nur über einen Ordner verfügen, der den Kriterien entspricht, ist das Abfrageergebnis dasselbe wie unter [Lesen einer Teilmenge von Dateien im Ordner](#read-subset-of-files-in-folder) und [Lesen aller Dateien aus einem bestimmten Ordner](#read-all-files-from-specific-folder). Komplexere Szenarien für die Verwendung von Platzhalterzeichen werden unter [Abfragen von Parquet-Dateien](query-parquet-files.md) behandelt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie im Artikel [Abfragen von bestimmten Dateien](query-specific-files.md).
