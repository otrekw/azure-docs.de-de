---
title: Synonyme für die Abfrageerweiterung über einen Suchindex
titleSuffix: Azure Cognitive Search
description: Erstellen einer Synonymzuordnung zum Erweitern des Bereichs einer Suchabfrage für einen Index der kognitiven Azure-Suche. Der Bereich wird erweitert, um gleichwertige Begriffe einzubeziehen, die Sie in einer Liste bereitstellen.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/18/2020
ms.openlocfilehash: b62621a77f383b5c6413e7c187e7ba3d60beabad
ms.sourcegitcommit: a89a517622a3886b3a44ed42839d41a301c786e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2020
ms.locfileid: "97732086"
---
# <a name="synonyms-in-azure-cognitive-search"></a>Synonyme in der kognitiven Azure-Suche

Mit Synonymzuordnungen können Sie entsprechende Begriffe zuordnen, um den Bereich einer Abfrage zu erweitern, ohne dass der Benutzer den Begriff tatsächlich bereitstellen muss. Wenn zum Beispiel „dog“, „canine“ und „canine“ Synonyme sind, wird eine Abfrage nach „canine“ Dokumente finden, die „canine“ enthalten.

## <a name="create-synonyms"></a>Erstellen von Synonymen

Eine Synonymzuordnung ist ein Objekt, das einmalig erstellt und von vielen Indizes verwendet werden kann. Die [Dienstebene](search-limits-quotas-capacity.md#synonym-limits) bestimmt, wie viele Synonymzuordnungen Sie erstellen können, von 3 Synonymzuordnungen für die Tarife „Free“ und „Basic“, bis zu 20 für die Standard-Tarife. 

Sie können mehrere Synonymzuordnungen für verschiedene Sprachen erstellen, z. B. englische und französische Versionen, oder Lexika, wenn Ihr Inhalt technische oder unbekannte Terminologie enthält. Sie können zwar mehrere Synonymzuordnungen erstellen, ein Feld kann derzeit aber nur eine davon verwenden.

Eine Synonymzuordnung besteht aus Name, Format und Regeln, die als Synonymzuordnungseinträge fungieren. Das einzige unterstützte Format ist `solr`, und das `solr`-Format bestimmt die Regelkonstruktion.

```http
POST /synonymmaps?api-version=2020-06-30
{
    "name": "geo-synonyms",
    "format": "solr",
    "synonyms": "
        USA, United States, United States of America\n
        Washington, Wash., WA => WA\n"
}
```

Um eine Synonymzuordnung zu erstellen, verwenden Sie [Synonymzuordnung erstellen (REST-API)](/rest/api/searchservice/create-synonym-map) oder ein Azure-SDK. C#-Entwickler sollten mit dem [Hinzufügen von Synonymen in der kognitiven Azure-Suche mit C#](search-synonyms-tutorial-sdk.md) beginnen.

## <a name="define-rules"></a>Definieren von Regeln

Zuordnungsregeln entsprechen den Open-Source-Synonymfilterspezifikation von Apache Solr, die im folgenden Dokument beschrieben ist: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter). Das Format `solr` unterstützt zwei Arten von Regeln:

+ Äquivalenz (wobei Begriffe in der Abfrage gleichwertige Alternativen sind)

+ Explizite Zuordnungen (wobei Begriffe vor dem Ausführen der Abfrage einem expliziten Begriff zugeordnet werden)

