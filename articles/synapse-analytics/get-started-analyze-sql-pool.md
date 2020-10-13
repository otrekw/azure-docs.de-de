---
title: 'Tutorial: Erste Schritte beim Analysieren von Daten mit dedizierten SQL-Pools'
description: In diesem Tutorial verwenden Sie die NYC Taxi-Beispieldaten, um die Analysefunktionen des SQL-Pools zu erforschen.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 11c11d3e43f15b569a74c282b15a95525cea7e7c
ms.sourcegitcommit: 4bebbf664e69361f13cfe83020b2e87ed4dc8fa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2020
ms.locfileid: "91620034"
---
# <a name="analyze-data-with-dedicated-sql-pools"></a>Analysieren von Daten mit dedizierten SQL-Pools

Azure Synapse Analytics ermöglicht das Analysieren von Daten mit einem dedizierten SQL-Pool. In diesem Tutorial werden die NYC Taxi-Daten verwendet, um die Analysefunktionen eines dedizierten SQL-Pools zu untersuchen.

## <a name="load-the-nyc-taxi-data-into-sqldb1"></a>Laden der NYC Taxi-Daten in SQLDB1

1. Navigieren Sie in Synapse Studio zum Hub **Entwickeln**, und erstellen Sie ein neues SQL-Skript.
1. Geben Sie den folgenden Code ein:
    ```
    CREATE TABLE [dbo].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    );

    COPY INTO [dbo].[Trip]
    FROM 'https://nytaxiblob.blob.core.windows.net/2013/Trip2013/QID6392_20171107_05910_0.txt.gz'
    WITH
    (
        FILE_TYPE = 'CSV',
        FIELDTERMINATOR = '|',
        FIELDQUOTE = '',
        ROWTERMINATOR='0X0A',
        COMPRESSION = 'GZIP'
    )
    OPTION (LABEL = 'COPY : Load [dbo].[Trip] - Taxi dataset');
    ```
1. Die Ausführung dieses Skripts dauert ungefähr eine Minute. Es lädt zwei Millionen Zeilen NYC Taxi-Daten in eine Tabelle mit dem Namen **dbo.Trip**.

## <a name="explore-the-nyc-taxi-data-in-the-dedicated-sql-pool"></a>Untersuchen der NYC Taxi-Daten im dedizierten SQL-Pool

1. Navigieren Sie in Synapse Studio zum Hub **Daten**.
1. Navigieren Sie zu **SQLDB1** > **Tabellen**. Es werden mehrere geladene Tabellen angezeigt.
1. Klicken Sie mit der rechten Maustaste auf die Tabelle **dbo.Trip**, und wählen Sie **Neues SQL-Skript** > **OBERSTE 100 Zeilen auswählen** aus.
1. Warten Sie, während ein neues SQL-Skript erstellt und ausgeführt wird.
1. Beachten Sie, dass am oberen Rand des SQL-Skripts **Verbinden mit** automatisch auf den SQL-Pool mit dem Namen **SQLDB1** festgelegt ist.
1. Ersetzen Sie den Text des SQL-Skripts durch diesen Code, und führen Sie ihn aus.

    ```sql
    SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
    FROM  dbo.Trip
    WHERE TripDistanceMiles > 0 AND PassengerCount > 0
    GROUP BY PassengerCount
    ORDER BY PassengerCount
    ```

    Diese Abfrage zeigt, wie die Gesamtzahl der Fahrtstrecken und die durchschnittliche Fahrtstrecke mit der Anzahl der Fahrgäste in Beziehung stehen.
1. Ändern Sie im Ergebnisfenster des SQL-Skripts die **Ansicht** in **Diagramm**, um eine Visualisierung der Ergebnisse als Liniendiagramm anzuzeigen.



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Analysieren mithilfe von Spark](get-started-analyze-spark.md)

