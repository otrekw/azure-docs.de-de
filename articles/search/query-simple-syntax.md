---
title: Einfache Abfragesyntax
titleSuffix: Azure Cognitive Search
description: Referenz für die einfache Abfragesyntax, die für Volltextsuchabfragen in der kognitiven Azure-Suche verwendet wird.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/14/2020
ms.openlocfilehash: f679d6fbab57bcbcccc09b722f6b2f670df49eb2
ms.sourcegitcommit: 66479d7e55449b78ee587df14babb6321f7d1757
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2020
ms.locfileid: "97516585"
---
# <a name="simple-query-syntax-in-azure-cognitive-search"></a>Einfache Abfragesyntax in der kognitiven Azure-Suche

Die kognitive Azure-Suche implementiert zwei Lucene-basierte Abfragesprachen: [Einfacher Abfrageparser](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html) und der [Lucene-Abfrageparser](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). Der einfache Parser ist flexibler und versucht auch dann, eine Anforderung zu interpretieren, wenn sie nicht perfekt zusammengesetzt ist. Aufgrund dieser Flexibilität ist er die Standardeinstellung für Abfragen in Azure Cognitive Search.

Die einfache Syntax wird für Abfrageausdrücke verwendet, die im Parameter **`search`** einer Anforderung zum [Durchsuchen von Dokumenten (REST-API)](/rest/api/searchservice/search-documents) übergeben werden. Sie ist nicht zu verwechseln mit der [OData-Syntax](query-odata-filter-orderby-syntax.md), die für die Ausdrücke [ **`$filter`**](search-filters.md) und [ **`$orderby`** ](search-query-odata-orderby.md) in derselben Anforderung verwendet wird. Für OData-Parameter gelten eine andere Syntax und andere Regeln für das Erstellen von Abfragen, das Auskommentieren von Zeichenfolgen usw.

Der einfache Parser basiert zwar auf der [Apache Lucene Simple Query Parser](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html)-Klasse, die Implementierung in Cognitive Search schließt jedoch die Fuzzysuche aus. Wenn Sie die [Fuzzysuche](search-query-fuzzy.md) benötigen, sollten Sie stattdessen die alternative [vollständige Lucene-Abfragesyntax](query-lucene-syntax.md) in Betracht ziehen.

## <a name="example-simple-syntax"></a>Beispiel (einfache Syntax)

Der Parameter **`queryType`** ist zwar unten festgelegt, aber dies ist die Standardeinstellung und kann weggelassen werden, sofern Sie nicht einen alternativen Typ wiederherstellen. Im folgenden Beispiel wird eine Suche nach unabhängigen Begriffen durchgeführt, wobei alle übereinstimmenden Dokumente das Wort „Pool“ enthalten müssen.

```http
POST https://{{service-name}}.search.windows.net/indexes/hotel-rooms-sample/docs/search?api-version=2020-06-30
{
  "queryType": "simple",
  "search": "budget hotel +pool",
  "searchMode": "all"
}
```

Der Parameter **`searchMode`** ist in diesem Beispiel relevant. Wenn boolesche Operatoren in der Abfrage enthalten sind, sollten Sie im Allgemeinen `searchMode=all` festlegen, um sicherzustellen, dass *alle* Kriterien berücksichtigt werden. Andernfalls können Sie die Standardeinstellung `searchMode=any` verwenden, die den Abruf gegenüber der Genauigkeit bevorzugt.

Weitere Beispiele finden Sie unter [Beispiele für die einfache Abfragesyntax](search-query-simple-examples.md). Ausführliche Informationen zur Abfrageanforderung und zu den entsprechenden Parametern finden Sie unter [Durchsuchen von Dokumenten (REST-API)](/rest/api/searchservice/Search-Documents).

## <a name="keyword-search-on-terms-and-phrases"></a>Stichwortsuche nach Begriffen und Ausdrücken

Zeichenfolgen, die an den Parameter **`search`** übergeben werden, können Begriffe oder Ausdrücke in jeder unterstützten Sprache, boolesche Operatoren, Rangfolgenoperatoren, Platzhalter- oder Präfixzeichen für „Beginnt mit“-Abfragen, Escapezeichen und URL-Codierungszeichen enthalten. Der Parameter **`search`** ist optional. Wenn keine Angabe erfolgt, werden bei der Suche (`search=*` oder `search=" "`) die ersten 50 Dokumente in beliebiger Reihenfolge (ohne Rangfolge) zurückgegeben.

+ Eine *Begriffsuche* ist eine Abfrage mit einem oder mehreren Begriffen, von denen jeder der Begriffe als Übereinstimmung betrachtet wird.

