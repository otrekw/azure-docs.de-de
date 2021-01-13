---
title: Verwenden der einfachen Lucene-Abfragesyntax
titleSuffix: Azure Cognitive Search
description: Lernen Sie anhand von Beispielen, indem Sie Abfragen auf der Grundlage der einfachen Syntax für Volltextsuche, Filtersuche, geografische Suche und Facettensuche für einen Azure Cognitive Search-Index ausführen.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/12/2020
ms.openlocfilehash: ff9495e37a499b5502d8f8ced79b69608fa9552a
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97401745"
---
# <a name="use-the-simple-search-syntax-in-azure-cognitive-search"></a>Verwenden der „einfachen“ Suchsyntax in Azure Cognitive Search

In Azure Cognitive Search wird bei der [einfachen Abfragesyntax](query-simple-syntax.md) der Standardabfrageparser für die Ausführung von Volltextsuchabfragen aufgerufen. Der Parser ist schnell und eignet sich für gängige Szenarien, einschließlich Volltextsuche, Filter- und Facettensuche und Präfixsuche. In diesem Artikel wird die Verwendung der einfachen Syntax in einer Anforderung zum [Durchsuchen von Dokumenten (REST-API)](/rest/api/searchservice/search-documents) anhand von Beispielen veranschaulicht.

> [!NOTE]
> Eine alternative Abfragesyntax ist die [vollständige Lucene-Abfragesyntax](query-lucene-syntax.md), die komplexere Abfragestrukturen wie z. B. die Fuzzysuche und die Platzhaltersuche unterstützt. Weitere Informationen und Beispiele finden Sie unter [Verwenden der vollständigen Lucene-Syntax](search-query-lucene-examples.md).

## <a name="nyc-jobs-examples"></a>Beispiele für Jobs in New York City

