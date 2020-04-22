---
title: 'Schnellstart: Massenladen von Daten mit einer einzelnen T-SQL-Anweisung'
description: Massenladen von Daten mit der COPY-Anweisung
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/08/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: d39b3085a802ca0ff745ab1f63f4a8fba966ea48
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115006"
---
# <a name="quickstart-bulk-load-data-using-the-copy-statement"></a>Schnellstart: Massenladen von Daten mit der COPY-Anweisung

In dieser Schnellstartanleitung verwenden Sie das Massenladen von Daten in Ihren SQL-Pool mithilfe der einfachen und flexiblen [COPY-Anweisung](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) für die Datenerfassung mit hohem Durchsatz. Die COPY-Anweisung ist das empfohlene Ladehilfsprogramm, da sie Ihnen mit folgender Funktionalität das nahtlose und flexible Laden von Daten ermöglicht:

- Benutzer mit niedrigeren Berechtigungen können ohne strikte Berechtigungen zur STEUERUNG im Data Warehouse laden
- Nutzen einer einzelnen T-SQL-Anweisung, ohne dass zusätzliche Datenbankobjekte erstellt werden müssen
- Nutzen eines feineren Berechtigungsmodells, ohne Speicherkontoschlüssel über SAS (Share Access Signature) verfügbar zu machen
- Angeben eines anderen Speicherkontos für den ERRORFILE-Speicherort (REJECTED_ROW_LOCATION)
- Anpassen der Standardwerte für jede Zielspalte und Angeben der Quelldatenfelder, die in bestimmte Zielspalten geladen werden sollen
- Angeben eines benutzerdefinierten Zeilenabschlusszeichens für CSV-Dateien
- Escapezeichenfolge, Feld und Zeilentrennzeichen für CSV-Dateien
- Nutzen von SQL Server-Datumsformaten für CSV-Dateien
- Angeben von Platzhaltern und mehreren Dateien im Speicherortpfad

## <a name="prerequisites"></a>Voraussetzungen

In dieser Schnellstartanleitung wird vorausgesetzt, dass Sie bereits über einen SQL-Pool verfügen. Wenn ein SQL-Pool nicht erstellt wurde, verwenden Sie den [Schnellstart: Erstellen und Abfragen eines Synapse-SQL-Pools mit dem Azure-Portal](create-data-warehouse-portal.md).

## <a name="create-the-target-table"></a>Erstellen der Zieltabelle

In diesem Beispiel laden wir Daten aus dem New York-Taxi-Dataset. Wir laden eine Tabelle mit dem Namen „Trip“, die Taxifahrten darstellt, die innerhalb eines einzelnen Jahres durchgeführt wurden. Führen Sie zum Erstellen der Tabelle Folgendes aus:

```sql
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
```

## <a name="run-the-copy-statement"></a>Ausführen der COPY-Anweisung

Führen Sie die folgende Copy-Anweisung aus, mit der Daten aus dem Azure-Blobspeicherkonto in die Trip-Tabelle geladen werden.

```sql
COPY INTO [dbo].[Trip] FROM 'https://nytaxiblob.blob.core.windows.net/2013/Trip2013/'
WITH (
   FIELDTERMINATOR='|',
   ROWTERMINATOR='0x0A'
) OPTION (LABEL = 'COPY: dbo.trip');
```

## <a name="monitor-the-load"></a>Überwachen des Ladens

Überprüfen Sie, ob Ihr Ladevorgang Fortschritte macht, indem Sie die folgende Abfrage regelmäßig ausführen:

```sql
SELECT  r.[request_id]                           
,       r.[status]                               
,       r.resource_class                         
,       r.command
,       sum(bytes_processed) AS bytes_processed
,       sum(rows_processed) AS rows_processed
FROM    sys.dm_pdw_exec_requests r
              JOIN sys.dm_pdw_dms_workers w
                     ON r.[request_id] = w.request_id
WHERE [label] = 'COPY: dbo.trip' and session_id <> session_id() and type = 'WRITER'
GROUP BY r.[request_id]                           
,       r.[status]                               
,       r.resource_class                         
,       r.command;

```

## <a name="next-steps"></a>Nächste Schritte

- Bewährte Methoden zum Laden von Daten finden Sie unter [Bewährte Methoden für das Laden von Daten mit einem Synapse SQL-Pool](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/guidance-for-loading-data).
- Informationen zum Verwalten der Ressourcen für Ihre Datenladevorgänge finden Sie unter [Schnellstart: Konfigurieren der Workloadisolation mithilfe von T-SQL](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/quickstart-configure-workload-isolation-tsql). 
