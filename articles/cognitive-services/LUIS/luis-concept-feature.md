---
title: Features – LUIS
description: Fügen Sie einem Sprachmodell Features hinzu, um Hinweise zur Erkennung von Eingaben, die Sie bezeichnen oder klassifizieren möchten, bereitzustellen.
ms.topic: conceptual
ms.date: 06/10/2020
ms.openlocfilehash: 823c51f0b58481e30ff54814dde03285ad094b9e
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84677590"
---
# <a name="machine-learning-ml-features"></a>Machine Learning-Features (ML)

Beim maschinellen Lernen ist ein  **Feature**  ein eindeutiges Merkmal oder Attribut der Daten, die Ihr System untersucht und durch die es lernt.

Features des maschinellen Lernens liefern für LUIS wichtige Hinweise dazu, wo nach Informationen gesucht werden sollte, die für ein Konzept den Unterschied ausmachen. Hierbei handelt es sich um Hinweise, die von LUIS befolgt werden können, aber nicht um feste Regeln.  Diese Hinweise werden zusammen mit den Bezeichnungen genutzt, um Daten finden zu können.

## <a name="what-is-a-feature"></a>Was ist ein Feature?

Ein Feature ist ein eindeutiges Merkmal, das als Funktion bezeichnet werden kann: f(x) = y. Das Feature wird verwendet, um zu erfahren, wo in der Beispieläußerung nach dem unterscheidenden Merkmal zu suchen ist. Was wissen Sie bei Erstellung Ihres Schemas über die Beispieläußerung, die das Merkmal anzeigt? Ihre Antwort ist die beste Anleitung zum Erstellen von Features.

## <a name="types-of-features"></a>Typen von Features

 LUIS unterstützt als Features sowohl Ausdruckslisten als auch Modelle:
* Feature „Ausdrucksliste“
* Modell (Absicht oder Entität) als Feature

Features sollten als erforderlicher Bestandteil Ihres Schemaentwurfs angesehen werden.

## <a name="how-you-find-features-in-your-example-utterances"></a>Auffinden von Features in Ihren Beispieläußerungen

Da LUIS eine sprachbasierte Anwendung ist, sind die Features textbasiert. Wählen Sie Text aus, der das Merkmal anzeigt, das Sie unterscheiden möchten. Für LUIS ist die kleinste textbasierte Einheit das Token. Für die englische Sprache ist ein Token eine angrenzende Abfolge (ohne Leerzeichen oder Interpunktion) von Buchstaben und Zahlen. Ein Leerzeichen ist kein Token.

Da Leerzeichen und Interpunktionszeichen keine Token sind, konzentrieren Sie sich auf die Texthinweise, die Sie als Features verwenden können. Denken Sie daran, Variationen von Wörtern aufzunehmen, wie:
* Pluralformen
* Zeitformen von Verben
* Abkürzungen
* Rechtschreibung und falsche Schreibweisen

Muss der Text als unterscheidendes Merkmal Folgendes erfüllen:
* Genaue Übereinstimmung mit einem Wort oder Ausdruck: Erwägen Sie das Hinzufügen einer regulären Ausdrucksentität oder einer Listenentität als Feature zu der Entität oder Absicht.
* Übereinstimmung mit einem bekannten Konzept wie Datumsangaben, Uhrzeiten oder Personennamen: Verwenden Sie eine vorgefertigte Entität als Feature für die Entität oder Absicht.
* Lernen neuer Beispiele im Laufe der Zeit: Verwenden Sie eine Ausdrucksliste mit einigen Beispielen des Konzepts als ein Feature für die Entität oder Absicht.

## <a name="combine-features"></a>Kombinieren von Features

Da es mehrere Möglichkeiten gibt, wie ein Merkmal beschrieben wird, können Sie mehr als ein Feature verwenden, das zur Beschreibung dieses Merkmals oder Konzepts beiträgt. Eine gängige Paarung besteht darin, ein Ausdruckslistenfeature und einen der häufig als Feature verwendeten Entitätstypen zu verwenden: vorgefertigte Entität, reguläre Ausdrucksentität oder Listenentität.

