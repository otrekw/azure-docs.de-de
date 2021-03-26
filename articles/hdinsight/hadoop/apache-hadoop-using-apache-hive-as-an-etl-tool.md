---
title: Verwenden von Apache Hive als ETL-Tool – Azure HDInsight
description: Verwenden Sie Apache Hive, um Daten in Azure HDInsight zu extrahieren, zu transformieren und zu laden (ETL).
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/28/2020
ms.openlocfilehash: bcf2f39423f033ccd5bfdb6bf51ebc89e254f802
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104867813"
---
# <a name="use-apache-hive-as-an-extract-transform-and-load-etl-tool"></a>Verwenden von Apache Hive als Tool zum Extrahieren, Transformieren und Laden (ETL)

In der Regel müssen Sie eingehende Daten bereinigen und transformieren, bevor Sie sie auf ein Ziel laden, das für Analysezwecke geeignet ist. ETL-Vorgänge (Extrahieren, Transformieren und Laden) werden genutzt, um Daten vorzubereiten und auf ein Datenziel zu laden.  Apache Hive in HDInsight kann unstrukturierte Daten lesen, die Daten je nach Bedarf verarbeiten und sie dann in ein relationales Data Warehouse für Systeme zur Entscheidungsunterstützung laden. Bei dieser Vorgehensweise werden Daten aus der Quelle extrahiert. Dann werden sie in anpassbarem Speicher gespeichert, z. B in Azure Storage-Blobs oder Azure Data Lake Storage. Im nächsten Schritt werden die Daten mithilfe einer Abfolge von Hive-Abfragen transformiert. Schließlich werden sie innerhalb von Hive bereitgestellt, um das Massenladen in den Zieldatenspeicher vorzubereiten.

## <a name="use-case-and-model-overview"></a>Übersicht über den Anwendungsfall und das Modell

Die folgende Abbildung enthält eine Übersicht über den Anwendungsfall und das Modell für die ETL-Automatisierung. Eingabedaten werden transformiert, um die jeweilige Ausgabe zu generieren.  Während dieser Transformation wird bei den Daten die Form, der Datentyp und sogar die Sprache geändert.  Für ETL-Prozesse können Maße von „Britisch“ in „Metrisch“ konvertiert, Zeitzonen geändert und die Genauigkeit verbessert werden, um eine Anpassung an die vorhandenen Daten am Ziel durchzuführen. Außerdem können für ETL-Prozesse neue Daten mit vorhandenen Daten kombiniert werden, um die Berichterstellung aktuell zu halten oder weitere Einblicke in vorhandene Daten zu ermöglichen. Anwendungen, z. B. Berichterstellungstools und Dienste, können diese Daten dann im gewünschten Format nutzen.

:::image type="content" source="./media/apache-hadoop-using-apache-hive-as-an-etl-tool/hdinsight-etl-architecture.png" alt-text="Apache Hive als ETL-Architektur" border="false":::

Hadoop wird normalerweise in ETL-Prozessen verwendet, bei denen eine große Anzahl von Textdateien (z B. CSV-Dateien) importiert wird. Oder es wird eine kleinere, aber sich häufig ändernde Anzahl von Textdateien (oder aber beides) importiert.  Hive ist ein hervorragendes Tool zum Vorbereiten der Daten, bevor diese auf das Datenziel geladen werden.  Hive ermöglicht Ihnen die Erstellung eines Schemas für CSV und die Verwendung einer SQL-ähnlichen Sprache zum Generieren von MapReduce-Programmen, die mit den Daten interagieren.

Die typischen Schritte zur Ausführung von ETL-Vorgängen mithilfe von Hive lauten wie folgt:

1. Laden Sie Daten in Azure Data Lake Storage oder Azure Blob Storage.
2. Erstellen Sie eine Metadatenspeicher-Datenbank (mit Azure SQL-Datenbank), die von Hive beim Speichern Ihrer Schemas verwendet werden kann.
3. Erstellen Sie einen HDInsight-Cluster, und stellen Sie eine Verbindung mit dem Datenspeicher her.
4. Definieren Sie das Schema, das zur Lesezeit auf Daten im Datenspeicher angewendet werden soll:

    ```hql
    DROP TABLE IF EXISTS hvac;

    --create the hvac table on comma-separated sensor data stored in Azure Storage blobs

    CREATE EXTERNAL TABLE hvac(`date` STRING, time STRING, targettemp BIGINT,
        actualtemp BIGINT,
        system BIGINT,
        systemage BIGINT,
        buildingid BIGINT)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    STORED AS TEXTFILE LOCATION 'wasbs://{container}@{storageaccount}.blob.core.windows.net/HdiSamples/SensorSampleData/hvac/';
    ```

