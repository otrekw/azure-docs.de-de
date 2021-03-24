---
title: Verwenden der vollständigen Lucene-Abfragesyntax
titleSuffix: Azure Cognitive Search
description: Abfragebeispiele zum Demonstrieren der Lucene-Abfragesyntax für Fuzzysuche, NEAR-Suche, Term Boosting, Suche mit regulären Ausdrücken und Platzhaltersuche in einem Azure Cognitive Search-Index.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/03/2021
ms.openlocfilehash: 6213efb6ba14052c6f957a6d999f48f55f65186c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101693559"
---
# <a name="use-the-full-lucene-search-syntax-advanced-queries-in-azure-cognitive-search"></a>Verwenden der „vollständigen“ Lucene-Suchsyntax (erweiterte Abfragen in der kognitiven Azure-Suche)

Beim Erstellen von Abfragen für Azure Cognitive Search können Sie den standardmäßigen [einfachen Abfrageparser](query-simple-syntax.md) durch den leistungsfähigeren [Lucene-Abfrageparser](query-lucene-syntax.md) ersetzen, um spezialisierte und erweiterte Abfrageausdrücke zu formulieren.

Der Lucene-Parser unterstützt komplexe Abfrageformate, z. B. feldbezogene Abfragen, Fuzzysuche, Infix- und Suffixplatzhaltersuche, NEAR-Suche, Term Boosting (Begriffsverstärkung) und die Suche mit regulären Ausdrücken. Die zusätzliche Leistung kommt mit zusätzlichen Verarbeitungsanforderungen, stellen Sie sich deshalb auf eine etwas längere Ausführungsdauer ein. In diesem Artikel finden Sie Beispiele, in denen Abfragevorgänge basierend auf der vollständigen Syntax veranschaulicht werden.

