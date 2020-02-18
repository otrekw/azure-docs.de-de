---
title: Previewfunktionen in der REST-API
titleSuffix: Azure Cognitive Search
description: Die REST-API für den Azure Cognitive Search-Dienst Version 2019-05-06-Preview beinhaltet experimentelle Funktionen wie Wissensspeicher und Indexerzwischenspeicherung für die inkrementelle Anreicherung.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: db941152186127302680b5e659e43cd2d82a8908
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77162275"
---
# <a name="preview-features-in-azure-cognitive-search"></a>Previewfunktionen in Azure Cognitive Search

In diesem Artikel werden die Funktionen aufgeführt, die derzeit in der Vorschauversion verfügbar sind. Funktionen, die aus der Vorschauphase in die allgemeine Verfügbarkeit übergehen, werden aus dieser Liste entfernt. In den [Dienstupdates](https://azure.microsoft.com/updates/?product=search) sowie unter [Neuigkeiten](whats-new.md) finden Sie Ankündigungen für die allgemeine Verfügbarkeit.

Auch wenn einige Previewfunktionen im Portal und im .NET SDK verfügbar sein können, enthält die REST-API immer alle Previewfunktionen.

+ Für Suchvorgänge lautet die aktuelle Vorschauversion [ **`2019-05-06-Preview`** ](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview).
+ Für Verwaltungsvorgänge lautet die aktuelle Vorschauversion [ **`2019-10-01-Preview`** ](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview).

> [!IMPORTANT]
> Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="ai-enrichment-features"></a>Funktionen zur KI-Anreicherung

Informieren Sie sich über die neuesten Ergänzungen für die KI-Anreicherung durch die [Vorschauversion der Such-API](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview).

|||
|-|-|
| [Qualifikation “Benutzerdefinierte Entitätssuche“ (Vorschau)](cognitive-search-skill-custom-entity-lookup.md ) | Eine kognitive Fähigkeit, die nach Text aus einer benutzerdefinierten Liste von Wörtern und Ausdrücken sucht. Mithilfe dieser Liste werden alle Dokumente mit übereinstimmenden Entitäten mit einer Bezeichnung markiert. Die Qualifikation unterstützt auch einen gewissen Grad an Fuzzyübereinstimmung, der für die Suche nach ähnlichen, aber nicht exakten Übereinstimmungen verwendet werden kann. | 
| [Qualifikation „PII-Erkennung“ (Vorschau)](cognitive-search-skill-pii-detection.md) | Eine kognitive Fähigkeit, die bei der Indizierung eingesetzt wird. Sie extrahiert personenbezogene Informationen aus einem Eingabetext und bietet verschiedene Maskierungsmöglichkeiten für diese Informationen.| 
| [Inkrementelle Anreicherung (Vorschau)](cognitive-search-incremental-indexing-conceptual.md) | Fügt einer Anreicherungspipeline die Zwischenspeicherung hinzu, sodass Sie vorhandene Ausgaben wiederverwenden können, wenn die Inhalte durch eine Zieländerung, z. B. eine Aktualisierung am Skillset oder einem anderen Objekt, nicht geändert werden. Die Zwischenspeicherung betrifft nur angereicherte Dokumente, die über ein Skillset erstellt werden.| 
| [Wissensspeicher (Vorschau)](knowledge-store-concept-intro.md) | Ein neues Ziel einer KI-basierten Anreicherungspipeline. Die physische Datenstruktur befindet sich in Azure Blob Storage und Azure Table Storage und wird erstellt und aufgefüllt, wenn Sie einen Indexer ausführen, der über ein angefügtes kognitives Skillset verfügt. Die eigentliche Definition eines Wissensspeichers wird innerhalb einer Skillsetdefinition angegeben. Innerhalb der Wissensspeicherdefinition steuern Sie die physischen Strukturen Ihrer Daten über *Projektionselemente*, die bestimmen, wie Daten dargestellt werden, ob Daten in Table Storage oder Blob Storage gespeichert werden und ob mehrere Ansichten verfügbar sind.| 

## <a name="indexing-and-query-features"></a>Indizierungs- und Abfragefunktionen

Die Previewfunktionen des Indexers sind in der Vorschauversion der Such-API verfügbar. 

|||
|-|-|
| [Cosmos DB-Indexer](search-howto-index-cosmosdb.md) | Unterstützt die API-Typen MongoDB-API (Vorschau), Gremlin-API (Vorschau) und Cassandra-API (Vorschau). | 
|  [Azure Data Lake Storage Gen2-Indexers (Vorschau)](search-howto-index-azure-data-lake-storage.md) | Indizieren von Inhalten und Metadaten aus Data Lake Storage Gen2.| 
| [Abfrageparameter „moreLikeThis“ (Vorschau)](search-more-like-this.md) | Sucht Dokumente, die für ein bestimmtes Dokument relevant sind. Dieses Feature war in früheren Vorschauversionen enthalten. | 

## <a name="management-features"></a>Verwaltungsfunktionen

|||
|-|-|
| [Unterstützung für den privaten Endpunkt](service-create-private-endpoint.md) | Sie können ein virtuelles Netzwerk mit einem sicheren Client (z. B. einem virtuellen Computer) und dann einen Suchdienst erstellen, der einen privaten Endpunkt verwendet. |
| IP-Zugriffseinschränkung | Mithilfe von [`api-version=2019-10-01-Preview`](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) der Verwaltungs-REST-API können Sie einen Dienst mit Einschränkungen für den Zugriff auf IP-Adressen erstellen. |

## <a name="earlier-preview-features"></a>Frühere Previewfunktionen

In früheren Vorschauversionen angekündigte Funktionen befinden sich, wenn sie noch nicht auf die allgemeine Verfügbarkeit umgestellt wurden, weiterhin in der öffentlichen Vorschau. Wenn Sie eine API mit einer früheren API-Vorschauversion aufrufen, können Sie weiterhin diese Version verwenden oder zu `2019-05-06-Preview` wechseln, ohne Änderungen am erwarteten Verhalten feststellen zu müssen.

## <a name="how-to-call-a-preview-api"></a>Aufrufen einer Vorschau-API

Ältere Vorschauversionen sind zwar noch betriebsbereit, veralten jedoch mit der Zeit. Wenn Ihr Code `api-version=2016-09-01-Preview` oder `api-version=2017-11-11-Preview` aufruft, sind diese Aufrufe noch gültig. Allerdings wird nur die neueste Vorschauversion mit Verbesserungen aktualisiert. 

Die folgende Beispielsyntax veranschaulicht einen Aufruf der API-Vorschauversion.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Der Dienst für die kognitive Azure-Suche ist in mehreren Versionen verfügbar. Weitere Informationen finden Sie unter [API-Versionen](search-api-versions.md).

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die Referenzdokumentation für die Search-REST-API an. Wenn Probleme auftreten, können Sie sich über [Stack Overflow](https://stackoverflow.com/) an uns wenden oder [den Support kontaktieren](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Referenz zur REST-API für den Azure Search-Dienst](https://docs.microsoft.com/rest/api/searchservice/)