---
title: Quelltransformation in einem Zuordnungsdatenfluss
description: Erfahren Sie, wie Sie eine Quelltransformation in Mapping Data Flow einrichten.
author: kromerm
ms.author: makromer
manager: anandsub
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.openlocfilehash: b2f533e8bd9199025260aaca9cff587b13adce64
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606306"
---
# <a name="source-transformation-in-mapping-data-flow"></a>Quelltransformation in einem Zuordnungsdatenfluss 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Mit einer Quelltransformation wird die Datenquelle für den Datenfluss konfiguriert. Beim Entwerfen von Datenflüssen ist der erste Schritt immer das Konfigurieren einer Quelltransformation. Um eine Quelle hinzuzufügen, klicken Sie im Datenfluss-Zeichenbereich auf das Feld **Quelle hinzufügen**.

Jeder Datenfluss erfordert mindestens eine Quelltransformation, aber Sie können so viele Quellen wie erforderlich hinzufügen, um Ihre Datentransformationen abzuschließen. Sie können diese Quellen zusammen mit einer Join-, Lookup- oder Union-Transformation verwenden.

Jede Quelltransformation ist genau einem Data Factory-Dataset zugeordnet. Das Dataset definiert die Form und Position der Daten, in die geschrieben oder aus denen gelesen werden soll. Wenn Sie ein dateibasiertes Dataset verwenden, können Sie Platzhalter und Dateilisten in Ihrer Quelle verwenden, um mit mehreren Dateien gleichzeitig zu arbeiten.

## <a name="supported-source-connectors-in-mapping-data-flow"></a>Unterstützte Quellconnectors im Zuordnungsdatenfluss

Die Zuordnung des Datenflusses folgt einem Ansatz zum Extrahieren, Laden und Transformieren (ELT) und funktioniert mit *Stagingdatasets* in Azure. Derzeit können die folgenden Datasets in einer Quelltransformation verwendet werden:
    
