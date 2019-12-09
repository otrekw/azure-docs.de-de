---
title: Entitätstypen – LUIS
titleSuffix: Azure Cognitive Services
description: 'Entitäten extrahieren Daten aus der Äußerung. Entitätstypen ermöglichen eine vorhersagbare Extraktion von Daten. Es gibt zwei Arten von Entitäten: maschinell erlernte und nicht maschinell erlernte. Es ist wichtig zu wissen, mit welcher Art von Entität Sie in Äußerungen arbeiten.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: diberry
ms.openlocfilehash: 808e110ccb45b0b4f7bf34a43597c1f7a7bc0fed
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422583"
---
# <a name="entities-and-their-purpose-in-luis"></a>Entitäten und ihr Zweck in LUIS

Der primäre Zweck von Entitäten besteht darin, der Clientanwendung eine vorhersagbare Extraktion von Daten zu ermöglichen. Ein _optionaler_ sekundärer Zweck besteht darin, die Vorhersage der Absicht oder anderer Entitäten mit Deskriptoren zu verbessern.

Es gibt zwei Arten von Entitäten:

* durch maschinelles Lernen erworben – im Kontext
* nicht durch maschinelles Lernen erworben – für genaue Textübereinstimmungen, Musterübereinstimmungen oder Erkennung über vordefinierte Entitäten

