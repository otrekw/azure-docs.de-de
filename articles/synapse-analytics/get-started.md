---
title: 'Tutorial: Erste Schritte mit Azure Synapse Analytics'
description: Schrittweise Anleitung zum schnellen Verständnis grundlegender Konzepte in Azure Synapse
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: quickstart
ms.date: 05/19/2020
ms.openlocfilehash: dcad90713227e55437523c91997175242078e9e4
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836480"
---
# <a name="getting-started-with-azure-synapse-analytics"></a>Erste Schritte mit Azure Synapse Analytics

Dieses Tutorial führt Sie durch alle grundlegenden Schritte, die zum Einrichten und Verwenden von Azure Synapse Analytics erforderlich sind.

## <a name="prepare-a-storage-account-for-use-with-a-synapse-workspace"></a>Vorbereiten eines Speicherkontos für die Verwendung mit einem Synapse-Arbeitsbereich

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
1. Erstellen Sie mit den folgenden Einstellungen ein neues Speicherkonto:
    * Auf der Registerkarte **Grundlagen**

    |Einstellung | Vorgeschlagener Wert | BESCHREIBUNG |
    |---|---|---|
    |**Speicherkontoname**| Sie können einen beliebigen Namen vergeben.|In diesem Dokument haben wir es `contosolake` genannt.
    |**Kontoart**|Muss auf `StorageV2` festgelegt werden.||
    |**Location**|Sie können einen beliebigen Ort auswählen.| Es wird empfohlen, dass sich Ihr Synapse-Arbeitsbereich und das Azure Data Lake Storage (ADLS) Gen2-Konto in derselben Region befinden.|
    ||||
    
    * Auf der Registerkarte **Erweitert**
    
    |Einstellung | Vorgeschlagener Wert | BESCHREIBUNG |
    |---|---|---|
    |**Data Lake Storage Gen2**|`Enabled`| Azure Synapse funktioniert nur mit Speicherkonten, bei denen diese Einstellung aktiviert ist.|
    ||||

1. Nachdem das Speicherkonto erstellt wurde, wählen Sie im linken Navigationsbereich **Zugriffssteuerung (IAM)** aus. Weisen Sie dann die folgenden Rollen zu, oder stellen Sie sicher, dass sie bereits zugewiesen sind. 
    a. * Weisen Sie sich selbst der Rolle **Besitzer** im Speicherkonto zu. b. * Weisen Sie sich selbst der Rolle **Besitzer von Speicherblobdaten** im Speicherkonto zu.
1. Wählen Sie im linken Navigationsbereich **Container** aus, und erstellen Sie einen Container. Sie können einen beliebigen Namen vergeben. Akzeptieren Sie die standardmäßige **öffentliche Zugriffsebene**. In diesem Dokument rufen wir den Container `users` auf. Klicken Sie auf **Erstellen**. 

## <a name="create-a-synapse-workspace"></a>Erstellen eines Synapse-Arbeitsbereichs

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und suchen Sie am oberen Rand nach `Synapse`.
1. Wählen Sie in den Suchergebnissen unter **Dienste** den Eintrag **Azure Synapse Analytics (Arbeitsbereichsvorschau)** aus.
1. Wählen Sie **+ Hinzufügen** aus.
1. Registerkarte **Grundlagen**:

    |Einstellung | Vorgeschlagener Wert | BESCHREIBUNG |
    |---|---|---|
    |**Arbeitsbereichsname**|Sie können einen beliebigen Namen vergeben.| In diesem Dokument verwenden wir `myworkspace`.
    |**Region**|Zuordnen der Region des Speicherkontos||
    |||

1. Wählen Sie unter **Data Lake Storage Gen 2** das Konto und den Container aus, die Sie zuvor erstellt haben.
    > [!NOTE]
    > Wir bezeichnen das hier ausgewählte Speicherkonto als „primäres“ Speicherkonto des Synapse-Arbeitsbereichs. Dieses Konto wird zum Speichern von Daten in Apache Spark-Tabellen und für Protokolle verwendet, die erstellt werden, wenn Spark-Pools erstellt oder Spark-Anwendungen ausgeführt werden.