* [Azure Blob Storage](connector-azure-blob-storage.md#mapping-data-flow-properties) (JSON, Avro, Text, Parquet)
* [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties) (JSON, Avro, Text, Parquet)
* [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md#mapping-data-flow-properties) (JSON, Avro, Text, Parquet)
* [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#mapping-data-flow-properties)
* [Azure SQL-Datenbank](connector-azure-sql-database.md#mapping-data-flow-properties)
* [Azure CosmosDB](connector-azure-cosmos-db.md#mapping-data-flow-properties)

Die für diese Connectors spezifischen Einstellungen befinden sich auf der Registerkarte **Quelloptionen**. Informationen zu diesen Einstellungen finden Sie in der Connector-Dokumentation. 

Azure Data Factory hat Zugriff auf über [90 native Connectors](connector-overview.md). Um Daten aus diesen anderen Quellen in Ihren Datenfluss einzubeziehen, verwenden Sie die Kopieraktivität, um die Daten in einen der unterstützten Stagingbereiche zu laden.

## <a name="source-settings"></a>Quelleinstellungen

Nachdem Sie eine Quelle hinzugefügt haben, konfigurieren Sie sie über die Registerkarte **Quelleinstellungen**. Hier können Sie das Dataset auswählen oder erstellen, auf das Ihre Quelle verweist. Sie können auch das Schema und die Samplingoptionen für Ihre Daten auswählen.

![Registerkarte „Quelleinstellungen“](media/data-flow/source1.png "Registerkarte „Quelleinstellungen“")

**Verbindung testen:** Testen Sie, ob der Spark-Dienst des Datenflusses erfolgreich eine Verbindung mit dem verknüpften Dienst herstellen kann, der in Ihrem Quelldataset verwendet wird. Damit diese Funktion aktiviert werden kann, muss der Debugmodus aktiviert sein.

**Schemaabweichung**: [Schemaabweichung](concepts-data-flow-schema-drift.md) ist die Fähigkeit von Data Factory, flexible Schemas in Ihren Datenflüssen nativ zu verarbeiten, ohne explizit Spaltenänderungen definieren zu müssen.

* Aktivieren Sie das Kontrollkästchen **Schemaabweichung zulassen**, wenn die Quellspalten häufig geändert werden. Diese Einstellung ermöglicht es, dass alle eingehenden Quellfelder durch die Transformationen zur Senke fließen.

* Mit der Option **Abweichende Spaltentypen ableiten** wird Data Factory angewiesen, Datentypen für jede neue erkannte Spalte zu ermitteln und zu definieren. Wenn dieses Feature deaktiviert ist, weisen alle abweichende Spalten den Typ „String“ auf.

**Schema überprüfen**: Ist „Schema überprüfen“ aktiviert, führt das Ausführen des Datenflusses zu einem Fehler, wenn die eingehenden Daten nicht mit dem definierten Schema des Datasets übereinstimmen.

**Anzahl zu überspringender Zeilen**: Das Feld „Anzahl zu überspringender Zeilen“ gibt an, wie viele Zeilen am Anfang des Datasets ignoriert werden sollen.

**Stichprobenentnahme**: Aktivieren Sie „Stichprobenentnahme“, um die Anzahl der Zeilen aus der Quelle zu beschränken. Verwenden Sie diese Einstellung, wenn Sie für das Debuggen Stichproben der Daten an der Quelle erstellen möchten.

**Mehrzeilig:** Wählen Sie „Mehrzeilig“ aus, wenn die Quelltextdatei Zeichenfolgenwerte enthält, die mehrere Zeilen umfassen, d.h. Zeilenumbrüche innerhalb eines Werts. Diese Einstellung ist nur in DelimitedText-Datasets verfügbar.

Um zu überprüfen, ob die Quelle ordnungsgemäß konfiguriert ist, aktivieren Sie den Debugmodus, und rufen Sie eine Datenvorschau ab. Weitere Informationen finden Sie unter [Debugmodus](concepts-data-flow-debug-mode.md).

> [!NOTE]
> Wenn der Debugmodus aktiviert ist, überschreibt die Zeilenlimit-Konfiguration in den Debugeinstellungen bei der Datenvorschau die Stichprobenentnahme-Einstellung in der Quelle.

## <a name="projection"></a>Projektion

Wie Schemas in Datasets definiert die Projektion in einer Quelle die Datenspalten, Datentypen und Datenformate aus den Quelldaten. Bei den meisten Datasettypen, z.B. SQL und Parquet, wird die Projektion in einer Quelle so festgelegt, dass sie das in einem Dataset definierte Schema widerspiegelt. Wenn Ihre Quelldateien nicht stark typisiert sind (z.B. flache CSV-Dateien im Gegensatz zu Parquet-Dateien), können Sie in der Quelltransformation die Datentypen für jedes Feld definieren.

![Einstellungen auf der Registerkarte „Projektion“](media/data-flow/source3.png "Projektion")

Wenn in Ihrer Textdatei kein Schema definiert ist, wählen Sie **Datentyp erkennen** aus, damit Data Factory Stichproben erstellt und die Datentypen ableitet. Wählen Sie **Standarddatenformat** aus, um die Standarddatenformate automatisch zu ermitteln.

**Schema zurücksetzen** setzt die Projektion auf die Vorgaben im referenzierten Dataset zurück.

Sie können die Spaltendatentypen in einer späteren Transformation für nachgeschaltete Spalten ändern. Verwenden Sie eine Transformation, um die Spaltennamen zu ändern.

### <a name="import-schema"></a>Schema importieren

Mithilfe der Schaltfläche **Schema importieren** auf der Registerkarte **Projektion** können Sie einen aktiven Debugcluster zum Erstellen einer Schemaprojektion verwenden. Diese Funktion ist für jeden Quelltyp verfügbar. Durch das Importieren des Schemas wird die im Dataset definierte Projektion überschrieben. Das Datasetobjekt wird nicht geändert.

Dies ist insbesondere bei Datasets wie Avro und Cosmos DB hilfreich, die komplexe Datenstrukturen unterstützen und keine Schemadefinitionen im Dataset erfordern.

## <a name="optimize-the-source-transformation"></a>Optimieren der Quelltransformation

Auf der Registerkarte **Optimieren** für die Quelltransformation wird möglicherweise der Partitionstyp **Quelle** angezeigt. Diese Option ist nur verfügbar, wenn die Quelle eine Azure SQL-Datenbank ist. Dies liegt daran, dass Data Factory versucht, parallele Verbindungen herzustellen, um umfangreiche Abfragen für Ihre SQL-Datenbankquelle auszuführen.

![Einstellungen der Quellpartition](media/data-flow/sourcepart3.png "Partitionierung")

Sie verfügen nicht über Partitionsdaten in Ihrer SQL-Datenbank-Quelle, Partitionen sind aber besonders hilfreich für große Abfragen. Sie können Ihre Partitionierung auf eine Spalte oder einer Abfrage basieren.

### <a name="use-a-column-to-partition-data"></a>Verwenden einer Spalte zum Partitionieren von Daten

Wählen Sie in Ihrer Quelltabelle eine Spalte aus, nach der partitioniert werden soll. Legen Sie auch die Anzahl von Partitionen fest.

### <a name="use-a-query-to-partition-data"></a>Verwenden einer Abfrage zum Partitionieren von Daten

Sie können die Verbindungen basierend auf einer Abfrage partitionieren. Geben Sie den Inhalt eines WHERE-Prädikats ein. Geben Sie beispielsweise „year > 1980“ ein.

Weitere Informationen zur Optimierung in Mapping Data Flow finden Sie auf der [Registerkarte „Optimieren“](concepts-data-flow-overview.md#optimize).

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit dem Erstellen einer [Transformation für abgeleitete Spalten](data-flow-derived-column.md) und einer [select-Transformation](data-flow-select.md).