Durch maschinelles Lernen erworbene Entitäten verfügen über die größte Auswahl an Optionen für die Datenextraktion. Für nicht durch maschinelles Lernen erworbene Entitäten wird die Textübereinstimmung genutzt, und sie können separat oder als [Einschränkung](#design-entities-for-decomposition) einer durch maschinelles Lernen erworbenen Entität eingesetzt werden.

## <a name="entities-represent-data"></a>Darstellen von Daten durch Entitäten

Entitäten sind Daten, die Sie aus der Äußerung extrahieren möchten, z. B. Namen, Datumsangaben, Produktnamen oder signifikante Wortgruppen. Eine Äußerung kann viele Entitäten oder auch überhaupt keine enthalten. Eine Clientanwendung benötigt die Daten _eventuell_, um die jeweilige Aufgabe auszuführen.

Entitäten müssen für alle Trainingsäußerungen jeder Absicht eines Modells bezeichnet werden.

 Sie können eigene Entitäten definieren oder vorgefertigte Entitäten verwenden, um für gängige Konzepte, z. B. [datetimeV2](luis-reference-prebuilt-datetimev2.md), [ordinal](luis-reference-prebuilt-ordinal.md), [email](luis-reference-prebuilt-email.md) und [phone number](luis-reference-prebuilt-phonenumber.md), Zeit zu sparen.

|Äußerung|Entität|Data|
|--|--|--|
|Kaufe 3 Tickets nach New York|Vordefinierte Anzahl<br>Location.Destination|3<br>New York|
|Kaufe ein Ticket von New York nach London am 5. März|Location.Origin<br>Location.Destination<br>Vordefinierte datetimeV2|New York<br>London<br>5\. März 2018|

### <a name="entities-are-optional"></a>Entitäten sind optional

Im Gegensatz zu Absichten sind Entitäten optional. Sie müssen keine Entitäten für sämtliche Konzepte Ihrer App erstellen, sondern lediglich für die, die für Aktionen der Clientanwendung erforderlich sind.

Wenn Ihre Äußerungen nicht die Daten enthalten, die von der Clientanwendung benötigt werden, müssen Sie keine Entitäten hinzufügen. Wenn Ihre Anwendung weiterentwickelt wird und sich neue Datenanforderungen ergeben, können Sie Ihrem LUIS-Modell auch später noch weitere geeignete Entitäten hinzufügen.

## <a name="entity-compared-to-intent"></a>Entität im Vergleich zu Absichten

Die Entität stellt ein Datenkonzept innerhalb der Äußerung dar, die Sie extrahieren möchten.

Eine Äußerung kann optional Entitäten enthalten. Im Vergleich dazu ist die Vorhersage der Absicht für eine Äußerung _erforderlich_ und stellt die gesamte Äußerung dar. Für LUIS müssen die Beispieläußerungen in einer Absicht enthalten sind.

Gehen Sie von den folgenden vier Äußerungen aus:

|Äußerung|Vorhergesagte Absicht|Extrahierte Entitäten|Erklärung|
|--|--|--|--|
|Hilfe|help|-|Nichts zu extrahieren.|
|Send something|sendSomething|-|Nichts zu extrahieren. Das Modell wurde nicht trainiert, um `something` in diesem Kontext zu extrahieren, und es ist auch kein Empfänger vorhanden.|
|Send Bob a present|sendSomething|`Bob`, `present`|Das Modell wurde mit der vordefinierten Entität [personName](luis-reference-prebuilt-person.md) trainiert, sodass der Name `Bob` extrahiert wurde. Eine durch maschinelles Lernen erworbene Entität wurde genutzt, um `present` zu extrahieren.|
|Send Bob a box of chocolates|sendSomething|`Bob`, `box of chocolates`|Die beiden wichtigen Datenelemente `Bob` und `box of chocolates` wurden von Entitäten extrahiert.|

## <a name="design-entities-for-decomposition"></a>Entwerfen von Entitäten zur Analyse

Beim Entwerfen von Entitäten ist es eine gute Vorgehensweise, Ihre Entität der obersten Ebene zu einer durch maschinelles Lernen erworbenen Entität zu machen. Dies ermöglicht spätere Änderungen an Ihrem Entitätsentwurf und die Verwendung von **Unterkomponenten** (untergeordneten Entitäten) – optional mit **Einschränkungen** und **Deskriptoren**, um die Entität der obersten Ebene in die Teile zu zerlegen, die von der Clientanwendung benötigt werden.

Der Entwurf für die Analyse bietet LUIS eine genauere Aufschlüsselung der Entitäten für Ihre Clientanwendung. Hierdurch kann sich die Clientanwendung auf Geschäftsregeln konzentrieren und die Datenauflösung LUIS überlassen.

### <a name="machine-learned-entities-are-primary-data-collections"></a>Durch maschinelles Lernen erworbene Entitäten sind primäre Datensammlungen

[**Durch maschinelles Lernen erworbene Entitäten**](tutorial-machine-learned-entity.md) sind die Dateneinheit auf oberster Ebene. Unterkomponenten sind untergeordnete Entitäten von durch maschinelles Lernen erworbenen Entitäten.

Eine durch maschinelles Lernen erworbene Entität wird basierend auf dem Kontext ausgelöst, der durch Trainingsäußerungen erlernt wird. **Einschränkungen** sind optionale Regeln, die auf eine durch maschinelles Lernen erworbene Entität angewendet werden. Die Auslösung wird hierbei anhand der Definition der genauen Textübereinstimmung einer nicht durch maschinelles Lernen erworbenen Entität, z. B. [List](reference-entity-list.md) oder [Regex](reference-entity-regular-expression.md), weiter eingeschränkt. Die durch maschinelles Lernen erworbene Entität `size` kann beispielsweise über eine Einschränkung der Listenentität `sizeList` verfügen. Hiermit wird die Entität `size` dahingehend eingeschränkt, dass die Auslösung nur erfolgt, wenn in der Entität `sizeList` enthaltene Werte vorkommen.

[**Deskriptoren**](luis-concept-feature.md) sind Features, mit denen die Relevanz von Wörtern oder Ausdrücken für die Vorhersage erhöht wird. Sie werden als *Deskriptoren* bezeichnet, weil sie zum *Beschreiben* einer Absicht oder Entität verwendet werden. Mit Deskriptoren werden eindeutige Merkmale oder Datenattribute, z. B. wichtige Wörter oder Ausdrücke, beschrieben. Mit LUIS werden sie erkannt und erlernt.

Wenn Sie in Ihrer LUIS-App ein Feature für eine Ausdrucksliste erstellen, ist sie standardmäßig global aktiviert und gilt einheitlich für alle Absichten und Entitäten. Wenn Sie die Ausdrucksliste aber als Deskriptor (Feature) einer durch maschinelles Lernen erworbenen Entität (oder eines *Modells*) anwenden, gilt der jeweilige Bereich nur für dieses Modell und wird für alle anderen Modelle nicht mehr genutzt. Die Verwendung einer Ausdrucksliste als Deskriptor für ein Modell ist hilfreich für die Zerlegung, weil dies die Genauigkeit für das entsprechende Modell erhöht.

<a name="composite-entity"></a>
<a name="list-entity"></a>
<a name="patternany-entity"></a>
<a name="prebuilt-entity"></a>
<a name="regular-expression-entity"></a>
<a name="simple-entity"></a>

## <a name="types-of-entities"></a>Typen von Entitäten

Wählen Sie die Entität basierend darauf aus, wie die Daten extrahiert und nach der Extraktion dargestellt werden sollen.

|Entitätstyp|Zweck|
|--|--|
|[**Durch maschinelles Lernen erworben**](tutorial-machine-learned-entity.md)|Durch maschinelles Lernen erworbene Entitäten lernen über den Kontext der Äußerung. Übergeordnete Gruppierung von Entitäten, unabhängig vom Entitätstyp. Aus diesem Grund ist die Variation der Platzierung in Beispieläußerungen wichtig. |
|[**Liste**](reference-entity-list.md)|Liste mit Elementen und den zugehörigen Synonymen, die per **genauer Textübereinstimmung** extrahiert werden.|
|[**Pattern.any**](reference-entity-pattern-any.md)|Entität, bei der das Entitätsende schwierig zu ermitteln ist. |
|[**Vordefiniert**](luis-reference-prebuilt-entities.md)|Bereits trainierte oder extrahierte spezielle Daten URLs oder E-Mail-Adressen. Einige dieser vordefinierten Entitäten werden im Open-Source-Projekt [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text) definiert. Wenn Ihre Kultur oder Entität derzeit nicht unterstützt wird, können Sie sich am Projekt beteiligen.|
|[**Regulärer Ausdruck**](reference-entity-regular-expression.md)|Verwendet einen regulären Ausdruck für **genaue Textübereinstimmungen**.|

## <a name="extracting-contextually-related-data"></a>Extrahieren von Daten mit Kontextzusammenhang

Eine Äußerung kann zwei oder mehr Vorkommen einer Entität enthalten, wobei die Bedeutung der Daten auf dem Kontext in der Äußerung basiert. Ein Beispiel ist eine Äußerung zum Buchen eines Flugs mit zwei Orten: Abflugs- und Ankunftsort.

`Book a flight from Seattle to Cairo`

Die zwei Beispiele einer `location`-Entität müssen extrahiert werden. Die Clientanwendung muss den jeweiligen Ort kennen, um den Ticketkauf abschließen zu können.

Es gibt zwei Verfahren zum Extrahieren von Daten mit Kontextzusammenhang:

 * Die Entität `location` ist eine durch maschinelles Lernen erworbene Entität, für die zwei Unterkomponenten-Entitäten verwendet werden, um `origin` und `destination` (bevorzugt) zu erfassen.
 * Für die Entität `location` werden die beiden **Rollen** `origin` und `destination` genutzt.

Eine Äußerung kann mehrere Entitäten enthalten und ohne Verwendung einer Zerlegung oder von Rollen extrahiert werden, wenn der Nutzungskontext keine besondere Bedeutung hat. Wenn die Äußerung beispielsweise eine Liste mit Orten enthält (`I want to travel to Seattle, Cairo, and London.`), haben die einzelnen Elemente der Liste keine zusätzliche Bedeutung.

### <a name="using-subcomponent-entities-of-a-machine-learned-entity-to-define-context"></a>Verwenden von Unterkomponenten-Entitäten einer durch maschinelles Lernen erworbenen Entität zum Definieren des Kontexts

Sie können eine [**durch maschinelles Lernen erworbene Entität**](tutorial-machine-learned-entity.md) zum Extrahieren der Daten verwenden, mit der die Aktion einer Flugbuchung beschrieben wird, und die Entität der obersten Ebene dann in die einzelnen Teile zerlegen, die von der Clientanwendung benötigt werden.

Im Beispiel `Book a flight from Seattle to Cairo` kann die Entität der obersten Ebene `travelAction` lauten und dafür bestimmt sein, `flight from Seattle to Cairo` zu extrahieren. Anschließend werden die beiden Unterkomponenten-Entitäten `origin` und `destination` erstellt, für die jeweils eine Einschränkung durch die vordefinierte Entität `geographyV2` angewendet wird. In den Trainingsäußerungen sind `origin` und `destination` entsprechend bezeichnet.

### <a name="using-entity-role-to-define-context"></a>Verwenden der Entitätsrolle zum Definieren des Kontexts

Eine Rolle ist ein benannter Alias für eine Entität, der auf dem Kontext innerhalb der Äußerung basiert. Eine Rolle kann mit allen vordefinierten oder benutzerdefinierten Entitätstypen und sowohl in Beispieläußerungen als auch in Mustern verwendet werden. In diesem Beispiel benötigt die Entität `location` die beiden Rollen `origin` und `destination`, die beide in den Beispieläußerungen gekennzeichnet werden müssen.

Wenn LUIS zwar `location` findet, aber die Rolle nicht ermitteln kann, wird die Entität für den Ort trotzdem zurückgegeben. Die Clientanwendung muss eine Anschlussfrage stellen, um zu ermitteln, welche Art von Ort der Benutzer gemeint hat.


## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Wenn mehr als die maximale Anzahl von Entitäten erforderlich ist

Wenn Sie mehr als das Limit benötigen, wenden Sie sich an den Support. Sammeln Sie dazu ausführliche Informationen über Ihr System, navigieren Sie zur [LUIS](luis-reference-regions.md#luis-website)-Website, und wählen Sie dann **Support** aus. Wenn Ihr Azure-Abonnement Supportdienste umfasst, wenden Sie sich an den [technischen Support von Azure](https://azure.microsoft.com/support/options/).

## <a name="entity-prediction-status"></a>Status der Entitätsvorhersage

Im LUIS-Portal wird angezeigt, wenn die Entität in einer Beispieläußerung eine andere Entitätsvorhersage aufweist als die von Ihnen ausgewählte Entität. Der Unterschied beim Score kommt vom aktuellen trainierten Modell.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Konzepte für gute [Äußerungen](luis-concept-utterance.md).

Weitere Informationen zum Hinzufügen von Entitäten zu LUIS-Apps finden Sie unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md).

Eine Schritt-für-Schritt-Anleitung finden Sie im [Tutorial: Extrahieren strukturierter Daten aus Benutzeräußerungen mithilfe der durch maschinelles Lernen erworbenen Entitäten in Language Understanding (LUIS)](tutorial-machine-learned-entity.md). Darin wird beschrieben, wie Sie strukturierte Daten mithilfe der durch maschinelles Lernen erworbenen Entität aus einer Äußerung extrahieren.
