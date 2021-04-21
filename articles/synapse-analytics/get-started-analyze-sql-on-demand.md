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
ms.date: 04/15/2021
ms.openlocfilehash: c6f2dfe0d4846227400ac9b3c7ac3e6ead8f0b57
ms.sourcegitcommit: 590f14d35e831a2dbb803fc12ebbd3ed2046abff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107567552"
---
# <a name="analyze-data-with-a-serverless-sql-pool"></a>Analysieren von Daten mit einem serverlosen SQL-Pool

In diesem Tutorial erfahren Sie, wie Sie Daten mit einem serverlosen SQL-Pool analysieren. 

## <a name="the-built-in-serverless-sql-pool"></a>Der serverlose SQL-Pool „Built-in“

Mit serverlosen SQL-Pools können Sie SQL verwenden, ohne dass Sie Kapazität reservieren müssen. Die Abrechnung für einen serverlosen SQL-Pool basiert auf der Menge der Daten, die zum Ausführen der Abfrage verarbeitet werden, und nicht auf der Anzahl von Knoten, die zum Ausführen der Abfrage verwendet werden.

Jeder Arbeitsbereich wird mit einem vorkonfigurierten serverlosen SQL-Pool namens **Built-in** (Integriert) bereitgestellt. 

## <a name="analyze-nyc-taxi-data-with-a-serverless-sql-pool"></a>Analysieren von NYC Taxi-Daten mit einem serverlosen SQL-Pool

1. Navigieren Sie in Synapse Studio zum Hub **Entwickeln**.
1. Erstellen Sie ein neues SQL-Skript.
1. Fügen Sie den folgenden Code in das Skript ein:

    ```
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK 'https://contosolake.dfs.core.windows.net/users/NYCTripSmall.parquet',
            FORMAT='PARQUET'
        ) AS [result]
    ```
1. Klicken Sie auf **Ausführen**. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Analysieren von Daten mit einem serverlosen Spark-Pool](get-started-analyze-spark.md)
