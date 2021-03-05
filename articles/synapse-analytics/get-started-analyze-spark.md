---
title: 'Schnellstart: Erste Schritte beim Analysieren mit Spark'
description: In diesem Tutorial erfahren Sie, wie Sie Daten mit Apache Spark analysieren.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: spark
ms.topic: tutorial
ms.date: 12/31/2020
ms.openlocfilehash: 7410fcbc890780281763a91f33525562e9de1853
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102182484"
---
# <a name="analyze-with-apache-spark"></a>Analysieren mit Apache Spark

In diesem Tutorial werden die grundlegenden Schritte zum Laden und Analysieren von Daten mit Apache Spark für Azure Synapse beschrieben.

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-spark"></a>Analysieren von NYC Taxi-Daten im Blobspeicher mithilfe von Spark

1. Klicken Sie im Hub **Daten** auf die Schaltfläche **+** (**Neue Ressource hinzufügen**) und anschließend auf **Katalog durchsuchen**. 
1. Suchen Sie nach **NYC Taxi and Limousine Commission – Fahrtenaufzeichnungen für „Yellow Taxi“** , und klicken Sie darauf. 
1. Klicken Sie am unteren Rand der Seite auf **Weiter** und anschließend auf **Dataset hinzufügen**. 
1. Klicken Sie nach einem kurzen Moment im Hub **Daten** unter **Verknüpft** mit der rechten Maustaste auf **Azure Blob Storage > Beispieldatasets > nyc_tlc_yellow**, und wählen Sie **Neues Notebook** und dann **In Datenrahmen laden** aus.
1. Dadurch wird ein neues Notebook mit dem folgendem Code erstellt:
    ```

    from azureml.opendatasets import NycTlcYellow

    data = NycTlcYellow()
    df = data.to_spark_dataframe()
    # Display 10 rows
    display(df.limit(10))
    ```
1. Wählen Sie im Notebook im Menü **Anfügen an** den zuvor erstellten serverlosen Spark-Pool **Spark1** aus.
1. Wählen Sie für die Zelle **Ausführen** aus.
1. Wenn Sie nur das Schema des Datenrahmens anzeigen möchten, führen Sie eine Zelle mit dem folgenden Code aus:
    ```

    df.printSchema()
    ```

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>Laden der NYC Taxi-Daten in die Spark-Datenbank „nyctaxi“

In einer Tabelle in **SQLPOOL1** sind Daten verfügbar. Laden Sie sie in eine Spark-Datenbank mit dem Namen **nyctaxi**.

1. Navigieren Sie in Synapse Studio zum Hub **Entwickeln**.
1. Wählen Sie **+**  > **Notebook** aus.
1. Legen Sie am oberen Rand des Notebooks den Wert von **Anfügen an** auf **Spark1** fest.
1. Geben Sie in der ersten Codezelle des neuen Notebooks den folgenden Code ein:


    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLPOOL1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```


1. Führen Sie das Skript aus. Der Vorgang kann zwei bis drei Minuten dauern.
1. Klicken Sie im Hub **Daten** auf der Registerkarte **Arbeitsbereich** mit der rechten Maustaste auf **Datenbanken**, und wählen Sie dann **Aktualisieren** aus. Nun sollte die Datenbank **nyctaxi (Spark)** in der Liste angezeigt werden.


## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Analysieren der NYC Taxi-Daten mithilfe von Spark und Notebooks

1. Wechseln Sie zurück zu Ihrem Notebook.
1. Erstellen Sie eine neue Codezelle, und geben Sie den folgenden Code ein: 


   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Führen Sie die Zelle aus, um die NYC Taxi-Daten anzuzeigen, die Sie in die Spark-Datenbank **nyctaxi** geladen haben.
1. Erstellen Sie eine neue Codezelle, und geben Sie den folgenden Code ein: Führen Sie die folgende Zelle aus, um dieselbe Analyse durchzuführen, die Sie zuvor mit dem dedizierten SQL-Pool **SQLPOOL1** vorgenommen haben. Mit diesem Code werden die Ergebnisse der Analyse in einer Tabelle mit dem Namen **nyctaxi.passengercountstats** gespeichert und angezeigt.


   ```py
   %%pyspark
   df = spark.sql("""
      SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
      FROM nyctaxi.trip
      WHERE TripDistanceMiles > 0 AND PassengerCount > 0
      GROUP BY PassengerCount
      ORDER BY PassengerCount
   """) 
   display(df)
   df.write.saveAsTable("nyctaxi.passengercountstats")
   ```

1. Wählen Sie in den Zellenergebnissen **Diagramm** aus, um die visualisierten Daten anzuzeigen.

## <a name="load-data-from-a-spark-table-into-a-dedicated-sql-pool-table"></a>Laden von Daten aus einer Spark-Tabelle in eine Tabelle mit dedizierten SQL-Pools

Weiter oben haben Sie Daten aus der Tabelle mit den dedizierten SQL-Pools **SQLPOOL1.dbo.Trip** in die Spark-Tabelle **nyctaxi.trip** kopiert. Anschließend haben Sie die Daten in der Spark-Tabelle **nyctaxi.passengercountstats** aggregiert. Nun kopieren Sie die Daten aus **nyctaxi.passengercountstats** in eine Tabelle in einem dedizierten SQL-Pool mit dem Namen **SQLPOOL1.dbo.PassengerCountStats**.

1. Erstellen Sie eine neue Codezelle, und geben Sie den folgenden Code ein: Führen Sie die Zelle in Ihrem Notebook aus. Hiermit wird die aggregierte Spark-Tabelle zurück in die Tabelle mit dedizierten SQL-Pools kopiert.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLPOOL1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Analysieren von Daten mit einem serverlosen SQL-Pool](get-started-analyze-sql-on-demand.md)
