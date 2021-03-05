---
title: Erstellen einer Semantikabfrage
titleSuffix: Azure Cognitive Search
description: Legen Sie eine Abfrage vom Typ „semantic“ (semantisch) fest, um die Deep Learning-Modelle mit der Abfrageverarbeitung zu verknüpfen und so die Absicht und den Kontext als Teil des Suchrangs und der Relevanz abzuleiten.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.openlocfilehash: 7551ef88c2251b64cf6f6db1de4fed22db2c69e2
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101693644"
---
# <a name="create-a-semantic-query-in-cognitive-search"></a>Erstellen einer Semantikabfrage in Azure Cognitive Search

> [!IMPORTANT]
> Der Abfragetyp „semantic“ (semantisch) befindet sich in der öffentlichen Vorschau und ist über die Vorschau-REST-API und das Azure-Portal verfügbar. Vorschaufeatures werden wie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) beschrieben im Ist-Zustand angeboten. Während des anfänglichen Vorschaustarts ist die semantische Suche kostenlos. Weitere Informationen finden Sie unter [Verfügbarkeit und Preise](semantic-search-overview.md#availability-and-pricing).

In diesem Artikel erfahren Sie, wie sie eine Suchanforderung formulieren, die eine semantische Rangfolge verwendet und semantische Beschriftungen sowie Antworten erzeugt.

## <a name="prerequisites"></a>Voraussetzungen

+ Ein Suchdienst in einem Standard-Tarif (S1, S2, S3) in einer der folgenden Regionen: „USA, Norden-Mitte“, „USA, Westen“, „USA, Westen 2“, „USA, Osten 2“, „Europa, Norden“, „Europa, Westen“. Wenn Sie in einer dieser Regionen über einen Dienst mit der Ebene S1 oder höher verfügen, können Sie Zugriff anfordern, ohne einen neuen Dienst erstellen zu müssen.

+ Zugriff auf die Vorschau der semantischen Suche: [Registrieren](https://aka.ms/SemanticSearchPreviewSignup)

+ Ein vorhandener Suchindex mit englischem Inhalt

+ Ein Suchclient zum Senden von Abfragen

  Der Suchclient muss in der Abfrageanforderung Vorschau-REST-APIs unterstützen. Sie können [Postman](search-get-started-rest.md), [Visual Studio Code](search-get-started-vs-code.md) oder Code verwenden, den Sie geändert haben, um REST-Aufrufe an die Vorschau-APIs zu senden. Sie können auch den [Suchexplorer](search-explorer.md) im Azure-Portal verwenden, um eine Semantikabfrage zu übermitteln.

+ Dies ist eine [Suchdokumentanforderung](/rest/api/searchservice/preview-api/search-documents) mit der semantischen Option und weiteren in diesem Artikel beschriebenen Parametern

## <a name="whats-a-semantic-query"></a>Was ist eine Semantikabfrage?

In Azure Cognitive Search handelt es sich bei einer Abfrage um eine parametrisierte Anforderung, mit der die Abfrageverarbeitung und die Form der Antwort bestimmt werden. Eine *Semantikabfrage* fügt Parameter hinzu, die den semantischen Algorithmus zur Neuanordnung aufrufen. Mit diesem können der Kontext sowie die Bedeutung übereinstimmender Ergebnisse bewertet und relevantere Übereinstimmungen in der Rangfolge nach oben verschoben werden.

Die folgende Anforderung ist charakteristisch für eine einfache Semantikabfrage (ohne Antwort).

```http
POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=2020-06-30-Preview      
{    
    "search": " Where was Alan Turing born?",    
    "queryType": "semantic",  
    "searchFields": "title,url,body",  
    "queryLanguage": "en-us",  
}
```

Wie bei allen Abfragen in Azure Cognitive Search zielt die Anforderung auf die Dokumentensammlung eines einzelnen Indexes ab. Außerdem wird bei einer Semantikabfrage dieselbe Sequenz von Parsen, Analysieren und Überprüfen durchlaufen wie bei einer nicht semantischen Abfrage. Der Unterschied liegt in der Berechnung der Relevanz. Wie in diesem Vorschaurelease definiert, ist eine Semantikabfrage eine Abfrage, deren *Ergebnisse* mithilfe erweiterter Algorithmen erneut verarbeitet werden. Dadurch wird eine Möglichkeit bereitgestellt, die Übereinstimmungen, die von dem Semantikrangfolge-Modul als am relevantesten eingestuft werden, als erstes anzuzeigen. 

Nur die 50 besten Treffer aus den anfänglichen Ergebnissen können in eine semantische Rangfolge gebracht werden, und alle enthalten Beschriftungen in der Antwort. Optional können Sie bei der Anforderung einen **`answer`** -Parameter angeben, um eine mögliche Antwort zu extrahieren. Dieses Modell formuliert bis zu fünf mögliche Antworten auf die Abfrage, die Sie oben auf der Suchseite anzeigen lassen können.

## <a name="query-using-rest-apis"></a>Abfragen über REST-APIs

Die vollständige Spezifikation der REST-API finden Sie unter [Dokumente durchsuchen (REST-Vorschau)](/rest/api/searchservice/preview-api/search-documents).

Semantikabfragen sind für offene Fragen gedacht, wie z. B. „Welche Pflanze ist am besten als Bestäuber geeignet?“ oder „Wie brät man Eier?“ Wenn das Ergebnis Antworten enthalten soll, können Sie für die Anforderung einen optionalen **`answer`** -Parameter hinzufügen.

Im folgenden Beispiel wird „hotels-sample-index“ verwendet, um eine semantische Abfrageanforderung mit semantischen Antworten und Beschriftungen zu erstellen:

```http
POST https://[service name].search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview      
{
    "search": "newer hotel near the water with a great restaurant",
    "queryType": "semantic",
    "queryLanguage": "en-us",
    "searchFields": "HotelName,Category,Description",
    "speller": "lexicon",
    "answers": "extractive|count-3",
    "highlightPreTag": "<strong>",
    "highlightPostTag": "</strong>",
    "select": "HotelId,HotelName,Description,Category",
    "count": true
}
```

### <a name="formulate-the-request"></a>Formulieren der Anforderung

1. Legen Sie **`"queryType"`** auf „semantic“ und **`"queryLanguage"`** auf „en-us“ fest. Es müssen beide Parameter angegeben werden.

   Der queryLanguage-Parameter muss mit den [Sprachanalysetools](index-add-language-analyzers.md) übereinstimmen, die den Felddefinitionen im Indexschema zugewiesen sind. Wenn „queryLanguage=en-us“ ist, dann müssen alle Sprachananalysetools auch eine englische Variante sein („en.microsoft“ oder „en.lucene“). Alle sprachunabhängigen Analysetools, wie z. B. „keyword“ oder „simple“, verursachen keinen Konflikt mit den queryLanguage-Werten.

   Wenn Sie in einer Abfrageanforderung auch die [Rechtschreibkorrektur](speller-how-to-add.md) verwenden, gilt die festgelegte „queryLanguage“ gleichermaßen für Rechtschreibprüfung, Antworten und Beschriftungen. Es gibt keine Außerkraftsetzung einzelner Teile. 

   Inhalt in einem Suchindex kann zwar aus mehreren Sprachen zusammengesetzt sein, die Abfrageeingabe erfolgt jedoch höchstwahrscheinlich nur in einer Sprache. Die Suchmaschine prüft nicht die Kompatibilität von „queryLanguage“, dem Sprachanalysetool und der Sprache, in der der Inhalt verfasst ist. Stellen Sie daher sicher, dass Sie Abfragen entsprechend einschränken, um falsche Ergebnisse zu vermeiden.

<a name="searchfields"></a>

1. Legen Sie **`"searchFields"`** fest (optional, aber empfohlen).

   In einer Semantikabfrage spiegelt die Reihenfolge der Felder in „searchFields“ die Priorität oder relative Relevanz der Felder in semantischen Rangfolgen wider. Nur Zeichenfolgenfelder der obersten Ebene (eigenständig oder in einer Sammlung) werden verwendet. Da sich „searchFields“ in Lucene-Abfragen vom Typ „simple“ (einfach) oder „full“ (vollständig) unterschiedlich verhält (es gibt keine implizite Prioritätsreihenfolge), geben beliebige weitere Felder und Unterfelder, die keine Zeichenfolgen sind, zwar keinen Fehler aus, sie werden jedoch auch nicht in der semantischen Reihenfolge verwendet.

   Beachten Sie bei der Angabe von „searchFields“ die folgenden Vorgaben:

   + Präzise Felder, wie „Hotelname“ oder ein Titel, sollten Feldern vom Typ „Ausführlich“ (z. B. „Beschreibung“) vorangestellt werden.

   + Falls Ihr Index ein URL-Feld besitzt, das Textinhalt aufweist, also für Benutzer lesbar (wie `www.domain.com/name-of-the-document-and-other-details`) und nicht computerbezogen (wie `www.domain.com/?id=23463&param=eis`) ist, legen Sie es in der Liste an zweiter Stelle fest. Wenn kein prägnantes Titelfeld vorhanden ist, legen Sie es auf den ersten Platz fest.

   + Wenn nur ein Feld angegeben ist, wird es als beschreibendes Feld für die semantische Rangfolge von Dokumenten betrachtet.  

   + Sind keine Felder angegeben, werden alle durchsuchbaren Felder für die semantische Rangfolge von Dokumenten berücksichtigt. Dies wird jedoch nicht empfohlen, da es möglicherweise nicht die optimalsten Ergebnisse aus Ihrem Suchindex liefert.

1. Entfernen Sie **`"orderBy"`** -Klauseln, wenn sie in einer vorhandenen Anforderung existieren. Die semantische Bewertung dient zum Sortieren der Ergebnisse, und wenn Sie eine explizite Sortierlogik hinzufügen, wird ein HTTP 400-Fehler zurückgegeben.

1. Fügen Sie optional auf „extractive“ (extraktive) festgelegte **`"answers"`** (Antworten) hinzu, und geben Sie die Anzahl der Antworten an, wenn Sie mehr als eine erhalten möchten.

1. Optional können Sie den hervorgehobenen Stil anpassen, der auf Beschriftungen angewendet wird. Beschriftungen stellen eine Hervorhebung der Formatierung für Schlüsselpassagen im Dokument dar, die die Antwort zusammenfassen. Der Standardwert lautet `<em>`. Wenn Sie den Formatierungstyp angeben möchten (beispielsweise gelber Hintergrund), können Sie „highlightPreTag“ und „highlightPostTag“ festlegen.

1. Geben Sie alle anderen Parameter an, die in der Anforderung enthalten sein sollen. Parameter wie [speller](speller-how-to-add.md), [select](search-query-odata-select.md) und „count“ verbessern die Qualität der Anforderung und die Lesbarkeit der Antwort.

### <a name="review-the-response"></a>Antwort überprüfen

Die Antwort auf die oben stehende Abfrage gibt den folgenden Treffer als beste Übereinstimmung zurück. Beschriftungen werden automatisch zurückgegeben, mit Klartext und hervorgehobenen Versionen. Weitere Informationen zu semantischen Antworten finden Sie unter [Semantische Rangfolge und semantische Antworten](semantic-how-to-query-response.md).

```json
"@odata.count": 29,
"value": [
    {
        "@search.score": 1.8920634,
        "@search.rerankerScore": 1.1091284966096282,
        "@search.captions": [
            {
                "text": "Oceanside Resort. Budget. New Luxury Hotel. Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
                "highlights": "<strong>Oceanside Resort.</strong> Budget. New Luxury Hotel. Be the first to stay.<strong> Bay views</strong> from every room, location near the pier, rooftop pool, waterfront dining & more."
            }
        ],
        "HotelId": "18",
        "HotelName": "Oceanside Resort",
        "Description": "New Luxury Hotel.  Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
        "Category": "Budget"
    },
```

### <a name="parameters-used-in-a-semantic-query"></a>In einer Semantikabfrage verwendete Parameter

In der folgenden Tabelle erhalten Sie eine Gesamtübersicht über die zusammengefassten Abfrageparameter, die in einer Semantikabfrage verwendet werden. Eine Liste aller Parameter finden Sie unter [Dokumente durchsuchen (REST-Vorschau)](/rest/api/searchservice/preview-api/search-documents).

| Parameter | type | BESCHREIBUNG |
|-----------|-------|-------------|
| queryType | String | Gültige Werte sind „simple“, „full“ und „semantic“. Der Wert „semantic“ wird für Semantikabfragen benötigt. |
| queryLanguage | String | Für Semantikabfragen erforderlich. Derzeit wird nur „en-us“ implementiert. |
| searchFields | String | Hierbei handelt es sich um eine durch Trennzeichen getrennte Liste mit durchsuchbaren Feldern. Optional, aber empfohlen. Gibt die Felder an, für die die semantische Rangfolge ermittelt wird. </br></br>Im Gegensatz zu einfachen und vollständigen Abfragetypen bestimmt die Reihenfolge, in der Felder aufgeführt sind, welches Feld Vorrang hat.|
| answers |String | Dies ist ein optionales Feld, um anzugeben, ob semantische Antworten in das Ergebnis einbezogen werden sollen. Derzeit wird nur „extractive“ implementiert. Antworten können so konfiguriert werden, dass maximal fünf Werte zurückgegeben werden. In diesem Beispiel für|„extractive count3“ werden drei Antworten angezeigt. Der Standardwert ist 1.|

## <a name="query-with-search-explorer"></a>Abfragen mit Suchexplorer

Die folgende Abfrage zielt auf den integrierten Beispielindex für Hotels ab, verwendet API-Version 2020-06-30-Preview ab und wird im Suchexplorer ausgeführt. Die `$select`-Klausel schränkt die Ergebnisse auf einige wenige Felder ein, was die Überprüfung im ausführlichen JSON-Code im Suchexplorer vereinfacht.

### <a name="with-querytypesemantic"></a>Mit queryType=semantic

```json
search=I want a nice hotel on the water with a great restaurant&$select=HotelId,HotelName,Description,Tags&queryType=semantic&queryLanguage=english&searchFields=Description,Tags
```

Die ersten Ergebnisse sind wie folgt.

```json
{
    "@search.score": 0.38330218,
    "@search.rerankerScore": 0.9754053303040564,
    "HotelId": "18",
    "HotelName": "Oceanside Resort",
    "Description": "New Luxury Hotel. Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
    "Tags": [
        "view",
        "laundry service",
        "air conditioning"
    ]
},
{
    "@search.score": 1.8920634,
    "@search.rerankerScore": 0.8829904259182513,
    "HotelId": "36",
    "HotelName": "Pelham Hotel",
    "Description": "Stunning Downtown Hotel with indoor Pool. Ideally located close to theatres, museums and the convention center. Indoor Pool and Sauna and fitness centre. Popular Bar & Restaurant",
    "Tags": [
        "view",
        "pool",
        "24-hour front desk service"
    ]
},
{
    "@search.score": 0.95706713,
    "@search.rerankerScore": 0.8538530203513801,
    "HotelId": "22",
    "HotelName": "Stone Lion Inn",
    "Description": "Full breakfast buffet for 2 for only $1.  Excited to show off our room upgrades, faster high speed WiFi, updated corridors & meeting space. Come relax and enjoy your stay.",
    "Tags": [
        "laundry service",
        "air conditioning",
        "restaurant"
    ]
},
```

### <a name="with-querytype-default"></a>Mit queryType (Standard)

Führen Sie zum Vergleich dieselbe Abfrage wie oben aus, und entfernen Sie `&queryType=semantic&queryLanguage=english&searchFields=Description,Tags`. Beachten Sie, dass es in diesen Ergebnissen keine `"@search.rerankerScore"` gibt und dass für die ersten drei Positionen verschiedene Hotels angezeigt werden.

```json
{
    "@search.score": 8.633856,
    "HotelId": "3",
    "HotelName": "Triple Landscape Hotel",
    "Description": "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
    "Tags": [
        "air conditioning",
        "bar",
        "continental breakfast"
    ]
},
{
    "@search.score": 6.407289,
    "HotelId": "40",
    "HotelName": "Trails End Motel",
    "Description": "Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport.",
    "Tags": [
        "continental breakfast",
        "view",
        "view"
    ]
},
{
    "@search.score": 5.843788,
    "HotelId": "14",
    "HotelName": "Twin Vertex Hotel",
    "Description": "New experience in the Making.  Be the first to experience the luxury of the Twin Vertex. Reserve one of our newly-renovated guest rooms today.",
    "Tags": [
        "bar",
        "restaurant",
        "air conditioning"
    ]
},
```

## <a name="next-steps"></a>Nächste Schritte

Beachten Sie, dass semantische Rangfolge und semantische Antworten über ein anfängliches Resultset erstellt werden. Jede Logik, die die Qualität der anfänglichen Ergebnisse verbessert, wird an die semantische Suche weitergeleitet. Als nächsten Schritt überprüfen Sie die Features, die zu den anfänglichen Ergebnissen beitragen, einschließlich der Analysetools, die die Tokenisierung von Zeichenketten beeinflussen, der Bewertungsprofile, mit denen die Ergebnisse optimiert werden können, und des Standardrelevanzalgorithmus.

+ [Analysetools für Textverarbeitung](search-analyzers.md)
+ [Ähnlichkeit und Bewertung in Azure Cognitive Search](index-similarity-and-scoring.md)
+ [Hinzufügen von Bewertungsprofilen](index-add-scoring-profiles.md)
+ [Übersicht über die semantische Suche](semantic-search-overview.md)
+ [Hinzufügen der Rechtschreibprüfung für Abfragebegriffe](speller-how-to-add.md)
