---
title: Abgleichen von Mustern und Sonderzeichen
titleSuffix: Azure Cognitive Search
description: Verwenden Sie Platzhalter- und Präfixabfragen, um in einer Azure Cognitive Search-Abfrageanforderung einen Abgleich für Ausdrücke oder Teile von Ausdrücken durchzuführen. Zum Lösen von Mustern mit Sonderzeichen, deren Abgleich schwierig ist, können eine vollständige Abfragesyntax und benutzerdefinierte Analysetools verwendet werden.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/14/2020
ms.openlocfilehash: f78ba5b351a3da46d7b8b3780cf00772c4f3b2ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80289310"
---
# <a name="match-on-patterns-and-special-characters-dashes"></a>Abgleichen von Mustern und Sonderzeichen (Bindestriche)

Für Abfragen mit Sonderzeichen (`-, *, (, ), /, \, =`) oder Abfragemuster, die auf Teilen eines längeren Ausdrucks basieren, sind normalerweise zusätzliche Konfigurationsschritte erforderlich, um sicherzustellen, dass der Index den erwarteten Inhalt im richtigen Format enthält. 

Eine Telefonnummer der Art `+1 (425) 703-6214` wird standardmäßig wie folgt tokenisiert: `"1"`, `"425"`, `"703"`, `"6214"`. Dies verdeutlicht, dass die Suche nach Teilen von Ausdrücken (hier beispielsweise nach `"3-62"`), die einen Bindestrich enthalten, nicht erfolgreich ist, weil diese Folge im Index nicht vorhanden ist. 

Wenn Sie nach Teilen von Zeichenfolgen oder Sonderzeichen suchen müssen, können Sie anstelle des Standardanalysetools ein benutzerdefiniertes Analysetool nutzen, für das vereinfachte Tokenisierungsregeln verwendet und die gesamten Ausdrücke beibehalten werden. Dies ist erforderlich, wenn Abfragezeichenfolgen Teile eines Ausdrucks oder Sonderzeichen enthalten. Der grundsätzliche Ansatz ist wie folgt:

+ Auswählen eines vordefinierten Analysetools oder Definieren eines benutzerdefinierten Analysetools für die Generierung der gewünschten Ausgabe
+ Zuweisen des Analysetools zum Feld
+ Erstellen des Index und Durchführen von Tests

In diesem Artikel werden diese Schritte beschrieben. Dieser Ansatz ist auch für andere Szenarien hilfreich: Für Abfragen mit Platzhaltern und regulären Ausdrücken werden als Basis für den Musterabgleich ebenfalls vollständige Ausdrücke benötigt. 

