---
title: 'Hinzufügen von Entitäten: LUIS'
description: Erstellen Sie Entitäten, um die Schlüsseldaten aus Benutzeräußerungen in Language Understanding-Apps (LUIS) zu extrahieren. Extrahierte Entitätsdaten werden von der Clientanwendung verwendet, um Kundenanforderungen zu erfüllen.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 05/17/2020
ms.openlocfilehash: c5c6836c2d68036bf2b9c5abe191943537349b8d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "91540964"
---
# <a name="add-entities-to-extract-data"></a>Hinzufügen von Entitäten zum Extrahieren von Daten

Erstellen Sie Entitäten, um die Schlüsseldaten aus Benutzeräußerungen in Language Understanding-Apps (LUIS) zu extrahieren. Extrahierte Entitätsdaten werden von Ihrer Clientanwendung verwendet, um Kundenanforderungen zu erfüllen.

Die Entität stellt ein Wort oder einen Ausdruck innerhalb der Äußerung dar, die extrahiert werden soll. Entitäten beschreiben Informationen mit Bezug zur Absicht. Manchmal sind sie auch für die Ausführung der Aufgabe Ihrer App wichtig. Sie können Entitäten erstellen, wenn Sie einer Absicht eine Beispieläußerung hinzufügen, oder auch getrennt vom Hinzufügen einer Beispieläußerung zu einer Absicht (vor oder nach dem Hinzufügen).

## <a name="plan-entities-then-create-and-label"></a>Planen von Entitäten mit anschließender Erstellung und Bezeichnung

Machine Learning-Entitäten können über die Beispieläußerungen oder über die Seite **Entitäten** erstellt werden.

Es empfiehlt sich im Allgemeinen, Zeit für die Planung der Entitäten aufzuwenden, bevor eine Machine Learning-Entität im Portal erstellt wird. Erstellen Sie dann die Machine Learning-Entität auf der Grundlage der Beispieläußerung mit so vielen Details in den untergeordneten Entitäten und Features, wie Ihnen zu diesem Zeitpunkt bekannt sind. Das [Tutorial „Zerlegbare Entität“](tutorial-machine-learned-entity.md) veranschaulicht die Verwendung dieser Methode.

Im Rahmen der Planung der Entitäten ist zu beachten, dass Sie Entitäten für den Textvergleich benötigen (z. B. vorgefertigte Entitäten, Entitäten vom Typ „Regular Expression“ oder vom Typ „List“). Sie können diese auf der Seite **Entitäten** erstellen, bevor sie in Beispieläußerungen bezeichnet werden.

Sie können einzelne Entitäten bezeichnen und dann zu einer übergeordneten Machine Learning-Entität aufbauen. Alternativ können Sie mit einer übergeordneten beginnen und sie in untergeordnete Entitäten zerlegen.

> [!TIP]
>Bezeichnen Sie alle Wörter, die auf eine Entität hinweisen können, auch wenn die Wörter bei der Extraktion in der Clientanwendung nicht verwendet werden.

## <a name="when-to-create-an-entity"></a>Zeitpunkt für die Entitätserstellung

Sie sollten Ihre Machine Learning-Entitäten und untergeordneten Entitäten erstellen, nachdem Sie Ihre Entitäten geplant haben. Hierzu müssen ggf. vorgefertigte Entitäten oder Entitäten mit Textvergleich hinzugefügt werden, um Features für Ihre Machine Learning-Entitäten bereitzustellen. Dies sollte alles vor dem Bezeichnen geschehen.

Wenn Sie mit dem Bezeichnen von Beispieläußerungen begonnen haben, können Sie durch maschinelles Lernen erworbene Entitäten erstellen oder Listenentitäten erweitern.

Die folgende Tabelle gibt Aufschluss darüber, wo die einzelnen Entitätstypen erstellt oder der App hinzugefügt werden:

