---
title: Bezeichnen einer Entität in einer Beispieläußerung
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie eine durch maschinelles Lernen erworbene Entität mit Unterkomponenten in einer Beispieläußerung auf einer Absichtsdetailseite des LUIS-Portals bezeichnen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: diberry
ms.openlocfilehash: 5499e8362e1107a18b997a6a485e11343c328d0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898370"
---
# <a name="label-machine-learned-entity-in-an-example-utterance"></a>Bezeichnen einer durch maschinelles Lernen erworbenen Entität in einer Beispieläußerung

Die Bezeichnung einer Entität in einer Beispieläußerung bietet LUIS ein Beispiel der Entität und wo die Entität in der Äußerung auftauchen kann.

## <a name="labeling-machine-learned-entity"></a>Bezeichnen einer durch maschinelles Lernen erworbenen Entität

Nehmen wir als Beispiel den Satz `hi, please I want a cheese pizza in 20 minutes`.

1. Wählen Sie den am weitesten links stehenden Text und dann den Text ganz rechts in der Entität aus, und wählen Sie dann die Entität für die Bezeichnung aus (in diesem Fall „CompleteOrder“). In der folgenden Abbildung ist _CompleteOrder_ mit einer Bezeichnung versehen.

    > [!div class="mx-imgBorder"]
    > ![Bezeichnen der vollständigen, durch maschinelles Lernen erworbenen Entität](media/label-utterances/example-1-label-machine-learned-entity-complete-order.png)

1. Wählen Sie die Entität im Popupfenster aus. Die bezeichnete Entität für die Pizzabestellung enthält alle Wörter (im Englischen von links nach rechts), die bezeichnet sind.

## <a name="review-labeled-text"></a>Überprüfen des bezeichneten Texts

Überprüfen Sie nach der Bezeichnung die Beispieläußerung, und vergewissern Sie sich, dass der ausgewählte Textabschnitt mit der ausgewählten Entität unterstrichen ist. Die durchgezogene Linie gibt an, dass der Text bezeichnen wurde.

> [!div class="mx-imgBorder"]
> ![Bezeichnete, vollständige, durch maschinelles Lernen erworbene Entität](media/label-utterances/example-1-label-machine-learned-entity-complete-order-labeled.png)

## <a name="confirm-predicted-entity"></a>Bestätigen der vorhergesagten Entität

Wenn ein gepunkteter Rahmen um den Textabschnitt gezogen wurde und der Entitätsname über dem Ausdruck steht, bedeutet dies, dass der Text vorhergesagt, aber _noch nicht bezeichnet_ wurde. Wählen Sie die Zeile mit der Äußerung und anschließend **Entitätsvorhersagen bestätigen** aus, um aus der Vorhersage eine Bezeichnung zu machen.

> [!div class="mx-imgBorder"]
> ![Vorhersagen der vollständigen, durch maschinelles Lernen erworbenen Entität](media/label-utterances/example-1-label-machine-learned-entity-complete-order-predicted.png)

Alternativ können Sie den Entitätsnamen über dem Text und dann im angezeigten Menü **Vorhersage bestätigen** auswählen.

> [!div class="mx-imgBorder"]
> ![Vorhersagen der vollständigen, durch maschinelles Lernen erworbenen Entität mit Menü](media/label-utterances/example-1-label-machine-learned-entity-complete-order-predicted-menu.png)

## <a name="label-entity-by-painting-with-entity-palette-cursor"></a>Bezeichnen der Entität durch Zeichnen mit dem Entitätspalettencursor

Die Entitätspalette stellt eine Alternative zur oben beschrieben Form der Bezeichnung dar. Sie können damit über Text streichen, um diesen sofort mit einer Entität zu bezeichnen.

1. Sie öffnen die Entitätspalette, indem Sie rechts oben in der Tabelle mit den Äußerungen das Textmarkersymbol auswählen.

    > [!div class="mx-imgBorder"]
    > ![Entitätspalette für eine durch maschinelles Lernen erworbene Entität](media/label-utterances/example-1-label-machine-learned-entity-palette.png)

2. Wählen Sie die Entitätskomponente aus. Diese Aktion wird visuell durch einen neuen Cursor dargestellt. Der Cursor folgt dem Mauszeiger, wenn Sie ihn im Portal bewegen.

    > [!div class="mx-imgBorder"]
    > ![Entitätspalette für eine durch maschinelles Lernen erworbene Entität](media/label-utterances/example-1-label-machine-learned-entity-palette-menu.png)

3. _Zeichnen_ Sie die Entität in der Beispieläußerung mithilfe des Cursors.

    > [!div class="mx-imgBorder"]
    > ![Entitätspalette für eine durch maschinelles Lernen erworbene Entität](media/label-utterances/example-1-label-machine-learned-entity-palette-label-action.png)