> [!Note]
> Viele der spezialisierten Abfragekonstruktionen, die über die vollständige Lucene-Abfragesyntax aktiviert werden, verfügen nicht über eine [Textanalyse](search-lucene-query-architecture.md#stage-2-lexical-analysis). Dies kann zu Überraschungen führen, wenn Sie mit Wortstammerkennung und Lemmatisierung rechnen. Die lexikalische Analyse erfolgt nur für vollständige Begriffe (Begriffsabfrage oder Ausdrucksabfrage). Abfragetypen mit unvollständigen Begriffen (Präfixabfrage, Platzerhalterabfrage, Abfrage regulärer Ausdrücke, Fuzzyabfrage) werden direkt unter Umgehung der Analysephase an die Abfragestruktur angehängt. Die einzige Transformation, die für partielle Abfrageausdrücke durchgeführt wird, ist die Umwandlung in Kleinbuchstaben. 
>

## <a name="hotels-sample-index"></a>Beispielindex für Hotels

Die folgenden Abfragen basieren auf „hotels-sample-index“. Diesen Index können Sie mithilfe der Anweisungen in diesem [Schnellstart](search-get-started-portal.md) erstellen.

Beispielabfragen werden mithilfe der REST-API und POST-Anforderungen formuliert. Sie können sie in [Postman](search-get-started-rest.md) oder in [Visual Studio Code mit Cognitive Search-Erweiterung](search-get-started-vs-code.md) einfügen und ausführen.

Anforderungsheader müssen die folgenden Werte aufweisen:

| Schlüssel | Wert |
|-----|-------|
| Content-Type | Anwendung/json|
| api-key  | `<your-search-service-api-key>`, entweder Abfrage- oder Administratorschlüssel |

URI-Parameter müssen Ihren Suchdienstendpunkt mit dem Indexnamen, den Dokumentationssammlungen, dem Suchbefehl und der API-Version enthalten, ähnlich wie im folgenden Beispiel gezeigt:

```http
https://{{service-name}}.search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30
```

Der Anforderungstext muss als gültiger JSON-Code vorliegen:

```json
{
    "search": "*",
    "queryType": "full",
    "select": "HotelId, HotelName, Category, Tags, Description",
    "count": true
}
```

+ Wenn „search“ auf `*` festgelegt ist, erfolgt die Abfrage ohne Angaben und entspricht einer NULL-Suche oder leeren Suche. Dies ist nicht besonders nützlich, aber es ist die einfachste Suche, die Sie ausführen können, und es werden alle abrufbaren Felder im Index mit allen Werten angezeigt.

+ Wenn „queryType“ auf „full“ festgelegt ist, wird der vollständige Lucene-Abfrageparser aufgerufen. Dieser ist für diese Syntax erforderlich.

+ „select“ ist auf eine durch Trennzeichen getrennte Liste von Feldern festgelegt und wird zur Zusammenstellung von Suchergebnissen verwendet. Es werden nur die Felder einbezogen, die im Kontext von Suchergebnissen nützlich sind.

+ „count“ gibt die Anzahl der Dokumente zurück, die mit den Suchkriterien übereinstimmen. Bei einer leeren Suchzeichenfolge ist dies die Anzahl aller Dokumente im Index (50 im Fall von „hotels-sample-index“).

## <a name="example-1-fielded-search"></a>Beispiel 1: Feldbezogene Suche

Die feldbezogene Suche bezieht sich auf einzelne, eingebettete Suchausdrücke für ein bestimmtes Feld. In diesem Beispiel wird nach Namen von Hotels gesucht, die den Begriff „Hotel“, aber nicht den Begriff „Motel“ enthalten. Sie können mehrere Felder mithilfe von „AND“ angeben. 

Bei Verwendung dieser Abfragesyntax können Sie den Parameter „searchFields“ auslassen, wenn sich die Felder, die Sie abfragen möchten, im Suchausdruck selbst befinden. Wenn Sie „searchFields“ in die feldbezogene Suche einschließen, hat `fieldName:searchExpression` grundsätzlich Vorrang vor „searchFields“.

```http
POST /indexes/hotel-samples-index/docs/search?api-version=2020-06-30
{
    "search": "HotelName:(hotel NOT motel) AND Category:'Resort and Spa'",
    "queryType": "full",
    "select": "HotelName, Category",
    "count": true
}
```

Die Antwort für diese Abfrage sollte in etwa dem folgenden Beispiel (gefiltert nach „Resort and Spa“) entsprechen und Hotels zurückgeben, deren Name den Begriff „Hotel“ oder „Motel“ enthält.

```json
"@odata.count": 4,
"value": [
    {
        "@search.score": 4.481559,
        "HotelName": "Nova Hotel & Spa",
        "Category": "Resort and Spa"
    },
    {
        "@search.score": 2.4524608,
        "HotelName": "King's Palace Hotel",
        "Category": "Resort and Spa"
    },
    {
        "@search.score": 2.3970203,
        "HotelName": "Triple Landscape Hotel",
        "Category": "Resort and Spa"
    },
    {
        "@search.score": 2.2953436,
        "HotelName": "Peaceful Market Hotel & Spa",
        "Category": "Resort and Spa"
    }
]
```

Bei dem Suchausdruck kann es sich um einen einzelnen Begriff, einen einfachen Ausdruck oder einen komplexeren Ausdruck in Klammern handeln, optional mit booleschen Operatoren. Einige Beispiele für Änderungen sind in der folgenden Liste aufgeführt:

+ `HotelName:(hotel NOT motel)`
+ `Address/StateProvince:("WA" OR "CA")`
+ `Tags:("free wifi" NOT "free parking") AND "coffee in lobby"`

Achten Sie darauf, dass Sie einen Ausdruck in Anführungszeichen setzen, wenn beide Zeichenfolgen als einzelne Entität ausgewertet werden sollen, wie in diesem Fall bei der Suche nach zwei verschiedenen Standorten im Feld „Address/StateProvince“. Abhängig vom Client müssen Sie die Anführungszeichen möglicherweise mit Escapezeichen (`\`) versehen.

Das in `fieldName:searchExpression` angegebene Feld muss durchsuchbar sein. Ausführliche Informationen zum Hinzufügen von Attributen zu Felddefinitionen finden Sie unter [Erstellen eines Index (REST-API)](/rest/api/searchservice/create-index).

## <a name="example-2-fuzzy-search"></a>Beispiel 2: Fuzzysuche

Bei der Fuzzysuche stimmen ähnliche Begriffe überein, einschließlich falsch geschriebener Wörter. Hängen Sie für eine Fuzzysuche das Tildesymbol `~` an das Ende eines einzelnen Worts mit einem optionalen Parameter an, einem Wert zwischen 0 und 2, der die Editierdistanz angibt. Beispielsweise würden bei `blue~` oder `blue~1` die Werte „blue“, „blues“ und „glue“ zurückgegeben.

```http
POST /indexes/hotel-samples-index/docs/search?api-version=2020-06-30
{
    "search": "Tags:conserge~",
    "queryType": "full",
    "select": "HotelName, Category, Tags",
    "searchFields": "HotelName, Category, Tags",
    "count": true
}
```

Die Antwort für diese Abfrage (der Übersicht halber gekürzt) wird in den übereinstimmenden Dokumenten in „concierge“ aufgelöst:

```json
"@odata.count": 12,
"value": [
    {
        "@search.score": 1.1832147,
        "HotelName": "Secret Point Motel",
        "Category": "Boutique",
        "Tags": [
            "pool",
            "air conditioning",
            "concierge"
        ]
    },
    {
        "@search.score": 1.1819803,
        "HotelName": "Twin Dome Motel",
        "Category": "Boutique",
        "Tags": [
            "pool",
            "free wifi",
            "concierge"
        ]
    },
    {
        "@search.score": 1.1773309,
        "HotelName": "Smile Hotel",
        "Category": "Suite",
        "Tags": [
            "view",
            "concierge",
            "laundry service"
        ]
    },
```

Ausdrücke werden nicht direkt unterstützt. Sie können jedoch eine Fuzzyübereinstimmung für jeden Begriff eines mehrteiligen Ausdrucks angeben, wie z. B. `search=Tags:landy~ AND sevic~`.  Dieser Abfrageausdruck findet 15 Übereinstimmungen für „laundry service“.

> [!Note]
> Fuzzyabfragen werden nicht [analysiert](search-lucene-query-architecture.md#stage-2-lexical-analysis). Abfragetypen mit unvollständigen Begriffen (Präfixabfrage, Platzerhalterabfrage, Abfrage regulärer Ausdrücke, Fuzzyabfrage) werden direkt unter Umgehung der Analysephase an die Abfragestruktur angehängt. Die einzige Transformation, die für Teilabfrageausdrücke durchgeführt wird, ist die Umwandlung in Kleinbuchstaben.
>

## <a name="example-3-proximity-search"></a>Beispiel 3: NEAR-Suche

Bei der NEAR-Suche wird nach Begriffen gesucht, die in einem Dokument nahe beieinander vorkommen. Fügen Sie ein Tildesymbol „~“ Symbol am Ende eines Ausdrucks ein, gefolgt von der Anzahl der Wörter, die den NEAR-Bereich bilden.

Diese Abfrage sucht nach den Begriffen „hotel“ und „airport“, wenn diese in einem Abstand von fünf Wörtern in einem Dokument vorkommen. Die Anführungszeichen werden mit Escapezeichen versehen (`\"`), um den Ausdruck beizubehalten:

```http
POST /indexes/hotel-samples-index/docs/search?api-version=2020-06-30
{
    "search": "Description: \"hotel airport\"~5",
    "queryType": "full",
    "select": "HotelName, Description",
    "searchFields": "HotelName, Description",
    "count": true
}
```

Die Antwort auf diese Abfrage sollte in etwa wie im folgenden Beispiel aussehen:

```json
"@odata.count": 2,
"value": [
    {
        "@search.score": 0.6331726,
        "HotelName": "Trails End Motel",
        "Description": "Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport."
    },
    {
        "@search.score": 0.43032226,
        "HotelName": "Catfish Creek Fishing Cabins",
        "Description": "Brand new mattresses and pillows.  Free airport shuttle. Great hotel for your business needs. Comp WIFI, atrium lounge & restaurant, 1 mile from light rail."
    }
]
```

## <a name="example-4-term-boosting"></a>Beispiel 4: Term Boosting

Die Begriffsverstärkung (Term Boosting) bezieht sich auf das Höherbewerten eines Dokuments, wenn es den verstärkten Begriff enthält, im Verhältnis zu Dokumenten, die den Begriff nicht enthalten. Verwenden Sie zum Verstärken eines Begriffs das Caretzeichen (`^`) mit einem Verstärkungsfaktor (einer Zahl) am Ende des Begriffs, nach dem Sie suchen. Der Standardwert für den Verstärkungsfaktor ist 1. Der Faktor muss zwar positiv sein, darf jedoch kleiner als 1 sein (z. B. 0,2). Das Term Boosting unterscheidet sich von Bewertungsprofilen darin, dass bei Bewertungsprofilen anstelle von bestimmten Begriffen bestimmte Felder verstärkt werden.

Suchen Sie in dieser „Vorher“-Abfrage nach „beach access“, und beachten Sie, dass es sieben Dokumente gibt, die mit einem oder beiden Begriffen übereinstimmen.

```http
POST /indexes/hotel-samples-index/docs/search?api-version=2020-06-30
{
    "search": "beach access",
    "queryType": "full",
    "select": "HotelName, Description, Tags",
    "searchFields": "HotelName, Description, Tags",
    "count": true
}
```

Tatsächlich gibt es nur ein Dokument, das mit „access“ übereinstimmt. Da dies die einzige Übereinstimmung ist, weist es eine hohe Platzierung auf (zweite Position), obwohl der Begriff „beach“ im Dokument fehlt.

```json
"@odata.count": 7,
"value": [
    {
        "@search.score": 2.2723424,
        "HotelName": "Nova Hotel & Spa",
        "Description": "1 Mile from the airport.  Free WiFi, Outdoor Pool, Complimentary Airport Shuttle, 6 miles from the beach & 10 miles from downtown."
    },
    {
        "@search.score": 1.5507699,
        "HotelName": "Old Carrabelle Hotel",
        "Description": "Spacious rooms, glamorous suites and residences, rooftop pool, walking access to shopping, dining, entertainment and the city center."
    },
    {
        "@search.score": 1.5358944,
        "HotelName": "Whitefish Lodge & Suites",
        "Description": "Located on in the heart of the forest. Enjoy Warm Weather, Beach Club Services, Natural Hot Springs, Airport Shuttle."
    },
    {
        "@search.score": 1.3433652,
        "HotelName": "Ocean Air Motel",
        "Description": "Oceanfront hotel overlooking the beach features rooms with a private balcony and 2 indoor and outdoor pools. Various shops and art entertainment are on the boardwalk, just steps away."
    },
```

Wiederholen Sie die Suche mit der „Nachher“-Abfrage, und verstärken Sie dieses Mal die Ergebnisse mit dem Begriff „beach“ gegenüber dem Begriff „access“. Eine für Menschen lesbare Version der Abfrage ist `search=Description:beach^2 access`. Abhängig vom Client müssen Sie `^2` möglicherweise als `%5E2` formulieren.

Nachdem Sie den Begriff „beach“ verstärkt haben, wird die Übereinstimmung für das Old Carrabelle Hotel auf den sechsten Platz nach unten verschoben.

<!-- Consider a scoring profile that boosts matches in a certain field, such as "genre" in a music app. Term boosting could be used to further boost certain search terms higher than others. For example, "rock^2 electronic" will boost documents that contain the search terms in the "genre" field higher than other searchable fields in the index. Furthermore, documents that contain the search term "rock" will be ranked higher than the other search term "electronic" as a result of the term boost value (2). -->

## <a name="example-5-regex"></a>Beispiel 5: Regulärer Ausdruck

Bei einer Suche mit regulärem Ausdruck werden Übereinstimmungen basierend auf dem Inhalt zwischen Schrägstrichen „/“ gefunden, wie in der [RegExp-Klasse](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/util/automaton/RegExp.html)dokumentiert.

```http
POST /indexes/hotel-samples-index/docs/search?api-version=2020-06-30
{
    "search": "HotelName:/(Mo|Ho)tel/",
    "queryType": "full",
    "select": "HotelName",
    "count": true
}
```

Die Antwort auf diese Abfrage sollte in etwa wie im folgenden Beispiel aussehen:

```json
    "@odata.count": 22,
    "value": [
        {
            "@search.score": 1.0,
            "HotelName": "Days Hotel"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Triple Landscape Hotel"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Smile Hotel"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Pelham Hotel"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Sublime Cliff Hotel"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Twin Dome Motel"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Nova Hotel & Spa"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Scarlet Harbor Hotel"
        },
```

> [!Note]
> Abfragen mit regulärem Ausdruck werden nicht [analysiert](./search-lucene-query-architecture.md#stage-2-lexical-analysis). Die einzige Transformation, die für Teilabfrageausdrücke durchgeführt wird, ist die Umwandlung in Kleinbuchstaben.
>

## <a name="example-6-wildcard-search"></a>Beispiel 6: Platzhaltersuche

Sie können die allgemein bekannte Syntax für die Platzhaltersuche nach mehreren (`*`) oder einzelnen (`?`) Zeichen verwenden. Beachten Sie, dass der Lucene-Abfrageparser die Verwendung dieser Symbole bei einem einzelnen Begriff, nicht bei einem Ausdruck, unterstützt.

In dieser Abfrage suchen Sie nach den Namen von Hotels, die das Präfix „sc“ enthalten. Das Symbol `*` oder `?` kann nicht als erstes Zeichen in einer Suche verwendet werden.

```http
POST /indexes/hotel-samples-index/docs/search?api-version=2020-06-30
{
    "search": "HotelName:sc*",
    "queryType": "full",
    "select": "HotelName",
    "count": true
}
```

Die Antwort auf diese Abfrage sollte in etwa wie im folgenden Beispiel aussehen:

```json
    "@odata.count": 2,
    "value": [
        {
            "@search.score": 1.0,
            "HotelName": "Scarlet Harbor Hotel"
        },
        {
            "@search.score": 1.0,
            "HotelName": "Scottish Inn"
        }
    ]
```

> [!Note]
> Abfragen mit Platzhaltern werden nicht [analysiert](./search-lucene-query-architecture.md#stage-2-lexical-analysis). Die einzige Transformation, die für Teilabfrageausdrücke durchgeführt wird, ist die Umwandlung in Kleinbuchstaben.
>

## <a name="next-steps"></a>Nächste Schritte

Versuchen Sie, Abfragen im Code anzugeben. Unter den folgenden Links wird erläutert, wie Suchabfragen mithilfe der Azure SDKs eingerichtet werden.

+ [Abfragen des Index mit dem .NET SDK](search-get-started-dotnet.md)
+ [Abfragen des Index mit dem Python SDK](search-get-started-python.md)
+ [Abfragen des Index mit dem JavaScript SDK](search-get-started-javascript.md)

Eine zusätzliche Syntaxreferenz, eine Abfragearchitektur und Beispiele finden Sie unter den folgenden Links:

+ [Beispiele für die Lucene-Abfragesyntax zum Erstellen von Abfragen in Azure Search](search-query-lucene-examples.md)
+ [Funktionsweise der Volltextsuche in Azure Cognitive Search](search-lucene-query-architecture.md)
+ [Einfache Abfragesyntax](query-simple-syntax.md)
+ [Vollständige Lucene-Abfragesyntax](query-lucene-syntax.md)
+ [Filtersyntax](search-query-odata-filter.md)