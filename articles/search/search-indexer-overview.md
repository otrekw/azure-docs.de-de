---
title: Indexer für das Crawlen von Daten während des Imports
titleSuffix: Azure Cognitive Search
description: Hier erfahren Sie, wie Sie Azure SQL-Datenbank, SQL Managed Instance, Azure Cosmos DB oder Azure-Speicher per Crawler durchlaufen, um durchsuchbare Daten zu extrahieren und einen Azure Cognitive Search-Index aufzufüllen.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/11/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: 5861e79054bed0d9d75258dfa9cb39b198f0f93d
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98216443"
---
# <a name="indexers-in-azure-cognitive-search"></a>Indexer in Azure Cognitive Search

Ein *Indexer* in Azure Cognitive Search ist ein Crawler, der durchsuchbare Daten und Metadaten aus einer externen Azure-Datenquelle extrahiert und einen Suchindex mithilfe von Feld-zu-Feld-Zuordnungen zwischen Quelldaten und Ihrem Index auffüllt. Dieser Ansatz wird auch als „Pullmodell“ bezeichnet, weil der Dienst Daten abruft, ohne dass Sie Code schreiben müssen, der einem Index Daten hinzufügt.

Indexer gibt es nur in Azure, mit einzelnen Indexern für [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Azure Cosmos DB](search-howto-index-cosmosdb.md), [Azure Table Storage](search-howto-indexing-azure-tables.md) und [Blob Storage](search-howto-indexing-azure-blob-storage.md). Beim Konfigurieren eines Indexers geben Sie eine Datenquelle (Ursprung) sowie einen Index (Ziel) an. Bei mehreren Quellen, z. B. Blob Storage, gibt es zusätzliche Konfigurationseigenschaften, die für diesen Inhaltstyp spezifisch sind.

Sie können Indexer bei Bedarf oder nach einem Zeitplan für die regelmäßige Datenaktualisierung ausführen (z. B. alle fünf Minuten). Für häufigere Aktualisierungen ist ein Pushmodell erforderlich, bei dem Daten in Azure Cognitive Search und Ihrer externen Datenquelle gleichzeitig aktualisiert werden.

## <a name="usage-scenarios"></a>Verwendungsszenarios

Sie können entweder einen Indexer als einziges Mittel für die Datenerfassung oder eine Kombination aus Techniken verwenden, die das Laden von nur einigen der Felder in Ihren Index einbeziehen. Optional können Sie Inhalte dabei transformieren oder anreichern. In der folgenden Tabelle werden die Hauptszenarios zusammengefasst.