## <a name="labeling-subcomponents-of-a-machine-learned-entity"></a>Bezeichnen von Unterkomponenten einer durch maschinelles Lernen erworbenen Entität

Unterkomponenten in Entitäten werden auf genau die gleiche Weise wie Entitäten der obersten Ebene bezeichnet. Wenn Sie Text markieren, entsprechen die im Popupfenster verfügbaren Entitäten dem Kontext des Texts. Wenn Sie also beispielsweise über eine durch maschinelles Lernen erworbene Entität mit fünf Ebenen verfügen und Text auswählen, der auf der ersten und zweiten Ebene bezeichnet ist (angegeben durch einen Entitätsnamen unter der Beispieläußerung), sind die im Popupfenster verfügbaren Entitäten auf den Kontext der Komponenten der dritten Ebene beschränkt. Um den Text mit anderen Entitäten zu bezeichnen, wählen Sie die Option **Label as another entity** (Als andere Entität bezeichnen) aus.

> [!div class="mx-imgBorder"]
> ![Entitätspalette für eine durch maschinelles Lernen erworbene Entität](media/label-utterances/example-1-label-machine-learned-entity-subcomponent.png)

Unterkomponenten können nur bezeichnet werden, wenn die übergeordnete Komponente ebenfalls bezeichnet ist.

## <a name="labeling-entity-roles"></a>Beschriften von Entitätsrollen

Entitätsrollen werden mithilfe der Entitätspalette beschriftet.

1. Wählen Sie auf der Detailseite der Absicht die **Entitätspalette** aus der Kontext-Symbolleiste aus.
1. Wenn die Entitätspalette offen ist, wählen Sie aus der Liste die gewünschte Entität aus.
1. Wechseln Sie zur **Entitätsprüfung**, und wählen Sie eine vorhandene Rolle aus, oder erstellen Sie eine neue Rolle.
1. Beschriften Sie in der Beispieläußerung den Text mit der Entitätsrolle.

## <a name="unlabel-entities"></a>Entitäten ohne Bezeichnung

Um die Bezeichnung einer Entität aufzuheben, wählen Sie den Entitätsnamen unterhalb des Texts und dann **Unlabel** (Bezeichnung entfernen) aus. Wenn die Entität, deren Bezeichnung Sie entfernen möchten, über bezeichnete Unterkomponenten verfügt, müssen zuerst deren Bezeichnungen entfernt werden.

## <a name="editing-labels-using-the-entity-palette"></a>Bearbeiten von Bezeichnungen mithilfe der Entitätspalette

Wenn Sie bei der Bezeichnung einen Fehler machen, können Sie mit der Entitätspalette schnelle Änderungen vornehmen. Wenn z. B. eine Entitätsbezeichnung versehentlich ein zusätzliches Wort einschließt und diese Entität bereits bezeichnete Unterkomponenten aufweist, können Sie mithilfe der Entitätspalette den gewünschten kürzeren Bereich von Wörtern markieren.

Beispiel:

1. Die Unterkomponente „Pizza Type“ umfasst „cheese pizza with“, wobei das Wort „with“ fälschlicherweise einbezogen wurde.

    > [!div class="mx-imgBorder"]
    > ![Entitätspalette für eine durch maschinelles Lernen erworbene Entität](media/label-utterances/edit-label-with-palette-1.png)

2. Verwenden Sie die Entitätspalette, um „Pizza Type“ auszuwählen und „cheese pizza“ zu überzeichnen. Als Ergebnis wird nur noch „cheese pizza“ als Bezeichnung für „Pizza Type“ verwendet.

    > [!div class="mx-imgBorder"]
    > ![Entitätspalette für eine durch maschinelles Lernen erworbene Entität](media/label-utterances/edit-label-with-palette-2.png)

## <a name="labels-for-matching-text-entities"></a>Bezeichnungen für Textentsprechungsentitäten

Zu den Textentsprechungsentitäten gehören vordefinierte Entitäten, RegEx-Entitäten, Listenentitäten und pattern.any-Entitäten. Diese werden automatisch von LUIS bezeichnet, sodass sie nicht manuell von den Benutzern bezeichnet werden müssen.

## <a name="entity-prediction-errors"></a>Fehler bei der Entitätsvorhersage

Vorhersagefehler bei Entitäten deuten darauf hin, dass die vorhergesagte Entität nicht der bezeichneten Entität entspricht. Dies wird mit durch einen Warnindikator neben der Äußerung verdeutlicht.

> [!div class="mx-imgBorder"]
> ![Entitätspalette für eine durch maschinelles Lernen erworbene Entität](media/label-utterances/example-utterance-indicates-prediction-error.png)

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie das [Dashboard](luis-how-to-use-dashboard.md), und [überprüfen Sie die Endpunktäußerungen](luis-how-to-review-endpoint-utterances.md), um die Vorhersagequalität Ihrer App zu verbessern.