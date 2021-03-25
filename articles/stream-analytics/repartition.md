---
title: Verwenden der Neupartitionierung zur Optimierung von Azure Stream Analytics-Aufträgen
description: In diesem Artikel wird beschrieben, wie Sie Azure Stream Analytics Aufträge, die nicht parallelisiert werden können, mithilfe der Neupartitionierung optimieren.
ms.service: stream-analytics
author: sidramadoss
ms.author: sidram
ms.date: 03/04/2021
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 95749f2acea6b605cfdba5a4f3d4f5526e751c5a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102182535"
---
# <a name="use-repartitioning-to-optimize-processing-with-azure-stream-analytics"></a>Verwenden der Neupartitionierung zur Optimierung der Verarbeitung mit Azure Stream Analytics

In diesem Artikel erfahren Sie, wie Sie mit der Neupartitionierung Ihre Azure Stream Analytics-Abfrage für Szenarien skalieren, die nicht vollständig [parallelisiert](stream-analytics-scale-jobs.md) werden können.

Die Verwendung der [Parallelisierung](stream-analytics-parallelization.md) ist in folgenden Situationen möglicherweise nicht möglich:

* Sie verfügen nicht über den Partitionsschlüssel für den Eingabestream.
* Ihre Quelle verteilt die Eingaben auf mehrere Partitionen, die später zusammengeführt werden müssen.

Eine Neupartitionierung oder „Umsortierung“ ist erforderlich, wenn Sie Daten in einem Stream verarbeiten, der nicht nach einem natürlichen Eingabeschema (z. B. nach der **Partitions-ID** für Event Hubs) horizontal partitioniert wird. Nach einer Neupartitionierung kann jeder Shard unabhängig verarbeitet werden, sodass Sie die Streamingpipeline linear skalieren können. 

## <a name="how-to-repartition"></a>Neupartitionierung
Sie können Ihre Eingabe auf zwei Arten neu partitionieren:
1. Verwenden eines separaten Stream Analytics-Auftrags für die Neupartitionierung
2. Verwenden eines einzelnen Auftrags und Durchführen der Neupartitionierung vor Ihrer benutzerdefinierten Analyselogik

### <a name="creating-a-separate-stream-analytics-job-to-repartition-input"></a>Erstellen eines separaten Stream Analytics-Auftrags zum Neupartitionieren der Eingabe
Sie können einen Auftrag erstellen, durch den die Eingabe gelesen und unter Verwendung eines Partitionsschlüssels in eine Event Hub-Ausgabe geschrieben wird. Dieser Event Hub kann dann als Eingabe für einen anderen Stream Analytics-Auftrag verwendet werden, in dem Sie Ihre Analyselogik implementieren. Wenn Sie diese Event Hub-Ausgabe in Ihrem Auftrag konfigurieren, müssen Sie den Partitionsschlüssel angeben, auf dessen Grundlage Ihre Daten von Stream Analytics neu partitioniert werden sollen. 
```sql
-- For compat level 1.2 or higher
SELECT * 
INTO output
FROM input

--For compat level 1.1 or lower
SELECT *
INTO output
FROM input PARTITION BY PartitionId
```

### <a name="repartition-input-within-a-single-stream-analytics-job"></a>Neupartitionieren der Eingabe innerhalb eines einzelnen Stream Analytics-Auftrags
Sie können auch einen Schritt in Ihre Abfrage einfügen, durch den die Eingabe zunächst neu partitioniert wird. Das Ergebnis kann dann von anderen Schritten in der Abfrage verwendet werden. Wenn Sie also beispielsweise die Eingabe auf der Grundlage der Geräte-ID (**DeviceId**) neu partitionieren möchten, lautet Ihre Abfrage wie folgt:
```sql
WITH RepartitionedInput AS 
( 
SELECT * 
FROM input PARTITION BY DeviceID
)

SELECT DeviceID, AVG(Reading) as AvgNormalReading  
INTO output
FROM RepartitionedInput  
GROUP BY DeviceId, TumblingWindow(minute, 1)  
```

In der folgenden Beispielabfrage werden zwei Streams mit neu partitionierten Daten verknüpft. Beim Verknüpfen von zwei Streams mit neu partitionierten Daten müssen diese Streams denselben Partitionsschlüssel und dieselbe Anzahl aufweisen. Das Ergebnis ist ein Stream, der über das gleiche Partitionsschema verfügt.

```sql
WITH step1 AS (SELECT * FROM input1 PARTITION BY DeviceID),
step2 AS (SELECT * FROM input2 PARTITION BY DeviceID)

SELECT * INTO output FROM step1 PARTITION BY DeviceID UNION step2 PARTITION BY DeviceID
```

Das Ausgabeschema muss mit dem Schemaschlüssel und der Anzahl im Stream identisch sein, damit jeder Teilstream unabhängig geleert werden kann. Der Stream kann auch vor dem Leeren zusammengeführt und mit einem anderen Schema neu partitioniert werden. Sie sollten diese Methode jedoch vermeiden, da sie die allgemeine Latenz bei der Verarbeitung und die Ressourcennutzung erhöht.

## <a name="streaming-units-for-repartitions"></a>Streamingeinheiten für Neupartitionierungen

Experimentieren Sie, und beobachten Sie die Ressourcennutzung Ihres Auftrags, um die genaue Anzahl der benötigten Partitionen zu ermitteln. Die Anzahl der [Streamingeinheiten](stream-analytics-streaming-unit-consumption.md) (SU) muss an die physischen Ressourcen angepasst werden, die für die einzelnen Partitionen benötigt werden. Im Allgemeinen sind für jede Partition sechs SUs erforderlich. Wenn dem Auftrag nicht genügend Ressourcen zugewiesen sind, wendet das System die Neupartitionierung nur an, wenn es für den Auftrag von Vorteil ist.

## <a name="repartitions-for-sql-output"></a>Neupartitionierungen für SQL-Ausgaben

Wenn Ihr Auftrag SQL-Datenbank für die Ausgabe verwendet, nutzen Sie die explizite Neupartitionierung, um die optimale Partitionsanzahl zum Maximieren des Durchsatzes zuzuordnen. Da SQL am besten mit acht Writern funktioniert, kann die Neupartitionierung des Flows auf acht vor dem Leeren (oder früher) die Auftragsleistung steigern. 

Wenn mehr als acht Eingabepartitionen vorliegen, ist das Erben des Eingabepartitionierungsschemas möglicherweise keine geeignete Option. Erwägen Sie die Verwendung von [INTO](/stream-analytics-query/into-azure-stream-analytics#into-shard-count) in Ihrer Abfrage, um die Zahl der Ausgabeschreiber explizit anzugeben. 

Das folgende Beispiel liest aus der Eingabe, unabhängig davon, ob sie natürlich partitioniert ist, und partitioniert den Datenstrom zehnfach entsprechend der DeviceID-Dimension neu und leert die Daten für die Ausgabe. 

```sql
SELECT * INTO [output] FROM [input] PARTITION BY DeviceID INTO 10
```

Weitere Informationen finden Sie unter [Azure Stream Analytics-Ausgabe an Azure SQL-Datenbank](stream-analytics-sql-output-perf.md).


## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Azure Stream Analytics](stream-analytics-introduction.md)
* [Nutzen der Parallelisierung von Abfragen in Azure Stream Analytics](stream-analytics-parallelization.md)
