---
title: Features – LUIS
titleSuffix: Azure Cognitive Services
description: Verwenden Sie Language Understanding (LUIS), um App-Features hinzuzufügen, die die Erkennung oder Vorhersage von Absichten und Entitäten verbessern können, die Kategorien und Muster
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: diberry
ms.openlocfilehash: 12445ec5b14f4c274e471bf1b061a3b221664d20
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592303"
---
# <a name="use-features-to-boost-signal-of-word-list"></a>Verstärken des Signals von Wortlisten mithilfe von Features

Sie können Ihrer LUIS-App Features hinzufügen, um ihre Genauigkeit zu verbessern. Features unterstützen LUIS mit Hinweisen, dass bestimmte Wörter und Ausdrücke Teil eines App-Domänenvokabulars sind.

Machen Sie sich mit den [Konzepten](luis-concept-feature.md) vertraut, um zu erfahren, wann und warum ein Feature verwendet wird.

## <a name="add-phrase-list-as-a-feature"></a>Hinzufügen einer Begriffsliste als Feature

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.
1. Wählen Sie **Erstellen** und anschließend im linken Bereich Ihrer App die Option **Features** aus.

1. Wählen Sie auf der Seite **Features** die Option **+ Erstellen** aus.

1. Geben Sie im Dialogfeld **Create new phrase list feature** (Neues Ausdruckslistenfeature erstellen) einen Namen für das Feature ein (beispielsweise `Cities`). Geben Sie im Feld **Wert** Beispiele für die Städte ein (beispielsweise `Seattle`). Sie können jeweils einen Wert oder eine Gruppe durch Kommas getrennter Werte eingeben und dann die **EINGABETASTE** drücken.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Hinzufügen eines Features (Ausdrucksliste) in Form von Städten](./media/luis-add-features/add-phrase-list-cities.png)

    Sobald Sie genügend Werte für LUIS eingegeben haben, werden Vorschläge angezeigt. Sie können **+ Alle hinzufügen** verwenden, um alle vorgeschlagenen Werte hinzufügen, oder einzelne Begriffe auswählen.

1. Lassen Sie die Option **These values are interchangeable** (Diese Werte sind austauschbar.) aktiviert, wenn die Ausdrücke synonym verwendet werden können.

1. Die Liste der Ausdrücke kann auf die gesamte App mit der Einstellung **Global** oder nur auf ein bestimmtes Modell (Absicht oder Entität) angewendet werden. Wenn Sie die Ausdrucksliste als _Feature_ auf der Grundlage einer Absicht oder Entität erstellen, ist die Umschaltfläche nicht auf global festgelegt. In diesem Fall bedeutet die Umschaltfläche, dass es sich um ein lokales Feature handelt, das nur für dieses Modell (und somit _nicht global_ für die Anwendung) gilt.

1. Wählen Sie **Fertig**aus. Das neue Feature wird der Seite **ML Features** (ML-Features) hinzugefügt.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Eine Begriffsliste kann über die kontextbezogene Symbolleiste auf der Seite **ML Features** (ML-Features) gelöscht oder deaktiviert werden.

## <a name="next-steps"></a>Nächste Schritte

[Trainieren und testen Sie die App](luis-interactive-test.md) nach dem Hinzufügen, Bearbeiten, Löschen oder Deaktivieren eines Features erneut, um festzustellen, ob sich die Leistung verbessert hat.
