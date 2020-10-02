---
title: 'Tutorial: Erste Schritte zum Analysieren von Daten mit SQL On-Demand'
description: In diesem Tutorial erfahren Sie, wie Sie Daten mit SQL On-Demand mithilfe von Daten analysieren, die sich in Spark-Datenbanken befinden.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 93ebc1c5e89e54f4813f270b9f8b7b13f672fbe3
ms.sourcegitcommit: 43558caf1f3917f0c535ae0bf7ce7fe4723391f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90016111"
---
# <a name="analyze-data-with-sql-on-demand"></a>Analysieren von Daten mit SQL On-Demand

In diesem Tutorial erfahren Sie, wie Sie Daten mit SQL On-Demand mithilfe von Daten analysieren, die sich in Spark-Datenbanken befinden. 

## <a name="analyze-nyc-taxi-data-in-blob-storage--using-sql-on-demand"></a>Analysieren von NYC Taxi-Daten im Blobspeicher mithilfe von SQL On-Demand

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
> [Analysieren mithilfe von Spark](get-started-analyze-spark.md)
