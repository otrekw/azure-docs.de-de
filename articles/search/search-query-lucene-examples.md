---
title: Verwenden der vollständigen Lucene-Abfragesyntax
titleSuffix: Azure Cognitive Search
description: Lucene-Abfragesyntax für Fuzzysuche, NEAR-Suche, Term Boosting, Suche mit regulären Ausdrücken und Platzhaltersuche in einem Dienst der kognitiven Azure-Suche.
manager: nitinme
author: HeidiSteen
ms.author: heidist
tags: Lucene query analyzer syntax
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 10/05/2020
ms.openlocfilehash: df26cfc3b220f40a7e73ff1c750d2b2ae37e7625
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97401456"
---
# <a name="use-the-full-lucene-search-syntax-advanced-queries-in-azure-cognitive-search"></a>Verwenden der „vollständigen“ Lucene-Suchsyntax (erweiterte Abfragen in der kognitiven Azure-Suche)

Beim Erstellen von Abfragen für Azure Cognitive Search können Sie den standardmäßigen [einfachen Abfrageparser](query-simple-syntax.md) durch den leistungsfähigeren [Lucene-Abfrageparser in Azure Cognitive Search](query-lucene-syntax.md) ersetzen, um spezialisierte und erweiterte Abfragedefinitionen zu formulieren. 

Der Lucene-Parser unterstützt komplexe Abfragekonstrukte, z. B. feldbezogene Abfragen, Fuzzysuche, Infix- und Suffixplatzhaltersuche, NEAR-Suche, Term Boosting (Begriffsverstärkung) und die Suche mit regulären Ausdrücken. Die zusätzliche Leistung kommt mit zusätzlichen Verarbeitungsanforderungen, stellen Sie sich deshalb auf eine etwas längere Ausführungsdauer ein. In diesem Artikel finden Sie Beispiele, in denen Abfragevorgänge basierend auf der vollständigen Syntax veranschaulicht werden.