Der in den folgenden Beispielen verwendete [Suchindex für Jobs in NYC](https://azjobsdemo.azurewebsites.net/) besteht aus Stellenangeboten basierend auf einem Dataset, das von der [Initiative „NYC OpenData“](https://nycopendata.socrata.com/) bereitgestellt wird. Diese Daten sollten weder als aktuell noch als vollständig betrachtet werden. Der Index wird über einen Sandboxdienst von Microsoft bereitgestellt. Das bedeutet, dass Sie zum Testen dieser Abfragen weder ein Azure-Abonnement noch Azure Cognitive Search benötigen.

Sie benötigen lediglich Postman oder ein äquivalentes Tool zum Senden von HTTP-Anforderungen per GET oder POST. Wenn Sie mit diesen Tools nicht vertraut sind, lesen Sie den Artikel [Schnellstart: Untersuchen der Azure Cognitive Search-REST-API](search-get-started-rest.md).

## <a name="set-up-the-request"></a>Einrichten der Anforderung

1. Anforderungsheader müssen die folgenden Werte aufweisen:

   | Schlüssel | Wert |
   |-----|-------|
   | Content-Type | `application/json`|
   | api-key  | `252044BE3886FE4A8E3BAA4F595114BB` </br> (Dies ist der tatsächliche API-Abfrageschlüssel für den Sandbox-Suchdienst, der den Index für Jobs in NYC hostet.) |

1. Legen Sie das Verb auf **`GET`** fest.

1. Legen Sie die URL auf **`https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2020-06-30&search=*&$count=true`** fest. 

   + Die Dokumentsammlung für den Index enthält den gesamten durchsuchbaren Inhalt. Der im Anforderungsheader angegebene API-Abfrageschlüssel funktioniert nur für Lesevorgänge für die Dokumentsammlung.

   + **`$count=true`** gibt die Anzahl der Dokumente zurück, die den Suchkriterien entsprechen. Bei einer leeren Suchzeichenfolge ist dies die Anzahl aller Dokumente im Index (in unserem Beispiel „2558“ im Index für Jobs in NYC).

   + **`search=*`** ist eine Abfrage ohne Angaben, die einer NULL-Suche oder leeren Suche entspricht. Dies ist nicht besonders nützlich, aber es ist die einfachste Suche, die Sie ausführen können, und es werden alle abrufbaren Felder im Index mit allen Werten angezeigt.

1. Fügen Sie als Überprüfungsschritt die folgende Anforderung in GET ein, und klicken Sie auf **Senden**. Die Ergebnisse werden als ausführliche JSON-Dokumente zurückgegeben.

   ```http
   https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2020-06-30&$count=true&search=*&queryType=full
   ```

### <a name="how-to-invoke-simple-query-parsing"></a>Aufrufen der einfachen Abfrageanalyse

Für interaktive Abfragen müssen Sie nichts angeben: einfache Abfragen sind die Standardeinstellung. Wenn Sie zuvor **`queryType=full`** aufgerufen haben, können Sie im Code die Standardeinstellung mit **`queryType=simple`** zurücksetzen.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
{
    "queryType": "simple"
}
```

## <a name="example-1-full-text-search-on-specific-fields"></a>Beispiel 1: Volltextsuche für bestimmte Felder

Dieses erste Beispiel ist nicht parserspezifisch, wir beginnen jedoch mit diesem Beispiel, um das erste Grundkonzept für Abfragen vorzustellen: die Eingrenzung. In diesem Beispiel werden sowohl die Abfrageausführung als auch die Antwort auf wenige bestimmte Felder beschränkt. Bei Verwendung des Tools Postman oder Suchexplorer ist es wichtig, zu wissen, wie eine lesbare JSON-Antwort strukturiert wird. 

Diese Abfrage zielt nur auf *business_title* in **`searchFields`** ab, wobei der Parameter **`select`** das gleiche Feld in der Antwort angibt.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
{
    "count": true,
    "queryType": "simple",
    "search": "*",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Die Antwort auf diese Abfrage sollte etwa wie im folgenden Screenshot aussehen.

  :::image type="content" source="media/search-query-lucene-examples/postman-sample-results.png" alt-text="Postman-Beispielantwort" border="false":::

Möglicherweise ist Ihnen die Suchbewertung in der Antwort aufgefallen. Zu einer einheitlichen Bewertung von **1** kommt es, wenn kein Rang vorhanden ist, weil es entweder keine Volltextsuche war oder weil keine Kriterien angegeben wurden. Bei einer leeren Suche werden Zeilen in beliebiger Reihenfolge zurückgegeben. Wenn Sie tatsächliche Kriterien einfügen, werden aussagekräftige Werte für die Suchbewertungen angezeigt.

## <a name="example-2-look-up-by-id"></a>Beispiel 2: Suchen anhand der ID

Wenn Sie Suchergebnisse in einer Abfrage zurückgeben, besteht ein logischer nachfolgender Schritt darin, eine Detailseite bereitzustellen, die mehr Felder aus dem Dokument enthält. In diesem Beispiel wird veranschaulicht, wie Sie ein einzelnes Dokument mithilfe eines [Suchvorgangs](/rest/api/searchservice/lookup-document) zurückgeben, um die Dokument-ID zu übergeben.

Alle Dokumente verfügen über einen eindeutigen Bezeichner. Beim Ausprobieren der Syntax für eine Suchabfrage können Sie zuerst eine Liste mit Dokument-IDs zurückgeben, um eine geeignete ID auszuwählen. Für „NYC Jobs“ sind diese Bezeichner im Feld `id` gespeichert.

```http
GET /indexes/nycjobs/docs?api-version=2020-06-30&search=*&$select=id&$count=true
```

Rufen Sie als Nächstes ein Dokument aus der Sammlung basierend auf der `id` „9E1E3AF9-0660-4E00-AF51-9B654925A2D5“ ab, die in der vorherigen Antwort an erster Stelle angezeigt wurde. Mit der folgenden Abfrage werden alle abrufbaren Felder für das gesamte Dokument zurückgegeben.

```http
GET /indexes/nycjobs/docs/9E1E3AF9-0660-4E00-AF51-9B654925A2D5?api-version=2020-06-30
```

## <a name="example-3-filter-queries"></a>Beispiel 3: Filterabfragen

Die [Filtersyntax](./search-query-odata-filter.md) ist ein OData-Ausdruck, den Sie allein oder mit **`search`** verwenden können. Ein eigenständiger Filter ohne Suchparameter ist nützlich, wenn der Filterausdruck Dokumente von Interesse vollständig qualifizieren kann. Ohne Abfragezeichenfolge wird keine lexikalische oder linguistische Analyse, Bewertung („1“ für alle Bewertungen) und Rangzuweisung durchgeführt. Beachten Sie, dass die Suchzeichenfolge leer ist.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "salary_frequency eq 'Annual' and salary_range_from gt 90000",
      "select": "job_id, business_title, agency, salary_range_from"
    }
```

Wenn sie zusammen verwendet werden, wird der Filter zuerst auf den gesamten Index angewendet, und anschließend wird die Suche für die Ergebnisse des Filters ausgeführt. Filter können daher eine nützliche Methode zum Verbessern der Abfrageleistung darstellen, da sie die Menge der Dokumente reduzieren, die bei der Suchabfrage verarbeitet werden müssen.

  :::image type="content" source="media/search-query-simple-examples/filtered-query.png" alt-text="Filtern von Abfrageantworten" border="false":::

Eine andere leistungsstarke Möglichkeit zum Kombinieren von Filter und Suche ist die Verwendung von **`search.ismatch*()`** in einem Filterausdruck, wobei Sie eine Suchabfrage innerhalb des Filters durchführen können. Bei diesem Filterausdruck wird ein Platzhalter für *plan* (um „term plan“, „planner“, „planning“ usw. einzuschließen) verwendet, um „business_title“ auszuwählen.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "search.ismatch('plan*', 'business_title', 'full', 'any')",
      "select": "job_id, business_title, agency, salary_range_from"
    }
