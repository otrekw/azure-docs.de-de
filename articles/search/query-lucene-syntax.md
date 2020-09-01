---
title: Lucene-Abfragesyntax
titleSuffix: Azure Cognitive Search
description: Referenz der vollständigen Lucene-Abfragesyntax, die in Azure Cognitive Search für Platzhalter, Fuzzysuche, RegEx und andere erweiterte Abfragekonstrukte verwendet wird.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/23/2020
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 6ea8bc2551df4f85e4b856dc9cf1c06a9bd571fd
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88923448"
---
# <a name="lucene-query-syntax-in-azure-cognitive-search"></a>Lucene-Abfragesyntax in Azure Cognitive Search

Sie können für Azure Cognitive Search basierend auf der umfassenden Syntax des [Lucene-Abfrageparsers](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html) spezielle Abfragen schreiben: Platzhaltersuche, Fuzzysuche, NEAR-Suche und Suche mit regulären Ausdrücken sind einige Beispiele hierfür. Der Großteil der Syntax des Lucene-Abfrageparsers wird [in Azure Cognitive Search unverändert implementiert](search-lucene-query-architecture.md). Die einzige Ausnahme sind *Bereichssuchen*, die in Azure Cognitive Search mit `$filter`-Ausdrücken erstellt werden. 

> [!NOTE]
> Die vollständige Lucene-Syntax wird für Abfrageausdrücke verwendet, die im **search**-Parameter der [Dokumente durchsuchen](/rest/api/searchservice/search-documents)-API übergeben werden. Sie sollte nicht mit der [OData-Syntax](query-odata-filter-orderby-syntax.md) verwechselt werden, die für den [$filter](search-filters.md)-Parameter dieser API verwendet wird. Für diese unterschiedlichen Syntaxen gelten eigene Regeln für das Erstellen von Abfragen, das Auskommentieren von Zeichenfolgen usw.

## <a name="invoke-full-parsing"></a>Aufrufen der vollständigen Analyse

Legen Sie den Suchparameter `queryType` fest, um den gewünschten Parser anzugeben. Gültige Werte sind `simple|full` (der Standardwert ist `simple`) und `full` für Lucene. 

<a name="bkmk_example"></a> 

### <a name="example-showing-full-syntax"></a>Beispiel für die vollständige Syntax

Im folgenden Beispiel wird mithilfe der Lucene-Abfragesyntax (dies wird im Parameter `queryType=full` deutlich) eine Suche nach Dokumenten im Index durchgeführt. Bei dieser Abfrage werden Hotels zurückgegeben, bei denen das Kategoriefeld den Begriff „budget“ enthält und alle durchsuchbaren Felder die Wörter „recently renovated“ enthalten. Dokumenten mit dem Ausdruck „recently renovated“ wird aufgrund des Werts für die Begriffsverstärkung (3) ein höherer Rang zugewiesen.  

Der Parameter `searchMode=all` ist in diesem Beispiel wichtig. Bei Abfragen mit Operatoren sollten Sie generell `searchMode=all` festlegen, um sicherzustellen, dass *alle* Kriterien abgeglichen werden.

```
GET /indexes/hotels/docs?search=category:budget AND \"recently renovated\"^3&searchMode=all&api-version=2020-06-30&querytype=full
```

 Alternativ können Sie POST verwenden:  

```
POST /indexes/hotels/docs/search?api-version=2020-06-30
{
  "search": "category:budget AND \"recently renovated\"^3",
  "queryType": "full",
  "searchMode": "all"
}
```

Weitere Beispiele finden Sie unter [Beispiele für die Lucene-Abfragesyntax zum Erstellen von Abfragen in Azure Cognitive Search](search-query-lucene-examples.md). Ausführliche Informationen zur Angabe sämtlicher Abfrageparameter finden Sie unter [Durchsuchen von Dokumenten (REST-API für Azure Cognitive Search)](/rest/api/searchservice/Search-Documents).

> [!NOTE]  
>  Azure Cognitive Search unterstützt darüber hinaus die [einfache Abfragesyntax](query-simple-syntax.md), eine unkomplizierte und stabile Abfragesprache, die für die einfache Schlüsselwortsuche verwendet werden kann.  

##  <a name="syntax-fundamentals"></a><a name="bkmk_syntax"></a> Grundlagen der Syntax  