> [!Note]
> Viele der spezialisierten Abfragekonstruktionen, die über die vollständige Lucene-Abfragesyntax aktiviert werden, verfügen nicht über eine [Textanalyse](search-lucene-query-architecture.md#stage-2-lexical-analysis). Dies kann zu Überraschungen führen, wenn Sie mit Wortstammerkennung und Lemmatisierung rechnen. Die lexikalische Analyse erfolgt nur für vollständige Begriffe (Begriffsabfrage oder Ausdrucksabfrage). Abfragetypen mit unvollständigen Begriffen (Präfixabfrage, Platzerhalterabfrage, Abfrage regulärer Ausdrücke, Fuzzyabfrage) werden direkt unter Umgehung der Analysephase an die Abfragestruktur angehängt. Die einzige Transformation, die für partielle Abfrageausdrücke durchgeführt wird, ist die Umwandlung in Kleinbuchstaben. 
>

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

1. Legen Sie die URL auf **`https://azs-playground.search.windows.net/indexes/nycjobs/docs/search=*&api-version=2020-06-30&queryType=full`** fest.

   + Die Dokumentsammlung für den Index enthält den gesamten durchsuchbaren Inhalt. Ein im Anforderungsheader angegebener API-Abfrageschlüssel funktioniert nur für Lesevorgänge für die Dokumentsammlung.

   + **`$count=true`** gibt die Anzahl der Dokumente zurück, die den Suchkriterien entsprechen. Bei einer leeren Suchzeichenfolge ist dies die Anzahl aller Dokumente im Index (in unserem Beispiel „2558“ im Index für Jobs in NYC).

   + **`search=*`** ist eine Abfrage ohne Angaben, die einer NULL-Suche oder leeren Suche entspricht. Dies ist nicht besonders nützlich, aber es ist die einfachste Suche, die Sie ausführen können, und es werden alle abrufbaren Felder im Index mit allen Werten angezeigt.

   + **`queryType=full`** ruft das Analysetool für die vollständige Lucene-Analyse auf.

1. Fügen Sie als Überprüfungsschritt die folgende Anforderung in GET ein, und klicken Sie auf **Senden**. Die Ergebnisse werden als ausführliche JSON-Dokumente zurückgegeben.

   ```http
   https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2020-06-30&$count=true&search=*&queryType=full
   ```

### <a name="how-to-invoke-full-lucene-parsing"></a>Aufrufen der vollständigen Lucene-Analyse

Fügen Sie **`queryType=full`** hinzu, um die vollständige Abfragesyntax aufzurufen und die standardmäßige einfache Abfragesyntax außer Kraft zu setzen. In allen Beispielen in diesem Artikel wird der Suchparameter **`queryType=full`** verwendet. Damit wird angegeben, dass die vollständige Syntax vom Lucene-Abfrageparser verarbeitet wird. 

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
{
    "queryType": "full"
}
```

## <a name="example-1-query-scoped-to-a-list-of-fields"></a>Beispiel 1: Auf eine Liste von Feldern begrenzte Abfrage

Dieses erste Beispiel ist nicht parserspezifisch, wir beginnen jedoch mit diesem Beispiel, um das erste Grundkonzept für Abfragen vorzustellen: die Eingrenzung. In diesem Beispiel werden sowohl die Abfrageausführung als auch die Antwort auf wenige bestimmte Felder beschränkt. Bei Verwendung des Tools Postman oder Suchexplorer ist es wichtig, zu wissen, wie eine lesbare JSON-Antwort strukturiert wird. 

Diese Abfrage zielt nur auf *business_title* in **`searchFields`** ab, wobei der Parameter **`select`** das gleiche Feld in der Antwort angibt.

```http
POST https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "*",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Die Antwort auf diese Abfrage sollte etwa wie im folgenden Screenshot aussehen.

  ![Postman-Beispielantwort mit Bewertungen](media/search-query-lucene-examples/postman-sample-results.png)

Möglicherweise ist Ihnen die Suchbewertung in der Antwort aufgefallen. Zu einer einheitlichen Bewertung von **1** kommt es, wenn kein Rang vorhanden ist, weil es entweder keine Volltextsuche war oder weil keine Kriterien angegeben wurden. Bei einer leeren Suche werden Zeilen in beliebiger Reihenfolge zurückgegeben. Wenn Sie tatsächliche Kriterien einfügen, werden aussagekräftige Werte für die Suchbewertungen angezeigt.

## <a name="example-2-fielded-search"></a>Beispiel 2: Feldbezogene Suche

Die vollständige Lucene-Syntax unterstützt die Bereichsdefinition einzelner Ausdrücke auf ein bestimmtes Feld. Dieses Beispiel sucht nach Geschäftstiteln, in denen der Begriff „Senior“, jedoch nicht der Begriff „Junior“ enthalten ist. Sie können mehrere Felder mithilfe von „AND“ angeben.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:(senior NOT junior) AND posting_type:external",
    "searchFields": "business_title, posting_type",
    "select": "business_title, posting_type"
}
```

Die Antwort auf diese Abfrage sollte in etwa wie im folgenden Screenshot aussehen („posting_type“ wird nicht angezeigt).

  :::image type="content" source="media/search-query-lucene-examples/intrafieldfilter.png" alt-text="Postman-Beispielantwort für Suchausdruck" border="false":::

Beim Suchausdruck kann es sich um ein einzelnes Wort, einen einfachen Ausdruck oder einen komplexeren Ausdruck in Klammern handeln, optional mit booleschen Operatoren. Einige Beispiele für Änderungen sind in der folgenden Liste aufgeführt:

+ `business_title:(senior NOT junior)`
+ `state:("New York" OR "New Jersey")`
+ `business_title:(senior NOT junior) AND posting_type:external`

Achten Sie darauf, dass Sie mehrere Zeichenfolgen in Anführungszeichen setzen, wenn beide Zeichenfolgen als einzelne Entität ausgewertet werden sollen, da in diesem Fall im Feld `state` nach zwei verschiedenen Standorten gesucht wird. Abhängig vom Tool müssen Sie möglicherweise die Anführungszeichen mit Escapezeichen (`\`) versehen. 

Das in **fieldName:searchExpression** angegebene Feld muss durchsuchbar sein. Einzelheiten zur Verwendung von Indexattributen in Felddefinitionen finden Sie unter [Index erstellen (REST-API für die kognitive Azure-Suche)](/rest/api/searchservice/create-index).

> [!NOTE]
> Im obigen Beispiel wurde der Parameter **`searchFields`** weggelassen, weil für jeden Teil der Abfrage explizit ein Feldname angegeben ist. Sie können jedoch weiterhin **`searchFields`** verwenden, wenn die Abfrage mehrere Teile umfasst (mit AND-Anweisungen). Zum Beispiel würde `senior NOT junior` in der Abfrage `search=business_title:(senior NOT junior) AND external&searchFields=posting_type` nur mit dem Feld `business_title` und „external“ mit dem Feld `posting_type` abgeglichen werden. Der in `fieldName:searchExpression` angegebene Feldname hat immer Vorrang vor **`searchFields`** . Deshalb können wir in diesem Beispiel `business_title` aus **`searchFields`** weglassen.

## <a name="example-3-fuzzy-search"></a>Beispiel 3: Fuzzysuche

Die vollständige Lucene-Syntax unterstützt auch die Fuzzysuche, bei der sich Übereinstimmungen für Begriffe ergeben, die über eine ähnliche Konstruktion verfügen. Hängen Sie für eine Fuzzysuche das Tildesymbol `~` an das Ende eines einzelnen Worts mit einem optionalen Parameter an, einem Wert zwischen 0 und 2, der die Editierdistanz angibt. Beispielsweise würden bei `blue~` oder `blue~1` die Werte „blue“, „blues“ und „glue“ zurückgegeben.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:asosiate~",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Ausdrücke werden nicht direkt unterstützt. Sie können jedoch eine Fuzzyübereinstimmung für jeden Begriff eines mehrteiligen Ausdrucks angeben, wie z. B. `search=business_title:asosiate~ AND comm~`.  Im nachfolgenden Screenshot enthält die Antwort eine Übereinstimmung für *Community Associate*.

  :::image type="content" source="media/search-query-lucene-examples/fuzzysearch.png" alt-text="Antwort für die Fuzzysuche" border="false":::

> [!Note]
> Fuzzyabfragen werden nicht [analysiert](search-lucene-query-architecture.md#stage-2-lexical-analysis). Abfragetypen mit unvollständigen Begriffen (Präfixabfrage, Platzerhalterabfrage, Abfrage regulärer Ausdrücke, Fuzzyabfrage) werden direkt unter Umgehung der Analysephase an die Abfragestruktur angehängt. Die einzige Transformation, die für partielle Abfrageausdrücke durchgeführt wird, ist die Umwandlung in Kleinbuchstaben.
>

## <a name="example-4-proximity-search"></a>Beispiel 4: NEAR-Suche

NEAR-Suchen werden verwendet, um Begriffe zu suchen, die in einem Dokument nahe beieinander liegen. Fügen Sie ein Tildesymbol „~“ Symbol am Ende eines Ausdrucks ein, gefolgt von der Anzahl der Wörter, die den NEAR-Bereich bilden. Beispielsweise finden Sie mit der Abfrage „"Hotel Flughafen"~5“ die Begriffe „Hotel“ und „Flughafen“, wenn sie innerhalb von 5 Wörtern in einem Dokument vorkommen.

Diese Abfrage sucht nach den Begriffen „Senior“ und „Analyst“, wobei jeder Begriff durch nicht mehr als ein Wort getrennt ist, und die Anführungszeichen werden mit Escapezeichen (`\"`) versehen, um den Ausdruck beizubehalten:

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:\"senior analyst\"~1",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Die Antwort auf diese Abfrage sollte in etwa wie im folgenden Screenshot aussehen. 

  :::image type="content" source="media/search-query-lucene-examples/proximity-before.png" alt-text="Näherungsabfrage" border="false":::