> [!TIP]
> Die Auswertung von Analysetools ist ein iterativer Prozess, bei dem der Index häufig neu erstellt werden muss. Sie können diesen Schritt vereinfachen, indem Sie Postman und die REST-APIs für [Index erstellen](https://docs.microsoft.com/rest/api/searchservice/create-index), [Index löschen](https://docs.microsoft.com/rest/api/searchservice/delete-index),[Dokumente laden](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) und [Dokumente durchsuchen](https://docs.microsoft.com/rest/api/searchservice/search-documents) verwenden. Für „Dokumente laden“ sollte der Anforderungstext ein kleines repräsentatives Dataset enthalten, das getestet werden soll (z. B. ein Feld mit Telefonnummern oder Produktcodes). Wenn diese APIs in derselben Postman-Sammlung enthalten sind, können Sie diese Schritte schnell durchlaufen.

## <a name="choosing-an-analyzer"></a>Auswählen eines Analysetools

Für die Generierung von Token mit vollständigen Ausdrücken werden häufig die folgenden Analysetools ausgewählt:

| Analysetool | Verhalten |
|----------|-----------|
| [keyword](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) | Der Inhalt des gesamten Felds wird als ein Ausdruck tokenisiert. |
| [whitespace](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/WhitespaceAnalyzer.html) | Die Trennung erfolgt nur anhand von Leerzeichen. Ausdrücke, die Bindestriche oder andere Zeichen enthalten, werden als einzelnes Token behandelt. |
| [Benutzerdefiniertes Analysetool](index-add-custom-analyzers.md) | (Empfohlen) Beim Erstellen eines benutzerdefinierten Analysetools können Sie sowohl den Tokenizer als auch den Tokenfilter angeben. Die obigen Analysetools müssen unverändert verwendet werden. Bei einem benutzerdefinierten Analysetool können Sie auswählen, welche Tokenizer und Tokenfilter genutzt werden sollen. <br><br>Eine empfohlene Kombination ist der [Schlüsselwort-Tokenizer](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordTokenizer.html) mit einem [Kleinbuchstaben-Tokenfilter](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/LowerCaseFilter.html). Ohne Filter wandelt das vordefinierte [Schlüsselwort-Analysetool](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) Großbuchstaben nicht in Kleinbuchstaben um. Dies kann dazu führen, dass Abfragen nicht erfolgreich sind. Bei einem benutzerdefinierten Analysetool erhalten Sie einen Mechanismus zum Hinzufügen des Kleinbuchstaben-Tokenfilters. |

Wenn Sie ein Web-API-Testtool wie Postman nutzen, können Sie den [REST-Aufruf für die Textanalyse](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) hinzufügen, um die tokenisierte Ausgabe zu untersuchen. Bei Verwendung eines vorhandenen Index und eines Felds mit Bindestrichen oder Teilen von Ausdrücken können Sie für bestimmte Ausdrücke verschiedene Analysetools einsetzen, um zu ermitteln, welche Token ausgegeben werden.  

1. Verwenden Sie das Standardanalysetool, um herauszufinden, wie Ausdrücke standardmäßig tokenisiert werden.

   ```json
   {
   "text": "SVP10-NOR-00",
   "analyzer": "standard"
   }
    ```

1. Werten Sie die Antwort aus, um zu ermitteln, wie der Text im Index tokenisiert wird. Beachten Sie, dass jeder Ausdruck nur Kleinbuchstaben enthält und unterteilt wird.

    ```json
    {
        "tokens": [
            {
                "token": "svp10",
                "startOffset": 0,
                "endOffset": 5,
                "position": 0
            },
            {
                "token": "nor",
                "startOffset": 6,
                "endOffset": 9,
                "position": 1
            },
            {
                "token": "00",
                "startOffset": 10,
                "endOffset": 12,
                "position": 2
            }
        ]
    }
    ```
1. Ändern Sie die Anforderung so, dass das Analysetool `whitespace` oder `keyword` verwendet wird:

    ```json
    {
    "text": "SVP10-NOR-00",
    "analyzer": "keyword"
    }
    ```

1. Die Antwort enthält nun ein einzelnes Token in Großbuchstaben, bei dem die Bindestriche der Zeichenfolge beibehalten wurden. Falls Sie nach einem Muster oder dem Teil eines Ausdrucks suchen müssen, verfügt die Abfrage-Engine jetzt über die Grundlage für die Ermittlung einer Übereinstimmung.


    ```json
    {

        "tokens": [
            {
                "token": "SVP10-NOR-00",
                "startOffset": 0,
                "endOffset": 12,
                "position": 0
            }
        ]
    }
    ```
> [!Important]
> Hierbei ist zu beachten, dass Abfrageparser beim Erstellen der Abfragestruktur die Ausdrücke eines Suchbegriffs häufig in Kleinbuchstaben umwandeln. Bei Verwendung eines Analysetools, bei dem Texteingaben nicht in Kleinbuchstaben umgewandelt werden, kann dies der Grund dafür sein, warum Sie nicht die erwarteten Ergebnisse erhalten. Die Lösung besteht darin, einen Kleinbuchstaben-Tokenfilter hinzuzufügen.

## <a name="analyzer-definitions"></a>Definitionen von Analysetools
 
Unabhängig davon, ob Sie Analysetools auswerten oder eine bestimmte Konfiguration einsetzen, gilt Folgendes: Sie müssen das Analysetool in der Felddefinition angeben und ggf. das Analysetool selbst konfigurieren, falls Sie keine integrierte Version nutzen. Beim Austauschen von Analysetools müssen Sie in der Regel den Index neu erstellen (löschen, neu erstellen und neu laden). 

### <a name="use-built-in-analyzers"></a>Verwenden von integrierten Analysetools

Integrierte oder vordefinierte Analysetools können anhand des Namens in einer `analyzer`-Eigenschaft einer Felddefinition angegeben werden, ohne dass im Index eine zusätzliche Konfiguration erforderlich ist. Im folgenden Beispiel wird veranschaulicht, wie Sie das Analysetool `whitespace` für ein Feld festlegen.

```json
    {
      "name": "phoneNumber",
      "type": "Edm.String",
      "key": false,
      "retrievable": true,
      "searchable": true,
      "analyzer": "whitespace"
    }
```
Weitere Informationen zu allen verfügbaren integrierten Analysetools finden Sie in der [Liste mit den vordefinierten Analysetools](https://docs.microsoft.com/azure/search/index-add-custom-analyzers#predefined-analyzers-reference). 

### <a name="use-custom-analyzers"></a>Verwenden von benutzerdefinierten Analysetools

Wenn Sie ein [benutzerdefiniertes Analysetool](index-add-custom-analyzers.md) verwenden, definieren Sie es im Index mit einer benutzerdefinierten Kombination aus Tokenizer und Tokenfilter mit den möglichen Konfigurationseinstellungen. Als Nächstes verweisen Sie in einer Felddefinition darauf, wie Sie dies auch für ein integriertes Analysetool tun.

Wenn das Ziel die Tokenisierung des gesamten Ausdrucks ist, empfehlen wir Ihnen die Verwendung eines benutzerdefinierten Analysetools mit einem **Schlüsselwort-Tokenizer** und einem **Kleinbuchstaben-Tokenfilter**.

+ Der Schlüsselwort-Tokenizer erstellt für den gesamten Inhalt eines Felds jeweils ein Token.
+ Mit dem Kleinbuchstaben-Tokenfilter werden Großbuchstaben in Kleinbuchstaben umgewandelt. Mit Abfrageparsern werden Texteingaben in Großbuchstaben normalerweise in Kleinbuchstaben umgewandelt. Bei der Umwandlung in Kleinbuchstaben werden die Eingaben mit den tokenisierten Ausdrücken homogenisiert.

Im folgenden Beispiel ist ein benutzerdefiniertes Analysetool dargestellt, das über einen Schlüsselwort-Tokenizer und einen Kleinbuchstaben-Tokenfilter verfügt.

```json
{
"fields": [
  {
  "name": "accountNumber",
  "analyzer":"myCustomAnalyzer",
  "type": "Edm.String",
  "searchable": true,
  "filterable": true,
  "retrievable": true,
  "sortable": false,
  "facetable": false
  }
],

"analyzers": [
  {
  "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
  "name":"myCustomAnalyzer",
  "charFilters":[],
  "tokenizer":"keyword_v2",
  "tokenFilters":["lowercase"]
  }
],
"tokenizers":[],
"charFilters": [],
"tokenFilters": []
```

> [!NOTE]
> Der Tokenizer `keyword_v2` und der Tokenfilter `lowercase` sind dem System bekannt, und es werden die entsprechenden Standardkonfigurationen verwendet. Daher können Sie darauf anhand des Namens verweisen, ohne sie zuerst definieren zu müssen.

## <a name="tips-and-best-practices"></a>Tipps und Best Practices

### <a name="tune-query-performance"></a>Optimieren der Abfrageleistung

Wenn Sie die empfohlene Konfiguration mit dem Tokenizer „keyword_v2“ und dem Kleinbuchstaben-Tokenfilter implementieren, kommt es ggf. zu einer Verringerung der Abfrageleistung, weil zusätzlich zu den vorhandenen Token in Ihrem Index die Tokenfilterverarbeitung durchgeführt werden muss. 

Im folgenden Beispiel wird ein [EdgeNGramTokenFilter](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/ngram/EdgeNGramTokenizer.html)-Element hinzugefügt, um den Präfixabgleich zu beschleunigen. Zusätzliche Token werden für Kombinationen mit 2 bis 25 Zeichen generiert: (nicht nur MS, MSF, MSFT, MSFT/, MSFT/S, MSFT/SQ, MSFT/SQL). Diese zusätzliche Tokenisierung führt natürlich zu einem größeren Index.

```json
{
"fields": [
  {
  "name": "accountNumber",
  "analyzer":"myCustomAnalyzer",
  "type": "Edm.String",
  "searchable": true,
  "filterable": true,
  "retrievable": true,
  "sortable": false,
  "facetable": false
  }
],

"analyzers": [
  {
  "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
  "name":"myCustomAnalyzer",
  "charFilters":[],
  "tokenizer":"keyword_v2",
  "tokenFilters":["lowercase", "my_edgeNGram"]
  }
],
"tokenizers":[],
"charFilters": [],
"tokenFilters": [
  {
  "@odata.type":"#Microsoft.Azure.Search.EdgeNGramTokenFilterV2",
  "name":"my_edgeNGram",
  "minGram": 2,
  "maxGram": 25,
  "side": "front"
  }
]
```

### <a name="use-different-analyzers-for-indexing-and-query-processing"></a>Verwenden von verschiedenen Analysetools für die Indizierung und Abfrageverarbeitung

Analysetools werden beim Indizieren und während der Abfrageausführung aufgerufen. Es ist üblich, für beide Vorgänge dasselbe Analysetool zu nutzen, aber Sie können für jede Workload benutzerdefinierte Analysetools konfigurieren. Außerkraftsetzungen von Analysetools werden in der [Indexdefinition](https://docs.microsoft.com/rest/api/searchservice/create-index) in einem `analyzers`-Abschnitt angegeben, und dann wird in bestimmten Feldern darauf verwiesen. 

Falls die benutzerdefinierte Analyse nur während der Indizierung erforderlich ist, können Sie das benutzerdefinierte Analysetool nur auf die Indizierung anwenden und für Abfragen weiterhin das Lucene-Standardanalysetool (oder ein anderes) nutzen.

Zum Angeben einer rollenspezifischen Analyse können Sie jeweils Eigenschaften im Feld festlegen, indem Sie `indexAnalyzer` und `searchAnalyzer` anstelle der `analyzer`-Standardeigenschaft angeben.

```json
"name": "featureCode",
"indexAnalyzer":"my_customanalyzer",
"searchAnalyzer":"standard",
```

### <a name="duplicate-fields-for-different-scenarios"></a>Doppelte Felder für unterschiedliche Szenarien

Eine andere Option ist die Nutzung der Zuweisung eines Analysetools pro Feld, um für unterschiedliche Szenarien eine Optimierung zu erzielen. Sie können beispielsweise „featureCode“ und „featureCodeRegex“ definieren, um für ein Szenario eine reguläre Volltextsuche und für ein anderes Szenario einen erweiterten Musterabgleich zu unterstützen.

```json
{
  "name": "featureCode",
  "type": "Edm.String",
  "retrievable": true,
  "searchable": true,
  "analyzer": null
},
{
  "name": "featureCodeRegex",
  "type": "Edm.String",
  "retrievable": true,
  "searchable": true,
  "analyzer": "my_customanalyzer"
},
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde beschrieben, wie Analysetools sowohl Probleme bei Abfragen verursachen als auch zu deren Lösung beitragen können. Im nächsten Schritt soll genauer beschrieben werden, wie sich Analysetools auf die Indizierung und Abfrageverarbeitung auswirken. Informieren Sie sich besonders über die Nutzung der API für die Textanalyse, um eine tokenisierte Ausgabe zu erhalten. Sie können dann genau verfolgen, welche Elemente von einem Analysetool für Ihren Index erstellt werden.

+ [Sprachanalysen](search-language-support.md)
+ [Analysetools für Textverarbeitung in Azure Cognitive Search](search-analyzers.md)
+ [Text analysieren (Azure Cognitive Search-REST-API)](https://docs.microsoft.com/rest/api/searchservice/test-analyzer)
+ [Funktionsweise der Volltextsuche (Abfragearchitektur)](search-lucene-query-architecture.md)
