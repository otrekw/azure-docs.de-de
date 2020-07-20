---
title: Features – LUIS
description: Fügen Sie einem Sprachmodell Features hinzu, um Hinweise zur Erkennung von Eingaben, die Sie bezeichnen oder klassifizieren möchten, bereitzustellen.
ms.topic: conceptual
ms.date: 06/10/2020
ms.openlocfilehash: fbf39382e418bef9a7d39886076a4100a26ce3e7
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2020
ms.locfileid: "85362457"
---
# <a name="machine-learning-features"></a>Features des maschinellen Lernens

Beim maschinellen Lernen ist ein  *Feature*  ein eindeutiges Merkmal oder Attribut der Daten, die Ihr System untersucht und durch die es lernt.

Features des maschinellen Lernens liefern LUIS wichtige Hinweise dazu, wo nach Informationen gesucht werden soll, die ein Konzept von anderen unterscheiden. Hierbei handelt es sich um Hinweise, die von LUIS genutzt werden können, nicht um feste Regeln. LUIS verwendet diese Hinweise zusammen mit Bezeichnungen, um Daten zu finden.

Ein Feature lässt sich als Funktion beschreiben, z. B. f(x) = y. In einer Beispieläußerung informiert das Feature darüber, wo nach dem unterscheidenden Merkmal zu suchen ist. Verwenden Sie diese Informationen, um Ihr Schema zu erstellen.

## <a name="types-of-features"></a>Typen von Features

LUIS unterstützt als Features sowohl Ausdruckslisten als auch Modelle:

* Feature „Ausdrucksliste“ 
* Modell (Absicht oder Entität) als Feature

Features sollten als erforderlicher Bestandteil Ihres Schemaentwurfs angesehen werden.

## <a name="find-features-in-your-example-utterances"></a>Auffinden von Features in Beispieläußerungen

Da LUIS eine sprachbasierte Anwendung ist, sind die Features textbasiert. Wählen Sie Text aus, der das Merkmal anzeigt, das Sie unterscheiden möchten. Für LUIS ist die kleinste Einheit das *Token*. In der Linguistik ist ein Token eine zusammenhängende Abfolge von Buchstaben und Ziffern ohne Leerzeichen oder Satzzeichen.

Da Leerzeichen und Interpunktionszeichen keine Token sind, konzentrieren Sie sich auf die Texthinweise, die Sie als Features verwenden können. Denken Sie daran, Variationen von Wörtern aufzunehmen, wie beispielsweise:

* Pluralformen
* Zeitformen von Verben
* Abkürzungen
* Verschiedene Schreibweisen, auch falsche Schreibungen

Ermitteln Sie, ob für den Text Folgendes gelten muss, weil er ein Merkmal unterscheidet:

* Exakte Übereinstimmung mit einem Wort oder Ausdruck: Erwägen Sie das Hinzufügen einer regulären Ausdrucksentität oder einer Listenentität als Feature zu der Entität oder Absicht.
* Übereinstimmung mit einem bekannten Konzept wie Datumsangaben, Uhrzeiten oder Namen von Personen: Verwenden Sie eine vorgefertigte Entität als Feature für die Entität oder Absicht.
* Lernen neuer Beispiele im Lauf der Zeit: Verwenden Sie eine Ausdrucksliste mit einigen Beispielen des Konzepts als Feature für die Entität oder Absicht.

## <a name="combine-features"></a>Kombinieren von Features

Sie können mehrere Features verwenden, um ein Merkmal oder Konzept zu beschreiben. Ein gängige Kombination ist die Verwendung eines Ausdruckslistenfeatures und eines Entitätstyps, der häufig als Feature verwendet wird:

 * Vordefinierte Entität
 * Entität vom Typ „Regulärer Ausdruck“
 * Entität vom Typ „List“

### <a name="ticket-booking-entity-example"></a>Beispiel: Ticketbuchungsentität

Sehen Sie sich als erstes Beispiel eine App für die Buchung eines Flugs mit einer Flugreservierungsabsicht und einer Ticketbuchungsentität an.

Die Ticketbuchungsentität ist eine Entität des maschinellen Lernens für das Flugziel. Um den Ort zu extrahieren, ziehen Sie zwei Features zur Hilfe heran:

* Eine Ausdrucksliste relevanter Wörter wie **Flugzeug**, **Flug**, **Buchung** oder **Ticket**
* Eine vorgefertigte Entität **geographyV2** als Feature für die Entität

### <a name="pizza-entity-example"></a>Beispiel: Pizza-Entität

Sehen Sie sich als weiteres Beispiel eine App für die Bestellung einer Pizza mit der Absicht „Pizzabestellung erstellen“ und einer Entität „Pizza“ an.

Die Entität „Pizza“ ist eine Entität des maschinellen Lernens für die Details zur Pizza. Um die Details zu extrahieren, ziehen Sie zwei Features heran:

* Eine Ausdrucksliste relevanter Wörter wie **Käse**, **Rand**, **Peperoni** oder **Ananas**
* Eine vorgefertigte Entität **number** als Feature für die Entität

## <a name="create-a-phrase-list-for-a-concept"></a>Erstellen einer Liste von Ausdrücken für ein Konzept

Eine Ausdrucksliste ist eine Liste mit Wörtern oder Ausdrücken, die ein Konzept beschreiben. Eine Ausdrucksliste wird auf Tokenebene als Übereinstimmung ohne Beachtung der Groß- und Kleinschreibung angewendet.

Beim Hinzufügen einer Ausdrucksliste können Sie das Feature als **[global](#global-features)** festlegen. Ein globales Feature gilt für die gesamte App.

### <a name="when-to-use-a-phrase-list"></a>Wann Sie Ausdruckslisten verwenden

Verwenden Sie eine Ausdrucksliste, wenn Ihre LUIS-App verallgemeinern und neue Elemente für das Konzept identifizieren soll. Ausdruckslisten sind wie ein fachgebietsspezifischer Wortschatz. Sie verbessern das Verständnis von Absichten und Entitäten.

### <a name="how-to-use-a-phrase-list"></a>Verwenden von Ausdruckslisten

Bei einer Ausdrucksliste berücksichtigt LUIS den Kontext und verallgemeinert, um Elemente zu identifizieren, die einander ähnlich sind, aber keine genaue textliche Übereinstimmung darstellen. Um eine Ausdrucksliste zu verwenden, führen Sie folgende Schritte aus:

1. Beginnen Sie mit einer Entität des maschinellen Lernens:
    1. Fügen Sie Beispieläußerungen hinzu.
    1. Bezeichnen Sie diese mit einer Entität des maschinellen Lernens.
1. Fügen Sie eine Ausdrucksliste hinzu:
    1. Fügen Sie Wörter mit ähnlicher Bedeutung hinzu. Fügen Sie nicht jedes denkbare Wort oder jeden denkbaren Ausdruck hinzu. Fügen Sie immer nur einige Wörter oder Ausdrücke hinzu. Dann trainieren Sie die App erneut und veröffentlichen sie.
    1. Überprüfen Sie das Ergebnis, und fügen Sie vorgeschlagene Wörter hinzu.

### <a name="a-typical-scenario-for-a-phrase-list"></a>Typisches Szenario für eine Ausdrucksliste

Ein typisches Szenario für eine Ausdrucksliste ist die Verstärkung von Wörtern, die sich auf ein bestimmtes Thema beziehen.

Medizinische Fachbegriffe sind ein gutes Beispiel für Wörter, für die zum Verstärken der Signifikanz ggf. eine Ausdrucksliste benötigt wird. Die Begriffe können eine bestimmte physikalische, chemische, therapeutische oder abstrakte Bedeutung haben. Ohne Verwendung einer Ausdrucksliste weiß LUIS nicht, dass die Begriffe für Ihren Themenbereich wichtig sind.

So extrahieren Sie medizinische Fachbegriffe:

1. Erstellen Sie Beispieläußerungen, und bezeichnen Sie die medizinischen Begriffe in diesen Äußerungen.
2. Erstellen Sie eine Ausdrucksliste mit Beispielen für die Begriffe innerhalb des Themenbereichs. Diese Ausdrucksliste sollte den von Ihnen bezeichneten eigentlichen Begriff und andere Begriffe enthalten, mit denen dasselbe Konzept beschrieben wird.
3. Fügen Sie die Ausdrucksliste der Entität oder untergeordneten Entität hinzu, von der das in der Ausdrucksliste verwendete Konzept extrahiert wird. Das häufigste Szenario ist eine Komponente (untergeordnetes Element) einer Machine Learning-Entität. Wenn die Ausdrucksliste übergreifend auf alle Absichten bzw. Entitäten angewendet werden soll, kennzeichnen Sie sie als globale Ausdrucksliste. Das **enabledForAllModels**-Flag steuert diesen Modellbereich in der API.

### <a name="token-matches-for-a-phrase-list"></a>Tokenübereinstimmungen für eine Ausdrucksliste

Eine Ausdrucksliste wird immer auf Tokenebene angewendet. Die folgende Tabelle zeigt, wie eine Ausdrucksliste, die das Wort **Anne** enthält, auf Variationen derselben Zeichen in dieser Reihenfolge angewendet wird.


| Tokenvariation von **Anne**. | Übereinstimmung mit der Ausdrucksliste, wenn das Token gefunden wird |
|--------------------------|---------------------------------------|
| **ANNE**<br>**aNNe**<br>           | Ja, das Token ist **Anne**.                  |
| **Annes**                    | Ja, das Token ist **Anne**.                  |
| **Anna**                     | Nein, das Token ist **Anne**.                  |

<a name="how-to-use-phrase-lists"></a>
<a name="how-to-use-a-phrase-lists"></a>
<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="a-model-as-a-feature-helps-another-model"></a>Ein Modell als Feature dient als Unterstützung für ein anderes Modell

Sie können ein Modell (Absicht oder Entität) einem anderen Modell (Absicht oder Entität) als Feature hinzufügen. Indem Sie eine vorhandene Absicht oder Entität als Feature hinzufügen, erstellen Sie ein klar definiertes Konzept mit bezeichneten Beispielen.

Beim Hinzufügen eines Modells als Feature können Sie das Feature wie folgt festlegen:
* **[Erforderlich](#required-features)** : Es muss ein erforderliches Feature gefunden werden, damit das Modell vom Vorhersageendpunkt zurückgegeben wird.
* **[Global](#global-features)** . Ein globales Feature gilt für die gesamte App.

### <a name="when-to-use-an-entity-as-a-feature-to-an-intent"></a>Verwenden einer Entität als Feature für eine Absicht

Fügen Sie eine Entität einer Absicht als Feature hinzu, wenn die Erkennung dieser Entität für die Absicht wichtig ist.

Wenn die Absicht beispielsweise das Buchen eines Flugs (**BookFlight**) ist und die Entität die Ticketinformationen sind (z. B. Sitzplatznummer, Abflugort und Ankunftsort), würde die Ermittlung der Entität mit den Ticketinformationen der Vorhersage der Flugbuchungsabsicht (**BookFlight**) ein erheblich höheres Gewicht verleihen.

### <a name="when-to-use-an-entity-as-a-feature-to-another-entity"></a>Verwenden einer Entität als Feature für eine andere Entität

Eine Entität (A) sollte einer anderen Entität (B) als Feature hinzugefügt werden, wenn die Erkennung dieser Entität (A) für die Vorhersage von Entität (B) wichtig ist.

Wenn eine Entität für Lieferadressen beispielsweise in einer untergeordnete Anschriftenentität enthalten ist, verleiht die Ermittlung der untergeordneten Anschriftenentität der Vorhersage der Lieferadressenentität ein erheblich höheres Gewicht.

* Lieferadresse (Entität des maschinellen Lernens):

    * Hausnummer (untergeordnete Entität)
    * Anschrift (untergeordnete Entität)
    * Ort (untergeordnete Entität)
    * Bundesland oder Kanton (untergeordnete Entität)
    * Länder/Regionen (untergeordnete Entität)
    * Postleitzahl (untergeordnete Entität)

## <a name="nested-subentities-with-features"></a>Geschachtelte untergeordnete Entitäten mit Features

Eine untergeordnete Entität des maschinellen Lernens weist darauf hin, dass für die übergeordnete Entität ein Konzept vorhanden ist. Die übergeordnete Entität kann eine untergeordnete Entität oder die oberste Entität sein. Der Wert der untergeordneten Entität fungiert als Feature für das übergeordnete Element.

Eine untergeordnete Entität kann als Feature sowohl eine Ausdrucksliste als auch ein Modell (eine andere Entität) aufweisen.

Wenn die untergeordnete Entität über eine Ausdrucksliste verfügt, wird das Vokabular des Konzepts verstärkt, der JSON-Antwort der Vorhersage werden jedoch keinerlei Informationen hinzugefügt.

Wenn die untergeordnete Entität über ein Feature einer anderen Entität verfügt, enthält die JSON-Antwort die extrahierten Daten dieser anderen Entität.


## <a name="required-features"></a>Erforderliche Features

Es muss ein erforderliches Feature gefunden werden, damit das Modell vom Vorhersageendpunkt zurückgegeben wird. Verwenden Sie ein erforderliches Feature, wenn Sie wissen, dass die eingehenden Daten mit dem Feature übereinstimmen müssen.

Wenn der Text der Äußerung nicht mit dem erforderlichen Feature übereinstimmt, wird er nicht extrahiert.

Für ein erforderliches Feature wird eine nicht durch maschinelles Lernen erworbene Entität verwendet:

* Entität vom Typ „Regulärer Ausdruck“
* Entität vom Typ „List“
* Vordefinierte Entität

Wenn Sie sicher sind, dass Ihr Modell in den Daten zu finden ist, legen Sie das Feature als erforderlich fest. Für ein erforderliches Feature wird nichts zurückgegeben, wenn es nicht gefunden wird.

Wir verwenden wieder das Beispiel für die Lieferadresse:

Lieferadresse (durch maschinelles Lernen erworbene Entität)

 * Hausnummer (untergeordnete Entität) 
 * Anschrift (untergeordnete Entität) 
 * Straße (untergeordnete Entität) 
 * Ort (untergeordnete Entität) 
 * Bundesland oder Kanton (untergeordnete Entität) 
 * Länder/Regionen (untergeordnete Entität) 
 * Postleitzahl (untergeordnete Entität)

### <a name="required-feature-using-prebuilt-entities"></a>Erforderliches Feature mit vordefinierten Entitäten

Bei Ort, Bundesland/Kanton und Land/Region handelt es sich meist um feste Listen, die sich in der Regel nur selten ändern. Unter Umständen verfügen diese Entitäten über die relevanten empfohlenen Features, und diese Features können dann als erforderlich gekennzeichnet werden. Das bedeutet, dass die gesamte Lieferadresse nicht zurückgegeben wird, wenn die Entitäten mit erforderlichen Features nicht gefunden werden.

Was passiert, wenn Ort, Bundesland/Kanton oder Land/Region in der Äußerung enthalten sind, sich aber an einem Ort oder in einem Slangausdruck befinden, der von LUIS nicht erwartet wird? Falls Sie eine Nachverarbeitung als Hilfe bei der Auflösung der Entität durchführen möchten, weil von LUIS eine niedrige Zuverlässigkeitsbewertung vergeben wurde, kennzeichnen Sie das Feature nicht als erforderlich.

Ein weiteres Beispiel für ein erforderliches Feature für die Lieferadresse ist es, die Hausnummer als erforderliche [vordefinierte](luis-reference-prebuilt-entities.md) Zahl festzulegen. Benutzer können dann entweder „1 Microsoft Way“ oder „One Microsoft Way“ eingeben. Beide Fälle werden für die untergeordnete Entität „Hausnummer“ als Ziffer „1“ aufgelöst.

### <a name="required-feature-using-list-entities"></a>Erforderliches Feature bei Verwendung von Listenentitäten

Eine [Listenentität](reference-entity-list.md) wird als Liste mit kanonischen Namen und den zugehörigen Synonymen verwendet. Wenn die Äußerung als erforderliches Feature nicht entweder den kanonischen Namen oder ein Synonym enthält, wird die Entität nicht als Teil des Vorhersageendpunkts zurückgegeben.

Nehmen Sie einmal an, Ihr Unternehmen liefert nur an einige Länder oder Regionen. Sie können eine Listenentität erstellen, bei der es mehrere Möglichkeiten gibt, wie Ihre Kunden das Land bzw. die Region angeben können. Falls LUIS im Text der Äußerung keine genaue Übereinstimmung findet, wird die Entität (mit dem erforderlichen Feature der Listenentität) in der Vorhersage nicht zurückgegeben.

|Kanonischer Name|Synonyme|
|--|--|
|USA|USA<br>U.S.A<br>US<br>USA<br>0|

Eine Clientanwendung, wie z. B. ein Chatbot, kann zur Unterstützung eine Folgefrage stellen. Dies hilft dem Kunden zu verstehen, dass die Auswahl von Land und Region begrenzt und *erforderlich* ist.

### <a name="required-feature-using-regular-expression-entities"></a>Erforderliches Feature mit Entitäten für reguläre Ausdrücke

Eine [Entität vom Typ „Regulärer Ausdruck“](reference-entity-regular-expression.md), die als erforderliches Feature verwendet wird, bietet umfassende Möglichkeiten zum Abgleichen von Text.

Im Beispiel mit der Lieferadresse können Sie einen regulären Ausdruck für die Syntaxregeln der Postleitzahlen des Landes/der Region erstellen.

## <a name="global-features"></a>Globale Features

Die häufigste Verwendung ist die Anwendung eines Features auf ein bestimmtes Modell, aber Sie können das Feature auch als **globales Feature** konfigurieren, um es auf Ihre gesamte Anwendung anzuwenden.

Die häufigste Verwendung eines globalen Features ist das Hinzufügen von zusätzlichem Vokabular zur App. Wenn Ihre Kunden beispielsweise hauptsächlich eine bestimmte Sprache nutzen, gleichzeitig aber erwarten, dass sie innerhalb derselben Äußerung noch eine weitere Sprache verwenden können, können Sie ein Feature für Wörter der zweiten Sprache hinzufügen.

Da die Benutzer erwarten, die zweite Sprache in allen Absichten oder Entitäten verwenden zu können, fügen Sie Wörter aus der zweiten Sprache zur Ausdrucksliste hinzu. Konfigurieren Sie die Ausdrucksliste als globales Feature.

## <a name="best-practices"></a>Bewährte Methoden

Informationen zu [Best Practices](luis-concept-best-practices.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erweitern](schema-change-prediction-runtime.md) Sie Ihre App-Modelle zur Laufzeit der Vorhersage.
* Weitere Informationen zum Hinzufügen von Features zu LUIS-Apps finden Sie unter [Hinzufügen von Features](luis-how-to-add-features.md).