5. Transformieren Sie die Daten, und laden Sie sie auf das Ziel.  Es gibt mehrere Möglichkeiten, Hive während der Transformation und beim Laden zu verwenden:

    * Führen Sie mit Hive das Abfragen und Vorbereiten der Daten durch. Speichern Sie sie im CSV-Format in Azure Data Lake Storage oder Azure Blob Storage.  Verwenden Sie anschließend ein Tool wie SQL Server Integration Services (SSIS), um diese CSVs zu beschaffen, und laden Sie die Daten dann in eine relationale Zieldatenbank, z.B. SQL Server.
    * Fragen Sie die Daten aus Excel oder über C# ab, indem Sie den Hive ODBC-Treiber verwenden.
    * Verwenden Sie [Apache Sqoop](apache-hadoop-use-sqoop-mac-linux.md), um die vorbereiteten CSV-Flatfiles zu lesen, und laden Sie sie in die relationale Zieldatenbank.

## <a name="data-sources"></a>Datenquellen

Bei Datenquellen handelt es sich in der Regel um externe Daten, die an vorhandene Daten in Ihrem Datenspeicher angepasst werden können, z.B.:

* Social Media-Daten, Protokolldateien, Sensoren und Anwendungen, die Datendateien generieren.
* Von Datenanbietern beschaffte Datasets, z.B. Wetterstatistiken oder Verkaufszahlen von Herstellern.
* Streamingdaten, die über ein geeignetes Tool oder Framework erfasst, gefiltert und verarbeitet werden.

<!-- TODO: (see Collecting and loading data into HDInsight). -->

## <a name="output-targets"></a>Ausgabeziele

Sie können Daten mithilfe von Hive auf verschiedenen Arten von Zielen ausgeben, z. B.:

* Eine relationale Datenbank, z.B. SQL Server oder Azure SQL-Datenbank
* Ein Data Warehouse, z. B. Azure Synapse Analytics.
* Excel
* Azure-Tabellen- und -Blobspeicher
* Anwendungen oder Dienste, für die Daten in bestimmten Formaten oder als Dateien mit bestimmten Arten von Informationsstrukturen verarbeitet werden müssen.
* Ein JSON-Dokumentspeicher wie Azure Cosmos DB.

## <a name="considerations"></a>Überlegungen

Das ETL-Modell wird normalerweise für die folgenden Fälle verwendet:

`*` Laden von Streamingdaten oder großen Mengen von halb- oder unstrukturierten Daten aus externen Quellen in eine vorhandene Datenbank oder ein Informationssystem.
`*` Bereinigen, Transformieren und Überprüfen der Daten vor dem Laden, ggf. mit mehr als einem Transformationsdurchlauf durch den Cluster.
`*` Generieren von Berichten und Visualisierungen, die regelmäßig aktualisiert werden. Falls die Berichterstellung während des Tags beispielsweise zu lange dauert, können Sie die Erstellung des Berichts für die Nacht planen. Um eine Hive-Abfrage automatisch auszuführen, können Sie [Azure Logic Apps](../../logic-apps/logic-apps-overview.md) und PowerShell verwenden.

Wenn das Ziel für die Daten keine Datenbank ist, können Sie eine Datei im passenden Format innerhalb der Abfrage generieren, z.B. eine CSV-Datei. Diese Datei kann dann in Excel oder Power BI importiert werden.

Falls Sie im Rahmen des ETL-Prozesses mehrere Vorgänge für die Daten ausführen müssen, sollten Sie die damit verbundene Verwaltung berücksichtigen. Entscheiden Sie bei Vorgängen, die durch ein externes Programm (und nicht als Workflow innerhalb der Lösung) gesteuert werden, ob einige Vorgänge parallel ausgeführt werden können. Und ob erkannt werden soll, wann jeder Auftrag abgeschlossen ist. Die Verwendung eines Workflowmechanismus wie Oozie in Hadoop kann einfacher als der Versuch sein, mithilfe von externen Skripts oder benutzerdefinierten Programmen eine Sequenz von Vorgängen zu orchestrieren.

## <a name="next-steps"></a>Nächste Schritte

* [Bedarfsorientiertes Extrahieren, Transformieren und Laden (ETL)](apache-hadoop-etl-at-scale.md)
* [`Operationalize a data pipeline`](../hdinsight-operationalize-data-pipeline.md)
