---
title: Datenflussaktivität
description: 'Gewusst wie: Ausführen von Datenflüssen aus einer Data Factory-Pipeline heraus.'
services: data-factory
documentationcenter: ''
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: makromer
ms.date: 04/30/2020
ms.openlocfilehash: 1004f7fcc8ff93a170b724a6d8b1c2216b9c39b8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84726961"
---
# <a name="data-flow-activity-in-azure-data-factory"></a>Datenflussaktivität in Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Verwenden Sie die Datenflussaktivität, um Daten mithilfe von Mapping Data Flow zu transformieren und zu verschieben. Wenn Sie mit Datenflüssen noch nicht vertraut sind, finden Sie weitere Informationen in der [Übersicht über Mapping Data Flow](concepts-data-flow-overview.md).

## <a name="syntax"></a>Syntax

```json
{
    "name": "MyDataFlowActivity",
    "type": "ExecuteDataFlow",
    "typeProperties": {
      "dataflow": {
         "referenceName": "MyDataFlow",
         "type": "DataFlowReference"
      },
      "compute": {
         "coreCount": 8,
         "computeType": "General"
      },
      "staging": {
          "linkedService": {
              "referenceName": "MyStagingLinkedService",
              "type": "LinkedServiceReference"
          },
          "folderPath": "my-container/my-folder"
      },
      "integrationRuntime": {
          "referenceName": "MyDataFlowIntegrationRuntime",
          "type": "IntegrationRuntimeReference"
      }
}

```

## <a name="type-properties"></a>Typeigenschaften

Eigenschaft | BESCHREIBUNG | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | --------
dataflow | Der Verweis auf den Datenfluss, der ausgeführt wird. | DataFlowReference | Ja
integrationRuntime | Die Computeumgebung, in der der Datenfluss ausgeführt wird. Ohne Angabe wird die Azure Integration Runtime mit automatischer Auflösung verwendet. | IntegrationRuntimeReference | Nein
compute.coreCount | Die Anzahl von Kernen, die im Spark-Cluster verwendet werden. Kann nur angegeben werden, wenn die Azure Integration Runtime mit automatischer Auflösung verwendet wird. | 8, 16, 32, 48, 80, 144, 272 | Nein
compute.computeType | Der Computetyp, der im Spark-Cluster verwendet wird. Kann nur angegeben werden, wenn die Azure Integration Runtime mit automatischer Auflösung verwendet wird. | "General", "ComputeOptimized", "MemoryOptimized" | Nein
staging.linkedService | Das für das PolyBase-Staging verwendete Speicherkonto, wenn Sie eine SQL Data Warehouse-Quelle oder -Senke verwenden. | LinkedServiceReference | Nur, wenn der Datenfluss Lese-oder Schreibvorgänge in SQL Data Warehouse ausführt.
staging.folderPath | Der für das PolyBase-Staging verwendete Ordnerpfad im Blob-Speicherkonto, wenn Sie eine SQL Data Warehouse-Quelle oder -Senke verwenden. | String | Nur, wenn der Datenfluss Lese-oder Schreibvorgänge in SQL Data Warehouse ausführt.

![Ausführen eines Datenflusses](media/data-flow/activity-data-flow.png "Ausführen eines Datenflusses")

### <a name="dynamically-size-data-flow-compute-at-runtime"></a>Dynamisches Bestimmen der Größe der Computekapazität für den Datenfluss zur Laufzeit

Die Eigenschaften „Anzahl Kerne“ und „Computetyp“ können dynamisch festgelegt werden, um die Größe der eingehenden Quelldaten zur Laufzeit anzupassen. Verwenden Sie Pipelineaktivitäten wie „Nachschlagen“ oder „Metadaten abrufen“, um die Größe der Daten im Quelldatenset zu bestimmen. Verwenden Sie dann in den Eigenschaften der Datenflussaktivität „Dynamischen Inhalt hinzufügen“.

![Dynamischer Datenfluss](media/data-flow/dyna1.png "Dynamischer Datenfluss")

