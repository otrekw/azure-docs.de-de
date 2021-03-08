---
title: Verwenden der einfachen Lucene-Abfragesyntax
titleSuffix: Azure Cognitive Search
description: Diese Abfragebeispiele zeigen die einfache Syntax für Volltextsuche, Filtersuche und geografische Suche für einen Azure Cognitive Search-Index.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/03/2021
ms.openlocfilehash: 2abe19351c92bf9cea85c85dd55f47b5ee6d1625
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101694035"
---
# <a name="use-the-simple-search-syntax-in-azure-cognitive-search"></a>Verwenden der „einfachen“ Suchsyntax in Azure Cognitive Search

In Azure Cognitive Search wird bei der [einfachen Abfragesyntax](query-simple-syntax.md) der Standardabfrageparser für die Ausführung von Volltextsuchabfragen aufgerufen. Der Parser ist schnell und eignet sich für gängige Szenarien, einschließlich Volltextsuche, Filter- und Facettensuche und Präfixsuche. In diesem Artikel wird die Verwendung der einfachen Syntax in einer Anforderung zum [Durchsuchen von Dokumenten (REST-API)](/rest/api/searchservice/search-documents) anhand von Beispielen veranschaulicht.

> [!NOTE]
> Eine alternative Abfragesyntax ist die [vollständige Lucene-Abfragesyntax](query-lucene-syntax.md), die komplexere Abfragestrukturen wie z. B. die Fuzzysuche und die Platzhaltersuche unterstützt. Weitere Informationen und Beispiele finden Sie unter [Verwenden der vollständigen Lucene-Syntax](search-query-lucene-examples.md).

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
    "queryType": "simple",
    "select": "HotelId, HotelName, Category, Tags, Description",
    "count": true
}
```

+ Wenn „search“ auf `*` festgelegt ist, erfolgt die Abfrage ohne Angaben und entspricht einer NULL-Suche oder leeren Suche. Dies ist nicht besonders nützlich, aber es ist die einfachste Suche, die Sie ausführen können, und es werden alle abrufbaren Felder im Index mit allen Werten angezeigt.

+ „queryType“ ist standardmäßig auf „simple“ festgelegt und kann ausgelassen werden. Hier ist dieser Ausdruck jedoch enthalten, um zu verdeutlichen, dass die Abfragebeispiele in diesem Artikel in einfacher Syntax ausgedrückt werden.

+ „select“ ist auf eine durch Trennzeichen getrennte Liste von Feldern festgelegt und wird zur Zusammenstellung von Suchergebnissen verwendet. Es werden nur die Felder einbezogen, die im Kontext von Suchergebnissen nützlich sind.

+ „count“ gibt die Anzahl der Dokumente zurück, die mit den Suchkriterien übereinstimmen. Bei einer leeren Suchzeichenfolge ist dies die Anzahl aller Dokumente im Index (50 im Fall von „hotels-sample-index“).

## <a name="example-1-full-text-search"></a>Beispiel 1: Volltextsuche

Die Volltextsuche kann eine beliebige Anzahl von eigenständigen Begriffen oder in Anführungszeichen eingeschlossenen Ausdrücken mit oder ohne boolesche Operatoren sein. 

```http
POST /indexes/hotel-samples-index/docs/search?api-version=2020-06-30
{
    "search": "pool spa +airport",
    "searchMode": any,
    "queryType": "simple",
    "select": "HotelId, HotelName, Category, Description",
    "count": true
}
```

Eine Schlüsselwortsuche, die aus wichtigen Begriffen oder Ausdrücken besteht, funktioniert tendenziell am besten. Während der Indizierung und der Abfrage werden Zeichenfolgenfelder einer Textanalyse unterzogen, wobei unwesentliche Wörter wie Artikel (der/die/das) oder Pronomen (er/sie/es) gelöscht werden. Wenn Sie sehen möchten, wie eine Abfragezeichenfolge im Index mit Token versehen wird, übergeben Sie die Zeichenfolge in einem [Textanalyse](/rest/api/searchservice/test-analyzer)-Aufruf an den Index.

Der Parameter „searchMode“ steuert die Genauigkeit und den Abruf. Wenn Sie mehr Abrufe benötigen, verwenden Sie den Standardwert „any“. Dieser gibt ein Ergebnis zurück, wenn ein beliebiger Teil der Abfragezeichenfolge übereinstimmt. Wenn Sie ein genaues Ergebnis bevorzugen, bei dem alle Teile der Zeichenfolge übereinstimmen müssen, ändern Sie „searchMode“ in „all“. Testen Sie die obige Abfrage mit beiden Optionen, um zu sehen, wie „searchMode“ das Ergebnis beeinflusst.

Das Ergebnis der Abfrage „pool spa +airport“ sollte in etwa wie im folgenden (gekürzten) Beispiel aussehen.

```json
"@odata.count": 6,
"value": [
    {
        "@search.score": 7.3617697,
        "HotelId": "21",
        "HotelName": "Nova Hotel & Spa",
        "Description": "1 Mile from the airport.  Free WiFi, Outdoor Pool, Complimentary Airport Shuttle, 6 miles from the beach & 10 miles from downtown.",
        "Category": "Resort and Spa",
        "Tags": [
            "pool",
            "continental breakfast",
            "free parking"
        ]
    },
    {
        "@search.score": 2.5560288,
        "HotelId": "25",
        "HotelName": "Scottish Inn",
        "Description": "Newly Redesigned Rooms & airport shuttle.  Minutes from the airport, enjoy lakeside amenities, a resort-style pool & stylish new guestrooms with Internet TVs.",
        "Category": "Luxury",
        "Tags": [
            "24-hour front desk service",
            "continental breakfast",
            "free wifi"
        ]
    },
    {
        "@search.score": 2.2988036,
        "HotelId": "35",
        "HotelName": "Suites At Bellevue Square",
        "Description": "Luxury at the mall.  Located across the street from the Light Rail to downtown.  Free shuttle to the mall and airport.",
        "Category": "Resort and Spa",
        "Tags": [
            "continental breakfast",
            "air conditioning",
            "24-hour front desk service"
        ]
    }
```

Beachten Sie die Suchbewertung („search.score“) im Ergebnis. Dies ist die Relevanzbewertung der Übereinstimmung. Standardmäßig gibt ein Suchdienst basierend auf dieser Bewertung die ersten 50 Übereinstimmungen zurück.

Zu einer einheitlichen Bewertung von „1.0“ kommt es, wenn kein Rang vorliegt, weil es entweder keine Volltextsuche war oder weil keine Kriterien angegeben wurden. Beispielsweise werden bei einer leeren Suche (search=`*`) Zeilen in beliebiger Reihenfolge zurückgegeben. Wenn Sie tatsächliche Kriterien einfügen, werden aussagekräftige Werte für die Suchbewertungen angezeigt.

## <a name="example-2-look-up-by-id"></a>Beispiel 2: Suchen anhand der ID

Wenn Sie Suchergebnisse in einer Abfrage zurückgeben, besteht ein logischer nachfolgender Schritt darin, eine Detailseite bereitzustellen, die weitere Felder aus dem Dokument enthält. In diesem Beispiel wird veranschaulicht, wie Sie ein einzelnes Dokument mithilfe eines [Suchdokuments](/rest/api/searchservice/lookup-document) zurückgeben, indem Sie die Dokument-ID übergeben.

```http
GET /indexes/hotels-sample-index/docs/41?api-version=2020-06-30
```

Alle Dokumente verfügen über einen eindeutigen Bezeichner. Wenn Sie das Portal verwenden, wählen Sie den Index auf der Registerkarte **Indizes** aus, und stellen Sie dann anhand der Felddefinitionen fest, welches Feld als Schlüssel dient. Bei Verwendung von REST gibt der Aufruf [Get Index](/rest/api/searchservice/get-index) die Indexdefinition im Antworttext zurück.

Die Antwort für die obige Abfrage besteht aus dem Dokument mit dem Schlüssel 41. Alle Felder, die in der Indexdefinition als abrufbar gekennzeichnet sind, können in den Suchergebnissen zurückgegeben und in Ihrer App gerendert werden.

```json
{
    "HotelId": "41",
    "HotelName": "Ocean Air Motel",
    "Description": "Oceanfront hotel overlooking the beach features rooms with a private balcony and 2 indoor and outdoor pools. Various shops and art entertainment are on the boardwalk, just steps away.",
    "Description_fr": "L'hôtel front de mer surplombant la plage dispose de chambres avec balcon privé et 2 piscines intérieures et extérieures. Divers commerces et animations artistiques sont sur la promenade, à quelques pas.",
    "Category": "Budget",
    "Tags": [
        "pool",
        "air conditioning",
        "bar"
    ],
    "ParkingIncluded": true,
    "LastRenovationDate": "1951-05-10T00:00:00Z",
    "Rating": 3.5,
    "Location": {
        "type": "Point",
        "coordinates": [
            -157.846817,
            21.295841
        ],
        "crs": {
            "type": "name",
            "properties": {
                "name": "EPSG:4326"
            }
        }
    },
    "Address": {
        "StreetAddress": "1450 Ala Moana Blvd 2238 Ala Moana Ctr",
        "City": "Honolulu",
        "StateProvince": "HI",
        "PostalCode": "96814",
        "Country": "USA"
    },
```

## <a name="example-3-filter-on-text"></a>Beispiel 3: Filtern nach Text

Die [Filtersyntax](search-query-odata-filter.md) ist ein OData-Ausdruck, den Sie allein oder mit „search“ verwenden können. Wenn sie zusammen verwendet werden, wird „filter“ zuerst auf den gesamten Index angewendet, und anschließend wird die Suche für die Ergebnisse des Filters ausgeführt. Filter können daher eine nützliche Methode zum Verbessern der Abfrageleistung darstellen, da sie die Menge der Dokumente reduzieren, die bei der Suchabfrage verarbeitet werden müssen.

Filter können für alle Felder definiert werden, die in der Indexdefinition als filterbar gekennzeichnet sind. Für „hotels-sample-index“ gehören „Category“, „Tags“, „ParkingIncluded“, „Rating“ und die meisten Adressfelder zu den filterbaren Feldern.

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "search": "art tours",
    "queryType": "simple",
    "filter": "Category eq 'Resort and Spa'",
    "select": "HotelId,HotelName,Description,Category",
    "count": true
}
```

Die Antwort für die obige Abfrage ist auf die Hotels beschränkt, die sich in der Kategorie „Resort and Spa“ befinden und die Begriffe „art“ oder „tours“ enthalten. In diesem Fall gibt es nur eine Übereinstimmung.

```json
{
    "@search.score": 2.8576312,
    "HotelId": "31",
    "HotelName": "Santa Fe Stay",
    "Description": "Nestled on six beautifully landscaped acres, located 2 blocks from the Plaza. Unwind at the spa and indulge in art tours on site.",
    "Category": "Resort and Spa"
}
```

## <a name="example-4-filter-functions"></a>Beispiel 4: Filterfunktionen

Filterausdrücke können die [Funktionen „search.isMatch“ und „search.ismatchscoring“](search-query-odata-full-text-search-functions.md) enthalten, sodass Sie eine Suchabfrage innerhalb des Filters erstellen können. Dieser Filterausdruck verwendet einen Platzhalter für *free*, um die enthaltenen Zusatzleistungen wie z. B. kostenloses WLAN oder kostenloses Parken auszuwählen.

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
  {
    "search": "",
    "filter": "search.ismatch('free*', 'Tags', 'full', 'any')",
    "select": "HotelId, HotelName, Category, Description",
    "count": true
  }
```

Die Antwort auf die obigen Abfrage enthält Übereinstimmungen mit 19 Hotels, die kostenlose Zusatzleistungen anbieten. Beachten Sie, dass die Suchbewertung in den gesamten Ergebnissen einheitlich „1.0“ lautet. Dies liegt daran, dass der Suchausdruck NULL oder leer ist. So erhalten Sie wörtliche Filterübereinstimmungen, es wird jedoch keine Volltextsuche durchgeführt. Relevanzbewertungen werden nur bei einer Volltextsuche zurückgegeben. Wenn Sie Filter ohne „search“ verwenden, stellen Sie sicher, dass genügend sortierbare Felder vorliegen, damit Sie den Suchrang steuern können.

```json
"@odata.count": 19,
"value": [
    {
        "@search.score": 1.0,
        "HotelId": "31",
        "HotelName": "Santa Fe Stay",
        "Tags": [
            "view",
            "restaurant",
            "free parking"
        ]
    },
    {
        "@search.score": 1.0,
        "HotelId": "27",
        "HotelName": "Super Deluxe Inn & Suites",
        "Tags": [
            "bar",
            "free wifi"
        ]
    },
    {
        "@search.score": 1.0,
        "HotelId": "39",
        "HotelName": "Whitefish Lodge & Suites",
        "Tags": [
            "continental breakfast",
            "free parking",
            "free wifi"
        ]
    },
    {
        "@search.score": 1.0,
        "HotelId": "11",
        "HotelName": "Regal Orb Resort & Spa",
        "Tags": [
            "free wifi",
            "restaurant",
            "24-hour front desk service"
        ]
    },
```

## <a name="example-5-range-filters"></a>Beispiel 5: Bereichsfilter

Die Bereichsfilterung wird durch Filterausdrücke für jeden Datentyp unterstützt. In den folgenden Beispielen werden numerische Bereiche und Zeichenfolgenbereiche veranschaulicht. Datentypen sind bei Bereichsfiltern wichtig und funktionieren am besten bei numerischen Daten in numerischen Feldern und Zeichenfolgendaten in Zeichenfolgenfeldern. Numerische Daten in Zeichenfolgenfeldern eignen sich nicht für Bereiche, weil sich numerische Zeichenfolgen nicht vergleichen lassen.

In der folgenden Abfrage wird ein numerischer Bereich verwendet. In „hotels-sample-index“ ist „Rating“ das einzige filterbare numerische Feld.

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "search": "*",
    "filter": "Rating ge 2 and Rating lt 4",
    "select": "HotelId, HotelName, Rating",
    "orderby": "Rating desc",
    "count": true
}
```

Das Ergebnis dieser Abfrage sollte in etwa wie im folgenden (gekürzten) Beispiel aussehen.

```json
"@odata.count": 27,
"value": [
    {
        "@search.score": 1.0,
        "HotelId": "22",
        "HotelName": "Stone Lion Inn",
        "Rating": 3.9
    },
    {
        "@search.score": 1.0,
        "HotelId": "25",
        "HotelName": "Scottish Inn",
        "Rating": 3.8
    },
    {
        "@search.score": 1.0,
        "HotelId": "2",
        "HotelName": "Twin Dome Motel",
        "Rating": 3.6
    }
```

Bei der nächsten Abfrage handelt es sich um einen Bereichsfilter für ein Zeichenfolgenfeld (Address/StateProvince):

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "search": "*",
    "filter": "Address/StateProvince ge 'A*' and Address/StateProvince lt 'D*'",
    "select": "HotelId, HotelName, Address/StateProvince",
    "count": true
}
```

Das Ergebnis dieser Abfrage sollte in etwa wie im folgenden (gekürzten) Beispiel aussehen. In diesem Beispiel ist es nicht möglich, nach „StateProvince“ zu sortieren, weil das Feld in der Indexdefinition nicht mit dem Attribut „sortable“ (sortierbar) versehen ist.

```json
"@odata.count": 9,
"value": [
    {
        "@search.score": 1.0,
        "HotelId": "9",
        "HotelName": "Smile Hotel",
        "Address": {
            "StateProvince": "CA "
        }
    },
    {
        "@search.score": 1.0,
        "HotelId": "39",
        "HotelName": "Whitefish Lodge & Suites",
        "Address": {
            "StateProvince": "CO"
        }
    },
    {
        "@search.score": 1.0,
        "HotelId": "7",
        "HotelName": "Countryside Resort",
        "Address": {
            "StateProvince": "CA "
        }
    },
```

## <a name="example-6-geo-search"></a>Beispiel 6: Geografische Suche

„hotels-sample-index“ enthält ein Feld „geo_location“ mit Breiten- und Längengradkoordinaten. In diesem Beispiel wird die [geo.distance-Funktion](search-query-odata-geo-spatial-functions.md#examples) verwendet, mit der Dokumente im Umkreis eines Startpunkts bis zu einer beliebigen festgelegten Entfernung (in Kilometer) gefiltert werden. Sie können den letzten Wert in der Abfrage (10) anpassen, um die Fläche der Abfrage zu verkleinern oder zu vergrößern.

```http
POST /indexes/v/docs/search?api-version=2020-06-30
{
    "search": "*",
    "filter": "geo.distance(Location, geography'POINT(-122.335114 47.612839)') le 10",
    "select": "HotelId, HotelName, Address/City, Address/StateProvince",
    "count": true
}
```

Im Ergebnis dieser Abfrage werden alle Hotels zurückgegeben, die innerhalb eines 10-Kilometer-Radius von den angegebenen Koordinaten liegen:

```json
{
    "@odata.count": 3,
    "value": [
        {
            "@search.score": 1.0,
            "HotelId": "45",
            "HotelName": "Arcadia Resort & Restaurant",
            "Address": {
                "City": "Seattle",
                "StateProvince": "WA"
            }
        },
        {
            "@search.score": 1.0,
            "HotelId": "24",
            "HotelName": "Gacc Capital",
            "Address": {
                "City": "Seattle",
                "StateProvince": "WA"
            }
        },
        {
            "@search.score": 1.0,
            "HotelId": "16",
            "HotelName": "Double Sanctuary Resort",
            "Address": {
                "City": "Seattle",
                "StateProvince": "WA"
            }
        }
    ]
}
```

## <a name="example-7-booleans-with-searchmode"></a>Beispiel 7: Boolesche Werte mit „searchMode“

Die einfache Syntax unterstützt boolesche Operatoren in Form von Zeichen (`+, -, |`) zur Unterstützung der Abfragelogik UND, ODER und NICHT. Die boolesche Suche verhält sich erwartungsgemäß, mit einigen bemerkenswerten Ausnahmen. 

In den vorherigen Beispielen wurde der Parameter „searchMode“ als Mechanismus zum Beeinflussen der Genauigkeit und des Abrufs eingeführt, wobei „searchMode=any“ den Schwerpunkt auf den Abruf (ein Dokument, das beliebige Kriterien erfüllt, wird als Übereinstimmung betrachtet) und „searchMode=all“ den Schwerpunkt auf die Genauigkeit legt (in einem Dokument müssen alle Kriterien übereinstimmen). 

Im Kontext einer booleschen Suche kann der Standardwert „searchMode=any“ zu Verwirrung führen, wenn Sie eine Abfrage mit mehreren Operatoren stapeln und statt einer kleineren eine größere Ergebnismenge erzielen. Dies gilt besonders für den Operator NICHT, bei dem die Ergebnisse alle Dokumente umfassen, die einen bestimmten Begriff nicht enthalten.

Dies wird im folgenden Beispiel veranschaulicht. Bei Ausführung der folgenden Abfrage mit searchMode „any“ werden 42 Dokumente zurückgegeben: diejenigen, die den Begriff „restaurant“ enthalten, sowie alle Dokumente, die nicht den Ausdruck „air conditioning“ aufweisen. 

Beachten Sie, dass zwischen dem booleschen Operator (`-`) und dem Ausdruck „air conditioning“ kein Leerzeichen vorhanden ist.

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "search": "restaurant -\"air conditioning\"",
    "searchMode": "any",
    "searchFields": "Tags",
    "select": "HotelId, HotelName, Tags",
    "count": true
}
```

Wenn Sie die Abfrage in „searchMode=all“ ändern, wird eine kumulative Wirkung für die Kriterien erzwungen, und es wird ein kleineres Resultset (7 Übereinstimmungen) zurückgegeben. Dieses umfasst Dokumente, die den Begriff „Restaurant“ enthalten, abzüglich der Dokumente mit dem Ausdruck „air conditioning“.

Das Ergebnis dieser Abfrage sollte in etwa wie im folgenden (gekürzten) Beispiel aussehen.

```json
"@odata.count": 7,
"value": [
    {
        "@search.score": 2.5460577,
        "HotelId": "11",
        "HotelName": "Regal Orb Resort & Spa",
        "Tags": [
            "free wifi",
            "restaurant",
            "24-hour front desk service"
        ]
    },
    {
        "@search.score": 2.166792,
        "HotelId": "10",
        "HotelName": "Countryside Hotel",
        "Tags": [
            "24-hour front desk service",
            "coffee in lobby",
            "restaurant"
        ]
    },
```

## <a name="example-8-paging-results"></a>Beispiel 8: Paginieren von Ergebnissen

In den vorherigen Beispielen haben Sie Parameter kennengelernt, die sich auf die Zusammenstellung der Suchergebnisse auswirken, darunter „select“ zum Festlegen der in einem Ergebnis enthaltenen Felder, Sortierreihenfolgen und das Einbeziehen der Anzahl aller Übereinstimmungen. Bei diesem Beispiel handelt es sich um eine Fortsetzung der Zusammenstellung von Suchergebnissen in Form von Paginierungsparametern, mit denen Sie die Anzahl der Ergebnisse, die auf einer beliebigen Seite angezeigt werden, in Batches zusammenfassen können. 

Standardmäßig gibt ein Suchdienst die ersten 50 Übereinstimmungen zurück. Um die Anzahl der Übereinstimmungen auf jeder Seite zu steuern, definieren Sie die Größe des Batches mit „top“, und verwenden Sie dann „skip“, um nachfolgende Batches auszuwählen.

Im folgenden Beispiel werden ein Filter und eine Sortierreihenfolge für das Feld „Rating“ verwendet („Rating“ ist sowohl filter- als auch sortierbar), weil die Auswirkungen der Paginierung auf sortierte Ergebnisse leichter erkennbar sind. In einer regulären Volltextsuchabfrage werden die ersten Übereinstimmungen über @search.score mit einem Rang versehen und paginiert.

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "search": "*",
    "filter": "Rating gt 4",
    "select": "HotelName, Rating",
    "orderby": "Rating desc",
    "top": "5",
    "count": true
}
```

Die Abfrage findet 21 übereinstimmende Dokumente. Da Sie jedoch „top“ angegeben haben, gibt die Antwort nur die ersten fünf Übereinstimmungen zurück, deren Bewertungen bei 4.9 beginnen und mit „Lady of the Lake B & B“ bei 4.7 enden. 

Überspringen Sie den ersten Batch, um die nächsten fünf Dokumente zu erhalten:

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "search": "*",
    "filter": "Rating gt 4",
    "select": "HotelName, Rating",
    "orderby": "Rating desc",
    "top": "5",
    "skip": "5",
    "count": true
}
```

In der Antwort für den zweiten Batch werden die ersten fünf Übereinstimmungen übersprungen und die nächsten fünf zurückgegeben, beginnend bei „Pull‘r Inn Motel“. Um mit weiteren Batches fortzufahren, behalten Sie für „top“ den Wert 5 bei und erhöhen dann bei jeder neuen Anforderung den Wert für „Skip“ um 5 (skip=5, skip=10, skip=15 usw.).

```json
"value": [
    {
        "@search.score": 1.0,
        "HotelName": "Pull'r Inn Motel",
        "Rating": 4.7
    },
    {
        "@search.score": 1.0,
        "HotelName": "Sublime Cliff Hotel",
        "Rating": 4.6
    },
    {
        "@search.score": 1.0,
        "HotelName": "Antiquity Hotel",
        "Rating": 4.5
    },
    {
        "@search.score": 1.0,
        "HotelName": "Nordick's Motel",
        "Rating": 4.5
    },
    {
        "@search.score": 1.0,
        "HotelName": "Winter Panorama Resort",
        "Rating": 4.5
    }
]
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie hier die grundlegende Abfragesyntax kennengelernt haben, versuchen Sie, Abfragen im Code anzugeben. Unter den folgenden Links wird erläutert, wie Suchabfragen mithilfe der Azure SDKs eingerichtet werden.

+ [Abfragen des Index mit dem .NET SDK](search-get-started-dotnet.md)
+ [Abfragen des Index mit dem Python SDK](search-get-started-python.md)
+ [Abfragen des Index mit dem JavaScript SDK](search-get-started-javascript.md)

Eine zusätzliche Syntaxreferenz, eine Abfragearchitektur und Beispiele finden Sie unter den folgenden Links:

+ [Beispiele für die Lucene-Abfragesyntax zum Erstellen von Abfragen in Azure Search](search-query-lucene-examples.md)
+ [Funktionsweise der Volltextsuche in Azure Cognitive Search](search-lucene-query-architecture.md)
+ [Einfache Abfragesyntax](query-simple-syntax.md)
+ [Vollständige Lucene-Abfragesyntax](query-lucene-syntax.md)
+ [Filtersyntax](search-query-odata-filter.md)