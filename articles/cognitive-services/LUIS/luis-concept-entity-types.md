---
title: Entitätstypen – LUIS
description: Eine Entität extrahiert zur Vorhersagelaufzeit Daten aus einer Benutzeräußerung. Ein _optionaler_ sekundärer Zweck besteht darin, die Vorhersage der Absicht oder anderer Entitäten zu verstärken, indem die Entität als Feature verwendet wird.
ms.topic: conceptual
ms.date: 04/30/2020
ms.openlocfilehash: 9d8afd5a660b3af5556256835486e984d7d657bc
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83585639"
---
# <a name="extract-data-with-entities"></a>Extrahieren von Daten mit Entitäten

Eine Entität extrahiert zur Vorhersagelaufzeit Daten aus einer Benutzeräußerung. Ein _optionaler_ sekundärer Zweck besteht darin, die Vorhersage der Absicht oder anderer Entitäten zu verstärken, indem die Entität als Feature verwendet wird.

Es gibt mehrere Arten von Entitäten:

* [Durch maschinelles Lernen erworbene Entität](reference-entity-machine-learned-entity.md)
* Nicht durch maschinelles Lernen erworbene, als erforderliches [Feature](luis-concept-feature.md) verwendete Entität: für exakte Textübereinstimmungen, Musterübereinstimmungen oder die Erkennung durch vordefinierte Entitäten
* [Pattern.any](#patternany-entity): zum Extrahieren von Freiformtext (beispielsweise Buchtitel) aus einem [Muster](reference-entity-pattern-any.md)

Durch maschinelles Lernen erworbene Entitäten verfügen über die größte Auswahl an Optionen für die Datenextraktion. Nicht durch maschinelles Lernen erworbene Entitäten basieren auf einem Textabgleich und werden als [erforderliches Feature](#design-entities-for-decomposition) für eine durch maschinelles Lernen erworbene Entität oder für eine Absicht verwendet.

## <a name="entities-represent-data"></a>Darstellen von Daten durch Entitäten

Entitäten sind Daten, die Sie aus der Äußerung extrahieren möchten, z. B. Namen, Datumsangaben, Produktnamen oder signifikante Wortgruppen. Eine Äußerung kann viele Entitäten oder auch überhaupt keine enthalten. Eine Clientanwendung benötigt die Daten _eventuell_, um die jeweilige Aufgabe auszuführen.

Entitäten müssen für alle Trainingsäußerungen jeder Absicht eines Modells bezeichnet werden.

 Sie können eigene Entitäten definieren oder vorgefertigte Entitäten verwenden, um für gängige Konzepte, z. B. [datetimeV2](luis-reference-prebuilt-datetimev2.md), [ordinal](luis-reference-prebuilt-ordinal.md), [email](luis-reference-prebuilt-email.md) und [phone number](luis-reference-prebuilt-phonenumber.md), Zeit zu sparen.

|Äußerung|Entität|Daten|
|--|--|--|
|Kaufe 3 Tickets nach New York|Vordefinierte Anzahl<br>Destination|3<br>New York|


### <a name="entities-are-optional-but-recommended"></a>Entitäten sind optional, werden aber empfohlen.

[Absichten](luis-concept-intent.md) sind im Gegensatz zu Entitäten erforderlich. Entitäten müssen nicht für jedes Konzept in der App erstellt werden, sondern nur für jene, bei denen die Clientanwendung die Daten benötigt oder bei denen die Entität als Hinweis oder Signal für eine andere Entität oder Absicht fungiert.

Wenn Ihre Anwendung weiterentwickelt wird und sich neue Datenanforderungen ergeben, können Sie Ihrem LUIS-Modell auch später noch weitere geeignete Entitäten hinzufügen.

## <a name="entity-compared-to-intent"></a>Entität im Vergleich zu Absichten

Die Entität stellt ein Datenkonzept _innerhalb der Äußerung_ dar. Eine Absicht klassifiziert die _gesamte Äußerung_.

Sehen Sie sich die folgenden vier Äußerungen an:

|Äußerung|Vorhergesagte Absicht|Extrahierte Entitäten|Erklärung|
|--|--|--|--|
|Hilfe|help|-|Nichts zu extrahieren.|
|Send something|sendSomething|-|Nichts zu extrahieren. Das Modell verfügt über kein erforderliches Feature, um `something` in diesem Kontext zu extrahieren, und es ist kein Empfänger angegeben.|
|Send Bob a present|sendSomething|`Bob`, `present`|Das Modell extrahiert `Bob` durch Hinzufügen eines erforderlichen Features der vorgefertigten Entität `personName`. Eine durch maschinelles Lernen erworbene Entität wurde genutzt, um `present` zu extrahieren.|
|Send Bob a box of chocolates|sendSomething|`Bob`, `box of chocolates`|Die beiden wichtigen Datenelemente `Bob` und `box of chocolates` wurden von durch maschinelles Lernen erworbenen Entitäten extrahiert.|

## <a name="design-entities-for-decomposition"></a>Entwerfen von Entitäten zur Analyse

Durch maschinelles Lernen erworbene Entitäten ermöglichen es Ihnen, das App-Schema so zu gestalten, dass sich ein umfangreiches Konzept in untergeordnete Entitäten aufteilen lässt.

Der Entwurf für die Analyse bietet LUIS eine genauere Aufschlüsselung der Entitäten für Ihre Clientanwendung. Hierdurch kann sich die Clientanwendung auf Geschäftsregeln konzentrieren und die Datenauflösung LUIS überlassen.

Eine durch maschinelles Lernen erworbene Entität wird basierend auf dem Kontext ausgelöst, der durch Beispieläußerungen erlernt wird.

[**Durch maschinelles Lernen erworbene Entitäten**](tutorial-machine-learned-entity.md) sind die Extraktoren auf oberster Ebene. Bei untergeordneten Entitäten handelt es sich um untergeordnete Elemente von durch maschinelles Lernen erworbenen Entitäten.

<a name="composite-entity"></a>
<a name="list-entity"></a>
<a name="patternany-entity"></a>
<a name="prebuilt-entity"></a>
<a name="regular-expression-entity"></a>
<a name="simple-entity"></a>

## <a name="types-of-entities"></a>Typen von Entitäten

Eine untergeordnete Entität eines übergeordneten Elements sollte eine durch maschinelles Lernen erworbene Entität sein. Von der untergeordneten Entität kann eine nicht durch maschinelles Lernen erworbene Entität als [Feature](luis-concept-feature.md) verwendet werden.

Wählen Sie die Entität basierend darauf aus, wie die Daten extrahiert und nach der Extraktion dargestellt werden sollen.

|Entitätstyp|Zweck|
|--|--|
|[**Durch maschinelles Lernen erworben**](tutorial-machine-learned-entity.md)|Extrahieren geschachtelter, komplexer Daten, die anhand von beschrifteten Beispielen gelernt wurden |
|[**Liste**](reference-entity-list.md)|Liste mit Elementen und den zugehörigen Synonymen, die per **genauer Textübereinstimmung** extrahiert werden.|
|[**Pattern.any**](#patternany-entity)|Entität, bei der das Entitätsende schwierig zu bestimmen ist, da es sich um eine Freiformentität handelt. Nur in [Mustern](luis-concept-patterns.md) verfügbar.|
|[**Vordefiniert**](luis-reference-prebuilt-entities.md)|Bereits trainierte oder extrahierte spezielle Daten URLs oder E-Mail-Adressen. Einige dieser vordefinierten Entitäten werden im Open-Source-Projekt [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text) definiert. Wenn Ihre Kultur oder Entität derzeit nicht unterstützt wird, können Sie sich am Projekt beteiligen.|
|[**Regulärer Ausdruck**](reference-entity-regular-expression.md)|Verwendet einen regulären Ausdruck für **genaue Textübereinstimmungen**.|

## <a name="extracting-contextually-related-data"></a>Extrahieren von Daten mit Kontextzusammenhang

Eine Äußerung kann zwei oder mehr Vorkommen einer Entität enthalten, wobei die Bedeutung der Daten auf dem Kontext in der Äußerung basiert. Ein Beispiel ist eine Äußerung zum Buchen eines Flugs mit zwei geografischen Orten: Abflugs- und Ankunftsort.

`Book a flight from Seattle to Cairo`

Die beiden Orte müssen so extrahiert werden, dass der Clientanwendung jeweils die Art des Orts bekannt ist, um den Ticketkauf abwickeln zu können.

Erstellen Sie zum Extrahieren von Abflug- und Ankunftsort im Rahmen der durch maschinelles Lernen erworbenen Entität für die Ticketbestellung zwei untergeordnete Entitäten. Erstellen Sie für jede der untergeordneten Entitäten ein erforderliches Feature mit „geographyV2“.

<a name="using-component-constraints-to-help-define-entity"></a>
<a name="using-subentity-constraints-to-help-define-entity"></a>

### <a name="using-required-features-to-constrain-entities"></a>Verwenden erforderlicher Features zum Einschränken von Entitäten

Weitere Informationen zu erforderlichen Features finden Sie [hier](luis-concept-feature.md).

## <a name="patternany-entity"></a>Entität „Pattern.any“

Eine Entität vom Typ „Pattern.any“ ist nur in einem [Muster](luis-concept-patterns.md) verfügbar.

<a name="if-you-need-more-than-the-maximum-number-of-entities"></a>
## <a name="exceeding-app-limits-for-entities"></a>Überschreiten von App-Grenzwerten für Entitäten

Sollte das [Limit](luis-limits.md#model-limits) nicht ausreichen, wenden Sie sich an den Support. Sammeln Sie dazu ausführliche Informationen über Ihr System, navigieren Sie zur [LUIS](luis-reference-regions.md#luis-website)-Website, und wählen Sie dann **Support** aus. Wenn Ihr Azure-Abonnement Supportdienste umfasst, wenden Sie sich an den [technischen Support von Azure](https://azure.microsoft.com/support/options/).

## <a name="entity-prediction-status"></a>Status der Entitätsvorhersage

Im LUIS-Portal sehen Sie, ob die Entität in einer Beispieläußerung eine andere Entitätsvorhersage aufweist als die Entität, die Sie für eine Beispieläußerung ausgewählt haben. Der Unterschied beim Score kommt vom aktuellen trainierten Modell. Verwenden Sie diese Informationen, um Trainingsfehler mithilfe einer oder mehrerer der folgenden Maßnahmen zu beheben:
* Erstellen eines [Features](luis-concept-feature.md) für die Entität, um die Ermittlung des Konzepts der Entität zu erleichtern
* Hinzufügen weiterer [Beispieläußerungen](luis-concept-utterance.md) und Beschriften mit der Entität
* [Überprüfen von Vorschlägen für aktives Lernen](luis-concept-review-endpoint-utterances.md) für Äußerungen, die vom Vorhersageendpunkt empfangen werden und zur Ermittlung des Konzepts der Entität beitragen können

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Konzepte für gute [Äußerungen](luis-concept-utterance.md).

Weitere Informationen zum Hinzufügen von Entitäten zu LUIS-Apps finden Sie unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md).

Siehe [Tutorial: Extrahieren strukturierter Daten aus Benutzeräußerungen mithilfe der durch maschinelles Lernen erworbenen Entitäten in Language Understanding (LUIS)](tutorial-machine-learned-entity.md). Darin wird beschrieben, wie Sie strukturierte Daten mithilfe der durch maschinelles Lernen erworbenen Entität aus einer Äußerung extrahieren.

