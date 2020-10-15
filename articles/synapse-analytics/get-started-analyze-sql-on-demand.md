---
title: 'Tutorial: Erste Schritte bei Analysieren von Daten mit SQL ohne Server'
description: In diesem Tutorial erfahren Sie, wie Sie Daten mit SQL On-Demand mithilfe von Daten analysieren, die sich in Spark-Datenbanken befinden.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 9c42d1d988bc280d5e62c24f109225d91cb446ce
ms.sourcegitcommit: b437bd3b9c9802ec6430d9f078c372c2a411f11f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91893491"
---
# <a name="analyze-data-with-sql-on-demand"></a>Analysieren von Daten mit SQL On-Demand

In diesem Tutorial erfahren Sie, wie Sie Daten mit SQL ohne Server analysieren und dabei einen On-Demand-SQL-Pool sowie Daten in Spark-Datenbanken verwenden. 

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-sql-on-demand-pool"></a>Analysieren von NYC Taxi-Daten im Blobspeicher mithilfe eines SQL On-Demand-Pools

1. Klicken Sie im Hub **Daten** unter **Verknüpft** mit der rechten Maustaste auf **Azure Blob Storage > Beispieldatasets > nyc_tlc_yellow**, und wählen Sie **OBERSTE 100 Zeilen auswählen** aus.
1. Dadurch wird ein neues SQL-Skript mit dem folgendem Code erstellt:

    ```
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK     'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
            FORMAT = 'parquet'
        ) AS [result];
    ```
1. Klicken Sie auf **Run**.

## <a name="analyze-nyc-taxi-data-in-spark-databases-using-sql-on-demand"></a>Analysieren von NYC Taxi-Daten in Spark-Datenbanken mithilfe von SQL On-Demand

Tabellen in Spark-Datenbanken sind automatisch sichtbar und können von SQL On-Demand abgefragt werden.

1. Navigieren Sie in Synapse Studio zum Hub **Entwickeln**, und erstellen Sie ein neues SQL-Skript.
1. Legen Sie **Verbinden mit** auf **SQL On-Demand** fest.
1. Fügen Sie den folgenden Text in das Skript ein, und führen Sie das Skript aus.

    ```sql
    SELECT *
    FROM nyctaxi.dbo.passengercountstats
    ```

    > [!NOTE]
    > Beim erstmaligen Ausführen einer Abfrage mit Verwendung von SQL On-Demand dauert es ungefähr zehn Sekunden, bis SQL On-Demand die SQL-Ressourcen gesammelt hat, die zum Ausführen Ihrer Abfragen benötigt werden. Für nachfolgende Abfragen wird dann deutlich weniger Zeit benötigt.
  


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Analysieren von Daten im Speicher](get-started-analyze-storage.md)
