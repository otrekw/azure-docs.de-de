---
title: Migrieren zur Machine Learning-Entität in V3
description: Die V3-Erstellung bietet einen neuen Entitätstyp, die Machine Learning-Entität, sowie die Möglichkeit, Beziehungen zur Machine Learning-Entität und anderen Entitäten oder Funktionen der Anwendung hinzuzufügen.
ms.topic: how-to
ms.date: 05/08/2020
ms.openlocfilehash: aaa5472f25a5eca5ceadf979c57a83874ce4cb6e
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684594"
---
# <a name="migrate-to-v3-authoring-entity"></a>Migrieren zur V3-Erstellungsentität

Die V3-Erstellung bietet einen neuen Entitätstyp, die Machine Learning-Entität, sowie die Möglichkeit, Beziehungen zur Machine Learning-Entität und anderen Entitäten oder Funktionen der Anwendung hinzuzufügen.

## <a name="entities-are-decomposable-in-v3"></a>Entitäten sind in V3 zerlegbar

Entitäten, die mit den V3-Erstellungs-APIs erstellt werden (entweder mithilfe der [APIs](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview) oder über das Portal), ermöglichen das Erstellen eines mehrstufigen Entitätsmodells mit einem übergeordneten Element und mehreren untergeordneten Elementen. Das übergeordnete Element wird als **Machine Learning-Entität** bezeichnet, und die untergeordneten Elemente werden als **untergeordnete Entitäten** der Machine Learning-Entität bezeichnet.

Jede untergeordnete Entität ist ebenfalls eine Machine Learning-Entität, verfügt aber über die zusätzlichen Konfigurationsoptionen von Features.

* **Erforderliche Features** sind Regeln, die sicherstellen, dass eine Entität extrahiert wird, wenn sie einem Feature entspricht. Die Regel wird durch das erforderliche Feature für das Modell definiert:
    * [Vordefinierte Entität](luis-reference-prebuilt-entities.md)
    * [Entität vom Typ „RegEx“](reference-entity-regular-expression.md)
    * [Entität vom Typ „List“](reference-entity-list.md)

## <a name="how-do-these-new-relationships-compare-to-v2-authoring"></a>Wie sehen diese neuen Beziehungen im Vergleich mit der Erstellung in V2 aus

In der V2-Erstellung standen für die gleiche Aufgabe hierarchische und zusammengesetzte Entitäten in Verbindung mit Rollen und Features zur Verfügung. Da die Entitäten, Features und Rollen nicht explizit aufeinander bezogen waren, war es schwierig, zu verstehen, in welcher Weise LUIS die Beziehungen bei der Vorhersage voraussetzte.

In V3 ist die Beziehung explizit und von den Autoren der App entworfen. Dies gibt Ihnen als Autor der App folgende Möglichkeiten:

* Visuell zu sehen, wie LUIS diese Beziehungen in den Beispieläußerungen vorhersagt
* Auf diese Beziehungen entweder mit dem [interaktiven Testbereich](luis-interactive-test.md) oder am Endpunkt zu testen
* Diese Beziehungen in der Clientanwendung mithilfe eines wohlstrukturierten, benannten, geschachtelten [JSON-Objekts](reference-entity-machine-learned-entity.md) zu verwenden

## <a name="planning"></a>Planung

Berücksichtigen Sie bei der Migration die folgenden Punkte in Ihrem Migrationsplan:

* Sichern Sie Ihre LUIS-App, und führen Sie die Migration in einer separaten App durch. Wenn eine V2- und eine V3-App gleichzeitig verfügbar sind, können Sie die erforderlichen Änderungen und die Auswirkungen auf die Vorhersageergebnisse überprüfen.
* Erfassen Sie aktuelle Vorhersageerfolgsmetriken
* Erfassen Sie aktuelle Dashboardinformationen als Momentaufnahme des App-Status
* Überprüfen Sie vorhandene Absichten, Entitäten, Ausdruckslisten, Muster und Batchtests
* Die folgenden Elemente können **ohne Änderung** migriert werden:
    * Absichten
    * Entitäten
        * Entität vom Typ „RegEx“
        * Entität vom Typ „List“
    * Features
        * Ausdrucksliste
* Die folgenden Elemente können **mit Änderungen** migriert werden:
    * Entitäten
        * Entität vom Typ „Hierarchical“
        * Entität vom Typ „Composite“
    * Rollen: Rollen können nur auf eine (übergeordnete) Machine Learning-Entität angewendet werden. Rollen können nicht auf untergeordnete Entitäten angewendet werden.
    * Batchtests und Muster, die hierarchische und zusammengesetzte Entitäten verwenden

