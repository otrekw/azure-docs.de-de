---
title: Hinzufügen der Rechtschreibprüfung
titleSuffix: Azure Cognitive Search
description: Fügen Sie der Abfragepipeline eine Rechtschreibkorrektur hinzu, um Tippfehler in Abfragebegriffen zu korrigieren, bevor die Abfrage ausgeführt wird.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.custom: references_regions
ms.openlocfilehash: be72cae8d109bfeccd60e8c821625f75357568e3
ms.sourcegitcommit: ec39209c5cbef28ade0badfffe59665631611199
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2021
ms.locfileid: "103232357"
---
# <a name="add-spell-check-to-queries-in-cognitive-search"></a>Hinzufügen der Rechtschreibprüfung zu Abfragen in Azure Cognitive Search

> [!IMPORTANT]
> Die Rechtschreibkorrektur ist nur über die Vorschau-REST-API in der öffentlichen Vorschau verfügbar. Vorschaufeatures werden wie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) beschrieben im Ist-Zustand angeboten. Während des anfänglichen Vorschaustarts ist die Rechtschreibprüfung kostenlos. Weitere Informationen finden Sie unter [Verfügbarkeit und Preise](semantic-search-overview.md#availability-and-pricing).

Sie können die Trefferquote (Recall) verbessern, indem Sie die Rechtschreibung einzelner Suchbegriffe der Abfrage korrigieren, bevor sie an die Suchmaschine übergeben werden. Der Parameter **speller** (Rechtschreibprüfung) wird für alle Abfragetypen unterstützt: [simple](query-simple-syntax.md) (einfach), [full](query-lucene-syntax.md) (vollständig) sowie die neue Option [semantic](semantic-how-to-query-request.md) (semantisch), die sich derzeit in der öffentlichen Vorschau befindet.

## <a name="prerequisites"></a>Voraussetzungen

+ Ein vorhandener Suchindex mit englischem Inhalt

+ Ein Suchclient zum Senden von Abfragen

  Der Suchclient muss in der Abfrageanforderung Vorschau-REST-APIs unterstützen. Sie können [Postman](search-get-started-rest.md), [Visual Studio Code](search-get-started-vs-code.md) oder Code verwenden, den Sie geändert haben, um REST-Aufrufe an die Vorschau-APIs zu senden.

+ Eine [Abfrageanforderung](/rest/api/searchservice/preview-api/search-documents), die Rechtschreibkorrektur verwendet, weist folgende Einstellungen auf: „api-version=2020-06-30-Preview“, „speller=lexicon“ und „queryLanguage=en-us“.

  „queryLanguage“ muss für die Rechtschreibprüfung angegeben werden, und derzeit ist „en-us“ der einzige gültige Wert.

> [!Note]
> Der Parameter „speller“ ist in allen Tarifen verfügbar, und zwar in denselben Regionen, die die semantische Suche bereitstellen. Sie müssen sich für den Zugriff auf diese Previewfunktion nicht registrieren. Weitere Informationen finden Sie unter [Verfügbarkeit und Preise](semantic-search-overview.md#availability-and-pricing).

## <a name="spell-correction-with-simple-search"></a>Rechtschreibkorrektur mit einfacher Suche

Im folgenden Beispiel wird der integrierte Index „hotels-sample“ verwendet, um die Rechtschreibkorrektur bei einer einfachen Freitextabfrage zu veranschaulichen. Ohne Rechtschreibkorrektur gibt die Abfrage keine Ergebnisse zurück. Bei der Korrektur gibt die Abfrage ein Ergebnis für die familienfreundliche Ferienunterkunft von Johnson zurück.

```http
POST https://[service name].search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview
{
    "search": "famly acitvites",
    "speller": "lexicon",
    "queryLanguage": "en-us",
    "queryType": "simple",
    "select": "HotelId,HotelName,Description,Category,Tags",
    "count": true
}
```

## <a name="spell-correction-with-full-lucene"></a>Rechtschreibkorrektur mit Lucene-Abfragen vom Typ „full“ (vollständig)

Die Rechtschreibkorrektur erfolgt für einzelne Abfragebegriffe, die einer Analyse unterzogen werden, weshalb Sie den Parameter „speller“ bei einigen Lucene-Abfragen verwenden können, bei anderen jedoch nicht.

+ Nicht kompatible Abfrageformulare, die die Textanalyse umgehen, sind: Platzhalter, Regex, Fuzzy
+ Kompatible Abfrageformulare umfassen Folgendes: feldbezogene Suche, NEAR-Suche, Term Boosting

In diesem Beispiel wird die feldbezogene Suche über das Feld „Kategorie“ mit vollständiger Lucene-Syntax und einen falsch geschriebenen Abfragebegriff verwendet. Durch die Einbeziehung der Rechtschreibprüfung wird der Tippfehler in „Suiite“ korrigiert, und die Abfrage ist erfolgreich.

```http
POST https://[service name].search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview
{
    "search": "Category:(Resort and Spa) OR Category:Suiite",
    "queryType": "full",
    "speller": "lexicon",
    "queryLanguage": "en-us",
    "select": "Category",
    "count": true
}
```

## <a name="spell-correction-with-semantic-search"></a>Rechtschreibkorrektur mit semantischer Suche

Für diese Abfrage, bei der jeder Begriff mit Ausnahme von einem Tippfehler enthält, werden Rechtschreibkorrekturen vorgenommen, um relevante Ergebnisse zurückzugeben. Weitere Informationen finden Sie unter [Erstellen einer Semantikabfrage](semantic-how-to-query-request.md).

```http
POST https://[service name].search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview     
{
    "search": "hisotoric hotell wiht great restrant nad wiifi",
    "queryType": "semantic",
    "speller": "lexicon",
    "queryLanguage": "en-us",
    "searchFields": "HotelName,Tags,Description",
    "select": "HotelId,HotelName,Description,Category,Tags",
    "count": true
}
```

## <a name="language-considerations"></a>Sprachbezogene Überlegungen

Der für eine Semantikabfrage erforderliche queryLanguage-Parameter muss mit den [Sprachanalysetools](index-add-language-analyzers.md) übereinstimmen, die den Felddefinitionen im Indexschema zugewiesen sind. 

+ queryLanguage bestimmt, welche Lexika für die Rechtschreibprüfung verwendet werden, und dient zudem als Eingabe für den [semantischen Klassifizierungsalgorithmus](semantic-answers.md), sofern Sie "queryType=semantic" verwenden.

+ Sprachanalyzer werden bei der Indizierung und Abfrageausführung übereinstimmender Dokumente im Suchindex eingesetzt. Ein Beispiel für eine Felddefinition mit einem Sprachanalyzer ist `"name": "Description", "type": "Edm.String", "analyzer": "en.microsoft"`.

Wenn queryLanguage auf "en-us" festgelegt ist, müssen alle Sprachanalyzer ebenfalls einer Variante des Englischen entsprechen ("en.microsoft" oder "en.lucene").

> [!NOTE]
> Sprachunabhängige Analysetools (wie z. B. „keyword“, „simple“, “standard“, „stop“, „whitespace“ oder `standardasciifolding.lucene`) stehen nicht in Konflikt mit den queryLanguage-Einstellungen.

In einer Abfrageanforderung gilt der festgelegte queryLanguage-Wert gleichermaßen für Rechtschreibprüfung, Antworten und Beschriftungen. Es gibt keine Außerkraftsetzung einzelner Teile.

Inhalt in einem Suchindex kann zwar aus mehreren Sprachen zusammengesetzt sein, die Abfrageeingabe erfolgt jedoch höchstwahrscheinlich nur in einer Sprache. Die Suchmaschine prüft nicht die Kompatibilität von „queryLanguage“, dem Sprachanalysetool und der Sprache, in der der Inhalt verfasst ist. Stellen Sie daher sicher, dass Sie Abfragen entsprechend einschränken, um falsche Ergebnisse zu vermeiden.

## <a name="next-steps"></a>Nächste Schritte

+ [Erstellen einer Semantikabfrage](semantic-how-to-query-request.md)
+ [Erstellen einer einfachen Abfrage](search-query-create.md)
+ [Verwenden der vollständigen Lucene-Abfragesyntax](query-Lucene-syntax.md)
+ [Verwenden einfacher Abfragesyntax](query-simple-syntax.md)
+ [Übersicht über die semantische Suche](semantic-search-overview.md)