+ Eine *Ausdruckssuche* ist ein in Anführungszeichen (`" "`) eingeschlossener exakter Ausdruck. Beispiel: ```Roach Motel``` (ohne Anführungszeichen) sucht nach Dokumenten, die ```Roach``` oder ```Motel``` enthalten, und zwar überall in beliebiger Reihenfolge; ```"Roach Motel"``` (mit Anführungszeichen) stimmt nur mit Dokumenten überein, die den gesamten Ausdruck zusammen und in dieser Reihenfolge enthalten (die lexikalische Analyse gilt weiterhin). 

  Abhängig von Ihrem Suchclient müssen Sie die Anführungszeichen in einer Ausdruckssuche möglicherweise mit Escapezeichen versehen. Beispiel: In Postman würde in einer POST-Anforderung eine Ausdruckssuche nach `"Roach Motel"` im Anforderungstext mit `"\"Roach Motel\""` angegeben werden.

Alle im Parameter **`search`** übergebenen Begriffe oder Ausdrücke werden standardmäßig einer lexikalischen Analyse unterzogen. Sie müssen daher unbedingt das Tokenisierungsverhalten des von Ihnen verwendeten Analysetools verstehen. Wenn Abfrageergebnisse unerwartet ausfallen, kann der Grund dafür häufig darauf zurückzuführen sein, wie Begriffe zur Abfragezeit tokenisiert werden.

Jeder Text mit einem oder mehreren Begriffen gilt als gültiger Ausgangspunkt für die Ausführung der Abfrage. Die kognitive Azure-Suche findet Dokumente, die einen oder alle der Begriffe enthalten, einschließlich aller Variationen, die bei der Analyse des Textes gefunden wurden.

