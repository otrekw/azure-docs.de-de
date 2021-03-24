---
title: Mustersyntaxreferenz – LUIS
description: Erstellen Sie Entitäten, um die Schlüsseldaten aus Benutzeräußerungen in Language Understanding-Apps (LUIS) zu extrahieren. Extrahierte Daten werden von der Clientanwendung verwendet.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 04/14/2020
ms.openlocfilehash: 3caccd6766226ce68b371856b081b052c1033f71
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91542188"
---
# <a name="pattern-syntax"></a>Mustersyntax

Die Mustersyntax ist eine Vorlage für eine Äußerung. Die Vorlage sollte sowohl abzugleichende Wörter und Entitäten als auch zu ignorierende Wörter und [Interpunktion](luis-reference-application-settings.md#punctuation-normalization) enthalten. Sie ist **kein** regulärer Ausdruck.

> [!CAUTION]
> Muster enthalten nur übergeordnete Machine Learning-Entitäten, keine untergeordneten Entitäten.

Entitäten in Mustern sind in geschweifte Klammern, `{}`, eingeschlossen. Muster können Entitäten und Entitäten mit Rollen enthalten. [Pattern.any](luis-concept-entity-types.md#patternany-entity) ist eine Entität, die ausschließlich in Mustern verwendet wird.

Die Mustersyntax unterstützt folgende Syntax:

|Funktion|Syntax|Schachtelungsebene|Beispiel|
|--|--|--|--|
|Entität| {} – geschweifte Klammern|2|Where is form {entity-name}? (Wo befindet sich Formular {Entitätsname}?)|
|optional|[] – eckige Klammern<BR><BR>Auf Schachtelungsebenen jeder beliebigen Kombination von optionaler und Gruppierungssyntax sind diese auf 3 begrenzt. |2|The question mark is optional [?] (Das Fragezeichen ist optional [?])|
|Gruppierung|() – Klammern|2|is (a \| b) (ist (a \| b))|
|oder| \| – senkrechter Strich (Pipe)<br><br>Die senkrechten Striche („oder“) sind pro Gruppe auf 2 begrenzt. |-|Where is form ({form-name-short} &#x7c; {form-name-long} &#x7c; {form-number}) (Wo ist Formular ({kurzer Formularname} &#x7c; {langer Formularname} &#x7c; {Formularnummer}))|
|Anfang und/oder Ende der Äußerung|^ – Caretzeichen|-|^begin the utterance (Anfang der Äußerung)<br>the utterance is done^ (Ende der Äußerung)<br>^strict literal match of entire utterance with {number} entity^ (^strikte Literalübereinstimmung der gesamten Äußerung mit {Anzahl} Entität(en))|

## <a name="nesting-syntax-in-patterns"></a>Schachteln von Syntax in Mustern

Die **optionale** Syntax mit eckigen Klammern kann in zwei Ebenen geschachtelt werden. Beispiel: `[[this]is] a new form`. Dieses Beispiel ermöglicht die folgenden Äußerungen:

|Beispiel für geschachtelte optionale Äußerung|Erklärung|
|--|--|
|this is a new form (dies ist ein neues Formular)|stimmt mit allen Wörtern im Muster überein|
|is a new form (ist ein neues Formular)|stimmt mit dem äußeren optionalen Wort und nicht optionalen Wörtern im Muster überein|
|a new form (ein neues Formular)|stimmt nur mit den erforderlichen Wörtern überein|

Die **Gruppierungssyntax** mit Klammern kann in zwei Ebenen geschachtelt werden. Beispiel: `(({Entity1.RoleName1} | {Entity1.RoleName2} ) | {Entity2} )`. Durch dieses Feature kann jede der drei Entitäten verglichen werden.

Wenn es sich bei „Entity1“ um einen Standort mit Rollen wie „origin“ (Ursprung, Seattle) und „destination“ (Ziel, Kairo) und bei „Entity2“ um einen bekannten Gebäudenamen aus einer Listenentität (RedWest-C) handelt, stimmen die folgenden Äußerungen mit diesem Muster überein:

|Beispiel für geschachtelte Gruppierungsäußerung|Erklärung|
|--|--|
|RedWest-C|stimmt mit der äußeren Gruppierungsentität überein|
|Seattle|stimmt mit einer der inneren Gruppierungsentitäten überein|
|Kairo|stimmt mit einer der inneren Gruppierungsentitäten überein|

## <a name="nesting-limits-for-groups-with-optional-syntax"></a>Schachtelungsbegrenzungen für Gruppen mit optionaler Syntax

Bei der Kombination von **Gruppierungssyntax** und **optionaler** Syntax gilt ein Grenzwert von 3 Schachtelungsebenen.

|Zulässig|Beispiel|
|--|--|
|Ja|( [ ( test1 &#x7c; test2 ) ] &#x7c; test3 )|
|Nein|( [ ( [ test1 ] &#x7c; test2 ) ] &#x7c; test3 )|

## <a name="nesting-limits-for-groups-with-or-ing-syntax"></a>Schachtelungsbegrenzungen für eine Gruppierung mit ODER-Syntax (|)

Bei der Kombination von **Gruppierungssyntax** mit **or-ing**-Syntax gilt ein Grenzwert von 2 senkrechten Strichen.

|Zulässig|Beispiel|
|--|--|
|Ja|( test1 &#x7c; test2 &#x7c; ( test3 &#x7c; test4 ) )|
|Nein|( test1 &#x7c; test2 &#x7c; test3 &#x7c; ( test4 &#x7c; test5 ) ) |

## <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Syntax zum Hinzufügen einer Entität zu einer Mustervorlage
Um der Mustervorlage eine Entität hinzuzufügen, schließen Sie den Namen der Entität in geschweiften Klammern ein, wie hier: `Who does {Employee} manage?`.

|Muster mit Entität|
|--|
|`Who does {Employee} manage?`|

## <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Syntax zum Hinzufügen einer Entität und einer Rolle zu einer Mustervorlage
Eine Entitätsrolle wird als `{entity:role}` notiert, wobei ein Doppelpunkt und dann der Name der Rolle auf den Namen der Entität folgen. Um der Mustervorlage eine Entität mit einer Rolle hinzuzufügen, schließen Sie den Namen der Entität und den Namen der Rolle in geschweiften Klammern ein, wie hier: `Book a ticket from {Location:Origin} to {Location:Destination}`.

|Muster mit Entitätsrollen|
|--|
|`Book a ticket from {Location:Origin} to {Location:Destination}`|

## <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Syntax zum Hinzufügen eines „pattern.any“ zu einer Mustervorlage
Die Pattern.any-Entität ermöglicht Ihnen das Hinzufügen einer Entität mit variabler Länge zum Muster. Sofern die Mustervorlage befolgt wird, kann „pattern.any“ jede beliebige Länge aufweisen.

Um der Mustervorlage eine **Pattern.any**-Entität hinzuzufügen, schließen Sie die Pattern.any-Entität in geschweifte Klammern ein, wie hier: `How much does {Booktitle} cost and what format is it available in?`.

|Muster mit Entität „Pattern.any“|
|--|
|`How much does {Booktitle} cost and what format is it available in?`|

|Buchtitel im Muster|
|--|
|Wie viel kostet **stiehl dieses Buch**, und in welchen Formaten ist es verfügbar?|
|Wie viel kostet **fragen**, und in welchen Formaten ist es verfügbar?|
|Wie viel kostet **Supergute Tage oder die sonderbare Welt des Christopher Boone**, und in welchen Formaten ist es verfügbar?|

Die Wörter des Buchtitels sind für LUIS nicht verwirrend, da LUIS basierend auf der „Pattern.any“-Entität weiß, wo der Buchtitel endet.

## <a name="explicit-lists"></a>Explizite Listen

Erstellen Sie eine [explizite Liste](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8) durch die Erstellungs-API, um die Ausnahme zu ermöglichen, wenn Folgendes zutrifft:

* Ihr Muster enthält eine [Pattern.any](luis-concept-entity-types.md#patternany-entity)-Entität.
* Diese Mustersyntax lässt die Möglichkeit einer falschen Entitätsextraktion auf Grundlage der Äußerung zu.

Nehmen wir beispielsweise an, Sie haben ein Muster, das sowohl optionale Syntax `[]` als auch Entitätssyntax `{}` in einer Kombination enthält, die zu einer fehlerhaften Extraktion der Daten führt.

Betrachten Sie das Muster ‚[Suche] E-Mail zu {Betreff} [von {Person}]‘.

In den folgenden Äußerungen werden die Entitäten **Betreff** und **Person** richtig und falsch extrahiert:

|Äußerung|Entität|Richtige Extraktion|
|--|--|:--:|
|E-Mail über Hunde von Chris|Betreff=Hunde<br>Person=Chris|✔|
|E-Mail über den Mann aus La Mancha|Betreff=der Mann<br>Person=La Mancha|X|

In der vorstehenden Tabelle sollte der Betreff `the man from La Mancha` (ein Buchtitel) sein. Da der Betreff jedoch das optionale Wort `from` enthält, wird der Titel falsch vorhergesagt.

Um diese Ausnahme im Muster zu beheben, fügen Sie mithilfe der [Verfasser-API für explizite Listen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8)`the man from la mancha` als expliziten Listentreffer für die {Betreff}-Entität hinzu.

## <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>Syntax zum Kennzeichnen von optionalem Text in einer Vorlagenäußerung
Kennzeichnen Sie optionalen Text in der Äußerung mithilfe der Syntax für eckige Klammern in regulären Ausdrücken, `[]`. In optionalem Text können eckige Klammern nur bis zu einer Tiefe von zwei Klammern verschachtelt werden.

|Muster mit optionalem Text|Bedeutung|
|--|--|
|`[find] email about {subject} [from {person}]`|`find` und `from {person}` sind optional.|
|`Can you help me[?] (Können Sie mir weiterhelfen[?])|Das Satzzeichen ist optional|

Satzzeichen (`?`, `!`, `.`) sollten ignoriert werden. Dies erreichen Sie, indem Sie die Syntax mit eckigen Klammern in Mustern verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Mustern:

* [Hinzufügen von Mustern](luis-how-to-model-intent-pattern.md)
* [Hinzufügen einer pattern.any-Entität](luis-how-to-add-entities.md#add-a-patternany-entity)
* [Musterkonzepte](luis-concept-patterns.md)

Erfahren Sie, wie die [Stimmung](luis-reference-prebuilt-sentiment.md) in der JSON-Antwort zurückgegeben wird.