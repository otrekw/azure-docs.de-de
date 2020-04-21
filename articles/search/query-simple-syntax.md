---
title: Einfache Abfragesyntax
titleSuffix: Azure Cognitive Search
description: Referenz für die einfache Abfragesyntax, die für Volltextsuchabfragen in der kognitiven Azure-Suche verwendet wird.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/12/2020
ms.openlocfilehash: 066190ff6b735d30db351ff90c0b6e5173b7f583
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "81258863"
---
# <a name="simple-query-syntax-in-azure-cognitive-search"></a>Einfache Abfragesyntax in der kognitiven Azure-Suche

Die kognitive Azure-Suche implementiert zwei Lucene-basierte Abfragesprachen: [Einfacher Abfrageparser](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html) und der [Lucene-Abfrageparser](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). 

In der kognitiven Azure-Suche (Azure Cognitive Search) schließt die einfache Abfragesyntax Fuzzysuchvorgänge aus. Verwenden Sie stattdessen die vollständige Lucene-Syntax für die [Fuzzysuche](search-query-fuzzy.md).

> [!NOTE]
> Die einfache Abfragesyntax wird für Abfrageausdrücke verwendet, die im **search**-Parameter der [Dokumente durchsuchen](https://docs.microsoft.com/rest/api/searchservice/search-documents)-API übergeben werden. Sie sollte nicht mit der [OData-Syntax](query-odata-filter-orderby-syntax.md) verwechselt werden, die für den [$filter](search-filters.md)-Parameter dieser API verwendet wird. Diese unterschiedlichen Syntaxen verfügen über eigene Regeln für das Erstellen von Abfragen, das Auskommentieren von Zeichenfolgen usw.
>
> Für komplexere Abfragen verfügt Azure Cognitive Search im Parameter **search** über eine alternative [vollständige Lucene-Abfragesyntax](query-lucene-syntax.md). Weitere Informationen über die Abfrageanalysearchitektur und die Vorteile der einzelnen Syntaxen finden Sie unter [Funktionsweise der Volltextsuche in der kognitiven Azure-Suche](search-lucene-query-architecture.md).

## <a name="invoke-simple-parsing"></a>Aufrufen der einfachen Analyse

Die einfache Syntax ist die Standardeinstellung. Der Aufruf ist nur notwendig, wenn Sie die Syntax von vollständig auf einfach zurücksetzen. Um die Syntax explizit festzulegen, verwenden Sie den `queryType`-Suchparameter. Gültige Werte sind `queryType=simple` oder `queryType=full`. Dabei ist `simple` der Standardwert, und `full` ruft den [vollständigen Lucene-Abfrageparser](query-lucene-syntax.md) für erweiterte Abfragen auf. 

## <a name="syntax-fundamentals"></a>Grundlagen der Syntax

Jeder Text mit einem oder mehreren Begriffen gilt als gültiger Ausgangspunkt für die Ausführung der Abfrage. Die kognitive Azure-Suche findet Dokumente, die einen oder alle der Begriffe enthalten, einschließlich aller Variationen, die bei der Analyse des Textes gefunden wurden.

So einfach das klingt, es gibt einen Aspekt der Abfrageausführung in der kognitiven Azure-Suche, der *möglicherweise* unerwartete Ergebnisse liefert, die die Suchergebnisse eher erhöhen als verringern, da mehr Begriffe und Operatoren zur Eingabezeichenkette hinzugefügt werden. Ob diese Erweiterung tatsächlich erfolgt, hängt von der Einbeziehung eines NOT-Operators ab, kombiniert mit einer **searchMode**-Parametereinstellung, die bestimmt, wie NOT in Form von AND- oder OR-Verhalten interpretiert wird. Weitere Informationen finden Sie unter [NOT-Operator](#not-operator).

### <a name="precedence-operators-grouping"></a>Rangfolgenoperatoren (Gruppierung)

Sie können mithilfe von Klammern Unterabfragen erstellen, die Operatoren innerhalb der Anweisung in Klammern enthalten. Beispielsweise sucht `motel+(wifi||luxury)` nach Dokumenten, die den Begriff „motel“ und entweder „wifi“ oder „luxury“ (oder beides) enthalten.

Die Feldgruppierung funktioniert ähnlich, beschränkt die Gruppierung jedoch auf ein einzelnes Feld. Beispielsweise durchsucht `hotelAmenities:(gym+(wifi||pool))` das Feld „hotelAmenities“ nach „gym“ und „wifi“ oder nach „gym“ und „pool“.  

### <a name="escaping-search-operators"></a>Escaping-Suchoperatoren  

Wenn Sie einen der Suchoperatoren in Suchtext einbeziehen möchten, machen Sie ihn zu einem Escapezeichen, indem Sie davor einen einzelnen umgekehrten Schrägstrich (`\`) setzen. So würden Sie beispielsweise für eine Platzhaltersuche nach `https://`, in der `://` ein Teil der Abfragezeichenfolge ist, `search=https\:\/\/*` angeben. Entsprechend könnte ein Telefonnummernmuster mit Escapezeichen so aussehen: `\+1 \(800\) 642\-7676`.

Zu den Sonderzeichen, die Escapezeichen erfordern, gehören die folgenden: `- * ? \ /`  

Um es für die typischeren Fälle einfach zu machen, gibt es zwei Ausnahmen von dieser Regel, bei denen ein Escape nicht erforderlich ist:  

+ Der NOT-Operator `-` muss nur dann escaped werden, wenn es sich um das erste Zeichen nach Leerzeichen handelt, nicht, wenn es sich um die Mitte eines Begriffs handelt. So ist beispielsweise die folgende GUID ohne Escapezeichen gültig: `3352CDD0-EF30-4A2E-A512-3B30AF40F3FD`.

+ Der Suffix-Operator `*` muss nur dann escaped werden, wenn es sich um das letzte Zeichen vor dem Leerzeichen handelt, nicht, wenn es sich um die Mitte eines Begriffs handelt. Beispielsweise erfordert `4*4=16` keinen umgekehrten Schrägstrich.

> [!NOTE]  
> Obwohl Token durch die Verwendung von Escapezeichen zusammengehalten werden, können sie durch eine [lexikalische Analyse](search-lucene-query-architecture.md#stage-2-lexical-analysis) während der Indizierung eventuell entfernt werden. Die Lucene-Standardanalyse löscht und teilt z. B. Wörter an Bindestrichen, Leerzeichen und anderen Zeichen auf. Wenn in der Abfragezeichenfolge Sonderzeichen erforderlich sind, benötigen Sie möglicherweise ein Analysetool, das sie im Index beibehält. Zu den Optionen gehören Microsoft Natural [Language Analyzer](index-add-language-analyzers.md) (Analysetool für natürliche Sprache), bei dem Wörter mit Bindestrichen beibehalten werden, oder ein benutzerdefiniertes Analysetool für komplexere Muster. Weitere Informationen finden Sie unter [Teilausdrücke, Muster und Sonderzeichen](search-query-partial-matching.md).

### <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>Codierung von unsicheren und reservierten Zeichen in URLs

Stellen Sie sicher, dass alle unsicheren und reservierten Zeichen in einer URL codiert werden. „#“ ist beispielsweise ein Fragment-/Ankerbezeichner in einer URL und deshalb ein unsicheres Zeichen. Bei der Verwendung in einer URL muss das Zeichen in `%23` codiert werden. „&“ und „=“ sind Beispiele für reservierte Zeichen, da sie in Azure Cognitive Search zum Trennen von Parametern und Angeben von Werten dienen. Weitere Informationen finden Sie unter [RFC1738: Uniform Resource Locator (URL)](https://www.ietf.org/rfc/rfc1738.txt).

Unsichere Zeichen sind ``" ` < > # % { } | \ ^ ~ [ ]``. Reservierte Zeichen sind `; / ? : @ = + &`.

###  <a name="query-size-limits"></a><a name="bkmk_querysizelimits"></a> Abfragegrößenlimits

 Die Größe der Abfragen, die Sie an Azure Cognitive Search senden können, ist begrenzt. Insbesondere können Sie maximal 1.024 Klauseln (durch AND, OR usw. getrennte Ausdrücke) verwenden. Für die Größe der einzelnen Begriffe in einer Abfrage gilt zudem ein Grenzwert von ungefähr 32 KB. Wenn Ihre Anwendung programmgesteuert Suchabfragen generiert, sollten Sie durch den Anwendungsentwurf sicherstellen, dass sie keine Abfragen unbegrenzter Größe erzeugt.  

## <a name="boolean-search"></a>Boolesche Suche

Sie können boolesche Operatoren (AND, OR, NOT) in eine Abfragezeichenfolge einbetten, um einen umfangreichen Satz von Kriterien zu erstellen, anhand derer passende Dokumente gefunden werden. 

### <a name="and-operator-"></a>AND-Operator `+`

Der AND-Operator ist ein Pluszeichen (+). Zum Beispiel sucht `wifi+luxury` nach Dokumenten, die sowohl `wifi` als auch `luxury` enthalten.

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

+ [Abfragebeispiele für die einfache Suche](search-query-simple-examples.md)
+ [Abfragebeispiele für die vollständige Lucene-Suche](search-query-lucene-examples.md)
+ [Suchen von Dokumenten &#40;Azure Cognitive Search-REST-API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
+ [Lucene-Abfragesyntax](query-lucene-syntax.md)
+ [OData-Ausdruckssyntax](query-odata-filter-orderby-syntax.md) 
