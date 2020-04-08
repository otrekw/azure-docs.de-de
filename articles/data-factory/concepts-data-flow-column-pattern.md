---
title: Spaltenmuster in Azure Data Factory-Mappingdatenflüssen
description: Erstellen generalisierter Datentransformationsmuster unter Verwendung von Spaltenmustern in Azure Data Factory-Mappingdatenflüssen
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/21/2019
ms.openlocfilehash: bfb4eeef482c4944e75b7805642bc93c23195208
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80065518"
---
# <a name="using-column-patterns-in-mapping-data-flow"></a>Verwenden von Spaltenmustern in Mappingdatenflüssen

In verschiedenen Mappingdatenfluss-Transformationen kann auf Vorlagenspalten mithilfe von Mustern (anstelle von hartcodierten Spaltennamen) verwiesen werden. Dieser Abgleich wird als *Spaltenmuster* bezeichnet. Sie können Muster definieren, um Spalten auf der Grundlage von Name, Datentyp, Stream oder Position abgleichen, anstatt exakte Feldnamen zu verwenden. Spaltenmuster sind in zwei Szenarien hilfreich:

* Wenn Sie eingehende Quellfelder häufig ändern – etwa im Falle von sich ändernden Spalten in Textdateien oder NoSQL-Datenbanken. Dieses Szenario wird als [Schemaabweichung](concepts-data-flow-schema-drift.md) bezeichnet.
* Wenn Sie einen Vorgang für eine große Gruppe von Spalten ausführen möchten, um beispielsweise alle Spalten, deren Spaltenname „total“ enthält, in einen Double-Wert umzuwandeln.

Spaltenmuster stehen aktuell für die Transformationen „Abgeleitete Spalte“, „Aggregieren“, „Auswählen“ und „Senke“ zur Verfügung.

## <a name="column-patterns-in-derived-column-and-aggregate"></a>Spaltenmuster in „Abgeleitete Spalte“ und „Aggregieren“

Wenn Sie ein Spaltenmuster in einer abgeleiteten Spalte oder auf der Registerkarte „Aggregate“ einer Aggregattransformation hinzufügen möchten, klicken Sie rechts neben einer vorhandenen Spalte auf das Plussymbol. Wählen Sie **Add column pattern** (Spaltenmuster hinzufügen) aus. 

![Spaltenmuster](media/data-flow/columnpattern.png "Spaltenmuster")

Verwenden Sie den [Ausdrucks-Generator](concepts-data-flow-expression-builder.md), um die Übereinstimmungsbedingung einzugeben. Erstellen Sie einen booleschen Ausdruck, um Spalten auf der Grundlage von `name`, `type`, `stream` und `position` der Spalte abzugleichen. Das Muster wirkt sich sowohl auf Spalten mit Datendrift als auch auf definierte Spalten aus, bei denen die Bedingung zutrifft (also „true“ zurückgegeben wird).

Die beiden Ausdrucksfelder unterhalb der Übereinstimmungsbedingung geben die neuen Namen und Werte der betroffenen Spalten an. Verwenden Sie `$$`, um auf den vorhandenen Wert des abgeglichenen Felds zu verweisen. Das linke Ausdrucksfeld definiert den Namen, das rechte den Wert.

![Spaltenmuster](media/data-flow/columnpattern2.png "Spaltenmuster")

Das obige Spaltenmuster gleicht jede Spalte vom Typ „Double“ ab und erstellt pro Übereinstimmung eine einzelne Aggregatspalte. Zur Benennung der neuen Spalte wird der Name der übereinstimmenden Spalte mit „_total“ verkettet. Der Wert der neuen Spalte ist die gerundete aggregierte Summe des vorhandenen Double-Werts.

Um zu überprüfen, ob die Übereinstimmungsbedingung korrekt ist, können Sie auf der Registerkarte **Untersuchen** das Ausgabeschema definierter Spalten überprüfen oder auf der Registerkarte **Datenvorschau** eine Momentaufnahme der Daten anzeigen. 

![Spaltenmuster](media/data-flow/columnpattern3.png "Spaltenmuster")

## <a name="rule-based-mapping-in-select-and-sink"></a>Regelbasierte Zuordnung in „Auswählen“ und „Senke“

Beim Zuordnen von Spalten in Quell- und Auswahltransformationen können Sie entweder eine feste Zuordnung oder regelbasierte Zuordnungen hinzufügen. Gleichen Sie basierend auf den Werten `name`, `type`, `stream` und `position` von Spalten ab. Es kann eine beliebige Kombination aus festen und regelbasierten Zuordnungen verwendet werden. Standardmäßig werden alle Projektionen mit mehr als 50 Spalten auf eine regelbasierte Zuordnung festgelegt, die einen Abgleich für jede Spalte vornimmt und den eingegebenen Namen ausgibt. 

