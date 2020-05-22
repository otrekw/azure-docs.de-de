---
title: 'Hinzufügen von Absichten: LUIS'
titleSuffix: Azure Cognitive Services
description: Fügen Sie Ihrer LUIS-App Absichten hinzu, um Gruppen von Fragen oder Befehlen zu identifizieren, die den gleichen Zweck verfolgen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/07/2020
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: b2cb7494ae3d26fa14bef906b8f5222b9dbc70e1
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83584967"
---
# <a name="add-intents-to-determine-user-intention-of-utterances"></a>Hinzufügen von Absichten, um die Absicht von Benutzeräußerungen zu bestimmen

Fügen Sie Ihrer LUIS-App [Absichten](luis-concept-intent.md) hinzu, um Gruppen von Fragen oder Befehlen zu identifizieren, die den gleichen Zweck verfolgen.

Navigieren Sie in der oberen Navigationsleiste zum Abschnitt **Build** und dann im linken Bereich zu **Absichten**, um die Absichten zu verwalten.

## <a name="add-intent"></a>Hinzufügen einer Absicht

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.
1. Wählen Sie auf der Seite **Absichten** die Option **+ Erstellen** aus.
1. Geben Sie im Dialogfeld **Neue Absicht erstellen** den Namen der Absicht ein, z. B. `ModifyOrder`, und wählen Sie **Fertig** aus.

    > [!div class="mx-imgBorder"]
    > ![Hinzufügen einer Absicht](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

    Für die Absicht sind Beispieläußerungen erforderlich.

## <a name="add-an-example-utterance"></a>Hinzufügen einer Beispieläußerung

Beispieläußerungen sind Textbeispiele für Benutzerfragen oder -befehle. Sie müssen einer Absicht Beispieläußerungen hinzufügen, um LUIS (Language Understanding) für die Vorhersage dieser Absicht zu trainieren. LUIS benötigt zwischen 15 und 30 Beispieläußerungen, um die Absicht zu verstehen. Fügen Sie nicht massenhaft Beispieläußerungen hinzu. Jede Äußerung sollte sorgfältig ausgewählt werden, inwieweit sie sich von den anderen Beispielen unterscheidet, die sich bereits in der Absicht befinden.

1. Geben Sie auf der Detailseite der Absicht im Textfeld unter ihrem Namen eine relevante Äußerung ein, die Sie von Ihren Benutzern erwarten, z. B. `Deliver a large cheese pizza`, und drücken Sie dann die EINGABETASTE.

    > [!div class="mx-imgBorder"]
    > ![Screenshot der Seite für die Details zu den Absichten mit hervorgehobener Äußerung](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png)

    LUIS konvertiert alle Äußerungen in Kleinbuchstaben und fügt vor und nach [Token](luis-language-support.md#tokenization), z. B. Bindestrichen, Leerzeichen ein.

<a name="#intent-prediction-discrepancy-errors"></a>

## <a name="intent-prediction-errors"></a>Fehler bei Absichtsvorhersagen

Eine Beispieläußerung in eine Absicht, bei der möglicherweise ein Fehler bei der Absichtsvorhersage zwischen der Absicht, zu der die Beispieläußerung gehört, und der während des Trainings ermittelten Absicht aufgetreten ist.

Um Äußerungsvorhersagefehler zu finden und zu beheben, verwenden Sie die **Filteroptionen** „Falsch“ und „Unklar“ zusammen mit der Option **Ansicht** von **Detailansicht**.

![Verwenden Sie für das Ermitteln und Beheben von Äußerungsvorhersagefehlern die Option „Filter“.](./media/luis-how-to-add-intents/find-intent-prediction-errors.png)

Wenn die Filter und die Ansicht angewandt wurden und Beispieläußerungen mit Fehlern vorhanden sind, enthält die Liste der Beispieläußerungen die Äußerungen und die Probleme.

> [!div class="mx-imgBorder"]
> ![![Wenn die Filter und die Ansicht angewandt wurden und Beispieläußerungen mit Fehlern vorhanden sind, enthält die Liste der Beispieläußerungen die Äußerungen und die Probleme.](./media/luis-how-to-add-intents/find-errors-in-utterances.png)](./media/luis-how-to-add-intents/find-errors-in-utterances.png#lightbox)

Jede Zeile zeigt das aktuelle Vorhersageergebnis des Trainings für die Beispieläußerung, das nächstschlechtere Ergebnis und den Unterschied zwischen diesen beiden Bewertungen.

### <a name="fixing-intents"></a>Beheben von Absichten

Wenn Sie erfahren möchten, wie Sie Absichtsvorhersagefehler beheben, verwenden Sie das [Zusammenfassungs-Dashboard](luis-how-to-use-dashboard.md). Das Zusammenfassungs-Dashboard enthält eine Analyse des letzten Trainings der aktiven Version und die besten Vorschläge für das Verbessern Ihres Modells.

## <a name="using-the-contextual-toolbar"></a>Verwenden der kontextbezogenen Symbolleiste

Die Kontextsymbolleiste bietet weitere Aktionen:

* Bearbeiten oder Löschen einer Beispieläußerung
* Erneutes Zuweisen einer Beispieläußerung zu einer anderen Absicht
* Filter und Ansichten: Zeigen Sie nur Äußerungen an, die gefilterte Entitäten enthalten, oder zeigen Sie optionale Details an.
* Durchsuchen von Beispieläußerungen

## <a name="train-your-app-after-changing-model-with-intents"></a>Trainieren der App nach dem Ändern des Modells mit Absichten

Nachdem Sie Absichten hinzugefügt, bearbeitet oder entfernt haben, [trainieren](luis-how-to-train.md) und [veröffentlichen](luis-how-to-publish-app.md) Sie Ihre App, damit die Änderungen auf Endpunktabfragen angewendet werden. Führen Sie das Training nicht nach jeder einzelnen Änderung durch. Führen Sie das Training nach einer Gruppe von Änderungen durch.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zum Hinzufügen von [Beispieläußerungen](luis-how-to-add-example-utterances.md) mit Entitäten.