```

Weitere Informationen zu dieser Funktion finden Sie unter [„search.ismatch“ unter „Filterbeispiele“](./search-query-odata-full-text-search-functions.md#examples).

## <a name="example-4-range-filters"></a>Beispiel 4: Bereichsfilter

Die Bereichsfilterung wird durch **`$filter`** -Ausdrücke für jeden Datentyp unterstützt. In den folgenden Beispielen werden numerische und Zeichenfolgenfelder durchsucht. 

Datentypen sind bei Bereichsfiltern wichtig und funktionieren am besten bei numerischen Daten in numerischen Feldern und Zeichenfolgendaten in Zeichenfolgenfeldern. Numerische Daten in Zeichenfolgenfeldern eignen sich nicht für Bereiche, da sich numerische Zeichenfolgen in Azure Cognitive Search nicht vergleichen lassen.

In der folgenden Abfrage wird ein numerischer Bereich verwendet:

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "num_of_positions ge 5 and num_of_positions lt 10",
      "select": "job_id, business_title, num_of_positions, agency",
      "orderby": "agency"
    }
```
Die Antwort auf diese Abfrage sollte etwa wie im folgenden Screenshot aussehen.

  :::image type="content" source="media/search-query-simple-examples/rangefilternumeric.png" alt-text="Bereichsfilter für numerische Bereiche" border="false":::

In der folgenden Abfrage liegt der Bereich über einem Zeichenfolgenfeld („business_title“):

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "business_title ge 'A*' and business_title lt 'C*'",
      "select": "job_id, business_title, agency",
      "orderby": "business_title"
    }
```

Die Antwort auf diese Abfrage sollte etwa wie im folgenden Screenshot aussehen.

  :::image type="content" source="media/search-query-simple-examples/rangefiltertext.png" alt-text="Bereichsfilter für Textbereiche" border="false":::

> [!NOTE]
> Die Verwendung von Facettenwertbereichen ist eine verbreitete Anforderung für Suchanwendungen. Weitere Informationen und Beispiele finden Sie unter [Erstellen eines Facettenfilters](search-filters-facets.md).

## <a name="example-5-geo-search"></a>Beispiel 5: Geografische Suche

Der Beispielindex enthält das Feld „geo_location“ mit Breiten- und Längengradkoordinaten. In diesem Beispiel wird die [geo.distance-Funktion](search-query-odata-geo-spatial-functions.md#examples) verwendet, mit der Dokumente im Umkreis eines Startpunkts bis zu einer beliebigen festgelegten Entfernung (in Kilometer) gefiltert werden. Sie können den letzten Wert in der Abfrage (4) anpassen, um die Fläche der Abfrage zu verkleinern oder zu vergrößern.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "geo.distance(geo_location, geography'POINT(-74.11734 40.634384)') le 4",
      "select": "business_title, work_location"
    }
```

Um die Lesbarkeit der Ergebnisse zu verbessern, werden die Suchergebnisse gekürzt, sodass sie die Position und den Arbeitsstandort enthalten. Die Anfangskoordinaten wurden aus einem zufällig ausgewählten Dokument im Index abgerufen (in diesem Fall für einen Arbeitsstandort auf Staten Island).

  :::image type="content" source="media/search-query-simple-examples/geo-search.png" alt-text="Karte von Staten Island" border="false":::

## <a name="example-6-search-precision"></a>Beispiel 6: Suchgenauigkeit

Begriffsabfragen sind einzelne Begriffe (ggf. in größerer Zahl), die unabhängig voneinander ausgewertet werden. Ausdrucksabfragen sind in Anführungszeichen gesetzt und werden Wort für Wort als Zeichenfolge ausgewertet. Die Genauigkeit der Übereinstimmung wird mithilfe von Operatoren und des searchMode-Elements gesteuert.

Beispiel 1: `search=fire` führt zu 140 Ergebnissen, wobei alle Übereinstimmungen das Wort „fire“ an irgendeiner Stelle im Dokument enthalten.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "fire"
    }
```

Beispiel 2: `search=fire department` gibt 2.002 Ergebnisse zurück. Übereinstimmungen werden für Dokumente zurückgegeben, die entweder den Begriff „fire“ oder „department“ enthalten.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "fire department"
    }
```

