---
title: REST-API-Version 2019-05-06-Preview
titleSuffix: Azure Cognitive Search
description: Die REST-API für den Azure Cognitive Search-Dienst, Version 2019-05-06-Preview, beinhaltet experimentelle Funktionen wie Wissensspeicher und Indexer-Zwischenspeicherung für inkrementelle Anreicherung.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/04/2020
ms.openlocfilehash: eb73d614ca94bc1fa007a14f3705e50c74ab9e4f
ms.sourcegitcommit: f34165bdfd27982bdae836d79b7290831a518f12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2020
ms.locfileid: "75922479"
---
# <a name="azure-cognitive-search-service-rest-api-version-2019-05-06-preview"></a>REST-API für den Dienst für die kognitive Azure-Suche: Version 2019-05-06-Preview

Dieser Artikel beschreibt die Version `api-version=2019-05-06-Preview` der Search-Dienst-REST-API, welche die folgenden experimentellen Features bietet, die noch nicht allgemein verfügbar sind.

> [!NOTE]
> Vorschaufeatures stehen für Tests und Versuche zur Verfügung, um Feedback zu sammeln, und können sich ändern. Wir raten dringend davon ab, Vorschau-APIs in Produktionsanwendungen einzusetzen.


## <a name="new-in-2019-05-06-preview"></a>Neuerungen in 2019-05-06-Preview

+ Durch die [inkrementelle Anreicherung (Vorschauversion)](cognitive-search-incremental-indexing-conceptual.md) wird die Zwischenspeicherung einer Anreicherungspipeline hinzugefügt, sodass Sie vorhandene Ausgaben wiederverwenden können, wenn die Inhalte durch eine Zieländerung, z. B. eine Aktualisierung am Skillset oder einem anderen Objekt, nicht geändert werden. Die Zwischenspeicherung betrifft nur angereicherte Dokumente, die über ein Skillset erstellt werden.

+ Der [Cosmos DB-Indexer](search-howto-index-cosmosdb.md) unterstützt die MongoDB-API (Vorschauversion), die Gremlin-API (Vorschauversion) und die Cassandra-API (Vorschauversion).

+ Der [Azure Data Lake Storage Gen2-Indexer (Vorschauversion)](search-howto-index-azure-data-lake-storage.md) kann Inhalte und Metadaten aus Data Lake Storage Gen2 indizieren.

+ Die [Dokumentextrahierung (Vorschauversion)](cognitive-search-skill-document-extraction.md) ist eine kognitive Qualifikation, die während der Indizierung verwendet wird und es Ihnen ermöglicht, den Inhalt einer Datei aus einem Skillset zu extrahieren. Bisher fand die Dokumententschlüsselung nur vor der Ausführung von Qualifikationsgruppen statt. Durch diese zusätzliche Qualifikation können Sie diesen Vorgang auch bei der Ausführung von Qualifikationsgruppen durchführen.

+ Die [Textübersetzung (Vorschauversion)](cognitive-search-skill-text-translation.md) ist eine kognitive Qualifikation, die während der Indizierung zum Auswerten von Text verwendet wird und für jeden Datensatz Text zurückgibt, der in die angegebene Zielsprache übersetzt wurde.

+ Der [Wissensspeicher](knowledge-store-concept-intro.md) ist ein neues Ziel einer KI-basierten Anreicherungspipeline. Die physische Datenstruktur befindet sich in Azure Blob Storage und Azure Table Storage und wird erstellt und aufgefüllt, wenn Sie einen Indexer ausführen, der über ein angefügtes kognitives Skillset verfügt. Die eigentliche Definition eines Wissensspeichers wird innerhalb einer Skillsetdefinition angegeben. Innerhalb der Wissensspeicherdefinition steuern Sie die physischen Strukturen Ihrer Daten über *Projektionselemente*, die bestimmen, wie Daten dargestellt werden, ob Daten in Table Storage oder Blob Storage gespeichert werden und ob mehrere Ansichten verfügbar sind.

## <a name="earlier-preview-features"></a>Frühere Previewfunktionen

In früheren Vorschauversionen angekündigte Features befinden sich immer noch in der öffentlichen Vorschau. Wenn Sie eine API mit einer früheren API-Vorschauversion aufrufen, können Sie weiterhin diese Version verwenden oder zu `2019-05-06-Preview` wechseln, ohne Änderungen am erwarteten Verhalten feststellen zu müssen.

+ [moreLikeThis-Abfrageparameter](search-more-like-this.md) sucht Dokumente, die für ein bestimmtes Dokument relevant sind. Dieses Feature war in früheren Vorschauversionen enthalten. 

+ [CSV-Blobindizierung](search-howto-index-csv-blobs.md) erstellt ein Dokument pro Zeile, statt einem Dokument pro Textblob.

## <a name="how-to-call-a-preview-api"></a>Aufrufen einer Vorschau-API

Ältere Vorschauversionen sind zwar noch betriebsbereit, veralten jedoch mit der Zeit. Wenn Ihr Code `api-version=2016-09-01-Preview` oder `api-version=2017-11-11-Preview` aufruft, sind diese Aufrufe noch gültig. Allerdings wird nur die neueste Vorschauversion mit Verbesserungen aktualisiert. 

Die folgende Beispielsyntax veranschaulicht einen Aufruf der API-Vorschauversion.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Der Dienst für die kognitive Azure-Suche ist in mehreren Versionen verfügbar. Weitere Informationen finden Sie unter [API-Versionen](search-api-versions.md).

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die Referenzdokumentation für die Search-REST-API an. Wenn Probleme auftreten, können Sie sich über [Stack Overflow](https://stackoverflow.com/) an uns wenden oder [den Support kontaktieren](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Referenz zur REST-API für den Azure Search-Dienst](https://docs.microsoft.com/rest/api/searchservice/)