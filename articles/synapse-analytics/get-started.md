---
title: 'Tutorial: Erste Schritte mit Azure Synapse Analytics'
description: In diesem Tutorial werden die grundlegenden Schritte zum Einrichten und Verwenden von Azure Synapse Analytics beschrieben.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 05/19/2020
ms.openlocfilehash: daa8b594b06203c7de9a9b462be469dd71ed2e49
ms.sourcegitcommit: 6571e34e609785e82751f0b34f6237686470c1f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2020
ms.locfileid: "84791859"
---
# <a name="get-started-with-azure-synapse-analytics"></a>Erste Schritte mit Azure Synapse Analytics

In diesem Tutorial werden die grundlegenden Schritte zum Einrichten und Verwenden von Azure Synapse Analytics beschrieben.

## <a name="prepare-a-storage-account"></a>Vorbereiten eines Speicherkontos

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
1. Erstellen Sie ein neues Speicherkonto mit den folgenden Einstellungen:

    |Registerkarte|Einstellung | Vorgeschlagener Wert | BESCHREIBUNG |
    |---|---|---|---|
    |Grundlagen|**Speicherkontoname**| Sie können einen beliebigen Namen vergeben.|In diesem Dokument wird **contosolake** verwendet.|
    |Grundlagen|**Kontoart**|Muss auf **StorageV2** festgelegt werden.||
    |Grundlagen|**Location**|Wählen Sie einen beliebigen Standort aus.| Wir empfehlen Ihnen, Ihren Azure Synapse Analytics-Arbeitsbereich und das Azure Data Lake Storage Gen2-Konto in derselben Region anzuordnen.|
    |Erweitert|**Data Lake Storage Gen2**|**Aktiviert**| Azure Synapse funktioniert nur mit Speicherkonten, bei denen diese Einstellung aktiviert ist.|
    |||||

1. Wählen Sie nach dem Erstellen des Speicherkontos im linken Bereich die Option **Zugriffssteuerung (IAM)** aus. Weisen Sie anschließend die folgenden Rollen zu, bzw. stellen Sie sicher, dass sie bereits zugewiesen sind:
    1. Weisen Sie sich selbst die Rolle **Besitzer** zu.
    1. Weisen Sie sich selbst die Rolle **Besitzer von Speicherblobdaten** zu.
1. Wählen Sie im linken Bereich die Option **Container** aus, und erstellen Sie einen Container.
1. Sie können dem Container einen beliebigen Namen geben. In diesem Dokument hat der Container den Namen **users**.
1. Übernehmen Sie die Standardeinstellung **Öffentliche Zugriffsebene**, und wählen Sie anschließend **Erstellen** aus.

Im folgenden Schritt konfigurieren Sie Ihren Azure Synapse-Arbeitsbereich so, dass dieses Speicherkonto als „primäres“ Speicherkonto und der Container zur Speicherung von Arbeitsbereichsdaten verwendet wird. Im Arbeitsbereich werden Daten in Apache Spark-Tabellen gespeichert. Die Spark-Anwendungsprotokolle werden im Ordner **/synapse/workspacename** gespeichert.

## <a name="create-a-synapse-workspace"></a>Erstellen eines Synapse-Arbeitsbereichs

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und suchen Sie oben nach **Synapse**.
1. Wählen Sie in den Suchergebnissen unter **Dienste** den Eintrag **Azure Synapse Analytics (Arbeitsbereichsvorschau)** aus.
1. Wählen Sie **Hinzufügen** aus, um einen Arbeitsbereich mit diesen Einstellungen zu erstellen:

    |Registerkarte|Einstellung | Vorgeschlagener Wert | BESCHREIBUNG |
    |---|---|---|---|
    |Grundlagen|**Arbeitsbereichsname**|Sie können einen beliebigen Namen vergeben.| In diesem Dokument wird **myworkspace** verwendet.|
    |Grundlagen|**Region**|Wählen Sie dieselbe Region wie für das Speicherkonto aus.|

1. Wählen Sie unter **Data Lake Storage Gen 2** das Konto und den Container aus, die Sie zuvor erstellt haben.
1. Wählen Sie **Bewerten + erstellen** > **Erstellen** aus. Ihr Arbeitsbereich steht nach wenigen Minuten zur Verfügung.

## <a name="verify-access-to-the-storage-account"></a>Überprüfen des Zugriffs auf das Speicherkonto

