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
ms.openlocfilehash: 7c228bfe5897b45e6345234f2ed8e0f5cfbec73a
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107312789"
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
1. Klicken Sie auf **Run**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Analysieren von Daten mit einem serverlosen Spark-Pool](get-started-analyze-spark.md)
