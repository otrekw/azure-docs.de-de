---
title: 'Tutorial: Erste Schritte zum Analysieren von Daten mit dem SQL-Pool'
description: In diesem Tutorial verwenden Sie die NYC Taxi-Beispieldaten, um die Analysefunktionen des SQL-Pools zu erforschen.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: e2e1d0479b8edacaae8816d74db061eeedb805a7
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87325218"
---
# <a name="analyze-data-with-sql-pools"></a>Analysieren von Daten mit SQL-Pools

Azure Synapse Analytics bietet Ihnen die Möglichkeit, Daten mit dem SQL-Pool zu analysieren. In diesem Tutorial verwenden Sie die NYC Taxi-Beispieldaten, um die Analysefunktionen des SQL-Pools zu erforschen.

## <a name="load-the-nyc-taxi-sample-data-into-the-sqldb1-database"></a>Laden der NYC Taxi-Beispieldaten in die Datenbank SQLDB1

1. Wählen Sie in Synapse Studio im obersten blauen Menü das Fragezeichensymbol ( **?** ) aus.
1. Wählen Sie **Erste Schritte** >  **„Erste Schritte“-Hub** aus.
1. Wählen Sie auf der Karte mit der Bezeichnung **Abfragebeispieldaten** den SQL-Pool mit dem Namen **SQLDB1** aus.
1. Wählen Sie **Abfragedaten** aus. Es wird kurz eine Benachrichtigung mit dem Hinweis angezeigt, dass die Beispieldaten geladen werden. In einer hellblauen Statusleiste im oberen Bereich von Synapse Studio wird angegeben, dass Daten in SQLDB1 geladen werden.
1. Schließen Sie die Statusleiste, wenn sich ihre Farbe in Grün geändert hat.

## <a name="explore-the-nyc-taxi-data-in-the-sql-pool"></a>Untersuchen der NYC Taxi-Daten im SQL-Pool

1. Navigieren Sie in Synapse Studio zum Hub **Daten**.
1. Navigieren Sie zu **SQLDB1** > **Tabellen**. Es werden mehrere geladene Tabellen angezeigt.
1. Klicken Sie mit der rechten Maustaste auf die Tabelle **dbo.Trip**, und wählen Sie **Neues SQL-Skript** > **OBERSTE 100 Zeilen auswählen** aus.
1. Warten Sie, während ein neues SQL-Skript erstellt und ausgeführt wird.
1. Beachten Sie, dass am oberen Rand des SQL-Skripts **Verbinden mit** automatisch auf den SQL-Pool mit dem Namen **SQLDB1** festgelegt ist.
1. Ersetzen Sie den Text des SQL-Skripts durch diesen Code, und führen Sie ihn aus.

    ```sql
    SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
    FROM  dbo.Trip
    WHERE TripDistanceMiles > 0 AND PassengerCount > 0
    GROUP BY PassengerCount
    ORDER BY PassengerCount
    ```

    Diese Abfrage zeigt, wie die Gesamtzahl der Fahrtstrecken und die durchschnittliche Fahrtstrecke mit der Anzahl der Fahrgäste in Beziehung stehen.
1. Ändern Sie im Ergebnisfenster des SQL-Skripts die **Ansicht** in **Diagramm**, um eine Visualisierung der Ergebnisse als Liniendiagramm anzuzeigen.



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Analysieren mithilfe von Spark](get-started-analyze-spark.md)