Verwaltete Identitäten für Ihren Azure Synapse-Arbeitsbereich verfügen unter Umständen bereits über Zugriff auf das Speicherkonto. Führen Sie die folgenden Schritte aus, um dies zu überprüfen:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) und dann das primäre Speicherkonto, das Sie für Ihren Arbeitsbereich ausgewählt haben.
1. Wählen Sie im linken Bereich **Zugriffssteuerung (IAM)** aus.
1. Weisen Sie die folgenden Rollen zu, bzw. stellen Sie sicher, dass sie bereits zugewiesen sind. Wir verwenden für die Identität und den Namen des Arbeitsbereichs den gleichen Namen.
    1. Weisen Sie für die Rolle **Mitwirkender an Storage-Blobdaten** des Speicherkontos **myworkspace** als Identität des Arbeitsbereichs zu.
    1. Weisen Sie **myworkspace** als Namen für den Arbeitsbereich zu.

1. Wählen Sie **Speichern** aus.

## <a name="open-synapse-studio"></a>Öffnen von Synapse Studio

Nachdem Ihr Azure Synapse-Arbeitsbereich erstellt wurde, haben Sie zwei Möglichkeiten zum Öffnen von Synapse Studio:

* Öffnen Sie Ihren Synapse-Arbeitsbereich im [Azure-Portal](https://portal.azure.com). Wählen Sie oben im Abschnitt **Übersicht** die Option **Synapse Studio starten** aus.
* Navigieren Sie zu https://web.azuresynapse.net, und melden Sie sich bei Ihrem Arbeitsbereich an.

## <a name="create-a-sql-pool"></a>Erstellen eines SQL-Pools

1. Wählen Sie in Synapse Studio im linken Bereich **Verwalten** > **SQL-Pools** aus.
1. Wählen Sie **Neu** aus, und geben Sie diese Einstellungen ein:

    |Einstellung | Vorgeschlagener Wert | 
    |---|---|---|
    |**Name des SQL-Pools**| **SQLDB1**|
    |**Leistungsstufe**|**DW100C**|
    |||

1. Wählen Sie **Bewerten + erstellen** > **Erstellen** aus. Ihr SQL-Pool steht in wenigen Minuten zur Verfügung. Ihr SQL-Pool ist einer SQL-Pool-Datenbank zugeordnet, die auch als **SQLDB1** bezeichnet wird.

Ein SQL-Pool nutzt abrechenbare Ressourcen, solange er aktiv ist. Sie können den Pool später anhalten, um die Kosten zu senken.

## <a name="create-an-apache-spark-pool"></a>Erstellen eines Apache Spark-Pools

1. Wählen Sie in Synapse Studio im linken Bereich die Option **Verwalten** > **Apache Spark-Pools** aus.
1. Wählen Sie **Neu** aus, und geben Sie diese Einstellungen ein:

    |Einstellung | Vorgeschlagener Wert | 
    |---|---|---|
    |**Name des Apache Spark-Pools**|**Spark1**
    |**Knotengröße**| **Klein**|
    |**Anzahl von Knoten**| Legen Sie den Mindestwert auf 3 und den Höchstwert auf 3 fest.|

1. Wählen Sie **Bewerten + erstellen** > **Erstellen** aus. Ihr Apache Spark-Pool steht in wenigen Sekunden zur Verfügung.

> [!NOTE]
> Trotz des Namens ist ein Apache Spark-Pool nicht mit einem SQL-Pool vergleichbar. Es handelt sich nur um ein paar grundlegende Metadaten, mit denen Sie den Azure Synapse-Arbeitsbereich darüber informieren, wie mit Spark interagiert werden soll.

Da es sich hierbei um Metadaten handelt, können Spark-Pools nicht gestartet oder beendet werden.

Wenn Sie eine Spark-Aktivität in Azure Synapse durchführen, geben Sie den zu verwendenden Spark-Pool an. Der Pool informiert Azure Synapse, wie viele Spark-Ressourcen verwendet werden sollen. Sie zahlen nur für die Ressourcen, die Sie verwenden. Wenn Sie die Verwendung des Pools aktiv beenden, erfolgt automatisch ein Timeout für die Ressourcen, und sie werden wiederverwendet.

> [!NOTE]
> Spark-Datenbanken werden unabhängig von Spark-Pools erstellt. Ein Arbeitsbereich verfügt immer über eine Spark-Datenbank mit dem Namen **default**. Sie können auch zusätzliche Spark-Datenbanken erstellen.

## <a name="the-sql-on-demand-pool"></a>Der SQL On-Demand-Pool

Jeder Arbeitsbereich verfügt über einen vordefinierten Pool mit dem Namen **SQL On-Demand**. Dieser Pool kann nicht gelöscht werden. Der Pool „SQL On-Demand“ ermöglicht es Ihnen, mit SQL zu arbeiten, ohne dass Sie einen SQL-Pool in Azure Synapse erstellen oder sich darüber Gedanken machen müssen.

Im Gegensatz zu den anderen Arten von Pools basiert die Abrechnung für SQL On-Demand auf der Menge der Daten, die zum Ausführen der Abfrage durchsucht werden, und nicht auf der Anzahl von Ressourcen, die zum Ausführen der Abfrage verwendet werden.

* SQL On-Demand verfügt über eigene SQL On-Demand-Datenbanken, die unabhängig von einem SQL On-Demand-Pool vorhanden sind.
* Ein Arbeitsbereich besitzt immer nur genau einen SQL On-Demand-Pool mit dem Namen **SQL On-Demand**.

## <a name="load-the-nyc-taxi-sample-data-into-the-sqldb1-database"></a>Laden der NYC Taxi-Beispieldaten in die Datenbank SQLDB1

1. Wählen Sie in Synapse Studio im obersten blauen Menü das Fragezeichen ( **?** ) aus.
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

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>Laden der NYC Taxi-Daten in die Spark-Datenbank „nyctaxi“

In einer Tabelle in **SQLDB1** sind Daten verfügbar. Laden Sie sie in eine Spark-Datenbank mit dem Namen **nyctaxi**.

1. Navigieren Sie in Synapse Studio zum Hub **Entwickeln**.
1. Wählen Sie **+**  > **Notebook** aus.
1. Legen Sie am oberen Rand des Notebooks den Wert von **Anfügen an** auf **Spark1** fest.
1. Wählen Sie **Code hinzufügen** aus, um eine Notebook-Codezelle hinzuzufügen, und fügen Sie dann den folgenden Text ein:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLDB1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. Navigieren Sie zum Hub **Daten**, klicken Sie mit der rechten Maustaste auf **Datenbanken**, und wählen Sie dann **Aktualisieren** aus. Die folgenden Datenbanken sollten angezeigt werden:

    - **SQLDB1** (SQL-Pool)
    - **nyctaxi** (Spark)

## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Analysieren der NYC Taxi-Daten mithilfe von Spark und Notebooks

1. Wechseln Sie zurück zu Ihrem Notebook.
1. Erstellen Sie eine neue Codezelle, und geben Sie den folgenden Text ein. Führen Sie die Zelle anschließend aus, um die NYC Taxi-Daten anzuzeigen, die wir in die Spark-Datenbank **nyctaxi** geladen haben.

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Führen Sie den folgenden Code aus, um dieselbe Analyse durchzuführen, die wir zuvor mit dem SQL-Pool **SQLDB1** vorgenommen haben. Mit diesem Code werden die Ergebnisse der Analyse in einer Tabelle mit dem Namen **nyctaxi.passengercountstats** gespeichert und die Ergebnisse visualisiert.

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

## <a name="customize-data-visualization-with-spark-and-notebooks"></a>Anpassen der Datenvisualisierung mit Spark und Notebooks

Sie können mit Notebooks steuern, wie Diagramme gerendert werden. Mit dem folgenden Code wird ein einfaches Beispiel angezeigt. Hierbei werden die gängigen Bibliotheken **matplotlib** und **seaborn** verwendet. Mit dem Code wird die gleiche Art von Liniendiagramm wie bei den zuvor ausgeführten SQL-Abfragen gerendert.

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

Weiter oben haben wir Daten aus der SQL-Pooltabelle **SQLDB1.dbo.Trip** in die Spark-Tabelle **nyctaxi.trip** kopiert. Anschließend haben wir mit Spark die Daten in der Spark-Tabelle **nyctaxi.passengercountstats** aggregiert. Nun kopieren wir die Daten aus **nyctaxi.passengercountstats** in eine SQL-Pooltabelle mit dem Namen **SQLDB1.dbo.PassengerCountStats**.

Führen Sie die folgende Zelle in Ihrem Notebook aus. Hiermit wird die aggregierte Spark-Tabelle zurück in die SQL-Pooltabelle kopiert.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLDB1.dbo.PassengerCountStats", Constants.INTERNAL )
```

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
  
## <a name="orchestrate-activities-with-pipelines"></a>Orchestrieren von Aktivitäten mit Pipelines

Sie können eine Vielzahl verschiedener Aufgaben in Azure Synapse orchestrieren.

1. Navigieren Sie in Synapse Studio zum Hub **Orchestrieren**.
1. Wählen Sie **+**  > **Pipeline** aus, um eine neue Pipeline zu erstellen.
1. Navigieren Sie zum Hub **Entwickeln**, und suchen Sie nach dem Notebook, das Sie zuvor erstellt haben.
1. Ziehen Sie dieses Notebook in die Pipeline.
1. Wählen Sie in der Pipeline **Trigger hinzufügen** > **Neu/Bearbeiten** aus.
1. Wählen Sie unter **Trigger auswählen** den Befehl **Neu** aus, und legen Sie dann unter **Serie** fest, dass der Trigger jede Stunde ausgeführt werden soll.
1. Klicken Sie auf **OK**.
1. Wählen Sie **Alle veröffentlichen**. Die Pipeline wird stündlich ausgeführt.
1. Wählen Sie zum sofortigen Ausführen der Pipeline (ohne bis zur nächsten Ausführung nach einer Stunde zu warten) **Trigger hinzufügen** > **Neu/Bearbeiten** aus.

## <a name="work-with-data-in-a-storage-account"></a>Arbeiten mit Daten in einem Speicherkonto

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
1. Öffnen Sie den Ordner **PassengerCountStats.parquet**. Er sollte eine Parquet-Datei mit einem Namen wie *part-00000-2638e00c-0790-496b-a523-578da9a15019-c000.snappy.parquet* enthalten.
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

## <a name="visualize-data-with-power-bi"></a>Visualisieren von Daten mit Power BI

Aus den NYC Taxi-Daten haben wir aggregierte Datasets in zwei Tabellen erstellt:
- **nyctaxi.passengercountstats**
- **SQLDB1.dbo.PassengerCountStats**

Sie können einen Power BI-Arbeitsbereich mit Ihrem Azure Synapse-Arbeitsbereich verknüpfen. Auf diese Weise können Sie problemlos Daten in Ihren Power BI-Arbeitsbereich einfügen. Sie können Ihre Power BI-Berichte direkt in Ihrem Azure Synapse-Arbeitsbereich bearbeiten.

### <a name="create-a-power-bi-workspace"></a>Erstellen eines Power BI-Arbeitsbereichs

1. Melden Sie sich bei [powerbi.microsoft.com](https://powerbi.microsoft.com/) an.
1. Erstellen Sie einen neuen Power BI-Arbeitsbereich mit dem Namen **NYCTaxiWorkspace1**.

### <a name="link-your-azure-synapse-workspace-to-your-new-power-bi-workspace"></a>Verknüpfen Ihres Azure Synapse-Arbeitsbereichs mit Ihrem neuen Power BI-Arbeitsbereich

1. Wechseln Sie in Synapse Studio zu **Verwalten** > **Verknüpfte Dienste**.
1. Wählen Sie **Neu** > **Mit Power BI verbinden** aus, und legen Sie dann die folgenden Felder fest:

    |Einstellung | Vorgeschlagener Wert | 
    |---|---|
    |**Name**|**NYCTaxiWorkspace1**|
    |**Arbeitsbereichsname**|**NYCTaxiWorkspace1**|

1. Klicken Sie auf **Erstellen**.

### <a name="create-a-power-bi-dataset-that-uses-data-in-your-azure-synapse-workspace"></a>Erstellen eines Power BI-Datasets, das Daten in Ihrem Azure Synapse-Arbeitsbereich verwendet

1. Wechseln Sie in Synapse Studio zu **Entwickeln** > **Power BI**.
1. Navigieren Sie zu **NYCTaxiWorkspace1** > **Power BI-Datasets**, und wählen Sie **Neues Power BI-Dataset** aus.
1. Zeigen Sie auf die Datenbank **SQLDB1**, und wählen Sie **.pbids-Datei herunterladen** aus.
1. Öffnen Sie die heruntergeladene **.pbids**-Datei. Power BI Desktop wird geöffnet, und es wird automatisch eine Verbindung mit **SQLDB1** in Ihrem Azure Synapse-Arbeitsbereich hergestellt.
1. Gehen Sie wie folgt vor, wenn ein Dialogfeld mit dem Namen **SQL Server-Datenbank** angezeigt wird:
    1. Wählen Sie **Microsoft-Konto** aus.
    1. Wählen Sie **Anmelden** aus, und melden Sie sich bei Ihrem Konto an.
    1. Wählen Sie **Verbinden**.
1. Aktivieren Sie nach dem Öffnen des Dialogfelds **Navigator** die Tabelle **PassengerCountStats**, und wählen Sie **Laden** aus.
1. Wählen Sie nach Anzeige des Dialogfelds **Verbindungseinstellungen** die Option **DirectQuery** > **OK** aus.
1. Wählen Sie auf der linken Seite die Schaltfläche **Bericht** aus.
1. Fügen Sie Ihrem Bericht **Liniendiagramm** hinzu.
    1. Ziehen Sie die Spalte **PassengerCount** auf **Visualisierungen** > **Achse**.
    1. Ziehen Sie die Spalten **SumTripDistance** und **AvgTripDistance** auf **Visualisierungen** > **Werte**.
1. Wählen Sie auf der Registerkarte **Start** die Option **Veröffentlichen** aus.
1. Wählen Sie **Speichern**, um Ihre Änderungen zu speichern.
1. Wählen Sie den Dateinamen **PassengerAnalysis.pbix** und dann die Option **Speichern** aus.
1. Wählen Sie unter **Ziel auswählen** die Option **NYCTaxiWorkspace1** aus, und klicken Sie dann auf **Auswählen**.
1. Warten Sie, bis die Veröffentlichung abgeschlossen ist.

### <a name="configure-authentication-for-your-dataset"></a>Konfigurieren der Authentifizierung für Ihr Dataset

1. Öffnen Sie [powerbi.microsoft.com](https://powerbi.microsoft.com/), und **melden Sie sich an**.
1. Wählen Sie auf der linken Seite unter **Arbeitsbereiche** den Arbeitsbereich **NYCTaxiWorkspace1** aus.
1. Suchen Sie in diesem Arbeitsbereich nach dem Dataset **PassengerAnalysis** und dem Bericht **PassengerAnalysis**.
1. Zeigen Sie auf das Dataset **PassengerAnalysis**, und wählen Sie die Schaltfläche mit den Auslassungspunkten (...) und dann die Option **Einstellungen** aus.
1. Legen Sie in **Anmeldeinformationen für Datenquelle** die **Authentifizierungsmethode** auf **OAuth2** fest, und wählen Sie anschließend **Anmelden** aus.

### <a name="edit-a-report-in-synapse-studio"></a>Bearbeiten eines Berichts in Synapse Studio

1. Wechseln Sie zurück zu Synapse Studio, und wählen Sie **Schließen und aktualisieren** aus.
1. Navigieren Sie zum Hub **Entwickeln**.
1. Zeigen Sie auf **Power BI**, und wählen Sie die Option „Aktualisieren“ für den Knoten **Power BI-Berichte** aus.
1. Unter **Power BI** sollte Folgendes angezeigt werden:
    1. Unter **NYCTaxiWorkspace1** > **Power BI-Datasets** ein neues Dataset mit dem Namen **PassengerAnalysis**.
    1. Unter **NYCTaxiWorkspace1** > **Power BI-Berichte** ein neuer Bericht mit dem Namen **PassengerAnalysis**.
1. Wählen Sie den Bericht **PassengerAnalysis** aus. Der Bericht wird geöffnet, und Sie können ihn direkt in Synapse Studio bearbeiten.

## <a name="monitor-activities"></a>Überwachen von Aktivitäten

1. Navigieren Sie in Synapse Studio zum Hub **Überwachen**.
1. An dieser Stelle können Sie einen Verlauf aller Aktivitäten anzeigen, die im Arbeitsbereich stattfinden, und welche davon jetzt aktiv sind.
1. Erkunden Sie die **Pipelineausführungen**, **Apache Spark-Anwendungen** und **SQL-Anforderungen**, um zu überprüfen, was Sie im Arbeitsbereich bereits durchgeführt haben.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich weiter über [Azure Synapse Analytics (Vorschau für Arbeitsbereiche)](overview-what-is.md).