1. Klicken Sie auf **Überprüfen + erstellen**. Klicken Sie auf **Erstellen**. Ihr Arbeitsbereich steht in wenigen Minuten zur Verfügung.

## <a name="verify-the-synapse-workspace-msi-has-access-to-the-storage-account"></a>Überprüfen Sie, ob der MSI des Synapse-Arbeitsbereichs Zugriff auf das Speicherkonto hat.

Dies wurde möglicherweise schon für Sie erledigt. Sie sollten dies in diesem Fall überprüfen.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und öffnen Sie das primäre Speicherkonto, das Sie für Ihren Arbeitsbereich ausgewählt haben.
1. Wählen Sie im linken Navigationsbereich auf **Zugriffssteuerung (IAM)** . Weisen Sie dann die folgenden Rollen zu, oder stellen Sie sicher, dass sie bereits zugewiesen sind. 
    a. Weisen Sie die Arbeitsbereichsidentität der Rolle **Mitwirkender an Storage-Blobdaten** im Speicherkonto zu. Die Arbeitsbereichsidentität trägt denselben Namen wie der Arbeitsbereich. In diesem Dokument lautet der Name `myworkspace`, sodass die Arbeitsbereichsidentität `myworkspaced` ist.
1. Wählen Sie **Speichern** aus.
    
## <a name="launch-synapse-studio"></a>Starten von Synapse Studio