| Szenario |Strategie |
|----------|---------|
| Einzelne Quelle | Dieses Muster ist das einfachste: eine einzige Datenquelle ist der einzige Inhaltsanbieter für einen Suchindex. In der Quelle identifizieren Sie ein Feld mit eindeutigen Werten, die im Suchindex als Dokumentschlüssel dienen sollen. Der eindeutige Wert wird als Bezeichner verwendet. Alle anderen Quellfelder werden entsprechenden Feldern in einem Index implizit oder explizit zugeordnet. </br></br>Ein Punkt dabei ist, dass der Wert eines Dokumentschlüssels aus den Quelldaten stammt. Ein Suchdienst generiert keine Schlüsselwerte. Bei nachfolgenden Ausführungen werden eingehende Dokumente mit neuen Schlüsseln hinzugefügt, während eingehende Dokumente mit vorhandenen Schlüsseln entweder zusammengeführt oder überschrieben werden. Dies ist davon abhängig, ob Indexfelder NULL sind oder ausgefüllt werden. |
| Mehrere Quellen| Ein Index kann Inhalte aus mehreren Quellen akzeptieren, wobei jede Ausführung neuen Inhalt aus einer anderen Quelle bringt. </br></br>Ein Ergebnis könnte ein Index sein, der Dokumente nach jeder Indexerausführung erstellt, wobei ganze Dokumente vollständig aus jeder Quelle erstellt werden. So stammen beispielsweise die Dokumente 1–100 aus Blob Storage, die Dokumente 101–200 stammen aus Azure SQL usw. Die Herausforderung bei diesem Szenario ist das Entwerfen eines Indexschemas, das für alle eingehenden Daten funktioniert, und eine Dokumentschlüsselstruktur, die im Suchindex einheitlich ist. Systemintern sind die Werte, die ein Dokument eindeutig identifizieren, „metadata_storage_path“ in einem Blobcontainer und ein Primärschlüssel in einer SQL-Tabelle. Sie können sich vorstellen, dass eine oder beide Quellen so geändert werden müssen, dass Schlüsselwerte – unabhängig vom Inhaltsursprung – in einem gemeinsamen Format bereitgestellt werden. Bei diesem Szenario sollten Sie davon ausgehen, dass Sie ein gewisses Maß an Vorverarbeitung zum Homogenisieren der Daten durchführen müssen, damit sie in einen einzelnen Index gepullt werden können.</br></br>Eine Alternative könnten Suchdokumente sein, die bei der ersten Ausführung teilweise und dann durch nachfolgende Ausführungen weiter aufgefüllt werden, um Werte aus anderen Quellen einzubeziehen. Beispielsweise stammen die Felder 1–10 aus Blob Storage, die Felder 11–20 aus Azure SQL usw. Die Herausforderung bei diesem Muster besteht darin sicherzustellen, dass das Ziel jedes Indizierungslaufs dasselbe Dokument ist. Zum Zusammenführen von Feldern in ein vorhandenes Dokument muss der Dokumentschlüssel übereinstimmen. Eine Demonstration dieses Szenarios finden Sie im [Tutorial: Indizieren von mehreren Datenquellen mithilfe des .NET SDK](tutorial-multiple-data-sources.md) |
| Inhaltstransformation | Cognitive Search unterstützt optionale Verhaltensweisen der [KI-Anreicherung](cognitive-search-concept-intro.md), die Bildanalyse und Verarbeitung natürlicher Sprache zum Erstellen neuer durchsuchbarer Inhalte und Struktur hinzufügen. KI-Anreicherung ist über ein angefügtes [Skillset](cognitive-search-working-with-skillsets.md) indexergesteuert. Für die KI-Anreicherung benötigt der Indexer noch einen Index und eine Datenquelle, aber in diesem Szenario wird der Indexerausführung Skillsetverarbeitung hinzugefügt. |

## <a name="approaches-for-creating-and-managing-indexers"></a>Ansätze zum Erstellen und Verwalten von Indexern

Indexer können mit folgenden Tools erstellt und verwaltet werden:

+ [Portal > Datenimport-Assistent ](search-import-data-portal.md)
+ [Dienst-REST-API](/rest/api/searchservice/Indexer-operations)
+ [.NET SDK](/dotnet/api/azure.search.documents.indexes.models.searchindexer)

Wenn Sie ein SDK verwenden, erstellen Sie einen [SearchIndexerClient](/dotnet/api/azure.search.documents.indexes.searchindexerclient) zum Arbeiten mit Indexern, Datenquellen und Skillsets. Der vorstehende Link ist für das .NET SDK vorgesehen, aber alle SDKs stellen einen SearchIndexerClient und ähnliche APIs bereit.

Anfänglich werden neue Datenquellen als Previewfunktionen angekündigt und sind „nur REST“. Nach dem Übergang in allgemeine Verfügbarkeit wird vollständiger Support in das Portal und die verschiedenen SDKs integriert, bei denen es jeweils eigene Releasezeitpläne gibt.

## <a name="permissions"></a>Berechtigungen

Alle Vorgänge im Zusammenhang mit Indexern, einschließlich GET-Anforderungen für den Status oder Definitionen, erfordern einen [Admin-API-Schlüssel](search-security-api-keys.md).

<a name="supported-data-sources"></a>

## <a name="supported-data-sources"></a>Unterstützte Datenquellen

Indexer durchforsten Datenspeicher in Azure.

