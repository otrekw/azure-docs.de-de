---
title: Quelltransformation in einem Zuordnungsdatenfluss
description: Erfahren Sie, wie Sie eine Quelltransformation in Mapping Data Flow einrichten.
author: kromerm
ms.author: makromer
manager: anandsub
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 07/16/2020
ms.openlocfilehash: adbe3ceb72d88c2a6441c04d876d92ac2ab85930
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86522770"
---
# <a name="source-transformation-in-mapping-data-flow"></a>Quelltransformation in einem Zuordnungsdatenfluss 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Mit einer Quelltransformation wird die Datenquelle für den Datenfluss konfiguriert. Beim Entwerfen von Datenflüssen ist der erste Schritt immer das Konfigurieren einer Quelltransformation. Um eine Quelle hinzuzufügen, klicken Sie im Datenfluss-Zeichenbereich auf das Feld **Quelle hinzufügen**.

Jeder Datenfluss erfordert mindestens eine Quelltransformation, aber Sie können so viele Quellen wie erforderlich hinzufügen, um Ihre Datentransformationen abzuschließen. Sie können diese Quellen zusammen mit einer Join-, Lookup- oder Union-Transformation verwenden.

Jede Quelltransformation ist genau einem Dataset oder einem verknüpften Dienst zugeordnet. Das Dataset definiert die Form und Position der Daten, in die geschrieben oder aus denen gelesen werden soll. Wenn Sie ein dateibasiertes Dataset verwenden, können Sie Platzhalter und Dateilisten in Ihrer Quelle verwenden, um mit mehreren Dateien gleichzeitig zu arbeiten.

## <a name="inline-datasets"></a>Inline-Datasets

Die erste Entscheidung, die Sie beim Erstellen einer Quelltransformation treffen, ist die, ob Ihre Quellinformationen innerhalb eines Datasetobjekts oder innerhalb der Quelltransformation definiert sind. Die meisten Formate sind nur in dem einen oder dem anderen Format verfügbar. Verweisen Sie auf das entsprechende Connectordokument, um zu erfahren, wie ein bestimmter Connector verwendet wird.

Wenn ein Format sowohl für Inline- als auch in einem Datasetobjekt unterstützt wird, ergeben sich Vorteile für beide. Datasetobjekte sind wiederverwendbare Entitäten, die in anderen Datenflüssen und Aktivitäten wie Kopiervorgängen genutzt werden können. Diese sind besonders nützlich, wenn ein festgeschriebenes Schema verwendet wird. Die Datasets basieren nicht auf Spark, und gelegentlich kann es erforderlich sein, bestimmte Einstellungen oder Schemaprojektionen in der Quelltransformation außer Kraft zu setzen.

Inline-Datasets werden empfohlen, wenn flexible Schemata, einmalige Quellinstanzen oder parametrisierte Quellen verwendet werden. Wenn Ihre Quelle stark parametrisiert ist, können Sie mit Inline-Datasets kein „Dummy“-Objekt erstellen. Inline-Datasets basieren auf Spark und ihre Eigenschaften sind nativ für den Datenfluss.

Um ein Inline-Dataset zu verwenden, wählen Sie das gewünschte Format im Selektor **Quelltyp** aus. Anstatt ein Quelldataset auszuwählen, wählen Sie den verknüpften Dienst aus, mit dem Sie eine Verbindung herstellen möchten.

![Inline-Dataset](media/data-flow/inline-selector.png "Inline-Dataset")

##  <a name="supported-source-types"></a><a name="supported-sources"></a> Unterstützte Quelltypen

Die Zuordnung des Datenflusses folgt einem Ansatz zum Extrahieren, Laden und Transformieren (ELT) und funktioniert mit *Stagingdatasets* in Azure. Derzeit können die folgenden Datasets in einer Quelltransformation verwendet werden:

