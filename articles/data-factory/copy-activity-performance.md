---
title: Handbuch zur Leistung und Skalierbarkeit der Kopieraktivität
description: Hier erfahren Sie, welche Faktoren sich entscheidend auf die Leistung auswirken, wenn Sie Daten in Azure Data Factory mithilfe der Kopieraktivität verschieben.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/11/2020
ms.openlocfilehash: aedb3df69821d1436b03b2eb1f12873b624d426e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414171"
---
# <a name="copy-activity-performance-and-scalability-guide"></a>Handbuch zur Leistung und Skalierbarkeit der Kopieraktivität

> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version von Azure Data Factory aus:"]
> * [Version 1](v1/data-factory-copy-activity-performance.md)
> * [Aktuelle Version](copy-activity-performance.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Unabhängig davon, ob Sie eine umfangreiche Datenmigration aus Data Lake oder einem Data Warehouse auf Unternehmensniveau zu Azure durchführen möchten oder ob Sie umfassende Datenmengen aus verschiedenen Datenquellen für die Big Data-Analyse in Azure erfassen möchten, es ist unerlässlich, für die optimale Leistung und Skalierbarkeit zu sorgen.  Azure Data Factory bietet einen leistungsfähigen, robusten und kostengünstigen Mechanismus für die Erfassung von Daten im gewünschten Umfang. Dadurch ist es ideal geeignet für Datentechniker, die leistungsstarke und skalierbare Pipelines für die Datenerfassung erstellen möchten.

Nach dem Lesen dieses Artikels können Sie die folgenden Fragen beantworten:

- Welche Leistung und Skalierbarkeit kann ich mit ADF-Kopieraktivität für Datenmigrations- und Datenerfassungsszenarien erzielen?

- Welche Schritte sind durchzuführen, um die Leistung der ADF-Kopieraktivität zu optimieren?
- Welche Steuerelemente für die ADF-Leistungsoptimierung kann ich verwenden, um die Leistung für eine einzelne Kopieraktivitätsausführung zu optimieren?
- Welche anderen Faktoren außerhalb von ADF sind bei der Optimierung der Kopierleistung zu berücksichtigen?

> [!NOTE]
> Falls Sie mit dem grundlegenden Konzept der Kopieraktivität nicht vertraut sind, finden Sie weitere Informationen unter [Kopieraktivität – Übersicht](copy-activity-overview.md), bevor Sie sich mit diesem Artikel beschäftigen.

## <a name="copy-performance-and-scalability-achievable-using-adf"></a>Durch ADF erreichbare Kopierleistung und -skalierbarkeit

ADF verfügt über eine serverlose Architektur, die Parallelität auf unterschiedlichen Ebenen ermöglicht. Entwickler können dann Pipelines erstellen, um Ihre Netzwerkbandbreite vollständig zu nutzen – sowie IOPS und Bandbreite des Speichers, um den Durchsatz für die Datenverschiebung Ihrer Umgebung zu maximieren.  Das heißt, dass Sie den erzielbaren Durchsatz schätzen können, indem Sie den Mindestdurchsatz des Quelldatenspeichers und des Zieldatenspeichers sowie die Netzwerkbandbreite zwischen Quelle und Ziel messen.  In der folgenden Tabelle wird die Kopierdauer anhand der Datenmenge und des Bandbreitenlimits für Ihre Umgebung berechnet. 

| Datengröße/ <br/> bandwidth | 50 MBit/s    | 100 MBit/s  | 500 MBit/s  | 1 GBit/s   | 5 GBit/s   | 10 GBit/s  | 50 GBit/s   |
| --------------------------- | ---------- | --------- | --------- | -------- | -------- | -------- | --------- |
| **1 GB**                    | 2,7 min    | 1,4 min   | 0,3 min   | 0,1 min  | 0,03 min | 0,01 min | 0,0 min   |
| **10 GB**                   | 27,3 min   | 13,7 min  | 2,7 min   | 1,3 min  | 0,3 min  | 0,1 min  | 0,03 min  |
| **100 GB**                  | 4,6 h    | 2,3 h   | 0,5 h   | 0,2 h  | 0,05 h | 0,02 h | 0,0 h   |
| **1 TB**                    | 46,6 h   | 23,3 h  | 4,7 h   | 2,3 h  | 0,5 h  | 0,2 h  | 0,05 h  |
| **10 TB**                   | 19,4 Tage  | 9,7 Tage  | 1,9 Tage  | 0,9 Tage | 0,2 Tage | 0,1 Tage | 0,02 Tage |
| **100 TB**                  | 194,2 Tage | 97,1 Tage | 19,4 Tage | 9,7 Tage | 1,9 Tage | 1 Tag    | 0,2 Tage  |
| **1 PB**                    | 64,7 Monate    | 32,4 Monate   | 6,5 Monate    | 3,2 Monate   | 0,6 Monate   | 0,3 Monate   | 0,06 Monate   |
| **10 PB**                   | 647,3 Monate   | 323,6 Monate  | 64,7 Monate   | 31,6 Monate  | 6,5 Monate   | 3,2 Monate   | 0,6 Monate    |

Die ADF-Kopieraktivität kann auf verschiedenen Ebenen skaliert werden:

![Skalierung der ADF-Kopieraktivität](media/copy-activity-performance/adf-copy-scalability.png)

- Mit der ADF-Ablaufsteuerung können mehrere Kopieraktivitäten parallel gestartet werden, z. B. per [foreach-Schleife](control-flow-for-each-activity.md).
- Für eine Kopieraktivität können skalierbare Computeressourcen genutzt werden: Bei Verwendung der Azure Integration Runtime können Sie [bis zu 256 Datenintegrationseinheiten (DIUs)](#data-integration-units) serverlos für jede Kopieraktivität angeben. Wenn Sie die selbstgehostete Integration Runtime nutzen, können Sie den Computer manuell hochskalieren oder das Aufskalieren auf mehrere Computer durchführen ([bis zu vier Knoten](create-self-hosted-integration-runtime.md#high-availability-and-scalability)). Bei einer einzelnen Kopieraktivität wird die Dateigruppe dann auf alle Knoten verteilt.
- Für eine einzelne Kopieraktivität werden mehrere Threads [parallel](#parallel-copy) genutzt, um für den Datenspeicher Lese- und Schreibvorgänge durchzuführen.

## <a name="performance-tuning-steps"></a>Schritte zur Optimierung der Leistung

Führen Sie diese Schritte aus, um die Leistung des Azure Data Factory-Diensts mit der Kopieraktivität zu verbessern.

1. **Wählen Sie ein Testdataset aus, und legen Sie eine Baseline fest.** Testen Sie Ihre Pipeline mit der Kopieraktivität in der Entwicklungsphase mit repräsentativen Beispieldaten. Das ausgewählte Dataset sollte Ihre üblichen Datenmuster widerspiegeln (Ordnerstruktur, Dateimuster, Datenschema usw.) und groß genug sein, um die Kopierleistung bewerten zu können (also ob eine Kopieraktivität zehn Minuten oder länger dauert). Sammeln Sie Ausführungsdetails und Leistungsmerkmale, und folgen Sie dabei der [Überwachung der Kopieraktivität](copy-activity-monitoring.md).

2. **So maximieren Sie die Leistung einer einzelnen Kopieraktivität**:

   Für den Anfang empfehlen wir, dass Sie zunächst die Leistung mit einer einzelnen Kopieraktivität maximieren.

   - **Wenn die Kopieraktivität für eine Azure Integration Runtime-Instanz ausgeführt wird:** Beginnen Sie mit Standardwerten für [Datenintegrationseinheiten (Data Integration Units, DIU)](#data-integration-units), und führen Sie für die Einstellungen das [parallele Kopieren](#parallel-copy) durch. 

   - **Wenn die Kopieraktivität für eine selbstgehostete Integration Runtime ausgeführt wird:** Wir empfehlen Ihnen, zum Hosten der Integration Runtime einen anderen dedizierten Computer als den Server zu nutzen, auf dem der Datenspeicher gehostet wird. Beginnen Sie mit den Standardwerten für [Paralleles Kopieren](#parallel-copy), und verwenden Sie einen einzelnen Knoten für die selbstgehostete Integration Runtime.  

   Führen Sie einen Leistungstestlauf aus, und notieren Sie sich sowohl die erzielte Leistung als auch die tatsächlich genutzten Werte, z. B. für DIUs und parallele Kopiervorgänge. Unter [Überwachung der Kopieraktivität](copy-activity-monitoring.md) finden Sie Informationen dazu, wie Sie die Ausführungsergebnisse und die verwendeten Leistungseinstellungen erfassen. Informieren Sie sich auch über die [Problembehandlung für die Leistung der Kopieraktivität](copy-activity-performance-troubleshooting.md), um den Engpass zu ermitteln und zu beseitigen. 

   Führen Sie weitere Leistungstests durch, indem Sie den Leitfaden für die Problembehandlung und die Optimierung verwenden. Wenn für eine einzelne Ausführung einer Kopieraktivität kein besserer Durchsatz mehr erzielt werden kann, sollten Sie eine Maximierung des aggregierten Durchsatzes erwägen, indem Sie mehrere Kopiervorgänge gleichzeitig durchführen (Schritt 3).


3. **So maximieren Sie den aggregierten Durchsatz durch paralleles Ausführen mehrerer Kopiervorgänge:**

   Nun haben Sie die Leistung einer einzelnen Kopieraktivität maximiert. Wenn Sie die Durchsatzobergrenzen Ihrer Umgebung (Netzwerk, Quelldatenspeicher und Zieldatenspeicher) noch nicht erreicht haben, können Sie mit ADF-Flusssteuerungskonstrukten wie [Für jede Schleife](control-flow-for-each-activity.md) mehrere Kopieraktivitäten parallel ausführen. Die Lösungsvorlagen [Kopieren von Dateien aus mehreren Containern](solution-template-copy-files-multiple-containers.md), [Migrieren von Daten aus Amazon S3 zu ADLS Gen2](solution-template-migration-s3-azure.md) und [Massenkopieren mit Steuertabelle](solution-template-bulk-copy-with-control-table.md) dienen hier als allgemeine Beispiele.

5. **Erweitern der Konfiguration auf das gesamte Dataset.** Wenn Sie mit den Ausführungsergebnissen und mit der Leistung zufrieden sind, können Sie die Definition und die Pipeline auf Ihr gesamtes Dataset erweitern.

## <a name="troubleshoot-copy-activity-performance"></a>Problembehandlung für die Leistung der Kopieraktivität

Führen Sie die [Schritte zur Optimierung der Leistung](#performance-tuning-steps) aus, um den Leistungstest für Ihr Szenario zu planen und durchzuführen. Informieren Sie sich auch unter [Problembehandlung für die Leistung der Kopieraktivität](copy-activity-performance-troubleshooting.md) darüber, wie Sie Probleme bei der Ausführung von einzelnen Kopieraktivitäten in Azure Data Factory beheben.

## <a name="copy-performance-optimization-features"></a>Features für die Leistungsoptimierung von Kopiervorgängen

Azure Data Factory bietet die folgenden Funktionen zur Leistungsoptimierung:

- [Datenintegrationseinheiten](#data-integration-units)
- [Skalierbarkeit der selbstgehosteten Integration Runtime-Infrastruktur](#self-hosted-integration-runtime-scalability)
- [Parallele Kopie](#parallel-copy)
- [Gestaffeltes Kopieren](#staged-copy)

### <a name="data-integration-units"></a>Datenintegrationseinheiten

Eine Datenintegrationseinheit ist eine Messgröße für die Leistungsfähigkeit (Kombination aus zugeteilten CPU-, Speicher- und Netzwerkressourcen) einer einzelnen Einheit in Azure Data Factory. Die Datenintegrationseinheit gilt nur für die [Azure-Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime), aber nicht für [selbstgehostete Integration Runtimes](concepts-integration-runtime.md#self-hosted-integration-runtime). [Weitere Informationen](copy-activity-performance-features.md#data-integration-units)

### <a name="self-hosted-integration-runtime-scalability"></a>Skalierbarkeit der selbstgehosteten Integration Runtime (IR)

Zum Hosten einer größeren gleichzeitigen Workload oder zum Erzielen einer höheren Leistung können Sie für die selbstgehostete Integration Runtime entweder das Hochskalieren oder das Aufskalieren durchführen. [Weitere Informationen](copy-activity-performance-features.md#self-hosted-integration-runtime-scalability)

### <a name="parallel-copy"></a>Parallele Kopie

Sie können das parallele Kopieren festlegen, um die Parallelität anzugeben, die für die Kopieraktivität verwendet werden soll. Diese Eigenschaft können Sie sich als die maximale Anzahl von Threads in der Kopieraktivität vorstellen, die parallel aus Ihrer Datenquelle lesen oder in Ihre Senkendatenspeicher schreiben. [Weitere Informationen](copy-activity-performance-features.md#parallel-copy)

### <a name="staged-copy"></a>gestaffeltem Kopieren

Beim Kopieren von Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher können Sie ggf. Blob Storage als Stagingzwischenspeicher verwenden. [Weitere Informationen](copy-activity-performance-features.md#staged-copy)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den anderen Artikeln zur Kopieraktivität:

- [Kopieraktivität – Übersicht](copy-activity-overview.md)
- [Troubleshoot copy activity performance](copy-activity-performance-troubleshooting.md) (Problembehandlung bei der Leistung der Kopieraktivität)
- [Features für die Leistungsoptimierung bei Kopieraktivitäten](copy-activity-performance-features.md)
- [Verwenden von Azure Data Factory zum Migrieren von Daten aus einem Data Lake oder Data Warehouse zu Azure](data-migration-guidance-overview.md)
- [Migrieren von Daten aus Amazon S3 zu Azure Storage](data-migration-guidance-s3-azure-storage.md)