|Entitätstyp|Erstellen einer Entität im LUIS-Portal|
|--|--|
|Machine Learning-Entität|Entitäten oder Absichtsdetails|
|Entität vom Typ „List“|Entitäten oder Absichtsdetails|
|Entität vom Typ „RegEx“|Entitäten|
|Entität „Pattern.any“|Entitäten|
|Vordefinierte Entität|Entitäten|
|Vordefinierte Domänenentität|Entitäten|

Sie können alle Entitäten auf der Seite **Entitäten** erstellen, oder Sie können einige Entitäten im Rahmen der Bezeichnung der Entität in der Beispieläußerung auf der Seite **Absichtsdetails** erstellen. Sie können eine Entität in einer Beispieläußerung von der Seite **Absichtsdetails** nur _bezeichnen_.



## <a name="how-to-create-a-new-custom-entity"></a>Erstellen einer neuen benutzerdefinierten Entität

Dieser Prozess funktioniert für durch maschinelles Lernen erworbene Entitäten, für Listenentitäten und für RegEx-Entitäten.

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.
1. Wählen Sie die Seite **Entitäten** aus.
1. Wählen Sie **+ Erstellen** und dann den Entitätstyp aus.
1. Fahren Sie mit der Konfiguration der Entität fort, und wählen Sie dann **Erstellen** aus, wenn Sie fertig sind.

## <a name="create-a-machine-learned-entity"></a>Erstellen einer durch maschinelles Lernen erworbenen Entität

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.
1. Wählen Sie im Abschnitt **Erstellen** im linken Bereich die Option **Entitäten** und dann **+ Erstellen** aus.
1. Geben Sie im Dialogfeld **Entitätstyp erstellen** den Namen der Entität ein, und wählen Sie anschließend **Machine Learned** (Durch maschinelles Lernen erworben) aus. Wählen Sie zum Hinzufügen untergeordneter Entitäten die Option **Add structure** (Struktur hinzufügen) aus. Klicken Sie auf **Erstellen**.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Erstellen einer durch maschinelles Lernen erworbenen Entität](media/add-entities/machine-learned-entity-with-structure.png)

1. Fügen Sie unter **Add subentities** (Untergeordnete Entitäten hinzufügen) eine untergeordnete Entität hinzu, indem Sie in der Zeile der übergeordneten Entität das Plussymbol ( **+** ) auswählen.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Hinzufügen untergeordneter Entitäten](media/add-entities/machine-learned-entity-with-subentities.png)

1. Wählen Sie **Erstellen** aus, um den Erstellungsprozess abzuschließen.

## <a name="add-a-feature-to-a-machine-learned-entity"></a>Hinzufügen eines Features zu einer durch maschinelles Lernen erworbenen Entität

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.
1. Wählen Sie im Abschnitt **Erstellen** im linken Bereich die Option **Entitäten** und anschließend die durch maschinelles Lernen erworbene Entität aus.
1. Fügen Sie ein Feature hinzu, indem Sie in der Zeile der Entität oder der untergeordneten Entität die Option **+ Feature hinzufügen** auswählen.
1. Wählen Sie aus den vorhandenen Entitäten und Begriffslisten.
1. Soll die Entität nur extrahiert werden, wenn das Feature gefunden wird, wählen Sie das Sternchen (`*`) für dieses Feature aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Hinzufügen eines Features zur Entität](media/add-entities/machine-learned-entity-schema-with-features.png)

## <a name="create-a-regular-expression-entity"></a>Erstellen einer Entität vom Typ „Regulärer Ausdruck“

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.
1. Wählen Sie im Abschnitt **Erstellen** im linken Bereich die Option **Entitäten** und dann **+ Erstellen** aus.

1. Geben Sie im Dialogfeld **Entitätstyp erstellen** den Namen der Entität ein, wählen Sie **RegEx** aus, geben Sie den regulären Ausdruck in das Feld **RegEx** ein, und wählen Sie **Erstellen** aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Erstellen einer Entität vom Typ „RegEx“](media/add-entities/add-regular-expression-entity.png)