Wenn Sie den Migrationsplan entwerfen, planen Sie auch etwas Zeit für die Überprüfung der endgültigen Machine Learning-Entitäten nach der Migration aller hierarchischen und zusammengesetzten Entitäten. Auch wenn eine geradlinige Migration funktioniert, kann Sie der stärker vereinheitlichte JSON nach dem Ändern und Überprüfen Ihrer Batchtestergebnisse und des Vorhersage-JSONs zu Änderungen veranlassen, sodass die endgültigen Informationen, die an die clientseitige App übermittelt werden, anders organisiert sind. Dieser Vorgang hat Ähnlichkeiten mit dem Refactoring von Code und sollte dem gleichen Reviewprozess unterzogen werden, wie er in Ihrer Organisation etabliert wurde.

Wenn für Ihr V2-Modell keine Batchtests eingerichtet wurden und die Batchtests im Rahmen der Migration zum V3-Modell migriert werden, können Sie nicht überprüfen, wie sich die Migration auf die Endpunktvorhersageergebnisse auswirkt.

## <a name="migrating-from-v2-entities"></a>Migration von V2-Entitäten

Wenn Sie mit der Umstellung auf das V3-Erstellungsmodell beginnen, müssen Sie sich überlegen, wie die Umstellung auf die Machine Learning-Entität und ihre untergeordneten Entitäten und Features erfolgen soll.

Die folgende Tabelle gibt an, welche Entitäten von einem V2- zu einem V3-Entitätsdesign migriert werden müssen.

