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
ms.openlocfilehash: 0ef76be2c083d6185b8b919d174de28aa3b65446
ms.sourcegitcommit: af6eba1485e6fd99eed39e507896472fa930df4d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2021
ms.locfileid: "106293769"
---
# <a name="analyze-data-with-a-serverless-sql-pool"></a>Analysieren von Daten mit einem serverlosen SQL-Pool

In diesem Tutorial erfahren Sie, wie Sie Daten mit einem serverlosen SQL-Pool analysieren. 

## <a name="the-built-in-serverless-sql-pool"></a>Der serverlose SQL-Pool „Built-in“

Mit serverlosen SQL-Pools können Sie SQL verwenden, ohne dass Sie Kapazität reservieren müssen. Die Abrechnung für einen serverlosen SQL-Pool basiert auf der Menge der Daten, die zum Ausführen der Abfrage verarbeitet werden, und nicht auf der Anzahl von Knoten, die zum Ausführen der Abfrage verwendet werden.

Jeder Arbeitsbereich wird mit einem vorkonfigurierten serverlosen SQL-Pool namens **Built-in** (Integriert) bereitgestellt. 

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-serverless-sql-pool"></a>Analysieren von NYC Taxi-Daten im Blobspeicher mithilfe eines serverlosen SQL-Pools

In diesem Abschnitt verwenden Sie einen serverlosen SQL-Pool, um NYC Taxi-Daten in einem Azure Blob Storage-Konto zu analysieren.

1. Navigieren Sie in Synapse Studio zum Hub **Entwickeln**.
1. Erstellen Sie ein neues SQL-Skript.
1. Fügen Sie den folgenden Code in das Skript ein:

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

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Analysieren von Daten mit einem serverlosen Spark-Pool](get-started-analyze-spark.md)