Versuchen Sie es noch mal, und entfernen Sie den Abstand (`~0`) zwischen den Begriffen „Senior Analyst“. Beachten Sie, dass für diese Abfrage acht Dokumente zurückgegeben werden, während es für die vorherige Abfrage zehn waren.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:\"senior analyst\"~0",
    "searchFields": "business_title",
    "select": "business_title"
}
```

## <a name="example-5-term-boosting"></a>Beispiel 5: Term Boosting

Die Begriffsverstärkung (Term Boosting) bezieht sich auf das Höherbewerten eines Dokuments, wenn es den verstärkten Begriff enthält, im Verhältnis zu Dokumenten, die den Begriff nicht enthalten. Verwenden Sie zum Verstärken eines Begriffs das Caretzeichen (`^`) mit einem Verstärkungsfaktor (einer Zahl) am Ende des Begriffs, nach dem Sie suchen.

In dieser „Vorher“-Abfrage wird nach Stellen mit dem Begriff *computer analyst* gesucht. Beachten Sie, dass keine Ergebnisse mit den beiden Wörtern *computer* und *analyst* vorhanden sind, aber dass Stellen mit *computer* in den Ergebnissen oben angezeigt werden.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:computer analyst",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Wiederholen Sie die Suche mit der „Nachher“-Abfrage, und verstärken Sie die Ergebnisse nun mit dem Begriff *analyst* für den Begriff *computer*, falls nicht beide Wörter vorhanden sind. Eine für Menschen lesbare Version der Abfrage ist `search=business_title:computer analyst^2`. Bei einer praktikablen Abfrage in Postman wird `^2` als `%5E2` codiert.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:computer analyst%5e2",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Die Antwort auf diese Abfrage sollte etwa wie im folgenden Screenshot aussehen.

  :::image type="content" source="media/search-query-lucene-examples/termboostingafter.png" alt-text="Term Boosting danach" border="false":::

Das Term Boosting unterscheidet sich von Bewertungsprofilen darin, dass bei Bewertungsprofilen anstelle von bestimmten Begriffen bestimmte Felder verstärkt werden. Im folgenden Beispiel werden die Unterschiede veranschaulicht.

Betrachten Sie ein Bewertungsprofil, das Übereinstimmungen in einem bestimmten Feld verstärkt, beispielsweise **genre** im musicstoreindex-Beispiel. Mit der Begriffsverstärkung könnten Sie bestimmte Suchbegriffe noch höher bewerten als andere. Zum Beispiel werden mit „rock^2 electronic“ Dokumente, die die Suchbegriffe im Feld **genre** enthalten, höher eingestuft als andere durchsuchbare Felder im Index. Darüber hinaus werden Dokumente, die den Suchbegriff „Rock“ enthalten, höher eingestuft als der andere Suchbegriff „electronic“ – aufgrund des Werts (2) für die Begriffsverstärkung.

Je höher beim Festlegen der Faktorebene der Verstärkungsfaktor ist, desto relevanter wird der Begriff im Verhältnis zu anderen Suchbegriffen. Der Standardverstärkungsfaktor ist 1. Der Verstärkungsfaktor muss zwar positiv, kann jedoch kleiner als 1 sein (z. B. „0.2“).

## <a name="example-6-regex"></a>Beispiel 6: RegEx

Bei einer Suche mit regulärem Ausdruck werden Übereinstimmungen basierend auf dem Inhalt zwischen Schrägstrichen „/“ gefunden, wie in der [RegExp-Klasse](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/util/automaton/RegExp.html)dokumentiert.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:/(Sen|Jun)ior/",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Die Antwort auf diese Abfrage sollte etwa wie im folgenden Screenshot aussehen.

  :::image type="content" source="media/search-query-lucene-examples/regex.png" alt-text="RegEx-Abfrage" border="false":::

> [!Note]
> Abfragen mit regulärem Ausdruck werden nicht [analysiert](./search-lucene-query-architecture.md#stage-2-lexical-analysis). Die einzige Transformation, die für partielle Abfrageausdrücke durchgeführt wird, ist die Umwandlung in Kleinbuchstaben.
>

## <a name="example-7-wildcard-search"></a>Beispiel 7: Platzhaltersuche

Sie können die allgemein bekannte Syntax für die Platzhaltersuche nach mehreren (\*) oder einzelnen (?) Zeichen verwenden. Beachten Sie, dass der Lucene-Abfrageparser die Verwendung dieser Symbole bei einem einzelnen Begriff, nicht bei einem Ausdruck, unterstützt.

Mit dieser Abfrage wird nach Stellen gesucht, die das Präfix „Prog“ enthalten. Dazu gehören beispielsweise Berufsbezeichnungen mit den Begriffen „Programmierer“ und „Programmierung“. Das Symbol `*` oder `?` kann nicht als erstes Zeichen in einer Suche verwendet werden.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:prog*",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Die Antwort auf diese Abfrage sollte etwa wie im folgenden Screenshot aussehen.

  :::image type="content" source="media/search-query-lucene-examples/wildcard.png" alt-text="Platzhalterabfrage" border="false":::

> [!Note]
> Abfragen mit Platzhaltern werden nicht [analysiert](./search-lucene-query-architecture.md#stage-2-lexical-analysis). Die einzige Transformation, die für partielle Abfrageausdrücke durchgeführt wird, ist die Umwandlung in Kleinbuchstaben.
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