<a name="add-list-entities"></a>

## <a name="create-a-list-entity"></a>Erstellen einer Listenentität

Listenentitäten stellen eine feste, abgeschlossene Gruppe mit verwandten Wörtern dar. Obwohl Sie als Ersteller die Liste ändern können, wird LUIS die Liste nicht vergrößern oder verkleinern. Sie können auch etwas in eine bereits vorhandene Listenentität importieren. Verwenden Sie hierzu das [JSON-Format für eine Listenentität](reference-entity-list.md#example-json-to-import-into-list-entity).

Die folgende Liste zeigt den kanonischen Namen und die Synonyme.

|Farbe – Name des Listenelements|Farbe – Synonyme|
|--|--|
|Red|Karmesin, Blut, Apfel, Feuerwehr|
|Blau|Himmel, Cobalt|
|Grün|Kelly, Limette|

Verwenden Sie dieses Verfahren zum Erstellen einer Listenentität. Nachdem die Listenentität erstellt wurde, müssen Sie keine Beispieläußerungen in einer Absicht bezeichnen. Listenelemente und Synonyme werden textgenau verglichen.
1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.
1. Wählen Sie im Abschnitt **Erstellen** im linken Bereich die Option **Entitäten** und dann **+ Erstellen** aus.

1. Geben Sie im Dialogfeld **Entitätstyp erstellen** den Namen der Entität ein, z. B. `Colors`, und wählen Sie dann **Liste** aus.
1. Geben Sie im Dialogfeld **Listenentität erstellen** in **Neue Unterliste hinzufügen....** den Namen des Listenelements ein, z.B. `Green`, und fügen Sie dann Synonyme hinzu.

    > [!div class="mx-imgBorder"]
    > ![Erstellen Sie eine Liste von Farben als Listenentität auf der Seite der Entitätsdetails.](media/how-to-add-entities/create-list-entity-of-colors.png)

1. Wenn Sie mit dem Hinzufügen von Listenelementen und Synonymen fertig sind, wählen Sie **Erstellen** aus.

    Wenn Sie eine Gruppe von Änderungen an der App vorgenommen haben, denken Sie daran, die App zu **trainieren**. Trainieren Sie die App nicht nach einer einzelnen Änderung.

    > [!NOTE]
    > Dieses Verfahren veranschaulicht das Erstellen und Bezeichnen einer Listenentität aus einer Beispieläußerung auf der Seite **Absichtsdetails**. Sie können dieselbe Entität auch auf der Seite **Entitäten** erstellen.

## <a name="add-a-role-for-an-entity"></a>Hinzufügen einer Rolle für eine Entität

Eine Rolle ist ein benannter Untertyp einer Entität, basierend auf dem Kontext.

### <a name="add-a-role-to-distinguish-different-contexts"></a>Hinzufügen einer Rolle zum Unterscheiden verschiedener Kontexte

In der folgenden Äußerung gibt es zwei Standorte, die jeweils semantisch durch die sie umgebenden Wörter wie `to` und `from` spezifiziert werden:

`Pick up the package from Seattle and deliver to New York City.`

Fügen Sie in diesem Verfahren `origin`- und `destination`-Rollen zu einer vordefinierten GeographyV2-Entität hinzu.
1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.
1. Wählen Sie im Abschnitt **Erstellen** im linken Bereich die Option **Entitäten**.

1. Wählen Sie **+ Vordefinierte Entität hinzufügen** aus. Wählen Sie **geographyV2** und dann **Fertig** aus. Dadurch wird der App eine vordefinierte Entität hinzugefügt.

    Wenn Ihr Muster eine Pattern.any-Entität enthält und Entitäten falsch extrahiert, verwenden Sie eine [explizite Liste](reference-pattern-syntax.md#explicit-lists), um dieses Problem zu beheben.

1. Wählen Sie die neu hinzugefügte vordefinierte GeographieV2-Entität aus der Seitenliste **Entitäten** aus.
1. Wählen Sie zum Hinzufügen einer neuen Rolle **+** neben **Keine Rollen hinzugefügt** aus.
1. Geben Sie im Textfeld **Rolle eingeben...** den Namen der Rolle `Origin` ein, und drücken Sie dann die EINGABETASTE. Fügen Sie eine zweite Rolle namens `Destination` hinzu, und drücken Sie dann die EINGABETASTE.

    > [!div class="mx-imgBorder"]
    > ![Screenshot vom Hinzufügen der Rolle „Origin“ zur Entität „Location“](media/how-to-add-entities//add-role-to-prebuilt-geographyv2-entity.png)

    Die Rolle wird der vordefinierten Entität hinzugefügt, aber nicht zu Äußerungen, die diese Entität verwenden.

### <a name="label-text-with-a-role-in-an-example-utterance"></a>Bezeichnen von Text mit einer Rolle in einer Beispieläußerung

> [!TIP]
> Rollen können durch die Bezeichnung mit untergeordneten Entitäten von Machine Learning-Entitäten ersetzt werden.

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.
1. Wechseln Sie zur Seite mit den Absichtsdetails, die Beispieläußerungen enthält, die die Rolle verwenden.
1. Wählen Sie zum Bezeichnen mit der Rolle die Entitätsbezeichnung (durchgezogene Linie unter Text) in der Beispieläußerung und dann **View in entity pane** (Im Entitätsbereich anzeigen) aus der Dropdownliste aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Das ausgewählte Menüelement „View in entity pane“ (Im Entitätsbereich anzeigen).](media/add-entities/view-in-entity-pane.png)

    Die Entitätspalette wird auf der rechten Seite geöffnet.

1. Wählen Sie die Entität aus, und wechseln Sie dann zum Ende der Palette, um die Rolle auszuwählen.

    > [!div class="mx-imgBorder"]
    > ![Der Screenshot zeigt, wo die Rolle auszuwählen ist.](media/add-entities/select-role-in-entity-palette.png)

<a name="add-pattern-any-entities"></a>
<a name="add-a-patternany-entity"></a>
<a name="create-a-pattern-from-an-utterance"></a>

## <a name="create-a-patternany-entity"></a>Erstellen einer Entität vom Typ „Pattern.any“

Die Entität **Pattern.any** ist nur mit [Mustern](luis-how-to-model-intent-pattern.md) verfügbar.


## <a name="do-not-change-entity-type"></a>Der Entitätstyp kann nicht geändert werden

LUIS erlaubt Ihnen nicht, den Entitätstyps zu ändern, da die Anwendung nicht weiß, was zum Erstellen der Entität hinzugefügt oder entfernt werden muss. Um den Typ zu ändern, sollten Sie besser eine neue Entität des richtigen Typs mit einem etwas anderen Namen erstellen. Sobald die Entität erstellt ist, entfernen Sie in jeder Äußerung den alten bezeichnenden Entitätsnamen, und fügen Sie den neuen Entitätsnamen hinzu. Sobald alle Äußerungen ihre neuen Bezeichnungen erhalten haben, löschen Sie die alte Entität.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwenden von vordefinierten Modellen](howto-add-prebuilt-models.md)

Weitere Informationen:
* Informationen zum [Trainieren](luis-how-to-train.md)
* Informationen zum [Testen](luis-interactive-test.md)
* Informationen zum [Veröffentlichen](luis-how-to-publish-app.md)
* Muster:
    * [Konzepte](luis-concept-patterns.md)
    * [Syntax](reference-pattern-syntax.md)
* [GitHub-Repository für vordefinierte Entitäten](https://github.com/Microsoft/Recognizers-Text)
* [Konzepte der Datenextraktion](luis-concept-data-extraction.md)



