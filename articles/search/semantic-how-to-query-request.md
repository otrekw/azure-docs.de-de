---
title: Erstellen einer Semantikabfrage
titleSuffix: Azure Cognitive Search
description: Legen Sie eine Abfrage vom Typ „semantic“ (semantisch) fest, um die Deep Learning-Modelle mit der Abfrageverarbeitung zu verknüpfen und so die Absicht und den Kontext als Teil des Suchrangs und der Relevanz abzuleiten.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/18/2021
ms.openlocfilehash: c33739124092a17acf0590f00b2f9c3c09bf894e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104654661"
---
# <a name="create-a-query-for-semantic-captions-in-cognitive-search"></a>Erstellen Sie eine Abfrage für semantische Beschriftungen in Cognitive Search

> [!IMPORTANT]
> Die semantische Suche befindet sich in der öffentlichen Vorschau und ist über die Vorschau-REST-API und das Azure-Portal verfügbar. Vorschaufeatures werden wie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) beschrieben im Ist-Zustand angeboten. Diese Features sind abrechenbar. Weitere Informationen finden Sie unter [Verfügbarkeit und Preise](semantic-search-overview.md#availability-and-pricing).

In diesem Artikel erfahren Sie, wie Sie eine Suchanfrage formulieren, die die semantische Rangfolge verwendet und semantische Beschriftungen (und optional [semantische Antworten](semantic-answers.md)) zurückgibt, wobei die wichtigsten Begriffe und Ausdrücke hervorgehoben werden. Beschriftungen und Antworten werden in Abfragen zurückgegeben, die mit dem Abfragetyp "Semantik" formuliert werden.

Sowohl Beschriftungen als auch Antworten werden wörtlich aus dem Text im Suchdokument extrahiert. Das semantische Subsystem bestimmt, welcher Inhalt die Merkmale einer Beschriftung oder Antwort hat, aber es werden keine neuen Sätze oder Ausdrücke verfasst. Aus diesem Grund funktionieren Inhalte, die Erläuterungen oder Definitionen enthalten, am besten für die semantische Suche.

## <a name="prerequisites"></a>Voraussetzungen

+ Ein Suchdienst in einem Standard-Tarif (S1, S2, S3) in einer der folgenden Regionen: „USA, Norden-Mitte“, „USA, Westen“, „USA, Westen 2“, „USA, Osten 2“, „Europa, Norden“, „Europa, Westen“. Wenn Sie in einer dieser Regionen über einen Dienst mit der Ebene S1 oder höher verfügen, können Sie Zugriff anfordern, ohne einen neuen Dienst erstellen zu müssen.

+ Zugriff auf die Vorschau der semantischen Suche: [Registrieren](https://aka.ms/SemanticSearchPreviewSignup)

+ Ein bestehender Suchindex, der englischen Inhalt enthält.

+ Ein Suchclient zum Senden von Abfragen

  Der Suchclient muss in der Abfrageanforderung Vorschau-REST-APIs unterstützen. Sie können [Postman](search-get-started-rest.md), [Visual Studio Code](search-get-started-vs-code.md) oder Code verwenden, um REST-Aufrufe an die Vorschau-APIs zu senden. Sie können auch den [Suchexplorer](search-explorer.md) im Azure-Portal verwenden, um eine Semantikabfrage zu übermitteln.

+ Eine [Abfrageanforderung](/rest/api/searchservice/preview-api/search-documents) muss die semantische Option und weitere in diesem Artikel beschriebene Parameter enthalten.

## <a name="whats-a-semantic-query"></a>Was ist eine Semantikabfrage?

In Azure Cognitive Search handelt es sich bei einer Abfrage um eine parametrisierte Anforderung, mit der die Abfrageverarbeitung und die Form der Antwort bestimmt werden. Eine *Semantikabfrage* fügt Parameter hinzu, die das Modell zur semantischen Neuanordnung aufrufen. Mit diesem können der Kontext sowie die Bedeutung übereinstimmender Ergebnisse bewertet, relevantere Übereinstimmungen in der Rangfolge nach oben verschoben und semantische Antworten und Beschriftungen zurückgegeben werden.

Die folgende Anforderung ist charakteristisch für eine minimale Semantikabfrage (ohne Antwort).

```http
POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=2020-06-30-Preview      
{    
    "search": " Where was Alan Turing born?",    
    "queryType": "semantic",  
    "searchFields": "title,url,body",  
    "queryLanguage": "en-us"  
}
```

Wie bei allen Abfragen in Azure Cognitive Search zielt die Anforderung auf die Dokumentensammlung eines einzelnen Indexes ab. Außerdem wird bei einer Semantikabfrage dieselbe Sequenz von Parsen, Analysieren, Überprüfen und Bewerten durchlaufen wie bei einer nicht semantischen Abfrage. 

Der Unterschied liegt in der Relevanz und Bewertung. Wie in diesem Vorschaurelease definiert, ist eine Semantikabfrage eine Abfrage, deren *Ergebnisse* mithilfe eines semantischen Sprachmodells erneut bewertet werden. Dadurch wird eine Möglichkeit bereitgestellt, die Übereinstimmungen, die von dem Semantikrangfolge-Modul als am relevantesten eingestuft werden, als erstes anzuzeigen.

Nur die 50 besten Treffer aus den anfänglichen Ergebnissen können in eine semantische Rangfolge gebracht werden, und alle enthalten Beschriftungen in der Antwort. Optional können Sie bei der Anforderung einen **`answer`** -Parameter angeben, um eine mögliche Antwort zu extrahieren. Weitere Informationen finden Sie unter [Semantische Antworten](semantic-answers.md).

## <a name="query-with-search-explorer"></a>Abfragen mit Suchexplorer

Der [Suchexplorer](search-explorer.md) wurde aktualisiert und enthält nun Optionen für semantische Abfragen. Diese Optionen werden im Portal angezeigt, nachdem Sie die folgenden Schritte ausgeführt haben:

1. [Registrieren](https://aka.ms/SemanticSearchPreviewSignup) und Zutritt Ihres Suchdienstes in das Vorschauprogramm

1. Öffnen Sie das Portal mit dieser Syntax: `https://portal.azure.com/?feature.semanticSearch=true`

Abfrageoptionen können Semantikabfragen, SearchFields und die Rechtschreibkorrektur aktivieren. Sie können auch die erforderlichen Abfrageparameter in die Abfragezeichenfolge einfügen.

:::image type="content" source="./media/semantic-search-overview/search-explorer-semantic-query-options.png" alt-text="Abfrageoptionen im Suchexplorer" border="true":::

## <a name="query-using-rest"></a>Abfragen über REST

Verwenden Sie die [Suchdokumente (REST-Vorschau)](/rest/api/searchservice/preview-api/search-documents), um die Anforderung programmgesteuert zu formulieren.

Eine Antwort enthält automatisch Beschriftungen und Hervorhebungen. Wenn das Ergebnis die Rechtschreibkorrektur oder Antworten enthalten soll, können Sie für die Anforderung einen optionalen **`speller`** - oder **`answers`** -Parameter hinzufügen.

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

In der folgenden Tabelle erhalten Sie eine Gesamtübersicht über die zusammengefassten Abfrageparameter, die in einer Semantikabfrage verwendet werden. Eine Liste aller Parameter finden Sie unter [Dokumente durchsuchen (REST-Vorschau)](/rest/api/searchservice/preview-api/search-documents).

| Parameter | Typ | BESCHREIBUNG |
|-----------|-------|-------------|
| queryType | String | Gültige Werte sind „simple“, „full“ und „semantic“. Der Wert „semantic“ wird für Semantikabfragen benötigt. |
| queryLanguage | String | Für Semantikabfragen erforderlich. Derzeit wird nur „en-us“ implementiert. |
| searchFields | String | Hierbei handelt es sich um eine durch Trennzeichen getrennte Liste mit durchsuchbaren Feldern. Gibt die Felder an, für die eine semantische Rangfolge auftritt, von der Beschriftungen und Antworten extrahiert werden. </br></br>Im Gegensatz zu einfachen und vollständigen Abfragetypen bestimmt die Reihenfolge, in der Felder aufgeführt sind, welches Feld Vorrang hat. Weitere Anweisungen zur Verwendung finden Sie unter [Schritt 2: Festlegen von searchFields](#searchfields). |
| speller | String | Dies ist ein optionaler Parameter und nicht spezifisch für semantische Abfragen. Er korrigiert falsch geschriebene Begriffe, bevor diese die Suchmaschine erreichen. Weitere Informationen finden Sie unter [Hinzufügen von Rechtschreibkorrekturen zu Abfragen](speller-how-to-add.md). |
| answers |String | Dies sind optionale Parameter, um anzugeben, ob semantische Antworten in das Ergebnis einbezogen werden sollen. Derzeit wird nur „extractive“ implementiert. Antworten können so konfiguriert werden, dass maximal fünf Werte zurückgegeben werden. Der Standardwert ist 1. In diesem Beispiel ist die Anzahl der semantischen Antworten 3: „extractive\|count3“. Weitere Informationen finden Sie unter [Rückgabe von semantischen Antworten](semantic-answers.md).|

### <a name="formulate-the-request"></a>Formulieren der Anforderung

In diesem Abschnitt werden die für die semantische Suche erforderlichen Abfrageparameter beschrieben.

#### <a name="step-1-set-querytype-and-querylanguage"></a>Schritt 1: Festlegen von queryType und queryLanguage

Fügen Sie der REST-API folgende Parameter hinzu. Es müssen beide Parameter angegeben werden.

```json
"queryType": "semantic",
"queryLanguage": "en-us",
```

Der queryLanguage-Parameter muss mit den [Sprachanalysetools](index-add-language-analyzers.md) übereinstimmen, die den Felddefinitionen im Indexschema zugewiesen sind. Wenn „queryLanguage=en-us“ ist, dann müssen alle Sprachananalysetools auch eine englische Variante sein („en.microsoft“ oder „en.lucene“). Alle sprachunabhängigen Analysetools, wie z. B. „keyword“ oder „simple“, verursachen keinen Konflikt mit den queryLanguage-Werten.

Wenn Sie in einer Abfrageanforderung auch die [Rechtschreibkorrektur](speller-how-to-add.md) verwenden, gilt die festgelegte „queryLanguage“ gleichermaßen für Rechtschreibprüfung, Antworten und Beschriftungen. Es gibt keine Außerkraftsetzung einzelner Teile. 

Inhalt in einem Suchindex kann zwar aus mehreren Sprachen zusammengesetzt sein, die Abfrageeingabe erfolgt jedoch höchstwahrscheinlich nur in einer Sprache. Die Suchmaschine prüft nicht die Kompatibilität von „queryLanguage“, dem Sprachanalysetool und der Sprache, in der der Inhalt verfasst ist. Stellen Sie daher sicher, dass Sie Abfragen entsprechend einschränken, um falsche Ergebnisse zu vermeiden.

<a name="searchfields"></a>

#### <a name="step-2-set-searchfields"></a>Schritt 2: Festlegen von searchFields

Der searchFields-Parameter wird verwendet, um Passagen zu identifizieren, die für die „semantische Ähnlichkeit“ zur Abfrage evaluiert werden. Für die Vorschau empfiehlt es sich nicht, searchFields leer zu lassen, da das Modell einen Hinweis benötigt, welche Felder für die Verarbeitung am wichtigsten sind.

Die Reihenfolge der Suchfelder ist wichtig. Wenn Sie SearchFields bereits in vorhandenen einfachen oder vollständigen Lucene-Abfragen verwenden, rufen Sie diesen Parameter beim Wechsel zu einer Semantikabfrage erneut auf, um die Feldreihenfolge zu überprüfen.

Für zwei oder mehr SearchFields:

+ Fügen Sie nur Zeichenfolgenfelder und Zeichenfolgenfelder der obersten Ebene in Sammlungen ein. Wenn Sie in eine Sammlung Nicht-Zeichenfolgenfelder oder Felder niedrigerer Ebenen einschließen, wird kein Fehler angezeigt. Diese Felder werden in der semantischen Rangfolge jedoch nicht verwendet.

+ Das erste Feld sollte immer präzise sein (z. B. ein Titel oder ein Name) und idealerweise weniger als 25 Wörter enthalten.

+ Falls der Index ein URL-Feld besitzt, das Textinhalt aufweist, also für Benutzer lesbar (wie `www.domain.com/name-of-the-document-and-other-details`) und nicht computerbezogen (wie `www.domain.com/?id=23463&param=eis`) ist, platzieren Sie es in der Liste an zweiter Stelle. Wenn kein präzises Titelfeld vorhanden ist, platzieren Sie es an erster Stelle.

+ Folgen Sie diesen Feldern nach beschreibenden Feldern, in denen die Antworten auf Semantikabfragen gefunden werden können, z. B. der Hauptinhalt eines Dokuments.

Wenn nur ein Feld angegeben ist, verwenden Sie ein beschreibendes Feld, in dem die Antwort auf Semantikabfragen gefunden werden kann, z. B. der Hauptinhalt eines Dokuments. 

#### <a name="step-3-remove-orderby-clauses"></a>Schritt 3: Entfernen der orderBy-Klauseln

Entfernen Sie alle orderBy-Klauseln, wenn diese in einer vorhandenen Anforderung existieren. Die semantische Bewertung dient zum Sortieren der Ergebnisse, und wenn Sie eine explizite Sortierlogik hinzufügen, wird ein HTTP 400-Fehler zurückgegeben.

#### <a name="step-4-add-answers"></a>Schritt 4: Hinzufügen von Antworten

Fügen Sie optional semantische Antworten („answers“) hinzu, wenn Sie zusätzliche Verarbeitungsschritte einschließen möchten, die eine semantische Antwort bereitstellen. Antworten (und Beschriftungen) werden aus den in searchFields aufgeführten Feldern extrahiert. Stellen Sie sicher, dass Sie inhaltsreiche Felder in searchFields einschließen, um die besten Antworten in einer Antwort zu erhalten. Weitere Informationen finden Sie unter [Rückgabe von semantischen Antworten](semantic-answers.md).

#### <a name="step-5-add-other-parameters"></a>Schritt 5: Hinzufügen weiterer Parameter

Legen Sie alle anderen Parameter fest, die in der Anforderung enthalten sein sollen. Parameter wie [speller](speller-how-to-add.md), [select](search-query-odata-select.md) und „count“ verbessern die Qualität der Anforderung und die Lesbarkeit der Antwort.

Optional können Sie den hervorgehobenen Stil anpassen, der auf Beschriftungen angewendet wird. Beschriftungen stellen eine Hervorhebung der Formatierung für Schlüsselpassagen im Dokument dar, die die Antwort zusammenfassen. Der Standardwert lautet `<em>`. Wenn Sie den Formatierungstyp angeben möchten (beispielsweise gelber Hintergrund), können Sie „highlightPreTag“ und „highlightPostTag“ festlegen.

## <a name="evaluate-the-response"></a>Auswertung der Antwort

Eine Antwort umfasst genau wie bei anderen Abfragen entweder alle als abrufbar markierten Felder oder nur Felder, die im Auswahlparameter aufgeführt sind. Sie enthält die ursprüngliche Relevanzbewertung und kann auch eine Anzahl oder Batchergebnisse enthalten, je nachdem, wie Sie die Anforderung formuliert haben.

In einer semantischen Abfrage weist die Antwort zusätzliche Elemente auf: eine neue Relevanzbewertung mit semantischer Reihenfolge, Beschriftungen in Nur-Text und mit Hervorhebungen und optional eine Antwort.

In einer Client-App können Sie die Suchseite so strukturieren, dass sie eine Beschriftung als Beschreibung der Übereinstimmung und nicht den gesamten Inhalt eines bestimmten Felds enthält. Dies ist hilfreich, wenn einzelne Felder für die Seite mit den Suchergebnissen zu umfangreich sind.

Die Antwort auf die oben stehende Beispielabfrage gibt den folgenden Treffer als beste Übereinstimmung zurück. Beschriftungen werden automatisch zurückgegeben, mit Klartext und hervorgehobenen Versionen. Antworten werden aus dem Beispiel ausgelassen, da keine für diese Abfrage und den Korpus bestimmt werden konnte.

```json
"@odata.count": 35,
"@search.answers": [],
"value": [
    {
        "@search.score": 1.8810667,
        "@search.rerankerScore": 1.1446577133610845,
        "@search.captions": [
            {
                "text": "Oceanside Resort. Luxury. New Luxury Hotel. Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
                "highlights": "<strong>Oceanside Resort.</strong> Luxury. New Luxury Hotel. Be the first to stay.<strong> Bay</strong> views from every room, location near the pier, rooftop pool, waterfront dining & more."
            }
        ],
        "HotelName": "Oceanside Resort",
        "Description": "New Luxury Hotel. Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
        "Category": "Luxury"
    },
```

## <a name="next-steps"></a>Nächste Schritte

Beachten Sie, dass semantische Rangfolge und semantische Antworten über ein anfängliches Resultset erstellt werden. Jede Logik, die die Qualität der anfänglichen Ergebnisse verbessert, wird an die semantische Suche weitergeleitet. Als nächsten Schritt überprüfen Sie die Features, die zu den anfänglichen Ergebnissen beitragen, einschließlich der Analysetools, die die Tokenisierung von Zeichenketten beeinflussen, der Bewertungsprofile, mit denen die Ergebnisse optimiert werden können, und des Standardrelevanzalgorithmus.

+ [Analysetools für Textverarbeitung](search-analyzers.md)
+ [Ähnlichkeitsalgorithmus für die Rangfolge](index-similarity-and-scoring.md)
+ [Bewertungsprofile](index-add-scoring-profiles.md)
+ [Übersicht über die semantische Suche](semantic-search-overview.md)
+ [Semantische Priorisierung in Azure Cognitive Search](semantic-ranking.md)
