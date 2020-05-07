---
title: Einfache Abfragesyntax
titleSuffix: Azure Cognitive Search
description: Referenz für die einfache Abfragesyntax, die für Volltextsuchabfragen in der kognitiven Azure-Suche verwendet wird.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: dfd75ad2c6ae246bfe6ee8b983744b3db07a841f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82194940"
---
# <a name="simple-query-syntax-in-azure-cognitive-search"></a>Einfache Abfragesyntax in der kognitiven Azure-Suche

Die kognitive Azure-Suche implementiert zwei Lucene-basierte Abfragesprachen: [Einfacher Abfrageparser](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html) und der [Lucene-Abfrageparser](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html).

Der einfache Parser ist flexibler und versucht auch dann, eine Anforderung zu interpretieren, wenn sie nicht perfekt zusammengesetzt ist. Aufgrund dieser Flexibilität ist er die Standardeinstellung für Abfragen in Azure Cognitive Search. 

Die einfache Syntax wird für Abfrageausdrücke verwendet, die im Parameter `search` einer [Anforderung zum Durchsuchen von Dokumenten](https://docs.microsoft.com/rest/api/searchservice/search-documents) übergeben werden. Sie ist nicht mit der [OData-Syntax](query-odata-filter-orderby-syntax.md) zu verwechseln, die für den Parameter [$filter expressions](search-filters.md) dieser API zum Durchsuchen von Dokumenten verwendet wird. Die Parameter `search` und `$filter` weisen eine unterschiedliche Syntax mit jeweils eigenen Regeln für Abfrageerstellung, Escapezeichen für Zeichenfolgen usw. auf.

Der einfache Parser basiert zwar auf der [Apache Lucene Simple Query Parser](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html)-Klasse, die Implementierung in Azure Cognitive Search schließt jedoch die Fuzzysuche aus. Wenn Sie [Fuzzysuche](search-query-fuzzy.md) oder andere erweiterte Abfrageformen benötigen, sollten Sie stattdessen die alternative [vollständige Lucene-Abfragesyntax](query-lucene-syntax.md) in Betracht ziehen.

## <a name="invoke-simple-parsing"></a>Aufrufen der einfachen Analyse

Die einfache Syntax ist die Standardeinstellung. Der Aufruf ist nur notwendig, wenn Sie die Syntax von vollständig auf einfach zurücksetzen. Um die Syntax explizit festzulegen, verwenden Sie den `queryType`-Suchparameter. Gültige Werte sind `queryType=simple` oder `queryType=full`. Dabei ist `simple` der Standardwert, und `full` ruft den [vollständigen Lucene-Abfrageparser](query-lucene-syntax.md) für erweiterte Abfragen auf. 

## <a name="syntax-fundamentals"></a>Grundlagen der Syntax

Jeder Text mit einem oder mehreren Begriffen gilt als gültiger Ausgangspunkt für die Ausführung der Abfrage. Die kognitive Azure-Suche findet Dokumente, die einen oder alle der Begriffe enthalten, einschließlich aller Variationen, die bei der Analyse des Textes gefunden wurden.

So einfach das klingt, es gibt einen Aspekt der Abfrageausführung in der kognitiven Azure-Suche, der *möglicherweise* unerwartete Ergebnisse liefert, die die Suchergebnisse eher erhöhen als verringern, da mehr Begriffe und Operatoren zur Eingabezeichenkette hinzugefügt werden. Ob diese Erweiterung tatsächlich erfolgt, hängt von der Einbeziehung eines NOT-Operators ab, kombiniert mit einer **searchMode**-Parametereinstellung, die bestimmt, wie NOT in Form von AND- oder OR-Verhalten interpretiert wird. Weitere Informationen finden Sie unter [NOT-Operator](#not-operator).

### <a name="precedence-operators-grouping"></a>Rangfolgenoperatoren (Gruppierung)

Sie können mithilfe von Klammern Unterabfragen erstellen, die Operatoren innerhalb der Anweisung in Klammern enthalten. Beispielsweise sucht `motel+(wifi|luxury)` nach Dokumenten, die den Begriff „motel“ und entweder „wifi“ oder „luxury“ (oder beides) enthalten.

Die Feldgruppierung funktioniert ähnlich, beschränkt die Gruppierung jedoch auf ein einzelnes Feld. Beispielsweise durchsucht `hotelAmenities:(gym+(wifi|pool))` das Feld „hotelAmenities“ nach „gym“ und „wifi“ oder nach „gym“ und „pool“.  

### <a name="escaping-search-operators"></a>Escaping-Suchoperatoren  

In der einfachen Syntax schließen die Suchoperatoren die folgenden Zeichen ein: `+ | " ( ) ' \`  

Wenn eines dieser Zeichen Teil eines Tokens im Index ist, versehen Sie es mit einem einzelnen umgekehrten Schrägstrich (`\`) als Escapezeichen in der Abfrage. Angenommen, Sie haben eine benutzerdefinierte Analyse für die gesamte begriffsbasierte Tokenisierung verwendet, und Ihr Index enthält die Zeichenfolge „Luxury+Hotel“. Fügen Sie ein Escapezeichen ein, um eine genaue Übereinstimmung für dieses Token zu erhalten:  `search=luxury\+hotel`. 

Zur Vereinfachung für typischere Fälle gibt es zwei Ausnahmen von dieser Regel, bei denen kein Escapezeichen erforderlich ist:  

+ Der NOT-Operator `-` muss nur dann mit einem Escapezeichen versehen werden, wenn es sich um das erste Zeichen nach einem Leerzeichen handelt. Wenn das `-` in der Mitte auftritt (z. B. in `3352CDD0-EF30-4A2E-A512-3B30AF40F3FD`), können Sie das Escapezeichen auslassen.

+ Der Suffix-Operator `*` muss nur dann mit einem Escapezeichen versehen werden, wenn es sich um das letzte Zeichen vor einem Leerzeichen handelt. Wenn das `*` in der Mitte auftritt (z. B. in `4*4=16`), ist kein Escapezeichen erforderlich.

> [!NOTE]  
> Die Lucene-Standardanalyse löscht bei der [lexikalischen Analyse](search-lucene-query-architecture.md#stage-2-lexical-analysis) standardmäßig Bindestriche, Leerzeichen, kaufmännische Und-Zeichen und andere Zeichen in Wörtern und teilt die Wörter an diesen Stellen auf. Wenn Sonderzeichen in der Abfragezeichenfolge beibehalten werden müssen, benötigen Sie möglicherweise ein Analysetool, das sie im Index belässt. Zu den Optionen gehören Microsoft Natural [Language Analyzer](index-add-language-analyzers.md) (Analysetool für natürliche Sprache), bei dem Wörter mit Bindestrichen beibehalten werden, oder ein benutzerdefiniertes Analysetool für komplexere Muster. Weitere Informationen finden Sie unter [Teilausdrücke, Muster und Sonderzeichen](search-query-partial-matching.md).

### <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>Codierung von unsicheren und reservierten Zeichen in URLs

Stellen Sie sicher, dass alle unsicheren und reservierten Zeichen in einer URL codiert werden. „#“ ist beispielsweise ein Fragment-/Ankerbezeichner in einer URL und deshalb ein unsicheres Zeichen. Bei der Verwendung in einer URL muss das Zeichen in `%23` codiert werden. „&“ und „=“ sind Beispiele für reservierte Zeichen, da sie in Azure Cognitive Search zum Trennen von Parametern und Angeben von Werten dienen. Weitere Informationen finden Sie unter [RFC1738: Uniform Resource Locator (URL)](https://www.ietf.org/rfc/rfc1738.txt).

Unsichere Zeichen sind ``" ` < > # % { } | \ ^ ~ [ ]``. Reservierte Zeichen sind `; / ? : @ = + &`.

###  <a name="query-size-limits"></a><a name="bkmk_querysizelimits"></a> Abfragegrößenlimits

 Die Größe der Abfragen, die Sie an Azure Cognitive Search senden können, ist begrenzt. Insbesondere können Sie maximal 1.024 Klauseln (durch AND, OR usw. getrennte Ausdrücke) verwenden. Für die Größe der einzelnen Begriffe in einer Abfrage gilt zudem ein Grenzwert von ungefähr 32 KB. Wenn Ihre Anwendung programmgesteuert Suchabfragen generiert, sollten Sie durch den Anwendungsentwurf sicherstellen, dass sie keine Abfragen unbegrenzter Größe erzeugt.  

## <a name="boolean-search"></a>Boolesche Suche

Sie können boolesche Operatoren (AND, OR, NOT) in eine Abfragezeichenfolge einbetten, um einen umfangreichen Satz von Kriterien zu erstellen, anhand derer passende Dokumente gefunden werden. 

### <a name="and-operator-"></a>AND-Operator `+`

Der AND-Operator ist ein Pluszeichen (+). Zum Beispiel sucht `wifi + luxury` nach Dokumenten, die sowohl `wifi` als auch `luxury` enthalten.

### <a name="or-operator-"></a>OR-Operator`|`

Der OR-Operator ist ein vertikaler Balken bzw. ein senkrechter Strich. Zum Beispiel sucht `wifi | luxury` nach Dokumenten, die entweder `wifi`oder `luxury` oder beides enthalten.

<a name="not-operator"></a>

### <a name="not-operator--"></a>NOT-Operator `-`

Der NOT-Operator ist ein Minuszeichen (-). Beispiel: `wifi –luxury` sucht nach Dokumenten, die den Begriff `wifi` enthalten und/oder `luxury` nicht enthalten.

Der Parameter **searchMode** in einer Abfrageanforderung steuert, ob ein Begriff mit dem NOT-Operator mit anderen Begriffen in der Abfrage per AND oder OR verknüpft wird (vorausgesetzt, dass es keinen Operator „`+`“ oder „`|`“ für die anderen Begriffe gibt). Gültige Werte sind `any` oder `all`.

`searchMode=any` erhöht die Trefferquote von Abfragen, weil mehr Ergebnisse einbezogen werden, und `-` wird standardmäßig als „OR NOT“ interpretiert. So stimmt beispielsweise `wifi -luxury` mit Dokumenten überein, die entweder den Begriff `wifi` enthalten oder die den Begriff `luxury` nicht enthalten.

`searchMode=all` erhöht die Genauigkeit von Abfragen, weil weniger Ergebnisse einbezogen werden und „-“ standardmäßig als „AND NOT“ interpretiert wird. So stimmt beispielsweise `wifi -luxury` mit Dokumenten überein, die den Begriff `wifi` enthalten und die nicht den Begriff „Luxus“ enthalten. Dies ist wohl ein intuitiveres Verhalten für den `-`-Operator. Daher sollten Sie die Verwendung von `searchMode=all` statt `searchMode=any` in Betracht ziehen, wenn Sie Suchen im Hinblick auf die Genauigkeit statt auf die Trefferquote optimieren möchten *und* Ihre Benutzer den `-`-Operator häufig bei Suchen verwenden.

Berücksichtigen Sie bei der Entscheidung über eine **searchMode**-Einstellung die Benutzerinteraktionsmuster für Abfragen in verschiedenen Anwendungen. Benutzer, die nach Informationen suchen, beziehen eher einen Operator in eine Abfrage mit ein – im Gegensatz zu e-Commerce-Websites mit mehr integrierten Navigationsstrukturen.

<a name="prefix-search"></a>

## <a name="prefix-search"></a>Präfixsuche

Der Suffixoperator ist ein Sternchen `*`. Beispielsweise findet `lingui*` „linguistisch“ oder „linguini“, wobei die Groß-/Kleinschreibung ignoriert wird. 

Ähnlich wie Filter sucht eine Präfixabfrage nach einer genauen Übereinstimmung. Daher wird die Relevanz nicht bewertet (alle Ergebnisse erhalten die Suchbewertung 1,0). Präfixabfragen können insbesondere dann langsam verlaufen, wenn der Index groß ist und das Präfix aus einer kleinen Anzahl von Zeichen besteht. 

Wenn Sie eine Suffixabfrage für den letzten Teil einer Zeichenfolge ausführen möchten, verwenden Sie eine [Platzhaltersuche](query-lucene-syntax.md#bkmk_wildcard) und die vollständige Lucene-Syntax.

## <a name="phrase-search-"></a>Ausdruckssuche `"`

Eine Begriffsuche ist eine Abfrage für einen oder mehrere Begriffe, bei denen jeder der Begriffe als Übereinstimmung betrachtet wird. Eine Ausdruckssuche ist ein exakter Ausdruck in Anführungszeichen (`" "`). Zum Beispiel: `Roach Motel` (ohne Anführungszeichen) würde nach Dokumenten suchen, die `Roach` und/oder `Motel` enthalten, und zwar überall in beliebiger Reihenfolge, `"Roach Motel"` (mit Anführungszeichen) stimmt nur mit Dokumenten überein, die den gesamten Ausdruck zusammen und in dieser Reihenfolge enthalten (die Textanalyse gilt weiterhin).

## <a name="see-also"></a>Weitere Informationen  

+ [Funktionsweise der Volltextsuche in Azure Cognitive Search](search-lucene-query-architecture.md)
+ [Abfragebeispiele für die einfache Suche](search-query-simple-examples.md)
+ [Abfragebeispiele für die vollständige Lucene-Suche](search-query-lucene-examples.md)
+ [Search Documents (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) (Suchen nach Dokumenten (Azure Search Service-REST-API))
+ [Lucene-Abfragesyntax](query-lucene-syntax.md)
+ [OData-Ausdruckssyntax](query-odata-filter-orderby-syntax.md) 