Beispiel 3: `search="fire department"` gibt 77 Ergebnisse zurück. Wenn Sie die Zeichenfolge in Anführungszeichen einschließen, wird eine aus beiden Begriffen bestehende Ausdruckssuche erstellt, und es werden Übereinstimmungen für tokenisierte Begriffe im Index gefunden, die aus einer Kombination der Begriffe bestehen. Dies ist der Grund dafür, warum die Suche nach `search=+fire +department` hiermit nicht gleichwertig ist. Beide Begriffe sind erforderlich, aber es wird unabhängig danach gesucht. 

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
    "count": true,
    "search": "\"fire department\""
    }
```

> [!Note]
> Weil eine Ausdrucksabfrage durch Anführungszeichen angegeben wird, haben wir in diesem Beispiel ein Escapezeichen (`\`) hinzugefügt, um die Syntax beizubehalten.

## <a name="example-7-booleans-with-searchmode"></a>Beispiel 7: Boolesche Werte mit „searchMode“

Bei der einfachen Syntax werden boolesche Operatoren in Form von Zeichen (`+, -, |`) unterstützt. Mit dem Parameter „searchMode“ werden Kompromisse in Bezug auf die Genauigkeit und Trefferquote angegeben, wobei bei **`searchMode=any`** die Trefferquote (eine Übereinstimmung mit einem der Kriterien qualifiziert ein Dokument für das Resultset) und bei **`searchMode=all`** die Genauigkeit (alle Kriterien müssen übereinstimmen) im Mittelpunkt steht. 

Die Standardeinstellung ist **`searchMode=any`** . Dies kann verwirrend sein, wenn Sie für eine Abfrage mehrere Operatoren verwenden und keinen engeren Bereich von Ergebnissen erhalten, sondern einen weiteren Bereich. Dies gilt besonders für den Operator NOT, bei dem die Ergebnisse alle Dokumente umfassen, die einen bestimmten Begriff „nicht enthalten“.

Bei Verwendung der Standardeinstellung „searchMode=any“ werden 2800 Dokumente zurückgegeben: Dokumente, die den Begriff „fire department“ enthalten, sowie alle Dokumente, die den Begriff „Metrotech Center“ nicht enthalten.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"-\"Metrotech Center\"",
      "searchMode": "any"
    }
```

Die Antwort auf diese Abfrage sollte etwa wie im folgenden Screenshot aussehen.

  :::image type="content" source="media/search-query-simple-examples/searchmodeany.png" alt-text="Suchmodus „any“" border="false":::

Wenn Sie die Einstellung in **`searchMode=all`** ändern, kommt es für die Kriterien zu einem kumulativen Effekt, und es wird ein kleineres Resultset mit nur 21 Dokumenten zurückgegeben. Dies sind Dokumente, die den gesamten Begriff „fire department“ enthalten, abzüglich der Jobs unter der Metrotech Center-Adresse.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"-\"Metrotech Center\"",
      "searchMode": "all"
    }
```

  :::image type="content" source="media/search-query-simple-examples/searchmodeall.png" alt-text="Suchmodus „all“" border="false":::

## <a name="example-8-structuring-results"></a>Beispiel 8: Strukturieren von Ergebnissen

Mit mehreren Parametern wird gesteuert, welche Felder in den Suchergebnissen enthalten sind, wie viele Dokumente in jedem Batch zurückgegeben werden und welche Sortierreihenfolge verwendet wird. In diesem Beispiel wird auf einige der vorherigen Beispiele zurückgegriffen, wobei die Ergebnisse mit der **`$select`** -Anweisung auf bestimmte Felder beschränkt und „wörtliche“ Suchkriterien verwendet werden. Es werden 82 Übereinstimmungen zurückgegeben.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"",
      "searchMode": "any",
      "select": "job_id,agency,business_title,civil_service_title,work_location,job_description"
    }
```

Basierend auf dem vorherigen Beispiel können Sie nach der Bezeichnung sortieren. Diese Sortierung funktioniert, weil „civil_service_title“ im Index *sortierbar* ist.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"",
      "searchMode": "any",
      "select": "job_id,agency,business_title,civil_service_title,work_location,job_description",
      "orderby": "civil_service_title"
    }
```

Die Auslagerungsergebnisse werden mit dem Parameter **`$top`** implementiert, und in diesem Fall werden die obersten fünf Dokumente zurückgegeben:

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"",
      "searchMode": "any",
      "select": "job_id,agency,business_title,civil_service_title,work_location,job_description",
      "orderby": "civil_service_title",
      "top": "5"
    }
```

Überspringen Sie den ersten Batch, um die nächsten fünf Dokumente zu erhalten:

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"",
      "searchMode": "any",
      "select": "job_id,agency,business_title,civil_service_title,work_location,job_description",
      "orderby": "civil_service_title",
      "top": "5",
      "skip": "5"
    }
```

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