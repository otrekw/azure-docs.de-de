---
title: 'Tutorial: Erste Schritte zum Analysieren von Daten in Speicherkonten'
description: In diesem Tutorial erfahren Sie, wie Sie Daten analysieren, die sich in einem Speicherkonto befinden.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 836e56884659c60c129eba0bb5505eddd9981283
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87093542"
---
# <a name="analyze-data-in-a-storage-account"></a>Analysieren von Daten in einem Speicherkonto

In diesem Tutorial erfahren Sie, wie Sie Daten analysieren, die sich in einem Speicherkonto befinden.

## <a name="overview"></a>Übersicht

Bisher haben wir Szenarien behandelt, bei denen sich Daten in Datenbanken im Arbeitsbereich befunden haben. Nun zeigen wir Ihnen, wie Sie mit Dateien in Speicherkonten arbeiten. In diesem Szenario verwenden wir das primäre Speicherkonto des Arbeitsbereichs und den Container gemäß unserer Angabe bei Erstellung des Arbeitsbereichs.

* Name des Speicherkontos: **contosolake**
* Name des Containers im Speicherkonto: **users**

### <a name="create-csv-and-parquet-files-in-your-storage-account"></a>Erstellen von CSV- und Parquet-Dateien in Ihrem Speicherkonto

Führen Sie den folgenden Code in einem Notebook aus. Hiermit werden eine CSV-Datei und eine Parquet-Datei im Speicherkonto erstellt.

```py
%%pyspark
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.repartition(1) # This ensure we'll get a single file during write()
df.write.mode("overwrite").csv("/NYCTaxi/PassengerCountStats.csv")
df.write.mode("overwrite").parquet("/NYCTaxi/PassengerCountStats.parquet")
```

### <a name="analyze-data-in-a-storage-account"></a>Analysieren von Daten in einem Speicherkonto

1. Navigieren Sie in Synapse Studio zum Hub **Daten**, und wählen Sie **Verknüpft** aus.
1. Navigieren Sie zu **Speicherkonten** > **myworkspace (Primär – contosolake)** .
1. Wählen Sie **Benutzer (Primär)** aus. Der Ordner **NYCTaxi** sollte angezeigt werden. Darin sollten die beiden Ordner **PassengerCountStats.csv** und **PassengerCountStats.parquet** angezeigt werden.
1. Öffnen Sie den Ordner **PassengerCountStats.parquet**. Darin wird eine Parquet-Datei mit einem Namen wie `part-00000-2638e00c-0790-496b-a523-578da9a15019-c000.snappy.parquet` angezeigt.
1. Klicken Sie mit der rechten Maustaste auf **.parquet**, und wählen Sie dann die Option **Neues Notebook** aus. Es wird ein Notebook mit einer Zelle der folgenden Art erstellt:

    ```py
    %%pyspark
    data_path = spark.read.load('abfss://users@contosolake.dfs.core.windows.net/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet', format='parquet')
    data_path.show(100)
    ```

1. Führen Sie die Zelle aus.
1. Klicken Sie mit der rechten Maustaste auf die darin enthaltene Parquet-Datei, und wählen Sie dann **Neues SQL-Skript** > **OBERSTE 100 Zeilen auswählen** aus. Es wird ein SQL-Skript der folgenden Art erstellt:

    ```sql
    SELECT TOP 100 *
    FROM OPENROWSET(
        BULK 'https://contosolake.dfs.core.windows.net/users/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet',
        FORMAT='PARQUET'
    ) AS [r];
    ```

     Im Skript ist das Feld **Anfügen an** auf **SQL On-Demand** festgelegt.

1. Führen Sie das Skript aus.



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Orchestrieren von Aktivitäten mit Pipelines](get-started-pipelines.md)
