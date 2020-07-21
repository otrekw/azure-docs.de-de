---
title: Features für die Leistungsoptimierung bei Kopieraktivitäten
description: Erfahren Sie mehr über die wichtigsten Funktionen, mit denen Sie die Leistung bei Kopieraktivitäten in Azure Data Factory optimieren können.
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
ms.date: 06/15/2020
ms.openlocfilehash: dfd439affe488805b4645211477c6d32bbbe7489
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84770933"
---
# <a name="copy-activity-performance-optimization-features"></a>Features für die Leistungsoptimierung bei Kopieraktivitäten

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel werden die Features für die Leistungsoptimierung bei Kopieraktivitäten beschrieben, die Sie in Azure Data Factory nutzen können.

## <a name="data-integration-units"></a>Datenintegrationseinheiten

Eine Datenintegrationseinheit ist eine Messgröße für die Leistungsfähigkeit (Kombination aus zugeteilten CPU-, Speicher- und Netzwerkressourcen) einer einzelnen Einheit in Azure Data Factory. Die Datenintegrationseinheit gilt nur für die [Azure-Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime), aber nicht für [selbstgehostete Integration Runtimes](concepts-integration-runtime.md#self-hosted-integration-runtime).

Die zulässige Anzahl von DIUs für die Ausführung einer Kopieraktivität liegt zwischen **2 und 256**. Wenn diese Option nicht angegeben ist oder Sie auf der Benutzeroberfläche „Auto“ auswählen, wendet die Data Factory dynamisch die optimale DIU-Einstellung basierend auf dem Quelle-Senke-Paar und dem Datenmuster an. In der nachstehenden Tabelle sind die unterstützten DIU-Bereiche und das Standardverhalten in verschiedenen Kopierszenarien aufgeführt:

| Kopierszenario | Unterstützter DIU-Bereich | Vom Dienst bestimmte Standard-DIUs |
|:--- |:--- |---- |
| Zwischen Dateispeichern |- **Kopieren aus einer einzelnen oder in eine einzelne Datei**: 2–4 <br>- **Kopieren aus mehreren oder in mehrere Dateien**: 2–256, je nach Anzahl und Größe der Dateien <br><br>Wenn Sie beispielsweise Daten aus einem Ordner mit 4 großen Dateien kopieren und die Hierarchie beibehalten möchten, ist die maximale effektive DIU gleich 16. Wenn Sie sich für das Zusammenführen der Datei entscheiden, ist die maximale effektive DIU gleich 4. |Zwischen 4 und 32, je nach Anzahl und Größe der Dateien |
| Aus Dateispeicher in Nicht-Dateispeicher |- **Kopieren aus einer einzelnen Datei**: 2–4 <br/>- **Kopieren aus mehreren Dateien**: 2–256, je nach Anzahl und Größe der Dateien <br/><br/>Wenn Sie beispielsweise Daten aus einem Ordner mit 4 großen Dateien kopieren, ist die maximale effektive DIU gleich 16. |- **Kopieren in Azure SQL-Datenbank oder Azure Cosmos DB**: zwischen 4 und 16, je nach der Senkenebene (DTUs/RUs) und dem Quelldateimuster<br>- **Kopieren in Azure Synapse Analytics** mithilfe von PolyBase oder der COPY-Anweisung: 2<br>– Anderes Szenario: 4 |
| Aus Nicht-Dateispeicher in Dateispeicher |- **Kopieren aus Datenspeichern mit aktivierter Partitionsoption** (einschließlich [Oracle](connector-oracle.md#oracle-as-source)/[Netezza](connector-netezza.md#netezza-as-source)/[Teradata](connector-teradata.md#teradata-as-source)): 2–256 beim Schreiben in einen Ordner und 2–4 beim Schreiben in eine einzelne Datei. Beachten Sie, dass pro Quelldatenpartition bis zu 4 DIUs verwendet werden können.<br>- **Andere Szenarien**: 2–4 |- **Kopieren aus REST oder HTTP**: 1<br/>- **Kopieren aus Amazon Redshift** mithilfe von UNLOAD: 2<br>- **Anderes Szenario**: 4 |
| Zwischen Nicht-Dateispeichern |- **Kopieren aus Datenspeichern mit aktivierter Partitionsoption** (einschließlich [Oracle](connector-oracle.md#oracle-as-source)/[Netezza](connector-netezza.md#netezza-as-source)/[Teradata](connector-teradata.md#teradata-as-source)): 2–256 beim Schreiben in einen Ordner und 2–4 beim Schreiben in eine einzelne Datei. Beachten Sie, dass pro Quelldatenpartition bis zu 4 DIUs verwendet werden können.<br/>- **Andere Szenarien**: 2–4 |- **Kopieren aus REST oder HTTP**: 1<br>- **Anderes Szenario**: 4 |

Sie können die für die einzelnen Kopiervorgänge verwendeten DIUs in der Überwachungsansicht der Kopieraktivität oder der Aktivitätsausgabe anzeigen. Weitere Informationen finden Sie unter [Überwachung der Kopieraktivität](copy-activity-monitoring.md). Wenn Sie diese Standardeinstellung überschreiben möchten, geben Sie einen Wert für die Eigenschaft `dataIntegrationUnits` wie folgt an. Die *tatsächliche Anzahl von DIUs*, die der Kopiervorgang zur Laufzeit verwendet, entspricht maximal dem konfigurierten Wert. Dies ist abhängig von Ihrem Datenmuster.

Ihnen wird die **Anzahl der verwendeten DIUs \* Kopierdauer \* Einheitenpreis/DIU-Stunde** in Rechnung gestellt. Sehen Sie sich [hier](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) die aktuellen Preise an. Je nach Abonnementtyp können lokale Währungen und separate Rabatte gelten.

**Beispiel:**

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "dataIntegrationUnits": 128
        }
    }
]
```

## <a name="self-hosted-integration-runtime-scalability"></a>Skalierbarkeit der selbstgehosteten Integration Runtime (IR)

Wenn Sie einen höheren Durchsatz erreichen möchten, können Sie die selbstgehostete IR zentral oder horizontal hochskalieren:

- Wenn die CPU und der verfügbare Arbeitsspeicher auf dem Knoten der selbstgehosteten IR nicht vollständig genutzt werden, bei der Ausführung paralleler Aufträge aber das Limit erreicht wird, sollten Sie eine zentrale Hochskalierung durchführen, indem Sie die Anzahl der parallelen Aufträge vergrößern, die auf einem Knoten ausgeführt werden können.  Anweisungen dazu finden Sie [hier](create-self-hosted-integration-runtime.md#scale-up).
- Ist jedoch die CPU-Auslastung auf dem Knoten der selbstgehosteten IR hoch oder der verfügbare Arbeitsspeicher gering, können Sie einen neuen Knoten hinzufügen, um die Last über mehrere Knoten horizontal zu skalieren.  Anweisungen dazu finden Sie [hier](create-self-hosted-integration-runtime.md#high-availability-and-scalability).

Beachten Sie in den folgenden Szenarien, dass bei der Ausführung einer einzelnen Kopieraktivität mehrere Knoten einer selbstgehosteten IR genutzt werden können:

- Kopieren Sie Daten aus dateibasierten Speichern, je nach Anzahl und Größe der Dateien.
- Kopieren Sie Daten aus einem Datenspeicher mit aktivierter Partitionsoption (darunter [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [SAP-Tabelle](connector-sap-table.md#sap-table-as-source) und [SAP Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source)), je nach Anzahl der Datenpartitionen.

## <a name="parallel-copy"></a>Parallele Kopie

Sie können eine parallele Kopie (Eigenschaft `parallelCopies`) für die Kopieraktivität festlegen, um die Parallelität anzugeben, die von der Kopieraktivität verwendet werden soll. Diese Eigenschaft können Sie sich als die maximale Anzahl von Threads in der Kopieraktivität vorstellen, die parallel aus Ihrer Datenquelle lesen oder in Ihre Senkendatenspeicher schreiben.

Die parallele Kopie ist orthogonal zu [Datenintegrationseinheiten](#data-integration-units) oder [Knoten der selbstgehosteten IR](#self-hosted-integration-runtime-scalability). Sie wird für alle DIUs oder Knoten der selbstgehosteten IR gezählt.

Bei jeder Ausführung einer Kopieraktivität wendet Azure Data Factory standardmäßig die optimale Einstellung für parallele Kopien basierend auf Ihrem Quell-Senke-Paar und Datenmuster dynamisch an. 

> [!TIP]
> Das Standardverhalten von parallelen Kopien bietet Ihnen normalerweise den besten Durchsatz, der von ADF automatisch festgelegt wird. Grundlage dafür ist das Quell-/Senkenpaar, das Datenmuster sowie die Anzahl der DIUs oder die CPU/der Arbeitsspeicher/die Anzahl der Knoten der selbstgehosteten IR. Weitere Informationen zur Optimierung des parallelen Kopierens finden Sie unter [Problembehandlung bei der Leistung der Kopieraktivität](copy-activity-performance-troubleshooting.md).

In der folgenden Tabelle wird das parallele Kopierverhalten aufgeführt:

| Kopierszenario | Paralleles Kopierverhalten |
| --- | --- |
| Zwischen Dateispeichern | `parallelCopies` bestimmt die Parallelität **auf Dateiebene**. Die Blockerstellung („Chunking“) innerhalb jeder Datei erfolgt darunter automatisch und transparent. Sie wurde so konzipiert, dass die am besten geeignete Blockgröße für einen bestimmten Datenspeichertyp zum parallelen Laden von Daten verwendet wird. <br/><br/>Die tatsächliche Anzahl paralleler Kopien, die von der Kopieraktivität zur Laufzeit verwendet werden, ist nicht mehr als die Anzahl Ihrer verfügbaren Dateien. Bei Verwendung des Kopierverhaltens **mergeFile** in die Dateisenke kann die Kopieraktivität die Parallelität auf Dateiebene nicht nutzen. |
| Aus Dateispeicher in Nicht-Dateispeicher | – Beim Kopieren von Daten in Azure SQL-Datenbank oder Azure Cosmos DB ist die standardmäßige parallele Kopie auch abhängig von der Senkenebene (Anzahl von DTUs/RUs).<br>– Beim Kopieren von Daten in Azure Table ist die standardmäßige parallele Kopie 4. |
| Aus Nicht-Dateispeicher in Dateispeicher | – Beim Kopieren von Daten aus einem Datenspeicher mit aktivierter Partitionsoption (darunter [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [SAP-Tabelle](connector-sap-table.md#sap-table-as-source) und [SAP Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source)) ist die standardmäßige parallele Kopie 4. Die tatsächliche Anzahl paralleler Kopien, die von der Kopieraktivität zur Laufzeit verwendet werden, ist nicht mehr als die Anzahl Ihrer verfügbaren Datenpartitionen. Wenn Sie eine selbstgehostete Integration Runtime verwenden und in Azure-Blob/ADLS Gen2 kopieren, beachten Sie, dass die maximale effektive parallele Kopie 4 oder 5 pro IR-Knoten ist.<br>– Bei anderen Szenarien wird die parallele Kopie nicht wirksam. Selbst wenn Parallelität angegeben wurde, wird sie nicht angewendet. |
| Zwischen Nicht-Dateispeichern | – Beim Kopieren von Daten in Azure SQL-Datenbank oder Azure Cosmos DB ist die standardmäßige parallele Kopie auch abhängig von der Senkenebene (Anzahl von DTUs/RUs).<br/>– Beim Kopieren von Daten aus einem Datenspeicher mit aktivierter Partitionsoption (darunter [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [SAP-Tabelle](connector-sap-table.md#sap-table-as-source) und [SAP Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source)) ist die standardmäßige parallele Kopie 4.<br>– Beim Kopieren von Daten in Azure Table ist die standardmäßige parallele Kopie 4. |

Sie können den Standardwert jedoch überschreiben und einen Wert für die Eigenschaft `parallelCopies` angeben, um die Last für die Computer zu steuern, auf denen Ihre Datenspeicher gehostet werden, oder um die Kopierleistung zu optimieren. Der Wert muss eine ganze Zahl größer als oder gleich 1 sein. Zur Laufzeit verwendet die Kopieraktivität maximal den von Ihnen festgelegten Wert, um eine optimale Leistung zu erzielen.

Berücksichtigen Sie bei der Angabe eines Werts für die Eigenschaft `parallelCopies` die höhere Auslastung Ihrer Quell- und Senkendatenspeicher. Berücksichtigen Sie auch den Lastanstieg zur selbstgehosteten Integration Runtime, wenn die Kopieraktivität davon abhängt. Dieser Lastanstieg tritt insbesondere auf, wenn Sie über mehrere Aktivitäten oder gleichzeitige Ausführungen der gleichen Aktivitäten verfügen, die für den gleichen Datenspeicher ausgeführt werden. Wenn Sie eine Überlastung des Datenspeichers oder der selbstgehosteten Integration Runtime feststellen, verringern Sie den Wert `parallelCopies`, um die Last zu verringern.

**Beispiel:**

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 32
        }
    }
]
```

## <a name="staged-copy"></a>gestaffeltem Kopieren

Beim Kopieren von Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher können Sie ggf. Blob Storage als Stagingzwischenspeicher verwenden. Staging ist besonders in folgenden Fällen hilfreich:

- **Sie möchten Daten aus verschiedenen Datenspeichern über PolyBase in Azure Synapse Analytics (ehemals SQL Data Warehouse) erfassen.** Azure Synapse Analytics nutzt PolyBase als Mechanismus mit hohem Durchsatz, um große Datenmengen in Azure Synapse Analytics zu laden. Die Quelldaten müssen sich in Blob Storage oder in Azure Data Lake Store befinden und einige andere Kriterien erfüllen. Wenn Sie Daten aus einem Blob Storage- oder Azure Data Lake Store-fremden Datenspeicher laden, können Sie das Kopieren von Daten über einen Staging-Blob-Zwischenspeicher aktivieren. In diesem Fall führt Azure Data Factory die erforderlichen Datentransformationen durch, um die Anforderungen von PolyBase zu erfüllen. Anschließend werden die Daten mithilfe von PolyBase auf effiziente Weise in Azure Synapse Analytics geladen. Weitere Informationen finden Sie unter [Use PolyBase to load data into Azure SQL Data Warehouse (Verwenden von PolyBase zum Laden von Daten in Azure SQL Data Warehouse)](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **Hybriddatenverschiebungen (also das Kopieren aus einem lokalen Datenspeicher in einen Clouddatenspeicher) können bei einer langsamen Netzwerkverbindung eine Weile dauern.** Zur Verbesserung der Leistung können Sie gestaffeltes Kopieren verwenden, um die Daten lokal zu komprimieren und dadurch die Verschiebung in den Stagingdatenspeicher in der Cloud zu beschleunigen. Anschließend können Sie die Daten im Stagingspeicher wieder dekomprimieren, bevor Sie sie in den Zieldatenspeicher laden.
- **Aufgrund der IT-Richtlinien des Unternehmens sollten Sie mit Ausnahme der Ports 80 und 443 keine weiteren Ports in der Firewall öffnen.** Ein Beispiel: Beim Kopieren von Daten aus einem lokalen Datenspeicher an eine Azure SQL-Datenbank- oder Azure Synapse Analytics-Senke muss die ausgehende TCP-Kommunikation über den Port 1433 sowohl für die Windows-Firewall als auch für die Unternehmensfirewall ermöglicht werden. In diesem Szenario kann das gestaffelte Kopieren die selbstgehostete Integration Runtime nutzen, um zunächst Daten per HTTP oder HTTPS über den Port 443 an eine Blob Storage-Staginginstanz kopieren. Anschließend können die Daten aus dem Blob Storage-Stagingspeicher in SQL-Datenbank oder in Azure Synapse Analytics geladen werden. Dadurch muss der Port 1433 nicht geöffnet werden.

### <a name="how-staged-copy-works"></a>Funktionsweise des gestaffelten Kopierens

Bei aktivierter Stagingfunktion werden die Daten zunächst aus dem Quelldatenspeicher in den (von Ihnen bereitgestellten) Blob-Stagingspeicher kopiert. Anschließend werden die Daten dann aus dem Stagingdatenspeicher in den Senkendatenspeicher kopiert. Dieser zweistufige Prozess wird automatisch von Azure Data Factory verwaltet. Nach Abschluss der Datenverschiebung bereinigt Azure Data Factory außerdem temporäre Daten im Stagingspeicher.

![gestaffeltem Kopieren](media/copy-activity-performance/staged-copy.png)

Wenn Sie die Datenverschiebung unter Verwendung des Stagingspeichers aktivieren, können Sie angeben, ob die Daten vor dem Verschieben aus dem Quelldatenspeicher in einen Zwischen- oder Stagingdatenspeicher komprimiert und vor dem Verschieben der Daten aus einem Zwischen- oder Stagingdatenspeicher in den Senkendatenspeicher wieder dekomprimiert werden sollen.

Derzeit können Sie keine Daten zwischen zwei Datenspeichern kopieren, die über verschiedene selbstgehostete IRs verbunden sind, weder mit noch ohne gestaffeltes Kopieren. Für ein solches Szenario können Sie zwei explizit verkettete Kopiervorgänge konfigurieren, um von der Quelle zum Stagingspeicher und aus dem Stagingspeicher zur Senke zu kopieren.

### <a name="configuration"></a>Konfiguration

Konfigurieren Sie für die Kopieraktivität die Einstellung **enableStaging**, um anzugeben, ob die Daten vor dem Laden in einen Zielspeicher in Blobspeicher bereitgestellt werden sollen. Wenn Sie **enableStaging** auf `TRUE` festlegen, müssen Sie zusätzliche Eigenschaften angeben. Diese sind in der folgenden Tabelle aufgeführt. Außerdem müssen Sie für das Staging einen verknüpften Azure Storage- oder Storage-SAS-Dienst erstellen, sofern noch keiner vorhanden ist.

| Eigenschaft | BESCHREIBUNG | Standardwert | Erforderlich |
| --- | --- | --- | --- |
| enableStaging |Geben Sie an, ob Sie Daten über einen Stagingzwischenspeicher kopieren möchten. |False |Nein |
| linkedServiceName |Geben Sie den Namen eines verknüpften Diensts vom Typ [AzureStorage](connector-azure-blob-storage.md#linked-service-properties) an, der auf die Storage-Instanz verweist, die Sie als Stagingzwischenspeicher verwenden. <br/><br/> Storage kann nicht mit einer Shared Access Signature (SAS) verwendet werden, um Daten über PolyBase in Azure Synapse Analytics zu laden. In allen anderen Szenarien ist dies hingegen problemlos möglich. |– |Ja, wenn **enableStaging** auf „TRUE“ festgelegt ist. |
| path |Geben Sie den gewünschten Blob Storage-Pfad für die bereitgestellten Daten an. Wenn Sie keinen Pfad angeben, erstellt der Dienst einen Container zum Speichern der temporären Daten. <br/><br/> Geben Sie nur dann einen Pfad an, wenn Sie Storage mit einer Shared Access Signature verwenden oder sich die temporären Daten an einem bestimmten Speicherort befinden müssen. |– |Nein |
| enableCompression |Gibt an, ob die Daten komprimiert werden sollen, bevor sie an das Ziel kopiert werden. Durch diese Einstellung wird die Menge der übertragenen Daten reduziert. |False |Nein |

>[!NOTE]
> Wenn Sie das gestaffelte Kopieren mit aktivierter Komprimierung verwenden, wird die Dienstprinzipal- oder MSI-Authentifizierung für das Staging eines verknüpften Blobdiensts nicht unterstützt.

Hier sehen Sie eine Beispieldefinition der Kopieraktivität mit den Eigenschaften aus der obigen Tabelle:

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                },
                "path": "stagingcontainer/path",
                "enableCompression": true
            }
        }
    }
]
```

### <a name="staged-copy-billing-impact"></a>Auswirkungen auf die Abrechnung von gestaffeltem Kopieren

Die Abrechnung basiert auf zwei Aspekten: Dauer des Kopiervorgangs und Art der Kopie.

* Wenn Sie einen Stagingspeicher während eines Cloudkopiervorgangs (d. h. Kopieren von Daten aus einem Clouddatenspeicher in einen anderen Clouddatenspeicher. Beide Phasen werden von Azure-Integration Runtime unterstützt) verwenden, gilt folgende Formel: [Gesamtkopierdauer für Schritt 1 und 2] [Einzelpreis für Cloudkopien].
* Wenn Sie Staging während eines Hybridkopiervorgangs (d. h. Kopieren von Daten aus einem lokalen Datenspeicher in einen Clouddatenspeicher. Eine Phase wird von der selbstgehosteten Integration Runtime-Infrastruktur unterstützt) verwenden, gilt folgende Formel: [Dauer des Hybridkopiervorgangs] [Einzelpreis für Hybridkopien] + [Dauer des Cloudkopiervorgangs] [Einzelpreis für Cloudkopien].

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den anderen Artikeln zur Kopieraktivität:

- [Kopieraktivität – Übersicht](copy-activity-overview.md)
- [Handbuch zur Leistung und Skalierbarkeit der Kopieraktivität](copy-activity-performance.md)
- [Troubleshoot copy activity performance](copy-activity-performance-troubleshooting.md) (Problembehandlung bei der Leistung der Kopieraktivität)
- [Verwenden von Azure Data Factory zum Migrieren von Daten aus einem Data Lake oder Data Warehouse zu Azure](data-migration-guidance-overview.md)
- [Migrieren von Daten aus Amazon S3 zu Azure Storage](data-migration-guidance-s3-azure-storage.md)