So einfach das klingt, es gibt einen Aspekt der Abfrageausführung in der kognitiven Azure-Suche, der *möglicherweise* unerwartete Ergebnisse liefert, die die Suchergebnisse eher erhöhen als verringern, da mehr Begriffe und Operatoren zur Eingabezeichenkette hinzugefügt werden. Ob diese Erweiterung tatsächlich erfolgt, hängt von der Einbeziehung eines NOT-Operators ab, kombiniert mit einer **`searchMode`** -Parametereinstellung, die bestimmt, wie NOT in Form von AND- oder OR-Verhalten interpretiert wird. Weitere Informationen zur Verwendung des NOT-Operators finden Sie unter [Boolesche Operatoren](#boolean-operators).

## <a name="boolean-operators"></a>Boolesche Operatoren

Sie können boolesche Operatoren in eine Abfragezeichenfolge einbetten, um die Genauigkeit einer Übereinstimmung zu erhöhen. In der einfachen Syntax sind boolesche Operatoren zeichenbasiert. Textoperatoren (wie z. B. das Wort UND) werden nicht unterstützt.

| Zeichen | Beispiel | Verwendung |
|----------- |--------|-------|
| `+` | `pool + ocean` | Ein UND-Vorgang. Beispiel: Mit `pool + ocean` wird vorgegeben, dass ein Dokument beide Begriffe enthalten muss.|
| `|` | `pool | ocean` | Ein ODER-Vorgang findet eine Übereinstimmung, wenn einer der beiden Begriffe gefunden wird. In dem Beispiel gibt das Abfragemodul eine Übereinstimmung für Dokumente zurück, die entweder `pool` oder `ocean` oder beide Begriffe enthalten. Da OR der standardmäßige Konjunktionsoperator ist, könnten Sie ihn auch weglassen, d. h. `pool ocean` entspricht `pool | ocean`.|
| `-` | `pool – ocean` | Ein NICHT-Vorgang gibt Übereinstimmungen für Dokumente zurück, die den Begriff ausschließen. <br/><br/>Um das erwartete Verhalten für einen NOT-Ausdruck zu erhalten, sollten Sie in der Anforderung **`searchMode=all`** festlegen. Andernfalls erhalten Sie bei der Standardeinstellung **`searchMode=any`** Übereinstimmungen mit `pool` sowie Übereinstimmungen mit allen Dokumenten, die nicht den Begriff `ocean` enthalten, und das können viele Dokumente sein. Der Parameter **`searchMode`** in einer Abfrageanforderung steuert, ob ein Begriff mit dem NOT-Operator mit anderen Begriffen in der Abfrage per AND oder OR verknüpft wird (vorausgesetzt, dass es keinen Operator „`+`“ oder „`|`“ für die anderen Begriffe gibt). **`searchMode=all`** erhöht die Genauigkeit von Abfragen, weil weniger Ergebnisse einbezogen werden und „-“ standardmäßig als „AND NOT“ interpretiert wird. <br/><br/>Berücksichtigen Sie bei der Entscheidung über eine **`searchMode`** -Einstellung die Benutzerinteraktionsmuster für Abfragen in verschiedenen Anwendungen. Benutzer, die nach Informationen suchen, beziehen eher einen Operator in eine Abfrage mit ein – im Gegensatz zu e-Commerce-Websites mit mehr integrierten Navigationsstrukturen. |

<a name="prefix-search"></a>

## <a name="prefix-queries"></a>Präfixabfragen

Fügen Sie bei „Beginnt mit“-Abfragen einen Suffixoperator (`*`) als Platzhalter für den Rest eines Begriffs hinzu. Eine Präfixabfrage muss mit mindestens einem alphanumerischen Zeichen beginnen, damit Sie den Suffixoperator hinzufügen können.

| Zeichen | Beispiel | Verwendung |
|----------- |--------|-------|
| `*` | `lingui*` ergibt eine Übereinstimmung mit „linguistisch“ oder „linguini“. | Das Sternchen (`*`) stellt ein oder mehrere Zeichen beliebiger Länge dar, wobei die Groß-/Kleinschreibung ignoriert wird.  |

Ähnlich wie Filter sucht eine Präfixabfrage nach einer genauen Übereinstimmung. Daher wird die Relevanz nicht bewertet (alle Ergebnisse erhalten die Suchbewertung 1,0). Achten Sie darauf, dass Präfixabfragen insbesondere dann langsam sein können, wenn der Index groß ist und das Präfix aus einer kleinen Anzahl von Zeichen besteht. Eine alternative Methode, z. B. die Edge-N-Gramm-Tokenisierung, kann möglicherweise schneller ausgeführt werden.

Die einfache Syntax unterstützt nur den Abgleich von Präfixen. Verwenden Sie bei Suffix- oder Infixabgleichen mit dem Ende oder der Mitte eines Begriffs die [vollständige Lucene-Syntax für die Platzhaltersuche](query-lucene-syntax.md#bkmk_wildcard).

## <a name="escaping-search-operators"></a>Escaping-Suchoperatoren  

In der einfachen Syntax schließen die Suchoperatoren die folgenden Zeichen ein: `+ | " ( ) ' \`  

Wenn eines dieser Zeichen Teil eines Tokens im Index ist, versehen Sie es mit einem einzelnen umgekehrten Schrägstrich (`\`) als Escapezeichen in der Abfrage. Angenommen, Sie haben eine benutzerdefinierte Analyse für die gesamte begriffsbasierte Tokenisierung verwendet, und Ihr Index enthält die Zeichenfolge „Luxury+Hotel“. Fügen Sie ein Escapezeichen ein, um eine genaue Übereinstimmung für dieses Token zu erhalten: `search=luxury\+hotel`.

Zur Vereinfachung für typischere Fälle gibt es zwei Ausnahmen von dieser Regel, bei denen kein Escapezeichen erforderlich ist:  

+ Der NOT-Operator `-` muss nur dann mit einem Escapezeichen versehen werden, wenn es sich um das erste Zeichen nach einem Leerzeichen handelt. Wenn das `-` in der Mitte auftritt (z. B. in `3352CDD0-EF30-4A2E-A512-3B30AF40F3FD`), können Sie das Escapezeichen auslassen.

+ Der Suffix-Operator `*` muss nur dann mit einem Escapezeichen versehen werden, wenn es sich um das letzte Zeichen vor einem Leerzeichen handelt. Wenn das `*` in der Mitte auftritt (z. B. in `4*4=16`), ist kein Escapezeichen erforderlich.

> [!NOTE]  
> Die Lucene-Standardanalyse löscht bei der [lexikalischen Analyse](search-lucene-query-architecture.md#stage-2-lexical-analysis) standardmäßig Bindestriche, Leerzeichen, kaufmännische Und-Zeichen und andere Zeichen in Wörtern und teilt die Wörter an diesen Stellen auf. Wenn Sonderzeichen in der Abfragezeichenfolge beibehalten werden müssen, benötigen Sie möglicherweise ein Analysetool, das sie im Index belässt. Zu den Optionen gehören Microsoft Natural [Language Analyzer](index-add-language-analyzers.md) (Analysetool für natürliche Sprache), bei dem Wörter mit Bindestrichen beibehalten werden, oder ein benutzerdefiniertes Analysetool für komplexere Muster. Weitere Informationen finden Sie unter [Teilausdrücke, Muster und Sonderzeichen](search-query-partial-matching.md).

## <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>Codierung von unsicheren und reservierten Zeichen in URLs

Stellen Sie sicher, dass alle unsicheren und reservierten Zeichen in einer URL codiert werden. „#“ ist beispielsweise ein Fragment-/Ankerbezeichner in einer URL und deshalb ein unsicheres Zeichen. Bei der Verwendung in einer URL muss das Zeichen in `%23` codiert werden. „&“ und „=“ sind Beispiele für reservierte Zeichen, da sie in Azure Cognitive Search zum Trennen von Parametern und Angeben von Werten dienen. Weitere Informationen finden Sie unter [RFC1738: Uniform Resource Locators (URL)](https://www.ietf.org/rfc/rfc1738.txt).

Unsichere Zeichen sind ``" ` < > # % { } | \ ^ ~ [ ]``. Reservierte Zeichen sind `; / ? : @ = + &`.

## <a name="special-characters"></a>Sonderzeichen

Unter bestimmten Umständen möchten Sie möglicherweise nach einem Sonderzeichen suchen, z. B. nach dem Emoji ❤ oder dem €-Zeichen. Stellen Sie in solchen Fällen sicher, dass das verwendete Analysetool diese Zeichen nicht herausfiltert. Das Standardanalysetool umgeht viele Sonderzeichen und schließt sie aus Ihrem Index aus.

Zu den Analysetools, die Sonderzeichen tokenisieren, gehört das Leerzeichen-Analysetool, das alle durch Leerzeichen getrennten Zeichensequenzen als Token berücksichtigt (sodass die Zeichenfolge „❤“ als Token angesehen würde). Ein Sprachanalysetool wie das Microsoft-Analysetool für englische Sprache („en.microsoft“) würde auch die Zeichenfolge „€“ als Token ansehen. Sie können [ein Analysetool testen](/rest/api/searchservice/test-analyzer), um herauszufinden, welche Token für eine bestimmte Abfrage generiert werden.

Wenn Sie Unicode-Zeichen verwenden, stellen Sie sicher, dass die Symbole in der Abfrage-URL ordnungsgemäß mit Escapezeichen versehen werden (z. B. muss für „❤“ die Escapefolge `%E2%9D%A4+` verwendet werden). Postman übernimmt diese Übersetzung automatisch.  

## <a name="precedence-grouping"></a>Rangfolge (Gruppierung)

Sie können mithilfe von Klammern Unterabfragen erstellen, die Operatoren innerhalb der Anweisung in Klammern enthalten. Beispielsweise sucht `motel+(wifi|luxury)` nach Dokumenten, die den Begriff „motel“ und entweder „wifi“ oder „luxury“ (oder beides) enthalten.

## <a name="query-size-limits"></a> Abfragegrößenlimits

Wenn Ihre Anwendung programmgesteuert Suchabfragen generiert, sollten Sie durch den Anwendungsentwurf sicherstellen, dass sie keine Abfragen unbegrenzter Größe erzeugt. 

+ Bei GET-Anforderungen darf die Länge der URL 8 KB nicht überschreiten.

+ Bei POST-Anforderungen (und allen anderen Anforderungen), bei denen der Anforderungstext `search` und andere Parameter wie `filter` und `orderby` enthält, beträgt die maximale Größe 16 MB, und die maximale Anzahl von Klauseln in `search` (durch AND, OR usw. getrennte Ausdrücke) beträgt 1024. Für die Größe der einzelnen Begriffe in einer Abfrage gilt zudem ein Grenzwert von ungefähr 32 KB. Weitere Informationen finden Sie unter [API-Anforderungsgrenzwerte](search-limits-quotas-capacity.md#api-request-limits).

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Abfragen programmgesteuert erstellen, lesen Sie [Volltextsuche in Azure Cognitive Search](search-lucene-query-architecture.md), um die Phasen der Abfrageverarbeitung und die Auswirkungen der Textanalyse zu verstehen.

Sie können auch die folgenden Artikel lesen, um mehr über das Erstellen von Abfragen zu erfahren:

+ [Abfragebeispiele für die einfache Suche](search-query-simple-examples.md)
+ [Abfragebeispiele für die vollständige Lucene-Suche](search-query-lucene-examples.md)
+ [Search Documents (Azure Search Service REST API)](/rest/api/searchservice/Search-Documents) (Suchen nach Dokumenten (Azure Search Service-REST-API))
+ [Lucene-Abfragesyntax](query-lucene-syntax.md)
+ [OData-Ausdruckssyntax für Filter- und Auswahlausdrücke](query-odata-filter-orderby-syntax.md)