|V2-Erstellungsentitätstyp|V3-Erstellungsentitätstyp|Beispiel|
|--|--|--|
|Entität vom Typ „Composite“|Durch maschinelles Lernen erworbene Entität|[Weitere Informationen](#migrate-v2-composite-entity)|
|Entität vom Typ „Hierarchical“|Rolle der Machine Learning-Entität|[Weitere Informationen](#migrate-v2-hierarchical-entity)|

## <a name="migrate-v2-composite-entity"></a>Migrieren einer zusammengesetzten V2-Entität

Jedes untergeordnete Element der zusammengesetzten V2-Entität muss durch eine untergeordnete Entität der V3-Machine Learning-Entität dargestellt werden. Wenn es sich bei dem zusammengesetzten untergeordneten Element um eine vordefinierte Entität, um einen regulären Ausdruck oder um eine Listenentität handelt, muss dies als erforderliches Feature auf die untergeordnete Entität angewendet werden.

Überlegungen bei der Planung der Migration einer zusammengesetzten Entität zu einer Machine Learning-Entität:
* Untergeordnete Entitäten können nicht in Mustern verwendet werden
* Untergeordnete Entitäten werden nicht mehr gemeinsam verwendet
* Untergeordnete Entitäten müssen bezeichnet werden, wenn sie bisher keine durch maschinelles Lernen erworbenen Entitäten waren

### <a name="existing-features"></a>Vorhandene Features

Jede Ausdruckliste, die zur Verstärkung von Wörtern in der zusammengesetzten Entität verwendet wird, muss als Feature entweder auf die (übergeordnete) Machine Learning-Entität, auf die untergeordnete Entität oder auf die Absicht (sofern sich die Ausdrucksliste nur auf eine einzelne Absicht bezieht) angewendet werden. Planen Sie, das Feature der Entität hinzuzufügen, die am meisten verstärkt werden soll. Fügen Sie das Feature nicht allgemein der (übergeordneten) Machine Learning-Entität hinzu, wenn damit die Vorhersage einer untergeordneten Entität am meisten verstärkt wird.

### <a name="new-features"></a>Neue Funktionen

Fügen Sie bei der V3-Erstellung einen Planungsschritt hinzu, um Entitäten als mögliche Features für alle Entitäten und Absichten auszuwerten.

### <a name="example-entity"></a>Beispielentität

Diese Entität ist lediglich ein Beispiel. Die Migration Ihrer eigenen Entität erfordert möglicherweise andere Überlegungen.

Betrachten wir eine zusammengesetzte V2-Entität zum Ändern einer `order` für eine Pizza, die Folgendes verwendet:
* vordefinierte datetimeV2 für die Lieferzeit
* Ausdrucksliste zur Verstärkung bestimmter Wörter, wie Pizza, Teig, Kruste und Belag
* Listenentität zum Erkennen von Belägen wie Pilzen, Oliven, Pepperoni.

Eine Beispieläußerung für diese Entität wäre etwa:

`Change the toppings on my pie to mushrooms and delivery it 30 minutes later`

Die Migration ist in der folgenden Tabelle veranschaulicht:

|V2-Modelle|V3-Modelle|
|--|--|
|Übergeordnete Entität: Komponentenentität mit dem Namen `Order`|Übergeordnete Entität: Machine Learning-Entität mit dem Namen `Order`|
|Untergeordnete Entität: vordefinierte datetimeV2|* Migrieren der vordefinierten Entität zur neuen App<br>* Fügen Sie das erforderliche Feature dem übergeordneten Element für „datetimeV2“ (vordefiniert) hinzu.|
|Untergeordnete Entität: Listenentität für Beläge|* Migrieren der Listenentität zur neuen App<br>* Fügen Sie anschließend dem übergeordneten Element ein erforderliches Feature für die Listenentität hinzu.|


## <a name="migrate-v2-hierarchical-entity"></a>Migrieren einer hierarchischen V2-Entität

Bei der V2-Erstellung wurde eine hierarchische Entität vor den in LUIS vorhandenen Rollen bereitgestellt. Beide dienten dem gleichen Zweck, Entitäten auf der Grundlage der Kontextverwendung zu extrahieren. Wenn Sie über hierarchische Entitäten verfügen, können Sie sich diese als einfache Entitäten mit Rollen vorstellen.

Bei der V3-Erstellung:
* Eine Rolle kann auf die (übergeordnete) Machine Learning-Entität angewendet werden.
* Eine Rolle kann auf keine untergeordneten Entitäten angewendet werden.

Diese Entität ist lediglich ein Beispiel. Die Migration Ihrer eigenen Entität erfordert möglicherweise andere Überlegungen.

Betrachten wir eine hierarchische V2-Entität zum Ändern einer `order` für eine Pizza:
* wobei jede untergeordnete Entität entweder einen ursprünglichen oder den endgültigen Belag bestimmt

Eine Beispieläußerung für diese Entität wäre etwa:

`Change the topping from mushrooms to olives`

Die Migration ist in der folgenden Tabelle veranschaulicht:

|V2-Modelle|V3-Modelle|
|--|--|
|Übergeordnete Entität: Komponentenentität mit dem Namen `Order`|Übergeordnete Entität: Machine Learning-Entität mit dem Namen `Order`|
|Untergeordnete Entität: hierarchische Entität mit ursprünglichem und endgültigem Pizzabelag|* Fügen Sie `Order` für jeden Belag eine Rolle hinzu.|

## <a name="api-change-constraint-replaced-with-required-feature"></a>API-Änderung: Einschränkung durch erforderliches Feature ersetzt

Diese Änderung wurde im Mai 2020 auf der //Build-Konferenz vorgenommen und gilt nur für die V3-Erstellungs-APIs, bei denen eine App ein eingeschränktes Feature verwendet. Wenn Sie von der V2-Erstellung zur V3-Erstellung migrieren oder keine eingeschränkten V3-Features verwendet haben, können Sie diesen Abschnitt überspringen.

**Funktion:** Möglichkeit, eine vorhandene Entität als Feature für ein anderes Modell erforderlich zu machen und dieses Modell nur zu extrahieren, wenn die Entität erkannt wird. Die Funktion ist unverändert, aber die API und die Terminologie haben sich geändert.

|Alte Terminologie|Neue Terminologie|
|--|--|
|`constrained feature`<br>`constraint`<br>`instanceOf`|`required feature`<br>`isRequired`|

#### <a name="automatic-migration"></a>Automatische Migration

Ab dem **19. Juni 2020** dürfen keine programmgesteuerten Einschränkungen mehr mithilfe der vorherigen Erstellungs-API erstellt werden, von der diese Funktion verfügbar gemacht wurde.

Bereits vorhandene Einschränkungsfeatures werden automatisch zum Flag für erforderliche Features migriert. Es sind keine programmgesteuerten Änderungen für Ihre Vorhersage-API erforderlich, und auch die Qualität der Vorhersagegenauigkeit bleibt unverändert.

#### <a name="luis-portal-changes"></a>Änderungen im LUIS-Portal

Im LUIS-Vorschauportal hat wurde diese Funktion als **Einschränkung** bezeichnet. Im aktuellen LUIS-Portal wird diese Funktion als **erforderliches Feature** bezeichnet.

#### <a name="previous-authoring-api"></a>Alte Erstellungs-API

Diese Funktion wurde in der Vorschauversion der **[API zum Erstellen einer untergeordneten Entität](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5d86cf3c6a25a45529767d77)** als Teil der Definition einer Entität unter Verwendung der Eigenschaft `instanceOf` des untergeordneten Elements einer Entität angewendet:

```json
{
    "name" : "dayOfWeek",
    "instanceOf": "datetimeV2",
    "children": [
        {
           "name": "dayNumber",
           "instanceOf": "number",
           "children": []
        }
    ]
}
```

#### <a name="new-authoring-api"></a>Neue Erstellungs-API

Diese Funktion wird nun mit der **[API zum Hinzufügen eine Entitätsfeaturebeziehung](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5d9dc1781e38aaec1c375f26)** unter Verwendung der Eigenschaften `featureName` und `isRequired` angewendet. Der Wert der Eigenschaft `featureName` ist der Name des Modells.

```json
{
    "featureName": "YOUR-MODEL-NAME-HERE",
    "isRequired" : true
}
```


## <a name="next-steps"></a>Nächste Schritte

* [Entwicklerressourcen](developer-reference-resource.md)