### <a name="ticket-booking-entity-example"></a>Beispiel: Ticketbuchungsentität

Sehen Sie sich als erstes Beispiel eine App für die Buchung eines Flugs mit einer Flugreservierungsabsicht und einer Ticketbuchungsentität an.

Die Ticketbuchungsentität ist eine durch maschinelles Lernen erworbene Entität für das Flugziel. Um den Ort zu extrahieren, ziehen Sie zwei Features zur Hilfe heran:
* Ausdrucksliste relevanter Wörter wie `plane`, `flight`, `reservation`, `ticket`
* Vorgefertigte Entität `geographyV2` als Feature der Entität

### <a name="pizza-entity-example"></a>Beispiel: Pizza-Entität

Sehen Sie sich als weiteres Beispiel eine App für die Bestellung einer Pizza mit der Absicht „Pizzabestellung erstellen“ und einer Pizza-Entität an.

Die Pizza-Entität ist eine durch maschinelles Lernen erworbene Entität für die Pizza-Details. Um die Details zu extrahieren, ziehen Sie zwei Features zur Hilfe heran:
* Ausdrucksliste relevanter Wörter wie `cheese`, `crust`, `pepperoni`, `pineapple`
* Vorgefertigte Entität `number` als Feature der Entität

## <a name="a-phrase-list-for-a-particular-concept"></a>Eine Ausdrucksliste für ein bestimmtes Konzept

Eine Ausdrucksliste ist eine Liste mit Wörtern oder Ausdrücken, die für ein bestimmtes Konzept steht und als Übereinstimmung ohne Berücksichtigung von Groß-/Kleinschreibung auf Tokenebene angewendet wird.

