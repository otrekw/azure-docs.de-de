---
title: 'Hinzufügen von Entitäten: LUIS'
titleSuffix: Azure Cognitive Services
description: Erstellen Sie Entitäten, um die Schlüsseldaten aus Benutzeräußerungen in Language Understanding-Apps (LUIS) zu extrahieren. Extrahierte Entitätsdaten werden von der Clientanwendung verwendet, um Kundenanforderungen zu erfüllen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/15/2019
ms.author: diberry
ms.openlocfilehash: 1f2b293acdc77e25e6b932c47d466cc28a04a2b6
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74383678"
---
# <a name="add-entities-to-extract-data"></a>Hinzufügen von Entitäten zum Extrahieren von Daten 

Erstellen Sie Entitäten, um die Schlüsseldaten aus Benutzeräußerungen in Language Understanding-Apps (LUIS) zu extrahieren. Extrahierte Entitätsdaten werden von Ihrer Clientanwendung verwendet, um Kundenanforderungen zu erfüllen.

Die Entität stellt ein Wort oder einen Ausdruck innerhalb der Äußerung dar, die extrahiert werden soll. Entitäten beschreiben Informationen mit Bezug zur Absicht. Manchmal sind sie auch für die Ausführung der Aufgabe Ihrer App wichtig. Sie können Entitäten erstellen, wenn Sie einer Absicht eine Beispieläußerung hinzufügen, oder auch getrennt vom Hinzufügen einer Beispieläußerung zu einer Absicht (vor oder nach dem Hinzufügen).

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="plan-entities-then-create-and-label"></a>Planen von Entitäten mit anschließender Erstellung und Bezeichnung

Durch maschinelles Lernen erworbene Entitäten können über die Beispieläußerungen oder über die Seite **Entitäten** erstellt werden. 

Im Allgemeinen ist es eine bewährte Methode, Zeit für die Planung der Entitäten aufzuwenden, bevor eine durch maschinelles Lernen erworbene Entität im Portal erstellt wird. Erstellen Sie dann die durch maschinelles Lernen erworbene Entität über die Beispieläußerung mit so vielen Details in den Teilkomponenten sowie Deskriptoren und Einschränkungen, wie Ihnen zu diesem Zeitpunkt bekannt sind. Das [Tutorial „Zerlegbare Entität“](tutorial-machine-learned-entity.md) veranschaulicht die Verwendung dieser Methode. 

Im Rahmen der Planung der Entitäten ist zu beachten, dass Sie Entitäten für den Textvergleich benötigen (z. B. vorgefertigte Entitäten, Entitäten vom Typ „Regular Expression“ oder vom Typ „List“). Sie können diese auf der Seite **Entitäten** erstellen, bevor sie in Beispieläußerungen bezeichnet werden. 

Beim Bezeichnen können Sie entweder einzelne Entitäten bezeichnen und dann zu einer übergeordneten, durch maschinelles Lernen erworbenen Entität aufbauen. Oder Sie beginnen mit einer übergeordneten, durch maschinelles Lernen erworbenen Entität und zerlegen sie in untergeordnete Entitäten. 

> [!TIP] 
>Bezeichnen Sie alle Wörter, die auf eine Entität hinweisen können, auch wenn die Wörter bei der Extraktion in der Clientanwendung nicht verwendet werden. 

## <a name="creating-an-entity-before-or-with-labeling"></a>Erstellen einer Entität vor der Bezeichnung oder beim Bezeichnen

Verwenden Sie die folgende Tabelle, um zu verstehen, welche Entitäten wo erstellt oder der App hinzugefügt werden sollen. 

|Entitätstyp|Erstellen einer Entität im LUIS-Portal|
|--|--|
|Durch maschinelles Lernen erworbene Entität|Entitäten oder Absichtsdetails|
|Entität vom Typ „List“|Entitäten oder Absichtsdetails|
|Entität vom Typ „RegEx“|Entitäten|
|Entität „Pattern.any“|Entitäten|
|Vordefinierte Entität|Entitäten|
|Vordefinierte Domänenentität|Entitäten|

Sie können alle Entitäten auf der Seite **Entitäten** erstellen, oder Sie können einige Entitäten im Rahmen der Bezeichnung der Entität in der Beispieläußerung auf der Seite **Absichtsdetails** erstellen. Sie können eine Entität in einer Beispieläußerung von der Seite **Absichtsdetails** nur _bezeichnen_. 

## <a name="create-a-machine-learned-entity"></a>Erstellen einer durch maschinelles Lernen erworbenen Entität

[!INCLUDE [Create and label entities in machine-learned tutorial](includes/decomposable-tutorial-links.md)]

## <a name="create-a-text-matching-entity"></a>Erstellen einer Entität mit Textvergleich

Die Verwendung von Entitäten mit Textvergleich bietet verschiedene Möglichkeiten zur Datenextraktion:

|Entitäten mit Textvergleich|Zweck|
|--|--|
|[Entität vom Typ „List“](#add-list-entities-for-exact-matches)|Liste der kanonischen Namen zusammen mit Synonymen als alternative Formen|
|Entität vom Typ „RegEx“|Textvergleich unter Verwendung einer Entität vom Typ „RegEx“|
|[Vordefinierte Entität](tutorial-machine-learned-entity.md#add-prebuilt-number-to-help-extract-data)|Vergleichen allgemeiner Datentypen wie „number“, „email“, „date“|
|Vordefinierte Domänenentität|Vergleich mit ausgewählten Motivdomänen|
|[Pattern.any-Entitäten](#add-a-patternany-entity)| Für den Vergleich von Entitäten, die leicht mit dem umgebenden Text verwechselt werden können|  

Vordefinierte Entitäten funktionieren ohne Bereitstellung benutzerdefinierter Trainingsdaten. Für die anderen Entitäten müssen Sie entweder Kundentrainingsdaten (z. B. die Elemente der Entität vom Typ „List“) oder einen Ausdruck (z. B. einen regulären Ausdruck oder pattern.any) bereitstellen.

<a name="add-list-entities"></a>

### <a name="how-to-create-a-new-custom-entity"></a>Erstellen einer neuen benutzerdefinierten Entität

1. Wechseln Sie im LUIS-Portal zum Abschnitt **Verwalten**, dann zur Seite **Entitäten**. 
1. Wählen Sie **+ Erstellen** und dann den Entitätstyp aus. 
1. Fahren Sie mit der Konfiguration der Entität fort, und wählen Sie dann **Erstellen** aus, wenn Sie fertig sind. 

### <a name="add-list-entities-for-exact-matches"></a>Hinzufügen von Listenentitäten für genaue Übereinstimmungen

Listenentitäten stellen eine feste, abgeschlossene Gruppe mit verwandten Wörtern dar. Obwohl Sie als Ersteller die Liste ändern können, wird LUIS die Liste nicht vergrößern oder verkleinern. Sie können auch in eine bestehende Listenentität importieren, indem Sie ein [list entity .json format]\(reference-entity-list.md#example-json-to-import-into-list-entity) verwenden. 

Die folgende Liste zeigt den kanonischen Namen und die Synonyme. 

|Farbe – Name des Listenelements|Farbe – Synonyme|
|--|--|
|Rot|Karmesin, Blut, Apfel, Feuerwehr|
|Blau|Himmel, Azur, Kobalt|
|Grün|Kelly, Limette|

Verwenden Sie dieses Verfahren zum Erstellen einer Listenentität. Nachdem die Listenentität erstellt wurde, müssen Sie keine Beispieläußerungen in einer Absicht bezeichnen. Listenelemente und Synonyme werden textgenau verglichen. 

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

1. Wechseln Sie zur Seite mit den Absichtsdetails, die Beispieläußerungen enthält, die die Rolle verwenden. 
1. Wählen Sie zum Bezeichnen mit der Rolle die Entitätsbezeichnung (durchgezogene Linie unter Text) in der Beispieläußerung und dann **In Entitätspalette anzeigen** aus der Dropdownliste aus. 

    > [!div class="mx-imgBorder"]
    > ![Screenshot zum Auswählen von „In Entitätspalette anzeigen“](media/how-to-add-entities/select-text-label-with-entity-palette-for-role.png)   

    Die Entitätspalette wird auf der rechten Seite geöffnet. 

1. Wählen Sie die Entität aus, und wechseln Sie dann zum Ende der Palette, um die Rolle auszuwählen. 

    > [!div class="mx-imgBorder"]
    > ![Screenshot zum Auswählen von „In Entitätspalette anzeigen“](media/how-to-add-entities/select-role-from-entity-palette-entity-inspector.png)

<a name="add-pattern-any-entities"></a>

## <a name="add-a-patternany-entity"></a>Hinzufügen einer pattern.any-Entität

[Pattern.Any](luis-concept-entity-types.md)-Entitäten sind nur in [Mustern](luis-how-to-model-intent-pattern.md) gültig, nicht in Beispieläußerungen von Absichten. Mit diesem Entitätstyp kann LUIS das Ende von Entitäten unterschiedlicher Länge und Wortwahl finden. Da diese Entität in einem Muster verwendet wird, weiß LUIS, wo sich das Ende der Entität in der Äußerungsvorlage befindet.

### <a name="steps-to-create-a-patternany-entity"></a>Schritte zum Erstellen einer Pattern.any-Entität

1. Wählen Sie im Abschnitt **Erstellen** im linken Bereich die Option **Entitäten** und dann **+ Erstellen** aus.

1. Geben Sie im Dialogfeld **Entitätstyps auswählen** den Entitätsnamen in das Feld **Name** ein, und wählen Sie **Pattern.Any** als **Typ** und anschließend **Erstellen** aus.

    Nachdem Sie mit dieser Entität [eine Musteräußerung](luis-how-to-model-intent-pattern.md) erstellt haben, wird die Entität mit einem kombinierten Algorithmus (durch maschinelles Lernen erworben und Textvergleich) extrahiert. 

### <a name="create-a-pattern-template-utterance-to-use-patternany-entity"></a>Erstellen einer Mustervorlageäußerung zur Verwendung der pattern.any-Entität

Um die pattern.any-Entität zu verwenden, fügen Sie auf der Seite **Muster** im Abschnitt **Improve app performance** (App-Leistung verbessern) mit der richtigen Syntax mit geschweiften Klammern ein Muster ein, z.B. `Where is **{HumanResourcesFormTitle}** on the server?`.

Wenn Ihr Muster eine Pattern.any-Entität enthält und Entitäten falsch extrahiert, verwenden Sie eine [explizite Liste](reference-pattern-syntax.md#explicit-lists), um dieses Problem zu beheben. 

## <a name="do-not-change-entity-type"></a>Der Entitätstyp kann nicht geändert werden

LUIS erlaubt Ihnen nicht, den Entitätstyps zu ändern, da die Anwendung nicht weiß, was zum Erstellen der Entität hinzugefügt oder entfernt werden muss. Um den Typ zu ändern, sollten Sie besser eine neue Entität des richtigen Typs mit einem etwas anderen Namen erstellen. Sobald die Entität erstellt ist, entfernen Sie in jeder Äußerung den alten bezeichnenden Entitätsnamen, und fügen Sie den neuen Entitätsnamen hinzu. Sobald alle Äußerungen ihre neuen Bezeichnungen erhalten haben, löschen Sie die alte Entität. 

<a name="create-a-pattern-from-an-utterance"></a>

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


 