Die folgenden Syntaxgrundlagen gelten für alle Abfragen mit der Lucene-Syntax.  

### <a name="operator-evaluation-in-context"></a>Operatorauswertung im Kontext

Die Platzierung bestimmt, ob ein Symbol als Operator oder einfach als ein weiteres Zeichen in einer Zeichenfolge interpretiert wird.

In der vollständigen Lucene-Syntax wird das Tildezeichen (~) beispielsweise sowohl für die Fuzzysuche als auch die NEAR-Suche verwendet. Wenn das Tildezeichen (~) nach einem Ausdruck in Anführungszeichen steht, ruft es die NEAR-Suche auf. Steht es am Ende eines Begriffs, ruft es die Fuzzysuche auf.

In einem Begriff (z. B. „Business~Analyst“) wird das Zeichen nicht als Operator interpretiert. Sofern es sich um eine Begriffs- oder Ausdrucksabfrage handelt, wird in diesem Fall bei der [Volltextsuche](search-lucene-query-architecture.md) mit [lexikalischer Analyse](search-lucene-query-architecture.md#stage-2-lexical-analysis) das „~“ entfernt, und der Begriff „Business~Analyst“ wird in zwei Begriffe aufgeteilt: „Business“ ODER „Analyst“.

Im obigen Beispiel geht es um das Tildezeichen (~), das gleiche Prinzip gilt jedoch für alle Operatoren.

### <a name="escaping-special-characters"></a>Verwenden von Escapezeichen für Sonderzeichen

Wenn Sie einen der Suchoperatoren in Suchtext einbeziehen möchten, machen Sie ihn zu einem Escapezeichen, indem Sie davor einen einzelnen umgekehrten Schrägstrich (`\`) setzen. So würden Sie beispielsweise für eine Platzhaltersuche nach `https://`, in der `://` ein Teil der Abfragezeichenfolge ist, `search=https\:\/\/*` angeben. Entsprechend könnte ein Telefonnummernmuster mit Escapezeichen so aussehen: `\+1 \(800\) 642\-7676`.

Zu den Sonderzeichen, die Escapezeichen erfordern, gehören die folgenden:  
`+ - & | ! ( ) { } [ ] ^ " ~ * ? : \ /`  

> [!NOTE]  
> Obwohl Token durch die Verwendung von Escapezeichen zusammengehalten werden, können sie durch eine [lexikalische Analyse](search-lucene-query-architecture.md#stage-2-lexical-analysis) während der Indizierung eventuell entfernt werden. Die Lucene-Standardanalyse teilt z. B. Wörter an Bindestrichen, Leerzeichen und anderen Zeichen auf. Wenn in der Abfragezeichenfolge Sonderzeichen erforderlich sind, benötigen Sie möglicherweise ein Analysetool, das sie im Index beibehält. Zu den Optionen gehören Microsoft Natural [Language Analyzer](index-add-language-analyzers.md) (Analysetool für natürliche Sprache), bei dem Wörter mit Bindestrichen beibehalten werden, oder ein benutzerdefiniertes Analysetool für komplexere Muster. Weitere Informationen finden Sie unter [Teilausdrücke, Muster und Sonderzeichen](search-query-partial-matching.md).

### <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>Codierung von unsicheren und reservierten Zeichen in URLs

Stellen Sie sicher, dass alle unsicheren und reservierten Zeichen in einer URL codiert werden. „#“ ist beispielsweise ein Fragment-/Ankerbezeichner in einer URL und deshalb ein unsicheres Zeichen. Bei der Verwendung in einer URL muss das Zeichen in `%23` codiert werden. „&“ und „=“ sind Beispiele für reservierte Zeichen, da sie in Azure Cognitive Search zum Trennen von Parametern und Angeben von Werten dienen. Weitere Informationen finden Sie unter [RFC1738: Uniform Resource Locator (URL)](https://www.ietf.org/rfc/rfc1738.txt).

Unsichere Zeichen sind ``" ` < > # % { } | \ ^ ~ [ ]``. Reservierte Zeichen sind `; / ? : @ = + &`.

###  <a name="query-size-limits"></a><a name="bkmk_querysizelimits"></a> Abfragegrößenlimits

 Die Größe der Abfragen, die Sie an Azure Cognitive Search senden können, ist begrenzt. Insbesondere können Sie maximal 1.024 Klauseln (durch AND, OR usw. getrennte Ausdrücke) verwenden. Für die Größe der einzelnen Begriffe in einer Abfrage gilt zudem ein Grenzwert von ungefähr 32 KB. Wenn Ihre Anwendung programmgesteuert Suchabfragen generiert, sollten Sie durch den Anwendungsentwurf sicherstellen, dass sie keine Abfragen unbegrenzter Größe erzeugt.  

### <a name="precedence-operators-grouping"></a>Rangfolgenoperatoren (Gruppierung)

 Sie können mithilfe von Klammern Unterabfragen erstellen, die Operatoren innerhalb der Anweisung in Klammern enthalten. Beispielsweise sucht `motel+(wifi||luxury)` nach Dokumenten, die den Begriff „motel“ und entweder „wifi“ oder „luxury“ (oder beides) enthalten.

Die Feldgruppierung funktioniert ähnlich, beschränkt die Gruppierung jedoch auf ein einzelnes Feld. Beispielsweise durchsucht `hotelAmenities:(gym+(wifi||pool))` das Feld „hotelAmenities“ nach „gym“ und „wifi“ oder nach „gym“ und „pool“.  

##  <a name="boolean-search"></a><a name="bkmk_boolean"></a> Boolesche Suche

 Geben Sie boolesche Operatoren in Textform (AND, OR, NOT) immer in Großbuchstaben an.  

### <a name="or-operator-or-or-"></a>OR-Operator `OR` oder `||`

Der OR-Operator ist ein vertikaler Balken bzw. ein senkrechter Strich. Beispiel: `wifi || luxury` sucht nach Dokumenten, die „wifi“ oder „luxury“ oder beides enthalten. Da OR der standardmäßige Konjunktionsoperator ist, könnten Sie ihn auch weglassen, d. h. `wifi luxury` entspricht `wifi || luxury`.

### <a name="and-operator-and--or-"></a>AND-Operator `AND`, `&&` oder `+`

Der AND-Operator ist ein kaufmännisches Und-Zeichen oder ein Pluszeichen. Beispiel: `wifi && luxury` sucht nach Dokumenten, die sowohl „wifi“ als auch „luxury“ enthalten. Das Pluszeichen (+) wird für erforderliche Begriffe verwendet. Beispielsweise legt `+wifi +luxury` fest, dass beide Begriffe im Feld eines einzelnen Dokuments vorkommen müssen.

### <a name="not-operator-not--or--"></a>NOT-Operator `NOT`, `!` oder `-`

Der NOT-Operator ist ein Minuszeichen (-). Beispiel: `wifi –luxury` sucht nach Dokumenten, die den Begriff `wifi` enthalten und/oder `luxury` nicht enthalten.

Der Parameter **searchMode** in einer Abfrageanforderung steuert, ob ein Begriff mit dem NOT-Operator mit anderen Begriffen in der Abfrage per AND oder OR verknüpft wird (vorausgesetzt, dass es keinen Operator „`+`“ oder „`|`“ für die anderen Begriffe gibt). Gültige Werte sind `any` oder `all`.

`searchMode=any` erhöht die Trefferquote von Abfragen, weil mehr Ergebnisse einbezogen werden, und `-` wird standardmäßig als „OR NOT“ interpretiert. So stimmt beispielsweise `wifi -luxury` mit Dokumenten überein, die entweder den Begriff `wifi` enthalten oder die den Begriff `luxury` nicht enthalten.

`searchMode=all` erhöht die Genauigkeit von Abfragen, weil weniger Ergebnisse einbezogen werden und „-“ standardmäßig als „AND NOT“ interpretiert wird. So stimmt beispielsweise `wifi -luxury` mit Dokumenten überein, die den Begriff `wifi` enthalten und die nicht den Begriff „Luxus“ enthalten. Dies ist wohl ein intuitiveres Verhalten für den `-`-Operator. Daher sollten Sie die Verwendung von `searchMode=all` statt `searchMode=any` in Betracht ziehen, wenn Sie Suchen im Hinblick auf die Genauigkeit statt auf die Trefferquote optimieren möchten *und* Ihre Benutzer den `-`-Operator häufig bei Suchen verwenden.

Berücksichtigen Sie bei der Entscheidung über eine **searchMode**-Einstellung die Benutzerinteraktionsmuster für Abfragen in verschiedenen Anwendungen. Benutzer, die nach Informationen suchen, beziehen eher einen Operator in eine Abfrage mit ein – im Gegensatz zu e-Commerce-Websites mit mehr integrierten Navigationsstrukturen.

##  <a name="fielded-search"></a><a name="bkmk_fields"></a>Feldbezogene Suche

Sie können einen feldbezogenen Suchvorgang mit der `fieldName:searchExpression`-Syntax definieren, wobei es sich bei dem Suchausdruck um ein einzelnes Wort, einen einfachen Ausdruck oder einen komplexeren Ausdruck in Klammern handeln kann, optional mit booleschen Operatoren. Einige Beispiele für Änderungen sind in der folgenden Liste aufgeführt:  

- genre:jazz NOT history  

- artists:("Miles Davis" "John Coltrane")

Achten Sie darauf, dass Sie mehrere Zeichenfolgen in Anführungszeichen setzen, wenn beide Zeichenfolgen als einzelne Entität ausgewertet werden sollen (in diesem Fall die Suche nach zwei verschiedenen Künstlern im Feld `artists`).  

Das in `fieldName:searchExpression` angegebene Feld muss ein Feld vom Typ `searchable` sein.  Einzelheiten zur Verwendung von Indexattributen in Felddefinitionen finden Sie unter [Create Index](/rest/api/searchservice/create-index) (Erstellen eines Index).  

> [!NOTE]
> Bei der Verwendung von feldbezogenen Suchausdrücken brauchen Sie den Parameter `searchFields` nicht zu verwenden, da in jedem feldbezogenen Suchausdruck explizit ein Feldname angegeben ist. Allerdings können Sie den Parameter `searchFields` trotzdem verwenden, wenn Sie eine Abfrage ausführen möchten, bei der einige Teile auf ein bestimmtes Feld beschränkt sind, der Rest sich jedoch auf mehrere Felder beziehen kann. Zum Beispiel würde `jazz` in der Abfrage `search=genre:jazz NOT history&searchFields=description` nur mit dem Feld `genre`, und `NOT history` mit dem Feld `description` abgeglichen werden. Der in `fieldName:searchExpression` angegebene Feldname hat immer Vorrang vor dem Parameter `searchFields`, weshalb `genre` in diesem Beispiel nicht in den Parameter `searchFields` aufgenommen werden muss.

##  <a name="fuzzy-search"></a><a name="bkmk_fuzzy"></a> Fuzzysuche

Bei einer Fuzzysuche werden Übereinstimmungen mit einer ähnlichen Konstruktion gefunden, wobei ein Begriff bis zu maximal 50 Begriffen erweitert wird, die die Abstandskriterien von zwei oder weniger erfüllen. Weitere Informationen finden Sie unter [Fuzzysuche](search-query-fuzzy.md).

 Verwenden Sie für eine Fuzzysuche das Tildezeichen „~“ am Ende eines einzelnen Worts mit einem optionalen Parameter, einer Zahl zwischen 0 und 2 (Standardwert), der die Edit-Distanz angibt. Beispielsweise würden bei „blue~“ oder „blue~1“ die Werte „blue“, „blues“ und „glue“ zurückgegeben.

 Die Fuzzysuche kann nur auf Begriffe, nicht auf Ausdrücke, angewendet werden. Sie können aber die Tilde an jeden Begriff in einem mehrteiligen Namen oder Ausdruck einzeln anfügen. So würde beispielsweise „Unviersty~ of~ „Wshington~“ mit „University of Washington“ übereinstimmen.
 
##  <a name="proximity-search"></a><a name="bkmk_proximity"></a> NEAR-Suche

NEAR-Suchen werden verwendet, um Begriffe zu suchen, die in einem Dokument nahe beieinander liegen. Fügen Sie ein Tildesymbol „~“ Symbol am Ende eines Ausdrucks ein, gefolgt von der Anzahl der Wörter, die den NEAR-Bereich bilden. Beispielsweise finden Sie mit der Abfrage `"hotel airport"~5` die Begriffe „hotel“ und „airport“, wenn sie in einem Abstand von fünf Wörtern voneinander in einem Dokument vorkommen.  


##  <a name="term-boosting"></a><a name="bkmk_termboost"></a> Begriffsverstärkung

Die Begriffsverstärkung (Term Boosting) bezieht sich auf das Höherbewerten eines Dokuments, wenn es den verstärkten Begriff enthält, im Verhältnis zu Dokumenten, die den Begriff nicht enthalten. Dies unterscheidet sich insofern von Bewertungsprofilen, als dass bei Bewertungsprofilen bestimmte Felder statt bestimmter Begriffe verstärkt werden.  

Im folgenden Beispiel werden die Unterschiede veranschaulicht. Angenommen, Sie haben ein Bewertungsprofil, das Übereinstimmungen in einem bestimmten Feld verstärkt, beispielsweise *genre* im [musicstoreindex-Beispiel](index-add-scoring-profiles.md#bkmk_ex). Mit der Begriffsverstärkung könnten Sie bestimmte Suchbegriffe noch höher bewerten als andere. Mit `rock^2 electronic` werden beispielsweise Dokumente, die die Suchbegriffe im Feld „genre“ enthalten, höher eingestuft als andere durchsuchbare Felder im Index. Darüber hinaus wird Dokumenten, die den Suchbegriff *rock* enthalten, aufgrund des Werts für die Begriffsverstärkung (2) ein höherer Rang zugewiesen als Dokumenten mit dem anderen Suchbegriff *electronic*.  

 Verwenden Sie zum Verstärken eines Begriffs das Caretzeichen „^“ mit einem Verstärkungsfaktor (einer Zahl) am Ende des Begriffs, nach dem Sie suchen. Sie können auch Ausdrücke verstärken. Je höher der Verstärkungsfaktor, desto relevanter wird der Begriff im Verhältnis zu anderen Suchbegriffen. Der Standardverstärkungsfaktor ist 1. Der Verstärkungsfaktor muss positiv sein, kann aber kleiner als 1 sein (z. B. 0.20).  

##  <a name="regular-expression-search"></a><a name="bkmk_regex"></a> Suche mit regulären Ausdrücken  
 Bei einer Suche mit regulären Ausdrücken werden Übereinstimmungen basierend auf unter Apache Lucene gültigen Mustern gefunden, wie in der [RegExp-Klasse](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/util/automaton/RegExp.html) dokumentiert. In Azure Cognitive Search wird ein regulärer Ausdruck in Schrägstriche `/` eingeschlossen.

 Geben Sie beispielsweise `/[mh]otel/` an, um nach Dokumenten zu suchen, die das Wort „motel“ oder „hotel“ enthalten. Suchen mit regulären Ausdrücken werden mit einzelnen Wörtern abgeglichen.

Für einige Tools und Sprachen müssen zusätzlich Escapezeichen verwendet werden. Bei JSON werden Zeichenfolgen, die einen Schrägstrich enthalten, mit einem Escapezeichen in Form eines umgekehrten Schrägstrichs versehen: „microsoft.com/azure/“ wird zu `search=/.*microsoft.com\/azure\/.*/`, wobei `search=/.* <string-placeholder>.*/` dem regulären Ausdruck und `microsoft.com\/azure\/` der Zeichenfolge mit einem Escapezeichen in Form eines Schrägstrichs entspricht.

##  <a name="wildcard-search"></a><a name="bkmk_wildcard"></a> Platzhaltersuche

Sie können die allgemein bekannte Syntax für die Platzhaltersuche nach mehreren (`*`) oder einzelnen (`?`) Zeichen verwenden. Beispielsweise gibt der Abfrageausdruck `search=alpha*` „alphanumerisch“ und „alphabetisch“ zurück. Beachten Sie, dass der Lucene-Abfrageparser die Verwendung dieser Symbole bei einem einzelnen Begriff, nicht bei einem Ausdruck, unterstützt.

Die vollständige Lucene-Syntax unterstützt Präfix-, Infix- und Suffixvergleiche. Wenn Sie jedoch nur eine Präfixübereinstimmung benötigen, können Sie die einfache Syntax verwenden. (Die Präfixübereinstimmung wird in beiden Fällen unterstützt.)

Für die Suffixübereinstimmung, bei denen der Zeichenfolge `*` oder `?` vorangestellt ist (z. B. in `search=/.*numeric./`), oder die Infixübereinstimmung, sind die vollständige Lucene-Syntax sowie das Schrägstrich-Trennzeichen `/` für reguläre Ausdrücke erforderlich. Sie können die Symbole * oder ? nicht ohne `/` als erstes Zeichen eines Begriffs oder innerhalb eines Begriffs verwenden. 

> [!NOTE]  
> Grundsätzlich ist der Musterabgleich langsam, sodass Sie möglicherweise alternative Methoden untersuchen sollten, z. B. Edge-N-Gramm-Tokenisierung, mit der Token für Zeichenfolgen in einem Begriff erstellt werden. Der Index wird damit größer, Abfragen werden jedoch möglicherweise schneller ausgeführt, je nach der Form des Musters und der Länge der Zeichenfolgen, die Sie indizieren.
>

### <a name="impact-of-an-analyzer-on-wildcard-queries"></a>Auswirkung eines Analysetools auf Platzhalterabfragen

Während der Abfrageanalyse werden Abfragen, die als Präfix-, Suffix-, Platzhaltersuche oder reguläre Ausdrücke formuliert werden, unverändert an die Abfragestruktur übergeben, wobei die [lexikalische Analyse](search-lucene-query-architecture.md#stage-2-lexical-analysis) umgangen wird. Es werden nur Übereinstimmungen gefunden, wenn der Index die Zeichenfolgen in dem in der Abfrage angegebenen Format enthält. In den meisten Fällen benötigen Sie während der Indizierung ein Analysetool, das die Integrität der Zeichenfolgen beibehält, damit der Abgleich von Teilausdrücken und -mustern gelingt. Weitere Informationen finden Sie im Thema zur [Suche nach Teilausdrücken in Azure Cognitive Search-Abfragen](search-query-partial-matching.md).

Angenommen, Sie möchten, dass die Suchabfrage „beend*“ Ergebnisse liefert, die Begriffe wie „beenden“, „Beendigung“ und „beendet“ enthalten.

Wenn Sie das Analysetool „de.lucene“ (deutsches Lucene) verwendeten, würde es eine aggressive Wortstammerkennung der einzelnen Begriffe anwenden. Beispielsweise werden „beenden“, „Beendigung“ und „beendet“ alle auf das Token „beend" in Ihrem Index tokenisiert. Einerseits werden Begriffe in Abfragen mit Platzhaltern oder Fuzzysuche überhaupt nicht analysiert. Deshalb gäbe es keine Ergebnisse, wenn die Abfrage „beend*" zum Abgleich verwendet würde.

Andererseits sind die Microsoft-Analysetools (in diesem Fall das Analysetool „de.microsoft“) etwas komplexer und verwenden Lemmatisierung statt Wortstammerkennung. Dies bedeutet, dass alle generierten Token gültige deutsche Wörter sein sollten. Beispielsweise bleiben „beenden“', „beendet“ und „Beendigung“ meistens ganz im Index und wären eine bevorzugte Wahl bei Szenarien, die sehr viel auf Platzhalter und Fuzzysuche angewiesen sind.

##  <a name="scoring-wildcard-and-regex-queries"></a><a name="bkmk_searchscoreforwildcardandregexqueries"></a> Bewerten von Platzhalterabfragen und Abfragen mit regulären Ausdrücken

Azure Cognitive Search verwendet für Textabfragen die häufigkeitsbasierte Bewertung ([TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)). Für Platzhalterabfragen und Abfragen mit regulären Ausdrücken, bei denen die Anzahl von Begriffen groß sein kann, wird der Häufigkeitsfaktor jedoch ignoriert. Dadurch wird verhindert, dass Übereinstimmungen für seltenere Begriffe bei der Rangzuweisung bevorzugt behandelt werden. Alle Übereinstimmungen werden bei Platzhalterabfragen und Abfragen mit regulären Ausdrücken gleich behandelt.

## <a name="see-also"></a>Weitere Informationen

+ [Abfragebeispiele für die einfache Suche](search-query-simple-examples.md)
+ [Abfragebeispiele für die vollständige Lucene-Suche](search-query-lucene-examples.md)
+ [Dokumente durchsuchen](/rest/api/searchservice/Search-Documents)
+ [OData expression syntax for filters and sorting](query-odata-filter-orderby-syntax.md) (OData-Ausdrucksfilter für Filter und die Sortierung)   
+ [Einfache Abfragesyntax in Azure Cognitive Search](query-simple-syntax.md)