| Connector | Format | Dataset/Inline |
| --------- | ------ | -------------- |
| [Azure Blob Storage](connector-azure-blob-storage.md#mapping-data-flow-properties) | [Avro](format-avro.md#mapping-data-flow-properties)<br>[Text mit Trennzeichen](format-delimited-text.md#mapping-data-flow-properties)<br>[Delta (Vorschau)](format-delta.md)<br>[Excel](format-excel.md#mapping-data-flow-properties)<br>[JSON](format-json.md#mapping-data-flow-properties) <br>[Parquet](format-parquet.md#mapping-data-flow-properties)<br>[XML](format-xml.md#mapping-data-flow-properties) | ✓/-<br>✓/-<br>-/✓<br>✓/✓<br/>✓/-<br>✓/-<br>✓/✓ |
| [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties) | [Avro](format-avro.md#mapping-data-flow-properties)<br>[Text mit Trennzeichen](format-delimited-text.md#mapping-data-flow-properties)<br>[Excel](format-excel.md#mapping-data-flow-properties)<br>[JSON](format-json.md#mapping-data-flow-properties)<br>[Parquet](format-parquet.md#mapping-data-flow-properties)<br>[XML](format-xml.md#mapping-data-flow-properties)  | ✓/-<br>✓/-<br>✓/✓<br/>✓/-<br>✓/-<br>✓/✓ |
| [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md#mapping-data-flow-properties) | [Avro](format-avro.md#mapping-data-flow-properties)<br>[Common Data Model (Vorschau)](format-common-data-model.md#source-properties)<br>[Text mit Trennzeichen](format-delimited-text.md#mapping-data-flow-properties)<br>[Delta (Vorschau)](format-delta.md)<br>[Excel](format-excel.md#mapping-data-flow-properties)<br>[JSON](format-json.md#mapping-data-flow-properties)<br>[Parquet](format-parquet.md#mapping-data-flow-properties)<br>[XML](format-xml.md#mapping-data-flow-properties) | ✓/-<br/>-/✓<br>✓/-<br>-/✓<br>✓/✓<br>✓/-<br/>✓/-<br>✓/✓ |
| [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#mapping-data-flow-properties) | | ✓/- |
| [Azure SQL-Datenbank](connector-azure-sql-database.md#mapping-data-flow-properties) | | ✓/- |
| [Azure Cosmos DB (SQL-API)](connector-azure-cosmos-db.md#mapping-data-flow-properties) | | ✓/- |

Die für diese Connectors spezifischen Einstellungen befinden sich auf der Registerkarte **Quelloptionen**. Informationen und Beispiele zu Datenflussskripts zu diesen Einstellungen finden Sie in der Connectordokumentation. 

Azure Data Factory hat Zugriff auf über [90 native Connectors](connector-overview.md). Um Daten aus diesen anderen Quellen in Ihren Datenfluss einzubeziehen, verwenden Sie die Kopieraktivität, um die Daten in einen der unterstützten Stagingbereiche zu laden.

## <a name="source-settings"></a>Quelleinstellungen

Nachdem Sie eine Quelle hinzugefügt haben, konfigurieren Sie sie über die Registerkarte **Quelleinstellungen**. Hier können Sie das Dataset auswählen oder erstellen, auf das Ihre Quelle verweist. Sie können auch das Schema und die Samplingoptionen für Ihre Daten auswählen.

![Registerkarte „Quelleinstellungen“](media/data-flow/source1.png "Registerkarte „Quelleinstellungen“")

**Name des Ausgabedatenstroms:** Der Name der Quelltransformation.

**Quelltyp:** Wählen Sie aus, ob Sie ein Inline-Dataset oder ein bestehendes Datasetobjekt verwenden möchten.

**Verbindung testen:** Testen Sie, ob der Spark-Dienst des Datenflusses erfolgreich eine Verbindung mit dem verknüpften Dienst herstellen kann, der in Ihrem Quelldataset verwendet wird. Damit diese Funktion aktiviert werden kann, muss der Debugmodus aktiviert sein.

**Schemaabweichung**: [Schemaabweichung](concepts-data-flow-schema-drift.md) ist die Fähigkeit von Data Factory, flexible Schemas in Ihren Datenflüssen nativ zu verarbeiten, ohne explizit Spaltenänderungen definieren zu müssen.

* Aktivieren Sie das Kontrollkästchen **Schemaabweichung zulassen**, wenn die Quellspalten häufig geändert werden. Diese Einstellung ermöglicht es, dass alle eingehenden Quellfelder durch die Transformationen zur Senke fließen.

* Mit der Option **Abweichende Spaltentypen ableiten** wird Data Factory angewiesen, Datentypen für jede neue erkannte Spalte zu ermitteln und zu definieren. Wenn dieses Feature deaktiviert ist, weisen alle abweichende Spalten den Typ „String“ auf.

**Schema überprüfen**: Ist „Schema überprüfen“ aktiviert, führt das Ausführen des Datenflusses zu einem Fehler, wenn die eingehenden Daten nicht mit dem definierten Schema des Datasets übereinstimmen.

**Anzahl zu überspringender Zeilen**: Das Feld „Anzahl zu überspringender Zeilen“ gibt an, wie viele Zeilen am Anfang des Datasets ignoriert werden sollen.

**Stichprobenentnahme**: Aktivieren Sie „Stichprobenentnahme“, um die Anzahl der Zeilen aus der Quelle zu beschränken. Verwenden Sie diese Einstellung, wenn Sie für das Debuggen Stichproben der Daten an der Quelle erstellen möchten.

Um zu überprüfen, ob die Quelle ordnungsgemäß konfiguriert ist, aktivieren Sie den Debugmodus, und rufen Sie eine Datenvorschau ab. Weitere Informationen finden Sie unter [Debugmodus](concepts-data-flow-debug-mode.md).

> [!NOTE]
> Wenn der Debugmodus aktiviert ist, überschreibt die Zeilenlimit-Konfiguration in den Debugeinstellungen bei der Datenvorschau die Stichprobenentnahme-Einstellung in der Quelle.

## <a name="source-options"></a>Quelloptionen

Die Registerkarte „Quelloptionen“ enthält Einstellungen, die spezifisch für den gewählten Connector und das gewählte Format sind. Weitere Informationen und Beispiele finden Sie in der entsprechenden [Connectordokumentation](#supported-sources).

## <a name="projection"></a>Projektion

Wie Schemas in Datasets definiert die Projektion in einer Quelle die Datenspalten, Datentypen und Datenformate aus den Quelldaten. Bei den meisten Datasettypen, z.B. SQL und Parquet, wird die Projektion in einer Quelle so festgelegt, dass sie das in einem Dataset definierte Schema widerspiegelt. Wenn Ihre Quelldateien nicht stark typisiert sind (z.B. flache CSV-Dateien im Gegensatz zu Parquet-Dateien), können Sie in der Quelltransformation die Datentypen für jedes Feld definieren.

![Einstellungen auf der Registerkarte „Projektion“](media/data-flow/source3.png "Projektion")

Wenn in Ihrer Textdatei kein Schema definiert ist, wählen Sie **Datentyp erkennen** aus, damit Data Factory Stichproben erstellt und die Datentypen ableitet. Wählen Sie **Standarddatenformat** aus, um die Standarddatenformate automatisch zu ermitteln.

**Schema zurücksetzen** setzt die Projektion auf die Vorgaben im referenzierten Dataset zurück.

Sie können die Spaltendatentypen in einer späteren Transformation für nachgeschaltete Spalten ändern. Verwenden Sie eine Transformation, um die Spaltennamen zu ändern.

### <a name="import-schema"></a>Schema importieren

Mithilfe der Schaltfläche **Schema importieren** auf der Registerkarte **Projektion** können Sie einen aktiven Debugcluster zum Erstellen einer Schemaprojektion verwenden. Diese Funktion ist für jeden Quelltyp verfügbar. Durch das Importieren des Schemas wird die im Dataset definierte Projektion überschrieben. Das Datasetobjekt wird nicht geändert.

Dies ist insbesondere bei Datasets wie Avro und Azure Cosmos DB hilfreich, die komplexe Datenstrukturen unterstützen und keine Schemadefinitionen im Dataset erfordern. Für Inline-Datasets ist dies die einzige Möglichkeit, ohne Schemaabweichung auf Spaltenmetadaten zu verweisen.

## <a name="optimize-the-source-transformation"></a>Optimieren der Quelltransformation

Die Registerkarte **Optimieren** ermöglicht die Bearbeitung von Partitionsinformationen bei jedem Transformationsschritt. In den meisten Fällen führt **Aktuelle Partitionierung verwenden** eine Optimierung durch, um die ideale Partitionsstruktur für eine Quelle zu erreichen.

Wenn Sie aus einer Azure SQL-Datenbankquelle lesen, liest die benutzerdefinierte **Quellpartition** die Daten wahrscheinlich am schnellsten. ADF liest große Abfragen, indem es parallel Verbindungen mit Ihrer Datenbank herstellt. Diese Quellpartition kann für eine Spalte oder über eine Abfrage erfolgen.

![Einstellungen der Quellpartition](media/data-flow/sourcepart3.png "Partitionierung")

Weitere Informationen zur Optimierung in Mapping Data Flow finden Sie auf der [Registerkarte „Optimieren“](concepts-data-flow-overview.md#optimize).

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie die Erstellung Ihres Datenflusses mit einer [Transformation für abgeleitete Spalten](data-flow-derived-column.md) und einer [select-Transformation](data-flow-select.md).