Klicken Sie zum Hinzufügen einer regelbasierten Zuordnung auf **Zuordnung hinzufügen**, und wählen Sie **Rule-based mapping** (Regelbasierte Zuordnung) aus.

![Regelbasierte Zuordnung](media/data-flow/rule2.png "Regelbasierte Zuordnung")

Jede regelbasierte Zuordnung erfordert zwei Eingaben: die Bedingung, entsprechend der der Abgleich erfolgen soll, und den Namen jeder zugeordneten Spalte. Beide Werte werden über den [Ausdrucks-Generator](concepts-data-flow-expression-builder.md) eingegeben. Geben Sie im linken Ausdrucksfeld Ihre boolesche Übereinstimmungsbedingung ein. Geben Sie im rechten Ausdrucksfeld das Zuordnungsziel für die übereinstimmende Spalte an.

![Regelbasierte Zuordnung](media/data-flow/rule-based-mapping.png "Regelbasierte Zuordnung")

Verwenden Sie die `$$`-Syntax, um auf den eingegebenen Namen einer übereinstimmenden Spalte zu verweisen. Nehmen wir das vorstehende Bild als Beispiel: Ein Benutzer möchte, dass alle Zeichenfolgenspalten übereinstimmen, deren Namen kürzer als sechs Zeichen sind. Wenn eine eingehende Spalte `test` genannt wurde, wird die Spalte durch den Ausdruck `$$ + '_short'` in `test_short` umbenannt. Falls dies die einzige vorhandene Zuordnung ist, werden alle Spalten, die die Bedingung nicht erfüllen, aus den ausgegebenen Daten entfernt.

Muster stimmen sowohl mit abweichenden als auch mit definierten Spalten überein. Wenn Sie sehen möchten, welche definierten Spalten durch eine Regel zugeordnet werden, klicken Sie neben der Regel auf das Brillensymbol. Überprüfen Sie die Ausgabe anhand der Datenvorschau.

### <a name="regex-mapping"></a>Zuordnung mit regulärem Ausdruck

Wenn Sie auf das nach unten zeigende Chevronsymbol klicken, können Sie eine RegEx-Zuordnungsbedingung angeben. Eine RegEx-Zuordnungsbedingung stimmt mit allen Spaltennamen überein, die mit der angegebenen RegEx-Bedingung übereinstimmen. Sie kann in Kombination mit standardmäßigen regelbasierten Zuordnungen verwendet werden.

![Regelbasierte Zuordnung](media/data-flow/regex-matching.png "Regelbasierte Zuordnung")

Das vorstehende Beispiel entspricht dem RegEx-Muster `(r)` oder einem beliebigen Spaltennamen, der den Kleinbuchstaben „r“ enthält. Ähnlich wie bei der standardmäßigen regelbasierten Zuordnung werden alle übereinstimmenden Spalten von der Bedingung auf der rechten Seite mit der `$$`-Syntax geändert.

### <a name="rule-based-hierarchies"></a>Regelbasierte Hierarchien

Wenn Ihre definierte Projektion eine Hierarchie hat, können Sie die Unterspalten mithilfe der regelbasierten Zuordnung der Hierarchie zuordnen. Geben Sie eine Übereinstimmungsbedingung und die komplexe Spalte an, deren Unterspalten Sie zuordnen möchten. Jede übereinstimmende Unterspalte wird mithilfe der rechts angegebenen Regel „Name as“ (Benennen als) ausgegeben.

![Regelbasierte Zuordnung](media/data-flow/rule-based-hierarchy.png "Regelbasierte Zuordnung")

Das vorstehende Beispiel wird mit allen Unterspalten der komplexen Spalte `a` abgeglichen. `a` enthält die beiden Unterspalten `b` und `c`. Das Ausgabeschema enthält die beiden Spalten `b` und `c`, da die „Name as“-Bedingung `$$` lautet.

## <a name="pattern-matching-expression-values"></a>Ausdruckswerte für den Musterabgleich

* `$$` wird zur Laufzeit in den Namen oder Wert der jeweiligen Übereinstimmung umgewandelt.
* `name` stellt den Namen der einzelnen eingehenden Spalten dar.
* `type` stellt den Datentyp der einzelnen eingehenden Spalten dar.
* `stream` stellt den Namen dar, der dem jeweiligen Datenstrom oder der Transformation in Ihrem Flow zugeordnet ist.
* `position` ist die Ordinalposition von Spalten in Ihrem Datenfluss.

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich ausführlicher über die [Ausdruckssprache](data-flow-expression-functions.md) von Mappingdatenflüssen für Datentransformationen.
* Verwenden Sie Spaltenmuster in der [Senkentransformation](data-flow-sink.md) und der [Auswahltransformation](data-flow-select.md) mit regelbasierter Zuordnung.