[Es folgt ein kurzes Videotutorial, das diese Technik erläutert.](https://www.youtube.com/watch?v=jWSkJdtiJNM)

### <a name="data-flow-integration-runtime"></a>Datenfluss-Integration Runtime

Wählen Sie die Integration Runtime aus, die für die Ausführung Ihrer Datenflussaktivität verwendet werden soll. Standardmäßig verwendet Data Factory die Azure Integration Runtime mit automatischer Auflösung, vier Workerkernen und ohne Gültigkeitsdauer (TTL). Diese Integration Runtime weist einen allgemeinen Computetyp auf und wird in derselben Region wie Ihre Factory ausgeführt. Sie können Ihre eigene Azure Integration Runtime erstellen, die bestimmte Regionen, den Computetyp, die Kernanzahl und die Gültigkeitsdauer (TTL) für die Ausführung Ihrer Datenflussaktivität definiert.

Bei Pipelineausführungen ist der Cluster ein Auftragscluster, der einige Minuten zum Starten braucht, bevor die Ausführung gestartet werden kann. Wenn keine Gültigkeitsdauer festgelegt ist, wird diese Startzeit bei jeder Pipelineausführung benötigt. Wenn Sie eine Gültigkeitsdauer angeben, bleibt ein „warmer“ Clusterpool über die angegebene Zeit nach der letzten Ausführung aktiv. Dies führt zu kürzeren Startzeiten. Wenn Sie beispielsweise eine Gültigkeitsdauer von 60 Minuten festgelegt haben und einmal pro Stunde einen Datenfluss ausführen, bleibt der Clusterpool aktiv. Weitere Informationen finden Sie unter [Azure Integration Runtime](concepts-integration-runtime.md).

![Azure Integration Runtime](media/data-flow/ir-new.png "Azure Integration Runtime")

> [!IMPORTANT]
> Die Auswahl von Integration Runtime in der Datenflussaktivität bezieht sich nur auf *getriggerte Ausführungen* Ihrer Pipeline. Das Debuggen Ihrer Pipeline mit Datenflüssen wird auf dem in der Debugsitzung angegebenen Cluster ausgeführt.

### <a name="polybase"></a>PolyBase

Wenn Sie Azure SQL Data Warehouse als Senke oder Quelle verwenden, müssen Sie einen Stagingspeicherort für Ihren PolyBase-Batchladevorgang auswählen. PolyBase ermöglicht das Batchladen per Massenvorgang, anstatt die Daten zeilenweise zu laden. Mit PolyBase wird die Zeit für das Laden in SQL Data Warehouse drastisch reduziert.

## <a name="parameterizing-data-flows"></a>Parametrisieren von Datenflüssen

### <a name="parameterized-datasets"></a>Parametrisierte Datasets

Wenn im Datenfluss parametrisierte Datasets verwendet werden, legen Sie die Parameterwerte auf der Registerkarte **Einstellungen** fest.

![Ausführen von Datenflussparametern](media/data-flow/params.png "Parameter")

### <a name="parameterized-data-flows"></a>Parametrisierte Datenflüsse

Wenn der Datenfluss parametrisiert ist, legen Sie die dynamischen Werte der Datenflussparameter auf der Registerkarte **Parameter** fest. Sie können entweder die Ausdruckssprache für die ADF-Pipeline oder die Ausdruckssprache für Datenflüsse verwenden, um dynamische oder literale Parameterwerte zuzuweisen. Weitere Informationen finden Sie unter [Datenflussparameter](parameters-data-flow.md).

### <a name="parameterized-compute-properties"></a>Parametrisierte Compute-Eigenschaften.

Sie können die Anzahl von Kernen oder den Computetyp parametrisieren, wenn Sie die Azure Integration Runtime mit automatischer Auflösung verwenden und Werte für compute.corecount und compute.computetype angeben.

![Ausführen von Datenflussparametern (Beispiel)](media/data-flow/parameterize-compute.png "Parameterbeispiel")

## <a name="pipeline-debug-of-data-flow-activity"></a>Debuggen der Pipeline der Datenflussaktivität

Wenn Sie ein Debugging der Pipeline mit einer Datenflussaktivität ausführen möchten, müssen Sie den Datenfluss-Debugmodus über den Schieberegler **Datenfluss debuggen** auf der oberen Leiste aktivieren. Im Debugmodus können Sie den Datenfluss für einen aktiven Spark-Cluster ausführen. Weitere Informationen finden Sie unter [Debugmodus](concepts-data-flow-debug-mode.md).

![Schaltfläche „Debuggen“](media/data-flow/debugbutton.png "Schaltfläche „Debuggen“")

Das Debuggen der Pipeline wird für den aktiven Debugcluster und nicht für die Integration Runtime-Umgebung ausgeführt, die in den Einstellungen für die Datenflussaktivität angegeben ist. Beim Starten des Debugmodus können Sie die Computeumgebung für das Debuggen auswählen.

## <a name="monitoring-the-data-flow-activity"></a>Überwachen der Datenflussaktivität

Die Datenflussaktivität verfügt über eine besondere Überwachungsoberfläche, auf der Sie Informationen zu Partitionierung, Phasenzeit und Datenherkunft anzeigen können. Sie öffnen den Überwachungsbereich über das Brillensymbol unter **Aktionen**. Weitere Informationen finden Sie unter [Überwachen von Datenflüssen](concepts-data-flow-monitoring.md).

### <a name="use-data-flow-activity-results-in-a-subsequent-activity"></a>Verwenden der Ergebnisse der Datenflussaktivität in einer nachfolgenden Aktivität

Die Datenflussaktivität gibt Metriken bezüglich der Anzahl der Zeilen aus, die in jeder Senke geschrieben werden und die in jeder Quelle gelesen werden. Diese Ergebnisse werden im Abschnitt `output` des Ergebnisses der Aktivitätsausführung zurückgegeben. Die zurückgegebenen Metriken weisen das Format des folgenden JSON auf.

``` json
{
    "runStatus": {
        "metrics": {
            "<your sink name1>": {
                "rowsWritten": <number of rows written>,
                "sinkProcessingTime": <sink processing time in ms>,
                "sources": {
                    "<your source name1>": {
                        "rowsRead": <number of rows read>
                    },
                    "<your source name2>": {
                        "rowsRead": <number of rows read>
                    },
                    ...
                }
            },
            "<your sink name2>": {
                ...
            },
            ...
        }
    }
}
```

Verwenden Sie beispielsweise `@activity('dataflowActivity').output.runStatus.metrics.sink1.rowsWritten`, um die Anzahl der in eine Senke namens „sink1“ in einer Aktivität namens „dataflowActivity“ geschriebenen Zeile zu erhalten.

Verwenden Sie `@activity('dataflowActivity').output.runStatus.metrics.sink1.sources.source1.rowsRead`, um die Anzahl der von einer Quelle namens „source1“ gelesenen Zeile abzurufen, die in dieser Senke verwendet wurde.

> [!NOTE]
> Wenn für eine Senke keine Zeilen geschrieben wurden, wird sie nicht in Metriken angezeigt. Vorhandene Zeilen können durch die `contains`-Funktion überprüft werden. Beispielsweise überprüft `contains(activity('dataflowActivity').output.runStatus.metrics, 'sink1')`, ob Zeilen in sink1 geschrieben wurden.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über Ablaufsteuerungsaktivitäten, die von Data Factory unterstützt werden: 

- [Aktivität „If Condition“](control-flow-if-condition-activity.md)
- [Aktivität „Pipeline ausführen“](control-flow-execute-pipeline-activity.md)
- [ForEach-Aktivität](control-flow-for-each-activity.md)
- [Aktivität „Metadaten abrufen“](control-flow-get-metadata-activity.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)
- [Webaktivität](control-flow-web-activity.md)
- [Until-Aktivität](control-flow-until-activity.md)
