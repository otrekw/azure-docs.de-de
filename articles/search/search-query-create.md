---
title: Erstellen einer einfachen Abfrage
titleSuffix: Azure Cognitive Search
description: Erfahren Sie, wie Sie in Cognitive Search eine Abfrageanforderung erstellen, welche Tools und APIs Sie für Tests und beim Programmieren verwenden können und welchen Einfluss der Indexentwurf auf Abfrageentscheidungen hat.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/14/2020
ms.openlocfilehash: 9bee391ddb0fa6c270c6d833fb7e81d5f4880497
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2021
ms.locfileid: "98118641"
---
# <a name="create-a-query-in-azure-cognitive-search"></a>Erstellen einer Abfrage in Azure Cognitive Search

Wenn Sie zum ersten Mal eine Abfrage erstellen, finden Sie in diesem Artikel eine Beschreibung der benötigten Tools und APIs. Außerdem erfahren Sie, welche Methoden zum Erstellen einer Abfrage verwendet werden und wie sich Indexstruktur und -inhalt auf die Abfrageergebnisse auswirken. Eine Einführung in Abfrageanforderungen finden Sie unter [Abfragetypen und -erstellung](search-query-overview.md).

## <a name="choose-tools-and-apis"></a>Auswählen der Tools und APIs

Sie benötigen ein Tool oder eine API, um eine Abfrage erstellen zu können. Die folgenden Vorschläge eignen sich für Test- und Produktionsworkloads.

| Methodik | BESCHREIBUNG |
|-------------|-------------|
| Portal| Der [Suchexplorer (Portal)](search-explorer.md) ist eine Abfrageschnittstelle im Azure-Portal, mit dem Sie Indizes im zugrunde liegenden Suchdienst abfragen können. Im Portal werden im Hintergrund REST-API-Aufrufe an den Vorgang [Dokumente suchen](/rest/api/searchservice/search-documents) durchgeführt. Aufrufe von „AutoVervollständigen“, „Vorschläge“ oder „Dokumentsuche“ sind hingegen nicht möglich.<br/><br/> Sie können einen beliebigen Index und eine REST-API-Version auswählen (einschließlich der Vorschauversion). Für die Abfragezeichenfolge kann die einfache oder vollständige Syntax mit Unterstützung für alle Abfrageparameter (filter, select, searchFields usw.) verwendet werden. Wenn Sie im Portal einen Index öffnen, können Sie den Suchexplorer zusammen mit der JSON-Indexdefinition in parallelen Registerkarten nutzen, um einfacher auf Feldattribute zugreifen zu können. Überprüfen Sie beim Testen Ihrer Abfragen, welche Felder durchsuchbar, sortierbar, filterbar und facettierbar sind. <br/>Empfohlen für das erste Erkunden, Testen und Validieren. [Weitere Informationen.](search-explorer.md) |
| Webtesttools| [Postman](search-get-started-rest.md) oder [Visual Studio Code](search-get-started-vs-code.md) bieten leistungsstarke Optionen für das Formulieren einer Anforderung vom Typ [Dokumente suchen](/rest/api/searchservice/search-documents) und jeder anderen Anforderung in REST. Die REST-APIs unterstützen jeden möglichen programmgesteuerten Vorgang in Azure Cognitive Search. Wenn Sie ein Tool wie Postman oder Visual Studio Code verwenden, können Sie Anforderungen interaktiv ausführen, um ihre Funktion besser zu verstehen, bevor Sie Arbeit in Code investieren. Ein Webtesttool ist auch eine gute Wahl, wenn Sie nicht die Berechtigungen „Mitwirkender“ oder „Administrator“ im Azure-Portal haben. Wenn Sie über eine Such-URL und einen Abfrage-API-Schlüssel verfügen, können Sie mit den Tools Abfragen für einen vorhandenen Index ausführen. |
| Azure SDK | Wenn Sie bereit sind, Code zu schreiben, können Sie die Clientbibliotheken von Azure.Search.Document in den Azure SDKs für .NET, Python, JavaScript oder Java verwenden. Für jedes SDK gilt ein eigener Veröffentlichungszeitplan, Sie können aber in allen Indizes erstellen und abfragen. <br/><br/>Mit [SearchClient (.NET)](/dotnet/api/azure.search.documents.searchclient) können Sie einen Suchindex in C# abfragen.  [Weitere Informationen.](search-howto-dotnet-sdk.md)<br/><br/>Mit [SearchClient (Python)](/dotnet/api/azure.search.documents.searchclient) können Sie einen Suchindex in Python abfragen. [Weitere Informationen.](search-get-started-python.md)<br/><br/>Mit [SearchClient (JavaScript)](/dotnet/api/azure.search.documents.searchclient) können Sie einen Suchindex in JavaScript abfragen. [Weitere Informationen.](search-get-started-javascript.md) |