+ [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Azure Data Lake Storage Gen2](search-howto-index-azure-data-lake-storage.md) (in der Vorschau)
+ [Azure Table Storage](search-howto-indexing-azure-tables.md)
+ [Azure Cosmos DB](search-howto-index-cosmosdb.md)
+ [Azure SQL-Datenbank](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [SQL Managed Instance](search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers.md)
+ [SQL Server auf virtuellen Azure-Computern](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md)

## <a name="stages-of-indexing"></a>Phasen der Indizierung

Bei einer anfänglichen Ausführung, bei der der Index leer ist, liest ein Indexer alle Daten, die in der Tabelle oder im Container bereitgestellt werden. Bei nachfolgenden Ausführungen kann der Indexer in der Regel nur die geänderten Daten erkennen und abrufen. Bei Blobdaten erfolgt die Änderungserkennung automatisch. Bei anderen Datenquellen wie Azure SQL oder Cosmos DB muss die Änderungserkennung aktiviert werden.

Für jedes Dokument, das ein Indexer erfasst, implementiert oder koordiniert er mehrere Schritte, vom Abrufen des Dokuments bis hin zu einer endgültigen Suchmaschinenübergabe für die Indizierung. Optional ist ein Indexer auch bei der Initiierung von Skillset-Ausführungen und -Ausgaben dienlich, vorausgesetzt, dass ein Skillset definiert ist.

:::image type="content" source="media/search-indexer-overview/indexer-stages.png" alt-text="Indexerphasen" border="false":::

### <a name="stage-1-document-cracking"></a>Phase 1: Dokumententschlüsselung

Dokumententschlüsselung ist der Vorgang des Öffnens von Dateien und des Extrahierens von Inhalt. Abhängig vom Typ der Datenquelle versucht der Indexer, verschiedene Vorgänge auszuführen, um potenziell indizierbaren Inhalt zu extrahieren.  

Beispiele:  

+ Wenn es sich bei dem Dokument um einen Datensatz in einer [Azure SQL-Datenquelle](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) handelt, extrahiert der Indexer alle Felder für den Datensatz.
+ Wenn das Dokument eine PDF-Datei in einer [Azure Blob Storage-Datenquelle](search-howto-indexing-azure-blob-storage.md) ist, extrahiert der Indexer Text, Bilder und Metadaten.
+ Wenn das Dokument ein Datensatz in einer [Cosmos DB-Datenquelle](search-howto-index-cosmosdb.md) ist, extrahiert der Indexer die Felder und Unterfelder aus dem Cosmos DB-Dokument.

### <a name="stage-2-field-mappings"></a>Phase 2: Feldzuordnungen 

Ein Indexer extrahiert Text aus einem Quellfeld und sendet ihn an ein Zielfeld in einem Index oder Wissensspeicher. Wenn Feldnamen und -typen übereinstimmen, ist der Pfad klar. Möglicherweise möchten Sie aber unterschiedliche Namen oder Typen in der Ausgabe, in welchem Fall Sie dem Indexer mitteilen müssen, wie das Feld zugeordnet werden soll. Dieser Schritt erfolgt nach der Dokumententschlüsselung, aber vor Transformationen, wenn der Indexer aus den Quelldokumenten liest. Wenn Sie eine [Feldzuordnung](search-indexer-field-mappings.md) definieren, wird der Wert des Quellfelds unverändert an das Zielfeld gesendet. Feldzuordnungen sind optional.

### <a name="stage-3-skillset-execution"></a>Phase 3: Skillset-Ausführung

Skillset-Ausführung ist ein optionaler Schritt, der die integrierte oder benutzerdefinierte KI-Verarbeitung aufruft. Sie benötigen diese möglicherweise für die optische Zeichenerkennung (OCR) in Form einer Bildanalyse, oder Sie benötigen eventuelle eine Sprachübersetzung. Wie auch immer die Transformation aussieht, die Skillset-Ausführung ist der Zeitpunkt, an dem eine Anreicherung erfolgt. Wenn ein Indexer eine Pipeline ist, können Sie sich einen [Skillset](cognitive-search-defining-skillset.md) als „Pipeline innerhalb der Pipeline“ vorstellen. Ein Skillset verfügt über eine eigene Abfolge von Schritten, die als Skills bezeichnet werden.

### <a name="stage-4-output-field-mappings"></a>Phase 4: Ausgabefeldzuordnungen

Wenn Sie ein Skillset einbeziehen, müssen Sie höchstwahrscheinlich auch Ausgabefeldzuordnungen einbeziehen. Die Ausgabe eines Skillsets ist tatsächlich eine Struktur von Informationen, die als angereichertes Dokument bezeichnet wird. Mithilfe von Ausgabefeldzuordnungen können Sie auswählen, welche Teile dieser Struktur Feldern in Ihrem Index zugeordnet werden sollen. Erfahren Sie, wie Sie [Ausgabefeldzuordnungen definieren](cognitive-search-output-field-mapping.md).

Während Feldzuordnungen Literalwerte aus der Datenquelle Zielfeldern zuordnen, informieren Ausgabefeldzuordnungen den Indexer, wie die transformierten Werte im angereicherten Dokument den Zielfeldern im Index zugeordnet werden sollen. Anders als bei Feldzuordnungen, die als optional angesehen werden, müssen Sie immer eine Ausgabefeldzuordnung für jeglichen transformierten Inhalt definieren, der sich in einem Index befinden muss.

Die nächste Abbildung zeigt eine Beispieldarstellung einer Indexer-[Debugsitzung](cognitive-search-debug-session.md)der Indexerphasen: Dokumententschlüsselung, Feldzuordnungen, Skillset-Ausführung und Ausgabefeldzuordnungen.

:::image type="content" source="media/search-indexer-overview/sample-debug-session.png" alt-text="Beispieldebugsitzung" lightbox="media/search-indexer-overview/sample-debug-session.png":::

## <a name="basic-configuration-steps"></a>Grundlegende Konfigurationsschritte

Indexer können Features bereitstellen, die für die Datenquelle eindeutig sind. In dieser Hinsicht variieren einige Aspekte von Indexern oder der Datenquellenkonfiguration nach Indexertyp. Für alle Indexer werden aber die gleiche grundlegende Zusammenstellung und die gleichen Anforderungen verwendet. Die Schritte, die für alle Indexer gelten, sind unten beschrieben.

### <a name="step-1-create-a-data-source"></a>Schritt 1: Erstellen einer Datenquelle

Ein Indexer ruft eine Datenquellenverbindung aus einem *Datenquelle*-Objekt ab. Die Datenquellendefinition enthält eine Verbindungszeichenfolge und ggf. Anmeldeinformationen. Rufen Sie die REST-API [Datenquelle erstellen](/rest/api/searchservice/create-data-source) oder die [SearchIndexerDataSourceConnection](/dotnet/api/azure.search.documents.indexes.models.searchindexerdatasourceconnection)-Klasse auf, um die Ressource zu erstellen.

Datenquellen werden unabhängig von den Indexern, die darauf zugreifen, konfiguriert und verwaltet. Dies bedeutet, dass eine Datenquelle von mehreren Indexern verwendet werden kann, um mehr als einen Index gleichzeitig zu laden.

### <a name="step-2-create-an-index"></a>Schritt 2: Erstellen eines Index

Mit einem Indexer werden einige Aufgaben in Bezug auf die Datenerfassung automatisiert, aber das Erstellen eines Index gehört im Allgemeinen nicht dazu. Als Voraussetzung hierfür müssen Sie über einen vordefinierten Index mit Feldern verfügen, die den Feldern in Ihrer externen Datenquelle entsprechen. Felder müssen nach Name und Datentyp übereinstimmen. Weitere Informationen zum Strukturieren eines Index finden Sie unter [Create Index (Azure Cognitive Search-REST-API)](/rest/api/searchservice/Create-Index) oder der [SearchIndex-Klasse](/dotnet/api/azure.search.documents.indexes.models.searchindex). Hilfreiche Informationen zu Feldzuordnungen finden Sie unter [Feldzuordnungen in Azure Cognitive Search-Indexern](search-indexer-field-mappings.md).

> [!Tip]
> Indexer können zwar keinen Index für Sie erstellen, diese Aufgabe kann jedoch mit dem Assistenten **Daten importieren** im Portal ausgeführt werden. In den meisten Fällen kann der Assistent ein Indexschema aus vorhandenen Metadaten in der Quelle ableiten und ein vorläufiges Indexschema bereitstellen, das Sie inline bearbeiten können, während der Assistent aktiv ist. Sobald der Index für den Dienst erstellt wurde, ist die weitere Bearbeitung im Portal hauptsächlich auf das Hinzufügen neuer Felder beschränkt. Verwenden Sie den Assistenten zum Erstellen, aber nicht zum Überarbeiten eines Index. Praxisnahe Lerninhalte finden Sie in der [exemplarischen Vorgehensweise zum Portal](search-get-started-portal.md).

### <a name="step-3-create-and-schedule-the-indexer"></a>Schritt 3: Erstellen und Planen des Indexers

Die Indexerdefinition ist ein Konstrukt, das alle Elemente im Zusammenhang mit der Datenerfassung kombiniert. Erforderliche Elemente enthalten eine Datenquelle und einen Index. Optionale Elemente enthalten einen Zeitplan und Feldzuordnungen. Feldzuordnungen sind nur optional, wenn Quellfelder und Indexfelder eindeutig übereinstimmen. Weitere Informationen zum Strukturieren eines Indexers finden Sie unter [Create Indexer (Azure Cognitive Search-REST-API)](/rest/api/searchservice/Create-Indexer).

<a id="RunIndexer"></a>

## <a name="run-indexers-on-demand"></a>Ausführen von Indexern bei Bedarf

Während es üblich ist, eine Indizierung zu planen, kann ein Indexer auch bei Bedarf mit dem [Befehl „Ausführen“](/rest/api/searchservice/run-indexer) aufgerufen werden:

```http
POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2020-06-30
api-key: [Search service admin key]
```

> [!NOTE]
> Wenn die Ausführungs-API einen Erfolgscode zurückgibt, wurde der Indexeraufruf geplant, doch die eigentliche Verarbeitung geschieht asynchron. 

Sie können den Indexerstatus im Portal oder über die [API zum Abrufen des Indexer-Status](/rest/api/searchservice/get-indexer-status) überwachen. 

<a name="GetIndexerStatus"></a>

## <a name="get-indexer-status"></a>Abrufen des Indexerstatus

Sie können den Status und Ausführungsverlauf eines Indexers über den [Befehl „Indexer-Status abrufen“](/rest/api/searchservice/get-indexer-status) abrufen:

```http
GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2020-06-30
api-key: [Search service admin key]
```

Die Antwort enthält den Gesamtstatus des Indexers, den letzten (oder laufenden) Aufruf des Indexers sowie den Verlauf der letzten Indexeraufrufe.

```output
{
    "status":"running",
    "lastResult": {
        "status":"success",
        "errorMessage":null,
        "startTime":"2018-11-26T03:37:18.853Z",
        "endTime":"2018-11-26T03:37:19.012Z",
        "errors":[],
        "itemsProcessed":11,
        "itemsFailed":0,
        "initialTrackingState":null,
        "finalTrackingState":null
     },
    "executionHistory":[ {
        "status":"success",
         "errorMessage":null,
        "startTime":"2018-11-26T03:37:18.853Z",
        "endTime":"2018-11-26T03:37:19.012Z",
        "errors":[],
        "itemsProcessed":11,
        "itemsFailed":0,
        "initialTrackingState":null,
        "finalTrackingState":null
    }]
}
```

Der Ausführungsverlauf enthält bis zu 50 der jüngsten abgeschlossenen Ausführungen. Diese sind in umgekehrter chronologischer Reihenfolge sortiert (somit ist die neueste Ausführung als Erstes in der Antwort aufgelistet).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich über die Grundlagen informiert haben, ist der nächste Schritt das Überprüfen der Anforderungen und Aufgaben für jeden Datenquellentyp.

+ [Azure SQL-Datenbank, SQL Managed Instance oder SQL Server auf einem virtuellen Azure-Computer](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB](search-howto-index-cosmosdb.md)
+ [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
+ [Azure Table Storage](search-howto-indexing-azure-tables.md)
+ [Indizieren von CSV-Blobs mit Azure Cognitive Search-Blobindexer](search-howto-index-csv-blobs.md)
+ [Indizieren von JSON-Blobs mit Azure Cognitive Search-Blobindexer](search-howto-index-json-blobs.md)