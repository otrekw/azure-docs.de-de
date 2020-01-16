---
title: Migrieren zu V3 einer durch maschinelles Lernen erworbenen Entität
titleSuffix: Azure Cognitive Services
description: Die V3-Erstellung bietet einen neuen Entitätstyp, die durch maschinelles Lernen erworbene Entität, sowie die Möglichkeit, Beziehungen zur durch maschinelles Lernen erworbenen Entität und anderen Entitäten oder Funktionen der Anwendung hinzuzufügen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: diberry
ms.openlocfilehash: b5dbcd9033d9a41e43ea907d043e0c0486b236db
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75563822"
---
# <a name="migrate-to-v3-authoring-entity"></a>Migrieren zur V3-Erstellungsentität

Die V3-Erstellung bietet einen neuen Entitätstyp, die durch maschinelles Lernen erworbene Entität, sowie die Möglichkeit, Beziehungen zur durch maschinelles Lernen erworbenen Entität und anderen Entitäten oder Funktionen der Anwendung hinzuzufügen.

## <a name="entities-are-decomposable-in-v3"></a>Entitäten sind in V3 zerlegbar

Entitäten, die mit den V3-Erstellungs-APIs erstellt werden, entweder mithilfe der [APIs](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview) oder im [Vorschau Portal](https://preview.luis.ai/), ermöglichen Ihnen das Erstellen eines mehrstufigen Entitätsmodells mit einem übergeordneten und mehreren untergeordneten Elementen. Das übergeordnete Element ist als **durch maschinelles Lernen erworbene Entität** bekannt, und die untergeordneten Elemente werden als **Unterkomponenten** der durch maschinelles Lernen erworbenen Entität bezeichnet.

Jede Unterkomponente ist ebenfalls eine durch maschinelles Lernen erworbene Entität, aber mit den zusätzlichen Konfigurationsoptionen für Einschränkungen und Deskriptoren.

* **Einschränkungen** sind Regeln für den exakten Textabgleich, die sicherstellen, dass eine Entität extrahiert wird, wenn sie einer Regel entspricht. Die Regel wird durch eine Entität für den exakten Textvergleich definiert, derzeit: eine [vordefinierte Entität](luis-reference-prebuilt-entities.md), eine [Entität für reguläre Ausdrücke](reference-entity-regular-expression.md) oder eine [Listenentität](reference-entity-list.md).
* **Deskriptoren** sind [Features](luis-concept-feature.md), wie z. B. Ausdruckslisten oder Entitäten, die verwendet werden, um die Entität anzugeben.

Die V3-Erstellung bietet einen neuen Entitätstyp, die durch maschinelles Lernen erworbene Entität, sowie die Möglichkeit, Beziehungen zur durch maschinelles Lernen erworbenen Entität und anderen Entitäten oder Funktionen der Anwendung hinzuzufügen.

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
    * Rollen: Rollen können nur auf eine durch maschinelles Lernen erworbene (übergeordnete) Entität angewendet werden. Rollen können nicht auf Unterkomponenten angewendet werden
    * Batchtests und Muster, die hierarchische und zusammengesetzte Entitäten verwenden

Wenn Sie den Migrationsplan entwerfen, sehen Sie Zeit zum Überprüfen der endgültigen durch maschinelles Lernen erworbenen Entitäten nach der Migration aller hierarchischen und zusammengesetzten Entitäten vor. Auch wenn eine geradlinige Migration funktioniert, kann Sie der stärker vereinheitlichte JSON nach dem Ändern und Überprüfen Ihrer Batchtestergebnisse und des Vorhersage-JSONs zu Änderungen veranlassen, sodass die endgültigen Informationen, die an die clientseitige App übermittelt werden, anders organisiert sind. Dieser Vorgang hat Ähnlichkeiten mit dem Refactoring von Code und sollte dem gleichen Reviewprozess unterzogen werden, wie er in Ihrer Organisation etabliert wurde.

Wenn für Ihr V2-Modell keine Batchtests eingerichtet wurden und die Batchtests im Rahmen der Migration zum V3-Modell migriert werden, können Sie nicht überprüfen, wie sich die Migration auf die Endpunktvorhersageergebnisse auswirkt.

## <a name="migrating-from-v2-entities"></a>Migration von V2-Entitäten

Wenn Sie mit der Umstellung auf das V3-Erstellungsmodell beginnen, sollten Sie berücksichtigen, wie die Umstellung auf die durch maschinelles Lernen erworbene Entität und ihre Unterkomponenten, einschließlich der Einschränkungen und Deskriptoren, erfolgen soll.

Die folgende Tabelle gibt an, welche Entitäten von einem V2- zu einem V3-Entitätsdesign migriert werden müssen.

|V2-Erstellungsentitätstyp|V3-Erstellungsentitätstyp|Beispiel|
|--|--|--|
|Entität vom Typ „Composite“|Durch maschinelles Lernen erworbene Entität|[Weitere Informationen](#migrate-v2-composite-entity)|
|Entität vom Typ „Hierarchical“|Rolle einer durch maschinelles Lernen erworbenen Entität|[Weitere Informationen](#migrate-v2-hierarchical-entity)|

## <a name="migrate-v2-composite-entity"></a>Migrieren einer zusammengesetzten V2-Entität

Jedes untergeordnete Element der zusammengesetzten V2-Entität muss durch eine Unterkomponente der durch maschinelles Lernen erworbenen V3-Entität dargestellt werden. Wenn es sich bei dem zusammengesetzten untergeordneten Element um einen vordefinierten regulären Ausdruck oder um eine Listenentität handelt, sollte diese(r) als **Einschränkung** auf die Unterkomponente angewendet werden, die das untergeordnete Element darstellt.

Überlegungen bei der Planung der Migration einer zusammengesetzten Entität zu einer durch maschinelles Lernen erworbenen Entität:
* Untergeordnete Entitäten können nicht in Mustern verwendet werden
* Untergeordnete Entitäten werden nicht mehr gemeinsam verwendet
* Untergeordnete Entitäten müssen bezeichnet werden, wenn sie bisher keine durch maschinelles Lernen erworbenen Entitäten waren

### <a name="existing-descriptors"></a>Vorhandene Deskriptoren

Jede Ausdruckliste, die zur Verstärkung von Wörtern in der zusammengesetzten Entität verwendet wird, sollte als Deskriptor entweder auf die durch maschinelles Lernen erworbene (übergeordnete) Entität, die Unterkomponente (untergeordnete Entität) oder die Absicht angewendet werden (wenn sich die Ausdrucksliste nur auf eine Absicht bezieht). Planen Sie, den Deskriptor der Entität hinzuzufügen, die am stärksten von ihm profitieren kann. Fügen Sie den Deskriptor nicht allgemein der durch maschinelles Lernen erworbenen (übergeordneten) Entität hinzu, wenn sie die Vorhersage einer Unterkomponente (untergeordneten Entität) in höherem Maß verstärkt.

### <a name="new-descriptors"></a>Neue Deskriptoren

Fügen Sie bei der V3-Erstellung einen Planungsschritt hinzu, um Entitäten als mögliche Deskriptoren für alle Entitäten und Absichten auszuwerten.

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
|Übergeordnete Entität: Komponentenentität mit dem Namen `Order`|Übergeordnete Entität: durch maschinelles Lernen erworbene Entität mit dem Namen `Order`|
|Untergeordnete Entität: vordefinierte datetimeV2|* Migrieren der vordefinierten Entität zur neuen App<br>* Hinzufügen einer Einschränkung zur übergeordneten Entität für die vordefinierte datetimeV2|
|Untergeordnete Entität: Listenentität für Beläge|* Migrieren der Listenentität zur neuen App<br>* Fügen Sie anschließend dem übergeordneten Element eine Einschränkung für die Listenentität hinzu.|


## <a name="migrate-v2-hierarchical-entity"></a>Migrieren einer hierarchischen V2-Entität

Bei der V2-Erstellung wurde eine hierarchische Entität vor den in LUIS vorhandenen Rollen bereitgestellt. Beide dienten dem gleichen Zweck, Entitäten auf der Grundlage der Kontextverwendung zu extrahieren. Wenn Sie über hierarchische Entitäten verfügen, können Sie sich diese als einfache Entitäten mit Rollen vorstellen.

Bei der V3-Erstellung:
* Eine Rolle kann auf die durch maschinelles Lernen erworbene (übergeordnete) Entität angewendet werden
* Rollen können auf keine der Unterkomponenten angewendet werden

Diese Entität ist lediglich ein Beispiel. Die Migration Ihrer eigenen Entität erfordert möglicherweise andere Überlegungen.

Betrachten wir eine hierarchische V2-Entität zum Ändern einer `order` für eine Pizza:
* wobei jede untergeordnete Entität entweder einen ursprünglichen oder den endgültigen Belag bestimmt

Eine Beispieläußerung für diese Entität wäre etwa:

`Change the topping from mushrooms to olives`

Die Migration ist in der folgenden Tabelle veranschaulicht:

|V2-Modelle|V3-Modelle|
|--|--|
|Übergeordnete Entität: Komponentenentität mit dem Namen `Order`|Übergeordnete Entität: durch maschinelles Lernen erworbene Entität mit dem Namen `Order`|
|Untergeordnete Entität: hierarchische Entität mit ursprünglichem und endgültigem Pizzabelag|* Fügen Sie `Order` für jeden Belag eine Rolle hinzu.|

## <a name="next-steps"></a>Nächste Schritte

* [Entwicklerressourcen](developer-reference-resource.md)