## <a name="set-up-a-search-client"></a>Einrichten eines Suchclients

Ein Suchclient authentifiziert sich beim Suchdienst, sendet Anforderungen und verarbeitet die Antworten. Ein Suchclient muss unabhängig vom verwendeten Tool oder der API über Folgendes verfügen:

| Eigenschaften | BESCHREIBUNG |
|------------|-------------|
| Endpunkt | Ein Suchdienst ist eine URL, die im folgenden Format adressierbar ist: `https://[service-name].search.windows.net`. |
| API-Zugriffsschlüssel (Administrator oder Abfrage) | Authentifiziert die Anforderung beim Suchdienst |
| Indexname | Abfragen werden immer an die Dokumentsammlung eines einzelnen Index weitergeleitet. Es ist nicht möglich, Indizes zu verknüpfen oder benutzerdefinierte oder temporäre Datenstrukturen als Abfrageziel zu erstellen. |
| API-Version | REST-Aufrufe erfordern explizit die `api-version` in der Anforderung. Im Gegensatz dazu werden Clientbibliotheken im Azure SDK für eine bestimmte REST-API-Version versioniert. Bei SDKs ist die `api-version` implizit. |

### <a name="in-the-portal"></a>Im Portal

Der Suchexplorer und andere Tools im Portal verfügen über eine integrierte Clientverbindung mit dem Dienst mit direktem Zugriff auf Indizes und andere Objekte auf den Seiten im Portal. Der Zugriff auf Tools, Assistenten und Objekte erfordert die Mitgliedschaft in der Rolle „Mitwirkender“ oder höher für den Dienst. 

### <a name="using-rest"></a>Verwenden von REST

Bei REST-Aufrufen können Sie [Postman oder ähnliche Tools](search-get-started-rest.md) als Client verwenden, um eine Anforderung vom Typ [Dokumente suchen](/rest/api/searchservice/search-documents) anzugeben. Jede Anforderung ist eigenständig. Daher müssen Sie den Endpunkt, den Indexnamen und die API-Version für jede Anforderung angeben. Andere Eigenschaften, der Inhaltstyp und die API-Schlüssel werden im Anforderungsheader weitergegeben. 

Sie können einen Index mit POST oder GET abfragen. POST ist bei Angabe der Parameter im Anforderungstext einfacher zu verwenden. Wenn Sie POST verwenden, müssen Sie `docs/search` in die URL einschließen:

```http
POST https://myservice.search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "count": true,
    "queryType": "simple",
    "search": "*"
}
```

### <a name="using-azure-sdks"></a>Verwenden von Azure SDKs

Wenn Sie ein Azure SDK verwenden, erstellen Sie den Client im Code. Alle SDKs stellen Suchclients bereit, die den Zustand beibehalten können, sodass die Verbindungen wiederverwendet werden können. Für Abfragevorgänge instanziieren Sie einen **`SearchClient`** und geben Werte für die folgenden Eigenschaften an: Endpoint, Key und Index. Sie können dann die **`Search method`** aufrufen, um die Abfragezeichenfolge zu übergeben. 