Beim Hinzufügen einer Ausdrucksliste können Sie das Feature wie folgt festlegen:
* **[Global](#global-features)** . Ein globales Feature gilt für die gesamte App.

### <a name="when-to-use-a-phrase-list"></a>Wann Sie Ausdruckslisten verwenden

Verwenden Sie eine Ausdrucksliste, wenn Ihre LUIS-App dazu in der Lage sein soll, zu generalisieren und neue Elemente für das Konzept zu identifizieren. Ausdruckslisten ähneln einem domänenspezifischen Vokabular, das zur Verbesserung des Verständnisses von Absichten und Entitäten beiträgt.

### <a name="how-to-use-a-phrase-list"></a>Verwenden von Ausdruckslisten

Bei einer Ausdrucksliste berücksichtigt LUIS den Kontext und verallgemeinert, um Elemente zu erkennen, die einander ähnlich sind, ohne eine genaue textliche Übereinstimmung darzustellen.

Schritte zur Verwendung einer Ausdrucksliste:
* Verwenden einer Machine Learning-Entität als Ausgangspunkt
    * Hinzufügen von Beispieläußerungen
    * Bezeichnen mit einer Machine Learning-Entität
* Hinzufügen einer Ausdrucksliste
    * Fügen Sie Wörter mit ähnlicher Bedeutung hinzu. Gehen Sie **nicht** so vor, dass Sie jedes mögliche Wort oder beliebige Ausdrücke hinzufügen. Fügen Sie jeweils einige Wörter oder Ausdrücke hinzu, trainieren Sie erneut, und veröffentlichen Sie.
    * Überprüfen und Hinzufügen von vorgeschlagenen Wörtern

### <a name="a-typical-scenario-for-a-phrase-list"></a>Typisches Szenario für eine Ausdrucksliste

Ein typisches Szenario für eine Ausdrucksliste ist die Verstärkung von Wörtern, die sich auf ein bestimmtes Thema beziehen.

Ein Beispiel für Wörter, für die zum Verstärken der Signifikanz ggf. eine Ausdrucksliste benötigt wird, sind medizinische Fachbegriffe. Die Begriffe können hierbei eine bestimmte physische, chemische, therapeutische oder abstrakte Bedeutung haben. Ohne Verwendung einer Ausdrucksliste weiß LUIS nicht, dass die Begriffe für Ihren Themenbereich wichtig sind.

Gehen Sie wie folgt vor, wenn Sie die medizinischen Fachbegriffe extrahieren möchten:
* Erstellen Sie zunächst Beispieläußerungen, und bezeichnen Sie die medizinischen Begriffe in diesen Äußerungen.
* Erstellen Sie anschließend eine Ausdrucksliste mit Beispielen für die Begriffe innerhalb des Themenbereichs. Diese Ausdrucksliste sollte den von Ihnen bezeichneten eigentlichen Begriff und andere Begriffe enthalten, mit denen dasselbe Konzept beschrieben wird.
* Fügen Sie die Ausdrucksliste der Entität oder untergeordneten Entität hinzu, von der das in der Ausdrucksliste verwendete Konzept extrahiert wird. Das häufigste Szenario ist eine Komponente (untergeordnetes Element) einer Machine Learning-Entität. Wenn die Ausdrucksliste übergreifend auf alle Absichten bzw. Entitäten angewendet werden soll, sollten Sie sie als globale Ausdrucksliste kennzeichnen. Das `enabledForAllModels`-Flag steuert diesen Modellbereich in der API.

### <a name="token-matches-for-a-phrase-list"></a>Tokenübereinstimmungen für eine Ausdrucksliste

Eine Ausdrucksliste wird ohne Berücksichtigung der Groß-/Kleinschreibung auf Tokenebene angewendet. Das folgende Diagramm zeigt, wie eine Ausdrucksliste, die das Wort `Ann` enthält, auf Variationen derselben Zeichen in dieser Reihenfolge angewendet wird.


| Tokenvariation von `Ann`. | Ausdruckslistenübereinstimmung, wenn Token gefunden wird |
|--------------------------|---------------------------------------|
| ANN<br>aNN<br>           | Ja, das Token ist `Ann`.                  |
| Ann's                    | Ja, das Token ist `Ann`.                  |
| Anne                     | Nein, das Token ist `Anne`.                  |


<a name="how-to-use-phrase-lists"></a>
<a name="how-to-use-a-phrase-lists"></a>
<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="a-model-as-a-feature-helps-another-model"></a>Ein Modell als Feature dient als Unterstützung für ein anderes Modell

Sie können ein Modell (Absicht oder Entität) einem anderen Modell (Absicht oder Entität) als Feature hinzufügen. Indem Sie eine vorhandene Absicht oder Entität als Feature hinzufügen, sorgen Sie für ein klar definiertes Konzept mit bezeichneten Beispielen.

Beim Hinzufügen eines Modells als Feature können Sie das Feature wie folgt festlegen:
* **[Erforderlich](#required-features)** : Es muss ein erforderliches Feature gefunden werden, damit das Modell vom Vorhersageendpunkt zurückgegeben wird.
* **[Global](#global-features)** . Ein globales Feature gilt für die gesamte App.

### <a name="when-to-use-an-entity-as-a-feature-to-an-intent"></a>Verwenden einer Entität als Feature für eine Absicht

Fügen Sie eine Entität einer Absicht als Feature hinzu, wenn die Erkennung dieser Entität für die Absicht wichtig ist.

Wenn die Absicht beispielsweise das Buchen eines Flugs (`BookFlight`) und die Entität die Ticketinformationen sind (z. B. Platzanzahl, Abflugort und Ankunftsort), würde die Ermittlung der Entität mit den Ticketinformationen der Vorhersage der Flugbuchungsabsicht (`BookFlight`) ein erheblich höheres Gewicht verleihen.

### <a name="when-to-use-an-entity-as-a-feature-to-another-entity"></a>Verwenden einer Entität als Feature für eine andere Entität

Eine Entität (A) sollte einer anderen Entität (B) als Feature hinzugefügt werden, wenn die Erkennung dieser Entität (A) für die Vorhersage von Entität (B) wichtig ist.

Wenn eine Lieferadressen-Entität beispielsweise eine untergeordnete Anschrift-Entität enthält, verleiht die Ermittlung der untergeordneten Anschrift-Entität der Vorhersage der Lieferadressen-Entität ein erheblich höheres Gewicht.

* Lieferadresse (durch maschinelles Lernen erworbene Entität)
    * Hausnummer (untergeordnete Entität)
    * Anschrift (untergeordnete Entität)
    * Ort (untergeordnete Entität)
    * Bundesland oder Kanton (untergeordnete Entität)
    * Länder/Regionen (untergeordnete Entität)
    * Postleitzahl (untergeordnete Entität)

## <a name="nested-subentities-with-features"></a>Geschachtelte untergeordnete Entitäten mit Features

Eine durch maschinelles Lernen erworbene untergeordnete Entität gibt an, dass ein Konzept für die übergeordnete Entität vorhanden ist. Dabei spielt es keine Rolle, ob es sich bei dem übergeordneten Element um eine weitere untergeordnete Entität oder um die oberste Entität handelt. Der Wert der untergeordneten Entität fungiert als Feature für das übergeordnete Element.

Eine untergeordnete Entität kann als Feature sowohl eine Ausdrucksliste als auch ein Modell (eine andere Entität) besitzen.

Wenn die untergeordnete Entität über eine Ausdrucksliste verfügt, wird dadurch das Vokabular des Konzepts verstärkt, der JSON-Antwort der Vorhersage werden jedoch keinerlei Informationen hinzugefügt.

Wenn die untergeordnete Entität über ein Feature einer anderen Entität verfügt, enthält die JSON-Antwort die extrahierten Daten dieser anderen Entität.

## <a name="required-features"></a>Erforderliche Features

Es muss ein erforderliches Feature gefunden werden, damit das Modell vom Vorhersageendpunkt zurückgegeben wird. Verwenden Sie ein erforderliches Feature, wenn Sie wissen, dass die eingehenden Daten mit dem Feature übereinstimmen müssen.

Wenn der Text der Äußerung nicht dem erforderlichen Feature entspricht, wird er nicht extrahiert.

**Für ein erforderliches Feature wird eine nicht durch maschinelles Lernen erworbene Entität verwendet**:
* Entität vom Typ „RegEx“
* Entität vom Typ „List“
* Vordefinierte Entität

Was sind gute Features, die als erforderlich gekennzeichnet werden sollten? Legen Sie das Feature entsprechend fest, wenn Sie sich sicher sind, dass Ihr Modell in den Daten zu finden ist. Für ein erforderliches Feature wird nichts zurückgegeben, wenn es nicht gefunden wird.

Wir verwenden wieder das Beispiel für die Lieferadresse:
* Lieferadresse (durch maschinelles Lernen erworbene Entität)
    * Hausnummer (untergeordnete Entität)
    * Anschrift (untergeordnete Entität)
    * Straße (untergeordnete Entität)
    * Ort (untergeordnete Entität)
    * Bundesland oder Kanton (untergeordnete Entität)
    * Länder/Regionen (untergeordnete Entität)
    * Postleitzahl (untergeordnete Entität)

### <a name="required-feature-using-prebuilt-entities"></a>Erforderliches Feature mit vordefinierten Entitäten

Bei Ort, Bundesland/Kanton und Land/Region handelt es sich meist um feste Listen, die sich in der Regel nur selten ändern. Unter Umständen verfügen diese Entitäten über die relevanten empfohlenen Features, und diese Features können dann als erforderlich gekennzeichnet werden. Dies bedeutet, dass die gesamte Lieferadresse nicht zurückgegeben wird, wenn die Entitäten mit den erforderlichen Features nicht gefunden werden.

Was passiert, wenn Ort, Bundesland/Kanton oder Land/Region in der Äußerung enthalten sind, sich aber an einem Ort oder in einem Slangausdruck befinden, der von LUIS nicht erwartet wird? Falls Sie eine Nachbearbeitung als Hilfe bei der Auflösung der Entität durchführen möchten, weil von LUIS eine niedrige Zuverlässigkeitsbewertung vergeben wurde, sollten Sie das Feature nicht als erforderlich kennzeichnen.

Ein weiteres Beispiel für ein erforderliches Feature für die Lieferadresse ist, die Hausnummer zu einer erforderlichen [vordefinierten](luis-reference-prebuilt-entities.md) Nummer zu machen. Benutzer können dann entweder „1 Microsoft Way“ oder „One Microsoft Way“ eingeben. In beiden Fällen wird dies für die untergeordnete Entität „Hausnummer“ richtig als Hausnummer „1“ aufgelöst.

### <a name="required-feature-using-list-entities"></a>Erforderliches Feature bei Verwendung von Listenentitäten

Eine [Listenentität](reference-entity-list.md) wird als Liste mit kanonischen Namen und den zugehörigen Synonymen verwendet. Wenn die Äußerung als erforderliches Feature nicht entweder den kanonischen Namen oder ein Synonym enthält, wird die Entität nicht als Teil des Vorhersageendpunkts zurückgegeben.

Nehmen Sie für das Beispiel mit der Lieferadresse an, dass Ihr Unternehmen nur in bestimmte Länder/Regionen liefert. Sie können eine Listenentität erstellen, bei der es mehrere Möglichkeiten gibt, wie Ihre Kunden das Land angeben können. Falls LUIS im Text der Äußerung keine genaue Übereinstimmung findet, wird die Entität (mit dem erforderlichen Feature der Listenentität) in der Vorhersage nicht zurückgegeben.

|Kanonischer Name|Synonyme|
|--|--|
|USA|USA<br>U.S.A<br>US<br>USA<br>0|

Die Clientanwendung (beispielsweise ein Chatbot) kann eine Folgefrage stellen, damit der Kunde versteht, dass die Länder-/Regionsauswahl eingeschränkt und _erforderlich_ ist.

### <a name="required-feature-using-regular-expression-entities"></a>Erforderliches Feature mit Entitäten für reguläre Ausdrücke

Eine [Entität vom Typ „Regulärer Ausdruck“](reference-entity-regular-expression.md), die als erforderliches Feature verwendet wird, verfügt über umfassende Funktionen zum Abgleichen von Text.

Im Beispiel mit der Lieferadresse können Sie einen regulären Ausdruck für die Syntaxregeln der Postleitzahlen des Landes/der Region erstellen.

## <a name="global-features"></a>Globale Features

Die häufigste Verwendung ist die Anwendung eines Features auf ein bestimmtes Modell, aber Sie können das Feature auch als **globales Feature** konfigurieren, um es auf Ihre gesamte Anwendung anzuwenden.

Die häufigste Verwendung eines globalen Features ist das Hinzufügen von zusätzlichem Vokabular, z. B. Wörter in einer anderen Sprache, zur App. Wenn Ihre Kunden hauptsächlich eine bestimmte Sprache nutzen, gleichzeitig aber erwarten, dass sie innerhalb derselben Äußerung noch eine weitere Sprache verwenden können, können Sie ein Feature für Wörter der zweiten Sprache hinzufügen.

Da vom Benutzer erwartet wird, dass er die zweite Sprache für alle Absichten oder Entitäten nutzen kann, sollte dies im Rahmen einer Ausdrucksliste hinzugefügt und diese Liste als globales Feature konfiguriert werden.

## <a name="best-practices"></a>Bewährte Methoden
Informationen zu [Best Practices](luis-concept-best-practices.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erweitern](schema-change-prediction-runtime.md) Ihrer App-Modelle zur Vorhersagelaufzeit
* Weitere Informationen zum Hinzufügen von Features zu LUIS-Apps finden Sie unter [Hinzufügen von Features](luis-how-to-add-features.md).