Jede Regel muss durch das Zeilenumbruchzeichen (`\n`) von anderen Regeln getrennt werden. Sie können bis zu 5.000 Regeln pro Synonymzuordnung in einem kostenlosen Dienst und 20.000 Regeln pro Zuordnung in anderen Tarifen definieren. Jede Regel kann bis zu 20 Erweiterungen (oder Elemente in einer Regel) aufweisen. Weitere Informationen finden Sie unter [Synonymeinschränkungen](search-limits-quotas-capacity.md#synonym-limits).

Abfrageparser wandeln alle Begriffe in Großbuchstaben oder gemischter Groß-/Kleinschreibung in Kleinbuchstaben um. Wenn Sie jedoch Sonderzeichen in der Zeichenfolge beibehalten möchten, wie z. B. ein Komma oder einen Gedankenstrich, fügen Sie beim Erstellen der Synonymzuordnung die entsprechenden Escape-Zeichen hinzu. 

### <a name="equivalency-rules"></a>Äquivalenzregeln

Regeln für äquivalente Begriffe innerhalb derselben Regel werden durch Kommas getrennt. Im ersten Beispiel wird eine Abfrage nach `USA` zu `USA` ODER `"United States"` ODER `"United States of America"` erweitert. Beachten Sie, dass, zum Abgleich mit einem Ausdruck, die Abfrage selbst eine Abfrage mit einem in Anführungszeichen eingeschlossene Ausdruck sein muss.

Im Falle einer Äquivalenzregel wird eine Abfrage nach `dog` so erweitert, dass sie auch `puppy` und `canine` einschließt.

```json
{
"format": "solr",
"synonyms": "
    USA, United States, United States of America\n
    dog, puppy, canine\n
    coffee, latte, cup of joe, java\n"
}
```

### <a name="explicit-mapping"></a>Explizite Zuordnung

Regeln für eine explizite Zuordnung werden durch einen Pfeil `=>` gekennzeichnet. Eine Begriffsfolge (sofern angegeben) einer Suchabfrage, die den linken Teil von `=>` vergleicht, wird zum Zeitpunkt der Abfrage durch die Alternativen auf der rechten Seite ersetzt.

Bei einer expliziten Zuordnung wird eine Abfrage nach `Washington`, `Wash.` oder `WA` als `WA` umformuliert, und die Abfrage-Engine sucht nur nach Übereinstimmungen für den Begriff `WA`. Die explizite Zuordnung gilt nur für die angegebene Richtung und schreibt in diesem Fall die Abfrage `WA` nicht zu `Washington` um.

```json
{
"format": "solr",
"synonyms": "
    Washington, Wash., WA => WA\n
    California, Calif., CA => CA\n"
}
```

### <a name="escaping-special-characters"></a>Verwenden von Escapezeichen für Sonderzeichen

Wenn Sie Synonyme definieren müssen, die Kommas oder andere Sonderzeichen enthalten, können Sie diese wie in diesem Beispiel mit einem umgekehrten Schrägstrich als Escapezeichen versehen:

```json
{
"format": "solr",
"synonyms": "WA\, USA, WA, Washington\n"
}
```

Da der umgekehrte Schrägstrich selbst ein Sonderzeichen in anderen Sprachen wie JSON und C# ist, müssen Sie ihn wahrscheinlich mit einem doppelten Escapezeichen versehen. Beispielsweise würde der JSON-Code, der für die obige Synonymzuordnung an die REST-API gesendet wird, wie folgt aussehen:

```json
{
"format":"solr",
"synonyms": "WA\\, USA, WA, Washington"
}
```

## <a name="upload-and-manage-synonym-maps"></a>Hochladen und Verwalten von Synonymzuordnungen

Wie bereits erwähnt, können Sie eine Synonymzuordnung erstellen oder aktualisieren, ohne die Abfrage- und Indizierungsworkloads zu unterbrechen. Eine Synonymzuordnung ist ein eigenständiges Objekt (wie Indizes oder Datenquellen), und solange sie von keinem Feld verwendet wird, führen Aktualisierungen nicht zu Fehlern bei Indizierungen oder Abfragen. Sobald Sie eine Synonymzuordnung jedoch zu einer Felddefinition hinzugefügt haben und sie dann löschen, schlägt jede Abfrage, die die betreffenden Felder enthält, mit einem 404-Fehler fehl.

Das Erstellen, Aktualisieren und Löschen einer Synonymzuordnung betrifft immer das ganze Dokument. Daher können Sie keine Teile der Synonymzuordnung inkrementell aktualisieren oder löschen. Selbst das Aktualisieren einer einzelnen Regel erfordert das erneute Laden.

## <a name="assign-synonyms-to-fields"></a>Zuweisen von Synonymen zu Feldern

Nach dem Hochladen einer Synonymzuordnung können Sie die Synonyme für Felder des Typs `Edm.String` oder `Collection(Edm.String)` auf Felder mit `"searchable":true` aktivieren. Wie bereits erwähnt, kann eine Felddefinition nur eine Synonymzuordnung verwenden.

```http
POST /indexes?api-version=2020-06-30
{
    "name":"hotels-sample-index",
    "fields":[
        {
            "name":"description",
            "type":"Edm.String",
            "searchable":true,
            "synonymMaps":[
            "en-synonyms"
            ]
        },
        {
            "name":"description_fr",
            "type":"Edm.String",
            "searchable":true,
            "analyzer":"fr.microsoft",
            "synonymMaps":[
            "fr-synonyms"
            ]
        }
    ]
}
```

## <a name="query-on-equivalent-or-mapped-fields"></a>Abfrage nach äquivalenten oder zugeordneten Feldern

Das Hinzufügen von Synonymen stellt keine neuen Anforderungen an die Abfragekonstruktion. Sie können Begriffe und Ausdrücke genau so abfragen wie vor dem Hinzufügen der Synonyme. Der einzige Unterschied besteht darin, dass die Abfrage-Engine bei Vorhandensein eines Abfragebegriffs in der Synonymzuordnung den Begriff oder Ausdruck je nach Regel entweder erweitert oder umformuliert.

## <a name="how-synonyms-interact-with-other-features"></a>Interaktion von Synonymen mit anderen Features

Das Feature „Synonyme“ ändert die ursprüngliche Abfrage mithilfe von Synonymen mit dem OR-Operator. Aus diesem Grund werden der ursprüngliche Begriff und Synonyme von der Treffermarkierung und von Bewertungsprofilen als gleichwertig behandelt.

Synonyme gelten nur für Suchabfragen und werden nicht für Filter, Facets, AutoVervollständigen oder Vorschläge unterstützt. AutoVervollständigen und Vorschläge basieren nur auf dem ursprünglichen Begriff. Synonymübereinstimmungen werden in der Antwort nicht angezeigt.

Die Synonymerweiterungen gelten nicht für Platzhaltersuchbegriffe. Präfix-, Fuzzy- und Regex-Begriffe werden nicht erweitert.

Wenn Sie eine einzelne Abfrage, die eine Synonymerweiterung sowie Platzhalter-, Regex- und Fuzzysuche umfasst, durchführen möchten, können Sie die Abfragen mit der OR-Syntax kombinieren. Um beispielsweise Synonyme mit Platzhaltern für eine einfache Abfragesyntax zu kombinieren, lautet der Begriff `<query> | <query>*`.

Wenn Sie über einen vorhandenen Index in einer Entwicklungsumgebung (nicht Produktionsumgebung) verfügen, experimentieren Sie mit einem kleinen Wörterbuch, um zu prüfen, wie sich das Hinzufügen von Synonymen auf die Suche auswirkt, einschließlich der Auswirkungen auf Bewertungsprofile, Treffermarkierungen und Vorschläge.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Synonymzuordnung (REST-API)](/rest/api/searchservice/create-synonym-map)