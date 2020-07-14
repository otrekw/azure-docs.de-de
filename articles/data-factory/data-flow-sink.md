---
title: Senkentransformation in einem Zuordnungsdatenfluss
description: Hier erfahren Sie, wie Sie im Zuordnungsdatenfluss eine Senkentransformation konfigurieren.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
manager: anandsub
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/03/2020
ms.openlocfilehash: 143c94527b947495709d2e94f107dc578e7f2866
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84610178"
---
# <a name="sink-transformation-in-mapping-data-flow"></a>Senkentransformation in einem Zuordnungsdatenfluss

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Nachdem Sie die Datentransformation abgeschlossen haben, schreiben Sie sie mithilfe der Senkentransformation in einen Zielspeicher. Jeder Datenfluss erfordert mindestens eine Senkentransformation. Sie können aber so viele Senken wie erforderlich hinzufügen, um Ihren Transformationsfluss zu realisieren. Um in weitere Senken zu schreiben, erstellen Sie über neue Verzweigungen und bedingte Teilungen neue Datenströme.

Jede Senkentransformation ist genau einem Azure Data Factory-Datasetobjekt oder einem verknüpften Dienst zugeordnet. Die Form und Position der Daten, in die geschrieben werden soll, richtet sich nach der Senkentransformation.

## <a name="inline-datasets"></a>Inlinedatasets

Beim Erstellen einer Senkentransformation legen Sie fest, ob Ihre Senkeninformationen innerhalb eines Datasetobjekts oder innerhalb der Senkentransformation definiert sind. Die meisten Formate sind nur in einem von beiden verfügbar. Weitere Informationen zur Verwendung eines bestimmten Connectors finden Sie im Dokument zu diesem Connector.

Wenn ein Format sowohl für Inline- als auch in einem Datasetobjekt unterstützt wird, hat beides seine Vorteile. Datasetobjekte sind wiederverwendbare Entitäten, die in anderen Datenflüssen und Aktivitäten wie Kopiervorgängen genutzt werden können. Diese sind besonders nützlich, wenn ein festgeschriebenes Schema verwendet wird. Die Datasets basieren nicht auf Spark, und gelegentlich kann es erforderlich sein, bestimmte Einstellungen oder Schemaprojektionen in der Senkentransformation außer Kraft zu setzen.

Inlinedatasets werden empfohlen, wenn flexible Schemas, einmalig genutzte Senkeninstanzen oder parametrisierte Senken verwendet werden. Wenn Ihre Senke stark parametrisiert ist, können Sie mit Inlinedatasets kein Dummyobjekt erstellen. Inlinedatasets basieren auf Spark und ihre Eigenschaften sind für den Datenfluss nativ.

Um ein Inlinedataset zu verwenden, wählen Sie das gewünschte Format im Selektor **Senkentyp** aus. Anstatt ein Senkendataset auszuwählen, wählen Sie den verknüpften Dienst aus, mit dem Sie eine Verbindung herstellen möchten.

![Inlinedataset](media/data-flow/inline-selector.png "Inlinedataset")

##  <a name="supported-sink-types"></a><a name="supported-sinks"></a> Unterstützte Senkentypen

Die Zuordnung des Datenflusses folgt einem Ansatz zum Extrahieren, Laden und Transformieren (ELT) und funktioniert mit *Stagingdatasets* in Azure. Derzeit können die folgenden Datasets in einer Quelltransformation verwendet werden:

| Connector | Format | Dataset/Inline |
| --------- | ------ | -------------- |
| [Azure Blob Storage](connector-azure-blob-storage.md#mapping-data-flow-properties) | [JSON](format-json.md#mapping-data-flow-properties) <br> [Avro](format-avro.md#mapping-data-flow-properties) <br> [Text mit Trennzeichen](format-delimited-text.md#mapping-data-flow-properties) <br> [Parquet](format-parquet.md#mapping-data-flow-properties) | ✓/- <br> ✓/- <br> ✓/- <br> ✓/- |
| [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties) | [JSON](format-json.md#mapping-data-flow-properties) <br> [Avro](format-avro.md#mapping-data-flow-properties) <br> [Text mit Trennzeichen](format-delimited-text.md#mapping-data-flow-properties) <br> [Parquet](format-parquet.md#mapping-data-flow-properties)  | ✓/- <br> ✓/- <br> ✓/- <br> ✓/- |
| [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md#mapping-data-flow-properties) | [JSON](format-json.md#mapping-data-flow-properties) <br> [Avro](format-avro.md#mapping-data-flow-properties) <br> [Text mit Trennzeichen](format-delimited-text.md#mapping-data-flow-properties) <br> [Parquet](format-parquet.md#mapping-data-flow-properties)  <br> [Common Data Model (Vorschau)](format-common-data-model.md#sink-properties) | ✓/- <br> ✓/- <br> ✓/- <br> ✓/- <br> -/✓ |
| [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#mapping-data-flow-properties) | | ✓/- |
| [Azure SQL-Datenbank](connector-azure-sql-database.md#mapping-data-flow-properties) | | ✓/- |
| [Azure CosmosDB (SQL-API)](connector-azure-cosmos-db.md#mapping-data-flow-properties) | | ✓/- |

Die für diese Connectors spezifischen Einstellungen befinden sich auf der Registerkarte **Einstellungen**. Informationen und Beispiele zu Datenflussskripts zu diesen Einstellungen finden Sie in der Connectordokumentation. 

Azure Data Factory hat Zugriff auf über [90 native Connectors](connector-overview.md). Um Daten aus Ihrem Datenfluss in diese anderen Quellen zu schreiben, verwenden Sie die Kopieraktivität zum Laden der Daten aus einer unterstützten Senke.

## <a name="sink-settings"></a>Senkeneinstellungen

Nachdem Sie eine Senke hinzugefügt haben, konfigurieren Sie die Senke über die Registerkarte **Senke**. Hier können Sie das Dataset auswählen oder erstellen, in das die Senke schreibt. Unten finden Sie ein Video, in dem verschiedene Senkenoptionen für Dateitypen mit Texttrennzeichen beschrieben werden:

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4tf7T]

![Senkeneinstellungen](media/data-flow/sink-settings.png "Senkeneinstellungen")

**Schemaabweichung**: [Schemaabweichung](concepts-data-flow-schema-drift.md) ist die Fähigkeit von Data Factory, flexible Schemas in Ihren Datenflüssen nativ zu verarbeiten, ohne explizit Spaltenänderungen definieren zu müssen. Aktivieren Sie **Schemaabweichung zulassen**, wenn Sie zusätzliche Spalten zusätzlich zur Definition im Datenschema der Senke schreiben möchten.

**Schema überprüfen**: Wenn „Schema überprüfen“ ausgewählt ist, führt der Datenfluss zu einem Fehler, wenn eine der Spalten des Eingangsquellschemas in der Quellprojektion nicht gefunden wird oder wenn die Datentypen nicht übereinstimmen. Verwenden Sie diese Einstellung, um eine Übereinstimmung der Quelldaten mit dem Vertrag Ihrer definierten Projektion zu erzwingen. Dies ist in Szenarien mit Datenbankquellen nützlich, um anzugeben, dass die Spaltennamen oder -typen geändert wurden.

## <a name="field-mapping"></a>Feldzuordnung

Ähnlich wie bei einer Auswahltansformation können Sie auf der Registerkarte **Zuordnung** der Senke festlegen, welche eingehenden Spalten geschrieben werden. Standardmäßig werden alle Eingabespalten – auch abweichende Spalten – zugeordnet. Dies wird als **automatische Zuordnung** bezeichnet.

Wenn Sie die automatische Zuordnung deaktivieren, können Sie eine feste spaltenbasierte Zuordnung oder eine regelbasierte Zuordnung hinzuzufügen. Die regelbasierte Zuordnung ermöglicht das Schreiben von Ausdrücken mit Musterabgleich, während bei der festen Zuordnung logische und physische Spaltennamen zugeordnet werden. Weitere Informationen zur regelbasierten Zuordnung finden Sie unter [Spaltenmuster im Zuordnungsdatenfluss](concepts-data-flow-column-pattern.md#rule-based-mapping-in-select-and-sink).

## <a name="custom-sink-ordering"></a>Benutzerdefinierte Senkenreihenfolge

Standardmäßig werden Daten ohne festgeschriebene Reihenfolge in mehrere Senken geschrieben. Die Ausführungs-Engine schreibt im Verlauf der Transformationslogik Daten parallel. Dabei kann die Senkenreihenfolge bei jeder Ausführung variieren. Um die genaue Reihenfolge der Senken anzugeben, aktivieren Sie auf der Registerkarte „Allgemein“ des Datenflusses die Option **Benutzerdefinierte Senkenreihenfolge**. Wenn diese Option aktiviert ist, erfolgt das Schreiben sequenziell in aufsteigender Reihenfolge in den Senken.

![Benutzerdefinierte Senkenreihenfolge](media/data-flow/custom-sink-ordering.png "Benutzerdefinierte Senkenreihenfolge")

## <a name="data-preview-in-sink"></a>Datenvorschau in Senken

Beim Abrufen einer Datenvorschau in einem Debugcluster werden keine Daten in die Senke geschrieben. Es wird eine Momentaufnahme der Daten zurückgegeben, aber nichts in das Ziel geschrieben. Um das Schreiben von Daten in die Senke zu überprüfen, führen Sie einen Pipelinedebugvorgang in der Pipelinecanvas aus.

## <a name="next-steps"></a>Nächste Schritte
Da Sie nun Ihren Datenfluss erstellt haben, fügen Sie [Ihrer Pipeline eine Aktivität zur Ausführung eines Datenflusses](concepts-data-flow-overview.md) hinzu.
