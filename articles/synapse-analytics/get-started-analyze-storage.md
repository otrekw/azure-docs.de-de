---
title: 'Tutorial: Erste Schritte zum Analysieren von Daten in Speicherkonten'
description: In diesem Tutorial erfahren Sie, wie Sie Daten analysieren, die sich in einem Speicherkonto befinden.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 12/31/2020
ms.openlocfilehash: f18977bb92b37546d5980134cba858b1f76b464c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104720014"
---
# <a name="analyze-data-in-a-storage-account"></a>Analysieren von Daten in einem Speicherkonto

In diesem Tutorial erfahren Sie, wie Sie Daten analysieren, die sich in einem Speicherkonto befinden.

## <a name="overview"></a>Übersicht

Bisher haben wir Szenarien behandelt, bei denen sich Daten in Datenbanken im Arbeitsbereich befunden haben. Nun zeigen wir Ihnen, wie Sie mit Dateien in Speicherkonten arbeiten. In diesem Szenario verwenden wir das primäre Speicherkonto des Arbeitsbereichs und den Container gemäß unserer Angabe bei Erstellung des Arbeitsbereichs.

* Name des Speicherkontos: **contosolake**
* Name des Containers im Speicherkonto: **users**

### <a name="create-csv-and-parquet-files-in-your-storage-account"></a>Erstellen von CSV- und Parquet-Dateien in Ihrem Speicherkonto

Führen Sie den folgenden Code in einem Notebook in einer neuen Codezelle aus. Hiermit werden eine CSV-Datei und eine Parquet-Datei im Speicherkonto erstellt.

```py
%%pyspark
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.repartition(1) # This ensure we'll get a single file during write()
df.write.mode("overwrite").csv("/NYCTaxi/PassengerCountStats_csvformat")
df.write.mode("overwrite").parquet("/NYCTaxi/PassengerCountStats_parquetformat")
```

### <a name="analyze-data-in-a-storage-account"></a>Analysieren von Daten in einem Speicherkonto

Sie können die Daten in Ihrem ADLS Gen2-Standardkonto Ihres Arbeitsbereichs analysieren, oder Sie können über **Verwalten** > **Verknüpfte Dienste** > **Neu** ein ADLS Gen2- oder Blob-Speicherkonto mit Ihrem Arbeitsbereich verknüpfen (die folgenden Schritte beziehen sich auf das primäre ADLS Gen2-Konto).

1. Navigieren Sie in Synapse Studio zum Hub **Daten**, und wählen Sie **Verknüpft** aus.
1. Navigieren Sie zu **Azure Data Lake Storage Gen2** > **myworkspace (Primär – contosolake)** .
1. Wählen Sie **Benutzer (Primär)** aus. Der Ordner **NYCTaxi** sollte angezeigt werden. Darin sollten die beiden Ordner **PassengerCountStats_csvformat** und **PassengerCountStats_parquetformat** angezeigt werden.
1. Öffnen Sie den Ordner **PassengerCountStats_parquetformat**. Darin wird eine Parquet-Datei mit einem Namen wie `part-00000-2638e00c-0790-496b-a523-578da9a15019-c000.snappy.parquet` angezeigt.
1. Klicken Sie mit der rechten Maustaste auf **.parquet**, und wählen Sie **Neues Notebook** und dann **In Datenframe laden** aus. Ein neues Notebook mit einer Zelle wie der folgenden wird erstellt:

    ```py
    %%pyspark
    abspath = 'abfss://users@contosolake.dfs.core.windows.net/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet'
    df = spark.read.load(abspath, format='parquet')
    display(df.limit(10))
    ```

1. Fügen Sie den Spark-Pool mit dem Namen **Spark1** an. Führen Sie die Zelle aus.
1. Navigieren Sie zurück zum Ordner **Benutzer**. Klicken Sie mit der rechten Maustaste erneut auf die **PARQUET**-Datei, und wählen Sie dann **Neues SQL-Skript** > **ERSTE 100 Zeilen auswählen** aus. Es wird ein SQL-Skript der folgenden Art erstellt:

    ```sql
    SELECT 
        TOP 100 *
    FROM OPENROWSET(
        BULK 'https://contosolake.dfs.core.windows.net/users/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet',
        FORMAT='PARQUET'
    ) AS [result]
    ```

    Stellen Sie sicher, dass im Skriptfenster das Feld **Verbinden mit** auf den **integrierten** serverlosen SQL-Pool festgelegt ist.

1. Führen Sie das Skript aus.



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Orchestrieren von Aktivitäten mit Pipelines](get-started-pipelines.md)