| Sprache | Client | Beispiel |
|----------|--------|---------|
| C# und .NET | [SearchClient](/dotnet/api/azure.search.documents.searchclient) | [Senden einer ersten Suchabfrage in C#](/dotnet/api/overview/azure/search.documents-readme#send-your-first-search-query) |
| Python      | [SearchClient](/python/api/azure-search-documents/azure.search.documents.searchclient) | [Senden einer ersten Suchabfrage in Python](/python/api/overview/azure/search-documents-readme#send-your-first-search-request) |
| Java        | [SearchClient](/java/api/com.azure.search.documents.searchclient) | [Senden einer ersten Suchabfrage in Java](/java/api/overview/azure/search-documents-readme#send-your-first-search-query)  |
| JavaScript  | [SearchClient](/javascript/api/@azure/search-documents/searchclient) | [Senden einer ersten Suchabfrage in JavaScript](/javascript/api/overview/azure/search-documents-readme#send-your-first-search-query)  |

## <a name="choose-a-parser-simple--full"></a>Auswählen eines Parsers: einfach | vollständig

Wenn es sich bei der Abfrage um eine Volltextsuche handelt, wird der Inhalt des Suchparameters mithilfe eines Parsers verarbeitet. Azure Cognitive Search bietet zwei Abfrageparser. Der einfache Parser versteht die [einfache Abfragesyntax](query-simple-syntax.md). Dieser Parser wird standardmäßig verwendet, da er bei Abfragen von Freiformtext eine hohe Geschwindigkeit und Effizienz bietet. Die Syntax unterstützt gängige Suchoperatoren (AND, OR, NOT) für die Suche nach Begriffen und Ausdrücken und die Präfixsuche (`*`, wie in „Sea*“ für „Seattle“ und „Seaside“). Ganz allgemein wird empfohlen, zuerst den einfachen Parser auszuprobieren und nur dann zum vollständigen Parser zu wechseln, wenn Anwendungsanforderungen leistungsfähigere Abfragen erfordern.

Die [vollständige Lucene-Abfragesyntax](query-Lucene-syntax.md#bkmk_syntax), die beim Hinzufügen von `queryType=full` zur Anforderung aktiviert wird, basiert auf dem [Apache Lucene-Parser](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html).

Vollständige Syntax und einfache Syntax stimmen darin überein, dass beide dieselben Präfix- und booleschen Vorgänge unterstützen. Bei der vollständigen Syntax stehen Ihnen allerdings mehr Operatoren zur Verfügung. Beim vollständigen Parser gibt es mehr Operatoren für boolesche Ausdrücke und mehr Operatoren für komplexere Abfragen, wie z. B. die Fuzzysuche, die Platzhaltersuche, die NEAR-Suche und reguläre Ausdrücke.

## <a name="choose-query-methods"></a>Auswählen der Abfragemethoden

Die Suche ist im Grunde genommen ein vom Benutzer gesteuerter Vorgang, bei dem Begriffe oder Ausdrücke aus einem Suchfeld oder von Klickereignissen auf einer Seite gesammelt werden. In der folgenden Tabelle werden die Verfahren zusammengefasst, mit denen Sie Benutzereingaben erfassen können. Außerdem sind die erwarteten Suchvorgänge aufgeführt.

| Eingabe | Erfahrung |
|-------|---------|
| [Suche](/rest/api/searchservice/search-documents) | Benutzer geben Begriffe oder Ausdrücke (mit oder ohne Operatoren) in ein Suchfeld ein und klicken auf „Suchen“, um die Anforderung zu senden. Die Suche kann mit Filtern für dieselbe Anforderung, jedoch nicht mit AutoVervollständigen oder Vorschlägen verwendet werden. |
| [AutoVervollständigen](/rest/api/searchservice/autocomplete) | Benutzer geben einige Zeichen ein, und nach der Eingabe jedes neuen Zeichens werden Abfragen initiiert. Die Antwort ist eine vollständige Zeichenfolge aus dem Index. Wenn die bereitgestellte Zeichenfolge passend ist, klicken die Benutzer auf „Suchen“, um die Abfrage an den Dienst zu senden. |
| [Vorschläge](/rest/api/searchservice/suggestions) | Wie beim AutoVervollständigen werden während der Eingabe einiger Zeichen durch Benutzer inkrementelle Abfragen generiert. Die Antwort ist eine Dropdownliste mit übereinstimmenden Dokumenten, die in der Regel durch einige eindeutige oder beschreibende Felder dargestellt werden. Wenn eine der Optionen passend ist, klicken die Benutzer darauf, und das entsprechende Dokument wird zurückgegeben. |
| [Facettennavigation](/rest/api/searchservice/search-documents#query-parameters) | Auf einer Seite werden klickbare Navigationslinks oder „Brotkrümel“ (Breadcrumbs) angezeigt, die den Suchbereich eingrenzen. Die Struktur der Facettennavigationsstruktur wird dynamisch basierend auf einer anfänglichen Abfrage gebildet. Mit `search=*` wird z. B. eine Facettennavigationsstruktur aufgefüllt, die alle möglichen Kategorien umfasst. Die Struktur bei einer Facettennavigation wird aus einer Abfrageantwort erstellt, sie dient aber auch zum Ausdrücken der nächsten Abfrage. In der REST-API-Referenz ist `facets` als Abfrageparameter eines Vorgangs „Dokumente suchen“ dokumentiert, er kann jedoch auch ohne den `search`-Parameter verwendet werden.|
| [Filtermethode](/rest/api/searchservice/search-documents#query-parameters) | Mithilfe von Filtern können Sie die Ergebnisse von Facetten eingrenzen. Sie können auch einen Filter im Hintergrund der Seite implementieren, um die Seite z. B. mit sprachspezifischen Feldern zu initialisieren. In der REST-API-Referenz ist `$filter` als Abfrageparameter eines Vorgangs „Dokumente suchen“ dokumentiert, er kann jedoch auch ohne den `search`-Parameter verwendet werden.|

## <a name="know-your-field-attributes"></a>Verstehen der Feldattribute

Wenn Sie sich bereits mit den [Grundlagen einer Abfrageanforderung](search-query-overview.md) vertraut gemacht haben, erinnern Sie sich möglicherweise daran, dass die Parameter in der Abfrageanforderung davon abhängen, wie Feldern in einem Index Attribute zugeordnet werden. Damit ein Feld beispielsweise in einer Abfrage, einem Filter oder einer Sortierreihenfolge verwendet werden kann, muss es *durchsuchbar*, *filterbar* und *sortierbar* sein. Darüber hinaus können in Ergebnissen nur Felder angezeigt werden, die als *abrufbar* gekennzeichnet sind. Wenn Sie damit beginnen, in Ihrer Anforderung die Parameter `search`, `filter` und `orderby` anzugeben, sollten Sie unbedingt die Attribute überprüfen, um unerwartete Ergebnisse zu vermeiden.

Im folgenden Screenshot des Portals mit dem [Beispielindex für Hotels](search-get-started-portal.md) können nur die letzten beiden Felder „LastRenovationDate“ und „Rating“ in einer `"$orderby"`-Klausel verwendet werden.

![Indexdefinition für das „hotels“-Beispiel](./media/search-query-overview/hotel-sample-index-definition.png "Indexdefinition für das „hotels“-Beispiel")

Eine Beschreibung der Feldattribute finden Sie unter [Erstellen eines Index (REST-API)](/rest/api/searchservice/create-index).

## <a name="know-your-tokens"></a>Verstehen der Token

Während der Indizierung verwendet die Abfrage-Engine ein Analysetool, um Textanalysen für Zeichenfolgen durchzuführen und so die Wahrscheinlichkeit von Übereinstimmungen zur Abfragezeit zu maximieren. Zeichenfolgen sind mindestens kleingeschrieben. Sie können aber auch eine Lemmatisierung und Stoppwortentfernung durchlaufen. Größere Zeichenfolgen oder zusammengesetzte Wörter werden in der Regel an Leerzeichen, Bindestrichen oder Gedankenstrichen aufgeteilt und als separate Token indiziert. 

Sie sollten also immer davon ausgehen, dass der Index etwas anderes enthält, als Sie erwarten. Wenn Ihre Abfragen nicht die erwarteten Ergebnisse zurückgeben, können Sie die vom Analysetool erstellten Token mit [Text analysieren (REST-API)](/rest/api/searchservice/test-analyzer) überprüfen. Weitere Informationen zur Tokenisierung und ihrer Auswirkungen auf Abfragen finden Sie unter [Suche nach Teilausdrücken und Mustern mit Sonderzeichen](search-query-partial-matching.md).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun ein besseres Verständnis von der Erstellung von Abfrageanforderungen haben, können Sie in den folgenden Schnellstarts praktische Erfahrungen sammeln.

+ [Suchexplorer](search-explorer.md)
+ [Abfragen des Azure Search-Index mit der REST-API](search-get-started-rest.md)
+ [Abfragen des Azure Search-Index mit dem .NET SDK](search-get-started-dotnet.md)
+ [Erstellen eines Azure Cognitive Search-Index in Python mithilfe von Jupyter Notebook-Instanzen](search-get-started-python.md)
+ [Erstellen eines Index für Azure Cognitive Search mit dem JavaScript-SDK](search-get-started-javascript.md)