Nachdem Ihr Synapse-Arbeitsbereich erstellt wurde, haben Sie zwei Möglichkeiten zum Öffnen von Synapse Studio:
* Öffnen Sie Ihren Synapse-Arbeitsbereich im [Azure-Portal](https://portal.azure.com), und wählen Sie am Anfang des Abschnitts **Übersicht** den Eintrag **Synapse Studio starten** aus.
* Wechseln Sie direkt zu https://web.azuresynapse.net, und melden Sie sich bei Ihrem Arbeitsbereich an.

## <a name="create-a-sql-pool"></a>Erstellen eines SQL-Pools

1. Wählen Sie in Synapse Studio im linken Navigationsbereich **Verwalten > SQL-Pools** aus.

    > [!NOTE] 
    > Alle Synapse-Arbeitsbereiche verfügen über einen vorab erstellten Pool namens **SQL On-Demand**.

1. Wählen Sie **+Neu** aus, und geben Sie diese Einstellungen ein:

    |Einstellung | Vorgeschlagener Wert | 
    |---|---|---|
    |**Name des SQL-Pools**| `SQLDB1`|
    |**Leistungsstufe**|`DW100C`|
    |||

1. Wählen Sie **Überprüfen und erstellen** und dann **Erstellen** aus.
1. Ihr SQL-Pool steht in wenigen Minuten zur Verfügung.

    > [!NOTE]
    > Ein Synapse SQL-Pool entspricht dem, was bisher als „Azure SQL Data Warehouse“ bezeichnet wurde.

Ein SQL-Pool nutzt abrechenbare Ressourcen, solange er ausgeführt wird. Somit können Sie den Pool bei Bedarf anhalten, um die Kosten zu senken.

Wenn Ihr SQL-Pool erstellt wird, wird er einer SQL-Pooldatenbank zugeordnet, die auch als **SQLDB1** bezeichnet wird.

## <a name="create-an-apache-spark-pool"></a>Erstellen eines Apache Spark-Pools

1. Wählen Sie in Synapse Studio auf der linken Seite **Verwalten > Apache Spark-Pools** aus.
1. Wählen Sie **+Neu** aus, und geben Sie diese Einstellungen ein:

    |Einstellung | Vorgeschlagener Wert | 
    |---|---|---|
    |**Name des Apache Spark-Pools**|`Spark1`
    |**Knotengröße**| `Small`|
    |**Anzahl von Knoten**| Legen Sie den Mindestwert auf 3 und den Höchstwert auf 3 fest.|
    |||

1. Wählen Sie **Überprüfen und erstellen** und dann **Erstellen** aus.
1. Ihr Apache Spark-Pool steht in wenigen Sekunden zur Verfügung.

> [!NOTE]
> Trotz des Namens ist ein Apache Spark-Pool nicht mit einem SQL-Pool vergleichbar. Es handelt sich nur um ein paar grundlegende Metadaten, die Sie verwenden, um den Synapse-Arbeitsbereich darüber zu informieren, wie mit Spark interagiert werden soll. 

Da es sich dabei um Metadaten handelt, können Spark-Pools nicht gestartet oder beendet werden. 

Wenn Sie eine Spark-Aktivität in Synapse ausführen, geben Sie einen Spark-Pool an, der verwendet werden soll. Der Pool informiert Synapse, wie viele Spark-Ressourcen verwendet werden sollen. Sie bezahlen nur für die Ressourcen, die verwendet werdet. Wenn Sie die Verwendung des Pools aktiv beenden, erfolgt automatisch ein Timeout der Ressourcen, und sie werden wiederverwendet.

> [!NOTE]
> Spark-Datenbanken werden unabhängig von Spark-Pools erstellt. Ein Arbeitsbereich verfügt immer über eine Spark-DB namens **Standard** (default), und Sie können zusätzliche Spark-Datenbanken erstellen.

## <a name="the-sql-on-demand-pool"></a>Der SQL On-Demand-Pool

Jeder Arbeitsbereich verfügt über einen vordefinierten und nicht löschbaren Pool namens **SQL On-Demand**. Der SQL On-Demand-Pool ermöglicht es Ihnen, mit SQL zu arbeiten, ohne einen Synapse SQL-Pool erstellen oder sich darum Gedanken machen zu müssen. Im Gegensatz zu den anderen Arten von Pools basiert die Abrechnung für SQL On-Demand auf der Menge der Daten, die zum Ausführen der Abfrage durchsucht werden, und nicht auf der Anzahl der Ressourcen, die zum Ausführen der Abfrage verwendet werden.

* SQL On-Demand verfügt auch über eigene SQL On-Demand-Datenbanken, die unabhängig von jeglichem SQL On-Demand-Pool vorhanden sind.
* Aktuell besitzt ein Arbeitsbereich immer genau einen SQL On-Demand-Pool namens **SQL On-Demand**.

## <a name="load-the-nyc-taxi-sample-data-into-the-sqldb1-database"></a>Laden der NYC Taxi-Beispieldaten in die Datenbank SQLDB1

1. Wählen Sie in Synapse Studio im obersten blauen Menü das **?** - Symbol aus.
1. Wählen Sie **Erste Schritte > „Erste Schritte“-Hub** aus.
1. Wählen Sie auf der Karte mit der Bezeichnung **Abfragebeispieldaten** den SQL-Pool namens `SQLDB1` aus.
1. Wählen Sie **Abfragedaten** aus. Es wird eine Benachrichtigung angezeigt und wieder ausgeblendet, die besagt, dass „Beispieldaten geladen werden“.
1. Am oberen Rand von Synapse Studio wird eine hellblaue Benachrichtigungsleiste mit dem Hinweis angezeigt, dass Daten in SQLDB1 geladen werden. Warten Sie, bis die Leiste grün wird, und schließen Sie sie dann.

## <a name="explore-the-nyc-taxi-data-in-the-sql-pool"></a>Untersuchen der NYC Taxi-Daten im SQL-Pool

1. Navigieren Sie in Synapse Studio zum Hub **Daten**.
1. Navigieren Sie zu **SQLDB1 > Tabellen**. Sie werden sehen, dass mehrere Tabellen geladen wurden.
1. Klicken Sie mit der rechten Maustaste auf die Tabelle **dbo.Trip**, und wählen Sie **Neues SQL-Skript > OBERSTE 100 Zeilen auswählen** aus.
1. Ein neues SQL-Skript wird erstellt und automatisch ausgeführt.
1. Beachten Sie, dass am oberen Rand des SQL-Skripts **Verbinden mit** automatisch auf den SQL-Pool namens SQLDB1 festgelegt ist.
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

1. Diese Abfrage zeigt, wie die Gesamtzahl der Fahrtstrecken und die durchschnittliche Fahrtstrecke mit der Anzahl der Fahrgäste in Beziehung stehen.
1. Ändern Sie im Ergebnisfenster des SQL-Skripts die **Ansicht** in **Diagramm**, um eine Visualisierung der Ergebnisse als Liniendiagramm anzuzeigen.

## <a name="load-the-nyc-taxi-sample-data-into-the-spark-nyctaxi-database"></a>Laden der NYC Taxi-Beispieldaten in die Spark-Datenbank „nyctaxi“

Wir haben Daten in einer Tabelle in `SQLDB1` verfügbar. Nun laden wir sie in eine Spark-Datenbank namens „nyctaxi“.

1. Navigieren Sie in Synapse Studio zum Hub **Entwickeln**.
1. Wählen Sie **+** und **Notebook** aus.
1. Legen Sie am oberen Rand des Notebooks den Wert von **Anfügen an** auf `Spark1` fest.
1. Wählen Sie **Code hinzufügen** aus, um eine Notebook-Codezelle hinzuzufügen, und fügen Sie den folgenden Text ein:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLDB1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. Navigieren Sie zum Hub **Daten**, klicken Sie mit der rechten Maustaste auf **Datenbanken**, und wählen Sie **Aktualisieren** aus.
1. Jetzt sollten diese Datenbanken angezeigt werden:
    - SQLDB (SQL-Pool)
    - nyctaxi (Spark)
      
## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Analysieren der NYC Taxi-Daten mithilfe von Spark und Notebooks

1. Wechseln Sie zurück zu Ihrem Notebook.
1. Erstellen Sie eine neue Codezelle, geben Sie den folgenden Text ein, und führen Sie die Zelle beispielsweise mit den NYC Taxi-Daten aus, die wir in die Spark-DB `nyctaxi` geladen haben.

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Führen Sie den folgenden Code aus, um dieselbe Analyse durchzuführen, die wir zuvor mit dem SQL-Pool `SQLDB1` vorgenommen haben. Dieser Code speichert auch die Ergebnisse der Analyse in einer Tabelle namens `nyctaxi.passengercountstats` und visualisiert die Ergebnisse.

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
 
## <a name="customize-data-visualization-data-with-spark-and-notebooks"></a>Anpassen von Datenvisualisierungsdaten mit Spark und Notebooks

Mit Notebooks können Sie kontrollieren, wie Diagramme gerendert werden. Der folgende Code zeigt ein einfaches Beispiel für die Verwendung der beliebten Bibliotheken `matplotlib` und `seaborn`. Er rendert dieselbe Art von Liniendiagramm, das Sie gesehen haben, als die SQL-Abfragen zuvor ausgeführt wurden.

```py
%%pyspark
import matplotlib.pyplot
import seaborn

seaborn.set(style = "whitegrid")
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.toPandas()
seaborn.lineplot(x="PassengerCount", y="SumTripDistance" , data = df)
seaborn.lineplot(x="PassengerCount", y="AvgTripDistance" , data = df)
matplotlib.pyplot.show()
```
    
## <a name="load-data-from-a-spark-table-into-a-sql-pool-table"></a>Laden von Daten aus einer Spark-Tabelle in eine SQL-Pooltabelle

Zuvor haben wir Daten aus einer SQL-Pooltabelle `SQLDB1.dbo.Trip` in eine Spark-Tabelle `nyctaxi.trip` kopiert. Anschließend haben wir unter Verwendung von Spark die Daten in der Spark-Tabelle `nyctaxi.passengercountstats` aggregiert. Jetzt kopieren wir die Daten aus `nyctaxi.passengercountstats` in eine SQL-Pooltabelle namens `SQLDB1.dbo.PassengerCountStats`. 

Führen Sie die unten stehende Zelle in Ihrem Notebook aus. Die aggregierte Spark-Tabelle wird zurück in die SQL-Pooltabelle kopiert.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLDB1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="analyze-nyc-taxi-data-in-spark-databases-using-sql-on-demand"></a>Analysieren von NYC Taxi-Daten in Spark-Datenbanken mithilfe von SQL On-Demand 

Tabellen in Spark-Datenbanken sind automatisch sichtbar und können automatisch von SQL On-Demand abgefragt werden.

1. Navigieren Sie in Synapse Studio zum Hub **Entwickeln**, und erstellen Sie ein neues SQL-Skript.
1. Legen Sie **Verbinden mit** auf **SQL On-Demand** fest. 
1. Fügen Sie den folgenden Text in das Skript ein, und führen Sie das Skript aus.

    ```sql
    SELECT *
    FROM nyctaxi.dbo.passengercountstats
    ```
    > [!NOTE]
    > Wenn Sie zum ersten Mal eine Abfrage ausführen, die SQL On-Demand verwendet, dauert es ungefähr 10 Sekunden, bis SQL On-Demand SQL-Ressourcen gesammelt hat, die zum Ausführen Ihrer Abfragen benötigt werden. Nachfolgende Abfragen benötigen diese Zeit nicht mehr und sind viel schneller.
  
## <a name="orchestrate-activities-with-pipelines"></a>Orchestrieren von Aktivitäten mit Pipelines

Sie können eine Vielzahl verschiedener Aufgaben in Azure Synapse orchestrieren. In diesem Abschnitt zeigen wir Ihnen, wie einfach dies ist.

1. Navigieren Sie in Synapse Studio zum Hub **Orchestrieren**.
1. Wählen Sie **+** und dann **Pipeline** aus. Eine neue Pipeline wird erstellt.
1. Navigieren Sie zum Hub „Entwickeln“, und suchen Sie das Notebook, das Sie zuvor erstellt haben.
1. Ziehen Sie dieses Notebook in die Pipeline.
1. Wählen Sie in der Pipeline **Trigger hinzufügen > Neu/Bearbeiten** aus.
1. Wählen Sie in **Trigger auswählen** den Befehl **Neu** aus, und legen Sie dann unter „Serie“ fest, dass der Trigger jede 1 Stunde ausgeführt werden soll.
1. Klicken Sie auf **OK**.
1. Wählen Sie **Alle veröffentlichen** aus, und die Pipeline wird stündlich ausgeführt.
1. Wenn Sie die Pipeline jetzt ausführen möchten, ohne bis zur nächsten Stunde zu warten, wählen Sie **Trigger hinzufügen > Neu/Bearbeiten** aus.

## <a name="working-with-data-in-a-storage-account"></a>Arbeiten mit Daten in einem Speicherkonto

Bisher haben wir Szenarien behandelt, bei denen sich Daten in Datenbanken im Arbeitsbereich befunden haben. Nun zeigen wir Ihnen, wie Sie mit Dateien in Speicherkonten arbeiten. In diesem Szenario verwenden wir das primäre Speicherkonto des Arbeitsbereichs und den Container, die wir beim Erstellen des Arbeitsbereichs angegeben haben.

* Der Name des Speicherkontos: `contosolake`
* Der Name des Containers in dem Speicherkonto: `users`

### <a name="creating-csv-and-parquet-files-in-your-storage-account"></a>Erstellen von CSV- und Parquet-Dateien in Ihrem Speicherkonto

Führen Sie den folgenden Code in einem Notebook aus. Er erstellt eine CSV-Datei und eine Parquet-Datei im Speicherkonto.

```py
%%pyspark
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.repartition(1) # This ensure we'll get a single file during write()
df.write.mode("overwrite").csv("/NYCTaxi/PassengerCountStats.csv")
df.write.mode("overwrite").parquet("/NYCTaxi/PassengerCountStats.parquet")
```

### <a name="analyzing-data-in-a-storage-account"></a>Analysieren von Daten in einem Speicherkonto

1. Navigieren Sie in Synapse Studio zum Hub **Daten**.
1. Wählen Sie **Verknüpft** aus.
1. Navigieren Sie zu **Speicherkonten > myworkspace (Primär – contosolake)** .
1. Wählen Sie **Benutzer (Primär)** aus.
1. Es sollte ein Ordner namens „NYCTaxi“ angezeigt werden. Darin sollten Sie die beiden Ordner „PassengerCountStats.csv“ und „PassengerCountStats.parquet“ sehen.
1. Navigieren Sie in den Ordner „PassengerCountStats.parquet“.
1. Klicken Sie mit der rechten Maustaste auf die darin enthaltene Parquet-Datei, und wählen Sie **Neues Notebook** aus. Daraufhin wird ein Notebook mit einer Zelle wie der folgenden erstellt:

    ```py
    %%pyspark
    data_path = spark.read.load('abfss://users@contosolake.dfs.core.windows.net/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet', format='parquet')
    data_path.show(100)
    ```

1. Führen Sie die Zelle aus.
1. Klicken Sie mit der rechten Maustaste auf die darin enthaltene Parquet-Datei, und wählen Sie **Neues SQL-Skript > OBERSTE 100 Zeilen AUSWÄHLEN** aus. Daraufhin wird ein SQL-Skript wie das folgende erstellt:

    ```sql
    SELECT TOP 100 *
    FROM OPENROWSET(
        BULK 'https://contosolake.dfs.core.windows.net/users/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet',
        FORMAT='PARQUET'
    ) AS [r];
    ```
    
1. In dem Skript wird das Feld **Anfügen an** auf **SQL On-Demand-** festgelegt.
1. Führen Sie das Skript aus.

## <a name="visualize-data-with-power-bi"></a>Visualisieren von Daten mit Power BI

Aus den NYC Taxi-Daten haben wir aggregierte Datasets in zwei Tabellen erstellt:
* `nyctaxi.passengercountstats`
* `SQLDB1.dbo.PassengerCountStats`

Sie können einen Power BI-Arbeitsbereich mit Ihrem Synapse-Arbeitsbereich verknüpfen. Auf diese Weise können Sie problemlos Daten in Ihren Power BI-Arbeitsbereich übernehmen, und Sie können Ihre Power BI-Berichte direkt in Ihrem Synapse-Arbeitsbereich bearbeiten.

### <a name="create-a-power-bi-workspace"></a>Erstellen eines Power BI-Arbeitsbereichs

1. Melden Sie sich bei [powerbi.microsoft.com](https://powerbi.microsoft.com/) an.
1. Erstellen Sie einen neuen Power BI-Arbeitsbereich namens `NYCTaxiWorkspace1`.

### <a name="link-your-synapse-workspace-to-your-new-power-bi-workspace"></a>Verknüpfen Ihres Synapse-Arbeitsbereichs mit Ihrem neuen Power BI-Arbeitsbereich

1. Navigieren Sie in Synapse Studio zu **Verwalten > Verknüpfte Dienste**.
1. Wählen Sie **+ Neu** und dann **Verbinden mit Power BI** aus, und legen Sie diese Felder fest:

    |Einstellung | Vorgeschlagener Wert | 
    |---|---|---|
    |**Name**|`NYCTaxiWorkspace1`|
    |**Arbeitsbereichsname**|`NYCTaxiWorkspace1`|
    |||
    
1. Klicken Sie auf **Erstellen**.

### <a name="create-a-power-bi-dataset-that-uses-data-in-your-synapse-workspace"></a>Erstellen eines Power BI-Datasets, das Daten in Ihrem Synapse-Arbeitsbereich verwendet

1. Navigieren Sie in Synapse Studio zu **Entwickeln > Power BI**.
1. Navigieren Sie zu **NYCTaxiWorkspace1 > Power BI-Datasets**, und wählen Sie **Neues Power BI-Dataset** aus.
1. Zeigen Sie auf die Datenbank `SQLDB1`, und wählen Sie **.pbids-Datei herunterladen** aus.
1. Öffnen Sie die heruntergeladene `.pbids`-Datei. 
1. Dadurch wird Power BI Desktop gestartet und automatisch mit `SQLDB1` in Ihrem Synapse-Arbeitsbereich verbunden.
1. Wenn ein Dialogfeld namens **SQL Server-Datenbank** angezeigt wird: a. Wählen Sie **Microsoft-Konto** aus. 
    b. Wählen Sie **Anmelden** aus, und melden Sie sich an.
    c. Wählen Sie **Verbinden**.
1. Das Dialogfeld **Navigator** wird geöffnet. Überprüfen Sie in diesem Fall die Tabelle **PassengerCountStats**, und wählen Sie **Laden** aus.
1. Das Dialogfeld **Verbindungseinstellungen** wird angezeigt. Wählen Sie **DirectQuery** und dann **OK** aus.
1. Wählen Sie die Schaltfläche **Bericht** auf der linken Seite aus.
1. Fügen Sie Ihrem Bericht **Liniendiagramm** hinzu.
    a. Ziehen Sie die Spalte **PasssengerCount** in **Visualisierungen > Achse**. b. Ziehen Sie die Spalten **SumTripDistance** und **AvgTripDistance** in **Visualisierungen > Werte**.
1. Wählen Sie auf der Registerkarte **Start** die Option **Veröffentlichen** aus.
1. Sie werden gefragt, ob Sie Ihre Änderungen speichern möchten. Wählen Sie **Speichern** aus.
1. Sie werden aufgefordert, einen Dateinamen auszuwählen. Wählen Sie `PassengerAnalysis.pbix` aus und anschließend **Speichern**.
1. Sie werden aufgefordert, **ein Ziel auszuwählen**. Wählen Sie `NYCTaxiWorkspace1` aus und dann **Auswählen**.
1. Warten Sie, bis die Veröffentlichung abgeschlossen ist.

### <a name="configure-authentication-for-your-dataset"></a>Konfigurieren der Authentifizierung für Ihr Dataset

1. Öffnen Sie [powerbi.microsoft.com](https://powerbi.microsoft.com/), und **melden Sie sich an**.
1. Wählen Sie links unter **Arbeitsbereiche** den Arbeitsbereich `NYCTaxiWorkspace1` aus.
1. In diesem Arbeitsbereich sollten Sie ein Dataset namens `Passenger Analysis` sowie einen Bericht mit dem Namen `Passenger Analysis` sehen.
1. Zeigen Sie auf das Dataset `PassengerAnalysis`, und wählen Sie zuerst das Symbol mit den drei Punkten und dann **Einstellungen** aus.
1. Legen Sie in **Anmeldeinformationen für Datenquelle** die **Authentifizierung**smethode auf **OAuth2** fest, und wählen Sie **Anmelden** aus.

### <a name="edit-a-report-in-synapse-studio"></a>Bearbeiten eines Berichts in Synapse Studio

1. Wechseln Sie zurück zu Synapse Studio, und wählen Sie **Schließen und aktualisieren** aus. 
1. Navigieren Sie zum Hub **Entwickeln**. 
1. Zeigen Sie auf **Power BI**, und klicken Sie auf „Aktualisieren“ für den Knoten **Power BI-Berichte**.
1. Unter **Power BI** sollte nun Folgendes angezeigt werden: a. * Unter **NYCTaxiWorkspace1 > Power BI-Datasets** ein neues Dataset namens **PassengerAnalysis**.
    b. * Unter **NYCTaxiWorkspace1 > Power BI-Berichte** ein neuer Bericht namens **PassengerAnalysis**.
1. Wählen Sie den Bericht **PassengerAnalysis** aus. 
1. Der Bericht wird geöffnet, und Sie können den Bericht jetzt direkt in Synapse Studio bearbeiten.

## <a name="monitor-activities"></a>Überwachen von Aktivitäten

1. Navigieren Sie in Synapse Studio zum Hub „Überwachen“.
1. An dieser Stelle können Sie einen Verlauf aller Aktivitäten anzeigen, die im Arbeitsbereich stattfinden, und welche davon jetzt aktiv sind.
1. Erkunden Sie die **Pipelineausführungen**, **Apache Spark-Anwendungen**und **SQL-Anforderungen**, und Sie können sehen, was Sie bereits im Arbeitsbereich gemacht haben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Azure Synapse Analytics (Vorschau)](overview-what-is.md).

