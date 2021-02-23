---
title: Verarbeiten von Daten aus Modellen für das automatisierte maschinelle Lernen (AutoML) mithilfe von Datenflüssen
description: Erfahren Sie, wie Sie mithilfe von Azure Data Factory-Datenflüssen Daten aus AutoML-Modellen (automatisiertes maschinelles Lernen) verarbeiten.
services: data-factory
author: amberz
co-author: ATLArcht
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 1/31/2021
ms.author: amberz
ms.co-author: Donnana
ms.openlocfilehash: 494fc2c227b6fe855e96a780d43cc6702722fa94
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100520770"
---
# <a name="process-data-from-automated-machine-learningautoml-models-using-data-flow"></a>Verarbeiten von Daten aus AutoML-Modellen (automatisiertes maschinelles Lernen) mithilfe von Datenflüssen

Automatisiertes maschinelles Lernen (AutoML) wird in Projekten mit maschinellem Lernen zum Trainieren, Optimieren und automatisierten Modellieren mithilfe der Zielmetriken verwendet, die Sie für Klassifizierung, Regression und Zeitreihenprognosen angeben. 

Eine Herausforderung besteht darin, dass die Rohdaten aus dem Data Warehouse oder der transaktionalen Datenbank ein riesiges Dataset ausmachen würden, z. B. 10 GB. Ein so großes Dataset erfordert sehr viel Zeit für das Trainieren von Modellen. Aus diesem Grund wird vor dem Trainieren von Azure Machine Learning-Modellen eine Optimierung der Datenverarbeitung empfohlen. In diesem Tutorial wird die Verwendung von ADF zum Partitionieren von Datasets in Parquet-Dateien für ein Azure Machine Learning-Dataset erläutert. 

Im AutoML-Projekt (automatisiertes maschinelles Lernen) werden die folgenden drei Szenarien für die Datenverarbeitung angewandt:

* Partitionieren großer Datenmengen vor dem Trainieren von Modellen in Parquet-Dateien 

     Der [Pandas](https://pandas.pydata.org/pandas-docs/stable/getting_started/overview.html)-Datenrahmen wird häufig zum Verarbeiten von Daten vor dem Trainieren von Modellen verwendet und eignet sich gut für Datengrößen unter 1 GB. Wenn die Daten jedoch größer als 1 GB sind, verarbeitet der Pandas-Datenrahmen die Daten sehr viel langsamer. es kann sogar vorkommen, dass eine Fehlermeldung über fehlenden Arbeitsspeicher ausgegeben wird. Für Aufgaben im Bereich maschinelles Lernen werden [Parquet](https://parquet.apache.org/)-Dateiformate empfohlen, da sie ein binäres Spaltenformat aufweisen.
    
    Azure Data Factorys-Zuordnungsdatenflüsse sind visuell entworfene Datentransformationen, für die Datentechniker keinen Code schreiben müssen. Sie eignen sich sehr gut für die Verarbeitung großer Datenmengen, da die Pipeline horizontal skalierte Spark-Cluster verwendet.

* Aufteilen in Trainingsdataset und Testdataset
    
    Das Trainingsdataset wird für das Trainieren des Modells verwendet und das Testdataset zum Bewerten der Modelle im Projekt mit maschinellem Lernen. Für das Aufteilen in Trainingsdaten und Testdaten wird die Aktivität für bedingtes Teilen in Zuordnungsdatenflüssen verwendet. 

* Entfernen nicht qualifizierter Daten

    Möglicherweise möchten Sie nicht qualifizierte Daten entfernen, z. B. Parquet Dateien mit null Zeilen. In diesem Tutorial verwenden wir die Aktivität zum Aggregieren, um die Anzahl von Zeilen abzufragen, die dann als Bedingung für das Entfernen nicht qualifizierter Daten verwendet wird. 


## <a name="preparation"></a>Vorbereitung
Verwenden Sie die folgende Tabelle aus Azure SQL-Datenbank. 
```
CREATE TABLE [dbo].[MyProducts](
    [ID] [int] NULL,
    [Col1] [char](124) NULL,
    [Col2] [char](124) NULL,
    [Col3] datetime NULL,
    [Col4] int NULL

) 

```

## <a name="convert-data-format-to-parquet"></a>Konvertieren der Daten in das Parquet-Format

Der Datenfluss konvertiert eine Tabelle aus Azure SQL-Datenbank in das Parquet-Dateiformat. 

**Quelldataset:** Transaktionstabelle aus Azure SQL-Datenbank

**Senkendataset:** Blob Storage mit Parquet-Format


## <a name="remove-unqualified-data-based-on-row-count"></a>Entfernen nicht qualifizierter Daten basierend auf der Zeilenanzahl

Angenommen, Sie möchten alle Daten mit einer Zeilenanzahl unter 2 entfernen. 

1. Verwenden Sie die Aktivität zum Aggregieren, um die Zeilenanzahl abzufragen. Verwenden Sie als Grundlage für **Gruppieren nach** in der Spalte „Col2“ **Aggregate** mit „count(1)“ für die Zeilenanzahl. 

    ![Konfigurieren der Aktivität zum Aggregieren für Abfragen der Zeilenanzahl](./media/scenario-dataflow-process-data-aml-models/aggregate-activity-addrowcount.png)

1. Wählen Sie für die Senkenaktivität den **Senkentyp** „Cache“ auf der Registerkarte **Senke** aus. Wählen Sie anschließend auf der Registerkarte **Einstellungen** in der Dropdownliste **Schlüsselspalten** die gewünschte Spalte aus. 

    ![Konfigurieren der CacheSink-Aktivität zum Abrufen der Zeilenanzahl in der zwischengespeicherten Senke](./media/scenario-dataflow-process-data-aml-models/cachesink-activity-addrowcount.png)

1. Verwenden Sie die Aktivität für abgeleitete Spalten, um die Spalte mit der Zeilenanzahl im Quelldatenstrom hinzuzufügen. Verwenden Sie auf der Registerkarte mit den **Einstellungen der abgeleiteten Spalte** den Ausdruck „CacheSink#lookup“ zum Abrufen der Zeilenanzahl aus dem Senkencache.
    ![Konfigurieren der Aktivität für abgeleitete Spalten zum Hinzufügen der Zeilenanzahl in Quelle 1](./media/scenario-dataflow-process-data-aml-models/derived-column-activity-rowcount-source-1.png)

1. Verwenden Sie Aktivität für bedingtes Teilen, um nicht qualifizierte Daten zu entfernen. In diesem Beispiel basiert die Zeilenanzahl auf der Spalte „Col2“, und durch die Bedingung werden Zeilen mit einer Anzahl unter 2 entfernt, sodass zwei Zeilen (ID = 2 und ID = 7) entfernt werden. Sie sollten nicht qualifizierte Daten für die Datenverwaltung in Blob Storage speichern. 

    ![Konfigurieren der Aktivität für bedingtes Teilen zum Abrufen von Daten, die größer oder gleich 2 sind](./media/scenario-dataflow-process-data-aml-models/conditionalsplit-greater-or-equal-than-2.png)

> [!NOTE]
>    *    Erstellen Sie eine neue Quelle zum Abrufen der Zeilenanzahl, die in späteren Schritten in der ursprünglichen Quelle verwendet werden. 
>    *    Verwenden Sie aus Leistungsgründen CacheSink. 

## <a name="split-training-data-and-test-data"></a>Teilen von Trainings- und Testdaten 

1. Sie möchten die Trainings- und Testdaten für jede Partition teilen. In diesem Beispiel rufen Sie für denselben Wert von „Col2“ die ersten beiden Zeilen als Testdaten und die übrigen Zeilen als Trainingsdaten ab. 

    Verwenden Sie die Fenster-Aktivität, um für jede Partition eine Zeilennummer für eine Spalte hinzuzufügen. Wählen Sie auf der Registerkarte **Über** die Spalte für die Partition aus (in diesem Tutorial „Col2“). Geben Sie auf der Registerkarte **Sortieren** eine Reihenfolge an (in diesem Tutorial basierend auf der ID), und fügen Sie auf der Registerkarte **Fensterspalten** eine Spalte als Zeilennummer für jede Partition hinzu. 
    ![Konfigurieren der Fensteraktivität zum Hinzufügen einer neuen Spalte als Zeilennummer](./media/scenario-dataflow-process-data-aml-models/window-activity-add-row-number.png)

1. Verwenden Sie die Aktivität für bedingtes Teilen, um die obersten beiden Zeilen der einzelnen Partitionen in das Testdataset und die übrigen Zeilen in das Trainingsdataset aufzuteilen. Geben Sie auf der Registerkarte **Einstellungen für Conditional Split** den Ausdruck „lesserOrEqual(RowNum,2)“ als Bedingung an. 

    ![Konfigurieren der Aktivität für bedingtes Teilen zum Aufteilen des aktuellen Datasets in Trainings- und Testdataset](./media/scenario-dataflow-process-data-aml-models/split-training-dataset-test-dataset.png)

## <a name="partition-training-dataset-and-test-dataset-with-parquet-format"></a>Partitionieren von Trainings- und Testdataset im Parquet-Format

1. Verwenden Sie die Senkenaktivität auf der Registerkarte **Optimieren**, und legen Sie in **Eindeutiger Wert pro Partition** eine Spalte als Spaltenschlüssel für die Partition fest. 
    ![Konfigurieren der Senkenaktivität zum Festlegen der Partition für das Trainingsdataset](./media/scenario-dataflow-process-data-aml-models/partition-training-dataset-sink.png)

    Sehen Sie sich hier die gesamte Pipelinelogik an.
    ![Logik der gesamten Pipeline](./media/scenario-dataflow-process-data-aml-models/entire-pipeline.png)


## <a name="next-steps"></a>Nächste Schritte

* Erstellen Sie die restliche Datenflusslogik mithilfe von Mapping Data Flow-[Transformationen](concepts-data-flow-overview.md).
