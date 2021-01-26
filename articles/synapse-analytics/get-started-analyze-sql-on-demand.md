---
title: 'Tutorial: Erste Schritte beim Analysieren von Daten mit einem serverlosen SQL-Pool'
description: In diesem Tutorial erfahren Sie, wie Sie Daten mit einem serverlosen SQL-Pool analysieren und dabei Daten in Spark-Datenbanken verwenden.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: tutorial
ms.date: 12/31/2020
ms.openlocfilehash: c9f8760bd1a7b5d3700f3fdf03331fe7013e116f
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98209405"
---
# <a name="analyze-data-with-serverless-sql-pool-in-azure-synapse-analytics"></a>Analysieren von Daten mit einem serverlosen SQL-Pool in Azure Synapse Analytics

In diesem Tutorial erfahren Sie, wie Sie Daten mit einem serverlosen SQL-Pool analysieren und dabei Daten in Spark-Datenbanken verwenden. 

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-serverless-sql-pool"></a>Analysieren von NYC Taxi-Daten im Blobspeicher mithilfe eines serverlosen SQL-Pools

1. Klicken Sie im Hub **Daten** unter **Verknüpft** mit der rechten Maustaste auf **Azure Blob Storage > Beispieldatasets > nyc_tlc_yellow**, und wählen Sie **Neues SQL-Skript** und anschließend **ERSTE 100 Zeilen auswählen** aus.
1. Dadurch wird ein neues SQL-Skript mit dem folgendem Code erstellt:

    ```
    -- This is auto-generated code
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK     'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
            FORMAT = 'parquet'
        ) AS [result];
    ```
1. Klicken Sie auf **Run**.

## <a name="analyze-nyc-taxi-data-in-spark-databases-using-serverless-sql-pool"></a>Analysieren von NYC Taxi-Daten in Spark-Datenbanken mithilfe eines serverlosen SQL-Pools

Tabellen in Spark-Datenbanken sind automatisch sichtbar und können von einem serverlosen SQL-Pool abgefragt werden.

1. Navigieren Sie in Synapse Studio zum Hub **Entwickeln**, und erstellen Sie ein neues SQL-Skript.
1. Legen Sie die Option **Verbinden mit**  auf den **integrierten** serverlosen SQL-Pool fest.
1. Fügen Sie den folgenden Text in das Skript ein, und führen Sie das Skript aus.

    ```sql
    SELECT *
    FROM nyctaxi.dbo.passengercountstats
    ```

    > [!NOTE]
    > Beim erstmaligen Ausführen einer Abfrage, die einen serverlosen SQL-Pool nutzt, dauert es ungefähr zehn Sekunden, bis der serverlose SQL-Pool die SQL-Ressourcen gesammelt hat, die zum Ausführen Ihrer Abfragen benötigt werden. Für nachfolgende Abfragen wird dann deutlich weniger Zeit benötigt.
  


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Analysieren von Daten im Speicher](get-started-analyze-storage.md)
